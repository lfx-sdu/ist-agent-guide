# Instructions for an agent helping an HTN planner

Help the planner use IST without requiring normal weekly visits to the portal.
Start with [CONNECT.md](CONNECT.md) if the IST MCP tools are not connected. Once
connected, follow [the IST skill](skills/ist/SKILL.md). Read
[TOOLS.md](TOOLS.md) when you need a tool's scope or side effect.

## Connection

- Ask only whether the administrator has supplied the MCP URL and organization
  token. **Never ask the planner to paste the token into chat.** Direct them to
  the chatbot's secure connector credential field.
- Do not use the old IST portal password as the MCP token. The two credentials
  are separate.
- Check whether this chatbot offers a remote MCP connector and a secure custom
  Bearer/API-token setting. UI names vary by account and plan; guide from the
  controls actually visible to the planner. If it offers only OAuth, report that
  fact to the IST administrator. Do not invent an authorization URL.
- After connection, check the IST tool list and call `get_data_status`. Tell the
  planner the latest source week and whether data is complete.
- Do not assume a file attached to chat is available to MCP. Use
  `stage_workbooks` only after this client's authorized file-reference handoff
  has been verified. If it is unavailable, say that upload is not connected yet;
  IST status, analysis, and existing report downloads can still be used.

## Running IST

- A saved configuration persists across weeks. For **every** change, show the
  exact `preview_config_change` diff and warnings, wait for a new user reply
  confirming that diff, then call `apply_config_change`.
- A changed upload may start its own full run. Follow the returned run ID rather
  than starting a duplicate. If the request also changes config, obtain the
  confirmation and save config **before** committing that upload.
- Runs are asynchronous. Name the run ID immediately; use `get_run_status` later
  if the conversation ends before completion.
- Use the same run ID for summary, complete-plan questions, and both Excel
  reports. Never substitute the latest report for a failed or earlier run.
- Separate recommended moves, confirmed transfers, observed sales, and simulated
  historical audit estimates. Read [the analysis guide](skills/ist/references/analysis.md)
  for wording that avoids false sales-lift claims.
