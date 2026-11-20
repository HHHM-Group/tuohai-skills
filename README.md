# oceavant AI 获客 Skills

[oceavant AI 获客 Skill](skills/tuohai-prospecting/SKILL.md) 帮助 AI 助手分析已交付的线索：按任务核对交付，再根据公司资料和匹配评估解释哪些公司值得跟进。数据通过 oceavant AI 的 MCP 服务读取，可访问的范围由当前账号和 MCP Key 决定。

## 接入 AI 助手

1. 在支持 Agent Skills 的客户端中加载整个 [`skills/tuohai-prospecting/`](skills/tuohai-prospecting/) 目录，保留其中的 `references/`。如果客户端只能读取文档，可以让它在当前对话中读取 [SKILL.md](skills/tuohai-prospecting/SKILL.md)，需要工具参数时再读取[操作参考](skills/tuohai-prospecting/references/operations.md)。
2. 按 oceavant AI 提供的接入说明，在客户端添加远程 MCP 服务，传输方式选 **Streamable HTTP**，填写说明中的 HTTPS 服务地址。在客户端的凭据设置中填写个人 MCP Key；如果使用自定义请求头，名称为 `Authorization`，值为 `Bearer <你的 MCP Key>`。不要把真实 Key 发到聊天里或写进仓库。查询任务和线索只需要 `prospecting.read` 权限。
3. 请助手调用 `list_prospecting_tasks`，列出最近的一个获客任务。成功返回任务列表就说明 MCP 已接通，空列表也算成功。加载 Skill 不会自动完成账号授权。

## 试问一句

> 帮我看当前运行中的获客任务。按北京时间统计最近 7 天每个任务交付了多少条线索、涉及多少家公司。从中选出最多 3 家值得优先跟进的公司，逐家说明理由。资料不足的标为待核实，只查询，不发邮件。

你可以补充具体的筛选条件，比如只看制造商、优先有采购负责人的公司。更多工具和所需权限见[操作参考](skills/tuohai-prospecting/references/operations.md)。
