# 面向 AKS 可观测性的 Agentic DevOps

这是一个面向客户演示与技术培训的 Agentic DevOps 模板项目，以 **AKS 监控告警接入** 为场景，展示从需求到发布的完整协作链路：

```text
Issue
  -> Repository Instructions
  -> Analyst / Architect
  -> Developer
  -> Tester / Reviewer / SRE
  -> Pull Request
  -> Release
```

项目使用 VS Code 和 GitHub Copilot 原生支持的仓库定制能力，不要求额外开发 Agent 平台。

## 项目结构

```text
.
|-- .github/
|   |-- copilot-instructions.md
|   |-- agents/
|   |   |-- analyst.agent.md
|   |   |-- architect.agent.md
|   |   |-- developer.agent.md
|   |   |-- tester.agent.md
|   |   |-- reviewer.agent.md
|   |   |-- sre.agent.md
|   |   `-- release.agent.md
|   |-- prompts/
|   |   |-- analyze-issue.prompt.md
|   |   |-- design-aks-monitoring.prompt.md
|   |   |-- implement-change.prompt.md
|   |   |-- validate-change.prompt.md
|   |   |-- review-iac.prompt.md
|   |   `-- prepare-release.prompt.md
|   |-- ISSUE_TEMPLATE/
|   `-- pull_request_template.md
|-- alerts/
|-- bicep/
|-- dashboards/
|-- docs/
`-- terraform/
```

> VS Code 当前约定是 `.github/agents/*.agent.md` 和 `.github/prompts/*.prompt.md`。仅使用 `analyst.md` 或根目录 `.prompts/` 可能无法被正确发现。

## 角色分工

| Agent 标识 | 主要职责 | 是否修改文件 |
|---|---|---|
| `需求分析师` | 需求、假设、验收标准、风险 | 否 |
| `解决方案架构师` | Azure/AKS 架构、身份、网络与遥测流 | 否 |
| `开发工程师` | IaC、告警、仪表盘和文档实现 | 是 |
| `测试工程师` | 验收标准追踪与自动/人工验证 | 是 |
| `代码审查员` | 正确性、安全性、部署风险审查 | 否 |
| `站点可靠性工程师` | SLO、仪表盘、告警、运行手册 | 是 |
| `发布经理` | 发布就绪检查、部署/回滚计划、发布说明 | 是 |

## 快速演示

1. 使用 **启用 AKS 监控** Issue 模板创建需求。
2. 在聊天中运行 `/分析-Issue`，或选择 `需求分析师` Agent 分析 Issue。
3. 运行 `/设计-AKS-监控` 形成架构方案并关闭开放决策。
4. 运行 `/实施变更` 生成或修改 IaC、告警和仪表盘。
5. 运行 `/验证变更`，生成验收追踪矩阵并执行安全的本地验证。
6. 运行 `/审查-IaC`，以只读方式检查安全性与部署风险。
7. 由 `站点可靠性工程师` Agent 补齐运行手册和运维就绪内容。
8. 创建拉取请求，并运行 `/准备发布` 生成发布与回滚材料。

完整讲解脚本见 [演示指南](docs/demo-guide.md)。

## MCP 集成

Agent 和 Prompt 可以在未配置 MCP 时完成仓库内分析与修改。要演示对外部系统的读取或操作，可按环境接入：

- GitHub MCP：读取 Issue、PR、检查状态与评论；
- Azure MCP：查询 AKS、Azure Monitor、Managed Prometheus 和 Managed Grafana；
- Terraform 或 Azure CLI 工具：执行静态验证、计划和经审批的部署。

MCP 权限应遵循最小权限。演示默认只读；任何会修改云资源、合并 PR 或发布版本的操作都必须获得明确批准。详见 [MCP 接入说明](docs/mcp-integration.md)。

## 使用原则

- Repository Instructions 是始终生效的项目规则。
- Prompt 是单一、可复用的任务入口。
- Custom Agent 用于角色隔离、工具约束和独立上下文。
- MCP 用于连接 GitHub、Azure 等外部系统，而不是替代仓库规范。
- 所有结果都必须从 Issue 验收标准追踪到实现、验证和发布说明。
