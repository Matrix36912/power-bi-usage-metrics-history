# Power BI Usage Metrics — Historical & Daily Capture

Preserve Power BI report usage analytics beyond the default rolling Usage Metrics window using Power Automate and SharePoint.

## Historical backfill flow

See the [historical backfill guide](docs/historical-backfill-batches.md) for its single end-to-end illustration, complete DAX query reference, batch instructions, and SharePoint field mappings.

- [Historical backfill guide and flow diagram](docs/historical-backfill-batches.md)
- [Full historical DAX query](docs/historical-backfill-dax-query.md)

## Daily incremental capture flow

See the [daily incremental capture guide](docs/daily-incremental-capture.md) for the end-to-end flow illustration, exact DAX query, recurrence settings, SharePoint duplicate check, condition branches, field mappings, and validation checklist.

- [Daily incremental capture guide and flow diagram](docs/daily-incremental-capture.md)
- [Open daily incremental flow SVG](docs/daily-incremental-capture-flow.svg)

## Two-flow design

1. **Historical backfill** — run controlled, non-overlapping date-range batches to seed the SharePoint list with older usage data.
2. **Daily incremental capture** — scheduled daily; queries the previous day and checks whether a page/date row already exists before inserting it.

The repository contains sanitized JSON examples derived from the flow definitions shared by the project author. They are reference examples, not drop-in deployment packages. Tenant-specific identifiers are replaced with placeholders; configure your own connections and environment values.

## Repository structure

- [`flows/historical-backfill.json`](flows/historical-backfill.json) — sanitized historical flow example.
- [`flows/daily-incremental-capture.json`](flows/daily-incremental-capture.json) — sanitized daily flow example.
- [Historical backfill guide](docs/historical-backfill-batches.md) — one illustration and batch-by-batch procedure.
- [Historical DAX query](docs/historical-backfill-dax-query.md) — complete copyable query.
- [Daily incremental capture guide](docs/daily-incremental-capture.md) — flow diagram, DAX query, duplicate check, mappings, and setup.
- [Daily flow diagram](docs/daily-incremental-capture-flow.svg) — standalone SVG illustration.
- [SharePoint schema](docs/sharepoint-schema.md) — suggested list columns.
- [`.gitignore`](.gitignore) — common local/secret exclusions.

## Prerequisites

- Power Automate cloud flow authoring access.
- Power BI connector permission to query the Usage Metrics semantic model.
- SharePoint access to create and read list items.
- A SharePoint list with columns matching the mappings in the examples.

## Setup

1. Create the SharePoint list using the [schema guide](docs/sharepoint-schema.md).
2. Configure Power BI and SharePoint connector connections in your own environment.
3. Replace placeholders for workspace ID, semantic model ID, site URL, list ID, and connection references. Do not publish private IDs or connection details if they expose internal configuration.
4. Follow the [historical batch procedure](docs/historical-backfill-batches.md), running and validating each date range before proceeding.
5. Follow the [daily flow guide](docs/daily-incremental-capture.md), including the duplicate check and True/False condition branches.
6. Test a short historical range and a daily run. Validate row counts, date boundaries, field mappings, and duplicate behavior before enabling schedules.
7. Review flow run history and periodically reconcile captured dates and totals against Power BI Usage Metrics.
8. can connect SharePoint list to visualize usage metrics of any captured period.

## Important caveats

- The historical flow uses a fixed date range and requires deliberate batch-by-batch execution; a monthly recurrence does not automatically manage all backfill ranges.
- The daily flow only skips records matching the configured page/date filter. If the intended grain includes additional dimensions, extend the uniqueness key accordingly.
- Concurrent runs can still create duplicates if both runs check before either creates an item. A stable key with a SharePoint unique constraint provides stronger protection.
- The daily flow uses UTC scheduling while DAX `TODAY()` follows the semantic model's evaluation context. Validate date boundaries.
- SharePoint OData filters may need date formatting and escaping for page names containing apostrophes.
- Flow JSON is a sanitized reference example, not necessarily a complete import-ready Power Automate package; configure connector bindings separately.
- Power BI, Power Automate, and SharePoint limits can affect batch size and runtime.

