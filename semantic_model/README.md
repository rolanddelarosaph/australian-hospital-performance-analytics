# Semantic Model

The semantic model exposes the Gold Lakehouse tables to Power BI using a Direct Lake model. Its definition is stored in TMDL format.

## Model structure

### Fact table

- **`gold_fact_measure_value`** — stores the hospital measure observations and the keys/context used to connect each observation to the dimensions. It also contains the DAX measures used by the report.

### Dimension tables

- **`gold_dim_measure`** — measure codes, names, categories, and units.
- **`gold_dim_period`** — reporting-period attributes, including period labels and financial-year information.
- **`gold_dim_reported_measure`** — reported-measure codes and names, including the `is_total` indicator.
- **`gold_dim_reporting_unit`** — reporting-unit identifiers, names, types, and available location attributes.

### Supporting table

- **`Hospital Display`** — a semantic-model table referenced by `model.tmdl`. It supports the hospital display/filter behaviour used in the report. Its table definition must be included in the TMDL project if it is part of the model.

## Relationships

The model defines four relationships from the fact table to its dimensions:

| Fact column | Dimension column |
|---|---|
| `gold_fact_measure_value[measure_key]` | `gold_dim_measure[measure_key]` |
| `gold_fact_measure_value[period_key]` | `gold_dim_period[period_key]` |
| `gold_fact_measure_value[reported_measure_code]` | `gold_dim_reported_measure[reported_measure_code]` |
| `gold_fact_measure_value[reporting_unit_code]` | `gold_dim_reporting_unit[reporting_unit_code]` |

These relationships provide the filtering paths used to analyse observations by measure, period, reported measure, and reporting unit.

## DAX measures

The current `gold_fact_measure_value.tmdl` contains 21 measures, including:

- Observation Count
- ED Presentations
- 4-Hour Departure Rate
- Median ED Departure Time
- 90% ED Departure Time
- Elective Surgeries
- Median Elective Surgery Wait (Days)
- Surgery Within Recommended Time
- National 4-Hour Departure Rate
- National Median Elective Surgery Wait (Days)
- Long Wait Rate by Procedure (%)
- Hospital Display Filter

The remaining measures support report display logic, hospital waiting-time calculations, admissions, and overnight-stay indicators. Refer to `gold_fact_measure_value.tmdl` for the complete DAX definitions.

## TMDL files to keep in GitHub

Keep the complete semantic-model definition, not only the table files:

```text
semantic_model/
├── .platform
├── definition.pbism
└── definition/
    ├── database.tmdl
    ├── expressions.tmdl
    ├── model.tmdl
    ├── relationships.tmdl
    └── tables/
        ├── gold_dim_measure.tmdl
        ├── gold_dim_period.tmdl
        ├── gold_dim_reported_measure.tmdl
        ├── gold_dim_reporting_unit.tmdl
        ├── gold_fact_measure_value.tmdl
        └── Hospital Display.tmdl
```

`model.tmdl` references `Hospital Display`, so include `Hospital Display.tmdl` under `definition/tables/` if this is the table definition used in your Fabric model. It was not among the TMDL files supplied for this review, so its contents have not been checked here.

The `expressions.tmdl` supplied for review contains a Direct Lake source expression with the original workspace and Lakehouse IDs. Those IDs are not passwords or access tokens, but they are environment-specific. You can keep the original export in a private backup and publish a sanitised copy or document that the source expression must be updated for another workspace. Do not replace it with arbitrary values if you expect the model to be immediately deployable.
