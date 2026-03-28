# 抽象解释权模型：Define / Implement / Use 的职责分工

## 1. 文档目的

本文用于澄清 Clean + DDD 中一个高频混淆点：

- 定义接口
- 实现接口
- 使用接口

这三者不应混为一谈。

目标是建立一个稳定判断模型，避免“谁提供功能谁就应使用功能”这类误导性结论。

## 2. 核心结论

在分层架构中，能力可以分离为三种角色：

1. Define（定义者）：定义抽象接口与业务语义
2. Implement（实现者）：提供技术实现
3. Use（使用者）：在业务流程中调用能力

一句话原则：

`谁负责业务语义建模，谁定义抽象；谁掌握技术细节，谁实现抽象；谁负责业务流程，谁使用抽象。`

## 3. 三角色模型

### 3.1 Define（定义者）

职责：定义“这个能力在业务上是什么意思”。

在 DDD/Clean 中通常由 Domain（或 Domain + Application 边界）承担。

判断标准：

- 方法签名是否体现业务语义
- 参数/返回值是否是领域概念
- 抽象是否用于保护核心语义

### 3.2 Implement（实现者）

职责：实现抽象能力的技术细节。

通常由 Infrastructure 承担。

判断标准：

- 是否依赖具体框架、中间件、数据库
- 是否涉及协议、存储、网络、序列化等实现细节

### 3.3 Use（使用者）

职责：在用例流程中调用能力，组织业务执行顺序。

通常由 Application（Use Case）承担。

判断标准：

- 是否在“当前流程”需要该能力
- 是否负责步骤编排、事务边界、交互顺序

## 4. 示例：IOrderRepository

示例调用：

```csharp
await _orderRepository.AddAsync(order, cancellationToken);
```

角色对应：

- Define：Domain 定义 `IOrderRepository`（语义是“订单如何被存取”）
- Implement：Infrastructure 实现（EF Core/SQL/Redis 等）
- Use：Application Use Case 调用（在“创建订单”流程中触发保存）

## 5. 常见误区与修正

### 误区 1：提供者必须同时是使用者

错误理解：谁提供功能谁来使用。

问题：混淆了能力提供与流程控制。

正确做法：提供者与使用者可分离。

### 误区 2：领域对象应直接持久化自己

错误写法：

```csharp
order.Save(); // 反模式
```

风险：

- Domain 反向依赖 Infrastructure
- 领域模型被技术细节污染
- 流程控制权下沉到实体对象，导致边界失控

正确做法：

- Domain 保持语义与规则
- Application 控制流程
- Infrastructure 负责技术落地

### 误区 3：把依赖倒置理解成“调用方向技巧”

更本质的理解是：

`依赖倒置是在控制抽象解释权归属。`

## 6. 解释权模型（核心）

可将系统核心理解为“业务语义的定义权”集中位置。

```text
Domain         -> 定义“是什么”（语义、规则、不变量）
Application    -> 决定“什么时候用”（流程、用例、编排）
Infrastructure -> 决定“怎么实现”（技术、协议、存储）
```

在该模型下：

- Domain 拥有抽象解释权
- Application 拥有流程控制权
- Infrastructure 拥有实现执行权

## 7. 架构检查清单

在评审时可用以下问题快速检查是否分工清晰：

1. 抽象接口是否由业务语义主导定义？
2. Application 是否承担流程编排而非纯转发？
3. Domain 是否不依赖外层技术细节？
4. Infrastructure 是否只实现、不反向定义业务语义？
5. 是否存在“实体主动调用外部资源”的反向依赖？

若存在多项否定，应优先修复解释权归属。

## 8. 结论

依赖倒置不只是“谁调用谁”的问题，而是“谁拥有抽象解释权”的问题。

当 Define / Implement / Use 被正确分离时，系统才具备：

- 语义稳定性
- 流程可控性
- 实现可替换性

最终原则：

`核心不在功能集合，而在业务语义定义权是否稳定地落在内层。`

## 延伸阅读

- `ddd-clean-core-unification.md`
- `problem-vs-requirement-driven-development.md`
- `model-first-over-naming-and-structure.md`
- 接口本质与三层模型：`./interface-essence-and-three-layer-system-model.md`
