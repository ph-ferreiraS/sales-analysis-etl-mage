# 📦 Olist E-commerce Logistics Analytics Platform

E-commerce Logistics Analytics Platform. A full-cycle data project (ETL → Dashboard) using Mage.ai, MinIO, and Streamlit. Features an AI-powered assistant (LangChain) to query shipping KPIs.

## 🏗️ Architecture Overview

This project implements a **Medallion Architecture** (Bronze → Silver → Gold) for processing and analyzing Olist e-commerce logistics data.

### Medallion Architecture Layers

#### 🥉 Bronze Layer (Raw Data)
- **Purpose**: Landing zone for raw data ingestion
- **Description**: Stores data in its original format as received from source systems
- **Data Format**: Raw CSV/JSON files from Olist dataset
- **Location**: MinIO bucket `s3://bronze/`
- **Characteristics**:
  - No transformations applied
  - Complete historical record
  - Append-only operations
  - Data lineage tracking enabled

#### 🥈 Silver Layer (Cleaned & Validated)
- **Purpose**: Cleaned and conformed data
- **Description**: Data is validated, deduplicated, and standardized
- **Transformations**:
  - Data type conversions
  - Null value handling
  - Duplicate removal
  - Schema enforcement
  - Data quality checks
- **Location**: MinIO bucket `s3://silver/`
- **Characteristics**:
  - Consistent schema across datasets
  - Filtered invalid records
  - Optimized storage format (Parquet)

#### 🥇 Gold Layer (Analytics-Ready)
- **Purpose**: Business-level aggregations and features
- **Description**: Curated datasets optimized for analytics and reporting
- **Use Cases**:
  - Delivery performance metrics
  - Customer segmentation
  - Product analytics
  - Geographic insights
  - Time-series analysis
- **Location**: MinIO bucket `s3://gold/`
- **Characteristics**:
  - Denormalized for query performance
  - Pre-calculated metrics
  - Business-friendly naming
  - Optimized for dashboard consumption

## 🛠️ Technology Stack

- **ETL Orchestration**: [Mage.ai](https://www.mage.ai/) - Modern data pipeline tool
- **Database**: PostgreSQL - Backend for Mage.ai metadata
- **Object Storage**: [MinIO](https://min.io/) - S3-compatible storage for data lakes
- **Dashboard**: [Streamlit](https://streamlit.io/) - Interactive analytics dashboard
- **AI Assistant**: [LangChain](https://www.langchain.com/) + [OpenAI](https://openai.com/) - Natural language queries for logistics KPIs
- **Data Processing**: pandas, pyarrow - Data manipulation and processing

## 🚀 Quick Start

### Prerequisites
- Docker and Docker Compose installed
- At least 4GB of RAM available
- Ports 6789, 9000, 9001 available

### Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/ph-ferreiraS/sales-analysis-etl-mage.git
   cd sales-analysis-etl-mage
   ```

2. **Install Python dependencies** (optional, for local development):
   ```bash
   pip install -r requirements.txt
   ```

3. **Start the infrastructure**:
   ```bash
   docker-compose up -d
   ```

4. **Access the services**:
   - **Mage.ai**: http://localhost:6789
   - **MinIO Console**: http://localhost:9001 (user: `minio`, password: `minio123`)
   - **MinIO API**: http://localhost:9000

5. **Run the Streamlit dashboard**:
   ```bash
   streamlit run dashboard/app.py
   ```

## 📊 Data Flow

```
┌─────────────┐
│ Olist Data  │
│   Source    │
└──────┬──────┘
       │
       ▼
┌─────────────────────────────────────┐
│    🥉 Bronze Layer (Raw)            │
│  - Raw CSV/JSON ingestion           │
│  - No transformations               │
│  - Full historical data             │
└──────────┬──────────────────────────┘
           │ Mage.ai Pipeline
           ▼
┌─────────────────────────────────────┐
│    🥈 Silver Layer (Cleaned)        │
│  - Data validation                  │
│  - Type conversion                  │
│  - Deduplication                    │
│  - Quality checks                   │
└──────────┬──────────────────────────┘
           │ Mage.ai Pipeline
           ▼
┌─────────────────────────────────────┐
│    🥇 Gold Layer (Analytics)        │
│  - Business aggregations            │
│  - KPI calculations                 │
│  - Optimized for queries            │
└──────────┬──────────────────────────┘
           │
           ▼
┌─────────────────────────────────────┐
│    📊 Streamlit Dashboard           │
│  - Interactive visualizations       │
│  - AI-powered insights              │
│  - Logistics KPIs                   │
└─────────────────────────────────────┘
```

## 📁 Project Structure

```
sales-analysis-etl-mage/
├── docker-compose.yml          # Infrastructure setup (Mage, PostgreSQL, MinIO)
├── requirements.txt            # Python dependencies
├── README.md                   # Project documentation
├── dashboard/
│   └── app.py                 # Streamlit dashboard application
├── scripts/                   # Utility scripts for data operations
└── mage_data/                 # Mage.ai pipelines (created on first run)
    ├── pipelines/             # ETL pipeline definitions
    └── data_loaders/          # Data ingestion modules
```

## 🎯 Key Logistics Metrics

The platform tracks and visualizes the following KPIs:

- **Delivery Performance**:
  - Average delivery time
  - On-time delivery rate
  - Delay distribution

- **Geographic Analysis**:
  - Orders by state/city
  - Delivery routes optimization
  - Regional performance comparison

- **Product Insights**:
  - Most shipped product categories
  - Product delivery time by category
  - High-delay products

- **Customer Metrics**:
  - Customer satisfaction scores
  - Repeat purchase patterns
  - Geographic distribution

## 🤖 AI-Powered Analytics

The integrated AI assistant allows you to query your logistics data using natural language:

- "What is the average delivery time for orders in São Paulo?"
- "Which products have the highest delay rate?"
- "Show me the trend of on-time deliveries over the last 6 months"

Powered by LangChain and OpenAI, the assistant translates natural language into SQL queries and provides insights.

## 🔧 Configuration

### MinIO Buckets
Create the following buckets in MinIO:
- `bronze` - Raw data storage
- `silver` - Cleaned data storage
- `gold` - Analytics-ready data storage

### Environment Variables
Configure the following in `docker-compose.yml`:
- `MAGE_DATABASE_CONNECTION_URL` - PostgreSQL connection
- `AWS_ACCESS_KEY_ID` - MinIO access key
- `AWS_SECRET_ACCESS_KEY` - MinIO secret key

## 📚 Data Sources

This project uses the [Olist E-commerce Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce), which contains:
- Orders data
- Products information
- Customers details
- Sellers information
- Geolocation data
- Reviews and ratings

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📄 License

This project is open-source and available under the MIT License.

## 🔗 Resources

- [Mage.ai Documentation](https://docs.mage.ai/)
- [MinIO Documentation](https://min.io/docs/)
- [Streamlit Documentation](https://docs.streamlit.io/)
- [Medallion Architecture](https://www.databricks.com/glossary/medallion-architecture)
