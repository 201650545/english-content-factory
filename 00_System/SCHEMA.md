# Schema (数据模型与结构契约规范)

> 版本：**v0.0.1**  
> 声明：**本规范暂时只定义未来 `vocab_master` 的数据结构与字段语义，v0.0.1 不实际创建 666 条数据实体。**

---

## 一、存储载体与工程约束

1. **结构化存储格式**：`vocab_master` 属于高密度教育数据资产，未来生产生成后统一存储于 `03_data/vocab_master.csv`、`03_data/vocab_master.json` 或 `03_data/vocab.db (SQLite)`。
2. **严禁拆解为散落 Markdown**：严禁将该主词表拆解为 666 个微型 Markdown 文件，避免造成 Obsidian 知识库性能退化与状态管理混乱。Markdown 仅在 `05_content/` 中作为章节/单元级的教研合稿载体。

---

## 二、`vocab_master` 字段定义契约

以下为 `vocab_master` 核心实体数据结构定义：

| 字段名称 (Field) | 数据类型 (Type) | 必填性 (Required) | 字段语义与说明 |
| :--- | :--- | :---: | :--- |
| `rank` | Integer | 是 | **综合词频排名**。依据综合考查评分（`score`）从高到低排列的顺位（1 至 666）。 |
| `word` | String | 是 | **单词表层词形**。最终呈现给考生的目标词汇（通常为最常见原型）。 |
| `lemma` | String | 是 | **词元 / 原型还原**。用于词形归并的标准词形基底（例如：`went`、`goes` 均指向 lemma `go`）。 |
| `pos` | String | 是 | **词性标注 (Part of Speech)**。如 `v.`（动词）、`n.`（名词）、`adj.`（形容词）等，兼类词用分号或斜线分隔。 |
| `phonetic` | String | 是 | **国际音标**。标准英式/美式音标标注，用于发音启蒙与朗读指南。 |
| `meaning_cn` | String | 是 | **核心中文释义**。紧扣初中英语课程标准与湖南中考真题常见考查义项，避免罗列冷僻罕见义项。 |
| `frequency_total`| Integer | 是 | **语料总频次**。在收录的全量湖南中考英语真题语料文本中出现的绝对次数总和。 |
| `paper_count` | Integer | 是 | **真题卷覆盖数**。在收录的各年份/各地区真题试卷中，出现该词的独立试卷总套数。 |
| `year_count` | Integer | 是 | **考查年份跨度数**。出现该词的独立考查自然年总数（反映跨年份稳定性）。 |
| `years` | Array/String | 是 | **具体考查年份列表**。例如 `[2020, 2021, 2023, 2024]` 或逗号分隔字符串。 |
| `sections` | Array/String | 是 | **出现题型分布**。例如 `["完形填空", "阅读理解", "短文填空"]`，用于标注其在卷面中的功能角色。 |
| `score` | Float | 是 | **综合考查权重得分**。基于频次、卷次跨度、年份跨度及关键题型赋权计算出的算法量化值。 |
| `level` | String | 是 | **考标分级与重要度**。如 `核心高频 (P1)`、`高频进阶 (P2)`、`考点冲刺 (P3)`。 |
| `collocations` | Array/String | 否 | **真题核心搭配**。真题语料中高频共现的固定短语与词组（例如：`look forward to`）。 |
| `word_family` | Array/String | 否 | **同源词族与派生变形**。记录常见派生变化（如 `decide (v.) -> decision (n.) -> decisive (adj.)`）。 |
| `example` | String | 否 | **真题代表性例句**。精选自湖南中考真题原文的典型句子，附带中英文对照与年份标注。 |
| `notes` | String | 否 | **教研与审核备忘**。易混考点提示、熟词生义提醒、命题陷阱分析或人工审核批注。 |
| `verified` | Boolean | 是 | **人工核验状态**。标记本条词目是否通过教研专家的最终复核（`true` / `false`）。 |

---

## 三、单条结构示意示例 (JSON Representation Example)

```json
{
  "rank": 1,
  "word": "develop",
  "lemma": "develop",
  "pos": "v.",
  "phonetic": "/dɪˈveləp/",
  "meaning_cn": "发展；发育；开发；养成（习惯）",
  "frequency_total": 48,
  "paper_count": 12,
  "year_count": 6,
  "years": [2020, 2021, 2022, 2023, 2024, 2025],
  "sections": ["阅读理解", "完形填空", "任务型阅读"],
  "score": 98.5,
  "level": "核心高频 (P1)",
  "collocations": ["develop a habit", "develop skills", "with the development of"],
  "word_family": ["development (n.)", "developed (adj.)", "developing (adj.)"],
  "example": "Reading helps students develop good learning habits. (2024 湖南中考阅读理解)",
  "notes": "中考常考发展习惯与技能的搭配，需区分 developed 与 developing 词性用法。",
  "verified": false
}
```
