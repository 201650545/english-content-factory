---
id: HN-ENGLISH-666
type: project
version: 0.0.1
status: design
title: 湖南中考英语真题高频666词
subject: English
region: Hunan
exam: Zhongkao
years:
  - 2020
  - 2021
  - 2022
  - 2023
  - 2024
  - 2025
  - 2026
target_count: 666
current_stage: design
source_status: collected_unverified
corpus_status: draft
analysis_status: draft
content_status: pending
render_status: pending
publish_status: pending
human_review_required: true
---

# HN-ENGLISH-666 · 湖南中考英语真题高频666词

> 项目 ID：`HN-ENGLISH-666`  
> 当前阶段：`design` (架构设计中)  
> 上级系统：[Education Content Factory](file:///D:/Work/English-Content-Factory/00_System/README.md)

---

## 一、项目目标

基于 2020—2026 年湖南中考英语真题语料，建立严密、可复现、可追溯的词频数据库，并形成“湖南中考英语真题高频666词”出版级教育内容产品。

---

## 二、v0.0.1 非目标 (Non-Goals)

为恪守最小可运行骨架原则，v0.0.1 严禁执行以下任务：

* ❌ **不获取真题**：不进行真题下载或外部抓取。
* ❌ **不解析PDF**：不执行 PDF 文本抽取或 OCR 识别。
* ❌ **不统计词频**：不跑批分词与词频矩阵计算。
* ❌ **不生成最终PDF**：不生成成品排版文件。
* ❌ **不发布社交媒体**：不进行任何对外宣传与渠道上架。
* ❌ **不建立收费系统**：不搭建支付与鉴权模块。

---

## 三、项目专属工作目录规范

| 目录 | 命名与定位 | 读写权限 / 职责说明 |
| :--- | :--- | :--- |
| `01_raw/` | 原始材料层 | **只读 / Immutable**。存放试卷扫描件、官方文件，禁止就地修改。 |
| `02_clean/` | 清洗文本层 | 存放剥离了试卷格式排版噪点、准考证号提示后的纯净真题文本。 |
| `03_data/` | 结构化数据层 | 存放结构化试卷 JSON 以及未来核心数据表（如 `vocab_master.csv`）。 |
| `04_analysis/` | 统计分析层 | 存放词频统计报告、矩阵分析结果与相关分布产物。 |
| `05_content/` | 教学内容层 | 存放 666 词的教研讲解文案、例句注释、考点卡片 Markdown。 |
| `06_templates/`| 版式模板层 | 存放 Typst/HTML/Markdown 页面排版模版与样式资产。 |
| `07_outputs/` | 排版成品层 | 存放最终排版构建出的出版样书、样张文件（PDF/EPUB等）。 |
| `08_publish/` | 发行就绪层 | 存放签署 `approved` 后待上线或分发的公开发布包。 |
| `09_runs/` | 运行追踪层 | 存放所有产生文件变动的 Agent 运行日志（Run Logs）。 |

---

## 四、关键治理文件链接

* **本次交付说明（题型专项）**：[`本次交付说明_题型专项.md`](file:///D:/Work/English-Content-Factory/02_Projects/HN-ENGLISH-666/本次交付说明_题型专项.md)
* **本次交付说明（词频统计）**：[`本次交付说明.md`](file:///D:/Work/English-Content-Factory/02_Projects/HN-ENGLISH-666/本次交付说明.md)（结果清单、目录导览、筛选口径、待决事项）
* 来源台账：[`SOURCES.md`](file:///D:/Work/English-Content-Factory/02_Projects/HN-ENGLISH-666/SOURCES.md)
* 统计与教研方法论：[`METHODOLOGY.md`](file:///D:/Work/English-Content-Factory/02_Projects/HN-ENGLISH-666/METHODOLOGY.md)
* 缺陷与仲裁记录：[`ISSUES.md`](file:///D:/Work/English-Content-Factory/02_Projects/HN-ENGLISH-666/ISSUES.md)
* 系统行为准则：[`AGENT_RULES.md`](file:///D:/Work/English-Content-Factory/00_System/AGENT_RULES.md)
* 全局流水线标准：[`WORKFLOW.md`](file:///D:/Work/English-Content-Factory/00_System/WORKFLOW.md)
* 数据结构字典：[`SCHEMA.md`](file:///D:/Work/English-Content-Factory/00_System/SCHEMA.md)

---

## 五、当前进度快照（截至 2026-10-09）

> ⚠️ 本节由 `RUN-ANL-0001` / `RUN-ANL-0004` 写入，记录**超出 v0.0.1 非目标范围**的实际进度，待人类裁定（见 ISS-0001）。

| 阶段 | 状态 | 产物位置 |
| :--- | :--- | :--- |
| SOURCE（真题搜集） | 已收集 **9 套（2021–2026）**，**待验证**；2021 长沙卷本次补齐 | `01_raw/` |
| CLEAN（文本清洗） | 草案就绪（9 套纯文本 + 3 套解析版文本） | `02_clean/` |
| ANALYZE（词频统计） | 草案就绪（17,138 词次 / 2,100 原形 / 1,992 内容词） | `03_data/`、`04_analysis/` |
| **CONTENT（题型专项）** | **10 个专项 HTML 已产出**（阅读理解 A/B/C/D、选句填空、完形、语法填空、阅读问答、翻译、书面表达） | `03_Products/04_题型专项/` |
| 报告产出 | 词频 HTML + Excel + CSV 就绪 | `04_analysis/` |
| ENRICH 及之后 | 未开始（依赖 METHODOLOGY 14 项 TBD 裁定 + ISS-0006/0007） | — |

### 已知缺陷（详见 `ISSUES.md`）

- **ISS-0006**：A 篇短文在原卷 docx 中为图片 → 已用 OCR 补回 9 处；**2021 长沙卷 A 篇仍缺**。
- **ISS-0007**：2024–2026 六套卷**参考答案缺失**，专项册内已标注「暂缺」。


