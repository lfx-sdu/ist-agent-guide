# IST MCP tool glossary

The MCP server enforces these actions. Your agent should explain the result of
each write and use the same run ID throughout a conversation. A shared HTN
organization token carries all IST capabilities; configuration changes still
require a separate, explicit user confirmation in chat.

| Tool | What it does | Effect |
|---|---|---|
| `get_data_status` | Shows uploaded weeks, source freshness, and completeness. | Read |
| `stage_workbooks` | Stages authorized Excel file references. The deployed MCP has no attachment fetcher yet, so chatbot uploads are currently unavailable. | Stages files when connected |
| `validate_upload` | Rechecks a staged upload and reports weeks, changes, errors, and warnings. | Read/check |
| `commit_upload` | Saves validated data. A changed upload may automatically start a full or reallocation run. | Writes data; may start run |
| `get_import_job` | Follows a background import and its forecast `run_id`. | Read |
| `get_config` | Reads the persistent policy revision and counts of available choices. | Read |
| `search_config_options` | Finds current store, style, season, channel, or region choices. | Read |
| `preview_config_change` | Shows an exact proposed setting/exclusion diff and warnings. | Preview only |
| `apply_config_change` | Saves a proposal after the user confirms that exact diff. Settings persist until changed again. | Writes policy |
| `start_run` | Queues a `full` forecast or cached-forecast `reallocate` and returns a run ID promptly. | Starts run |
| `get_run_status` | Reports the stage or error for one run ID. | Read |
| `list_runs` | Lists recent run IDs and states. | Read |
| `get_run_results` | Reads forward and historical-audit summaries for one completed run. | Read |
| `query_plan` | Searches the complete selected transfer lines beyond the dashboard preview. | Read |
| `download_report` | Issues a short-lived, single-use forward or audit Excel link for one run ID. | Read/download |

## Tool choices that matter

- An uploaded weekly sales/order change may start its own full run. Use the
  `run_id` in `get_import_job`; avoid a second `start_run` call.
- Save a requested policy change **before** committing an upload that will
  automatically run, so that run captures the intended revision.
- Excluding a season or changing a policy dial affects allocation alone. After
  a completed full run for the current weekly inputs, use `reallocate` to apply
  that change. It reuses the cached forecast.
- Changed weekly sales or order inputs need a `full` run to rebuild the
  forecast and allocation. Follow the upload's automatic run when it starts
  one. If no usable full-run cache exists, start `full` before reallocation.
- `get_run_results` is a summary. Use `query_plan` for a store/style question
  about the complete selected plan. The dashboard preview alone is incomplete.
- A forward result recommends moves for an unobserved week. Historical audit
  estimates are simulated; neither is a measured sales increase from that plan.
