---
title: "{Title}"
method_name: "{FirstAuthor}{Year}"
authors: [{Authors}]
year: {Year}
venue: {Venue}
journal_field: {finance | accounting | economics}
paper_type: {empirical | theory | structural | descriptive | methodology}
tags: [{tags}]
zotero_collection: {zotero_path}
doi: {doi}
ssrn: {ssrn_url}
created: {date}
---

# {FirstAuthor}{Year}: {论文短标题}

## 元信息

| 项目 | 内容 |
|------|------|
| 期刊 | {venue}, {Year}, Vol/Issue |
| 作者 | {完整作者列表} |
| 机构 | {Affiliations} |
| JEL | {JEL classifications, if listed} |
| 链接 | [DOI]({doi_url}) / [SSRN]({ssrn_url}) / [NBER]({nber_url}) |

> **论文类型**: {empirical / theory / structural / descriptive}
> **样本市场**: {US-listed firms / cross-country / specific market}
> **样本期**: {YYYY-YYYY}

---

## 一句话总结

> {一句话讲清这篇做什么、用什么方法、得出什么结论。要有数字或方向。}

---

## 1. 研究问题 (Research Question)

> 这一节必须用一句精确的问句开头。outcome variable + treatment variable + 因果方向都要点明。

{核心问题：X 是否（如何）影响 Y？为什么这个问题重要？}

**Why now**：{这个问题最近为什么 timely——监管变化、数据可得、文献空白等}

---

## 2. 核心结论 (Main Findings)

> 必须带具体数字。如果是理论论文，写关键命题的方向 + 边界条件。

- **结论 1**: {…，β = …，t-stat = …，经济意义 = …}
- **结论 2**: {…}
- **结论 3**: {…}
- **核心贡献的 delta**：相比之前的文献，这篇新增了什么具体的事实/机制？

---

## 3. 识别策略 / 模型设定 (Identification / Model Setup)

### 类型: {empirical 见 3a；theory 见 3b}

#### 3a. 实证识别（empirical 论文）

- **策略**: {DiD / IV / RDD / Event Study / Structural / Matching / Descriptive}
- **冲击或工具变量**: {具体是什么——监管事件、Russell 2000 reconstitution、自然灾害、CEO 死亡等}
- **Identifying assumption**: {parallel trends / exclusion restriction / continuity / monotonicity—文章是怎么 argue 的}
- **作者做了哪些 robustness**: {placebo、不同 bandwidth、子样本、安慰剂处理时点等}
- **可能仍然存在的 threats**: {作者没完全解决的内生性来源}

#### 3b. 理论模型（theory 论文）

- **模型框架**: {Kyle / Holmström-Tirole / global games / signaling / continuous-time…}
- **关键 agents**: {…}
- **关键时间点**: {t=0, t=1, t=2…每一步发生什么}
- **关键参数**: {…+ 参数的解释}
- **关键假设**（按重要性排序）:
  1. {假设 1，及其 economic interpretation}
  2. {假设 2}
  3. {假设 3}
- **解的类型**: {closed-form / numerical / equilibrium refinement}

---

## 4. 数据 / 模型变量 (Data / Variables)

#### 实证论文

| 项 | 内容 |
|---|---|
| 主样本 | {N obs / N firms / N firm-years} |
| 时间 | {YYYY-YYYY} |
| 国家/市场 | {US, S&P 500, Russell 1000…} |
| 数据来源 | {CRSP, Compustat, 13F, 13D, Execucomp, ISS, MSCI, Sustainalytics, Refinitiv…} |
| 主 outcome | {…} |
| 主 treatment | {…} |
| 主 controls | {…} |

#### 理论论文

- **State variables**: {…}
- **Choice variables**: {agents 的选择空间}
- **外生分布**: {噪声、信号的分布假设}

---

## 5. 关键设定与系数 / 关键命题 (Key Specifications / Propositions)

### 5a. 关键回归（实证）

主 spec：

$$
Y_{i,t} = \beta_1 \cdot \text{Treatment}_{i,t} + X_{i,t}'\gamma + \alpha_i + \delta_t + \varepsilon_{i,t}
$$

| spec | β (主系数) | 标准误 | t / p | obs | 经济意义 |
|---|---|---|---|---|---|
| Baseline | … | (…) | … | … | … |
| + firm FE | … | (…) | … | … | … |
| + industry-year FE | … | (…) | … | … | … |

### 5b. 关键命题（理论）

按重要性列出 propositions / lemmas。每条：陈述 → 经济直觉 → 关键比较静态。

- **Proposition 1**: {陈述}
  - 直觉：{为什么这个结论成立}
  - 比较静态：{核心参数变化的方向}
- **Proposition 2**: {…}

---

## 6. 关键公式

> 理论论文这一节会很重——把所有命题/推论的核心公式列出。
> 实证论文如果只有 reduced-form regression，5a 已经覆盖；这一节可写"无核心公式"。

### 公式 1: [[概念名|公式用途]]

$$
\text{LaTeX 公式}
$$

**含义**: {一句话解释}

**符号说明**:
- ${符号 1}$: {含义}
- ${符号 2}$: {含义}

{... 列出全部核心公式 ...}

---

## 7. 关键图表

> 不可省略。论文中所有 Figure 和 Table 必须出现。

### Figure 1: {标题}

![{caption}]({url 或 ![[本地图]]})

**说明**: {主要发现，及其与核心结论的对应关系}

### Table 1: {标题}

| col1 | col2 | col3 |
|---|---|---|
| … | … | … |

**说明**: {核心发现}

{... 列出全部 Figure 和 Table ...}

---

## 8. 机制 (Mechanism)

> 这一节是金融论文的灵魂——主回归只是 reduced form，机制证据才是说服力来源。

- **作者声称的机制**: {channel A / channel B}
- **支持机制的证据**:
  1. {Heterogeneity test：哪些 subsample 效应更强？为什么这支持 channel A 而不是 channel B}
  2. {Mediator：是否引入 mediator 变量后主系数显著下降}
  3. {Direct evidence：voting record / engagement letter / disclosure 等"硬"证据}
- **替代解释及作者怎么排除**:
  - 替代解释 1：{…}，作者用 {test} 排除
  - 替代解释 2：{…}，作者用 {test} 排除

---

## 9. 与已有文献的关系 (Related Literature)

### Build on
- [[Author1Year]]：{这篇 build on 它的什么 — 数据 / 方法 / 设定？}
- [[Author2Year]]：{…}

### Extend
- [[Author3Year]]：{在它的基础上扩展了什么}

### Refute / Contradict
- [[Author4Year]]：{这篇与它结论相反，谁的证据更可信？}

### 关联概念
- [[concept1]]、[[concept2]]、[[concept3]]

---

## 10. 锐评 (Critical Assessment)

> 至少 5-6 句，从 referee 角度逐项点评。

### 优点
- {Identification 的某个具体环节做得漂亮}
- {数据来源新颖 / 样本干净}
- {机制证据充分}

### 局限
- {Identifying assumption 的某个具体 threat}
- {样本范围 / 时间窗口的 trade-off}
- {Magnitude 是否 economically meaningful}
- {Claim 是否超出了 evidence 能支持的范围}

### 与你研究方向的距离 ⭐️

> 用户研究方向：ESG / sustainable investing、biodiversity & climate finance、institutional investors、shareholder activism、gender / diversity in finance。US-listed firms。

- **直接相关**: {如果是，哪部分对你最有用}
- **方法可借鉴**: {即便主题不直接相关，方法/setting 能否搬到你的方向}
- **数据可借鉴**: {新数据源是否能在你的研究里复用}

---

## 11. 研究 Gap & 未来方向 ⭐️⭐️⭐️

> **核心节。把这篇论文留下的空隙用"Gap → Direction"格式列出，至少 3 条。优先列对用户研究方向 actionable 的角度。**
>
> 用户方向：ESG / sustainable investing、biodiversity & climate finance、institutional investors、shareholder activism、gender / diversity in finance（美国市场为主）。

### Gap 1: {具体描述这篇没解决的问题}

**Direction**: {用什么数据 / identification 来填这个空。优先用 13F、13D、proxy voting、ESG ratings、Russell 2000 reconstitution、SEC disclosure 等用户熟悉的来源。}

**可行性**: {high / medium / low — 数据是否可得、identification 是否站得住脚}

### Gap 2: …

**Direction**: …

### Gap 3: …

**Direction**: …

### 推导 Gap 用的角度（参考）

- **Institutional channel gap** — firm-level 现象有没有用 13F/13D 看 institutional ownership 异质性？被动 vs 主动 investor 行为差异？
- **Activism mechanism gap** — 是相关性还是因果？有没有 voting record / engagement letter 作为 mechanism evidence？
- **ESG / biodiversity 维度** — 这个 setting 能不能套到 ESG / biodiversity / climate disclosure 上？
- **Gender / diversity 视角** — 论文里有没有性别异质性可挖（CEO 性别、董事会构成）？
- **US 市场限制** — 跨国样本砍到 US 子样本结果是否一致？反过来，US-only 能否扩到其他市场？
- **数据 frontier** — 是否有更新的数据（新版评级、新监管披露规则）能让 setting 更干净？

### Cross-pollination

- **可搬到的 setting**: {这篇的 identification 能不能套用到 biodiversity / activism / gender 主题上？}
- **新数据需求**: {如果有 X 数据接入，这个研究方向能怎么改进？}

---

## 12. 待确认 (Open Questions)

> 这一节列出读完仍然 unclear 的点。等再读 paper / 找人讨论时回来填。

- {Open question 1}
- {Open question 2}

---

## 速查卡片

> [!summary] {FirstAuthor}{Year}: {Short Title}
> - **问题**: {一句话研究问题}
> - **方法**: {DiD / IV / 理论 + …}
> - **数据/setting**: {US listed, 2010-2022, …}
> - **核心发现**: {带数字的一句话}
> - **对你方向的用法**: {Build on / 方法借鉴 / 不直接相关}
> - **必读 Gap**: {一句话最重要的 gap}
> - **{venue} {Year}** | DOI: {doi}

---

*笔记创建时间: {timestamp}*
