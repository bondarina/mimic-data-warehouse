# mimic-data-warehouse

# MIMIC Healthcare Data Warehouse

Building a healthcare data warehouse with Python and PostgreSQL, including data ingestion, SQL transformations, data quality checks, and hospital admission analytics.

Welcome to my **MIMIC Healthcare Data Warehouse** project! 👋

This repository documents my hands-on learning journey in Data Engineering. The goal is to build a reproducible pipeline that transforms source CSV files into structured analytical tables, starting locally and gradually introducing incremental loading and automation.

> 🚧 **Project status:** Initial setup and source data exploration. The architecture and requirements below describe the planned implementation.

---

## 🏗️ Data Architecture

The project follows **Medallion Architecture**, organizing data into **Bronze**, **Silver**, and **Gold** layers.

### Source Data

The source is **MIMIC-IV Clinical Database Demo v2.2**. The initial scope includes two related tables:

- **patients** — patient records, identified by `subject_id`.
- **admissions** — hospital admissions, identified by `hadm_id` and linked to patients through `subject_id`.

### Data Flow

```text
MIMIC-IV Demo
patients + admissions
        │
        ▼
CSV files → Python ingestion → PostgreSQL
                                  │
                                  ▼
                               Bronze
                           Source records
                                  │
                                  ▼
                               Silver
                        Cleaned and validated
                                  │
                                  ▼
                                Gold
                          Analytical tables
```

1. **Bronze Layer:** Stores incoming records with loading metadata, including the source file, batch, and ingestion time.
2. **Silver Layer:** Converts data types, processes timestamps, handles missing values and duplicates, and validates keys and relationships.
3. **Gold Layer:** Provides one row per hospital admission with calculated length of stay, summaries by admission type, and admission counts per patient.

---

## 📖 Project Overview

This project focuses on:

1. **Data Architecture:** Organizing a PostgreSQL warehouse into Bronze, Silver, and Gold layers.
2. **Data Ingestion:** Loading CSV files with Python and recording their origin.
3. **Data Transformation:** Preparing consistent, reliable tables using SQL.
4. **Data Quality:** Checking row counts, identifiers, relationships, and timestamps.
5. **Analytics:** Building tables and SQL summaries for hospital admission analysis.
6. **Reproducibility:** Documenting the workflow and preventing duplicates during repeated runs.

### Development Stages

- **Stage 1 — Local Pipeline:** Load the first batch into PostgreSQL and build the three warehouse layers.
- **Stage 2 — Incremental Loading:** Process additional batches, track processed files, and handle repeated runs and failures.
- **Stage 3 — Databricks:** Adapt the working pipeline to Apache Spark and Delta tables.
- **Stage 4 — Automation:** Schedule batch processing and monitor execution.

dbt will be explored after the basic ingestion and SQL transformation workflow is established.

---

## 📊 Dataset

**Dataset:** MIMIC-IV Clinical Database Demo  
**Version:** 2.2  
**Provider:** PhysioNet  
**Source:** https://physionet.org/content/mimic-iv-demo/2.2/

The initial source files contain:

| Table | Rows | Columns | Description |
| --- | ---: | ---: | --- |
| patients | 100 | 6 | Patient records |
| admissions | 275 | 16 | Hospital admission records |

A patient can have multiple admissions. The tables are connected through `subject_id`.

The dataset contains deidentified records with shifted dates. This project uses it to practice data engineering and does not aim to draw clinical conclusions or analyze real calendar trends.

### Local Data Storage

Source files are stored locally under `data/raw/`. The original download is preserved without modification.

The `data/` directory is excluded from Git. Original CSV files and generated data tables are not published in this repository.

### Attribution

The dataset is available under the **Open Data Commons Open Database License (ODbL) 1.0**:

https://opendatacommons.org/licenses/odbl/1-0/

Johnson, A., Bulgarelli, L., Pollard, T., Horng, S., Celi, L. A., & Mark, R. (2023).
*MIMIC-IV Clinical Database Demo (version 2.2).* PhysioNet.
https://doi.org/10.13026/dp1f-ex47

The dataset license applies to the source data and is separate from any license for this project's code.

---

## 🛠️ Tools & Technologies

| Tool | Purpose | Stage |
| --- | --- | --- |
| Python | Read source files and load data | Initial implementation |
| PostgreSQL | Store warehouse tables | Initial implementation |
| SQL | Transform, validate, and analyze data | Initial implementation |
| Git & GitHub | Version control and documentation | From project setup |
| Databricks, Spark & Delta | Extend the pipeline to a new platform | Later stage |
| dbt | Organize SQL transformations, tests, and documentation | Later stage |

The orchestration tool will be selected during the automation stage.

---

## 🚀 Project Requirements

### Building the Data Warehouse — Data Engineering

#### Objective

Build a reproducible pipeline that loads patient and admission data into PostgreSQL, validates it, and prepares analytical tables.

The first milestone is to process the initial batch, run quality checks, and produce an admissions table with calculated length of stay through a single command.

#### Specifications

- **Data Sources:** Use the `patients` and `admissions` tables from MIMIC-IV Demo v2.2.
- **Storage:** Organize PostgreSQL tables into Bronze, Silver, and Gold layers.
- **Ingestion Metadata:** Record the source file, batch, and loading time.
- **Data Quality:** Check row counts, required identifiers, key uniqueness, relationships, and timestamp validity.
- **Integration:** Connect admissions to patients using `subject_id`.
- **Missing Values:** Define and document handling rules before calculating metrics.
- **Repeatability:** Ensure repeated processing does not create duplicate prepared records.
- **Documentation:** Describe the source data, transformations, quality checks, and execution steps.

### Incremental Loading

#### Objective

Practice processing new data in batches while keeping the warehouse consistent.

#### Specifications

- Prepare three fixed learning batches from the original dataset.
- Load referenced patients before or together with their admissions.
- Start with manual batch submission.
- Track processed files and handle failed loads.
- Introduce updates to existing records as a separate exercise.
- Automate batch availability and processing at a later stage.

These batches simulate incoming data for learning; they do not reproduce the actual sequence of hospital operations.

### Analytics & Reporting

#### Objective

Prepare SQL-based analysis of hospital admissions.

#### Specifications

The Gold layer will support:

- **Admission-Level Analysis:** One row per admission with calculated length of stay.
- **Admission Types:** Admission counts and average length of stay by admission type.
- **Admissions per Patient:** The number of admissions associated with each patient.

Missing or invalid admission and discharge timestamps will be handled according to documented rules.

---

## 📂 Repository Structure

Planned structure — implementation files will be added as the project progresses.

```text
mimic-data-warehouse/
│
├── data/                     # Local only; excluded from Git
│   ├── raw/                  # Original source files and archive
│   └── processed/            # Generated local data files
│
├── src/                      # Python ingestion code
│   └── ingest.py
│
├── sql/                      # SQL scripts by warehouse layer
│   ├── bronze/
│   ├── silver/
│   └── gold/
│
├── tests/                    # Data quality and pipeline checks
│
├── docs/                     # Project documentation
│   └── data_sources.md
│
├── README.md
├── .gitignore
└── requirements.txt          # Python dependencies
```

Setup instructions and the pipeline run command will be added with the first working implementation.

---

## 📚 Learning Resources

The organization of this project is inspired by
[Data With Baraa's SQL Data Warehouse Project](https://github.com/DataWithBaraa/sql-data-warehouse-project).

The approach is adapted to MIMIC hospital admission data, Python, and PostgreSQL.

---

## 🌟 About Me

I'm learning Data Engineering through hands-on projects.

In this repository, I document my progress with Python, SQL, PostgreSQL, data modeling, and reliable data pipelines.

[My GitHub profile](https://github.com/bondarina)
