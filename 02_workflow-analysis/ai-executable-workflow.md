# AI 可执行的软件工程流程（AI-Executable Software Engineering Workflow）

## 1. 文档目的

本文用于定义“AI 协同开发”下的软件工程流程模型，重点回答：

1. 流程如何从“人执行”转向“人编排、AI执行”
2. 不同流程节点如何分配给 Human、AI Agent、System
3. 如何把流程结构化为可复用、可治理的工程资产

## 2. 核心定义

### 2.1 流程范式演进

传统范式：

`Human Workflow`

协同范式：

`Human + AI Workflow`

目标范式：

`Human Orchestrated AI Workflow`

即：人类负责目标、边界与决策；AI负责可结构化执行；系统负责自动化编排与验证。

### 2.2 AI 流程设计三问

每个流程步骤必须明确三项：

- `Who`：谁执行（Human / AI Agent / System）
- `How`：如何执行（步骤、规则、约束、验收）
- `Which AI`：使用哪类 AI（LLM / Code AI / Agent / Monitoring AI）

## 3. 总体模型

### 3.1 从三层流程到 AI 可执行流程

传统三层流程：

- Product Process
- Engineering Process
- Operations Process

AI 协同后，应改写为“可执行流程模型”：

`AI-assisted Engineering Process`

关键变化：

- 从“描述流程”转为“可分配任务节点”
- 从“人工串行执行”转为“人机并行协同执行”
- 从“经验驱动”转为“规则与资产驱动”

### 3.2 三层执行架构

```text
AI Software Engineering Workflow
│
├─ Human Layer
│    决策 / 架构 / 评审 / 风险兜底
│
├─ AI Layer
│    设计生成 / 代码生成 / 文档生成 / 测试生成
│
└─ System Layer
     CI/CD / 自动测试 / 自动部署 / 监控告警
```

## 4. 角色与职责

### 4.1 Human（人类）

职责：

- 业务目标定义
- 架构与关键方案决策
- 验收标准定义
- 风险裁决与最终发布决策

### 4.2 AI Agent（智能代理）

职责：

- 需求拆解与方案草拟
- 代码与测试生成
- 重构与文档补全
- 按约束执行工具链任务

### 4.3 System（系统自动化）

职责：

- CI/CD 执行
- 质量门禁与流水线控制
- 环境部署与回滚
- 运行期监控与告警联动

## 5. SDLC 的 AI 协同分配（示例）

传统 SDLC：

`需求 -> 设计 -> 开发 -> 测试 -> 部署 -> 维护`

AI 协同分配：

1. 需求：`Human + AI`
2. 设计：`Human 主导 + AI 辅助`
3. 开发：`AI 主导 + Human Review`
4. 测试：`AI 主导 + System 执行`
5. 部署：`System 自动化 + Human 审批`
6. 维护：`AI 分析 + Human 决策`

## 6. 标准流程模板（可执行）

每个流程节点统一使用 `Input-Task-Output` 模板：

1. Input（输入）
   - 需求、约束、上下文、验收标准
2. Task（任务）
   - AI 执行动作、工具调用、执行步骤
3. Output（输出）
   - 代码、测试、文档、报告、状态
4. Gate（门禁）
   - 质量检查、策略校验、人工审批

模板示例：

```text
Input: 用户故事 + 验收标准
Task: AI 生成 API 设计与实现，并补齐测试
Output: API 代码 + 单元测试 + 接口文档
Gate: CI 通过 + Code Review 通过
```

## 7. AI 能力映射

| 能力类型 | 典型作用 | 适配环节 |
| --- | --- | --- |
| LLM | 需求理解、设计草案、文档生成 | 需求/设计 |
| Code AI | 代码补全、重构、测试生成 | 开发/测试 |
| Agent | 工具编排、跨文件修改、命令执行 | 开发/集成 |
| CI/CD | 构建、测试、部署自动执行 | 构建/发布 |
| Monitoring AI | 日志分析、异常定位、容量建议 | 运维/维护 |

## 8. 接管模式

### 8.1 Assistant 模式

`Human -> AI -> Human`

适用：高不确定、需高频判断的任务。

### 8.2 Pair 模式

`Human <-> AI`

适用：编码与设计迭代、快速协作场景。

### 8.3 Agent 模式

`Human -> AI Agent -> System`

适用：边界清晰、可自动化、可验收的端到端任务。

## 9. 治理要求与风险控制

### 9.1 必备治理点

- 任务边界：定义 AI 可改动范围
- 权限控制：最小权限原则
- 可追溯性：日志、变更记录、审计链路
- 验收机制：自动门禁 + 人工兜底

### 9.2 主要风险

- 错误自动化扩大影响面
- 上下文不完整导致错误实现
- 工具调用越权或误操作

对应控制：

- 沙箱与分级权限
- 强制测试与静态检查
- 关键步骤人工审批

## 10. 落地路径（建议）

1. 阶段一：Assistant 化
   - 从需求整理、文档生成、测试草稿开始
2. 阶段二：Pair 化
   - 引入人机结对编码与评审协同
3. 阶段三：Agent 化
   - 将标准任务封装为可执行 Agent 工作流
4. 阶段四：系统化
   - 打通 CI/CD、监控与策略治理，形成工程操作系统

## 11. 结论

AI 协同开发的本质不是“AI 能写代码”，而是流程执行模式的升级：

`Human designs workflow, AI executes workflow, System guarantees workflow.`

因此，关键不在“是否使用 AI”，而在“流程是否结构化、可执行、可治理”。
