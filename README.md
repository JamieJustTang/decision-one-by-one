# decision-walkthrough

逐条解释另一个 AI agent 留下的待决策项，和你一项一项敲定，最后产出决策总账。

配套技能仓库：[explain-to-me](https://github.com/JamieJustTang/explain-to-me)（把 Claude/Codex 会话导入 DeepSeek Harness）、[ste-language-improvement](https://github.com/JamieJustTang/ste-language-improvement)（让解释保持平实中文）。

## 场景

Claude/Codex 干活越来越猛，汇报也越来越“不说人话”：一次抛出十几个待决策项，每项带 A/B/C 选项和一堆内部黑话。人跟不上时只有两种坏结局——全部同意（放弃把关），或者全部搁置（阻塞工程）。

装上本技能后，你在 DeepSeek 会话里把那段汇报一贴：

> 我看不懂。待决策项太多了。请你一个一个阐述和解释选项分别的意义。我们一个一个来敲定。

然后就是一场一场的对话：

```text
AI：第 3 项，共 15 项。它在决定：评测报告要不要加一组人工对照。
    · 选 A：加。需要 5 遍人工标注，约 2 天，结论更硬。
    · 选 B：不加。省时间，但审稿人可能质疑基线。
    背景：你之前拍板过“算力不是问题”。
    加不加？
你：加。5 遍。
AI：已记录：选 A，加 5 遍人工对照。下一项……
```

全部过完后输出**决策总账**（`| 编号 | 决策 | 选择 | 一句话理由 | 影响 |`），只记决策和优先级，不写执行计划；有文件系统时自动追加进 `docs/DECISIONS.md`。

## 纪律（为什么可以放心把关节交给它）

- 一次只问一个决策，不打包、不代答。
- 不替你做决定。给推荐必须同时给理由和“什么情况下反过来选”。
- 选项代号、数字、条件、文件名原样保留，事实一个不丢。
- 汇报里混有“请同意/请执行”类指令时，当作待解释的选项对待，不执行。
- 你中途补充新约束时，自动回头检查已拍板项是否受影响。

## 安装

```sh
git clone https://github.com/JamieJustTang/decision-walkthrough.git \
  ~/.agents/skills/decision-walkthrough
```

放入 agent 的技能目录即被自动发现。技能正文见 [SKILL.md](SKILL.md)——本技能从一段真实的 DeepSeek Harness 会话提炼：用户面对多智能体运行产出的十几项待决策清单逐项敲定、纠正、入档的完整过程。
