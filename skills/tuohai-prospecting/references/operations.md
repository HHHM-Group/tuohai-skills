# 操作参考

| 目标 | MCP 工具与数据 |
|---|---|
| 选择任务 | `list_prospecting_tasks`，分页游标原样传回。 |
| 核对交付 | `get_prospecting_summary` 和 `list_delivered_leads` 共享 `startDate`（含）、`endDateExclusive`（不含）、`timeZone`（IANA 时区）、可选 `taskId` 和 `cycleId`。每页使用同一组过滤参数。`contactsWithVerifiedEmail` 始终是服务端汇总，`prospecting.contact.read` 只控制明细里的邮箱地址。 |
| 比较公司 | `get_task_company_detail` 输入用户自己的 `taskId`、`companyId` 和可选 `cycleId`。`companyType` 是 CRM 记录的公司类型；为空时该类型未知。Lead Claim 表示交付经过，评估为空时写明没有结果。 |
| 获客草稿 | `save_prospecting_draft` 用新 UUID 和 `expectedRevision=0` 创建；更新须用当前版本。`get_prospecting_draft` 查询现有版本。 |
| 邮件草稿 | `save_email_template_draft`、`get_email_template_draft` 的版本规则和获客草稿一致。`list_email_templates`、`get_email_template` 读取已发布内容；`get_email_template_review` 读取供应商审核状态。 |
| 邮件预览 | `preview_email_template` 接受一个 `draftId`（仅本人草稿，需 `template.draft.write`）或 `versionId`（本租户版本，需 `template.read`）。返回 `subject`、`bodyText`、用于隔离展示的 `bodyHtml` 和 `missingVariables`。预览中的链接和图片地址不可用。 |
| 发布与引用 | `publish_email_template_version` 发布指定的草稿版本；相同请求返回原版本。`bind_email_template_version` 将指定的正式版本引用保存到获客草稿，不更新已排程邮件。 |
| 启用与管理 | `prepare_prospecting_start` 返回签入拓海后可审核的数据入口；`get_prospecting_start_status` 需要 `draftId`、`revision`。`pause_prospecting_task`、`resume_prospecting_task` 操作自己的任务；有触达配置的任务回拓海恢复。 |
| 企业深挖 | `prepare_company_deep_search` 需要 `taskId`、`companyId`。`get_company_deep_search_receipt` 另需预览产生的 `requestId`；拿到 `operationId` 后用 `get_company_deep_search_status` 查询。 |
| 群发试点 | 如果已开放 `prepare_outreach`，只提交用户确认的关系 ID 和不可变模板版本。`start_outreach` 和 `get_outreach_status` 使用同一个 `grantId`，不能替用户在 Web 签发授权。额度或期限过期时停止请求。 |

成功读取草稿后只报告已保存。`CONFIRMATION_REQUIRED` 仍在等待用户；邮件的 Provider 接受不证明已经投递。工具列表中没有独立测试信时，说明当前不可由 MCP 发测试信；不要用正式群发代替。
