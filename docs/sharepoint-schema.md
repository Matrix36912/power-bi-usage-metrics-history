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

