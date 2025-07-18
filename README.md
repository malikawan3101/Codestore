# Advanced Company Profiling and Data Validation

A Jupyter Notebook that:
- Ingests a CSV of company data  
- Cleans, deduplicates, profiles and validates the dataset 
- Generates Data Quality report, before and after processing 
- Matches records against Companies House API  
- Enriches missing data and validates formats  
- Produces visual summaries and exports a clean dataset

---

## Table of Contents

1. [Overview](#overview)  
2. [Setup & Requirements](#setup--requirements)  
3. [Workflow](#workflow)  
4. [Data Quality Reporting](#data-quality-reporting)  
5. [How to Run](#how-to-run)  
6. [Outputs](#outputs)  
7. [Future Improvements](#future-improvements)  

---

## Overview

This notebook validates and enhances a company dataset by:

- Ensuring data cleanliness and consistency  
- Removing duplicates by `CompanyNumber`, keeping the most recent  
- Cross-referencing with Companies House to verify and enrich records  
- Address validation, name similarity scoring, ID padding, date formatting, custom data quality and profiling rules, data quality framework,
- Generating a final clean dataset and summarizing data quality before and after

---

## Setup & Requirements

```bash
pip install pandas numpy seaborn matplotlib requests fuzzywuzzy os re request time
````
## Workflow

1. **Data ingestion**  
   Load the CSV into a pandas DataFrame and inspect its schema.

2. **Cleansing**  
   - Trim and standardize strings (e.g., company names, addresses)  
   - Parse dates and correct all dates 
   - Zero-pad `CompanyNumber` to ensure a consistent 8-digit format

3. **Deduplication**  
   - Sort by `CompanyNumber` ascending and `IncorporationDate` descending  
   - Keep only the most recent record for each `CompanyNumber`

4. **Data Profiling**  
   - Compute completeness, uniqueness_pct, duplicate_quality, validity_pct, address_completeness, rule_violations null counts and unique values  
   - Visualize value distributions and missingness using seaborn

5. **API Matching**  
   - Query the Companies House API using `CompanyName`  
   - Retrieve official match data: name, number, status, address snippet, creation date

6. **Enrichment**  
   - Fill missing `CompanyNumber` where `name_similarity > 85%`  
   - Extract additional fields: matched name, address snippet, and company status

7. **Validation**  
   - Create flags like `match_found`, `company_number_filled`  
   - Optionally validate address and date fields

8. **Reporting & Visualization**  
   - Generate summary metrics: total, matched, active, filled, duplicates removed  
   - Display bar charts, pie charts, and histograms  
   - Export supplementary datasets: unmatched, duplicates, etc.
   - Compare profiling data before and after processing data to compare results

9. **Export**  
   - Save the final cleansed dataset as `Final_validated_companies.csv`  
   - Export intermediate CSV files for audit and review

---
## Data Quality Reporting

The notebook produces a detailed data‑quality report including:

### Summary Metrics
- Total records processed  
- Companies matched vs unmatched  
- Active vs dissolved company breakdown  
- Count of high‑confidence (≥ 85%) name matches  

### Duplicate Analysis
- Number of duplicate CompanyNumbers removed  
- Tabular view of records involved in duplicate sets  

### Missing ID Fill Statistics
- Number of CompanyNumbers enriched from the API  
- Rows flagged as filled  

### Visual Diagnostics
- Bar chart: matched vs unmatched companies  
- Pie chart: company status distribution  
- Histogram: name similarity scores  
- Tabular view: duplicate records with key fields (company name, number, address, date, status)  


---

## Outputs

- **Final_validated_companies.csv** — Final cleaned and matched dataset  

---

## How to Run

1. Clone the repository  
2. Place your CSV in the project folder and update the file path in the notebook  
3. Impotant: Set your Companies House API key in the notebook  
4. Open the notebook in Jupyter and run all cells sequentially  
5. Review summary tables and visualizations directly in the notebook  
6. Check the working directory for exported CSV files  

---

## Future Improvements

- Add fuzzy matching for address and incorporation date validation  
- Integrate Great Expectations for automated data validations  
- Automate the pipeline via cron jobs or an orchestration tool like Airflow  
- Develop an interactive dashboard using Plotly Dash or Voila
- Further clean and massage data

