# CURRENT_STATE.md - 项目当前推进状态

> **更新时间**：2026-10-11（听力线 + 脚本归位状态刷新，交接见 `02_Projects/HN-ZK-ZHUANXIANG/09_runs/RUN-HO-0001.md`）
> **口径**：本文件只记**跨子项目的当前状态与入口**。**每个子项目的详细状态以它自己的 `PROJECT.md` / 看板 / `09_runs/` 为准**，此处不复制。

---

## 0. 子项目状态一览（现行 `02_Projects/`）

| project_id | 名称 | 状态 | 状态详情去哪看 | 生产规范正文 |
| :--- | :--- | :--- | :--- | :--- |
| `HN-ENGLISH-666` | 湖南中考英语 666 词 | `content_ready` | `02_Projects/HN-ENGLISH-666/项目主控看板.md` · `PROJECT.md` · `09_runs/` | `METHODOLOGY.md` |
| `CN-ENGLISH-MULTI` | 多地中考英语词汇（多地区） | `analyzing` | `02_Projects/CN-ENGLISH-MULTI/PROJECT.md` · `09_runs/` | —（见 `PROJECT.md`） |
| `HN-ZK-ZHUANXIANG` | 湖南中考英语 · 题型专项 | `rendered` | `02_Projects/HN-ZK-ZHUANXIANG/PROJECT.md` · `本次交付说明.md` · `09_runs/` | `PROJECT_SPEC.md` |

---

## 1. 项目级基础设施（已就绪）

- [x] **目录英文化**（2026-10-09 起生效）：现行结构 = `00_System/` · `01_Inbox/` · `02_Projects/` · `90_Automations/` · `99_Archive/`。
  ⚠️ **旧中文目录 `D:\Work\英语内容工厂\` 已退役删除**，不要再引用（原 `01_项目_湖南中考英语666词/…` 结构同样作废）。
- [x] **入口文件**：根 `AGENTS.md`（纯指针）+ `00_System/{README, AGENTS, PROJECT_MAP, DATA_SCHEMA, CURRENT_STATE}.md`。
- [x] **工位说明**：`00_System/工位说明/` 共 8 份（Kimi / DeepSeek / Qwen / QwenMax / GLM / GPT6extend / SpaceBunny / Step5Preview）。

---

## 2. HN-ENGLISH-666（666 词）

> 详细状态以 `02_Projects/HN-ENGLISH-666/项目主控看板.md` 为准。以下资产均于 **2026-10-09 实测存在**。

- [x] **8 套真题文本库**（2022–2026 湖南省卷与长沙卷）
- [x] **词频数据**：`03_data/` 下 `word_freq.json` · `词频数据集_{内容词,归并版,词形版}.csv` · `filter_words.json`
- [x] **Top 100 精准词表**：`03_data/方案B_Top100精准词表.json` · `03_data/top100_enriched.json`
- [x] **全量 666 词母表**：`03_data/全量666词_母表数据集.{json,csv}`
- [x] **小红书封面**：`03_Products/03_小红书封面/Top100_Vocabulary_Study_Desk_1086x1448.png`
- [x] **迁移映射**：`迁移映射表.csv`

### 2026-10-15 新增（688 词表格速查版 · 发布线）

- [x] **成册词表**：`03_data/688词_成册词表.csv`（688 行，与 HTML 逐字对齐；母表未动）
- [x] **成品 PDF**：`03_Products/湖南中考英语5年8套真题_高频688词_表格速查版.pdf`（28 页 A4，5.15MB，已验收：HTML==PDF 逐字一致 · 0 溢出 · 最小字号 10.5pt）
- [x] **封面 v4**：`03_Products/03_小红书封面/688_cover_v4.png`（1024×1536，用户确认"就这一版"，禁止重生成）
- [x] **轮播 4 张**：同目录 `xhs_p2~p5_*.png`（1405×1988）
- [x] **发布保底包**：`03_Products/04_发布保底包/`（`01_`~`05_` 编号图 + 标题正文纯文本 + 人工发布步骤）
- [x] **手机通道打通**（桌面硬墙绕行）：`adb forward 9222` + `adb reverse 19100/19825` 接管手机 Kiwi；发布页已填好，**停在发布按钮前等人工审核**
- [x] **可复用规范**：`00_System/手机通道发布小红书_可复用规范.md`（MOBILE-PUBLISH-CHANNEL v1）

**待推进**（引用前回源核对）：**小红书发布（待用户审核后点发布）** · 发布后置顶评论与 688 回复 · GitHub 上传与 RCLP 握手 · 约 10 条 OCR 残留词待用户拍板（`ai/com/costa/john` 等，为保 688 数保留）。

> ⚠️ 历史版本本文件曾写「`湖南中考英语_前100核心词_音标双义项.json`」与「`04_设计工坊与资产/` 下的封面」——**2026-10-09 实测二者均已不在原处**（前者文件名已变，后者目录为空），已按实测更正。

---

## 3. HN-ZK-ZHUANXIANG（题型专项）

> 详细状态见 `PROJECT.md` · `本次交付说明.md`；生产规范见 `PROJECT_SPEC.md`；**交接文档见 `09_runs/RUN-HO-0001.md`（2026-10-11）**。

- [x] **成品已 `rendered`**：`03_Products/` 12 个可打印 HTML（`00_总览` + 11 个题型专项），合计 **437.6 KB**（2026-10-09 实测）
- [x] 覆盖 **2021—2026 共 9 套卷**（暂不含听力成品）；省卷与长沙卷合并成册、逐篇标注来源
- [x] **参考答案 + 逐题解析**已并入：解析块 81 · 逐题 393 · 选项行 198 · 红标 1102 · NUL 字节 0（全部当轮实测）
- [x] 生产规范已固化：`PROJECT_SPEC.md`（题源取证纪律 / `build_zt.py` 架构 / 印刷 HTML 规范 / 质检口径 / 基线数字）
- [x] **生成链已归位 `90_Automations/hn-zk-zhuanxiang/`**（2026-10-10，commit `789b26d`；临时目录 `_hn_zt` 已退役）
- [x] **听力音频已落地（2026-10-10）**：`01_raw/听力音频/` 12 条全长 MP3（113.9 MB，B站免费源 opencli 抓取，防试看校验全过）；回执 `_manifest.json`，来源登记 `SOURCES.md §七`

**待推进**（详见 `ISSUES.md` 与 `09_runs/RUN-HO-0001.md`）：**听力原文（ISS-0014 open 项）** · **听力专项练习册（未开工）** · 图片素材补录 · OCR 校对 · 版权授权确认 · PDF 导出。

---

## 4. CN-ENGLISH-MULTI（多地词汇）

> 详细状态见 `02_Projects/CN-ENGLISH-MULTI/PROJECT.md` · `本次交付说明.md` · `本次交付说明_省统考口径.md`。

- 状态 `analyzing`。已核实存在的产出：`04_analysis/多地中考英语词汇统计表.{html,xlsx}` + `04_analysis/省统考口径/`；
  `03_data/` 下含分地区归并版 CSV（北京 / 上海 / 广东 / 杭州 / 武汉 / 重庆 等）与 `filter_multi.json` · `分卷_词频.csv`。
