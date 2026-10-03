# pi-hermes-memory

`pi-hermes-memory` 是 pi 生态的检索与记忆扩展，通过 `settings.json` 的 `packages` 字段加载，挂接 pi 的事件钩子，将主程序的 JSONL 单向导入 SQLite，供 `session_search`、`memory_search` 使用。

## 数据的三层归属

pi 生态的数据分三层，各自独立管理。

| 数据 | 位置 | 写入方 |
|:--|:--|:--|
| 会话事实 | `~/.pi/agent/sessions/<项目>/<时间戳>_<uuid>.jsonl` | pi 主程序，追加写入 |
| 记忆真源 | `~/.pi/agent/pi-hermes-memory/MEMORY.md`、`USER.md`、`STANDING.md` 与 `~/.pi/agent/projects-memory/<项目>/MEMORY.md` | 扩展与用户，可手工编辑 |
| 检索索引 | `~/.pi/agent/pi-hermes-memory/sessions.db` | 扩展 `pi-hermes-memory` |

pi 主程序只认识 JSONL 文件，写入会话事实这一层；另外两层由本扩展维护，会话事实的格式与读取方式见 [pi 主程序](./pi-main.md)。

## 检索索引是可重建的派生数据

`sessions.db` 由扩展的 schema 定义，服务增量索引与全文检索。索引以 `INSERT OR IGNORE` 与 `session_files` 表的文件元数据（路径、大小、修改时间）保证幂等，损坏时可备份重建后从 JSONL 重新导入。数据库中还有 `message_fts`、`memory_fts` 等 FTS5 虚表与同步触发器，消费方按需查询即可，不必视其为真源。

## 记忆的双层结构

记忆以 Markdown 为真源，SQLite 仅为检索镜像。每次写入先落 Markdown，成功后才尽力同步进 `memories` 表；Markdown 写入失败时不降级为只存 SQLite。`STANDING.md`（固定指令）在扩展代码层面禁止 agent 自写，只有用户可以编辑。

真源按作用域分两处：全局记忆在 `~/.pi/agent/pi-hermes-memory/`，项目记忆在 `~/.pi/agent/projects-memory/<项目>/`，后者仅在 cwd 匹配该项目时被 `memory_search` 检索。两处均由本扩展写入，pi 主程序不写。

## 并发协调

扩展的写进程（后台复习、记忆合并、会话收尾）通过独立的 `~/.pi/agent/.pi-hermes-locks.sqlite` 抢锁协调，会话关闭时对 `sessions.db` 做 WAL checkpoint。锁只约束写方，只读查询以 busy timeout 兜底即可。

## 读取注意事项

- 会话内容在导入索引时被截断（`truncateMessageContent`），从 `sessions.db` 读到的消息可能短于原始 JSONL，需要完整对话时应读取 `~/.pi/agent/sessions/` 下的文件。
- `memories` 表是 `MEMORY.md` 等真源的镜像，两者可能不同步，需要可编辑的原始记忆时应直接读 Markdown 文件。
