# IST Agent Guide

Public guide: https://github.com/lfx-sdu/ist-agent-guide

Use IST from your own AI assistant. This repository contains instructions for
connecting to the IST MCP service and for asking it to check weekly data, run a
transfer plan, explain results, and provide Excel reports. It contains **no
service code, customer data, endpoint credential, or token**.

**Pilot status:** The IST MCP service has passed isolated server tests. Connection
and Excel attachment handling in HTN's actual Claude and ChatGPT workspaces are
still being verified. An IST administrator will provide the MCP URL and shared
organization token through a secure channel when your workspace is ready.

## Start with your agent

Give your agent this public repository URL and say:

> Read https://github.com/lfx-sdu/ist-agent-guide/blob/main/AGENTS.md. Help me connect IST to this
> chatbot. Tell me where to enter the admin-provided MCP URL and token securely;
> do not ask me to paste the token into this conversation. After connection,
> check our latest IST data week and explain the available tools.

If your chatbot cannot open a GitHub URL, add [AGENTS.md](AGENTS.md) and
[CONNECT.md](CONNECT.md) as its reference documents. Agents that support a
`SKILL.md` package can use [skills/ist](skills/ist/SKILL.md). The guide does not
install the connector automatically; a workspace administrator may need to
enable it once.

- [Connect Claude or ChatGPT](CONNECT.md)
- [Tool glossary](TOOLS.md)
- [Example conversations](docs/EXAMPLES.md)
- [IST operating skill](skills/ist/SKILL.md)

**Shared-token access:** The HTN organization token can upload data, change
policy, and start runs in both chatbots. Keep it in each connector's secret
setting, never in a prompt or file. IST records actions under one organization
identity, so it cannot identify which planner made a change.
