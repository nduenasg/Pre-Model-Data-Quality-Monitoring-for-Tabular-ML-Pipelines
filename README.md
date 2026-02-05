## Problem Statement

In real-world machine learning systems, data quality issues are a primary cause of model degradation and training failures. Common problems include:

- Silent schema changes
- Unexpected missing values
- Outliers caused by upstream system errors
- Distribution drift between training and incoming data batches

These issues often occur *before* modeling begins and can invalidate training runs or bias evaluation results.

This project focuses on detecting such issues **before model training**, using statistical and schema-based checks on tabular data.

## Scope and Assumptions

**In scope**
- Batch-based tabular datasets
- Pre-modeling data validation
- Offline statistical checks
- Interpretable metrics and thresholds

**Out of scope**
- Real-time streaming validation
- Model performance monitoring
- Automated retraining pipelines
- Cloud-specific MLOps tooling

## Dataset

- **Source:** UCI / OpenML Adult Census Income Dataset
- **Type:** Tabular, mixed numerical and categorical features
- **Target variable:** `income` (used only for reference, not validation)
- **Rows:** ~48,000
- **Usage in this project:**
  - A clean reference dataset defines the expected data distribution
  - Synthetic batches are generated to simulate data quality failures


| Feature Type | Count |
|-------------|------|
| Numerical   | 6    |
| Categorical | 8    |

## Data Schema

A strict schema is defined to validate incoming batches.

For each column, the schema specifies:
- Data type
- Nullability
- Allowed categories (for categorical features)
- Valid value ranges (for numerical features)

The schema is stored as a versioned JSON file and used as the first validation gate.


## Data Quality Checks

### 1. Schema Validation
Detects:
- Missing or extra columns
- Incorrect data types
- Unexpected categorical values

### 2. Missing Value Monitoring
- Per-column missing rate
- Comparison against reference missing rates
- Threshold-based alerts

### 3. Outlier Detection
- Z-score and IQR-based methods
- Column-wise outlier proportions
- Alerts when proportions exceed expected bounds

### 4. Distribution Drift
- Numerical features: Kolmogorov–Smirnov test
- Categorical features: Chi-square test
- Drift severity classification (none / mild / severe)
- ## Batch Simulation

To test the system, synthetic batches are generated from the reference dataset:

- **Clean batches:** match reference distribution
- **Missing-data batches:** injected nulls
- **Outlier batches:** extreme value injection
- **Drifted batches:** shifted distributions
- **Schema-break batches:** column removal or type changes

This allows deterministic testing of each validation component.



