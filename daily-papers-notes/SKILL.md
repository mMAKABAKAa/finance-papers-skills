---
name: daily-papers-notes
description: |
  论文笔记生成（3 步流水线的第 3 步）。为本期"必读"论文生成金融研究视角的结构化笔记，
  包含 identification / 数据 / 机制 / 研究 gap & 未来方向，链接回填到推荐文件。
  目录页默认自动刷新，git 自动化默认关闭。

  触发词："批量笔记"、"跑一下论文笔记"
---

> **开始前**: 先说一声 "开始整理金融论文笔记 📓" 并告知今天日期。

# 论文笔记 (Finance Notes + Backfill)

3 步流水线的第 3 步。生成结构化金融笔记 → 链接回填 → 刷新目录页。

## Step 0: 读取共享配置

读取 `../_shared/user-config.json` 和（如存在）`../_shared/user-config.local.json`。

显式生成：

- `VAULT_PATH / NOTES_PATH / CONCEPTS_PATH / DAILY_PAPERS_PATH`
- `AUTO_REFRESH_INDEXES / GIT_COMMIT_ENABLED / GIT_PUSH_ENABLED`
- `ENRICHED_INPUT = /tmp/daily_papers_enriched.json`

## 前置检查

1. `/tmp/daily_papers_enriched.json` 存在且非空
2. 当期推荐文件 `{DAILY_PAPERS_PATH}/YYYY-MM-DD-期刊审稿.md` 存在
3. 任一不满足 → 提示用户跑前置步骤后退出

## 工作流程

### Step 1: 概念库补充

**1a: 提取概念**

1. 从当期审稿文件扫描所有 `[[...]]` 链接
2. 过滤掉论文笔记本身的 wikilink（如 `[[Battiston2026]]`），只保留概念词

**1b: 金融概念分类**

只保留以下类型（跳过通用词、人名、公司名、监管机构名）：

- **方法论 (Methods)**: DiD, IV, RDD, Event Study, Fama-MacBeth, GMM, Maximum Likelihood, Bootstrap, Bayesian, Structural Estimation
- **理论模型 (Theory)**: CAPM, Fama-French, Q-theory, Modigliani-Miller, Real Options, Agency Theory
- **数据 (Datasets)**: CRSP, Compustat, TAQ, Execucomp, Dealogic, Y-14, FFIEC Call Reports
- **机制概念 (Mechanisms)**: Information Asymmetry, Adverse Selection, Moral Hazard, Liquidity Premium, Limits to Arbitrage, Funding Constraints
- **政策/制度 (Institutions)**: MiFID II, Dodd-Frank, Basel III, JOBS Act, Reg FD, Sarbanes-Oxley

**1c: 创建缺失概念笔记**

按上面的子类放到 `{CONCEPTS_PATH}/` 对应目录。如果目录不存在，按上面 5 类创建（`Methods/ Theory/ Datasets/ Mechanisms/ Institutions/`）。

概念笔记模板（最小可用）：

```markdown
---
type: concept
category: {Methods/Theory/Datasets/Mechanisms/Institutions}
tags: [finance-concept]
---

# {概念名}

## 一句话定义

{1-2 句，不要长篇展开}

## 在金融研究中的角色

{这个概念在 finance literature 里通常用来解决什么问题}

## 关联论文

（自动累积，第 3 步会回填）
```

### Step 2: 论文笔记生成

读取当期审稿文件，**只为"必读"论文生成笔记**。

> **铁律**：
> - 只用 abstract + CrossRef 元数据，不调用 paper-reader（论文在 paywall 后，HTML/PDF 抓不到）
> - 笔记是 evidence-based 的初步分析，不要编 abstract 中没有的内容
> - 不确定的地方明确写"abstract 未提及"，不要瞎补

#### 笔记文件名

用第一作者姓 + 年份，如 `Battiston2026.md`。多作者也只取第一作者姓（`Cook` 而不是 `CookMarx`），简洁优先。同年同作者多篇，加 `a/b`：`Smith2026a.md`、`Smith2026b.md`。

#### 笔记保存位置

按主题归类放到 `{NOTES_PATH}/`：

- `资产定价/`、`公司金融/`、`银行与金融中介/`、`市场微观结构/`、`行为金融/`、`金融科技与监管/`、`房地产与家庭金融/`、`国际金融/`、`其他/`

如果目录不存在，按需创建。

#### 笔记模板（必须严格遵守这个结构）

```markdown
---
type: paper
journal: {Journal of Finance / ...}
date_published: YYYY-MM-DD
doi: 10.XXXX/...
authors: ...
tags: [finance-paper, {asset-pricing/corporate-finance/...}]
---

# {第一作者}{年}: {论文短标题}

> **来源**: 📰 {journal}（{date}） · [DOI](https://doi.org/...)
> **作者**: {完整作者列表}
> **机构**: {affiliations 或"abstract 未提及"}

## 1. 研究问题 (Research Question)

一句话——这篇在问什么。要具体到 outcome variable 和因果方向（"X 是否影响 Y？"，而不是"研究 X 与 Y 的关系"）。

## 2. 核心结论 (Main Findings)

- 结论 1（带数字，如 abstract 给出）
- 结论 2
- 结论 3（如有）

如果 abstract 没给具体 magnitude，明确写"abstract 未提供 magnitude，需查正文"。

## 3. 识别策略 (Identification)

- **方法**: {DiD / IV / RDD / Event Study / Structural / 描述性}
- **冲击/工具变量**: {具体是什么，如"2018 MiFID II 实施"、"行业平均工资 as IV"}
- **identifying assumption**: {parallel trends / exclusion restriction / continuity，写出来}
- **可能的 threats**: 基于 abstract 信息能想到的潜在 confounders（如果 abstract 信息不够，写"需读正文判断"）

## 4. 数据 (Data)

- **样本**: {样本范围、样本量}
- **时间**: {区间}
- **来源**: {CRSP / 监管机构 / 私有数据等，abstract 提到什么写什么}
- **频率**: {年/季/月/日}

## 5. 关键设定与系数 (Key Specifications)

> abstract 通常不写完整 spec。这一节如果 abstract 信息太少，可以只写一句"主要回归形式：Y = β·Treatment + X·γ + ε"作为占位，并标注"具体 spec 需查正文"。

如果 abstract 给出 magnitude，写：

- 主结果：β = ?（标准误 / t 值 / p 值）
- 经济意义：{X% / Y bps / 占样本均值多少}

## 6. 机制 (Mechanism)

- **作者声称的机制**: {abstract 里的 channel/mechanism}
- **支持机制的证据**: {如 heterogeneity test、subsample analysis}
- **替代解释**: {abstract 是否讨论？没讨论就标注"abstract 未涉及替代解释"}

## 7. 与已有文献的关系

- **Build on**: [[相关笔记或概念]] — 一句话说明
- **Extend**: ...
- **Refute / contradict**: ...
- **关联概念**: [[DiD]]、[[MiFID II]] 等

## 8. 锐评

3-5 句，从 referee 角度：

- identification 干净吗？identifying assumption 站得住吗？
- magnitude 是否 economically meaningful？
- 数据范围是否过窄？
- claim 和 evidence 对得上吗？
- 这篇在它所在的 sub-literature 里到底贡献了什么 delta？

## 9. 研究 Gap & 未来方向 ⭐️

> **这一节为用户自己服务——必须从用户的研究方向出发推导可能切入的 gap。**
>
> **用户研究方向**（来自 user-config 的 `research_focus`）：
> - ESG / sustainable investing、biodiversity & climate finance、institutional investors、shareholder activism、gender / diversity in finance
> - 美国上市公司，empirical 取向

至少给出 **3 条 Gap → Direction**，**优先列出与用户方向相关的切入角度**。每条要回答：这篇没做什么 + 你能怎么做。

- **Gap 1**: {这篇没解决的具体问题}
  **Direction**: {用什么数据 / identification 填这个空。优先用 13F / 13D / proxy voting / ESG ratings / Russell index reconstitution / NGO datasets 等用户熟悉的数据源}
- **Gap 2**: ...
- **Gap 3**: ...

**推导 Gap 的角度（用户方向相关，按优先级）：**

1. **Institutional channel gap** — 论文找到了一个 firm-level 现象，但有没有用 13F / 13D 数据看 institutional ownership 异质性？被动 vs 主动 investor 行为差异？
2. **Activism mechanism gap** — 是相关性还是因果？有没有 voting record / engagement letter / shareholder proposal 作为 mechanism evidence？
3. **ESG/biodiversity 维度** — 这个 setting 能不能套到 ESG / biodiversity / climate disclosure 上？或反过来，ESG 文献的方法能借来做这个？
4. **Gender / diversity 视角** — 论文里有没有性别异质性可挖（CEO 性别、董事会构成）？
5. **US 市场限制** — 如果是跨国样本，砍到 US 子样本结果是否一致？反过来，如果只用 US，能否扩到其他市场？
6. **数据 frontier** — 是否有更新的数据（如 Sustainalytics / MSCI 新版评级、新的监管披露规则）能让 setting 更干净？

可选：
- **Cross-pollination**: 这个 setting 能不能搬到 biodiversity / activism / gender 主题？
- **新数据需求**: 列举一个具体可获得的数据，如果接入能怎么改进？

## 10. 待确认 (Open Questions)

只在读完 abstract 还有疑问时写：

- {例：abstract 说 "exit rates higher for Black VC investments"，但没说统计显著性}
- {例：identification strategy 不够清楚，需查正文}
```

#### 质量校验（每篇必做）

生成后立即检查：

1. 文件行数 >= 80
2. 第 1、3、9 节非空（研究问题 / identification / gap）
3. 第 9 节至少 3 条 Gap → Direction
4. 没有 abstract 之外编造的 magnitude / 方法 / 数据集
5. 不满足任何一条 → 重写

> **绝对禁止**：
> - 编造 abstract 没有的回归系数 / 标准误 / t 值
> - 凭空说"作者还做了 robustness check"——abstract 通常不写
> - 把 CS 的概念（如 transformer / diffusion）塞进金融笔记
> - 在没读正文的情况下写"作者证明了 X" — 必须写"作者声称 X"

### Step 3: 笔记链接回填

**3a: 收集已有笔记**

Glob 扫描 `{NOTES_PATH}/` 下所有子目录（跳过 `{CONCEPTS_PATH}`），建立 `{文件名(不含.md): 相对路径}` 索引。

**3b: 匹配 + 回填**

对当期审稿文件每篇论文：

1. 用第一作者姓 + 年份生成预期文件名
2. 在索引里找匹配
3. 命中：在 `- **来源**:` 下方插入 `- 📒 **笔记**: [[文件名]]`（如已有则跳过）

**3c: 校正分流表 wikilink**

如果分流表里写的是 `[[Battiston]]` 但实际笔记是 `Battiston2026.md`，用 Edit 替换为 `[[Battiston2026]]`。

### Step 4: 刷新 MOC 索引

仅当 `AUTO_REFRESH_INDEXES=true`：

```bash
python3 ../_shared/generate_concept_mocs.py
python3 ../_shared/generate_paper_mocs.py
```

> 这两个脚本原本是为 CS 笔记设计的，对金融分类目录（如 `资产定价/`）应该也能正确扫描；如果生成的 MOC 显示乱了，告知用户脚本可能需要更新。

### Step 5: Git 提交

仅当 `GIT_COMMIT_ENABLED=true` 且 `VAULT_PATH/.git` 存在 + 有 staged changes：

```bash
cd {VAULT_PATH} && git add -A && git commit -m "finance papers: notes YYYY-MM-DD"
```

## 输出

- 创建了多少新概念
- 生成了多少篇论文笔记
- 回填了多少链接
- 流水线完成

## 注意事项

- **不调用 paper-reader**——它针对 CV/DL 论文，且金融论文 PDF 在 paywall 后抓不到
- 用户想精读时，自己把 PDF 放进 Zotero，再用 `读一下 Zotero 里的 论文标题` 触发 paper-reader（届时可以读 PDF）
- 仅为"必读"论文生成笔记
- "值得看"和"可跳过"不生成
- "必读"全部生成，一篇不能少；context 紧张时分批，**不能默默跳过**
