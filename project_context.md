# Project Context

## Why this project exists

Australian hospitals publish performance information through the Australian Institute of Health and Welfare (AIHW) MyHospitals API. The information is useful, but working with API responses directly is not the same as having a consistent analytical dataset that can be explored across hospitals, reporting periods, and measures.

This project was built to practise that end-to-end process: collecting the published data, preparing it for analysis, modelling it for reporting, and presenting selected hospital performance indicators in Power BI.

## Business problem

Hospital performance data comes from multiple API resources and includes different measures, reporting units, periods, and reporting details. Before it can be used reliably in a dashboard, those responses need to be brought into a consistent structure.

The project addresses the data preparation and reporting problem rather than attempting to evaluate the quality of individual hospitals or make clinical judgements.

## Objectives

- Ingest metadata and measure data from the AIHW MyHospitals API.
- Retain the original API responses in a Bronze layer.
- Transform the source data into structured Silver Delta tables using PySpark.
- Build a Gold dimensional model that supports analysis by measure, period, reporting unit, and reported measure.
- Create a semantic model with DAX measures for the selected indicators.
- Build Power BI report pages for emergency department and elective surgery performance.
- Orchestrate the workflow in Microsoft Fabric and configure a recurring refresh.

## Scope

The report focuses on two areas:

1. **Emergency department performance** — including presentations, four-hour departure rates, and ED departure-time indicators.
2. **Elective surgery performance** — including surgery volumes, waiting times, and the proportion of patients treated within recommended timeframes.

The project uses data made available through the AIHW MyHospitals API. It is an analytics engineering portfolio project, not an official AIHW product, a hospital benchmarking certification, or a clinical decision-support system.

## Data source

- [AIHW MyHospitals API](https://www.aihw.gov.au/hospitals/other-resources/myhospitals-api)

Interpretation should follow the definitions, reporting periods, coverage, and caveats published by AIHW. In particular, suppression, missing values, and differences in reporting-unit types need to be considered when comparing results.
