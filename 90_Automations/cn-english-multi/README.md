# cn-english-multi · 分析脚本

> 服务于 `02_Projects/CN-ENGLISH-MULTI`（多地中考英语真题词汇频率库 2021–2026）的抓取、抽取与词频统计脚本。
> 方法基座继承自 `90_Automations/hn-english-666/`；首次落地：`RUN-ANL-0002`（2026-10-09）。

---

## 脚本清单

| 脚本 | 作用 | 主要输入 | 主要输出 |
| :--- | :--- | :--- | :--- |
| `fetch_papers.py` | 从 shijuan1.com 抓取 29 套真题 rar 附件（含 5 次重试 + 退避） | `TARGETS` 内置清单 | `01_raw/原始下载件/压缩包/<地区>_<年份>_<ID>.rar` |
| `doc2docx.ps1` | Word COM 批量 `.doc` → `.docx`（`SaveAs2(..., 16)`） | `01_raw/原始下载件/docx原文/**/*.doc` | 同目录 `.docx` |
| `extract_text.py` | docx → 纯文本（段落 + 表格） | `01_raw/原始下载件/docx原文/**/*.docx` | `02_clean/试卷文本/<地区>_<年份>_<标签>.txt` |
| `wordfreq_multi.py` | **核心**：抽词 → 词表校验 → 去噪 → 词形归并 → 多地区降序统计 | `02_clean/试卷文本/*.txt`、本目录 `resources/*.txt` | `03_data[/省统考口径]/汇总_*.csv`、`分地区_*.csv`、`filter_multi.json` |
| `make_multi_report.py` | 由 `filter_multi.json` 生成交互 HTML + 13-sheet Excel | `03_data[/省统考口径]/filter_multi.json` | `04_analysis[/省统考口径]/多地中考英语词汇统计表.{html,xlsx}` |

> 路径前缀均为 `02_Projects/CN-ENGLISH-MULTI/`。

## 运行环境

- Python 3.12（系统解释器 `C:/Users/郭永涛/AppData/Local/Programs/Python/Python312/python.exe`）
- 依赖：`openpyxl`（报告）、`python-docx`（抽取）、`requests`/`urllib`（抓取）
- `doc2docx.ps1` 需本机安装 Microsoft Word（COM 自动化）
- 解包 rar 用 `C:/Windows/System32/tar.exe -xf`（bsdtar，可正确处理中文文件名）

## 目录布局（2026-10-10 起：直连项目，不再用独立工作区）

本目录 = **脚本 + 词表**的家；**数据一律落项目目录**，脚本以 `__file__` 自定位，可在任意 cwd 运行：

```
English-Content-Factory/
├── 90_Automations/cn-english-multi/     ← 本目录（脚本 + resources）
│   ├── wordfreq_multi.py / make_multi_report.py / extract_text.py / fetch_papers.py
│   ├── doc2docx.ps1
│   └── resources/{words_alpha.txt, common_10000.txt}
└── 02_Projects/CN-ENGLISH-MULTI/        ← 数据全在这
    ├── 01_raw/原始下载件/{docx原文, 压缩包, 来源网页/{文章页,列表页}}
    ├── 02_clean/试卷文本/*.txt            （语料输入）
    ├── 03_data[/省统考口径]/*.csv + filter_multi.json
    └── 04_analysis[/省统考口径]/*.{html,xlsx}
```

脚本内路径常量（四脚本一致）：

```python
HERE = os.path.dirname(os.path.abspath(__file__))   # ...\90_Automations\cn-english-multi
ECF  = os.path.dirname(os.path.dirname(HERE))       # ...\English-Content-Factory
PROJ = os.path.join(ECF, "02_Projects", "CN-ENGLISH-MULTI")
```

> **历史**：此前脚本在一套独立工作区（`<工作区>/{scripts,resources,papers_text,raw,out}`）里跑，
> 靠本 README 的映射表人工同步回项目。2026-10-10 改为**直连项目目录**，取消人工同步环节
> —— 那正是"临时目录里的副本与项目内副本漂移"的根源。

## 推荐运行顺序

在本目录下运行即可（路径自定位，cwd 无所谓）：

```bash
python fetch_papers.py                       # 1. 抓取 rar（联网，一次性）
powershell -File doc2docx.ps1                # 2. doc → docx（需本机 Word）
python extract_text.py                       # 3. docx → txt
python wordfreq_multi.py                     # 4. 统计（产出 filter_multi.json）
python make_multi_report.py                  # 5. 出报告（依赖上一步 json）
```

**日常只跑 4、5**（1–3 是取证步骤，已执行完毕）。

## 口径开关（ZK_SCOPE）

`wordfreq_multi.py` 与 `make_multi_report.py` 共用环境变量 `ZK_SCOPE` 控制统计口径，**输出目录随之切换**：

| ZK_SCOPE | 口径 | 卷数 / 地区 | 输出目录 |
| :--- | :--- | :--- | :--- |
| `all`（默认/不设） | 全量已获取试卷 | 29 套 / 6 地区 | `out/` |
| `prov` | **仅省级统一命题**（含直辖市全市统一卷） | 19 套 / 5 地区 | `out_prov/` |

`prov` 口径白名单（写在 `wordfreq_multi.py` 顶部 `PROV_ALLOW`）：

- 北京 2021–2026（直辖市全市统一命题）
- 上海 2021–2022（直辖市全市统一命题）
- 重庆 2021–2026 A 卷（直辖市全市统一命题）
- 广东 2024–2026（2024 起省统一命题）
- 杭州 2024–2025（实为**浙江省统一卷**，`REGION_REMAP` 归名到「浙江」）

被排除：武汉全部（市自主命题）、杭州 2021–2023（市自主）、广东 2022–2023（地市自主）。

```bash
ZK_SCOPE=prov python scripts/wordfreq_multi.py
ZK_SCOPE=prov python scripts/make_multi_report.py
```

> 两种口径共用同一套分词/归并/去噪算法，仅输入卷集合与地区归名不同，因此结果**可直接对比**。

## 验证命令（可复现性自证）

流水线输出**确定性**（无时间源；xlsx 的 zip 条目时间戳与 `core.xml` 的 `dcterms:modified`
已由 `make_multi_report.py` 的 `_normalize_xlsx()` 抹平），故「连跑两遍是否逐字节一致」即链路正确性判据：

```bash
# 跑两遍 ④⑤（两口径各一遍），比对项目产出
for s in all prov; do ZK_SCOPE=$s python wordfreq_multi.py && ZK_SCOPE=$s python make_multi_report.py; done
```

**2026-10-10 归位验收实测**（cwd=`/d`）：两套口径各跑两遍，`03_data` + `04_analysis` 共 **29 个产物逐字节一致**
（CSV + JSON + HTML + XLSX 全部）。与归位前 git HEAD 逐文件核对：26 个逐字节相同，其余仅
①`filter_multi.json` 新增 `scope` 元数据字段、②两个 XLSX 的 `core.xml` 时间戳 —— **数据零变化**。

## 变更记录

* **2026-10-09** 首版落地（`RUN-ANL-0002`）：脚本置于独立工作区，靠本 README 的映射表人工同步回项目。
* **2026-10-10** **归位**：脚本改为直连 `02_Projects/CN-ENGLISH-MULTI/`（取消工作区与人工同步）；
  报告文件名随口径切换（`多地中考英语词汇统计表` / `省级统考_中考英语词汇统计表`）；
  新增 `_normalize_xlsx()` 使 xlsx 可逐字节复现。归位动因：独立工作区（临时目录）与项目内副本已发生漂移。

## 词表资源来源

- `words_alpha.txt` — dwyl/english-words（370,105 词）
- `common_10000.txt` — first20hours/google-10000-english（10,000 词）

> ⚠️ `words_alpha.txt` 收词不全（连 `website`、`smartphone` 都不含），因此**「是否真实单词」不能只看它**；
> 拆分/归并候选一律以 `common_10000.txt` 为准（见 `wordfreq_multi.py` 的 `valid_part()`）。

## 与 HN 版本的差异

| 项 | HN 版 | 本版（multi） |
| :--- | :--- | :--- |
| 地区维度 | 单地区（湖南/长沙） | 6 地区并表，输出含各地区分列计数 |
| 文件名解析 | 固定映射 | `parse_name()` 解析 `<地区>_<年份>_<标签>.txt` |
| 上海拼写提示题 | 无 | 新增 `PAREN_HINT` 正则还原 `(b)uy` → `buy` |
| 类别 | 功能词 / 内容词 | 功能词 / **短词·缩写（长度≤2）** / 内容词 |
| 额外产出 | — | `分地区_*.csv`、`分卷_词频.csv`（长表） |
