Markdown# Supply Chain & Logistics Efficiency Tracker

An end-to-end enterprise data analytics project building a relational data pipeline and executive performance dashboard using **MySQL Workbench**, **ODBC connectivity**, and **Power BI Desktop**.

---

## Executive Summary
This project transforms unstructured transactional data into an optimized relational database schema and an executive dashboard. Designed to solve operational visibility challenges in global supply chains, the interactive dashboard monitors overall inventory movement, shipping delays, regional demand distribution, and carrier delivery reliability across global territories.

---

## Architecture & Data Pipeline

[ Kaggle DataCo CSV Dataset ]
│
▼
[ MySQL Staging Database ] ── (SQL Normalization & Indexing)
│
▼
[ Relational Schema ] ──────► warehouses ──┐
products   ──┼─► [ Power BI Star Schema ]
orders     ──┤               │
shipments  ──┘               ▼
[ Executive Dashboard ]
(ODBC DSN Direct Import)
1. **Database Layer (MySQL Workbench):**
   * Imported raw transactional records (`DataCoSupplyChainDataset.csv`) into a staging table (`staging_supply_chain`).
   * Cleaned character encodings and normalized staging data into a 3rd Normal Form (3NF) relational database schema:
     * `warehouses` (Warehouse metadata & region mapping)
     * `products` (Product categories & pricing attributes)
     * `orders` (Customer orders & item quantities)
     * `shipments` (Shipping modes, actual/scheduled days, & delivery outcomes)
   * Enforced foreign key constraints, optimal data types, and primary key indexing for fast relational queries.

2. **Data Modeling & Analytics Layer (Power BI Desktop):**
   * Established a live database link via a system **ODBC DSN** (`SupplyChainDSN`).
   * Configured a clean star schema data model:
     * `products` (1) ── (M) `orders`
     * `orders` (1) ── (M) `shipments`
     * `warehouses` (1) ── (M) `shipments`
   * Calculated custom DAX metrics for real-time logistics tracking.

---

## Key Performance Indicators & DAX Measures

* **Total Quantity:**
  ```dax
  Total Quantity = SUM(orders[order_item_quantity])
Average Shipping Delay (Days):Code snippetAvg Shipping Delay = AVERAGE(shipments[actual_shipping_days]) - AVERAGE(shipments[scheduled_shipping_days])
On-Time Delivery Rate (%):Code snippetOn-Time Delivery Pct = 
DIVIDE(
    CALCULATE(COUNT(shipments[shipment_id]), shipments[delivery_status] = "Shipping on time"),
    COUNT(shipments[shipment_id]),
    0
)
Visual Canvas BreakdownVisual ComponentChart TypeKey Insights & Operational ValueExecutive KPIsSingle Value CardsImmediate visibility into Total Quantity, Avg Shipping Delay (Days), and On-Time Delivery %.Regional DemandHorizontal Bar ChartTotal Quantity by Destination Region highlights high-demand market hubs (Europe, LATAM, Pacific Asia, USCA, Africa).Delivery ReliabilityDonut ChartShipment Distribution by Shipping Mode / Delivery Status categorizes order fulfillment outcomes (Late Delivery, Advance Shipping, Shipping On Time, Shipping Canceled).Corridor PerformanceOperational TableDelivery Performance & Warehouse Bottlenecks details granular throughput across fulfilling warehouses, shipping dates, delivery statuses, and unit volumes.🛠️ Tech Stack & ToolsDatabase Engine: MySQL Workbench 8.0Data Connectivity: MySQL Connector/ODBC 8.0 (DSN Configuration)Data Modeling & Visualization: Power BI DesktopLanguage/Logic: SQL (DDL/DML), DAX (Data Analysis Expressions)Dataset: DataCo Global Supply Chain Dataset (Kaggle)🚀 How to Run & Replicate1. Database Setup (MySQL)Open MySQL Workbench and create the database:SQLCREATE DATABASE supply_chain_analytics;
USE supply_chain_analytics;
Import DataCoSupplyChainDataset.csv into staging_supply_chain.Run sql/02_schema_normalization.sql to execute schema creation, indexing, and data population across warehouses, products, orders, and shipments.2. ODBC Connection SetupSearch for ODBC Data Sources (64-bit) in Windows.Under User DSN or System DSN, click Add and select MySQL ODBC 8.0 Unicode Driver.Name the DSN SupplyChainDSN, enter your MySQL host (localhost), port (3306), user credentials, and select the supply_chain_analytics database.3. Dashboard Launch (Power BI)Open dashboard/Supply_Chain_Analytics.pbix in Power BI Desktop.If prompted for data credentials, select DSN / Database, choose SupplyChainDSN, and click Apply Changes.📂 Repository StructurePlaintext├── sql/
│   ├── 01_staging_load.sql          # Staging database creation & CSV loading
│   └── 02_schema_normalization.sql # Relational schema, normalization DDL, and foreign keys
│
├── dashboard/
│   ├── Supply_Chain_Analytics.pbix  # Main Power BI Desktop file
│   └── dashboard_preview.png        # Screenshot of the completed executive canvas
│
├── docs/
│   └── data_dictionary.md           # Schema definitions and data model key mappings
│
└── README.md                        # Project documentation# Supply-Chain-Logistics-Efficiency-Tracker
End-to-end supply chain analytics pipeline using MySQL for relational data modeling and Power BI Desktop (via ODBC DSN) for executive performance tracking.
