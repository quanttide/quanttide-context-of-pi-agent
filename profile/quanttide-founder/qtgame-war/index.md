# qtgame-war

`apps/qtgame-war` — qtgame-war

战棋游戏应用：分支剧情与场景管理。

## 一、分支图渲染

分支可级联、可汇聚，结局可带概率，史实线只是 DAG 中已走过的路径。首版渲染器（深度分层横向带 + 顶部史实时间轴）被用户指「和我要的DAG不够搭，还是比较线性」，已改用 dagre 库做 Sugiyama 从左到右布局（x=时间/层级、y=分支通道，fork 子树分独立纵向通道）；build.py 生成前校验重复 id 与无环（四战役 acyclic 通过，曾检出 1 个重复节点 id 待修）。

<!-- created=2026-09-06, last=2026-09-06 -->

## 二、scene-manager 原型

scene-manager 原型（apps/qtgame-war/examples/scene-manager/）：用户认为 dagre 版 DAG 分支图（index.html，LR 分层布局 + vendored @dagrejs/dagre）"并不理想、仍偏线性感"。已知问题：rank 均分间距对长短链混排不紧凑、卡片高度差异大导致纵向留白不均、多 fork 时交叉偏多。注意：正式方案已定为 tkinter GUI（index.md Q3），index.html 只是浏览器原型。迭代前先让用户指出具体不满意点。

<!-- created=2026-09-06, last=2026-09-06 -->
