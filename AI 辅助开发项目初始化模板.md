# AI 辅助开发项目初始化模板

> 适用场景：使用 Codex、Cursor、Claude Code、ChatGPT 等工具进行 Vibe Coding / AI Assisted Development。  
> 核心目标：让项目从一开始就具备可迭代、可维护、可测试、可交接的基础结构。  
> 基本原则：先写规格，再写代码；先跑通 MVP，再逐步复杂化；AI 负责快速实现，人负责边界、架构、验收和关键判断。

---

# 1. 根据项目规模选择文档包

## 1.1 小型项目：1 天到 1 周，脚本 / Demo / 课程实验

适合场景：

- 单人开发
- 功能少
- 数据结构简单
- 主要目标是快速跑通
- 允许后期重构

推荐文件：

```text
project_root/
├── README.md
├── AGENTS.md
├── docs/
│   ├── PROJECT_CHARTER.md
│   └── specs/
│       └── mvp/
│           ├── requirements.md
│           ├── tasks.md
│           └── acceptance_tests.md
├── src/
├── tests/
└── examples/
```

最低要求：

- `README.md`：项目怎么运行
- `AGENTS.md`：AI 修改代码必须遵守的规则
- `PROJECT_CHARTER.md`：项目目标、边界、MVP
- `requirements.md`：当前功能需求
- `tasks.md`：任务拆解
- `acceptance_tests.md`：验收标准

---

## 1.2 中型项目：1 周到 1 个月，科研工具 / 小系统 / 可扩展 Demo

适合场景：

- 多模块
- 数据结构较稳定
- 后续需要持续迭代
- 需要测试和文档同步
- 可能由 AI 多轮修改

推荐文件：

```text
project_root/
├── README.md
├── AGENTS.md
├── docs/
│   ├── PROJECT_CHARTER.md
│   ├── ARCHITECTURE.md
│   ├── DATA_CONTRACT.md
│   ├── CODING_RULES.md
│   ├── TESTING_STRATEGY.md
│   ├── ROADMAP.md
│   ├── decisions/
│   │   └── ADR-0001-initial-architecture.md
│   └── specs/
│       └── feature_x/
│           ├── requirements.md
│           ├── design.md
│           ├── tasks.md
│           └── acceptance_tests.md
├── src/
├── tests/
├── examples/
├── scripts/
└── data/
```

核心要求：

- 每个功能单独建一个 `docs/specs/feature_x/`
- 重要设计决策写进 `docs/decisions/`
- 数据输入输出格式写进 `DATA_CONTRACT.md`
- AI 每次开发前必须读 `AGENTS.md` 和相关 spec

---

## 1.3 大型项目：1 个月以上，复杂系统 / 多人协作 / 长期维护项目

适合场景：

- 多人协作
- 多模块、多接口、多数据流
- 后期需要重构、部署、扩展
- 代码质量和可维护性要求高

推荐文件：

```text
project_root/
├── README.md
├── AGENTS.md
├── CONTRIBUTING.md
├── docs/
│   ├── PROJECT_CHARTER.md
│   ├── PRODUCT_REQUIREMENTS.md
│   ├── ARCHITECTURE.md
│   ├── MODULE_BOUNDARIES.md
│   ├── DATA_CONTRACT.md
│   ├── API_CONTRACT.md
│   ├── CODING_RULES.md
│   ├── TESTING_STRATEGY.md
│   ├── DEPLOYMENT.md
│   ├── SECURITY_AND_PRIVACY.md
│   ├── ROADMAP.md
│   ├── CHANGELOG.md
│   ├── decisions/
│   │   ├── ADR-0001-initial-architecture.md
│   │   └── ADR-0002-data-format-choice.md
│   └── specs/
│       ├── feature_a/
│       │   ├── requirements.md
│       │   ├── design.md
│       │   ├── tasks.md
│       │   └── acceptance_tests.md
│       └── feature_b/
│           ├── requirements.md
│           ├── design.md
│           ├── tasks.md
│           └── acceptance_tests.md
├── src/
├── tests/
│   ├── unit/
│   ├── integration/
│   └── golden/
├── examples/
├── scripts/
├── data/
└── configs/
```

大型项目必须注意：

- 明确模块边界
- 明确 API / 数据契约
- 每个功能必须先有 spec
- 重要修改必须同步测试
- 重要决策必须写 ADR
- 不允许 AI 大范围无目标重构

---

# 2. 必备文件模板

---

# 2.1 AGENTS.md

## 文件位置

```text
project_root/AGENTS.md
```

## 文件定位

这是 AI 开发助手的长期规则文件。  
Codex、Cursor、Claude Code 等工具在修改代码前都应该优先阅读它。

它的作用类似于：

- 项目级系统提示词
- AI 开发规范
- 防止 AI 乱改代码的约束文件

## 模板内容

```markdown
# AGENTS.md

## 1. 项目开发总原则

1. 先理解需求，再修改代码。
2. 每次只完成一个明确任务。
3. 不允许无关重构。
4. 不允许为了修复局部问题而破坏已有接口。
5. 不允许写死 examples、tests 中的特定输入。
6. 新增功能必须补充测试。
7. 修改数据结构时，必须同步更新 docs/DATA_CONTRACT.md。
8. 修改架构时，必须同步更新 docs/ARCHITECTURE.md，并在 docs/decisions/ 中新增 ADR。
9. 修改用户可见行为时，必须同步更新 README.md 或 examples。
10. 如果需求不明确，先在 spec 中写明假设，再实现最小可运行版本。

## 2. AI 修改代码前必须阅读

修改任何代码前，请先阅读：

1. docs/PROJECT_CHARTER.md
2. docs/ARCHITECTURE.md
3. docs/DATA_CONTRACT.md
4. docs/TESTING_STRATEGY.md
5. 当前功能对应的 docs/specs/<feature_name>/ 文件夹

## 3. 代码修改要求

每次修改后，请输出：

1. 修改了哪些文件
2. 每个文件为什么修改
3. 是否新增或修改测试
4. 如何运行测试
5. 是否影响已有接口或数据结构
6. 是否需要同步更新文档

## 4. 禁止行为

禁止以下行为：

1. 未经说明大范围重构。
2. 删除已有测试。
3. 跳过测试直接宣称完成。
4. 使用临时 hard-code 通过测试。
5. 修改无关模块。
6. 引入重量级依赖而不说明原因。
7. 改变公开函数签名但不更新调用处和文档。

## 5. 推荐开发方式

优先采用以下流程：

需求确认 → 写 spec → 写测试 → 实现代码 → 运行测试 → 更新文档 → 总结变更。
```

## 对 AI 的提示词

```text
请先阅读 AGENTS.md，并严格遵守其中的开发规则。
本次任务只允许修改与当前 spec 相关的文件。
完成后请说明修改文件、修改原因、测试方式和是否影响已有接口。
```

---

# 2.2 PROJECT_CHARTER.md

## 文件位置

```text
project_root/docs/PROJECT_CHARTER.md
```

## 文件定位

这是项目宪法。  
它定义项目为什么做、做什么、不做什么、第一阶段做到什么程度。

## 模板内容

```markdown
# PROJECT_CHARTER.md

## 1. 项目名称

项目名称：__________

## 2. 项目背景

简要说明为什么要做这个项目。

例如：

当前存在的问题是：__________  
现有方法的不足是：__________  
本项目希望解决：__________

## 3. 项目目标

本项目的核心目标是：

1. __________
2. __________
3. __________

## 4. 目标用户

本项目面向的用户是：

1. __________
2. __________

用户的典型使用场景包括：

1. __________
2. __________

## 5. 输入

系统输入包括：

1. __________
2. __________
3. __________

输入示例：

​```text
这里放一个最小输入示例
## 6. 输出

系统输出包括：

1. __________

2. __________

3. __________

输出示例：

​```text
这里放一个最小输出示例
​```

## 7. 当前阶段范围

当前阶段只做：

1. __________

2. __________

3. __________

当前阶段不做：

1. __________

2. __________

3. __________

## 8. MVP 最小闭环

MVP 的最小闭环是：

​```text
输入 → 核心处理 → 输出 → 验证
​```

具体为：

​```text
__________ → __________ → __________ → __________
​```

## 9. 成功标准

项目第一阶段成功的标准是：

1. 能够运行
2. 有最小示例
3. 有测试
4. 输出结果可解释
5. 后续功能可以继续扩展

## 10. 风险与假设

当前假设：

1. __________

2. __________

当前风险：

1. __________

2. __________

​```
```

## 对 AI 的提示词

```text
请根据我的项目想法，帮我生成 docs/PROJECT_CHARTER.md。
要求：
1. 明确项目目标、输入、输出、MVP 和暂不实现的内容。
2. 不要扩大项目范围。
3. 优先保证第一阶段可运行。
4. 对不明确的地方，请写成“当前假设”，不要自行过度发挥。
```

------

# 2.3 ARCHITECTURE.md

## 文件位置

```text
project_root/docs/ARCHITECTURE.md
```

## 文件定位

说明系统整体架构、模块职责、数据流和依赖关系。  
中型及以上项目建议必须写。

## 模板内容

```markdown
# ARCHITECTURE.md

## 1. 架构目标

本项目架构需要满足：

1. 可迭代
2. 易测试
3. 易替换模块
4. 数据流清晰
5. 避免过早复杂化

## 2. 总体数据流

​```text
输入层 → 解析层 → 核心处理层 → 结果生成层 → 输出层
具体数据流：

​```text
__________ → __________ → __________ → __________
​```

## 3. 模块划分

| 模块         | 目录 / 文件 | 职责                | 是否核心 |
| ------------ | --------- | ------------------- | -------- |
| 输入模块     | src/...     | 负责读取输入        | 是       |
| 解析模块     | src/...     | 负责结构化解析      | 是       |
| 核心算法模块 | src/...     | 负责核心计算 / 推理 | 是       |
| 输出模块     | src/...     | 负责生成结果        | 是       |
| 工具模块     | src/...     | 通用工具函数        | 否       |
| 测试模块     | tests/...   | 验证功能正确性      | 是       |

## 4. 目录结构

​```text
project_root/
├── src/
├── tests/
├── docs/
├── examples/
├── data/
└── scripts/
​```

## 5. 核心接口

### 接口 1：__________

输入：

​```text
__________
​```

输出：

​```text
__________
​```

说明：

​```text
__________
​```

### 接口 2：__________

输入：

​```text
__________
​```

输出：

​```text
__________
​```

说明：

​```text
__________
​```

## 6. 不允许随意改变的内容

以下内容属于稳定接口，不能随意修改：

1. __________

2. __________

3. __________

如果必须修改，需要同步更新：

1. docs/DATA_CONTRACT.md
2. tests/
3. examples/
4. README.md

## 7. 后续可扩展方向

后续可以扩展：

1. __________

2. __________

3. __________

当前不实现：

1. __________

2. __________

​```
```

## 对 AI 的提示词

```text
请根据 PROJECT_CHARTER.md，为项目生成 docs/ARCHITECTURE.md。
要求：
1. 架构以 MVP 为优先，不要过度设计。
2. 明确每个模块的职责。
3. 明确数据流。
4. 明确哪些接口不能随便改。
5. 给出推荐目录结构。
```

---

# 2.4 DATA_CONTRACT.md

## 文件位置

```text
project_root/docs/DATA_CONTRACT.md
```

## 文件定位

说明输入、输出、中间数据结构。  
只要项目涉及文件、表格、JSON、数据库、API，就应该写这个文件。

## 模板内容

```markdown
# DATA_CONTRACT.md

## 1. 输入数据格式

### 输入文件 1：__________

位置：

​```text
data/...
格式：

​```text
json / jsonl / csv / md / txt / xlsx / ...
​```

字段说明：

| 字段名  | 类型   | 是否必需 | 含义   | 示例   |
| ------- | ------ | -------- | ------ | ------ |
| field_a | string | 是       | ______ | ______ |
| field_b | number | 否       | ______ | ______ |

示例：

​```json
{
  "field_a": "example",
  "field_b": 1
}
​```

## 2. 输出数据格式

输出字段：

| 字段名 | 类型   | 含义   | 示例   |
| ------ | ------ | ------ | ------ |
| result | object | ______ | ______ |
| status | string | ______ | ______ |

示例：

​```json
{
  "status": "success",
  "result": {}
}
​```

## 3. 中间数据结构

### 结构 1：__________

用途：

​```text
__________
​```

字段：

| 字段名 | 类型   | 含义   |
| ------ | ------ | ------ |
| ______ | ______ | ______ |

## 4. 数据约束

1. __________

2. __________

3. __________

## 5. 兼容性要求

1. 新增字段必须向后兼容。
2. 删除字段必须同步修改测试和文档。
3. 改变字段含义必须新增 ADR。
4. examples 中的数据不能作为 hard-code 逻辑写入代码。

​```
```

## 对 AI 的提示词

```text
请根据当前项目输入输出，生成 docs/DATA_CONTRACT.md。
要求：
1. 明确所有输入文件、输出文件和中间数据结构。
2. 每个字段都要说明类型、含义、是否必需和示例。
3. 不要擅自新增复杂数据结构。
4. 保持第一阶段 MVP 简洁。
```

------

# 2.5 TESTING_STRATEGY.md

## 文件位置

```text
project_root/docs/TESTING_STRATEGY.md
```

## 文件定位

说明项目如何测试。  
AI 生成代码后，必须通过测试锁住行为。

## 模板内容

```markdown
# TESTING_STRATEGY.md

## 1. 测试目标

测试的目标是：

1. 验证核心功能正确
2. 防止 AI 后续修改破坏已有逻辑
3. 保证输入输出格式稳定
4. 保证示例可以持续运行

## 2. 测试类型

| 测试类型 | 目录 | 用途 |
|---|---|---|
| 单元测试 | tests/unit/ | 测试单个函数 |
| 集成测试 | tests/integration/ | 测试多个模块串联 |
| Golden 测试 | tests/golden/ | 固定输入对应固定输出 |
| 示例测试 | examples/ | 验证用户使用路径 |

## 3. 必须覆盖的核心逻辑

1. __________
2. __________
3. __________

## 4. Golden Test 设计

固定输入：

​```text
examples/case_001/...
期望输出：

​```text
tests/golden/case_001_expected.json
​```

验证方式：

​```text
运行程序输出结果，并与 expected 文件对比。
​```

## 5. 测试运行方式

​```bash
pytest
​```

或：

​```bash
pytest tests/unit
pytest tests/integration
​```

## 6. AI 修改代码后的要求

AI 每次修改代码后必须：

1. 说明新增或修改了哪些测试
2. 说明如何运行测试
3. 不允许删除已有测试来通过检查
4. 不允许用 hard-code 方式通过测试

​```
```

## 对 AI 的提示词

```text
请根据当前项目结构生成 docs/TESTING_STRATEGY.md。
要求：
1. 明确单元测试、集成测试和 golden test 的用途。
2. 给出第一阶段最少需要覆盖的测试。
3. 不要设计过重的测试框架。
4. 优先保证 MVP 功能可以被稳定验证。
```

---

# 2.6 CODING_RULES.md

## 文件位置

```text
project_root/docs/CODING_RULES.md
```

## 文件定位

规定代码风格、命名方式、错误处理方式和依赖管理方式。

## 模板内容

```markdown
# CODING_RULES.md

## 1. 基本原则

1. 代码应简单清晰，优先可读性。
2. 不为未来功能过度设计。
3. 函数职责单一。
4. 公共接口应有类型标注。
5. 复杂逻辑必须有注释。
6. 错误信息应明确说明原因。

## 2. 命名规范

- 文件名：使用 snake_case
- 函数名：使用 snake_case
- 类名：使用 PascalCase
- 常量名：使用 UPPER_CASE
- 测试文件：test_xxx.py

## 3. 错误处理

错误处理要求：

1. 输入文件不存在时，返回明确错误。
2. 输入格式错误时，指出具体字段。
3. 查询不到结果时，不要静默失败。
4. 不允许裸 except。
5. 不允许吞掉异常后返回错误结果。

## 4. 依赖管理

1. 不随意引入大型依赖。
2. 新增依赖必须说明原因。
3. 如果标准库可以解决，优先使用标准库。
4. 依赖版本应写入 pyproject.toml 或 requirements.txt。

## 5. AI 生成代码要求

AI 生成代码时必须：

1. 保持函数短小。
2. 不写死测试输入。
3. 不修改无关文件。
4. 对复杂分支补充测试。
5. 保持已有接口兼容。
```

## 对 AI 的提示词

```text
请根据项目语言和当前代码结构生成 docs/CODING_RULES.md。
要求：
1. 规则简洁可执行。
2. 不要写空泛口号。
3. 重点约束 AI 容易犯的错误，例如 hard-code、无关重构、吞异常、破坏接口。
```

---

# 2.7 ROADMAP.md

## 文件位置

```text
project_root/docs/ROADMAP.md
```

## 文件定位

说明项目分阶段推进路线。  
防止 AI 一次性把项目做得过大。

## 模板内容

```markdown
# ROADMAP.md

## 阶段 0：项目初始化

目标：

1. 建立目录结构
2. 写基础文档
3. 准备最小示例数据

完成标准：

1. README.md 存在
2. AGENTS.md 存在
3. docs/PROJECT_CHARTER.md 存在
4. examples/ 中有最小输入

## 阶段 1：MVP 最小闭环

目标：

​```text
输入 → 核心处理 → 输出
本阶段实现：

1. __________

2. __________

本阶段不实现：

1. __________

2. __________

完成标准：

1. 能运行
2. 有测试
3. 有示例
4. 输出结果可检查

## 阶段 2：功能增强

实现：

1. __________

2. __________

完成标准：

1. 新功能有 spec
2. 新功能有测试
3. 不破坏阶段 1 的 golden test

## 阶段 3：工程化整理

实现：

1. 重构模块边界
2. 完善 README
3. 补充异常处理
4. 补充集成测试

## 阶段 4：高级功能

可选实现：

1. __________

2. __________

3. __________

​```
```

## 对 AI 的提示词

```text
请根据 PROJECT_CHARTER.md 生成 docs/ROADMAP.md。
要求：
1. 按阶段推进。
2. 第一阶段只做 MVP。
3. 明确每个阶段做什么、不做什么。
4. 不要把高级功能提前塞进第一阶段。
```

---

# 2.8 ADR 架构决策记录

## 文件位置

```text
project_root/docs/decisions/ADR-0001-xxx.md
```

## 文件定位

记录重要技术选择。  
防止后续 AI 或开发者反复推翻已经确定的设计。

## 模板内容

```markdown
# ADR-0001：__________

## 状态

Accepted

## 背景

当前面临的问题是：

​```text
__________
可选方案包括：

1. 方案 A：__________
2. 方案 B：__________
3. 方案 C：__________

## 决策

本项目决定采用：

​```text
__________
​```

## 理由

选择该方案的原因：

1. __________

2. __________

3. __________

## 后果

该决策带来的好处：

1. __________

2. __________

该决策带来的限制：

1. __________

2. __________

## 后续调整条件

如果出现以下情况，可以重新评估该决策：

1. __________

2. __________

​```
```

## 对 AI 的提示词

```text
请为当前技术选择生成一个 ADR 文件。
要求：
1. 说明背景、可选方案、最终决策、理由和后果。
2. 不要只写结论，要说明为什么这么选。
3. 文件命名为 docs/decisions/ADR-0001-xxx.md。
```

---

# 3. 功能 Spec 模板

每个功能都应该单独建一个目录：

```text
docs/specs/<feature_name>/
├── requirements.md
├── design.md
├── tasks.md
└── acceptance_tests.md
```

例如：

```text
docs/specs/parse_input_file/
docs/specs/query_table/
docs/specs/generate_report/
```

---

# 3.1 requirements.md

## 文件定位

说明这个功能要实现什么。

## 模板内容

```markdown
# Requirements: __________

## 1. 功能目标

本功能要实现：

​```text
__________
## 2. 输入

输入包括：

1. __________

2. __________

## 3. 输出

输出包括：

1. __________

2. __________

## 4. 业务规则

1. __________

2. __________

3. __________

## 5. 不做的内容

本功能暂不实现：

1. __________

2. __________

## 6. 异常情况

需要处理以下异常：

1. 输入缺失：__________
2. 格式错误：__________
3. 查询失败：__________
4. 其他：__________
```

------

# 3.2 design.md

## 文件定位

说明这个功能怎么实现。

## 模板内容

```markdown
# Design: __________

## 1. 设计目标

本功能设计目标：

1. 简单可测试
2. 不影响其他模块
3. 可后续扩展

## 2. 相关文件

需要新增或修改：

| 文件 | 操作 | 说明 |
|---|---|---|
| src/... | 新增 / 修改 | ______ |
| tests/... | 新增 / 修改 | ______ |
| examples/... | 新增 / 修改 | ______ |

## 3. 核心流程

​```text
步骤 1：__________
步骤 2：__________
步骤 3：__________
步骤 4：__________
## 4. 核心函数设计

### 函数：__________

输入：

​```text
__________
​```

输出：

​```text
__________
​```

说明：

​```text
__________
​```

## 5. 对已有系统的影响

是否影响已有接口：

​```text
是 / 否
​```

如果影响，说明：

​```text
__________
​```
```

------

# 3.3 tasks.md

## 文件定位

把功能拆成 AI 可以逐个完成的小任务。

## 模板内容

```

# Tasks: __________

## Task 1：__________

目标：

​```text
__________
修改文件：

​```text
__________
​```

验收标准：

1. __________

2. __________

---

## Task 2：__________

目标：

​```text
__________
​```

修改文件：

​```text
__________
​```

验收标准：

1. __________

2. __________

---

## Task 3：__________

目标：

​```text
__________
​```

修改文件：

​```text
__________
​```

验收标准：

1. __________

2. __________
```

------

# 3.4 acceptance_tests.md

## 文件定位

说明怎么判断功能完成。

## 模板内容

```
# Acceptance Tests: __________

## 1. 正常输入测试

输入：

​```text
__________
期望输出：

​```text
__________
​```

通过标准：

1. __________

2. __________

## 2. 异常输入测试

输入：

​```text
__________
​```

期望行为：

​```text
__________
​```

## 3. 回归测试

要求：

1. 原有测试必须通过
2. 原有示例必须能运行
3. 不得改变已有公开输出格式

## 4. 运行命令

​```bash
pytest
​```

或：

​```bash
pytest tests/test_xxx.py
​```
```

------

# 4. 常用 AI 提示词模板

---

## 4.1 项目初始化提示词

```text
请根据下面的项目想法，初始化一个适合 AI 辅助开发的项目结构。

项目想法：
【在这里写项目背景和目标】

要求：
1. 先生成目录结构。
2. 生成 README.md、AGENTS.md、docs/PROJECT_CHARTER.md。
3. 如果项目属于中型项目，请额外生成 docs/ARCHITECTURE.md、docs/DATA_CONTRACT.md、docs/TESTING_STRATEGY.md。
4. 不要直接实现复杂功能。
5. 第一阶段只保留 MVP 所需结构。
6. 每个新文件都说明路径和用途。
```

---

## 4.2 生成功能 Spec 的提示词

```text
请根据当前项目文档，为下面这个功能生成 spec。

功能名称：
【feature_name】

功能目标：
【写清楚本功能要做什么】

要求生成以下文件内容：
1. docs/specs/<feature_name>/requirements.md
2. docs/specs/<feature_name>/design.md
3. docs/specs/<feature_name>/tasks.md
4. docs/specs/<feature_name>/acceptance_tests.md

要求：
1. 只描述这个功能，不要扩大范围。
2. 明确输入、输出、异常情况和验收标准。
3. tasks.md 中每个任务都要足够小，适合 AI 单独实现。
4. 不要直接写代码。
```

---

## 4.3 实现单个 Task 的提示词

```text
请阅读以下文件：

1. AGENTS.md
2. docs/PROJECT_CHARTER.md
3. docs/ARCHITECTURE.md
4. docs/DATA_CONTRACT.md
5. docs/specs/<feature_name>/requirements.md
6. docs/specs/<feature_name>/design.md
7. docs/specs/<feature_name>/tasks.md

现在只实现：

docs/specs/<feature_name>/tasks.md 中的 Task X。

要求：
1. 只修改 Task X 相关文件。
2. 不要修改无关模块。
3. 不要大范围重构。
4. 不要写死 examples 或 tests 中的数据。
5. 必须新增或更新测试。
6. 完成后说明：
   - 修改了哪些文件
   - 为什么修改
   - 如何运行测试
   - 是否影响已有接口
```

---

## 4.4 Debug 提示词

```text
程序运行时报错如下：

【粘贴完整报错】

相关输入是：

【粘贴运行命令或输入文件说明】

请帮我定位并修复问题。

要求：
1. 先解释错误原因。
2. 只修改导致错误的最小范围。
3. 不要顺手重构无关代码。
4. 修复后补充一个能覆盖该错误的测试。
5. 完成后说明修改文件和测试方式。
```

---

## 4.5 代码审查提示词

```text
请作为代码审查者检查当前修改。

重点检查：
1. 是否违反 AGENTS.md。
2. 是否有 hard-code。
3. 是否破坏 DATA_CONTRACT.md。
4. 是否修改了无关文件。
5. 是否缺少测试。
6. 是否有过度设计。
7. 是否存在异常处理不清晰的问题。
8. 是否需要更新 README 或文档。

请不要直接修改代码，先输出审查意见和建议修改清单。
```

---

## 4.6 重构提示词

```text
请根据当前代码情况，提出一个小范围重构方案。

要求：
1. 不改变外部行为。
2. 不改变公开输入输出格式。
3. 不改变测试期望结果。
4. 先说明重构目标。
5. 再列出计划修改文件。
6. 最后给出验证方式。
7. 在我确认前不要直接修改代码。
```

---

## 4.7 文档同步提示词

```text
请根据最近的代码修改，同步更新项目文档。

需要检查：
1. README.md
2. docs/ARCHITECTURE.md
3. docs/DATA_CONTRACT.md
4. docs/TESTING_STRATEGY.md
5. 当前功能 spec

要求：
1. 只更新与本次代码修改相关的文档。
2. 不要重写整个文档。
3. 明确说明每个文档更新了什么。
```

---

# 5. 推荐开发节奏

## 5.1 每个新项目的启动顺序

推荐顺序：

```text
1. 写 PROJECT_CHARTER.md
2. 写 AGENTS.md
3. 写 README.md
4. 写 ARCHITECTURE.md
5. 写 DATA_CONTRACT.md
6. 写 TESTING_STRATEGY.md
7. 写第一个功能 spec
8. 实现第一个 MVP 垂直切片
9. 加测试
10. 再扩展功能
```

---

## 5.2 每个功能的开发顺序

推荐顺序：

```text
1. requirements.md
2. design.md
3. tasks.md
4. acceptance_tests.md
5. 实现 Task 1
6. 测试 Task 1
7. 实现 Task 2
8. 测试 Task 2
9. 集成测试
10. 更新文档
```

---

## 5.3 每次让 AI 修改代码的最小单位

不要说：

```text
帮我把整个系统完善一下。
```

应该说：

```text
请只实现 docs/specs/xxx/tasks.md 中的 Task 2。
只允许修改 src/xxx.py 和 tests/test_xxx.py。
不要修改其他文件。
```

---

# 6. 示例：用机械加工参数项目类比

假设项目是“机械加工参数推荐 MVP”。

## PROJECT_CHARTER.md 可以这样写

```markdown
当前阶段只实现：

1. 输入一份工艺规程 markdown 文件。
2. 用户指定一个工步编号。
3. 程序提取该工步内容。
4. 程序读取一张标准化 jsonl 参数表。
5. 输出候选参数范围。
6. 输出一份 markdown 格式计算说明。

当前阶段不实现：

1. 不接入 LangGraph。
2. 不做完整 Agent。
3. 不自动处理所有工步。
4. 不做复杂多目标优化。
5. 不生成完整数据库系统。
```

## DATA_CONTRACT.md 可以这样写

```markdown
输入：

1. process_file: markdown 文件路径
2. step_id: 工步编号
3. table_file: jsonl 表格路径

输出：

1. step_id
2. extracted_step_text
3. matched_table_rows
4. parameter_range
5. calculation_explanation
6. warnings
```

## 第一个功能 spec 可以是

```text
docs/specs/parse_process_step/
```

只做一件事：

```text
从 markdown 工艺规程中，根据 step_id 提取对应工步文本。
```

不做：

```text
不查表，不计算参数，不生成最终报告。
```

这样 AI 就不容易一上来把系统写乱。

---

# 7. 总结原则

1. 小项目可以少写文件，但必须有 `AGENTS.md` 和 `PROJECT_CHARTER.md`。
2. 中型项目必须增加 `ARCHITECTURE.md`、`DATA_CONTRACT.md`、`TESTING_STRATEGY.md`。
3. 大型项目必须增加 `MODULE_BOUNDARIES.md`、`API_CONTRACT.md`、`ADR`、`DEPLOYMENT.md` 等。
4. 每个功能都要有独立 spec。
5. 每次只让 AI 实现一个小任务。
6. AI 生成代码后，必须用测试锁住行为。
7. 重要设计决策必须写 ADR。
8. 文档不是形式主义，而是给 AI 提供稳定上下文。
9. Vibe Coding 的核心不是“随便让 AI 写”，而是“用清晰上下文驱动 AI 高速执行”。
10. 最好的 AI 辅助开发方式是：规格先行、测试兜底、小步迭代、人工验收。