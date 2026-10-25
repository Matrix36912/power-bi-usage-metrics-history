# Historical backfill DAX query

This is the full query from the screenshot you supplied, formatted for readability. It includes the user key in the query result; the shown SharePoint Create item action does not map that field into the list.

```dax
EVALUATE
SELECTCOLUMNS(
    FILTER(
        SUMMARIZECOLUMNS(
            'Dates'[Date],
            'Report pages'[SectionName],
            'Report page views'[UserKey],
            "TotalPageUsers", [Total page users],
            "TotalPageViews", [Total page views],
            "PageViewShare", [Page view share]
        ),
        'Dates'[Date] >= DATE(2026, 2, 10) &&
        'Dates'[Date] <= DATE(2026, 2, 17)
    ),
    "Date", 'Dates'[Date],
    "PageName", 'Report pages'[SectionName],
    "User", 'Report page views'[UserKey],
    "TotalPageUsers", [TotalPageUsers],
    "TotalPageViews", [TotalPageViews],
    "PageViewShare", [PageViewShare]
)
```

## Change the batch dates

For each historical batch, change both dates in the `FILTER` expression:

```dax
'Dates'[Date] >= DATE(2026, 2, 10) &&
'Dates'[Date] <= DATE(2026, 2, 17)
```

The example above captures 10–17 February 2026, inclusive. Use consecutive, non-overlapping ranges for subsequent batches and validate each run before proceeding.

## Notes

- Keep the same query dimensions and output aliases unless you also update the downstream SharePoint mappings.
- The query returns `User`, but the provided SharePoint action does not store it. Consider whether user-level data is needed and approved before adding any mapping.
- The dates are hard-coded for the example. They do not automatically advance.
