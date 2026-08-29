# Remote Agent 与 Local Agent

同样是 Agent，跑在本机还是跑在云端，工作方式差很多。

```
Local Agent          Remote Agent
本机会话               云端会话
编辑器 / 终端           浏览器 / API
看得见文件             看不见你的桌面
```

## Local Agent

Cursor、Claude Code、Codex CLI 这类，Agent 就在你机器上。

它能直接读仓库、跑命令、改文件、开终端。上下文就是你当前工程，中断了也能就地接着看日志。

适合：**需要频繁交互、强依赖本机环境、要摸文件和调试** 的活。

代价也很清楚：占本机算力与会话；并行多开时机器会挤；人一走开，会话就容易断。

## Remote Agent

Cloud Agent、后台任务、托管会话这类，Agent 在远端跑。

你给出目标和工作区入口，它在云端环境里拉代码、改、测、提结果。人不必一直盯着编辑器。

适合：**可异步、可并行、边界清楚** 的任务——批量改动、长跑验证、多分支试错。

代价是另一套：环境要准备好；密钥与权限要单独配；本地临时状态带不过去；调试链路比本机长一截。

## 对照

| 维度 | Local | Remote |
| --- | --- | --- |
| 交互 | 强，边聊边改 | 弱，抛任务等结果 |
| 环境 | 本机即环境 | 要镜像/沙箱 |
| 并行 | 受本机限制 | 更容易开多路 |
| 中断恢复 | 当场看日志 | 依赖远端日志与重试 |
| 密钥与内网 | 天然在本机 | 要显式授权 |
| 人的姿态 | 坐在旁边 | 可以走开 |

一句话：Local 像同桌同事，Remote 像外包小队。

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

## 和需求管理、GitLab 打通吗

Remote Agent 的价值，很大程度看能不能接进 **需求 → 代码 → MR → 回写工单** 这条链。

### 原生集成（从工单直接开 Agent）

| 产品 | Jira | Linear | Azure Boards | Slack | GitHub Issues |
| --- | --- | --- | --- | --- | --- |
| Cursor Cloud | ✅ | ✅ | ❌ | ✅ | ✅ |
| Devin | ✅ | ✅ | ❌ | ✅ | ✅ |
| Copilot Agent | ✅ | ❌ | ✅ | ❌ | ✅ |
| Google Jules | ❌ | ❌ | ❌ | ❌ | ✅ |

Jira 里分配工单给 Cursor / Devin / Copilot，Agent 读描述和评论，干完把 PR 链回 ticket。Linear 上委派给 Cursor 或 Devin，可用 label 指定 repo 和分支。Azure Boards 能发给 Copilot，但代码仓仍须在 GitHub。

### GitLab 支持

| 产品 | GitLab 托管 | GitLab Issue 触发 |
| --- | --- | --- |
| Cursor Cloud | ✅（含 Self-Hosted） | ⚠️ 靠 MR 评论 / GitLab MCP |
| Devin | ✅（含 Self-Hosted） | ✅ Webhook + MR 协作 |
| Copilot Agent | ❌ | ❌ |
| Jules | ❌ | ❌ |

**GitLab 为主仓的团队**：Remote Agent 优先看 **Devin、Cursor Cloud**。Copilot Agent、Jules 基本不在选项里。

### 三种接法

```
1. 原生集成    Jira / Linear ←→ Agent ←→ GitHub / GitLab
2. MCP 扩展    Agent ←→ GitLab MCP / Jira MCP / 自建工具
3. 中间层      Webhook、CI、Actions 调 Agent API
```

按 stack 选：

| 你的环境 | 较顺的组合 |
| --- | --- |
| GitHub + Jira | Copilot Agent、Cursor、Devin |
| GitHub + Linear | Cursor、Devin |
| GitLab + Jira | Devin；或 Cursor（repo + Jira 分开接） |
| GitLab 全家桶 | Devin 优先；Cursor + GitLab MCP 次之 |

Local Agent 也能通过 MCP 读 Jira、改 GitLab Issue，但那是**你在编辑器里主动问**；Remote 的差异是**工单分配出去，Agent 自己跑完再回链**。

## 怎么选

```
任务边界清楚、可等结果 ──▶ Remote
需要盯着改、本机专属环境 ──▶ Local
两者都要                   ──▶ Local 定方向，Remote 批量落地
```

日常里更稳的组合往往是：

1. **Local** 把问题想清楚：计划、边界、验收标准。
2. **Remote** 去跑可拆分的执行：多 PR、长测试、重复劳动。
3. 再回到 **Local** 做审查与合并决策。

不是谁取代谁，是把「必须人在场」和「可以人走开」拆开。

## 结语

Agent 从助手走到团队之后，下一个分叉不是模型，而是**它住在哪里**。

住在本机，优势是贴身；住在远端，优势是可规模化、可接工单。

把任务按「要不要人盯着」切开，再按「代码在 GitHub 还是 GitLab、需求在 Jira 还是 Linear」选对集成，Local 与 Remote 才能同时用起来，而不是互相抢同一个会话。
