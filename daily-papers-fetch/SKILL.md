---
name: daily-papers-fetch
description: |
  论文抓取（3 步流水线的第 1 步）。从金融经济会计顶刊（CrossRef API 按 ISSN 抓取）拉取最近一周的新论文，
  打分筛选，输出到 /tmp/daily_papers_enriched.json 供后续 skill 使用。

  触发词："论文抓取"、"跑一下论文抓取"
  支持周期模式："本周新论文"、"过去两周论文"、"最近 3 天" → --days N
---

> **开始前**: 先说一声 "开始抓取期刊新论文 📰" 并告知今天日期 + 检测窗口（默认 7 天）。

# 论文抓取 (Fetch + Filter)

3 步流水线的第 1 步。读取期刊配置 → 调用 CrossRef API → 时间窗口过滤 → 关键词打分 → 输出 JSON。

## Step 0: 读取共享配置

读取 `../_shared/user-config.json`，如有 `../_shared/user-config.local.json` 则覆盖。

显式生成并使用：

- `VAULT_PATH`、`DAILY_PAPERS_PATH`
- `SOURCE = source 字段`（应为 `"journals"`）
- `JOURNALS = journals 列表`
- `KEYWORDS / NEGATIVE_KEYWORDS / DOMAIN_BOOST_KEYWORDS / MIN_SCORE / TOP_N`

## 解析窗口

从用户输入解析窗口大小，存为 `DAYS_ARG`：

- "本周"、"过去一周"、"最近 7 天"、未明确指定 → `--days 7`
- "过去 3 天" → `--days 3`
- "过去两周" → `--days 14`
- "过去一个月"、"本月" → `--days 30`

注意：金融期刊不每天发新论文，最小有意义窗口是 7 天。

## 工作流程

### Phase 1+2: 抓取 + 打分（纯 Python）

```bash
python3 ../daily-papers/fetch_and_score.py --days {DAYS_ARG} > /tmp/daily_papers_top30.json
```

脚本自动完成：

- 遍历 `JOURNALS` 列表，对每本期刊调用 CrossRef `/journals/{ISSN}/works` API
- 用 `--days N` 作为发表日期窗口，过滤掉超过窗口的旧论文
- **若某期刊在窗口内没有新论文，跳过该期刊（stderr 输出 [SKIP]）**
- 按关键词 + 领域加分打分，过滤负向关键词
- 跨期刊去重（按 DOI），按 score 降序取 Top N

### Phase 3: 富化（期刊模式：跳过）

期刊论文藏在 paywall 后，HTML/PDF 抓不到。**直接复制为 enriched.json，不调用 enrich_papers.py**：

```bash
cp /tmp/daily_papers_top30.json /tmp/daily_papers_enriched.json
```

CrossRef 已经返回了 abstract、authors、DOI、发表日期等核心字段，足够第 2 步点评使用。

## 空结果处理

如果 `/tmp/daily_papers_enriched.json` 是空数组 `[]`：

1. 告知用户："本{窗口描述}没有期刊新论文，流水线终止"
2. **不要继续运行后续步骤**
3. 退出（用户可以下周再试）

## 输出

非空时告知：

- 哪些期刊有新论文（跳过了哪些）
- 抓取了 N 篇
- 提示运行下一步：`跑一下论文点评`

## 注意事项

- 仅支持期刊模式（`source: "journals"`）。配置里如果 `source` 缺失或为其他值，脚本会回退到旧的 arXiv+HF 模式
- 如果 CrossRef API 失败（HTTP 错误、超时），stderr 会有 [WARN]，该期刊跳过
- 不做 git 操作
