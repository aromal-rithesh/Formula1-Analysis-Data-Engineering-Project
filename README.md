# Formula 1 Data Engineering on Azure

This project demonstrates an end-to-end data engineering solution for ingesting, processing, and analyzing Formula 1 race data. The platform is built entirely on Microsoft Azure, leveraging services like Azure Data Factory, Databricks, and Data Lake Storage to create a robust and scalable pipeline following the Medallion Architecture.

---

## 🏁 Project Goal

The primary objective is to build a modern data platform that transforms raw, complex Formula 1 data into structured, query-ready datasets. These final datasets can be used for various analytical purposes, such as analyzing driver performance, calculating championship standings, and identifying trends across seasons.

---

## 🛠️ Tech Stack

* **Orchestration:** Azure Data Factory (ADF)
* **Data Processing:** Azure Databricks (using PySpark & SQL)
* **Data Lake:** Azure Data Lake Storage (ADLS) Gen2
* **Data Architecture:** Medallion Architecture (raw, processed, presentation)
* **Secrets Management:** Azure Key Vault

---

## 🏛️ Architecture

The project follows a classic data engineering workflow orchestrated by Azure Data Factory. Secrets, such as API keys or database credentials, are securely stored in Azure Key Vault and accessed by ADF and Databricks without being hard-coded.


### Data Flow

1.  **Data Fetch (Raw Layer):** Raw data is fetched from the source. The data is landed in its original format (JSON,CSV) into the **raw** container in ADLS.
2.  **Ingestion (Processed Layer):** The ADF pipeline then calls a series of Databricks notebooks to process the raw data. These notebooks perform:
    * Schema enforcement and data type correction.
    * Cleaning of null values and inconsistencies.
    * Flattening of nested JSON structures.
    * The cleaned, validated data is stored as Delta tables in the **processed** container.
3.  **Transformation (Presentation Layer):** Another set of Databricks notebooks runs business logic on the Processed tables. This involves:
    * Joining different datasets (e.g., results, drivers, circuits).
    * Aggregating data to create meaningful insights (e.g., calculating driver points, race winners).
    * The final, aggregated data is stored as performant Delta tables in the **presentaion** container, ready for consumption by BI tools like Power BI or for direct querying.

## 🚀 How to Run the Pipeline

1.  **Prerequisites:**
    * An active Azure Subscription.
    * All required Azure resources (ADF, Databricks) have been deployed.
    * The Databricks workspace is configured with a cluster and has access to ADLS.
    * ADF Linked Services are configured to connect to Key Vault, Databricks, and ADLS.

2.  **Setup:**
    * Store all necessary secrets (e.g., API keys) in Azure Key Vault.
    * Ensure the Databricks notebooks are synced to the workspace.
    * Publish the ADF pipelines from your Git branch to the Data Factory service.

3.  **Execution:**
    * Navigate to the main orchestration pipeline in the Azure Data Factory UI.
    * Trigger the pipeline manually or wait for its scheduled run.
    * Monitor the pipeline run to ensure all activities (including the Databricks notebook executions) complete successfully.

---

## 📊 Example Insights (Presentaion Layer)

The Presentaion layer tables can be used to answer questions like:

* Who were the top 10 drivers with the most race wins in the 2000s?
* What is the average pit stop time at the Monaco Grand Prix?
* Which constructor has accumulated the most points in the hybrid era?

