# 优先建模原则：先建立系统模型，再使用结构与命名表达

## 1. 文档目的

本文用于确立一条工程实践中的核心原则：

`优先建立正确的系统模型，而不是依赖命名或结构来表达模型。`

目标是减少命名争议、结构误用和职责漂移，提升系统认知一致性与可演化性。

## 2. 问题背景

在真实开发中，命名和目录结构经常变化：

- `Service` / `Manager` / `Handler` 语义混用
- `WebApi` / `Api` / `Host` 命名歧义
- 项目结构随阶段反复调整
- 同名概念在不同团队中含义不一致

根因是：

`命名与结构都不足以承载稳定语义。`

## 3. 核心结论

工程设计应遵循以下次序：

1. 先建模（Model）
2. 再落结构（Structure）
3. 最后命名（Naming）

可简化为：

- 命名用于沟通
- 结构用于组织
- 模型用于决策

## 4. 三层概念区分（必须明确）

### 4.1 Model（模型，本质层）

回答“这个东西是什么、承担什么本质职责”。

示例：

- Host：系统运行入口
- Adapter：协议适配
- Domain：业务规则核心
- Infrastructure：技术实现

特性：

- 稳定
- 可作为架构判断依据

### 4.2 Structure（结构，组织层）

回答“代码如何组织与映射模型”。

示例：

- 项目拆分（WebApi / Application / Domain）
- 目录结构（Controllers / Services / DI）
- 分层方式（Clean / DDD）

特性：

- 可演进
- 是模型的映射，不是模型本身

### 4.3 Naming（命名，表达层）

回答“如何标记和沟通”。

示例：

- 项目名、类名、目录名
- `OrderService` / `OrderManager`

特性：

- 便于沟通，但不具备约束力
- 不应作为职责判定依据

## 5. 常见错误模式

### 错误 1：用命名替代建模

表现：看到 `Service` 就假定是业务核心。

后果：同名不同义，语义漂移，重命名频繁。

### 错误 2：用结构替代认知

表现：先套分层模板，再补职责解释。

后果：结构看似正确，行为实际混乱。

### 错误 3：通过改名解决根因问题

表现：`Service -> Manager -> Handler -> UseCase` 不断改名。

后果：表象变化，职责边界仍未清晰。

## 6. 正确方法：先模型，后结构，再命名

### Step 1：定义模型职责

先回答：

`这个单元的本质职责是什么？`

判断示例：

- 系统运行入口 -> Host
- 协议转换 -> Adapter
- 业务规则 -> Domain
- 技术接入 -> Infrastructure

### Step 2：将模型映射到结构

示例：

- Domain -> Domain 项目
- Application -> Application 项目
- Infrastructure -> Infrastructure 项目
- Host + Adapter -> Web 项目

### Step 3：命名作为表达，不作为定义

命名可以迭代，但模型职责不应被命名反向决定。

## 7. 案例：WebApi 项目的职责澄清

### 7.1 常见现象

ASP.NET Core 的 `WebApi` 项目通常包含：

- Controller
- `Program.cs`
- DI 注册
- 配置加载
- Middleware 管道

### 7.2 误区

误把 `WebApi` 仅理解为“接口层”。

### 7.3 正确模型

`WebApi = Host + Web Adapter`

- Host：启动、组装、配置、管道构建
- Web Adapter：HTTP 转换、DTO 映射、协议相关处理

关键点：

- Host 属于运行结构维度
- Web 属于分层结构维度
- 两者可在同一项目共存

### 7.4 命名处理建议

保持生态常见命名（如 `MyApp.Api` / `MyApp.WebApi`），
通过结构与说明文档明确内部职责分区。

## 8. 工程实践规则（建议固化）

1. 任何可执行项目默认视为 Host。
2. Adapter 可附着在 Host 上，不必强制拆分独立项目。
3. 业务逻辑必须独立于 Host 与协议细节。
4. 不根据名字判定职责，只根据模型判定职责。

## 9. 健康度检查清单

### 健康状态

- Web 项目负责启动、DI、Endpoint 映射
- Controller 仅做协议转换
- Application 承担用例编排
- Domain 承担业务规则

### 不健康状态

- Controller 编排核心业务决策
- `Program.cs` 含业务判断
- Application 依赖 `HttpContext`
- 内层直接依赖外层基础设施细节

## 10. 结论

命名不稳定，结构可变，模型才是系统稳定语义的来源。

工程实践中应坚持：

- 先建模，再分层，再命名
- 用模型统一认知
- 用结构承载模型
- 用命名辅助沟通

一句话原则：

`优先建立正确的系统模型，而不是依赖命名或结构来表达模型。`

## 延伸阅读

- `system-modeling-dimensions-and-uml.md`
- `ddd-clean-core-unification.md`
- `software-engineering-processes.md`
