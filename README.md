# IST Agent Guide

Public guide: https://github.com/lfx-sdu/ist-agent-guide

Use IST from your own AI assistant. This repository contains instructions for
connecting to the IST MCP service and for asking it to check weekly data, run a
transfer plan, explain results, and provide Excel reports. It contains **no
service code, customer data, endpoint credential, or token**.

**Pilot status:** The production IST MCP endpoint is live, and the server accepts
the production organization token. Connection and Excel attachment handling in
HTN's actual Claude and ChatGPT workspaces are still being verified. Workbook
upload from chat is currently unavailable because the server has no connected
attachment fetcher. The MCP URLs are in [CONNECT.md](CONNECT.md); an IST
administrator provides each environment's token privately.

## Start with your agent

Give your agent this public repository URL and say:

> Read https://github.com/lfx-sdu/ist-agent-guide/blob/main/AGENTS.md. Help me
> connect to the IST test endpoint in CONNECT.md. I will enter its token in the
> connector's secure credential field; do not ask me to paste it into chat.
> After connection, check the latest test data week and explain the tools.
> Use production only when I ask you to switch to it.

If your chatbot cannot open a GitHub URL, add [AGENTS.md](AGENTS.md) and
[CONNECT.md](CONNECT.md) as its reference documents. Agents that support a
`SKILL.md` package can use [skills/ist](skills/ist/SKILL.md). The guide does not
install the connector automatically; a workspace administrator may need to
enable it once.

- [Connect Claude or ChatGPT](CONNECT.md)
- [Tool glossary](TOOLS.md)
- [Example conversations](docs/EXAMPLES.md)
- [IST operating skill](skills/ist/SKILL.md)

**Shared-token access:** Test and production have different HTN organization
tokens. Within one environment, its token can be used by both chatbots to upload
data, change policy, and start runs. Keep each token in its connector's secret
setting, never in chat, a workbook, or this repository. IST records actions under
one organization identity, so it cannot identify which planner made a change.
