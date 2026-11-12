# Example IST conversations

These examples show the intended decisions. Your agent uses the connected IST
tools to obtain actual weeks, store codes, quantities, and run IDs; the examples
do not supply live values.

## Check whether this week's data is ready

**Planner:** “Is IST ready to run this week?”

**Agent:** Calls `get_data_status`, then names the latest source week, which
workbooks are present or missing, and whether the published plan includes that
week. It asks for a missing file only when the service identifies one.

## Upload and run

**Planner:** “Here are the weekly workbooks. Check and run IST.”

**Agent:** Checks filenames, sizes, and sheet weeks before sending files. If
this chatbot can pass authorized file references, it uses `stage_workbooks`.
Otherwise it calls `create_upload_ticket` and, if its runtime can send HTTP,
POSTs the original files to the one-use URL. If it cannot send files, it gives
the planner the ticket's file-picker link. It calls `validate_upload`, shows
the affected weeks and warnings, then commits a clean authorized upload. It
follows the import's forecast run ID instead of starting another run.

The agent never claims that a chat attachment was saved merely because it can
read it, and it never converts the whole workbook into chat text.

## Change a setting

**Planner:** “Exclude this season and cap the next plan at 820 units.”

**Agent:** Resolves the season from live IST choices, previews the two changes,
and shows their exact before/after values and warnings. It waits for the
planner's separate “Yes, apply those changes” before saving. The new setting
stays active in later weeks until another confirmed change.

## Inspect and download a result

**Planner:** “For run 123, why did store X receive this style? Give me the
Excel files.”

**Agent:** Calls `get_run_results(123)`, filters the complete plan with
`query_plan(123, ...)`, explains the relevant stock/demand and policy context,
then calls `download_report(123, "forward")` and
`download_report(123, "audit")`. If a link expires, it issues a new one for
the same run.

## Ask about sales

**Planner:** “How much did this plan sell?”

**Agent:** Names the run and outcome week. For a forward plan with no observed
outcome, it says sales cannot yet be attributed. For a past week, it separates
the recommendation, confirmed transfers, observed store sales, and any
simulated audit estimate. It does not call a simulated difference causal lift.
