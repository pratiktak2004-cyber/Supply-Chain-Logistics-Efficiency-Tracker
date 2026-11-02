[ Kaggle DataCo CSV ]
         │
         ▼
[ MySQL Staging Table ] ── (SQL Normalization)
         │
         ▼
┌────────────────────────────────────────┐
│ Relational Database (MySQL 8.0)        │
│ ├── warehouses                         │
│ ├── products                           │
│ ├── orders                             │
│ └── shipments (1:M Foreign Keys)       │
└────────────────────────────────────────┘
         │
         ▼ (ODBC DSN: SupplyChainDSN)
┌────────────────────────────────────────┐
│ Power BI Analytics Tier                │
│ ├── Star Schema Data Model             │
│ └── DAX Measures (Total Qty, Delay %)  │
└────────────────────────────────────────┘
         │
         ▼
┌────────────────────────────────────────┐
│ Executive Dashboard Visuals            │
│ ├── KPI Cards                          │
│ ├── Regional Demand Bar Chart          │
│ └── Delivery Reliability Donut Chart   │
└────────────────────────────────────────┘
