# pi 主程序

pi 主程序只写会话事实（JSONL），不写记忆，也不建索引。pi 生态的主线是事实源与检索索引分离：JSONL 和 Markdown 是事实源，SQLite 是检索索引，冲突时以事实源为准；术语定义、数据归属与读取决策表见 [pi-hermes-memory](./pi-hermes-memory.md)。本文面向任何消费 pi 数据的下游工具，不依赖具体项目。

## 会话文件

每次会话以追加写入的方式落到 `~/.pi/agent/sessions/<项目>/<时间戳>_<uuid>.jsonl`，按项目分目录。会话事实（JSONL）为事件日志：首行为 `session` 头（含 id、cwd，即当前工作目录），其后交替出现 `model_change`、`thinking_level_change`、`message` 等事件行。

## 读取要点

需要完整对话时读 `~/.pi/agent/sessions/` 下的会话事实（JSONL）；`sessions.db` 是从 JSONL 派生的检索索引，导入时消息被截断，只用于快速搜索。任务与读取位置的对应关系见 [pi-hermes-memory](./pi-hermes-memory.md) 的读取决策表。
