# 系统建模维度与 UML 对应关系

## 1. 文档目的

本文给出一套可执行的系统建模框架，用于统一回答三个问题：

1. 系统由什么组成（Structure）
2. 系统如何运行（Flow）
3. 系统在什么约束下运行（State）

并建立这些维度与 UML 图的对应关系，形成从问题定义到实现落地的建模流程。

## 2. 系统建模的核心维度

系统建模可归纳为三个核心维度：

`Structure + Flow + State`

### 2.1 Structure（结构）

定义：系统的静态组成与组织方式。

关注问题：

- 系统由什么组成？
- 模块如何划分？
- 依赖关系如何？

典型建模对象：

- 分层结构（Presentation / Application / Domain / Infrastructure）
- 模块边界（Service / Adapter / Repository）
- 类与接口关系

### 2.2 Flow（流程）

定义：系统运行时的执行路径与调用顺序。

关注问题：

- 事情如何发生？
- 谁调用谁？
- 执行顺序与并发关系是什么？

典型建模对象：

- 用例执行流程
- 调用链
- 同步/异步路径

### 2.3 State（状态）

定义：对象生命周期中的状态及其转换规则。

关注问题：

- 当前处于什么状态？
- 是否允许执行某个操作？
- 状态如何转移？

典型建模对象：

- 状态机
- 生命周期管理
- 不变量与业务约束

## 3. UML 图与建模维度映射

| 建模维度 | UML 图 | 主要表达内容 |
| --- | --- | --- |
| Structure | 架构图 / 类图 / 组件图 | 系统组成与依赖关系 |
| Flow | 时序图 / 活动图 | 执行路径与行为顺序 |
| State | 状态图 | 生命周期与合法性约束 |
| Data（补充） | 数据流图（非 UML 标准） | 数据流动与转换 |
| Problem Space（补充） | 用例图 | 系统目标与边界 |

### 3.1 用例图（Use Case）

用于定义问题空间：系统要做什么、边界在哪里、谁参与。

### 3.2 架构图 / 组件图

用于结构建模：分层、模块职责、依赖方向。

### 3.3 类图

用于细粒度结构：类关系、接口设计、模式落点。

### 3.4 时序图（Sequence Diagram）

用于流程建模：调用顺序、交互过程、协作关系。

### 3.5 状态图（State Diagram）

用于规则建模：生命周期、状态转移条件、约束表达。

### 3.6 数据流图（DFD，补充）

用于数据视角补充：数据来源、处理、存储与输出路径。

## 4. 统一运行模型

系统运行可统一表达为：

`System = Structure + (Flow × State)`

解释：

- Structure：提供执行能力的承载体
- Flow：定义执行路径
- State：定义执行是否合法

一句话：

`结构决定“由谁执行”，流程决定“怎么执行”，状态决定“是否允许执行”。`

## 5. 标准建模流程

### Step 1：定义问题（Use Case）

问题：系统要做什么？

输出：系统边界、参与者、核心用例。

### Step 2：设计结构（Architecture / Component）

问题：系统由谁组成？

输出：分层结构、模块职责、依赖关系。

### Step 3：建模流程（Sequence）

问题：系统如何运行？

输出：调用链、执行顺序、同步/异步关系。

### Step 4：建模状态（State）

问题：系统是否允许这样运行？

输出：状态机、状态转换规则、不变量约束。

### Step 5：细化实现（Class，按需）

问题：如何实现？

输出：类设计、接口定义、模式落地。

## 6. 建模关系总览

```text
Problem
  -> Use Case
  -> Architecture
  -> Execution Model
       ├─ Flow
       └─ State
  -> Implementation
```

## 7. 关键建模原则

1. 图是分析工具，不是交付目的。
2. 不同问题用不同图，不做图形堆叠。
3. Flow 必须服从 State，流程不能绕过状态约束。
4. 时序必须服从架构分层与依赖方向。
5. 业务状态与规则应优先落在 Domain 层。

## 8. 结论

系统建模的本质是：

- 用结构定义能力边界
- 用流程组织执行路径
- 用状态约束行为合法性

从而构建一个可预测、可控、可演化的系统运行机制。

最终结论：

`系统不是代码的集合，而是结构 + 流程 + 状态的统一体。`

## 延伸阅读

- `software-engineering-processes.md`
- `../02_workflow-analysis/software-development-growth-process.md`
- `../02_workflow-analysis/ai-executable-workflow.md`
- DDD 与 Clean 的核心统一：`./ddd-clean-core-unification.md`
- 模型优先原则：`./model-first-over-naming-and-structure.md`
- 不可变与事件驱动状态模型：`./immutability-and-event-driven-state-model.md`
