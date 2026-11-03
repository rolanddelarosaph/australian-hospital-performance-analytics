# Pipelines

The project uses two Fabric pipelines to separate measure-level ingestion from end-to-end orchestration.

## `pl_aihw_bronze_ingestion.json`

This is the child ingestion pipeline. It iterates over the selected AIHW measure codes, calls the MyHospitals API data-items endpoint for each measure, and stores the returned raw JSON responses in the Bronze area of the Lakehouse.

The pipeline uses a REST connection and pipeline parameters to make the ingestion configuration reusable within the Fabric workspace.

## `pl_aihw_master_orchestration.json`

This is the parent workflow. Its sequence is:

1. Run the Bronze metadata ingestion notebook.
2. Invoke the child Bronze ingestion pipeline.
3. Run the Silver transformation notebook.
4. Run the Gold dimensional modelling notebook.
5. Refresh the semantic model.

This ordering ensures that reference metadata and measure responses are ingested before transformation and modelling, and that the semantic model refresh occurs after the Gold tables are prepared.

## Schedule and operational behaviour

The master pipeline is configured for a weekly schedule on Sunday at 2:00 AM Philippine time (UTC+8). Activity retry settings are configured in Fabric.

## Reusing the exported JSON

The JSON files are exported definitions from the development workspace, not plug-and-play templates for every Fabric environment. They contain workspace-specific item references and connection configuration names. Before importing or adapting them in another workspace, replace the relevant IDs and configure valid connections, Lakehouse references, notebook references, and semantic model references for that environment.

Do not publish credentials, access tokens, passwords, or connection secrets. The public copies should also avoid unnecessary personal identifiers and workspace-specific metadata.
