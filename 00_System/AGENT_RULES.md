# Agent Rules (Agent 行为准则与运行规范)

> 适用范围：所有进入 Education Content Factory 参与生产、治理或分析的自主与半自主 Agent。

---

## 一、启动协议 (Bootstrap Protocol)

任何 Agent 在接受任务并开始实质性工作之前，**必须且只能按以下顺序完成前置环境与上下文核验**：

1. **读取系统规则**：[`00_System/AGENT_RULES.md`](file:///D:/Work/English-Content-Factory/00_System/AGENT_RULES.md)（掌握不可逾越的行为红线与日志契约）
2. **读取全局工作流**：[`00_System/WORKFLOW.md`](file:///D:/Work/English-Content-Factory/00_System/WORKFLOW.md)（理解当前任务处于流水线的哪个阶段及 DoD）
3. **读取目标项目状态**：当前操作项目根目录下的 `PROJECT.md`（获取项目元数据、当前 stage 与状态机快照）
4. **读取未决与阻塞问题**：当前操作项目根目录下的 `ISSUES.md`（确认是否存在直接前置依赖的未决缺陷或 blocker）

> [!IMPORTANT]
> 若上述前置文件缺失、损坏或存在冲突，Agent 必须立刻停止执行，向人类报告，不得擅自假定默认值进行跳过。

---

## 二、原始数据保护 (Raw Immutability)

* **不可变资产原则**：各项目 `01_raw/` 目录中的所有文件一律默认为 **严格不可变 (Immutable)**。
* **写保护红线**：严禁任何 Agent 修改、覆盖、移动、重命名或删除 `01_raw/` 中的任何原始文件。
* **衍生数据隔离**：所有清洗、抽取、格式转换过程产生的中间产物，必须显式写入下游目录（如 `02_clean/`、`03_data/` 等），严禁在原始目录就地修改。

---

## 三、不确定性处理与防幻觉纪律 (Uncertainty Handling)

在处理语料、算法、教研规则或字段对齐时，若遇到不确定、语义模糊或缺少上下文的信息：

1. **禁止自行猜测**：严禁基于“常理推测”自行决定教育学分类或技术映射规则。
2. **禁止静默修复**：严禁私自“顺手修正”看似错误但来源未知的历史标记。
3. **即时落盘 Issue**：必须在当前项目的 `ISSUES.md` 中按标准模板追加一条 Issue。
4. **统一打标**：将该项状态显式标记为 `needs_review`，并在 Run Log 中如实记录。
5. **安全降级**：若该不确定性影响后续阶段推进，应将当前项目状态或阶段置为 `blocked` 并安全退出。

---

## 四、日志纪律 (Run Log Protocol)

任何对项目仓库产生了实质性文件变更（包括文件的新建、内容的修改、配置变更等）的 Agent，在当次运行结束前，**必须在对应项目的 `09_runs/` 目录留下 Run Log**。

### 1. 命名规范
文件命名格式：`RUN-<阶段缩写>-<序号>.md` 或 `INIT-<序号>.md`，例如：`INIT-0001.md`、`RUN-CLN-0001.md`。

### 2. 必须包含的字段契约
每份 Run Log 必须包含以下九个核心项：

* **run_id**：唯一运行标识符（例如 `RUN-20261008-0001`）
* **agent**：执行本次任务的 Agent 角色及模型类型（例如 `Builder Agent`）
* **input**：本次运行接收的显式输入（包括输入文件路径、参数、配置版本）
* **action**：执行的具体原子动作与转换逻辑
* **files_created**：本次运行创建的文件绝对/相对路径清单
* **files_modified**：本次运行修改的文件清单（若无则填 `None`）
* **issues**：运行中捕获并记录到 `ISSUES.md` 的问题 ID 或说明（若无则填 `None`）
* **result**：本次运行的最终状态（`success` / `partial_success` / `failed`）
* **next_action**：推荐由人类审核或后续 Agent 执行的下一步动作

---

## 五、数据与表达形式约定 (Data Separation)

1. **Markdown 职责**：负责系统规范、流程指引、项目状态、方法论演化以及人类可读报告。
2. **结构化数据职责**：未来涉及词表（如 666 词主表）、分词明细、真题语料数据库等大规模结构化信息，一律存储于 CSV / JSON / SQLite，禁止生成成百上千个微型 Markdown 文件充当数据行。
