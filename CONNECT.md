# Connect IST to Claude or ChatGPT

**Current status:** This is a pilot guide. The production MCP service accepts a
direct Bearer token, but HTN's actual Claude and ChatGPT connector settings and
attachment handoff still need account-level verification. Your IST administrator
supplies the organization tokens privately.

**Workbook upload:** The deployed MCP offers `create_upload_ticket`. A coding
agent that can read the attached file and send HTTP can use its one-use URL to
POST the original Excel bytes. If that runtime cannot send files, the agent can
offer the same URL as a short-lived file-picker page; the planner selects the
files there and returns to chat. The agent then calls `validate_upload`, shows
the findings, and commits only the requested clean upload. Direct attachment
handoff through `stage_workbooks` still needs a chatbot-specific fetcher.

## Choose the environment

| Environment | MCP endpoint |
|---|---|
| Test (isolated data) | `https://ist-sea-dev-agtest-backend.grayisland-a9bf3b94.southeastasia.azurecontainerapps.io/mcp` |
| Production (live IST data) | `https://ist-sea-dev-backend.grayisland-a9bf3b94.southeastasia.azurecontainerapps.io/mcp` |

Start with test. The IST administrator supplies a **different reusable token for
each environment** through a private channel. Each token grants IST read,
upload, config-change, and run capabilities in its own environment. Everyone
using one token shares one audit identity; the administrator can revoke or
rotate it for both chatbots. No token is in this public repository.

## Connect in your chatbot

The exact menu names depend on the chatbot and workspace plan. Ask your agent to
guide you through the controls it can see.

1. Have a workspace administrator enable a **remote MCP connector** for IST.
2. Enter the selected environment's URL from the table above. If your chatbot
   supports two connections, name them “IST Test” and “IST Production”.
3. If the connector offers a secure **Bearer token** or **API token** field,
   enter that environment's token there once. Never paste it into a chat,
   instruction, GitHub issue, or workbook.
4. If the connector offers **OAuth only**, stop and tell the IST administrator.
   The shared token needs an IST OAuth connection bridge for that client; the
   old portal login is not that bridge.
5. Ask your agent: “Check the latest IST data week.” It should call
   `get_data_status`, name the environment, week, and freshness, and show no
   token value. Switch to production only when you explicitly ask for it.
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

In **both** HTN workspaces, complete these checks in test first. For production,
use its own URL and token, repeat the tool and data-status checks, and perform
writes only when the planner requests them:

- The shared token connects without being exposed in chat.
- `get_data_status` returns the expected latest week.
- `get_run_results` and `download_report` return the requested run's forward
  and audit files, including after a newer run has completed.
- An Excel attached in chat can reach `stage_workbooks` through an authorized
  file reference, or the agent can read and POST it through a one-use upload
  ticket. If neither works, the file-picker link is the fallback. Record which
  path works in each chatbot account.
- A long run can be followed by `get_run_status` after leaving and returning to
  the conversation. Note whether the chatbot can send an automatic completion
  message; on-demand status works regardless.

The connector is not fully accepted for direct chat attachment upload until
both chatbot accounts pass that check. Do not send the raw token back to the
IST team in a chat transcript.
