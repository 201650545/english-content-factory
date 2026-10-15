# CN-ENGLISH-MULTI · Issues (问题与仲裁追踪清单)

> 维护原则：**遇疑必记，不猜测，不隐瞒。所有影响真题入库、清洗分词或统计权重的问题均在此登记，由人类专家最终裁决。**

---

## 一、统一 Issue 记录模板

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

### ISS-0002

```markdown
ID: ISS-0002
Status: open
Severity: major
Created: 2026-10-09
Related Stage: SOURCE | INGEST

Problem:
6 个地区 2021—2026 全覆盖未达成，共 6 处缺口：上海 2023–2026（4 套）、浙江 2026（1 套）、武汉 2025（1 套），
另有广东 2021（当年无省统一卷）、重庆 2021–2024 B 卷未取。上海仅 2 套，样本量显著偏小。

Evidence:
- 第一试卷网（shijuan1.com）站内检索无对应文章；已抓取列表页 54 页（`01_raw/原始下载件/来源网页/列表页/`）逐一核对。
- 备用渠道核查结果：百度文库 / 豆丁网 / 人人文库（renrendoc）/ 51wendang.com / max.book118.com / 学科网 / 教习网
  均为**付费或积分墙**，无免费直链下载。

Suggested Action:
(a) 人类确认是否接受当前 29 套作为 v0.0.1 语料基线；
(b) 若须补齐，建议路径：各地教育考试院官网及官方公众号历史发布、付费平台采购、教研组线下交换；
(c) 若上海仅 2 套，建议在报告中对其词频标注「低样本，代表性弱」并不参与跨地区等权比较。

Human Decision:
[待人类裁定]
```

### ISS-0003

```markdown
ID: ISS-0003
Status: resolved
Severity: major
Created: 2026-10-09
Resolved: 2026-10-09
Related Stage: ANALYZE | VERIFY

Problem:
各地区命题性质不统一，跨地区词频**不可直接等权比较**：
- 北京 / 上海 / 重庆 = 直辖市自主命题；武汉、长沙 = 市自主命题；
- 广东 2024 起省统一（2021 无省卷）；浙江 2024 起省统一；
- 杭州 2024–2025 实际入库的是**浙江省统一卷**，而非杭州自命题卷。

Evidence:
- `SOURCES.md` 第 17、18 行标注为浙江省统一卷。
- `交付说明.md` 第六节已提示该风险。

Suggested Action:
由教研负责人裁定：(a) 跨地区统计是否需按「命题性质」分层（省卷层 / 市自主层 / 直辖市层）；
(b) 杭州 2024–2025 是否应从「杭州」地区样本中剔除或单列「浙江」；
(c) 是否需要在最终产品中显式声明地区可比性边界。

Human Decision:
2026-10-09 人类裁定：**「只看省统考」→ 采用「省级统一命题」口径**。
- 按教育部「中考省级统一命题」政策口径，**直辖市的全市统一命题计入省级统一命题**；
- 故口径内含北京、上海、重庆（直辖市全市统一卷）；
- 排除武汉（市自主）、杭州 2021–2023（市自主）、广东 2022–2023（地市自主）；
- 杭州 2024–2025 归名为「浙江」。

执行结果（RUN-ANL-0003）：
- 主口径 = 省级统一命题：**19 套 / 5 个省级行政区**（北京 6、上海 2、重庆 6、广东 3、浙江 2）；
- 数据规模：49,160 词次 / 3,251 原形 / 3,095 内容词；
- 全量 29 套口径保留为对照；
- 脚本以 `ZK_SCOPE=prov|all` 切换，共用同一套算法，两口径结果可直接对比。

遗留提示（未单独立 Issue）：主口径下上海、浙江各仅 2 套，样本量小，跨地区比较须注意。
```

### ISS-0004

```markdown
ID: ISS-0004
Status: needs_review
Severity: minor
Created: 2026-10-09
Related Stage: NORMALIZE | ANALYZE

Problem:
上海卷存在 `(b)uy` 式「首字母提示拼写」题型，原始文本会把提示字母与词尾粘成 `buy` 或残留 `b`；
另有短词噪声（`dr` `uk` `ii` `sb` `tk` `uy` `vr` 等）混入统计。
已做处理：用正则 `\(([A-Za-z])\)([A-Za-z]{2,})` 还原完整单词；新增「短词·缩写（长度≤2）」类别，
内容词表已剔除功能词与短词两类。

Evidence:
- `90_Automations/cn-english-multi/wordfreq_multi.py` 中 `PAREN_HINT` 与 `cat_of()`。
- 上海样本 `上海_2022_上海卷.txt`。

Suggested Action:
由教研确认：长度 ≤2 的 token（如 `Dr` `VR` `UV` `EA` `UK`）应保留为缩写词还是统一剔除；
以及 `(b)uy` 还原规则是否对所有年份上海卷适用。

Human Decision:
[待人类裁定]
```

### ISS-0005

```markdown
ID: ISS-0005
Status: needs_review
Severity: minor
Created: 2026-10-09
Related Stage: INGEST | ANALYZE

Problem:
29 套试卷均来自第三方教育网站免费公开资源，**非各地考试院官方渠道**；`是否完整` 字段默认填 `true`
但未逐卷人工比对题型是否齐全（听力文本 / 参考答案是否包含）。版权与使用范围亦未确认。

Evidence:
- `SOURCES.md` 全表 `是否已验证 = false`、`是否允许进入语料库 = false`。

Suggested Action:
人类确认：(a) 是否需要逐卷核对完整性；(b) 语料使用与再分发的授权边界；
(c) 在未取得官方来源前，本项目产物是否仅限内部使用。

Human Decision:
[待人类裁定]
```

---

### ISS-0009

```markdown
ID: ISS-0009
Status: needs_review
Severity: trivial
Created: 2026-10-10
Related Stage: NORMALIZE | ANALYZE

Problem:
`汇总_粘连噪声.csv` 为空（0 条），但独立探针显示仍有 3 个**非词表 token 未被拦截**，已作为「内容词」
计入词频表：
- `zujuan`（4 次，4 卷）—— 组卷网水印拼音，属来源噪声；
- `citylibrary`（4 次，1 卷）—— 疑为网址/用户名残片；
- `katebrown`（2 次，1 卷）—— 专名粘连（Kate Brown），应拆或按专名处理。

Evidence:
- `03_data/汇总_词形版.csv` 第 2125 / 2282 / 3401 行。
- `wordfreq_multi.py` 的拦截条件：`sp and len(sp)==2 and (any(p in STOPWORDS for p in sp) or len(low) >= 12)`
  —— 三者都不满足（无虚词、长度 < 12），故未被判为粘连噪声。

补充（2026-10-10 复核）：**「粘连噪声 = 0」本身是可信的**。本语料来自原生文本 docx（非扫描件、未 OCR），
空格完好；独立探针（长度 ≥ 12 且非词表项、小写且可拆成两个常用词）未发现 `tothink` /
`homeworkdifficult` 式粘连。绝大多数"可疑长串"实为**带连字符的合法复合词**
（`self-compassion` / `well-trained` / `family-friendly` …），且 `website` / `smartphones` /
`deskmate` / `motorsport` / `rainforests` / `laptops` / `birdwatching` 等**真词均未被误删**
（`words_alpha.txt` 收词不全，流水线已按规范用 `common_10000` 约束，未踩坑）。

Suggested Action:
由教研裁定是否收紧拦截规则（三选一）：
(a) 把 `zujuan` / `citylibrary` 加入 `JUNK` 白名单（最小改动，仅清来源噪声）；
(b) 放宽 `len(low) >= 12` 为「含虚词 **或** 长度 ≥ 8」（能拦 `katebrown`，但可能误伤复合词，需回归测试）；
(c) 维持现状 —— 三者合计仅 10 次，对 72,641 词次的总量影响 < 0.02%，可在报告中标注为已知噪声。
**注意：任何改动都会改变 `03_data/` 产出，属口径变更，须先裁定再动手。**

Human Decision:
[待人类裁定]
```

---

> 说明：本项目当前无 `blocker` 级缺陷。**ISS-0003 已于 2026-10-09 裁定并关闭**（采用省级统一命题口径）。
> **ISS-0009 于 2026-10-10 新增**（噪声拦截偏保守，3 个漏网 token）。
> 剩余 ISS-0002 / ISS-0004 / ISS-0005 / ISS-0009 共 4 项待人类裁定，之后方可进入 ENRICH（补 `pos / 音标 / 释义 / score`）阶段。
