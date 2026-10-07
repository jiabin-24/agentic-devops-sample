# MCP 接入说明

## 定位

MCP 让 Agent 能读取或操作 GitHub、Azure 和监控平台。仓库内的 Agent 未固定绑定某个 MCP Server，以避免环境中未安装相同工具时无法加载。启用 MCP 后，可根据组织实际工具清单为 Agent 增加最小范围的工具权限。

## 推荐映射

| Agent | GitHub 能力 | Azure 能力 | 默认权限 |
|---|---|---|---|
| Analyst | 读取 Issue、评论和标签 | 无 | 只读 |
| Architect | 读取 Issue 和仓库 | 查询资源、文档和架构信息 | 只读 |
| Developer | 读取 Issue/PR | 查询资源；部署需审批 | 仓库可写，云只读 |
| Tester | 读取 PR 和检查结果 | 查询监控与资源状态 | 只读 |
| Reviewer | 读取 PR、Diff 和检查结果 | 查询资源配置 | 只读 |
| SRE | 读取 Issue/PR | 查询 AKS、Monitor、Prometheus、Grafana | 只读 |
| Release | 读取 PR、检查和 Release | 查询部署状态 | 只读 |

## 安全基线

- 使用个人或工作负载身份，不在文件中保存 Token。
- 将生产环境写操作从默认工具集中移除。
- 为演示创建独立订阅、资源组或集群。
- 对部署、合并、发布和删除操作保留人工审批。
- 确认 MCP Server 的日志与组织审计策略一致。
- 不把 MCP 返回的敏感配置复制进 Prompt、Issue、日志或提交。

## 配置建议

不同 MCP Server 暴露的工具名称并不统一。配置时应先在 VS Code 中确认已发现的工具，再在对应 `.agent.md` 的 `tools` 列表中添加准确名称。不要假设服务器名称或使用不存在的通配符。

建议采用渐进方式：

1. 先保持 Agent 使用仓库内置的 `read`、`search`、`edit` 和 `execute` 工具。
2. 为 Analyst 和 Reviewer 增加 GitHub 只读工具。
3. 为 Architect 和 SRE 增加 Azure 资源及监控只读工具。
4. 在非生产环境验证参数、失败行为和审计日志。
5. 仅在确有需要时为 Developer 或 Release 增加经审批的写操作。

## 演示降级策略

MCP 不可用时：

- 将 Issue 或 PR 内容粘贴给相应 Agent；
- 使用本地样例参数执行静态验证；
- 把 Azure 查询与部署检查列为手工步骤；
- 明确标注未执行的检查，不生成成功形态的替代结果。
