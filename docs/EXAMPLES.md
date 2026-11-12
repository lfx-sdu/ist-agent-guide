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

**Agent:** If file handoff is connected, stages the files, shows validation and
changed weeks, then commits a clean authorized upload. It follows the import's
forecast run ID instead of starting another run. It gives that ID promptly and
can answer “How is my IST run going?” later with `get_run_status`.

If this chatbot cannot pass an attached Excel file to MCP, the agent says that
upload is not connected. It does not claim the file was saved or convert the
whole workbook into chat text.

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
