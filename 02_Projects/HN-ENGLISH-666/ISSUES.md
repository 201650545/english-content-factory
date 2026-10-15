# HN-ENGLISH-666 · Issues (问题与仲裁追踪清单)

> 维护原则：**遇疑必记，不猜测，不隐瞒。所有影响真题入库、清洗分词或统计权重的问题均在此登记，由人类专家最终裁决。**

---

## 一、统一 Issue 记录模板

任何新增 Issue 必须严格按照以下模板结构记录：

```markdown
ID: ISS-XXXX
Status: open | needs_review | resolved | closed
Severity: blocker | critical | major | minor | trivial
Created: YYYY-MM-DD
Related Stage: SOURCE | INGEST | CLEAN | NORMALIZE | ANALYZE | VERIFY | ENRICH | CONTENT | RENDER | REVIEW | PUBLISH | FREE | ARCHIVE

Problem:
[清晰客观陈述遇到的具体问题、分词冲突、格式异常或规则缺失]

Evidence:
[列出具体出现问题的原卷文件、行号、词汇、代码报错日志或上下文片段]

Suggested Action:
[Agent 建议的解决动作或备选方案]

Human Decision:
[由人类教研专家/项目负责人填写的仲裁结论与处理指令]
```

---

## 二、当前问题登记簿

### ISS-0001

```markdown
ID: ISS-0001
Status: needs_review
Severity: major
Created: 2026-10-08
Related Stage: SOURCE | CLEAN | NORMALIZE | ANALYZE

Problem:
本子项目 PROJECT.md v0.0.1 的「非目标」条款明确声明：❌ 不获取真题、❌ 不解析PDF、❌ 不统计词频。
但 RUN-ANL-0001 实际已完整执行了真题获取、PDF/DOCX 解析与 OCR、词频统计与报告生成，
项目声明范围与已发生的工作实体存在直接冲突，状态字段仍停留在 `design` / `pending`。

Evidence:
- PROJECT.md 第 45–52 行「v0.0.1 非目标 (Non-Goals)」四条禁令。
- 09_runs/RUN-ANL-0001.md：实际产出 8 套真题、词频数据集与统计报告。
- 交付物：04_analysis/词汇筛选统计表.html、词汇筛选统计表.xlsx 等。

Suggested Action:
由项目负责人确认：(a) 是否将项目阶段由 `design` 正式推进至 `collecting / analyzing`；
(b) 是否据实修订 PROJECT.md 的非目标条款（v0.0.1 骨架已由人工指令突破）；
(c) 是否需要补充 2020、2021 年真题以对齐声明的 2020–2026 全区间。

Human Decision:
[待人类裁定]
```

### ISS-0006

```markdown
ID: ISS-0006
Status: needs_review
Severity: major
Created: 2026-10-09
Related Stage: INGEST | CLEAN | CONTENT

Problem:
原卷 docx 中，阅读理解 A 篇（图文应用文：通知/海报/邮件/公告）以及部分「阅读问答」短文
在原文里是**图片**，文本提取结果为空 —— 只剩题干与选项，短文丢失。

Evidence:
- 8 套卷的阅读理解A片段均为「A + 题干 + 选项」，无正文（2024 湖南省卷、2026 长沙卷除外）。
- 2022 长沙卷「阅读问答」、2026 湖南省卷「阅读问答」同样只有题干。
- 已用 `04_analysis/OCR对照文本/` 的 PaddleOCR 结果补回 9 处（见 RUN-ANL-0004）。

Suggested Action:
(a) 人工校对 9 处 OCR 补录文本（存在粘连词如 `EnglishReading`、个别错字）；
(b) 2021 长沙卷 A 篇短文**仍缺**（该卷无 PDF/OCR），需原始扫描件或另寻来源；
(c) 建议长期方案：所有卷以「PDF + OCR」为语料真源，而非依赖 docx 文本层。

Human Decision:
[待人类裁定]
```

### ISS-0007

```markdown
ID: ISS-0007
Status: needs_review
Severity: major
Created: 2026-10-09
Related Stage: SOURCE | VERIFY | CONTENT

Problem:
9 套真题中仅 4 套有参考答案，2024–2026 六套卷的答案缺失，专项册内只能标注「参考答案暂缺」。

Evidence:
- 有答案：2021/2022/2023 长沙卷（第一试卷网解析版，组卷网出品，含解析）、2025 湖南省卷（知乎公开页）。
- 无答案：2024 湖南省卷、2024 长沙卷、2025 长沙卷、2026 湖南省卷、2026 长沙卷。
- 已尝试渠道：第一试卷网（2024–2026 无解析版）、知乎（仅 2025 两套，其中长沙卷答案在图片中无法提取）、
  百度文库/豆丁/道客巴巴/原创力文档/学科网/教习网（均付费或积分墙）。

Suggested Action:
(a) 人类确认是否采购付费解析版（学科网组卷网为主要来源）；
(b) 或由教研老师人工做题产出参考答案；
(c) 或接受「先出无答案版、答案后续补」。

Human Decision:
[待人类裁定]
```

---

> 说明：除上述 ISS-0001 外，尚无其他阻断性缺陷。`METHODOLOGY.md` 中 14 项教研待定项（有效词界定、题干/选项/听力是否计入、人名地名处理、派生词归并、score 权重公式）仍全部为 `TBD`，需人类专家裁定后方可产出正式 `vocab_master`。

