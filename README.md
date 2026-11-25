# Hang Ten Korea IST Agent Guide

Public guide: https://github.com/lfx-sdu/ist-agent-guide

Use IST from your own AI assistant. This repository contains instructions for
connecting to the IST MCP service and for asking it to check weekly data, run a
transfer plan, explain results, and provide Excel reports. It contains **no
service code, customer data, endpoint credential, or token**.

**Production access:** The Korea IST MCP endpoint is live at
`https://ist.sdu-alchemist.org/mcp` and accepts organization bearer tokens.
Connection and Excel attachment handling in
HTN's actual Claude and ChatGPT workspaces are still being verified. An agent
that can read an attached workbook and send HTTP can stage it through a one-use
upload ticket; other chat clients can offer the ticket's file-picker link.
Direct chatbot attachment handoff to MCP remains unverified. The MCP URLs are
in [CONNECT.md](CONNECT.md); an IST administrator provides each environment's
token privately.

## Connect to Korea production

If you received a production access file, add **IST Production** as a remote
Streamable HTTP server using the URL in the file. Enter the bearer token in the
connector's secure credential field. Share this public guide with your agent
and keep the private access file and token out of chat:

> Read https://github.com/lfx-sdu/ist-agent-guide/blob/main/AGENTS.md. Help me
> connect to IST Production at https://ist.sdu-alchemist.org/mcp. I will enter
> the token in the connector's secure credential settings. After connecting,
> run `get_data_status` and tell me the latest available data week.

For a practice run, use **IST Test** with its own URL and token from the
administrator. [Choose an environment](CONNECT.md#choose-the-environment)

If your chatbot cannot open a GitHub URL, add [AGENTS.md](AGENTS.md) and
[CONNECT.md](CONNECT.md) as its reference documents. Agents that support a
`SKILL.md` package can use [skills/ist](skills/ist/SKILL.md). The guide does not
install the connector automatically; a workspace administrator may need to
enable it once.

- [Connect Claude or ChatGPT](CONNECT.md)
- [Tool glossary](TOOLS.md)
- [Example conversations](docs/EXAMPLES.md)
- [IST operating skill](skills/ist/SKILL.md)

**Organization access:** Each token works in its issued environment and can be
revoked independently. It can be used by both chatbots to upload data, change
policy, and start runs. Keep the token in the connector's secure settings.
IST records actions under one HTN organization identity across its tokens.
