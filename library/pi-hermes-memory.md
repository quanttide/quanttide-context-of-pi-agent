# pi-hermes-memory

一句话模型：pi 主程序只写 JSONL（每行一个 JSON 对象的事件日志），本扩展把 JSONL 索引进 SQLite；记忆的事实源是 Markdown，SQLite 只是检索镜像。三个记住点：完整对话读 JSONL，编辑记忆读 Markdown，搜索才用 SQLite。

## 它是什么

`pi-hermes-memory` 通过 `settings.json` 的 `packages` 字段以扩展形式加载，挂接 pi 的事件钩子，把主程序的 JSONL 单向导入 SQLite，提供 `session_search` 与 `memory_search` 两个搜索工具。本文面向任何消费 pi 数据的下游工具，不依赖具体项目。

## 事实源与检索索引分离

本文称冲突时以此为准、需要长期保存的数据为**事实源**，称从事实源生成、仅供查询、可重建或可同步的数据为**检索索引**；「派生」描述二者的生成关系，如 `sessions.db` 从 JSONL 派生。

JSONL 和 Markdown 是事实源；SQLite 是检索索引。冲突时以事实源为准。

## 数据归属与数据流

| 数据 | 角色 | 位置 | 写入方 |
|:--|:--|:--|:--|
| 会话事实（JSONL） | 事实源 | `~/.pi/agent/sessions/<项目>/<时间戳>_<uuid>.jsonl` | pi 主程序，追加写入 |
| 记忆（Markdown） | 事实源 | `~/.pi/agent/pi-hermes-memory/MEMORY.md`、`USER.md`、`STANDING.md` 与 `~/.pi/agent/projects-memory/<项目>/MEMORY.md` | 扩展与用户，可手工编辑 |
| `sessions.db` | 检索索引 | `~/.pi/agent/pi-hermes-memory/sessions.db` | 扩展 `pi-hermes-memory` |
| `memories` 表 | 检索镜像 | `sessions.db` 内，从记忆 Markdown 同步 | 扩展 `pi-hermes-memory` |

```text
pi 主程序 ──追加──> sessions/*.jsonl ──导入──> sessions.db ──> session_search
用户/扩展 ──写──> MEMORY.md 等 ──镜像──> memories 表 ──> memory_search
项目记忆：projects-memory/<项目>/MEMORY.md ──仅 cwd（当前工作目录）匹配时检索
```

JSONL 和 Markdown 是事实源，SQLite 是检索索引。

pi 主程序只认识 JSONL 文件，写入会话事实这一层；另外两层由本扩展维护，会话事实的格式与读取方式见 [pi 主程序](./pi-main.md)。

## 记忆的作用域

记忆的事实源按作用域分两处：

- 全局记忆：`~/.pi/agent/pi-hermes-memory/` 下的 `MEMORY.md`、`USER.md`、`STANDING.md`，存放处处适用的事实（用户名、偏好、系统与工具环境）；
- 项目记忆：`~/.pi/agent/projects-memory/<项目>/` 下的 `MEMORY.md`，存放单个代码库的事实（架构决策、API 怪癖、团队约定），仅在 cwd 匹配该项目时被 `memory_search` 检索。

`STANDING.md`（固定指令）在扩展代码层面禁止 agent 自写，只有用户可以编辑。

## 写入与同步

每次写入先落 Markdown，成功后才尽力同步进 `memories` 表；Markdown 写入失败时不降级为只存 SQLite。

扩展的写进程（后台复习、记忆合并、会话收尾）通过独立的 `~/.pi/agent/.pi-hermes-locks.sqlite` 抢锁协调，会话关闭时对 `sessions.db` 做 WAL checkpoint（把预写日志合并回主库文件）。锁只约束写方，只读查询以 busy timeout（写锁等待上限）兜底即可。

## 读取与排错

| 你想做什么 | 应该读哪里 |
|:--|:--|
| 完整对话 | `~/.pi/agent/sessions/` 下的会话事实（JSONL） |
| 快速搜索会话 | `sessions.db`（`session_search`） |
| 编辑记忆 | `MEMORY.md`、`USER.md`、`~/.pi/agent/projects-memory/<项目>/MEMORY.md` |
| 查看固定指令 | `STANDING.md`，仅用户可编辑 |
| 重建索引 | 备份后重建 `sessions.db`，再从 JSONL 导入 |

截断：导入索引时消息被截断（`truncateMessageContent`），从 `sessions.db` 读到的消息可能短于 JSONL，需要完整对话时读会话事实（JSONL）。

不同步：`memories` 表是记忆 Markdown 的检索镜像，可能滞后于事实源，编辑记忆时直接读 Markdown。

重建：`sessions.db` 从 JSONL 派生，以 `INSERT OR IGNORE` 与 `session_files` 表的文件元数据（路径、大小、修改时间）保证幂等（重复导入不产生重复数据），损坏时备份后重建并重新导入。库中的 `message_fts`、`memory_fts` 等 FTS5（SQLite 全文检索）虚表与同步触发器按需查询即可，不必视为事实源。
