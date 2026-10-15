# AGENTS.md · English Content Factory（机器先读我）

> 本文件是**纯指针**，不含规则正文。正文在它指向的地方，改规则去那里改，**勿在此复制**。
> 跨项目默认规矩继承 handbook：`D:\记忆\通用规范\README.md`（引用须写明版本号）。
>
> **建此文件的原因（2026-10-09）**：`D:\Work\AGENTS.md` 第 4 条要求"读该项目的 `AGENTS.md`（没有则读 `README.md`）"，
> 而本项目根目录原先**两个都没有** ⇒ 外部 Agent 按标准读序接入时**第一跳就断**。本文件即补此缺口。

---

## 一、接入顺序（按序读，读完才准动手）

| 序 | 读什么 | 路径 | 回答 |
| :--- | :--- | :--- | :--- |
| 1 | 项目总览（人机入口） | `00_System/README.md` | 这个系统是什么、边界在哪 |
| 2 | **协同协议与铁律** | `00_System/AGENTS.md` | 我该怎么做事（含 Bootstrapping Order） |
| 3 | 全景架构与子项目登记 | `00_System/PROJECT_MAP.md` | 有哪些子项目、谁依赖谁 |
| 4 | 数据契约 | `00_System/DATA_SCHEMA.md` | 字段字典与清洗契约 |
| 5 | 当前状态 | `00_System/CURRENT_STATE.md` | 上一任做到哪、下一步是什么 |
| 6 | **工位约束** | `00_System/工位说明/` | 哪个模型负责哪一段、我能不能写 |

---

## 二、子项目（现行，一律在 `02_Projects/<project_id>/`）

| project_id | 名称 | 状态 | 生产规范正文 |
| :--- | :--- | :--- | :--- |
| `HN-ENGLISH-666` | 湖南中考英语 666 词 | `content_ready` | `02_Projects/HN-ENGLISH-666/METHODOLOGY.md` |
| `CN-ENGLISH-MULTI` | 多地中考英语词汇（多地区） | `analyzing` | —（见 `PROJECT.md`） |
| `HN-ZK-ZHUANXIANG` | 湖南中考英语 · 题型专项 | `rendered` | `02_Projects/HN-ZK-ZHUANXIANG/PROJECT_SPEC.md` |

**进入任一子项目时按此顺序读**：

```
PROJECT.md（元数据/交付清单）
  → PROJECT_SPEC.md 或 METHODOLOGY.md（生产规范正文 ← 干活看这个）
  → SOURCES.md（来源登记）
  → ISSUES.md（未决项）
  → 09_runs/（每轮流水）
```

> 五类文档**互不复制**：元数据 / 规范 / 来源 / 未决项 / 流水 各写各的。

---

## 三、外部路由（跨库）

| 用途 | 位置 |
| :--- | :--- |
| 本库路由真源（有哪些项目、去哪找） | `D:\记忆\项目清单\projects.yaml` |
| 跨会话记忆 | `D:\记忆\`（访问须执行其 README 的三步守则：git 查新 / 补漏 / 挂链） |
| 跨项目规范 | `D:\记忆\通用规范\`（handbook） |

⚠️ **截至 2026-10-09，本项目尚未登记**进 `projects.yaml` 与 `Work/Home.md` 子库一览（登记动作归库管理员，不由本项目自行写入）。

---

## 四、边界与红线

1. **本项目是多模型工位协同**（Kimi / DeepSeek / Qwen / QwenMax / GLM / GPT6extend / SpaceBunny / Step5Preview，共 8 份工位说明）。
   **动手改任何文件前，先确认没有别的工位正在写。**
2. **已退役、不要再写的旧路径**：`D:\Work\英语内容工厂\`（2026-10-09 已删除）。
3. **旧中文目录结构已不适用**：`01_项目_湖南中考英语666词/…` 是迁移前快照，现行一律 `02_Projects/<project_id>/`。
4. 本文件只放指针；**规则正文不在此**。
