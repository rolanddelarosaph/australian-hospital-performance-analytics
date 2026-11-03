# Project Plan

## Development stages

| Stage | Work completed |
|---|---|
| 1. Source exploration | Explored the AIHW API resources, measure categories, measures, datasets, reporting-unit types, reporting units, and measure data items. |
| 2. Bronze ingestion | Built metadata ingestion and a child pipeline that iterates through the selected measure codes and stores the raw API responses. |
| 3. Silver transformation | Used PySpark to parse and standardise the Bronze responses into structured Delta tables while retaining source details needed for interpretation and validation. |
| 4. Gold modelling | Created a fact table and supporting dimensions for reporting by measure, period, reporting unit, and reported measure. |
| 5. Semantic model | Created a Direct Lake semantic model, relationships, a display/selector table, and DAX measures for the report. |
| 6. Reporting | Built report pages for emergency department and elective surgery performance. |
| 7. Orchestration | Created a master pipeline to run the ingestion, transformation, and modelling stages, followed by semantic model refresh. Configured a weekly schedule. |
| 8. Documentation and publication | Prepared the notebooks, pipeline exports, semantic model definition, screenshots, and project documentation for GitHub. |

## Implementation decisions

### Medallion architecture

The project separates raw ingestion, transformation, and reporting-ready modelling into Bronze, Silver, and Gold layers. This keeps the original source responses available while allowing later stages to work with structured tables.

### Preserve source context

Null values and suppression information are retained rather than automatically treated as zero. This matters because a missing or suppressed value does not mean that the measured quantity was zero.

### Separate reporting-unit and measure context

The model includes dimensions for reporting units, measures, periods, and reported measures. This allows the report to filter and interpret observations using their source context.

### Keep national reporting distinct

National observations and national benchmarks are not treated as ordinary hospital records or added to hospital-level totals. They are used in the report where a national comparison is appropriate.

### Use a semantic model for report calculations

The Gold tables provide the reporting structure, while DAX measures centralise calculations used by the Power BI report. The semantic model is stored as a TMDL project definition.

### Automate the workflow

A master pipeline coordinates the main stages and refreshes the semantic model after the Gold layer is prepared. The configured schedule is weekly, on Sunday at 2:00 AM Philippine time (UTC+8).

## Current project status

The end-to-end Fabric pipeline has completed successfully in the development workspace. The GitHub repository contains exported project artefacts for review and portfolio presentation. Some exported pipeline and semantic-model references are specific to the original Fabric workspace and may need to be updated before reuse elsewhere.

## Possible next improvements

- Add a reproducible setup guide for a fresh Fabric workspace.
- Add more automated data-quality checks and documented acceptance criteria.
- Expand report coverage as additional measures are selected and validated.
- Add a clear explanation of indicator definitions and comparability limitations.
