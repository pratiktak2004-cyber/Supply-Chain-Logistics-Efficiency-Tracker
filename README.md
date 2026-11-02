graph TD
    %% Node Styling
    classDef source fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef mysql fill:#e8f4f8,stroke:#00758f,stroke-width:2px;
    classDef powerbi fill:#fff3e0,stroke:#f2c811,stroke-width:2px;
    classDef visual fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;

    %% Data Source & Staging
    A[📄 Raw Kaggle CSV<br>DataCoSupplyChainDataset]:::source -->|Data Ingestion| B[(🗄️ MySQL Staging<br>staging_supply_chain)]:::mysql

    %% Database Normalization
    subgraph MySQL_Database [" 🛢️ Relational Database Tier (supply_chain_analytics) "]
        B -->|SQL Normalization & DDL| C[(warehouses)]:::mysql
        B -->|SQL Normalization & DDL| D[(products)]:::mysql
        B -->|SQL Normalization & DDL| E[(orders)]:::mysql
        B -->|SQL Normalization & DDL| F[(shipments)]:::mysql
    end

    %% Data Modeling & Connectivity
    C -->|1:M Foreign Key| F
    D -->|1:M Foreign Key| E
    E -->|1:M Foreign Key| F

    %% Power BI Layer
    subgraph PowerBI_Analytics [" 📊 Analytics & BI Tier (Power BI Desktop) "]
        G[🔌 Windows DSN / ODBC<br>SupplyChainDSN] :::powerbi
        H[⭐ Star Schema Data Model]:::powerbi
        I[🧮 DAX Calculation Engine<br>Total Qty, Delay, On-Time %]:::powerbi
    end

    F ==>|Direct Import via ODBC| G
    G --> H
    H --> I

    %% Executive Visual Canvas
    subgraph Executive_Dashboard [" 📈 Visual Canvas Components "]
        J[📌 KPI Single Cards]:::visual
        K[📊 Bar Chart: Regional Demand]:::visual
        L[🍩 Donut Chart: Delivery Status]:::visual
        M[📋 Table: Warehouse Bottlenecks]:::visual
    end

    I --> J
    I --> K
    I --> L
    I --> M
