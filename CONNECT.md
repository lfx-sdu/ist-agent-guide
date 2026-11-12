# Connect IST to Claude or ChatGPT

**Current status:** This is a pilot guide. The service works with a direct
Bearer token in isolated MCP tests, but HTN's actual Claude and ChatGPT connector
settings and attachment handoff still need account-level verification. Your IST
administrator supplies the production MCP URL and organization token separately.

## What the administrator gives you

1. An IST MCP endpoint URL. It ends in `/mcp`.
2. One reusable HTN organization token, sent through a secure password-manager
   share or another approved private channel.

Neither item is in this public repository. The token grants IST read, upload,
config-change, and run capabilities. Everyone using it shares one audit identity;
the administrator can revoke or rotate it for both chatbots.

## Connect in your chatbot

The exact menu names depend on the chatbot and workspace plan. Ask your agent to
guide you through the controls it can see.

1. Have a workspace administrator enable a **remote MCP connector** for IST.
2. Enter the administrator-provided URL as the server endpoint.
3. If the connector offers a secure **Bearer token** or **API token** field,
   enter the IST organization token there once. Never paste it into a chat,
   instruction, GitHub issue, or workbook.
4. If the connector offers **OAuth only**, stop and tell the IST administrator.
   The shared token needs an IST OAuth connection bridge for that client; the
   old portal login is not that bridge.
5. Ask your agent: “Check the latest IST data week.” It should call
   `get_data_status`, name the week and freshness, and show no token value.
6. Ask for a completed run by its run ID and request both forward and audit
   Excel reports. Download links are short-lived and single-use; the agent can
   issue a new link when needed.

### Claude

Give Claude this repository URL or add [AGENTS.md](AGENTS.md) and the
[IST skill](skills/ist/SKILL.md) to its workspace instructions where supported.
Ask it to help configure the remote MCP connector using the steps above. Record
whether its account can pass an attached Excel file to an MCP tool as an
authorized file reference.

### ChatGPT

Give ChatGPT this repository URL or add [AGENTS.md](AGENTS.md) and the
[IST skill](skills/ist/SKILL.md) to its agent/app instructions where supported.
Ask it to help configure the remote MCP connector using the steps above. Record
whether its account can pass an attached Excel file to an MCP tool as an
authorized file reference.

## Connection check for the administrator

In **both** HTN workspaces, verify:

- The shared token connects without being exposed in chat.
- `get_data_status` returns the expected latest week.
- `get_run_results` and `download_report` return the requested run's forward
  and audit files, including after a newer run has completed.
- An Excel attached in chat can reach `stage_workbooks` through an authorized
  file reference. If not, tell the IST team which file reference or connected
  file store the chatbot offers; the server needs an adapter for it.
- A long run can be followed by `get_run_status` after leaving and returning to
  the conversation. Note whether the chatbot can send an automatic completion
  message; on-demand status works regardless.

The connector is not fully accepted for weekly upload until both chatbot
accounts pass the attachment check. Do not send the raw token back to the IST
team in a chat transcript.
