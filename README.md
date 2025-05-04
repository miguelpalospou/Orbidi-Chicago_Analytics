# GCP Chicago Weather Analytics Pipeline

This project sets up a serverless data pipeline using:
- **Terraform** for infrastructure on GCP
- **Cloud Functions** for scheduled data ingest
- **BigQuery** for storage and analysis
- **DBT** for transforming raw weather data into analytics-ready models

---

## 💡 Project Overview

### 🔧 Infrastructure
- Deploys datasets in BigQuery (`raw`, `chicago_analytics_staging`, `chicago_analytics_marts`)
- Cloud Function to pull weather data from API (e.g., NOAA or other)
- Cloud Scheduler to run the function daily
- IAM roles for secure access

### 🧠 Data Flow
1. Cloud Scheduler triggers Cloud Function
2. Cloud Function fetches weather data and writes to `raw.weather_daily`
3. DBT transforms data in `staging/` and `marts/` layers

📐 Architecture Overview

+------------------------+
| Cloud Scheduler        |
| (Daily Trigger)        |
+-----------+------------+
            |
            v
+------------------------+
| Cloud Function         |
| - Python + pandas      |
| - Ingest weather data  |
| - Push to BigQuery     |
+-----------+------------+
            |
            v
+------------------------+
| BigQuery               |
| - raw.weather_daily    |
| - staging/marts models |
+-----------+------------+
            |
            v
+------------------------+
| dbt                    |
| - Transformations      |
| - Exposures, tests     |
+------------------------+

gcp-terraform-project/
├── function-code/              # Python Cloud Function source
│   ├── main.py
│   ├── requirements.txt
│   └── weather_function.zip
├── modules/                    # Terraform modules
│   ├── datasets/               # BigQuery datasets and tables
│   ├── function/               # Cloud Function resource
│   ├── scheduler/              # Cloud Scheduler job
│   ├── iam/                    # IAM bindings
│   └── project_services/       # API service enablement
├── chicago-dbt/                # dbt models (optional if separate)
│   ├── models/
│   └── dbt_project.yml
├── main.tf                     # Terraform root config
├── variables.tf
├── outputs.tf
├── terraform.tfvars
└── README.md

---


### 🛠 Prerequisites
- Terraform CLI
- Python 3.10
- gcloud SDK
- DBT (BigQuery adapter)

### 🌍 Set Environment
```bash
export GOOGLE_CLOUD_PROJECT="your-project-id"
export GOOGLE_REGION="us-central1"

### 📦 Deploy Infrastructure

cd terraform
terraform init
terraform apply

### Cloud Function Packaging

cd function-code
zip -r ../weather_function.zip main.py requirements.txt
cd ..

### Run DBT

cd chicago-dbt
dbt run





