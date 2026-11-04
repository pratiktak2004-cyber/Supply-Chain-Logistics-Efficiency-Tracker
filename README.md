# Supply Chain & Logistics Efficiency Tracker

An end-to-end enterprise data analytics project building a relational data pipeline and executive performance dashboard using MySQL Workbench and Power BI Desktop.

---

### Live Preview & Dashboard
* **Explore the Interactive Report / App:** [View Live Dashboard Link Here](https://app.powerbi.com/groups/me/reports/1253426f-1738-42b8-88cd-206d8e9a4f1f/10ca1f6e1d82d7e16a4e?experience=power-bi)

---

### Dashboard Preview
*Here is a snapshot of the completed executive Power BI canvas:*

![Dashboard Preview](Screenshot%20(635).png)
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
