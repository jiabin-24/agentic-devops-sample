# AKS 监控告警 Agentic DevOps 演示指南

## 演示目标

在 30 至 45 分钟内展示 GitHub Copilot 如何在统一仓库规范下，让多个专职 Agent 协作完成需求分析、架构、实现、测试、审查和发布准备。

建议场景：**Enable AKS Control Plane Monitoring**。

## 演示前准备

1. 在 VS Code 中打开仓库并确认 GitHub Copilot Chat 可用。
2. 确认 Agent 选择器能够看到仓库中的自定义 Agent。
3. 确认输入 `/` 后能够看到仓库 Skill。
4. 如演示 MCP，提前完成 GitHub 和 Azure 身份验证，并使用只读或演示专用环境。
5. 准备一个非生产 AKS 集群；不要在现场首次执行生产变更。

## 步骤 1：创建 Issue

使用 **启用 AKS 监控** 模板，填写：

- 当前状态与目标结果；
- 集群范围和环境；
- 控制平面、工作负载和依赖信号；
- 身份、网络和合规约束；
- 可验证的完成条件。

强调：Issue 是后续所有 Agent 的事实来源，不完整的信息不应由 Agent 静默猜测。

## 步骤 2：需求分析

运行：

```text
/issue <Issue 编号、Issue URL 或粘贴的 Issue 内容>
```

展示输出中的：

- 范围内 / 范围外；
- 需求 / 假设；
- “假如/当/那么”格式的验收标准；
- 风险和实施清单。

遇到开放问题时，先由需求方确认，再进入架构阶段。

## 步骤 3：架构设计

运行：

```text
/aks <已批准的需求、约束条件和目标环境>
```

重点讨论：

- AKS 到 Azure Monitor 的遥测流；
- Managed Prometheus 与 Managed Grafana；
- Managed Identity、Azure RBAC 与 Kubernetes RBAC；
- 私有网络与 DNS；
- 成本、配额、数据保留和指标基数；
- 部署与回滚边界。

## 步骤 4：实现

运行：

```text
/implement-change <已批准的 Issue、验收标准和架构>
```

期望产出可包括：

- `bicep/` 下的 Azure 资源定义；
- `terraform/` 下的等价或主实现；
- `alerts/` 下的 Prometheus 规则；
- `dashboards/` 下的 Grafana Dashboard；
- `docs/` 下的操作与回滚说明。

演示中默认只生成和验证文件，不实际部署。

## 步骤 5：测试

运行：

```text
/validate-change <需要验证的变更集和验收标准>
```

要求 Tester 输出验收标准追踪矩阵，并明确区分：

- 已执行且通过；
- 已执行但失败；
- 未执行的部署依赖检查；
- 必须人工完成的验证。

## 步骤 6：审查

运行：

```text
/iac <需要审查的拉取请求、差异、提交或文件>
```

审查 Agent 应保持只读，重点检查：

- 公网暴露；
- Secret 或固定环境标识；
- Managed Identity 与最小权限；
- 资源替换或销毁风险；
- Prometheus 高基数与告警噪音；
- 缺失的验证与回滚步骤。

## 步骤 7：SRE 就绪

选择 `站点可靠性工程师` Agent，要求补齐：

- RED/USE 视图；
- SLI/SLO；
- 告警严重级别、Owner 和 Runbook；
- Missing data 和恢复行为；
- 上线后观察窗口与升级路径；
- RCA 模板所需的证据清单。

## 步骤 8：拉取请求与发布准备

按 PR 模板填写需求追踪、测试结果、运维影响和回滚。随后运行：

```text
/prepare-release <Issue、拉取请求、版本和测试证据>
```

发布 Agent 必须在证据缺失时返回 **阻塞**，而不是生成看似成功的发布结论。

## 建议的讲解重点

- Agent 并不是角色名称加提示词；关键是上下文隔离、工具权限和明确的输出契约。
- Repository Instructions 约束所有阶段，避免角色之间标准漂移。
- Skill 负责稳定复用一个工作流，Agent 负责稳定扮演一个角色。
- MCP 提供外部事实和执行能力，但审批、权限和审计边界仍然不可缺失。
- 最终价值是可追踪性：每项变更都能回到 Issue、验收标准和验证证据。
