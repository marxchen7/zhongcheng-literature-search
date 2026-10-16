# 数据库语法速查（WoS / Scopus / CNKI）

> 钟澄《科研论》第三章附录 + 本机 2026-10-14 实测可达性。术语：TS / TI / TOPIC / TITLE-ABS-KEY / SU %= / 专业检索 / 行式界面。
> **本文件只管"怎么写对"，不管"怎么选词"**——选词见 search-formula.md，导入见 library-building.md。

---

## 0. 三库字段对照

| 含义 | Web of Science | Scopus | CNKI |
|---|---|---|---|
| 仅标题 | `Title` / `TI=` | `TITLE` | `TI` / `TI %=` |
| 标题 + 摘要 + 关键词 | **`Topic` / `TS=`** | `TITLE-ABS-KEY` | `SU` / `SU %=`（主题） |
| 关键词 | `AK=`（作者关键词） | `AUTHKEYWORD` | `KY` |
| 摘要 | `AB=` | `AB` | `AB` |
| 篇关摘（篇名+关键词+摘要） | — | — | `TKA` |
| 全文 | — | — | `FT` |
| 作者 | `AU=` | `AUTH` | `AU` |
| 第一作者 | `FP=` | — | `FI` |
| 机构 | `OG=` / `OO=` | `AFFIL` | `AF` |
| 来源出版物 | `SO=` | `SRCTITLE` | — |
| 出版年 | `PY=` | `PUBYEAR` | `YE` |
| 被引频次 | — | — | `CF` |
| 限定期刊名精确匹配 | — | `LIMIT-TO ( EXACTSRCTITLE , "..." )` | — |

**最容易错的一点**：钟澄的字段阶梯里"主题"指 **TOPIC = 标题 + 摘要 + 关键词**，不是 TITLE。

> 「注意的是 **TOPIC，不是 TITLE**。」

---

## 1. Web of Science

### 1.1 字段代码

`TS=` 主题（**召回主力**）｜`TI=` 篇名｜`AB=` 摘要｜`AK=` 作者关键词｜`AU=` 作者｜`SO=` 来源出版物｜`PY=` 出版年｜`DT=` 文献类型｜`LA=` 语言｜`OG=`/`OO=` 机构｜`CU=` 国家/地区（仅地址字段）

### 1.2 运算符

| 运算符 | 含义 | 示例 |
|---|---|---|
| `AND` | 收紧 | `A AND B` |
| `OR` | 放宽（同一行内的同义表达） | `(A1 OR A2)` |
| `NOT` / `AND NOT` | 排除 | `Li*Ni NOT Lipioni or Lithosiini` |
| `NEAR/n` | 两词相距 n 个词以内，顺序不限 | `"sequenc*" NEAR/5 "polic*"` |
| `SAME` | 仅地址字段：同一地址行内 | `AD=(... SAME ...)` |

**优先级**：WoS 布尔运算有固定优先级，不按从左到右。**每个概念组一律用括号包住**——但注意：**括号的语义在本书方法是"同一行内的同义表达"**，不是单纯的优先级工具（见 search-formula §4.2）。

### 1.3 高级检索组合历史记录

每跑一次检索，WoS 给一个编号（`#1`、`#2`…），可用 `#3 AND #4` 组合。这是调试手段，也是记录检索过程的原料。

### 1.4 导出参数（详见 library-building.md）

- 单次导出上限 **1000 条**（Endnote Desktop 格式 → `.ciw`）
- 分批：`1至1000`、`1001至2000`…共 N 个文件
- **弹窗最下方选记录内容，默认不含摘要——强烈建议加上摘要**
- 记录内容选项：**完整记录** / **全记录与引用的参考文献**

---

## 2. Scopus

### 2.1 必须用新版

> 老版本只支持 1 次导出 2000 条，而**新版本可以一次性导出更多的文献，上限为 20,000 条**。

页面有"尝试新版本（Try the new version）"链接。

### 2.2 行式界面（Document search）

每个概念占一行，**行内 OR、行间 AND**——与万能检索式同构：

```
TITLE-ABS-KEY ( personalit* ) AND TITLE-ABS-KEY ( model* ) AND TITLE-ABS-KEY ( animal* )
```

或单行内写全（案例原文）：

```
( TITLE-ABS-KEY ( personalit* ) AND TITLE-ABS-KEY ( model* ) AND TITLE-ABS-KEY ( animal* ) )
AND ( LIMIT-TO ( EXACTSRCTITLE , "animal behaviour" ) OR LIMIT-TO ( EXACTSRCTITLE , "behavioral ecology and sociobiology" ) )
```

### 2.3 LIMIT-TO：无法进一步收紧检索词时的兜底

```
LIMIT-TO ( EXACTSRCTITLE , "期刊名精确匹配" )
```

用于检索词已经无法再收紧、但结果仍过泛的场景（生态行为建模案例）。

### 2.4 导出

1. 第一条记录上方 `All` → `Select all`
2. `Export` → `RIS format`
3. **弹窗中默认不含摘要（Abstract）和关键字（Author keywords），把这 2 个字段选上**
4. 其他字段按需 → `Export` → `.ris`

---

## 3. CNKI（知网）

### 3.1 字段代码

`SU`=主题（**推荐 `SU %=`**）｜`TI`=篇名｜`KY`=关键词｜`AB`=摘要｜`TKA`=篇关摘｜`FT`=全文｜`AU`=作者｜`AF`=机构｜`FI`=第一作者｜`YE`=年｜`CF`=被引频次

### 3.2 运算符

| 场景 | 运算符 |
|---|---|
| 同字段内 | `*`（与）、`+`（或）、`-`（非） |
| 跨字段 | `AND`、`OR`、`NOT`（大写，前后空格） |
| 匹配方式 | `=` 精确、`%` 模糊、`%=` 相关匹配（**主题推荐**） |
| 年份 | `YE BETWEEN ('2015','2025')` |

### 3.3 专业检索模板

```
SU %= ('关键词A' + '同义词A2') AND SU %= ('关键词B' + '同义词B2')
```

这正是万能检索式的中文引擎版本：**括号内 `+` 是行内 OR，`AND` 是行间 AND。**

文科/中文案例实例（社会化阅读）：

```
社会化阅读 (主题) AND literature (主题)
```

### 3.4 高级检索网格（不熟专业检索语法时用）

逐行填同义词，**行内或、行间与**，效果与专业检索等价。

### 3.5 URL 参数法（本机可自动化，见 §4）

```
https://kns.cnki.net/kns8s/search?classid=YSTT4HG0&kw=<URL编码后的检索词>
```

**注意：URL 不支持专业检索语法（如 `SU %=`）**；高级检索式需通过网页表单提交。

---

## 4. 本环境可达性（2026-10-14 实测）

| 库 | 状态 | 路径 |
|---|---|---|
| **CNKI**（kns.cnki.net/kns8s） | ✅ 可自动 | 自动识别东南大学机构，无需个人登录即可检索。一框式检索 + URL 参数（`kw` + `korder=SU` 主题字段）可直接跑，返回题名/作者/期刊/日期/下载量与分面统计；结果页导出完整（GB/T 7714-2025、CAJ-CD、MLA、APA、Refworks、EndNote）。**专业检索页（`/AdvSearch?type=expert`）可打开、字段说明在页面上，但提交按钮在自动化会话中无响应**（页面加载腾讯验证码组件，专业检索模块 JS 未完全初始化）→ 走一框式 + URL 参数，不走专业检索提交 |
| **WoS**（webofscience.clarivate.cn） | ❌ 检索被拦 | SEU CAS 登录链路通（学号已在浏览器 profile 预填，Shibboleth 回跳成功到达 Core Collection），但**提交检索即触发 hCaptcha「There is unusual activity coming from your institution」**，程序化输入与真实键盘输入同样被拦 → **机构 IP 级反自动化标记，非输入方式问题**。未尝试破解验证码 |
| **可行路径** | — | CNKI 由 Claude 直接检索 + 导出；**WoS 走手动导出工作流——用户在浏览器检索后导出，Claude 处理**。仓库 `_wos9.json` 即 2026-05-30 的 WoS 导出格式样本 |

### 4.1 遇到不可用的库怎么做

**记录限制，不重复重试。** 原文依据（AGENTS.md 规则与 academic_search_priority）：数据库不可用则记录限制、继续用可访问的学术源补充，不要反复尝试同一条被拦路径。

具体到本 skill：

1. WoS 被拦 → 明确告知用户"WoS 在本环境需你手动导出"，给出导出的完整步骤（library-building §1），请用户导出后把 `.ciw` 文件给你处理。
2. CNKI 可用 → 直接执行一框式检索，返回命中数并记录。
3. 两个都不可用 → 记录限制，用四渠道（百度百科/百度搜索/CNKI 学术翻译助手/有道翻译）先把同义表达表和检索式定稿，等环境可用再执行。

---

## 5. 符号规则（三库通用，全部半角）

| 符号 | 含义 | 示例 |
|---|---|---|
| `*` | 0 个及以上字符 | `batter*` → battery / batteries / batter |
| `?` | 恰好 1 个字符 | — |
| `"..."` | 精确匹配（词组、顺序一致、中间不夹杂其他单词） | `"high entropy" alloy*` |
| `( )` | 分组；**在本书方法中标记"同一行内的同义表达"** | `(A1 OR A2 OR A3)` |
| `AND` / `OR` / `NOT` | 行间 AND，行内 OR；`AND NOT` 排除 | `Li*Ni NOT Lipioni or Lithosiini` |
| 大小写 | WoS / Scopus / CNKI 检索词不区分大小写 | `Si` = `si` |

**原文警示**：

> 「注意以上通配符都是半角而非全角模式。」
> 「包括我们后期提到的检索词中输入的其他符号也都是半角格式。」

### 5.1 什么时候不加双引号

加了双引号 = 严格匹配（单词顺序一致、中间不夹杂其他单词）。

- **该加**：冠心病案例中通过百度搜索和学术词典已基本明确同义表达（不采用这些表达方式的文献比例很少）→ 用双引号精确匹配。
- **不该加**：词组中间可能夹杂其他单词时（如 `photocatalytic` 与 `hydrogen` 之间有 for / Activity for 等）→ 加双引号会漏。

原文：「在之后的案例中，我们还会看到有些情况是不加双引号更好。」

### 5.2 化学式与特殊形式

- 化学式可直接作检索词：`Li(NiCoMn)O2`、`LiNi(0.6)Co(0.2)Mn(0.2)O(2)`、`LiNi(0.8)Co(0.1)Mn(0.1)O2`、`LixNi(0.5)Mn1.5O(4)(x equals to 0.96, 0.98, 1.0, 1.02 and 1.04)`。
- 通配符简写：`Li*Ni`（Li 与 Ni 之间有其他字符）。
- **排除误报**：`Li*Ni` 会命中 `Lipioni`、`Lithosiini` → `Li*Ni NOT Lipioni or Lithosiini`。
- 型号命名：NCM333、NCM523、NCM622、NCM811、NCA（N=Nickel、C=Cobalt、M=Manganese、A=Aluminum）。
- 期刊名/专辑名可能全大写（`LITHIUM-ION BATTERIES`），不要因大小写误判。

---

## 6. 语法自检清单

写完检索式后逐条过：

1. [ ] 所有符号是半角（引号、括号、`*`、`?`）
2. [ ] 每一行内只有 OR，没有 AND
3. [ ] 行与行之间只有 AND，没有 OR
4. [ ] 每个概念组都用括号包住
5. [ ] 单复数用 `*` 解决，没有列两个词条
6. [ ] 首轮未引入缩写（除非行业约定俗成用缩写）
7. [ ] 每个候选词组都回引擎测过命中量级（不是凭直译）
8. [ ] 标题测完才扩 TOPIC；TOPIC 不是 TITLE
9. [ ] 笼统词（如 `chinese`）已换成精确词组
10. [ ] 记录下了本轮命中数和日期
