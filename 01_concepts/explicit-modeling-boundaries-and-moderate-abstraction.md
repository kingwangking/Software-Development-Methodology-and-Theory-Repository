# 显式建模的边界与适度抽象

## 1. 文档目的

本文聚焦一个核心工程问题：

`在软件开发中，应将概念显式建模到什么程度。`

以 DDD 的 Domain 层为例，讨论：

- 显式建模的价值与风险
- 概念是否应落地为代码结构的判断标准
- 抽象程度的控制方法
- 如何形成稳定且可演进的 Domain 骨架

本文重点不是 DDD 概念介绍，而是：

`如何对概念进行适度建模。`

## 2. 背景：建模能力与工程现实的张力

现代语言与平台（如 C# / .NET）具备很强的建模能力：

- 类型系统（class / record / struct）
- 抽象机制（interface / abstract class / generic）
- 依赖约束（项目分层、编译期校验）
- 元数据机制（attribute / reflection / source generator）

这意味着几乎所有设计意图都可以代码化。

但关键问题是：

`不是所有可以建模的东西，都应该建模。`

若不控制边界，常见后果包括：

- 抽象层级膨胀
- 结构复杂但语义不清
- 业务模型被技术骨架淹没
- “看起来规范”，实际维护困难

## 3. 核心判断：显式建模的三条边界

### 3.1 稳定性边界

只有稳定概念才应进入核心骨架。

若抽象随场景频繁变化、定义不确定，则不应过早固化为基类/接口。

### 3.2 复用性边界

高频重复且实现一致的模式，适合抽象。

单点使用或差异大的场景，抽象收益低，易过度设计。

### 3.3 语义增强边界

抽象的价值不只在复用，还在语义表达。

例如：

- `ValueObject`：按值相等
- `AggregateRoot`：聚合边界
- `DomainException`：领域规则错误

若抽象不增加语义信息，仅做形式统一，价值通常较低。

## 4. DDD Domain 层：边界问题的典型场景

DDD 提供了丰富概念：

- Entity
- Value Object
- Aggregate Root
- Domain Event
- Repository
- Domain Service
- Specification

从“可实现性”看，这些概念都可代码化；
从“工程可维护性”看，不应一次性全部落地。

关键不是“能不能做”，而是“是否值得现在做”。

## 5. 在 Domain 层中，优先显式建模的概念

### 5.1 Entity（建议显式）

理由：

- 稳定（身份 + 生命周期）
- 高频（多个聚合复用）
- 语义强

推荐：`Entity<TId>`

### 5.2 ValueObject（最值得显式）

理由：

- 规则稳定（值相等）
- 实现细节重复（Equals / HashCode）
- 使用频率高

推荐：`ValueObject`

### 5.3 AggregateRoot（轻量显式）

理由：

- 语义重要（聚合边界）
- 功能需求通常较轻

推荐：`AggregateRoot<TId>` 或 `IAggregateRoot`

### 5.4 DomainException（建议显式）

理由：

- 区分业务规则错误与技术异常
- 语义清晰、使用稳定

推荐：`DomainException`

### 5.5 DomainEvent（轻量预留）

理由：

- 模型稳定
- 扩展价值高
- 初期无需完整事件总线体系

推荐：`IDomainEvent`（先预留接口）

## 6. 不应过早显式建模的概念

### 6.1 Repository 泛型基类

风险：

- 查询语义难统一
- 易退化为“半通用 + 特例泛滥”

建议：按聚合定义专用接口，如 `IOrderRepository`。

### 6.2 Domain Service 基类

风险：

- 角色标签化，约束价值弱
- 为抽象而抽象

建议：由依赖与语义需求驱动是否定义抽象。

### 6.3 Specification（早期）

风险：

- 增加额外抽象层
- 简单系统收益有限

建议：在规则组合复杂到一定程度后引入。

### 6.4 横切接口（审计/软删除等）

风险：

- 技术关注点压过业务语义
- Domain 层被管理逻辑占据

建议：真实需求出现后再逐步加入。

## 7. Domain 骨架设计的正确方式

### 7.1 从最小语义集合启动

优先沉淀：

- Entity
- ValueObject
- AggregateRoot
- DomainException

目标：建立统一语言，不是搭完整框架。

### 7.2 骨架必须服务业务语义

Domain 层应优先呈现业务对象（Order/Customer/Payment），
而非优先暴露技术骨架（IRepository/ISpecification）。

### 7.3 抽象应从业务中生长

正确顺序：

1. 建模具体业务
2. 识别重复模式
3. 抽象共性
4. 回收进 seedwork

### 7.4 Seedwork 保持稳定

Seedwork 会被广泛依赖，应遵循：

- 宁少勿滥
- 低频变更
- 高确定性优先

## 8. 推荐的最小 Domain seedwork

```text
SeedWork
├── Entity.cs
├── AggregateRoot.cs
├── ValueObject.cs
├── DomainException.cs
└── IDomainEvent.cs
```

特点：

- 语义明确
- 结构轻量
- 长期可复用
- 便于按需扩展

## 9. 常见错误模式

### 9.1 把 DDD 当框架先行实现

表现：先搭完整基础设施，再写业务。

问题：抽象脱离问题，复杂度提前释放。

### 9.2 概念与代码结构机械一一映射

问题：DDD 是建模语言，不是代码清单。

### 9.3 技术统一压制领域语义

表现：过度泛型化、接口化。

问题：业务语言弱化，可读性下降。

## 10. 结论

显式建模的关键不是能力，而是边界。

正确实践不是“不建模”，也不是“全建模”，而是：

`只显式建模那些稳定、高频、具备语义价值的核心概念。`

最终目标不是“完整 DDD 框架”，而是：

`构建语义清晰、约束适度、可持续演进的 Domain 模型。`

## 延伸阅读

- `ddd-clean-core-unification.md`
- `abstraction-ownership-define-implement-use.md`
- `model-first-over-naming-and-structure.md`
