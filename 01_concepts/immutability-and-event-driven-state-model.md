# 不可变与事件驱动：构建可控状态系统的核心模型

## 1. 文档目的

本文回答三个核心问题：

1. 不可变（Immutable）到底是什么
2. 不可变为什么是系统复杂度控制的基础机制
3. 不可变与事件（Event）如何组合成可追踪、可验证、可演化的状态系统

## 2. 不可变的定义

不可变的最小定义：

`对象一旦创建，其状态不能被原地修改。`

其本质不是“风格偏好”，而是：

`状态变化必须通过新对象表达，而不是修改旧对象。`

## 3. 为什么不可变重要

### 3.1 消除隐式状态漂移

可变对象允许任何位置直接修改状态，导致：

- 谁改的不可知
- 为什么改不可知
- 是否合法不可知

不可变要求变化必须显式发生，状态变化从“隐式副作用”变成“显式行为”。

### 3.2 天然提升并发安全性

不可变对象没有共享写入，天然降低竞争条件与锁复杂度。

### 3.3 支撑值语义与不变量

对 Value Object 而言，不可变是语义成立前提：

- 值相等才有稳定意义
- 不变量只需在构造时校验一次

## 4. 可变与不可变的系统差异

| 维度 | 可变方式 | 不可变方式 |
| --- | --- | --- |
| 状态变化 | 原地修改对象 | 创建新对象表示变化 |
| 可追踪性 | 弱 | 强 |
| 可验证性 | 弱 | 强 |
| 并发安全 | 易冲突 | 更稳定 |

工程结论：

`不可变的代价是对象创建增多，收益是复杂度可控。`

## 5. 事件（Event）的工程语义

事件不是消息队列技术名词，而是：

`已经发生的事实（Past Fact）。`

例如：

- `OrderCreated`
- `OrderPaid`
- `DeviceStarted`
- `SpeedChanged`

事件用于回答：

- 发生了什么
- 在什么时间发生
- 哪个对象发生

## 6. 不可变 + 事件：状态系统完整模型

系统运行可表达为：

`System = State + Event + Apply`

其中：

- State：当前结果
- Event：状态变化的事实来源
- Apply：状态演化函数（根据事件推导新状态）

一句话：

`不要直接修改状态，用事件驱动状态变化。`

## 7. 状态演化示例（订单）

事件序列：

- `OrderCreated`
- `OrderPaid`
- `OrderShipped`
- `OrderCompleted`

状态演化：

```text
Created
  -> OrderPaid
Paid
  -> OrderShipped
Shipped
  -> OrderCompleted
Completed
```

关键点：

- 状态不是“被改掉”的
- 状态是“由事件推导”的

## 8. 代码层实现模式（推荐）

### 8.1 事件使用不可变结构

```csharp
public record OrderPaid(Guid OrderId);
```

### 8.2 状态演化函数返回新对象

```csharp
public Order Apply(OrderPaid evt)
{
    return this with { Status = OrderStatus.Paid };
}
```

### 8.3 业务行为产生事件

```csharp
public OrderPaid Pay()
{
    if (Status != OrderStatus.Created)
        throw new InvalidOperationException();

    return new OrderPaid(Id);
}
```

原则：

- 行为负责生成事件
- 演化函数负责更新状态
- 更新以新对象返回

## 9. 在工程中的落地层次

不必一开始就采用完整 Event Sourcing。

建议先用简化模型：

1. 当前状态持久化（主存储）
2. 关键事件用于驱动与通知
3. 逐步增加事件回放与审计能力

即：

`事件驱动 + 状态存储`（先于“全量事件存储”）

## 10. 在设备/上位机场景中的映射

可将系统拆为：

- 状态：`DeviceState`（Running/Speed/Tension...）
- 事件：`DeviceStarted`、`SpeedChanged`、`PhotoCaptured`
- 演化：`state = state.Apply(event)`

当系统对外推送事件消息（eventName / data / ts / eventId）时，
本质上是在输出可追踪的状态演化流。

## 11. 常见误区

1. 把不可变当成编码风格，而非复杂度控制机制。
2. 只引入事件命名，不建立状态演化规则。
3. 行为直接改状态，不通过事件表达变化。
4. 一开始就追求完整事件溯源，导致实现负担过重。

## 12. 结论

不可变的价值不在“禁止修改”，而在“控制变化”。

当不可变与事件驱动结合后，系统获得：

- 可追踪的变化来源
- 可验证的状态演化路径
- 可扩展的长期架构基础

最终模型可总结为：

`用类型定义状态，用事件驱动变化，用不可变保证正确性。`

## 延伸阅读

- `system-modeling-dimensions-and-uml.md`
- `explicit-modeling-boundaries-and-moderate-abstraction.md`
- `interface-essence-and-three-layer-system-model.md`
