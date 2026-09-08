# Remote Agent 与 Local Agent

## Local Agent

Cursor、Claude Code、Codex CLI 这类，Agent 在电脑上，需要开着电脑不停对话。

它能直接读仓库、跑命令、改文件、开终端。上下文就是当前文件夹，中断了也能看日志。

**需要频繁交互、强依赖本机环境；占电脑资源；依赖电脑保持开机。**

## Remote Agent

Cloud Agent、后台任务、托管会话这类，Agent 在服务端跑。

它在云端沙箱里拉代码、改、测、提结果。不用一直盯着电脑或者agent 交互界面。

**异步并行上限更高；**批量改动、长跑验证、多分支试错。

环境要准备好；密钥与权限要单独配；自动化测试更复杂；要求工具可用性更强。

## Remote 能做什么

2025 年以后，主流 Remote Agent 大多收敛到同一套能力：

```
工单 / Issue / Slack
        │
        ▼
  云端 VM clone 仓库
        │
   改代码 → 跑测试 → 迭代
        │
        ▼
   开 PR/MR + 截图/录屏
        │
        ▼
   链接回写到工单
```

常见能力包括：

- **异步执行**：任务在云端跑，人不必守着编辑器
- **仓库闭环**：拉分支、改多文件、提 PR/MR
- **验证**：装依赖、跑测试、看 CI，失败了自己再试
- **并行**：多个 issue 同时开（Cursor Cloud、Devin MultiDevin、Jules 并发 task）
- **产物**：PR 之外还有截图、录屏、日志
- **扩展**：MCP 接 Sentry、Datadog、自建 API；Secrets 配密钥、Tailscale 进内网

弱项也一致：需求写不清楚、验收标准模糊、强依赖本机独有环境的任务，Remote 都容易翻车。

Remote Agent 的价值，很大程度看能不能接进 **需求 → 代码 → MR → 回写工单** 这条链。


| 产品            | Jira | Linear | Azure Boards | Slack | GitHub Issues |
| ------------- | ---- | ------ | ------------ | ----- | ------------- |
| Cursor Cloud  | ✅    | ✅      | ❌            | ✅     | ✅             |
| Devin         | ✅    | ✅      | ❌            | ✅     | ✅             |
| Copilot Agent | ✅    | ❌      | ✅            | ❌     | ✅             |
| Google Jules  | ❌    | ❌      | ❌            | ❌     | ✅             |


Jira 里分配工单给 Cursor / Devin / Copilot，Agent 读描述和评论，干完把 PR 链回 ticket。Linear 上委派给 Cursor 或 Devin，可用 label 指定 repo 和分支。Azure Boards 能发给 Copilot，但代码仓仍须在 GitHub。

### GitLab 支持


| 产品            | GitLab 托管        | GitLab Issue 触发         |
| ------------- | ---------------- | ----------------------- |
| Cursor Cloud  | ✅（含 Self-Hosted） | ⚠️ 靠 MR 评论 / GitLab MCP |
| Devin         | ✅（含 Self-Hosted） | ✅ Webhook + MR 协作       |
| Copilot Agent | ❌                | ❌                       |
| Jules         | ❌                | ❌                       |


### 三种接法

```
1. 原生集成    Jira / Linear ←→ Agent ←→ GitHub / GitLab
2. MCP 扩展    Agent ←→ GitLab MCP / Jira MCP / 自建工具
3. 中间层      Webhook、CI、Actions 调 Agent API
```

按 stack 选：


| 你的环境            | 较顺的组合                           |
| --------------- | ------------------------------- |
| GitHub + Jira   | Copilot Agent、Cursor、Devin      |
| GitHub + Linear | Cursor、Devin                    |
| GitLab + Jira   | Devin；或 Cursor（repo + Jira 分开接） |
| GitLab 全家桶      | Devin 优先；Cursor + GitLab MCP 次之 |


Local Agent 也能通过 MCP 读 Jira、改 GitLab Issue，但那是**你在编辑器里主动问**；Remote 的差异是**工单分配出去，Agent 自己跑完再回链**。

## 结语

Remote Agent 是未来大方向，电脑关机人睡觉了，Agent还能干活，第二天早上人来 review 成果。但是要求大环境对 Remote Agent 友好，提供足够多的公司资源。人能把积累的知识传递给 Remote Agent。

