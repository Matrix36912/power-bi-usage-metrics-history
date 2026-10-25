# Suggested SharePoint list schema

Create a SharePoint list for captured usage metrics. The exact column types should match the query and connector behavior in your environment.

| Column | Suggested type | Purpose |
|---|---|---|
| Title | Single line of text | Optional label or unique row key |
| UsageDate | Date only | Date the usage occurred |
| PageName | Single line of text | Report page/section name |
| TotalPageUsers | Number | Distinct users for the page/date group |
| TotalPageViews | Number | Page views for the page/date group |
| PageViewShare | Number | Page-view share returned by the semantic model |
| CaptureDate | Date and time | Timestamp when the flow captured the row |

## Recommended improvements

- Consider adding a stable unique key based on the usage date, page, and any other dimensions needed for your grain.
- If capturing user-level data is required and approved, document the privacy rationale and restrict list access appropriately. Avoid exposing user identifiers in a public repository.
- Decide how to handle reruns: skip existing keys, update existing records, or write to a staging list before deduplication.
- Confirm whether `PageViewShare` should be stored as a decimal or percentage, and use the same convention in reporting.
- Validate SharePoint internal column names before mapping fields in Power Automate.
