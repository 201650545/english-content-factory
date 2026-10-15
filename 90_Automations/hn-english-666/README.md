# hn-english-666 · 分析脚本

> 服务于 `02_Projects/HN-ENGLISH-666`（湖南中考英语真题高频666词）的语料处理与词频统计脚本。
> 首次落地：`RUN-ANL-0001`（2026-10-08）。

---

## 脚本清单

| 脚本 | 作用 | 主要输入 | 主要输出 |
| :--- | :--- | :--- | :--- |
| `ocr_papers.py` | 调用本机 PaddleOCR 批量识别试卷页面图 | `pages/*.png` | `ocr/*.txt` + `_summary.json` |
| `word_freq.py` | 双轨词频统计（原生文本版 / OCR 版） | `papers_text/*.txt`、`ocr/*.txt` | `out/词频_*.csv`、`out/word_freq.json` |
| `filter_words.py` | **核心**：抽词 → 词表校验 → 去噪 → 词形归并 → 降序统计 | `papers_text/*.txt`、`resources/*.txt` | `out/筛选_*.csv`、`out/filter_words.json` |
| `make_report.py` | 由 `word_freq.json` 生成词频 HTML 报告 | `out/word_freq.json` | `out/词汇频率统计表.html` |
| `make_filter_report.py` | 由 `filter_words.json` 生成筛选 HTML + Excel | `out/filter_words.json` | `out/词汇筛选统计表.html`、`.xlsx` |

## 运行环境

- Python 3.12（系统解释器）
- 依赖：`openpyxl`（报告）；`paddleocr` + `paddlepaddle`（仅 `ocr_papers.py` 需要）
- OCR 前置环境变量：`PROTOCOL_BUFFERS_PYTHON_IMPLEMENTATION=python`
  （详见 `D:\临时文件夹\OCR-使用说明-给Agent-20261001.md`）

## 预期目录布局（重要）

脚本以「**脚本所在目录的上一级**」为 `ROOT`，因此运行时工作区应满足：

```
<工作区>/
├── scripts/            # 本目录（5 个 .py）
├── resources/
│   ├── words_alpha.txt     # 37 万通用词表（校验用）
│   └── common_10000.txt    # 1 万常用词表（归并/拆分约束用）
├── papers_text/        # 8 套纯净真题文本 *.txt
├── ocr/                # OCR 结果（可选）
├── pages/              # 试卷页面 PNG（仅 OCR 需要）
└── out/                # 全部产物落此
```

> 即：把本目录整体放到工作区内的 `scripts/`，`resources/` 与其平级即可直接跑。
> 若目录结构不同，需相应调整各脚本顶部的 `ROOT` 计算。

## 推荐运行顺序

```bash
python scripts/filter_words.py          # 先算（产出 filter_words.json）
python scripts/make_filter_report.py    # 再出报告（依赖上一步 json）
```

## 与项目目录的映射

| 工作区目录 | 对应项目目录 |
| :--- | :--- |
| `papers_text/` | `02_Projects/HN-ENGLISH-666/02_clean/试卷文本/` |
| `out/*.json`、`词频数据集_*.csv` | `.../03_data/` |
| `out/*.html`、`out/*.xlsx`、`out/词频_*.csv` | `.../04_analysis/` |
| `resources/` | 本目录 `resources/` |

## 词表资源来源

- `words_alpha.txt` — dwyl/english-words（370,105 词）
- `common_10000.txt` — first20hours/google-10000-english（10,000 词）

> ⚠️ `words_alpha.txt` 收词不全（连 `website`、`smartphone` 都不含），因此**"是否真实单词"不能只看它**；
> 拆分/归并候选一律以 `common_10000.txt` 为准（见 `filter_words.py` 的 `valid_part()`）。
