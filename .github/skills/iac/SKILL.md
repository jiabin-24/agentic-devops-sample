---
name: iac
description: 审查 IaC 和可观测性变更的正确性、安全性、部署安全和可维护性。
disable-model-invocation: true
argument-hint: "需要审查的拉取请求、差异、提交或文件"
---
根据 Issue、验收标准和仓库指令审查所提供的变更。

重点关注：

- 公网暴露和专用网络兼容性；
- 托管身份、机密处理和最小权限 RBAC；
- 破坏性操作或资源替换；
- Terraform 状态和 Provider 行为；
- Bicep 参数化；
- Prometheus 基数和告警噪音；
- Grafana 仪表盘可用性；
- 验证缺口；
- 回滚安全性。

仅报告有证据支持且位置精确的问题。
