---
name: ist-operator
description: Help an HTN planner use the connected IST MCP service for weekly data checks, confirmed policy edits, asynchronous transfer runs, analysis, and exact-run Excel reports. Use only when IST is the requested service.
---

# IST operator

If IST is not connected, use [CONNECT.md](../../CONNECT.md) to guide setup. Never
ask the user to paste the organization token into chat. The public guide repo
contains instructions, not credentials or service code.

Before an upload, policy save, or run, name the connected environment. A test
action does not itself authorize the same action in production; wait for the
user's production request.

## Weekly work

1. Call `get_data_status` before stating which weekly inputs are current.
2. When the user attaches Excel files, call `stage_workbooks` only if this
   chatbot's authorized file-reference handoff has been verified. Otherwise
   call `create_upload_ticket`. If the agent can read the attached files and
   send HTTP, inspect filenames, sizes, Excel format, and sheet weeks locally,
   then POST the original bytes as multipart files under the ticket's field
   names. Report filename/sheet-week discrepancies. Keep the URL out of chat
   when code can use it. If code cannot send files, share the one-use file-picker
   link with the planner and wait for them to select files. Never send workbook
   bytes or base64 through a model/tool text argument.
3. Show `validate_upload` weeks, changed/missing sources, errors, and warnings.
   Resolve a destructive replace or week remap with the user before committing.
4. If the same request includes a policy change, preview it and get the user's
   separate confirmation **before** `commit_upload`: a changed upload can start
   a full run using the policy saved at that moment.
5. Follow the import with `get_import_job`. When it returns `forecast_run.run_id`,
   follow that run; do not start a duplicate.

## Policy changes

Read the current revision with `get_config`; use `search_config_options` for
store/style/season names. Do not guess or hard-code current identifiers.

For **every** change, call `preview_config_change`, show its exact before/after
diff and warnings, and wait for a new user reply confirming **that diff**. Only
then call `apply_config_change` with a reference to that reply. If the proposal
expires or the revision changes, preview again and obtain fresh confirmation.
The saved policy persists across future weeks until another confirmed change.

## Runs and reports

- For a confirmed season exclusion or policy dial change, use `reallocate` if
  a completed full run covers the current weekly inputs. If weekly sales or
  order inputs changed, follow the upload's automatic `full` run; start `full`
  when no usable full-run cache exists.
- `start_run` returns an ID promptly. Reuse its idempotency key on retry. A run
  can take minutes; give the user the ID and use `get_run_status` later.
- On completion, use the **same** ID for `get_run_results`, `query_plan`, and
  both `download_report` calls (`forward` and `audit`). Reissue an expired
  download link when needed.
- On failure, name the failed stage and error. Do not label an earlier report
  as this run's result.
- Include the run ID, source week, generated time, and configuration revision
  in material analysis when IST provides them. Read
  [analysis.md](references/analysis.md) before answering sales-outcome questions.

Use [TOOLS.md](../../TOOLS.md) when a tool's side effect or scope is unclear.
