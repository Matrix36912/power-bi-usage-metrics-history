# Power BI Usage Metrics — Historical & Daily Capture

Automate the preservation of Power BI report usage analytics by querying the Usage Metrics semantic model and writing results to a SharePoint list.

Power BI Usage Metrics provides a rolling view of activity. This project documents a pattern for retaining a longer history by using two Power Automate cloud flows:

1. **Historical backfill (run once):** capture a defined historical date range in manageable batches.
2. **Daily incremental capture (scheduled):** capture the previous day's usage data regularly so new records are retained beyond the rolling window.

> **Important:** The repository currently includes a sanitized example based on one supplied flow definition. That definition uses a monthly recurrence and fixed dates; it is not a verified implementation of the daily flow or a complete historical backfill. Update and test the flow configuration in your own environment before using it.

## Repository structure

- `flows/example-monthly-capture.json` — sanitized example of the supplied flow definition.
- `docs/sharepoint-schema.md` — suggested SharePoint list columns.
- `.gitignore` — excludes common local and secret-bearing files.

## How it works

1. Power Automate runs a DAX query against the Power BI Usage Metrics semantic model.
2. The query groups usage data by date and report page, with metrics such as page users, page views, and page-view share.
3. An `Apply to each` loop writes each result to a SharePoint list.
4. The historical flow should process the chosen date range in batches.
5. The incremental flow should run daily and query a completed date (typically yesterday) to avoid querying an incomplete current day.

## Prerequisites

- Permission to query the relevant Power BI Usage Metrics semantic model.
- A Power Automate cloud flow with the Power BI connector.
- A SharePoint site and list where you have permission to create items.
- Approved organizational permissions and connector connections.

## Setup overview

1. Create or identify a SharePoint list using the suggested schema in [docs/sharepoint-schema.md](docs/sharepoint-schema.md).
2. Create a Power Automate flow and add **Run a query against a dataset**.
3. Replace all placeholder workspace, semantic-model, SharePoint site, and list values with your own configuration. Do not commit tenant-specific identifiers or connection exports to a public repository.
4. Use a DAX date filter for the range to capture. For a backfill, process a bounded interval per run or batch to avoid query and connector limits.
5. Add an **Apply to each** over the query's returned rows and create SharePoint items, mapping the query columns to list columns.
6. For the historical backfill, run once for each batch and verify counts before proceeding.
7. For daily capture, configure a daily recurrence and a dynamic date window (usually yesterday). The supplied example is monthly with hard-coded dates and must not be used as-is for this purpose.
8. Test with a small date range, check for duplicate rows, validate totals against Usage Metrics, and only then enable the production schedule.

## Important design considerations

- **Idempotency:** the sample pattern creates items. Re-running a date range can create duplicates unless you implement a unique key and check/update existing records.
- **Retention and completeness:** compare captured dates and row counts regularly. Usage Metrics definitions and available fields can change.
- **Privacy:** user-level identifiers can be sensitive. Store them only if necessary and authorized; keep public examples anonymized.
- **Permissions:** use least-privilege connector access and follow your organization's data-handling policies.
- **Limits:** Power BI query, Power Automate, and SharePoint thresholds can affect batch sizes and run duration.
- **Schema:** connector output column names may include bracketed names such as `[Date]`; verify the actual response before mapping expressions.
- **No secrets in source:** use managed connections and environment configuration. Never commit credentials, access tokens, or exported connection details.

## Status

Documentation starter and sanitized sample. The daily incremental flow and a complete historical backfill definition still need to be added and validated.

## License

No license has been added yet. Add a license only after deciding how you want others to reuse the material.
