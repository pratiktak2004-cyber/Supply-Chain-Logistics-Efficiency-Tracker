## System Architecture & Data Flow

```mermaid
graph TD
    %% Node Styling
    classDef source fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef mysql fill:#e8f4f8,stroke:#00758f,stroke-width:2px;
    classDef powerbi fill:#fff3e0,stroke:#f2c811,stroke-width:2px;
    classDef visual fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;

    %% Data Ingestion
    RAW[Kaggle Supply Chain CSV]:::source -->|Import & Load| STAGE[(MySQL Staging Table)]:::mysql

    %% Data Engineering / Normalization
    subgraph MySQL_Tier ["Data Engineering Tier (MySQL 8.0) "]
        STAGE -->|Schema Normalization| W[(warehouses)]:::mysql
        STAGE -->|Schema Normalization| P[(products)]:::mysql
        STAGE -->|Schema Normalization| O[(orders)]:::mysql
        STAGE -->|Schema Normalization| S[(shipments)]:::mysql
    end

    %% Relational Connections
    W -.->|FK Relationship| S
    P -.->|FK Relationship| O
    O -.->|FK Relationship| S

    %% BI Pipeline
    subgraph PowerBI_Tier ["Data Modeling & Analytics Tier (Power BI) "]
        ODBC[64-bit System ODBC DSN]:::powerbi
        MODEL[Star Schema & DAX Measures]:::powerbi
    end

    S ==>|ODBC Connection| ODBC
    ODBC --> MODEL

    %% Dashboard Output
    subgraph Dashboard_Tier ["Executive Dashboard Visuals "]
        KPI[KPI Cards]:::visual
        BAR[Regional Demand Bar Chart]:::visual
        DONUT[Delivery Reliability Donut Chart]:::visual
        TBL[Warehouse Bottlenecks Table]:::visual
    end

    MODEL --> KPI
    MODEL --> BAR
    MODEL --> DONUT
    MODEL --> TBL
