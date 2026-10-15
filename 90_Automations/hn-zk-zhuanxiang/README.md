# hn-zk-zhuanxiang/ — 湖南中考英语 · 题型专项 生成脚本链

> **归属**：本项目专属工具（handbook 12-号《工具归属》第 1 问：只服务一个项目 → 归该项目脚本目录）。
> 在 ECF 里，项目脚本目录 = **`90_Automations/<project_id>/`**（本目录）。
> 数据一律落 `02_Projects/HN-ZK-ZHUANXIANG/`。
>
> **历史**：本目录 2026-10-09 15:59 曾放过一版 `build_zt.py`（29 KB，早于"答案解析"工作，已陈旧）；
> 2026-10-09 晚间的最新版（52.8 KB，含逐题解析）一度寄居 `D:\临时文件夹\_hn_zt\`。
> **2026-10-10 归位**：以最新版覆盖陈旧副本，并把三个脚本改为**直连项目目录**（取消临时目录与人工同步）。

## 依赖

仅 **Python 3 标准库**（`os` / `re` / `json` / `html`），无第三方包，无网络请求。

## 脚本清单

| 脚本 | 作用 | 读 | 写 |
| :--- | :--- | :--- | :--- |
| `parse_analysis.py` | 2021–2023 长沙卷「解析版」txt → 结构化 JSON | `02_clean/试卷文本/<paper>_解析版.txt` | `03_data/analysis_zh.json` |
| `add_ext2.py` | 合并 2024 长沙 / 2026 湖南 / 2026 长沙 的逐题解析 | （内置文本） | `03_data/analysis_ext.json` |
| `build_zt.py` | **主生成器**：渲染 12 册可打印 HTML | `02_clean/试卷文本/*.txt` + `03_data/analysis_*.json` | `03_Products/*.html` |
| `scaffold.py` | 一次性脚手架（建目录 + 归置语料） | 原始暂存区 `_hn_zk_2021` | `01_raw/`、`02_clean/`、`03_data/manifest.json` |
| `结构探查_probe.txt` | 分块坐标探查留痕 | — | — |

> `scaffold.py` 已执行完毕，其输入是**临时暂存目录**，不具备长期可复现性；过程登记于
> `02_Projects/HN-ZK-ZHUANXIANG/09_runs/RUN-BLD-0001.md`，保留仅作取证。

## 执行顺序

三步链，可整体重跑，也可只跑第 3 步：

```bash
python parse_analysis.py     # 1. 2021–2023 长沙卷官方解析 → analysis_zh.json
python add_ext2.py           # 2. 追加 2024/2026 解析 → analysis_ext.json
python build_zt.py           # 3. 渲染 12 册 HTML
```

**日常只跑第 3 步**即可出册。三脚本路径均以 `__file__` 自定位，可在任意 cwd 下运行：

```python
HERE = os.path.dirname(os.path.abspath(__file__))   # ...\90_Automations\hn-zk-zhuanxiang
ECF  = os.path.dirname(os.path.dirname(HERE))       # ...\English-Content-Factory
PROJ = os.path.join(ECF, "02_Projects", "HN-ZK-ZHUANXIANG")
SRC  = PROJ/02_clean/试卷文本      # 真题文本输入
DATA = PROJ/03_data                # analysis_zh.json / analysis_ext.json
OUT  = PROJ/03_Products            # HTML 输出
```

## 回执

`build_zt.py` 每次运行打印**每册的篇目数与含答案数**，末行给出输出目录与总览页路径：

```
01_阅读理解A专项.html              篇目=9  含答案=9
...
OUT = ...\03_Products
INDEX: ...\03_Products\00_总览.html
```

## 验证命令（可复现性自证）

输出**确定性**（无时间戳、无随机源），故"重跑后交付物是否逐字节一致"即链路正确性判据：

```bash
cd 02_Projects/HN-ZK-ZHUANXIANG/03_Products && sha256sum *.html | sort > /tmp/before.txt
python ../../90_Automations/hn-zk-zhuanxiang/build_zt.py
cd 02_Projects/HN-ZK-ZHUANXIANG/03_Products && sha256sum *.html | sort > /tmp/after.txt
diff /tmp/before.txt /tmp/after.txt   # 空 = 通过
```

**2026-10-10 归位验收实测**（cwd=`/d`）：三步链重跑，`03_Products/*.html` **12/12 逐字节一致**，
`analysis_zh.json` / `analysis_ext.json` 亦逐字节一致。

## 数据契约

* `analysis_zh.json` — `{paper: {section_key: {"intro": 总述, "q": {"<题号>": 详解}}}}`
  仅含 2021—2023 长沙卷（组卷网官方解析，本地「解析版」txt 解析所得）。
* `analysis_ext.json` — 同结构，含 2024 长沙卷 / 2026 湖南省卷 / 2026 长沙卷（教习网、21cnjy）。
* `build_zt.py` 载入时以 `_` 开头的键视为元数据跳过；两份 JSON 以 `setdefault(...).update(...)` 合并进 `ANA`。
* 生产规范正文见 `02_Projects/HN-ZK-ZHUANXIANG/PROJECT_SPEC.md`（§3 `build_zt.py` 架构、§7 工具归属）。

## 变更记录

* **2026-10-10** 归位本目录；以最新版（52.8 KB）覆盖 2026-10-09 15:59 的陈旧副本；
  三脚本改为直连 `02_Projects/HN-ZK-ZHUANXIANG/`；补本 README。
  验收：三步链重跑，HTML 12/12 与两份 JSON 均逐字节一致。
