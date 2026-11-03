# Notebooks

The notebooks implement the exploration, ingestion, transformation, and modelling stages of the project. They are designed to run in Microsoft Fabric against the project's Lakehouse.

## Notebook flow

| Notebook | Purpose | Workflow position |
|---|---|---|
| `aihw_data_exploration.ipynb` | Explores the AIHW MyHospitals API and its available metadata and measure data. Used to understand the source before building ingestion. | Exploration |
| `bronze_aihw_metadata_ingestion.ipynb` | Retrieves source metadata needed to interpret measure responses, such as categories, measures, datasets, reporting-unit types, and reporting units. | Bronze |
| `silver_aihw_data_transformation.ipynb` | Reads the Bronze responses, parses and standardises the data with PySpark, and writes structured Silver Delta tables. | Silver |
| `gold_aihw_performance_model.ipynb` | Builds the reporting-ready Gold fact and dimension tables used by the semantic model. | Gold |

## How they fit together

1. Run the exploration notebook when reviewing the API or changing the selected source resources.
2. Run the Bronze metadata notebook to ingest reference metadata.
3. The Bronze ingestion pipeline retrieves the selected measure data-item responses.
4. Run the Silver notebook to transform the raw responses into structured tables.
5. Run the Gold notebook to create the dimensional model consumed by the semantic model.

The master orchestration pipeline coordinates the operational sequence. The exploration notebook is primarily for source investigation and is not a required recurring production step.

## Environment notes

These notebooks were developed in a specific Microsoft Fabric workspace and Lakehouse. Lakehouse references, workspace paths, and execution-related notebook metadata may need adjustment in another environment.

Before publishing notebook files, clear cell outputs and execution counts so that workspace-specific paths, execution identifiers, and run metadata are not unnecessarily included. Keep the code and explanatory markdown cells.
