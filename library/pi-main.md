# pi 主程序

pi 生态的数据由主程序与扩展 `pi-hermes-memory` 分工写入，读取其落盘数据前需要理解这一分工。本文面向任何消费 pi 数据的下游工具，不依赖具体项目。

## 数据的三层归属

pi 生态的数据分三层，各自独立管理。

| 数据 | 位置 | 写入方 |
|:--|:--|:--|
| 会话事实 | `~/.pi/agent/sessions/<项目>/<时间戳>_<uuid>.jsonl` | pi 主程序，追加写入 |
| 记忆真源 | `~/.pi/agent/pi-hermes-memory/MEMORY.md`、`USER.md`、`STANDING.md` 与 `~/.pi/agent/projects-memory/<项目>/MEMORY.md` | 扩展与用户，可手工编辑 |
| 检索索引 | `~/.pi/agent/pi-hermes-memory/sessions.db` | 扩展 `pi-hermes-memory` |

pi 主程序只认识 JSONL 文件，写入会话事实这一层；另外两层由扩展 `pi-hermes-memory` 维护，见 [pi-hermes-memory](./pi-hermes-memory.md)。

## 会话文件

每次会话以追加写入的方式落到 `~/.pi/agent/sessions/<项目>/<时间戳>_<uuid>.jsonl`，按项目分目录。

会话 JSONL 为事件日志格式：首行为 `session` 头（含 id、cwd），其后交替出现 `model_change`、`thinking_level_change`、`message` 等事件行。

## 读取注意事项

从检索索引读到的消息在导入时被截断，需要完整对话时应读取 `~/.pi/agent/sessions/` 下的原始 JSONL。
