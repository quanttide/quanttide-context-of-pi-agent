# quanttide-founder

quanttide-founder 项目的 Pi 记忆——**草稿**，先落在语境仓，定稿后再归位。

## 来源

本地 Pi 记忆扩展（pi-hermes-memory）的项目级记忆：`~/.pi/agent/projects-memory/quanttide-founder/MEMORY.md`，共 13 条。

## 脱敏口径

只藏两类：

- **本机敏感信息**：本机路径、密钥、私有数据；
- **部署敏感信息**：域名、对象存储桶名、CDN 与服务器信息、账号。

其余照实归档——来源仓 `quanttide-founder` 及其子模块本就是公开仓库，其中的作品资料、发布平台、内部路径与提交记录都可公开。

## 子模块

| 文件夹 | 子模块路径 | 仓库 | 记忆 |
|--------|-----------|------|------|
| `fiction/` | `assets/fiction` | quanttide-fiction-of-founder | 7 条 |
| `memory/` | `assets/memory` | quanttide-memory-of-founder | 2 条 |
| `qtgame-war/` | `apps/qtgame-war` | qtgame-war | 2 条 |
| `quanttide-founder-lab/` | `examples/quanttide-founder-lab` | quanttide-founder-lab | 1 条 |
| — | `apps/qtfounder` | qtfounder | 暂无 |
| — | `apps/qtgame-tycoon` | qtgame-tycoon | 暂无 |
| — | `apps/qtgame-weiqi` | qtgame-weiqi | 暂无 |
| — | `assets/archive` | quanttide-archive-of-founder | 暂无 |
| — | `packages/quanttide-founder-toolkit` | quanttide-founder-toolkit | 暂无 |

## 跨子模块

### 标签可达性（教训）

v1.0.0 CHANGELOG 孤立分支问题（2026-08-10 批量发布）：v1.0.0 标签指向分叉自 main 的孤立提交，main 缺 [1.0.0] 条目。影响 assets/fiction、assets/archive、assets/memory 三子模块，均已 cherry-pick 修复并更新主仓库引用。教训：changelog 提交必须落在 main 再打 tag；标签存在≠在 HEAD 历史中，用 `git merge-base --is-ancestor <tag> HEAD` 验证可达性；assets/memory 自 v0.5.2 后 250+ 提交未发布，下次版本号需重估。

<!-- created=2026-09-05, last=2026-09-06 -->
