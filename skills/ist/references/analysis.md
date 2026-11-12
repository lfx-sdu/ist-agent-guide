# Interpreting IST results

The forward report is a **proposed transfer plan** for a week whose outcome is
not yet observed. Its quantities have not necessarily shipped, arrived, or sold.

An order-processing workbook can show **confirmed or fulfilled transfers**.
“Period Sales Qty” is reported sales. “Total Fulfilled Qty (Incl. Sold)” is a
workbook arithmetic field; do not read the sum as additional shipped units.

The historical audit compares a past recommendation or ranking with observed
transfers and sales through a simulator. Name the week and comparison budget.
Simulated extra sales are not measured causal lift. A current forward plan has
no realized sales outcome yet.

When asked “How much did this plan sell?”, identify its run ID and outcome
week. If the outcome is not uploaded or matched to confirmed moves, say it is
not observable yet. If observations exist, separate proposed lines, confirmed
transfers, store sales, and simulated audit estimates. Do not attribute all
sales of a SKU at a store to the IST recommendation.

Use `get_run_results` for summaries and `query_plan` for full selected transfer
rows. Absence from a dashboard preview does not prove absence from the plan.
Use `download_report` with the exact run ID; a newer run can replace the portal's
latest alias.
