# 从助手到团队：Agent 在工作中的角色演变

找到「如何尽可能使用 AI 替代日常工作」这个问题的答案之前，我先来回顾下在我的工作经历中，agent 在不同阶段充当了什么角色。

助手： 24年初 openAI 推出 chatGPT，可以通过问答形式帮助用户。当时团队内特别震惊，机器人竟然可以和人一样，不需要通过预先编程，对不特定的输入都能进行贴切的回答。
不过此时对工作交付帮助有限，能够以答案形式产出代码片段。此时代码可能与工作代码库上下文无关，基本不能直接使用。

码农：
同年 cursor 和 github copilot 越来越多的进入到大众视野，能够快速补全代码，出现“Tab 工程师”戏称。
之后 cursor 能够基于整个工程作为模型输入，继而进行**特定目标**的代码编写任务。agent 开始能够基于工程理解，进行编程任务。
后续 cursor 继续推出 custom agent， Plan mode， agent review 等一系列丰富的功能，极大的方便了工作和提高效率。

工程师：
MCP 协议， Skill， A2A 开始出现，agent 开始入侵日常的工作，可以使用 mcp 来阅读文档，生成设计图、流程图，甚至搜索代码和部署。此时 agent 已经能独立完成工作，但是首先上下文限制，往往需要用户中途介入。

团队：
25年初在公司开始使用 claude-code， 与 cursor 极大的不同是以 Terminal UI 的形式代替了代码编辑器。极简的 UI 和交互，丰富的命令和高效的 sonnet 模型让大家快速替换掉了手中的 cursor。
因为 Terminal agent 弱化了代码文件在工作中的存在感，只需要通过不断地对话来完成任务。此后“Vibe coding“愈发流行，社区对 agent orchestraction 的热度也愈发火热。
在使用一个 terminal session 已经不够快速满足工作需要，往往需要开启多个session，此时越来越多的agent 管理和 agent orchestraction 工具出现。
到目前26年为止，现在 agent 的工具集成越来越丰富，几乎通过一句 ”Hi, could you help finish this task for me?“， agent 几乎可以持续的完成代码编写，提出 MR， 测试环境部署，端到端的验证。之后等待用户的审查和代码合并。
对于多 session 的管理，和 agent 上下文工程配置可以参考目录里的文章。
