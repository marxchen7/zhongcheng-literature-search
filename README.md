# 钟澄文献检索方法论

一个 Claude Code skill，把钟澄《科研论》的文献检索经验做成可执行的检索方案设计与检索问题诊断。

仓库根目录即 skill 根目录。克隆后软链到 `~/.claude/skills/` 即可被 Claude Code 发现。

页面版：https://marxchen7.github.io/zhongcheng-literature-search/

---

## 它解决什么问题

不是"用哪个数据库"，而是三个更前面的问题：

1. **搜什么** —— 概念怎么拆、同义表达从哪来、拆到什么程度
2. **怎么构造** —— 检索式长什么样、每轮测试改哪个变量
3. **怎么判断够不够** —— 多少篇算够、什么时候该停、什么情况必须补

## 五条原则

| | 原则 | 一句话 |
|---|---|---|
| 一 | 鲸吞法 | 先吞后吐。第一轮尽可能囊括，精准靠分组解决，不靠第一轮提纯 |
| 二 | 概念拆分到最小单元 | 作者研究 A 却从不提及 A；只需掌握每个单元的同义表达 |
| 三 | 万能检索式 | `(A1 OR A2) AND (B1 OR B2)`，行内只用 OR，行间只用 AND |
| 四 | 小步进实测 | 先测 A → 过多则 A AND B → 再逐步扩充，不一次性交付成品 |
| 五 | 完成比完美 | 90%+ 覆盖即启动；但也不因少量文献就开始研究 |

检索式设置属于**搜商**，是后续一切工作的起点。总耗时最多一天；因"差不多不就行了"导致检索式不完善，后期浪费的时间以年计。

## 两个模式

- **设计模式（8 步）** —— 还没有检索式，或只有直译的一个词
- **诊断模式（7 问）** —— 已有检索式 + 具体症状（搜不到 / 搜太多 / 一半不相关 / 同义词穷尽焦虑 / 文献看不完）

两条路径都通向：`library-building.md`（入库）→ `grouping-triage.md`（分组）→ `completeness.md`（查漏）。

## 文件

| 文件 | 内容 |
|---|---|
| [`SKILL.md`](SKILL.md) | 入口。原则、模式路由、阈值速查表、八条铁律、references 导航 |
| [`references/search-formula.md`](references/search-formula.md) | 概念拆分、同义四渠道、万能检索式、符号规则、字段阶梯、实测校验、删除测试 |
| [`references/library-building.md`](references/library-building.md) | WoS / Scopus 导出、EndNote 导入（ISI-CE）、去重、形成记录、增量更新 |
| [`references/grouping-triage.md`](references/grouping-triage.md) | 一 / 二 / 三次分组、组集、中转组、检索词提炼 8 规则、非排他标签化 |
| [`references/completeness.md`](references/completeness.md) | 查漏八机制、启动支点论、删除精简保留最小单元、钓鱼法兜底、阈值表 |
| [`references/cases-and-diagnosis.md`](references/cases-and-diagnosis.md) | 八个检索案例（含真实命中量级）、三个跨领域病例、12 行诊断库、17 条警示语 |
| [`references/database-syntax.md`](references/database-syntax.md) | WoS / Scopus / CNKI 字段与运算符、语法自检清单、本环境可达性实测 |
| [`tests/`](tests/) | TDD 证据：三个压力场景、RED 基线的失败模式分析、GREEN 复跑与 REFACTOR 记录 |

## 安装

```bash
git clone https://github.com/marxchen7/zhongcheng-literature-search.git

# macOS / Linux
ln -s "$PWD/zhongcheng-literature-search" ~/.claude/skills/zhongcheng-literature-search
```

```powershell
# Windows（管理员 PowerShell）
New-Item -ItemType SymbolicLink `
  -Path "$env:USERPROFILE\.claude\skills\zhongcheng-literature-search" `
  -Target "F:\code\zhongcheng-literature-search"
```

装好后直接提问即可触发。例如：

> 我在写硕士论文，方向是中国钢铁行业低碳转型的政策工具组合与实施顺序（政策 sequencing）。导师让我先把文献调研做扎实。请帮我设计一套 Web of Science 的检索方案，要尽量找全相关文献。我没有检索式基础，你直接告诉我怎么做。

## 它是被测试逼出来的

按 writing-skills 的 TDD 纪律开发：先写压力场景 → 无 skill 跑 RED 基线 → 带 skill 跑 GREEN → 从 GREEN 结果反推缺口做 REFACTOR。

| 阶段 | 结果 |
|---|---|
| RED（无 skill） | 15 项判据：1 通过 / 1 部分 / 13 失败 |
| GREEN（带 skill） | 15 项判据首轮全通过 |

三个场景的失败模式互不相同，说明这是通用默认行为与钟澄方法的系统性差异，不是偶发失误：

| 场景 | 压力 | RED 失败模式 |
|---|---|---|
| S1 设计 | 无检索基础 + 时间压力 + 导师催 | 堆词组 + 综述方法论：查准率 / 召回率 / PRISMA-ScR 漏斗 / 引文滚雪球 |
| S2 诊断 | 同义词穷尽焦虑 + 结果过载 + AI 摘要诱惑 | 开头两句是对的，随后滑回"查准率崩塌 / 特异性 0.5"，并给 AI 开 conditional 许可 |
| S3 边界 | 文献过载 + 导师催 + AI 捷径诱惑 | 改述为"漏斗"，主方案是 VOSviewer / CiteSpace 领域结构图 + Elicit / Litmaps 批量摘要 |

REFACTOR 阶段从 GREEN 结果反向关闭了一个真实缺口：**由多种材料组成的器件名是一个最小单元，不能拆成组成元素**。skill 原文只说"拆到最小单元"，没明确说锌镍电池不可拆成 `zinc OR nickel`——而这是"结果一半不相关 + 标题看不懂"最常见的单一根因。修补依据全部来自原材料（`lithium ion batter*` 是作为一整个检索词测的，标题 34,018 条）。

详见 [`tests/refactor.md`](tests/refactor.md) 与 [`tests/baseline-analysis.md`](tests/baseline-analysis.md)。

## 说明

方法源自钟澄《科研论》。本仓库是对其文献检索部分的结构化整理与可执行化，案例中的命中量级与原文引述均取自原书。
