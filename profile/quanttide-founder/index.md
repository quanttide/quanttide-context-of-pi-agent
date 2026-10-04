# quanttide-founder

quanttide-founder 项目的 Pi 记忆——**草稿**，先落在语境仓，定稿后再归位。

## 来源

本地 Pi 记忆扩展（pi-hermes-memory）的项目级记忆：`~/.pi/agent/projects-memory/quanttide-founder/MEMORY.md`，共 13 条。

## 脱敏口径

公开归档只留工程与流程经验——做法、判据、教训。脱敏后 11 条，按子模块分开归档。

不入档：作品名与人物名、发布平台与账号、域名与对象存储桶名、仓库内部路径、提交哈希，以及创作内容与个人议题的表述。

## 子模块

| 文件夹 | 子模块路径 | 仓库 | 记忆 |
|--------|-----------|------|------|
| `fiction/` | `assets/fiction` | quanttide-fiction-of-founder | 6 条 |
| `memory/` | `assets/memory` | quanttide-memory-of-founder | 2 条 |
| `qtgame-war/` | `apps/qtgame-war` | qtgame-war | 1 条 |
| `quanttide-founder-lab/` | `examples/quanttide-founder-lab` | quanttide-founder-lab | 1 条 |
| — | `apps/qtfounder` | qtfounder | 暂无 |
| — | `apps/qtgame-tycoon` | qtgame-tycoon | 暂无 |
| — | `apps/qtgame-weiqi` | qtgame-weiqi | 暂无 |
| — | `assets/archive` | quanttide-archive-of-founder | 暂无 |
| — | `packages/quanttide-founder-toolkit` | quanttide-founder-toolkit | 暂无 |

## 跨子模块

### 标签可达性（教训）

批量发布时，版本标签可能指向分叉自主干的孤立提交，主干缺该版本条目。纪律：CHANGELOG 提交必须先落在主干再打标签。标签存在不等于在 HEAD 历史中，用 `git merge-base --is-ancestor <tag> HEAD` 验证可达性。
