# CUSTEM ARCHITECTURE
CUSTEM (Customer Segmentation, Benchmarking and Forecasting Stack) is a Python-based marketing analytics system designed to turn transactional data into actionable insights.  

The architecture is deliberately structured as a **four-layer stack**:

1. **Data layer** – synthetic transactional data and data quality checks  
2. **Modelling layer** – segmentation and forecasting models  
3. **Observability layer** – experiment tracking and configuration management  
4. **Presentation layer** – visuals and analyst-facing notebooks

Each layer can evolve independently but is kept small and focused so the project remains achievable within the dissertation timeframe.

---

## 1. Data Layer

**Purpose:** Provide realistic but GDPR-safe transactional data and a clean feature set for modelling.

**Responsibilities**

- Define the schema for the synthetic dataset, including:
  - `customer_id`
  - `txn_timestamp`
  - `amount`
  - `channel` (e.g. online / in-store)
  - `category` (e.g. food, fashion, leisure)
  - optional customer attributes (e.g. local / visitor flags)
- Generate synthetic data using Python (NumPy / pandas and, where feasible, SDV/CTGAN) to mimic realistic distributions and behaviour.
- Store datasets under version-controlled paths:
  - `data/raw/` – direct synthetic output
  - `data/processed/` – cleaned and feature-engineered tables
- Run and document basic data quality checks:
  - missingness and outlier inspection  
  - schema validation (types, ranges, allowed categories)
  - sanity checks on volumes and time coverage

**Outcomes**

- A reproducible synthetic dataset that is safe to publish.  
- Clear evidence that the data is suitable for segmentation and forecasting.

---

## 2. Modelling Layer

**Purpose:** Learn patterns in customer behaviour and generate forward-looking insights.

**Responsibilities**

- **Segmentation**
  - Engineer features such as recency, frequency, monetary value (RFM) and category/channel preferences.
  - Implement at least one clustering algorithm (K-means) plus a comparator (e.g. Gaussian Mixture Models or DBSCAN).
  - Evaluate and select cluster solutions using metrics such as Silhouette score and Davies–Bouldin index.
  - Produce interpretable segment profiles (size, spend, behaviour patterns).

- **Forecasting / Prediction**
  - Define a clear forecasting question (e.g. weekly revenue per segment, or likelihood of customer inactivity).
  - Build a baseline model and at least one stronger model (e.g. Prophet, regression, or tree-based estimator).
  - Evaluate models with appropriate metrics (e.g. RMSE, MAE, accuracy/AUC where relevant).
  - Compare performance across segments or scenarios.

**Outcomes**

- A small but coherent library of segmentation and forecasting models.  
- Quantitative evidence of model performance and limitations.

---

## 3. Observability Layer

**Purpose:** Make experiments reproducible, comparable and auditable.

**Responsibilities**

- Use **Trackio** for experiment logging, with **Weights & Biases (W&B)** as an API-compatible fallback.
- For each significant run, record:
  - model type and hyperparameters
  - data version and feature set
  - evaluation metrics (e.g. Silhouette, RMSE)
  - notes on any anomalies or findings
- Store configuration files and logs under `experiments/` so runs can be re-created.

**Outcomes**

- A transparent trail of how models were developed and tuned.  
- Screenshots and logs that can be referenced in the dissertation to evidence systematic experimentation.

---

## 4. Presentation Layer

**Purpose:** Communicate insights to human stakeholders (and examiners) in a clear, interpretable way.

**Responsibilities**

- Build Jupyter notebooks (in `notebooks/`) that:
  - walk through EDA and data quality checks  
  - explain the segmentation logic and show segment profiles  
  - present forecasting results and comparisons
- Produce high-quality static visualisations using matplotlib / seaborn / plotly, such as:
  - distribution plots and heatmaps  
  - cluster scatter plots and RFM charts  
  - forecast curves and error plots
- Optionally, add light interactivity (e.g. ipywidgets to change `k` or filter segments) if time allows.

**Outcomes**

- A set of analyst-facing notebooks that double as reproducible analysis and as the basis for report figures.  
- Visual evidence of how CUSTEM could support real marketing decision-making.

---

## 5. Scope: Must, Should, Could

**Must-have**

- Synthetic dataset and EDA  
- At least one validated clustering model (plus evaluation)  
- At least one forecasting or predictive model (plus evaluation)  
- Basic experiment tracking for core runs  
- Clear visualisations and written interpretation

**Should-have**

- A second clustering algorithm as a comparator  
- A more advanced forecasting approach (e.g. Prophet or tree-based model)  
- Segment-level analysis of forecast outcomes  
- More structured experiment configs (YAML/JSON)

**Could-have (stretch goals)**

- Additional data sources or features (e.g. simple “web” or “campaign” variables)  
- A lightweight interactive front-end (e.g. Streamlit or Voila)  
- More advanced evaluation (e.g. stability analysis, what-if simulations)

**Out of scope**

- Production-grade MLOps pipelines  
- Live deployment into enterprise systems  
- Use of real client data (all examples are synthetic or aggregated)

---

## 6. Repository Structure (planned)

```text
custem-analytics/
├── data/
│   ├── raw/
│   └── processed/
├── notebooks/
│   ├── 01-eda.ipynb
│   ├── 02-segmentation.ipynb
│   └── 03-forecasting.ipynb
├── SRC/
│   ├── data_gen.py
│   ├── features.py
│   ├── segmentation.py
│   └── forecasting.py
├── experiments/
│   ├── configs/
│   └── logs/
└── README.md

# SCOPE (MoSCoW)

**Must have**
- lorem
- ipsum

**Should have**
- lorem
- ipsum

**Could have**
- lorem
- ipsum

**Out of scope**
- lorem
- ipsum