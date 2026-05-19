# 一、最小必备文件：建议一开始就有

如果你只是想把项目跑通，最低配是：

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

也就是说，**最小必备不是十几个文件，而是 5~6 个核心文档**。

------

# 二、哪些需要你手写，哪些可以让 AI 生成？

## 1. 必须由你主导手写的文件

这些文件决定项目方向，不能完全交给 AI。

| 文件                  | 是否必须 | 谁来写                       | 原因                                                    |
| --------------------- | -------- | ---------------------------- | ------------------------------------------------------- |
| `PROJECT_CHARTER.md`  | 必须     | **你主写，AI润色**           | 决定项目目标、边界、MVP，AI不能替你决定做什么、不做什么 |
| `AGENTS.md`           | 必须     | **AI生成初稿，你修改确认**   | 里面是约束 AI 的规则，需要符合你的开发习惯              |
| `requirements.md`     | 必须     | **你提供核心需求，AI整理**   | 功能需求必须来自你，AI可以帮你结构化                    |
| `acceptance_tests.md` | 必须     | **你定验收标准，AI补充细节** | 什么叫“做完了”，必须由你说了算                          |

这几个文件里，最重要的是：

```text
PROJECT_CHARTER.md
requirements.md
acceptance_tests.md
```

因为它们决定了：

> 项目做什么、做到什么程度、怎么判断对不对。

------

## 2. 可以主要由 AI 生成的文件

这些文件偏工程表达，AI 很适合先生成。

| 文件                  | 是否必须             | 谁来写 | 你的工作                   |
| --------------------- | -------------------- | ------ | -------------------------- |
| `README.md`           | 必须                 | AI生成 | 你检查运行命令是否正确     |
| `tasks.md`            | 必须                 | AI生成 | 你检查拆分是否合理         |
| `ARCHITECTURE.md`     | 中型项目需要         | AI生成 | 你检查模块划分是否过度设计 |
| `DATA_CONTRACT.md`    | 有数据输入输出时需要 | AI生成 | 你确认字段名、格式、含义   |
| `TESTING_STRATEGY.md` | 中型项目建议         | AI生成 | 你确认测试重点             |
| `CODING_RULES.md`     | 可选                 | AI生成 | 你补充个人偏好             |
| `ROADMAP.md`          | 可选但推荐           | AI生成 | 你调整优先级               |

所以你不用自己苦写所有文档。

你应该做的是：

> **你给方向和边界，AI 生成结构化文档，你审查和修改。**

------

## 3. 初期不一定需要的文件

这些文件不是没用，而是**项目大了以后再补**。

| 文件                      | 初期是否需要 | 什么时候需要                     |
| ------------------------- | ------------ | -------------------------------- |
| `MODULE_BOUNDARIES.md`    | 不需要       | 模块很多、经常互相影响时         |
| `API_CONTRACT.md`         | 不需要       | 有前后端、接口服务、外部调用时   |
| `DEPLOYMENT.md`           | 不需要       | 需要部署到服务器或给别人使用时   |
| `SECURITY_AND_PRIVACY.md` | 不需要       | 涉及用户数据、账号、权限、安全时 |
| `CHANGELOG.md`            | 不需要       | 项目长期迭代、多版本发布时       |
| 多个 `ADR`                | 不需要       | 出现重要技术选择时再写           |

所以一开始不要把自己拖进“文档地狱”。

------

# 三、我建议你的实际工作方式

你每开一个新项目，先只做这 4 步。

------

## 第一步：你手写一个极简项目说明

不用正式，写成这样就行：

```markdown
# 项目初始说明

## 项目目标

我要做一个 ________。

## 当前阶段只做

1. ________
2. ________
3. ________

## 当前阶段不做

1. ________
2. ________
3. ________

## 输入

- ________

## 输出

- ________

## 成功标准

1. 能运行
2. 有示例
3. 有测试
4. 输出结果符合 ________
```

这一步必须你来写，因为这是项目灵魂。

------

## 第二步：让 AI 根据这个生成基础文档

你可以直接用这个提示词：

```text
请根据下面的项目初始说明，帮我生成 AI 辅助开发所需的最小文档集。

需要生成：
1. README.md
2. AGENTS.md
3. docs/PROJECT_CHARTER.md
4. docs/specs/mvp/requirements.md
5. docs/specs/mvp/tasks.md
6. docs/specs/mvp/acceptance_tests.md

要求：
1. 不要扩大项目范围。
2. 当前阶段只围绕 MVP。
3. 不要引入复杂架构。
4. 每个文件都要说明应该放在哪个路径。
5. 文档内容要便于后续交给 Codex/Cursor 实现。

项目初始说明如下：
【粘贴你的说明】
```

------

## 第三步：你重点检查 3 个地方

AI 生成后，你不用逐字改，但必须检查：

### 1. 有没有扩大范围？

比如你只想做 MVP，AI 却写了：

```text
接入数据库、前端页面、用户登录、云部署、智能 Agent 调度
```

这些都应该删掉。

------

### 2. 有没有写死输入？

比如 AI 写：

```text
程序只处理 case_001.md
```

或者：

```text
默认工步编号为 20-01
```

这种要改成通用逻辑。

------

### 3. 验收标准是否明确？

不好的验收标准：

```text
程序正常运行。
```

好的验收标准：

```text
运行 pytest 通过；
输入 examples/case_001.md 和参数 xxx；
输出中必须包含字段 A、B、C；
不能使用输入文件中的错误参数作为最终结果。
```

------

## 第四步：再让 AI 按 tasks.md 一项一项实现

不要让它一次做完全部项目。

应该这样说：

```text
请阅读 AGENTS.md 和 docs/specs/mvp/tasks.md。

现在只实现 Task 1。

要求：
1. 只修改 Task 1 相关文件。
2. 不要实现 Task 2、Task 3。
3. 不要大范围重构。
4. 新增必要测试。
5. 完成后说明修改了哪些文件、如何运行测试。
```

------

# 四、不同规模项目的推荐文件数量

## 小项目：3~6 个文档足够

适合课程作业、实验脚本、快速 Demo。

```text
README.md
AGENTS.md
docs/PROJECT_CHARTER.md
docs/specs/mvp/requirements.md
docs/specs/mvp/tasks.md
docs/specs/mvp/acceptance_tests.md
```

你手写：

```text
项目目标、输入、输出、当前不做什么、验收标准
```

AI 生成：

```text
README、AGENTS、requirements、tasks、acceptance_tests 的正式文本
```

------

## 中型项目：8~10 个文档比较合适

适合你说的那种“后面还要持续迭代”的项目。

增加：

```text
docs/ARCHITECTURE.md
docs/DATA_CONTRACT.md
docs/TESTING_STRATEGY.md
docs/ROADMAP.md
docs/decisions/ADR-0001-xxx.md
```

你手写或确认：

```text
核心架构方向
数据字段含义
MVP边界
重要技术选择
```

AI 生成：

```text
架构说明初稿
数据契约表格
测试策略
路线图
ADR初稿
```

------

## 大型项目：再补充更多文件

大型项目才需要：

```text
MODULE_BOUNDARIES.md
API_CONTRACT.md
DEPLOYMENT.md
SECURITY_AND_PRIVACY.md
CHANGELOG.md
CONTRIBUTING.md
```

这些一开始不必写。

------

# 五、最推荐你的做法

你可以采用这个原则：

> **第一天只写“方向文档”，不要写“管理文档”。**

也就是说，项目初期只保留：

```text
AGENTS.md
PROJECT_CHARTER.md
requirements.md
tasks.md
acceptance_tests.md
README.md
```

等项目跑通第一个 MVP 后，再补：

```text
ARCHITECTURE.md
DATA_CONTRACT.md
TESTING_STRATEGY.md
ROADMAP.md
```

等项目进入长期维护后，再补：

```text
ADR
MODULE_BOUNDARIES
API_CONTRACT
DEPLOYMENT
CHANGELOG
```

------

# 六、最关键的分工

你和 AI 的分工应该是：

| 工作             | 你负责           | AI负责               |
| ---------------- | ---------------- | -------------------- |
| 项目要不要做     | 是               | 否                   |
| 第一阶段做到哪里 | 是               | 辅助整理             |
| 哪些功能暂时不做 | 是               | 辅助整理             |
| 输入输出是否合理 | 是               | 辅助检查             |
| 文档格式化       | 否               | 是                   |
| 任务拆分         | 共同             | 是，之后你审查       |
| 写代码           | 否，除非关键逻辑 | 是                   |
| 写测试           | 共同             | 是，之后你审查       |
| 判断结果是否正确 | 是               | 只能辅助             |
| 架构是否过度设计 | 是               | 容易犯错，需要你控制 |

一句话：

> **你手写方向，AI生成文档和代码；你验收关键逻辑，AI负责快速迭代。**

对于你这种中大型项目，我最建议从 **6 个文件最小模板** 开始，而不是一上来把所有文档都建满。