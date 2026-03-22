# Software Engineering Processes

## 概述

在软件工程实践中，流程不应只被理解为一条线性的“开发步骤”。
更完整的视角是：软件从规划、实现、上线到持续运行，需要多类流程协同。

本文采用“**五类流程为核心**、**三层流程为总结**”的方式来建立流程体系。

## 一、五类流程（核心内容）

### 1. 产品生命周期流程（SDLC）

`Software Development Life Cycle`

这是一条最经典的主流程，用来回答：**软件如何被生产出来**。

典型阶段：

1. 需求分析（Requirements）
2. 系统设计（Design）
3. 开发实现（Implementation）
4. 测试验证（Testing）
5. 部署上线（Deployment）
6. 维护迭代（Maintenance）

核心关注：

- 功能是否正确满足业务目标
- 需求是否清晰、可验证、可交付
- 质量是否达到发布标准
- 版本是否可管理、可追踪

常见输出物：

- 需求文档、用户故事、验收标准
- 设计文档、架构图、接口定义
- 测试计划、测试报告、发布记录
- 版本计划、迭代复盘结论

### 2. 工程环境流程（Environment / Infrastructure Setup）

`Engineering Environment Setup Process`

这类流程回答：**软件运行所需的基础设施如何准备**。

典型活动：

1. 准备服务器或云资源
2. 安装数据库、中间件、缓存组件
3. 安装运行时（如 .NET / Node.js / Java）
4. 配置环境变量与密钥管理
5. 配置 CI/CD 管道
6. 配置日志、监控、告警体系

核心关注：

- 环境一致性（开发/测试/生产尽量一致）
- 可重复性（可以自动化重建）
- 可观测性（问题可发现、可定位）
- 安全性（权限、凭据、网络边界）

关键词：

- `Environment`
- `Infrastructure`
- `Provisioning`

### 3. 开发者流程（Developer Workflow）

`Developer Workflow` / `Git Workflow`

这类流程回答：**开发者如何高效协作并稳定交付代码**。

典型流程：

1. 拉取最新代码（Pull）
2. 创建分支（Branch）
3. 开发与本地验证（Develop & Verify）
4. 提交代码（Commit）
5. 发起 PR/MR（Pull/Merge Request）
6. 代码评审（Code Review）
7. 合并主干并触发发布（Merge & Release）

常见协作模型：

- `GitFlow`
- `Trunk Based Development`

核心关注：

- 协作效率（减少冲突与等待）
- 变更可追踪（提交信息、PR 讨论记录）
- 变更可回滚（控制风险半径）
- 团队规范统一（分支策略、评审规则）

### 4. 运维流程（Operations / DevOps）

`Operations Workflow` / `DevOps Process`

这类流程回答：**软件上线后如何稳定运行、快速恢复并持续优化**。

典型活动：

1. 版本发布（全量、灰度、金丝雀）
2. 版本回滚与应急预案
3. 系统监控（指标、日志、链路）
4. 告警处理与事件响应
5. 日志分析与根因定位
6. 容量规划与弹性扩缩容

核心关注：

- 可用性与稳定性（SLA/SLO）
- 故障恢复速度（MTTR）
- 变更风险控制
- 成本与性能平衡

### 5. 质量流程（Quality Assurance Process）

`QA Process`

这类流程回答：**如何系统性保障软件质量**，并贯穿开发全周期。

典型活动：

1. 代码规范检查（Lint/Style）
2. 代码审查（Code Review）
3. 自动化测试（单元/集成/端到端）
4. 安全扫描（依赖漏洞、静态扫描）
5. 性能测试（压测、容量测试）

核心关注：

- 缺陷预防优先于缺陷修复
- 质量门禁左移（Shift Left）
- 自动化覆盖率与有效性
- 安全与性能的非功能质量保障

---

## 二、完整的软件工程流程体系

将五类流程放在一起，可以形成更清晰、可治理的全景：

```text
软件工程流程
│
├─ 产品生命周期流程 (SDLC)
│    需求 → 设计 → 开发 → 测试 → 部署 → 维护
│
├─ 工程环境流程 (Infrastructure / Environment Setup)
│    开发环境 / 测试环境 / 生产环境
│
├─ 开发者流程 (Developer Workflow)
│    Git流程 / PR流程 / 代码评审
│
├─ 运维流程 (Operations / DevOps)
│    发布 / 监控 / 回滚 / 事件响应
│
└─ 质量流程 (QA)
     自动化测试 / 代码规范 / 安全扫描 / 性能测试
```

## 三、从“五类流程”到“三层流程”的概括（总结）

三层流程不是替代五类流程，而是对五类流程的管理视角归纳：

1. Product Process（产品流程）
   - 主要承接：`SDLC`
   - 核心问题：做什么、为什么做、价值是否成立

2. Engineering Process（工程流程）
   - 主要承接：`Environment Setup` + `Developer Workflow` + `QA`（工程内建质量）
   - 核心问题：如何高质量、可持续地实现

3. Operations Process（运行流程）
   - 主要承接：`Operations / DevOps`（并吸收运行期质量与反馈）
   - 核心问题：上线后如何稳定运行并持续改进

因此，推荐的理解顺序是：

- 先用五类流程做落地分析（具体可执行）
- 再用三层流程做战略沟通（高层抽象）

## 一句话总结

软件开发流程可分为五类：`SDLC`、`Environment/Infrastructure`、`Developer Workflow`、`DevOps/Operations`、`QA/Quality`；而三层流程（产品/工程/运行）是对这五类流程的上层归纳与管理总结。

## 延伸阅读

- 进入 AI 可执行流程视角：`../02_workflow-analysis/ai-executable-workflow.md`
- 历史法方法框架：`./historical-method.md`
