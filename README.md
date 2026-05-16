# Data Storm 7.0: Latent Demand Estimation for Traditional FMCG Retail

## Project Overview

This repository contains the complete analytical solution for the **Data Storm 7.0 Advanced Analytics Competition**, focused on estimating latent maximum monthly purchase potential for retail outlets in a traditional-trade beverage distribution network across Sri Lanka.

### Challenge Objective

Given 2.376M historical transactions across 36 months (2023–2025), estimate the true monthly purchase potential for 20,000+ outlets, accounting for demand censoring under operational constraints (cooler capacity, distributor allocation, sales-rep effort).

**Key Insight**: Observed sales ≠ True demand. Historical transactions represent $Y_{\text{observed}} = \min(Y^*_{\text{latent}}, C_{\text{operational}})$. This solution recovers latent demand through forensic analysis, environmental clustering, and peer benchmarking.


---

## Repository Structure

```
Data_Storm/
├── README.md                          # This file
├── requirements.txt                   # Python dependencies
├── .gitignore                         # Git ignore configuration
│
├── data/                              # Data pipeline (Bronze → Silver → Gold)
│   ├── bronze/                        # RAW INPUT DATA (user-provided)
│   │   ├── transactions_history_final.csv       # 2.376M transaction records
│   │   ├── outlet_master.csv                    # 20K+ outlet reference data
│   │   ├── outlet_coordinates.csv               # Outlet lat/long
│   │   ├── distributor_seasonality_details.csv  # Regional seasonality
│   │   └── holiday_list.csv                     # Holiday calendar
│   │
│   ├── silver/                        # CLEANED & VALIDATED DATA (auto-generated)
│   │   ├── transactions_clean.csv               # 2.371M cleaned transactions
│   │   ├── outlet_master_clean.csv              # Cleaned outlet master
│   │   ├── outlet_coordinates_clean.csv         # Validated coordinates
│   │   ├── seasonality_clean.csv                # Cleaned seasonality
│   │   └── holiday_clean.csv                    # Validated holidays
│   │
│   ├── gold/                          # FINAL DELIVERABLES (auto-generated)
│   │   ├── InferaX_predictions.csv              # ⭐ PRIMARY OUTPUT: Outlet potential estimates
│   │   └── outlet_feature_table.csv             # Feature engineering intermediate
│   │
│   ├── rejected/                      # DATA QUALITY ARTIFACTS (auto-generated)
│   │   └── rejected_records.csv                 # 241K flagged records with reasons
│   │
│   └── reports/                       # QUALITY METRICS (auto-generated)
│       └── data_quality_summary.csv             # Rejection statistics
│
├── notebooks/                         # JUPYTER NOTEBOOKS (execute in order)
│   ├── 01_dataset_understanding.ipynb           # EDA, data profiling
│   ├── 02_data_forensics.ipynb                  # Quality checks, anomaly detection
│   └── 03_feature_engineering_gold_layer.ipynb  # Clustering, potential estimation
│
├── docs/                              # TECHNICAL DOCUMENTATION
│   ├── FORENSIC_ANALYSIS_REPORT.md              # Phase A findings (~8.5K words)
│   ├── REJECTED_RECORDS_METHODOLOGY.md          # Quality framework (~5.2K words)
│   ├── WHY_SALES_CANNOT_REPRESENT_DEMAND.md     # Demand censoring theory (~6.5K words)
│   ├── EXECUTIVE_BRIEF_FOR_JUDGES.md            # Competition strategy (~2.5K words)
│   └── README_DOCUMENTATION.md                  # Navigation guide for docs
│
├── outputs/                           # USER OUTPUTS (if generated)
│   └── [visualizations, reports]
│
├── src/                               # UTILITY MODULES (for future extension)
│   ├── checks/                        # Data validation functions
│   ├── feature_engineering/           # Feature engineering utilities
│   ├── modeling/                      # Model development
│   └── spatial/                       # Geospatial utilities
│
└── reports/                           # ANALYSIS REPORTS (if generated)
    └── [competitive analysis, market reports]
```

---

## Data Pipeline: Bronze → Silver → Gold

### Bronze Layer: Raw Input Data

The **Bronze layer** contains raw, unvalidated data as provided by the distributor. These files must be placed in `data/bronze/` before execution:

| File | Records | Purpose |
|------|---------|---------|
| `transactions_history_final.csv` | 2,376,389 | Transaction-level sales history (Outlet, Date, Volume, Bill, SKU, Distributor) |
| `outlet_master.csv` | 20,000+ | Outlet reference data (ID, Location, Size, Cooler Count, Region) |
| `outlet_coordinates.csv` | 20,000+ | Geospatial data (Outlet ID, Latitude, Longitude) |
| `distributor_seasonality_details.csv` | ~1,200 | Regional seasonality patterns by month |
| `holiday_list.csv` | ~36 | Holiday calendar (dates, significance) |

**Status**: RAW, UNVALIDATED. May contain nulls, duplicates, invalid references, negative values, outliers.

### Silver Layer: Cleaned & Validated Data

The **Silver layer** is auto-generated by Notebook 01 and 02. It contains cleaned, deduplicated, validated data with quality metrics.

**Key Changes from Bronze→Silver**:
- Null values handled (imputation or removal, documented)
- Duplicate transactions removed (0 exact duplicates found)
- Invalid references corrected (Outlet IDs, Distributor IDs validated against master)
- Negative volumes flagged and separated (4,753 ERP reversals identified)
- Zero volumes flagged and separated (100 system entry artifacts identified)
- Pricing outliers identified (236,818 SKU_09 premium product records flagged)
- Data lineage maintained (rejected records preserved with reason codes)

**Files Generated**:
- `transactions_clean.csv` — 2,371,536 validated transactions
- `outlet_master_clean.csv` — Outlet reference with corrections
- `outlet_coordinates_clean.csv` — Validated coordinates
- `seasonality_clean.csv` — Cleaned seasonality patterns
- `holiday_clean.csv` — Validated holiday calendar

**Quality Metrics**: Documented in `data/reports/data_quality_summary.csv` and Notebook 02 outputs.

### Gold Layer: Final Deliverables

The **Gold layer** is auto-generated by Notebook 03. It contains feature-engineered data and latent demand estimates ready for strategic decision-making.

**Primary Output** ⭐:
- `InferaX_predictions.csv` — **Main submission artifact**
  - Columns: `Outlet_ID`, `Maximum_Monthly_Liters`
  - Rows: 20,000+ outlets
  - Meaning: Estimated latent maximum monthly purchase potential (liters)
  - Methodology: Environmental clustering + peer benchmarking + conservative uncensoring

**Secondary Output**:
- `outlet_feature_table.csv` — Complete feature engineering intermediate
  - Columns: All 30+ features including behavioral, operational, spatial, cluster assignments
  - Use: Model development, feature importance analysis, audit trail

**Rejected Records Artifact**:
- `data/rejected/rejected_records.csv` — Quality control record
  - All 241,671 flagged records with rejection reason codes
  - Reason: `NEGATIVE_VOLUME`, `ZERO_VOLUME_GHOST`, `REVENUE_PER_LITER_ANOMALY`
  - Use: Audit trail, operational investigation, data quality metrics

---

## Quick Start: Environment Setup

### Prerequisites

- **Python**: 3.9 or later
- **Git**: For cloning repository
- **Disk Space**: ~2GB (raw data + intermediate files)

### Step 1: Clone Repository

```bash
git clone <repository_url>
cd Data_Storm
```

### Step 2: Prepare Bronze Data

Place the five required CSV files in the `data/bronze/` directory:

```bash
# Verify the files are in place
ls -la data/bronze/
# Expected output:
# transactions_history_final.csv
# outlet_master.csv
# outlet_coordinates.csv
# distributor_seasonality_details.csv
# holiday_list.csv
```

### Step 3: Create Virtual Environment

**macOS / Linux**:
```bash
python3 -m venv venv
source venv/bin/activate
```

**Windows (PowerShell)**:
```powershell
python -m venv venv
venv\Scripts\Activate.ps1
```

**Windows (Command Prompt)**:
```cmd
python -m venv venv
venv\Scripts\activate.bat
```

### Step 4: Install Dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

This installs:
- **Data Processing**: pandas, numpy
- **Visualization**: matplotlib, seaborn
- **Machine Learning**: scikit-learn, scipy, statsmodels
- **Geospatial**: geopandas, osmnx, shapely
- **Jupyter**: jupyter, ipykernel
- **Utilities**: openpyxl (Excel support), missingno (null visualization)

### Step 5: Register Jupyter Kernel

```bash
python -m ipykernel install --user --name data_storm --display-name "Python (Data Storm)"
```

Verify kernel is registered:
```bash
jupyter kernelspec list
# Should show: data_storm                     /path/to/data_storm
```

### Step 6: Launch Jupyter

```bash
jupyter notebook
```

This opens Jupyter Lab in your default browser at `http://localhost:8888`.

---

## Execution Order of Notebooks

Execute notebooks **sequentially** in this order. Each notebook depends on outputs of previous notebooks.

### Notebook 1: Dataset Understanding (EDA)

**File**: `notebooks/01_dataset_understanding.ipynb`

**Objective**: Exploratory Data Analysis, data profiling, initial pattern discovery


**Key Outputs**:
- Data shape, dtypes, value ranges
- Missing value patterns (visualization)
- Statistical summaries by SKU, Distributor, Outlet
- Temporal patterns (2023–2025)
- Correlation matrices

**Files Generated**: None (EDA only)

**Key Metrics Discovered**:
- 2,376,389 total transactions
- 10 SKUs, 5 distributor regions, 20,000+ outlets
- Revenue range: ₨2,700–₨132,500 per transaction
- Volume range: 1.2–600L per transaction

---

### Notebook 2: Data Forensics (Quality Validation)

**File**: `notebooks/02_data_forensics.ipynb`

**Objective**: Comprehensive data quality assessment, anomaly detection, rejection logic implementation


**Key Outputs**:
- Null value detection (result: 0 nulls)
- Duplicate row detection (result: 0 exact duplicates)
- Negative volume analysis (result: 4,753 records flagged as ERP reversals)
- Zero volume analysis (result: 100 records flagged as system ghosts)
- Pricing anomaly detection (result: 236,818 SKU_09 records flagged)
- Geographic anomalies (result: 0 invalid coordinates)
- Revenue per liter distribution analysis

**Files Generated**:
- `data/silver/transactions_clean.csv` — 2,371,536 cleaned transactions
- `data/silver/outlet_master_clean.csv` — Cleaned outlet reference
- `data/silver/outlet_coordinates_clean.csv` — Validated coordinates
- `data/silver/seasonality_clean.csv` — Cleaned seasonality data
- `data/silver/holiday_clean.csv` — Validated holidays
- `data/rejected/rejected_records.csv` — 241,671 flagged records with reason codes
- `data/reports/data_quality_summary.csv` — Quality metrics summary

**Critical Findings**:
- Data containment issue: 241,671 rejected records + 2,371,536 clean = 2,613,207 (exceeds raw 2,376,389)
  - Implies: Duplicate entries in rejection process or data leakage; requires downstream investigation
- SKU_09 extreme pricing: 231,302 records at exactly ₨2,200/liter (vs. ₨300–440 for other SKUs)
  - Interpretation: Separate product category (concentrate/premium), not data error
  - Decision: Retained in clean data for Phase 3 modeling (not rejected)
- Negative volumes by region: West 45.8%, indicating regional system differences

**Data Ready for Phase 3**: ✓ Yes

---

### Notebook 3: Feature Engineering & Gold Layer Generation

**File**: `notebooks/03_feature_engineering_gold_layer.ipynb`

**Objective**: Feature engineering, environmental clustering, latent potential estimation


**Key Outputs**:
- Outlet-level behavioral features (volume stats, revenue, diversity metrics)
- Monthly aggregation and growth trends
- Seasonality signals
- Cooler capacity operational signals
- Spatial enrichment (nearby schools, bus stops, hospitals within 500m)
- Environmental clustering (K-means, 8 clusters)
- Peer benchmarking (cluster ceilings at 95th percentile)
- Conservative uncensoring (70% recovery factor)
- Spatial demand adjustment (+1% per POI)

**Files Generated**:
- `data/gold/InferaX_predictions.csv` — ⭐ **PRIMARY SUBMISSION ARTIFACT**
  - Outlet_ID, Maximum_Monthly_Liters
  - 20,000+ rows with latent potential estimates
- `data/gold/outlet_feature_table.csv` — Complete feature engineering intermediate
  - 30+ features including cluster assignments, gaps, spatial scores

**Methodology Summary**:

1. **Load Silver Data**: Read cleaned transactions, outlet master, coordinates, seasonality, holidays
2. **Feature Engineering**: Aggregate transaction-level data to outlet-month, outlet-level metrics
3. **Spatial Intelligence**: Query OpenStreetMap for schools, bus stops, hospitals; compute 500m catchment POI counts
4. **Environmental Clustering**: K-means on 6 features (POIs + cooler + SKU diversity + avg volume)
5. **Peer Benchmarking**: Identify 95th percentile performer in each cluster as ceiling
6. **Uncensoring**: Estimate potential = current + 0.7 × (ceiling – current)
7. **Spatial Adjustment**: Multiply potential by (1 + 0.01 × spatial_score)
8. **Gold Layer Output**: Save Outlet_ID and Maximum_Monthly_Liters to CSV

**Key Parameters**:
- Environmental clusters: 8
- Catchment buffer: 500 meters
- Peer benchmarking percentile: 95th
- Recovery factor: 0.70 (conservative)
- Spatial adjustment rate: 0.01 (1% per POI)

**Quality Checks**:
- Verify outlet counts match master data
- Check for nulls in final predictions (should be minimal)
- Validate potential > average volume for most outlets (demand recovery signal)
- Confirm Gold layer file generated and readable

---

## Generated Files Reference

After executing all three notebooks, your `data/` directory will contain:

```
data/
├── bronze/                                    # INPUT (user-provided)
│   ├── transactions_history_final.csv         [2.376M rows]
│   ├── outlet_master.csv                      [20K+ rows]
│   ├── outlet_coordinates.csv                 [20K+ rows]
│   ├── distributor_seasonality_details.csv    [~1.2K rows]
│   └── holiday_list.csv                       [~36 rows]
│
├── silver/                                    # CLEANED (auto-generated by notebooks 1-2)
│   ├── transactions_clean.csv                 [2.371M rows] ✓ Notebook 2
│   ├── outlet_master_clean.csv                [20K+ rows] ✓ Notebook 2
│   ├── outlet_coordinates_clean.csv           [20K+ rows] ✓ Notebook 2
│   ├── seasonality_clean.csv                  [~1.2K rows] ✓ Notebook 2
│   └── holiday_clean.csv                      [~36 rows] ✓ Notebook 2
│
├── gold/                                      # DELIVERABLES (auto-generated by notebook 3)
│   ├── InferaX_predictions.csv                [20K+ rows] ✓ Notebook 3 ⭐ MAIN OUTPUT
│   └── outlet_feature_table.csv               [20K+ rows] ✓ Notebook 3
│
├── rejected/                                  # QUALITY AUDIT (auto-generated by notebook 2)
│   └── rejected_records.csv                   [241.7K rows] ✓ Notebook 2
│
└── reports/                                   # METRICS (auto-generated by notebook 2)
    └── data_quality_summary.csv               [8 rows summary] ✓ Notebook 2
```

---

## Key Output Interpretation

### InferaX_predictions.csv (Primary Deliverable)

This file contains your competition submission:

| Column | Type | Meaning | Example |
|--------|------|---------|---------|
| Outlet_ID | string | Unique outlet identifier | OUT_00123 |
| Maximum_Monthly_Liters | float | Latent monthly purchase potential (liters) | 127.3 |

**Interpretation**:
- Current monthly average for outlet: 80 liters (from historical data)
- Maximum monthly potential: 127 liters (estimated under optimized supply)
- Implied opportunity: 47-liter monthly uplift if operational constraints relaxed
- Recommendation: Consider cooler upgrade, increased allocation, promotional support

**Distribution Characteristics**:
- Mean potential: ~95 liters (varies by cluster)
- Median potential: ~88 liters
- Range: 30–450+ liters
- Cluster variance: High-context urban outlets peak higher than rural outlets

### Outlet Feature Table (Intermediate Reference)

This file contains complete feature engineering for audit and future modeling:

**Behavioral Features**:
- Total_Volume, Avg_Volume, Max_Volume, Volume_STD
- Total_Revenue, Avg_Revenue
- SKU_Diversity, Distributor_Diversity
- Active_Months, Growth_Rate, Seasonality_STD

**Operational Features**:
- Cooler_Count, Has_Cooler
- Outlet_Size (from master)

**Spatial Features**:
- Nearby_Schools, Nearby_Bus_Stops, Nearby_Hospitals
- Spatial_Score (sum of POIs)

**Clustering Features**:
- Cluster (0–7, K-means assignment)
- Cluster_Potential_Ceiling (95th percentile of cluster)
- Potential_Gap (ceiling – current)
- Estimated_Potential (before spatial adjustment)

**Use Cases**:
- Feature importance analysis
- Cluster profiling
- Validation of potential estimates
- Feature selection for future ML models

---

## Reproducibility & Validation

### Ensuring Reproducible Results

1. **Random Seeds**: All random processes (K-means clustering) use `random_state=42` for deterministic results
2. **Stable Versions**: `requirements.txt` pins specific package versions
3. **Data Versioning**: Bronze layer files should remain unchanged; Silver/Gold are regeneratable
4. **Notebook Execution Order**: Must execute Notebooks 1→2→3 sequentially

### Rerunning the Analysis

To regenerate all outputs from scratch:

```bash
# Activate environment
source venv/bin/activate

# Remove previous outputs (optional)
rm -rf data/silver/* data/gold/* data/rejected/* data/reports/*

# Launch Jupyter and re-execute all three notebooks in order
jupyter notebook
```


### Validation Checklist

After running all notebooks, verify:

- [ ] `data/silver/` contains 5 CSV files (cleaned data)
- [ ] `data/rejected/rejected_records.csv` exists (241.7K rows)
- [ ] `data/gold/InferaX_predictions.csv` exists (20K+ rows, 2 columns)
- [ ] `data/gold/outlet_feature_table.csv` exists (20K+ rows, 30+ columns)
- [ ] `data/reports/data_quality_summary.csv` contains rejection counts
- [ ] No nulls in `Maximum_Monthly_Liters` column (or documented if present)
- [ ] Potential estimates > average volume for 80%+ of outlets (demand recovery signal)
- [ ] Clusters 0–7 represented in outlet assignments










