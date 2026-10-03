# pi 主程序

pi 主程序只认识 JSONL 文件，会话事实是它唯一写入的数据层，pi 生态数据的三层归属见 [pi-hermes-memory](./pi-hermes-memory.md)。本文面向任何消费 pi 数据的下游工具，不依赖具体项目。

## 会话文件

每次会话以追加写入的方式落到 `~/.pi/agent/sessions/<项目>/<时间戳>_<uuid>.jsonl`，按项目分目录。

会话 JSONL 为事件日志格式：首行为 `session` 头（含 id、cwd），其后交替出现 `model_change`、`thinking_level_change`、`message` 等事件行。

## 读取注意事项

从 `sessions.db` 读到的消息在导入索引时被截断，需要完整对话时应读取 `~/.pi/agent/sessions/` 下的原始 JSONL。
