# 🚔 NZ Stolen Vehicle Intelligence — Power BI Dashboard

> **New Zealand Police Data · Oct 2021 – Apr 2022 · 4,553 Records**  
> An end-to-end analytical solution transforming raw crime records into a multi-layered intelligence system for vehicle theft detection, regional risk profiling, and temporal pattern analysis.

---

## 📌 Project Summary

This dashboard was built to go beyond surface-level crime reporting. Rather than simply visualising stolen vehicle counts, the solution applies **per-capita normalisation, Pareto classification, rolling average trend lines, and a composite Risk Score model** to surface insights that raw counts systematically hide.

A key design decision: Auckland shows 1,638 thefts (the highest in absolute terms), but **Gisborne records 338 thefts per 100,000 people** — nearly **4× Auckland's per-capita rate**. Without normalisation, Gisborne's risk would be invisible to policymakers. This distinction between volume and density is the analytical thread running through the entire report.

---

## 📊 Dashboard Pages & Analytical Approach

### 1. 🗺️ Overview
The executive summary page — designed for a senior stakeholder who needs situational awareness in under 60 seconds.

| KPI | Value |
|-----|-------|
| Total Stolen Vehicles | 4,553 |
| Theft Rate | 0.09% of total population |
| Theft per 100k Population | 88.87 |
| Highest Volume Region | Auckland (1,638 incidents) |
| Top Targeted Brand | Toyota (716 thefts — 43% share) |
| Highest Risk Age Group | 18–25 years (2,916 incidents) |
| Risk Score (Composite) | 0.49 |

**Design rationale:** KPI cards are paired with a live theft trend line and a regional bar chart so viewers can simultaneously assess magnitude (how many?) and trajectory (is it getting worse?). A brand % share donut separates Standard (95.81%) from Luxury (4.19%) vehicle categories — a split that matters for insurance risk modelling.

---

### 2. 📍 Regional Analysis
The most analytically nuanced page. Raw stolen counts are intentionally presented **alongside** a per-100k normalised rate to demonstrate why regional comparisons require population adjustment.

**Key finding — the normalisation effect:**

| Region | Raw Count | Theft per 100k | Rank Shift |
|--------|-----------|----------------|------------|
| Auckland | 1,638 | 96.63 | Drops from #1 to #6 |
| Gisborne | 176 | **337.81** | Rises to #1 |
| Nelson | 92 | 168.81 | Rises to #2 |
| Bay of Plenty | 446 | 128.27 | #3 |
| Southland | 26 | 25.39 | Lowest risk |

A **vehicle type × region matrix** allows analysts to identify theft concentration patterns — Auckland + Stationwagon is the highest-risk intersection, while Canterbury skews disproportionately toward Trailer thefts, indicating a distinct regional theft profile.

---

### 3. 📅 Temporal Analysis
Time-series analysis across multiple granularities to identify when theft risk peaks — actionable intelligence for patrol scheduling and prevention campaigns.

**Key findings:**
- **March is the peak month** — 1,053 thefts, a +26% spike over February, coinciding with back-to-school and increased road activity
- **Monday is the highest-risk day** — 767 thefts, consistent with a behavioural pattern: vehicles parked over the weekend are targeted Monday morning when owners are less alert
- **Q1 (Jan–Mar) accounts for 56.14%** of all thefts in the dataset period — indicating a clear seasonal concentration
- A **4-week rolling average** overlaid on the monthly bar chart smooths short-term noise and confirms an escalating trend through Q1 2022
- A **waterfall chart** showing month-over-month increases/decreases makes directional shifts instantly readable without requiring the viewer to mentally calculate deltas

---

### 4. 🚗 Vehicle Analysis
Focused on answering: *which vehicles, which colours, and does the 80/20 rule hold for theft?*

**Pareto Analysis (80/20 Rule applied to vehicle makes):**
The top 5 brands — Toyota, Trailer, Nissan, Mazda, Ford — account for the majority of thefts, confirming that the Pareto principle holds in vehicle crime data. This has practical implications: targeted anti-theft campaigns concentrated on Toyota owners alone would address a substantial portion of total incidents.

**Colour distribution:**

| Colour | Count | Share |
|--------|-------|-------|
| Silver | 1,272 | 27.94% |
| White | 934 | 20.51% |
| Black | 589 | 12.94% |
| Blue | 512 | 11.25% |
| Red | 390 | 8.57% |

Silver and White together account for nearly **50% of all stolen vehicles** — a finding relevant to both law enforcement (BOLO profiles) and insurers (risk-adjusted premiums by colour).

---

## 🛠️ Technical Implementation

### Tools & Technologies
- **Power BI Desktop** — report authoring, data modelling, and visualisation
- **DAX (Data Analysis Expressions)** — all KPIs, calculated columns, and measures
- **Power Query (M Language)** — data ingestion, cleaning, and transformation
- **Microsoft Excel** — source data management

### DAX Measures Developed
- **Theft per 100k** — `DIVIDE([Stolen Count], [Region Population], 0) * 100000` normalised across 16 regions
- **4-Week Rolling Average** — time-intelligence measure using `DATESINPERIOD` to smooth monthly volatility
- **Composite Risk Score** — weighted index combining theft rate, young-driver proportion, and brand concentration
- **Pareto Cumulative %** — running total using `RANKX` and `CALCULATE` for the 80/20 brand analysis
- **Month-over-Month Change** — `PARALLELPERIOD` measure driving the waterfall chart directionality
- **Brand % Share** — dynamic share calculation responsive to all slicer selections

### Data Model
- Star schema with a central fact table (theft records) connected to dimension tables for Region, Date, Vehicle Type, and Brand
- Inactive relationships managed via `USERELATIONSHIP` in DAX to support multi-axis date filtering
- Cross-filter direction carefully configured to prevent ambiguity in matrix visuals

---

## 💡 Key Analytical Decisions

**1. Normalisation as a first principle**  
Every regional comparison is presented with a per-capita rate alongside raw counts. This was a deliberate design choice to prevent the dashboard from reinforcing the misleading narrative that larger regions are automatically higher risk.

**2. Rolling average over raw monthly bars**  
Raw monthly counts create visual noise. The 4-week rolling average reveals the true escalation trend that a simple bar chart obscures — a technique borrowed from financial time-series analysis.

**3. Pareto chart for brand prioritisation**  
Rather than listing all 25+ vehicle brands in a flat bar chart, the Pareto view immediately communicates where 80% of the problem is concentrated — making the insight actionable for resource allocation decisions.

**4. Risk Score as a composite signal**  
A single composite metric (0.49 Risk Score) aggregates theft rate, age-group vulnerability, and brand concentration. This allows the dashboard to be used as a monitoring tool — the score can be tracked over time as a leading indicator rather than requiring analysts to manually synthesise multiple charts.

---

## 📁 Repository Structure

```
NZ-Stolen-Vehicle-Dashboard/
│
├── NZ_Stolen_Vehicle_Intelligence.pbix     # Power BI report file
├── NZ_Stolen_Vehicle_Dashboard.pdf         # Full dashboard export (all 4 pages)
├── Dataset_Motor_Theft_NZ.xlsx             # Source dataset (NZ Police, 4,553 records)
└── README.md                               # Project documentation
```

---

## 📥 How to Explore the Dashboard

1. Download the `.pbix` file from this repository
2. Open with **Power BI Desktop** (free — [download here](https://powerbi.microsoft.com/desktop/))
3. Use the **Vehicle Type**, **Year**, and **Month** slicers to dynamically filter all visuals
4. Navigate across the 4 report pages using the tab bar at the bottom

---

## 🔍 What This Project Demonstrates

- Translating a raw crime dataset into a structured, multi-page analytical product
- Building DAX measures that go beyond simple aggregations (rolling averages, Pareto logic, composite indices)
- Making conscious design decisions about when to normalise data and why
- Communicating insight narratives alongside visualisations — not leaving the interpretation to the viewer
- Applying real analytical frameworks (80/20 rule, per-capita risk indexing) to a domain dataset




