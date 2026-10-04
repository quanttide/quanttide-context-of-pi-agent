# quanttide-founder-lab

`examples/quanttide-founder-lab` — quanttide-founder-lab

实验室：原型与应用实验，小说规划器在其中。

## 一、任务看板的设计约束

任务看板最终形态（src/task_board.py tkinter GUI，配套 tests/test_task_board.py 17 项固定测试，纯逻辑与 GUI 类分离便于无显示环境跑）：评审模式一次只展示一张；分类字段严格用仓库文件夹名（folder 契约，rec 可选）；标注（标签）与多行理由分两行；无「选定做」「复制清单」按钮；「保存」= 快照进 feedback[标题].history（{time,tag,text}）+ 原子写盘 + 自动跳下一张，空标注不产生记录；随键入实时写盘。用户首轮真实标注（后续创作参考）：补写 6/7 章缓做（暂无思路）；定稿《酒吧表白》《海边再散步》缓做（氛围够但情节前后不连贯）；第 3 章主线「发火不符合市场逻辑，需斟酌」；《赏雪谈心》氛围依赖冬天、与男女主春夏重逢设定冲突，改「在一起之后」需把暧昧改亲密且要前文支撑。

<!-- created=2026-09-05, last=2026-09-05 -->
