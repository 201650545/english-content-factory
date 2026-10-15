---
id: CN-ENGLISH-MULTI
type: project
version: 0.0.1
status: analyzing
title: 多地中考英语真题词汇频率库（2021–2026）
subject: English
region: Multi
regions:
  - Beijing
  - Shanghai
  - Guangdong
  - Hangzhou(Zhejiang)
  - Chongqing
  - Wuhan(Hubei)
exam: Zhongkao
years:
  - 2021
  - 2022
  - 2023
  - 2024
  - 2025
  - 2026
target_count: null
current_stage: analyzing
primary_scope: prov
scopes:
  - all   # 全量 29 套 / 6 地区（对照）
  - prov  # 省级统一命题 19 套 / 5 地区（主口径）
source_status: collected_unverified
corpus_status: draft
analysis_status: draft
content_status: pending
render_status: pending
publish_status: pending
human_review_required: true
---

# CN-ENGLISH-MULTI · 多地中考英语真题词汇频率库（2021–2026）

> 项目 ID：`CN-ENGLISH-MULTI`  
> 当前阶段：`analyzing`（词频统计草案已出，待人工裁定口径）  
> 上级系统：[Education Content Factory](file:///D:/Work/English-Content-Factory/00_System/README.md)  
> 姊妹项目：[`HN-ENGLISH-666`](file:///D:/Work/English-Content-Factory/02_Projects/HN-ENGLISH-666/PROJECT.md)（湖南，方法论同源）

---

## 一、项目目标

以**湖南项目已验证的词汇流水线**为方法基座，横向扩展到 6 个地区（北京 / 上海 / 广东 / 杭州（浙江）/ 重庆 / 武汉）2021—2026 年中考英语真题语料，建立**跨地区可比、可复现、可追溯**的词频数据集，为后续「多地高频词对照」「地区差异分析」「通用高频词产品化」提供数据底座。

---

## 二、v0.0.1 非目标 (Non-Goals)

* ❌ **不产出最终词表**：本阶段只做词频数据集草案，不补 `pos / 音标 / 释义 / score` 字段。
* ❌ **不做跨地区等效性裁决**：各地区命题性质不同（省统考 / 市自主 / 直辖市卷），不做等权比较结论。
* ❌ **不生成成品排版文件**：不生成出版级 PDF/EPUB。
* ❌ **不发布社交媒体**：不进行对外宣传与渠道上架。
* ❌ **不建立收费系统**：不搭建支付与鉴权模块。

---

## 二之二、统计口径（ISS-0003 裁定）

本项目设**两套口径**，由脚本环境变量 `ZK_SCOPE` 切换，共用同一套分词/归并/去噪算法，结果可直接对比：

| 口径 | ZK_SCOPE | 卷数 / 地区 | 说明 |
| :--- | :--- | :--- | :--- |
| **省级统一命题（主口径）** | `prov` | **19 套 / 5 地区** | 北京 6 + 上海 2 + 重庆 6 + 广东 2024–2026（3）+ 浙江 2024–2025（2）；含直辖市全市统一卷 |
| 全量（对照口径） | `all` | 29 套 / 6 地区 | 另含武汉 5 套、杭州 2021–2023（3）、广东 2022–2023（2） |

> 人类裁定（2026-10-09）：**只看省统考** → 采用省级统一命题口径作为主分析口径，全量口径保留作对照。
> 杭州 2024/2025 两套实为浙江省统一卷，主口径下归名为「浙江」。

---

## 三、项目专属工作目录规范

| 目录 | 命名与定位 | 读写权限 / 职责说明 |
| :--- | :--- | :--- |
| `01_raw/` | 原始材料层 | **只读 / Immutable**。存放原始 rar 附件、解包 docx 原文、来源网页快照。禁止就地修改。 |
| `02_clean/` | 清洗文本层 | 存放由 docx 抽取、剥离排版噪点后的纯净试卷文本（统计输入）。 |
| `03_data/` | 结构化数据层 | 存放 `filter_multi.json` 及全部词频 CSV（汇总 / 分地区 / 分卷）。 |
| `04_analysis/` | 统计分析层 | 存放交互式 HTML 报告与多 sheet Excel 工作簿。 |
| `09_runs/` | 运行追踪层 | 存放本次运行的 Run Log。 |

> 说明：本项目当前不设 `05_content`–`08_publish` 目录，待人类裁定进入 ENRICH 阶段后再按需建立。

---

## 四、关键治理文件链接

* **本次交付说明（主口径 · 省统考）**：[`本次交付说明_省统考口径.md`](file:///D:/Work/English-Content-Factory/02_Projects/CN-ENGLISH-MULTI/本次交付说明_省统考口径.md)
* **本次交付说明（全量对照）**：[`本次交付说明.md`](file:///D:/Work/English-Content-Factory/02_Projects/CN-ENGLISH-MULTI/本次交付说明.md)
* 来源台账：[`SOURCES.md`](file:///D:/Work/English-Content-Factory/02_Projects/CN-ENGLISH-MULTI/SOURCES.md)
* 缺陷与仲裁记录：[`ISSUES.md`](file:///D:/Work/English-Content-Factory/02_Projects/CN-ENGLISH-MULTI/ISSUES.md)
* 运行日志：[`09_runs/RUN-ANL-0002.md`](file:///D:/Work/English-Content-Factory/02_Projects/CN-ENGLISH-MULTI/09_runs/RUN-ANL-0002.md)（全量口径）、[`09_runs/RUN-ANL-0003.md`](file:///D:/Work/English-Content-Factory/02_Projects/CN-ENGLISH-MULTI/09_runs/RUN-ANL-0003.md)（省统考口径）
* 流水线脚本：[`90_Automations/cn-english-multi/`](file:///D:/Work/English-Content-Factory/90_Automations/cn-english-multi/)
* 系统行为准则：[`AGENT_RULES.md`](file:///D:/Work/English-Content-Factory/00_System/AGENT_RULES.md)
* 全局流水线标准：[`WORKFLOW.md`](file:///D:/Work/English-Content-Factory/00_System/WORKFLOW.md)
* 数据结构字典：[`SCHEMA.md`](file:///D:/Work/English-Content-Factory/00_System/SCHEMA.md)

---

## 五、当前进度快照（截至 2026-10-09）

| 阶段 | 状态 | 产物位置 |
| :--- | :--- | :--- |
| SOURCE（真题搜集） | 已收集 **29 套 / 6 地区**，**待验证**，存在 4 处缺口（见 SOURCES.md） | `01_raw/` |
| CLEAN（文本清洗） | 草案就绪（29 套纯文本） | `02_clean/试卷文本/` |
| ANALYZE（词频统计 · 省统考主口径） | 草案就绪（19 套 / **49,160 词次 / 3,251 原形 / 3,095 内容词**） | `03_data/省统考口径/` |
| ANALYZE（词频统计 · 全量对照） | 草案就绪（29 套 / 72,641 词次 / 3,932 原形 / 3,763 内容词） | `03_data/` |
| 报告产出 | HTML + Excel 各 2 套（两口径） | `04_analysis/`、`04_analysis/省统考口径/` |
| ENRICH 及之后 | 未开始（依赖 ISS-0004/0005 裁定） | — |

