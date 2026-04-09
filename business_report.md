# Business Intelligence Report: NYC Taxi Operations Analysis

**Course:** DSA3050A – Business Intelligence & Data Visualization  
**Project:** Group A – New York City Taxi Rides Analysis  
**Date:** April 2026  

**Team Members**

| Name | Student ID (Last 3) |
| :--- | :--- |
| Tanveer Omar | 762 |
| Mohamed Mohamed | 006 |
| Mitchel Muthaura | 413 |
| Calvin Gacheru | 035 |
| Claire Mwarari | 470 |
| Lavender Nchagwa | 647 |

---

## Table of Contents

1. [Executive Summary](#1-executive-summary)
2. [Problem Statement](#2-problem-statement)
3. [Business Questions](#3-business-questions)
4. [Data Source & Scope](#4-data-source--scope)
5. [Data Pipeline: ETL & Transformations](#5-data-pipeline-etl--transformations)
6. [Data Architecture: Star Schema Model](#6-data-architecture-star-schema-model)
7. [DAX Measures & Calculated Logic](#7-dax-measures--calculated-logic)
8. [Dashboard Overview](#8-dashboard-overview)
9. [Key Findings & Insights](#9-key-findings--insights)
10. [Strategic Recommendations](#10-strategic-recommendations)
11. [Limitations & Next Steps](#11-limitations--next-steps)
12. [Conclusion](#12-conclusion)
13. [References](#13-references)

---

## 1. Executive Summary

New York City's taxi system is one of the world's most complex urban transportation networks. This report presents the findings of a Business Intelligence analysis conducted on over **50,000 NYC taxi trip records**, with the objective of uncovering actionable operational insights for fleet managers, dispatchers, and policy stakeholders.

**Key Takeaways:**

- **Insight:** Demand is highly concentrated in specific neighborhoods and time windows (peak hours), leaving the fleet over-supplied during off-peak periods.  
  **Impact:** Driver idle time and passenger wait times both increase when supply and demand are misaligned.  
  **Action:** Implement predictive rebalancing to shift driver availability toward historical demand peaks.

- **Insight:** A significant proportion of trips involve a single passenger, leaving vehicle capacity under-utilized.  
  **Impact:** Revenue per mile is suppressed, eroding the fleet's net margin.  
  **Action:** Introduce shared-ride incentive tiers to increase per-seat revenue.

- **Insight:** Cash transactions correlate with higher inter-trip idle time compared to digital payments.  
  **Impact:** Slower trip turnover reduces total trips-per-hour for drivers accepting cash.  
  **Action:** Offer incentives for digital payment adoption to increase operational velocity.

The analysis was conducted using Microsoft Power BI, with data modeled in a star schema and business logic implemented via DAX measures. An interactive dashboard supports real-time, slice-and-filter decision-making across all findings.

---

## 2. Problem Statement

NYC's taxi operations face a persistent challenge in **balancing supply and demand**. The mismatch between dynamic passenger demand and active vehicle supply creates two compounding problems:

1. **Driver Idle Time:** Taxis congregate in low-demand areas, causing extended periods without fares and reducing driver earnings.
2. **Passenger Wait Times:** High-traffic neighborhoods experience insufficient taxi coverage, especially during peak hours, leading to passenger abandonment and lost revenue.

To solve this, three critical operational variables must be analyzed and synchronized:

| Variable | Description |
| :--- | :--- |
| **Location** | Geographic hotspots for pickups vs. drop-offs across NYC neighborhoods |
| **Timing** | Hourly and day-of-week fluctuations in ride requests |
| **Trip Behavior** | Patterns in duration, distance, passenger count, and payment type |

**Project Goal:** To provide data-driven recommendations that improve taxi availability, reduce wait times, and maximize driver revenue through optimized routing and timing decisions.

---

## 3. Business Questions

This analysis was structured around seven core business questions that map directly to operational decision-making:

| # | Question | Theme |
| :- | :--- | :--- |
| 1 | What are the peak hours and days for taxi demand across different neighborhoods? | **Demand Timing** |
| 2 | Which neighborhoods generate the highest trip volumes and revenue? | **Location Performance** |
| 3 | How does trip distance correlate with fare amount and payment type? | **Fare & Payment** |
| 4 | What is the average trip duration and how does it vary by time of day? | **Operational Efficiency** |
| 5 | Which payment methods are most common and how do they vary by neighborhood? | **Fare & Payment** |
| 6 | How many passengers typically ride together, and does this affect trip distance? | **Vehicle Utilization** |
| 7 | What is the relationship between pickup and drop-off neighborhoods (commuter patterns)? | **Origin-Destination Flow** |

---

## 4. Data Source & Scope

**Dataset:** New York City Taxi Rides  
**Source:** Kaggle – Surekha Ramireddy (2023)  
**URL:** https://www.kaggle.com/datasets/surekharamireddy/new-york-city-taxi-rides  

### Source Files

| File | Type | Description |
| :--- | :--- | :--- |
| `trips_1.csv` | Fact | 50,000+ individual trip records |
| `pickup_neighborhoods.csv` | Dimension | Neighborhood lookup for trip origins |
| `dropoff_neighborhoods.csv` | Dimension | Neighborhood lookup for trip destinations |

### Key Data Fields

| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | String | Unique trip identifier |
| `vendor_id` | Integer | Taxi vendor code |
| `pickup_datetime` | Datetime | Trip start timestamp |
| `dropoff_datetime` | Datetime | Trip end timestamp |
| `passenger_count` | Integer | Number of passengers |
| `trip_distance` | Float | Distance traveled (miles) |
| `pickup_longitude / latitude` | Float | Pickup geolocation |
| `dropoff_longitude / latitude` | Float | Drop-off geolocation |
| `payment_type` | Integer | Payment code (1=Credit Card, 2=Cash, 3=No Charge, 4=Disputed) |
| `trip_duration` | Integer | Duration in seconds |
| `pickup_neighborhood` | FK | Neighborhood key from DIM table |
| `dropoff_neighborhood` | FK | Neighborhood key from DIM table |

---

## 5. Data Pipeline: ETL & Transformations

All data preparation was performed using **Power Query (M Language)** inside Microsoft Power BI. The pipeline covers the following transformation categories:

### 5.1 Initial Setup

- **Promoted Headers:** Set the first row of each CSV as column headers to ensure accurate field identification.
- **Renamed Queries:** Applied consistent naming conventions to align with the star schema design:
  - `trips_1` → `FACT_trips`
  - `pickup_neighborhoods` → `DIM_Pickup_hood`
  - `dropoff_neighborhoods` → `DIM_Dropoff_hood`

### 5.2 Column Management

- **Removed Redundant Columns:** Dropped `pickup_longitude`, `pickup_latitude`, `dropoff_longitude`, and `dropoff_latitude` from the Fact table, since geographic coordinates exist in the Dimension tables and are redundant in the Fact table.

### 5.3 Data Type Correction

- Corrected `pickup_datetime` and `dropoff_datetime` from text to DateTime format.
- Changed `passenger_count` from Float to Whole Number.

### 5.4 Column Splitting & Derivation

- **Date/Time Splitting:** Each datetime column was split into two separate columns:
  - `pickup_date` (Date only) and `pickup_time` (Time only)
  - `dropoff_date` (Date only) and `dropoff_time` (Time only)
- **Time Category (Conditional Column):** A text label column was created to classify each trip by time of day:
  - Morning Peak, Midday, Evening Peak, Night
- **Speed Column (Custom Column):** Derived using the formula:  
  `speed_mph = trip_distance / (trip_duration / 3600)`  
  This converts seconds to hours and calculates speed in MPH.

### 5.5 Data Quality

| Issue | Action Taken |
| :--- | :--- |
| **Null values** in neighborhood ID columns | Filtered out empty rows |
| **Zero-distance trips** | Excluded trips where `trip_distance = 0` (outliers that skew averages) |
| **Duplicate records** | Removed duplicate rows based on the unique `id` column |

### 5.6 Lookup Enrichment (Merge Queries)

- **Pickup Neighborhood Names:** Merged `FACT_trips` with `DIM_Pickup_hood` on `pickup_neighborhood` key to bring in neighborhood name as a readable column.
- **Dropoff Neighborhood Names:** Same process applied for drop-off neighborhoods.

### 5.7 Payment Type Decoding (Replace Values)

Payment codes were decoded into human-readable labels:

| Code | Label |
| :--- | :--- |
| 1 | Credit Card |
| 2 | Cash |
| 3 | No Charge |
| 4 | Disputed |

---

## 6. Data Architecture: Star Schema Model

To ensure high query performance and analytical flexibility, the data was structured as a **Star Schema**—a widely adopted dimensional modeling pattern in Business Intelligence.

### 6.1 Design Rationale

- **Fast Analytics:** Denormalized structure minimizes join complexity, enabling Power BI to evaluate measures rapidly.
- **Flexible Slicing:** Dimension tables allow filtering by date, time, neighborhood, or payment type without complex query logic.
- **Scalability:** New dimensions can be added (e.g., weather, events) without restructuring the Fact table.

### 6.2 Schema Structure

```
                    ┌─────────────┐
                    │  DIM_Date   │
                    └──────┬──────┘
                           │ 1:M
         ┌─────────────────┼─────────────────┐
         │                 │                 │
  ┌──────┴──────┐   ┌──────┴──────┐   ┌──────┴──────┐
  │  DIM_Time   │   │  FACT_trips │   │ DIM_Payment │
  └─────────────┘   └──────┬──────┘   └─────────────┘
                           │
              ┌────────────┴────────────┐
              │                         │
   ┌──────────┴──────────┐   ┌──────────┴──────────┐
   │ DIM_Pickup_hood     │   │ DIM_Dropoff_hood     │
   └─────────────────────┘   └─────────────────────┘
```

### 6.3 Table Definitions

**Fact Table: `FACT_trips`**

| Column | Description |
| :--- | :--- |
| `id` | Unique trip key |
| `vendor_id` | Taxi vendor |
| `pickup_date` / `pickup_time` | Temporal keys linking to DIM_Date and DIM_Time |
| `passenger_count` | Number of passengers |
| `trip_distance` | Miles traveled |
| `payment_type` | FK to DIM_Payment |
| `trip_duration` | Duration in seconds |
| `speed_mph` | Derived speed column |
| `pickup_neighborhood` | FK to DIM_Pickup_hood |
| `dropoff_neighborhood` | FK to DIM_Dropoff_hood |

**Dimension Tables**

| Table | Purpose | Key Fields |
| :--- | :--- | :--- |
| `DIM_Date` | Calendar hierarchy | Date, Year, Month, Day, Weekday, Quarter |
| `DIM_Time` | Hour-level analysis | Hour, TimeCategory (Peak/Off-Peak/Night) |
| `DIM_Payment` | Payment decoding | PaymentCode, PaymentDescription |
| `DIM_Pickup_hood` | Pickup geography | neighborhood_id, neighborhood_name |
| `DIM_Dropoff_hood` | Drop-off geography | neighborhood_id, neighborhood_name |

All dimension-to-fact relationships are **one-to-many**, with single-direction cross-filtering.

---

## 7. DAX Measures & Calculated Logic

Business logic was implemented using **DAX (Data Analysis Expressions)** in Power BI. The following measures and calculated tables were developed:

### 7.1 Calculated Dimension Tables

| Table | DAX Method | Description |
| :--- | :--- | :--- |
| `DIM_Date` | `CALENDAR` + `ADDCOLUMNS` | Auto-generates a full date table with Year, Month, Day, Weekday, Quarter columns |
| `DIM_Payment` | `DATATABLE` | Hardcoded lookup table mapping payment codes (1–4) to descriptive labels |

### 7.2 Core Measures

| Measure | DAX Formula | Business Purpose |
| :--- | :--- | :--- |
| **Total Trips** | `COUNTROWS(FACT_trips)` | Total count of valid taxi rides |
| **Total Distance** | `SUM(FACT_trips[trip_distance])` | Cumulative distance across all trips |
| **Average Passengers** | `AVERAGE(FACT_trips[passenger_count])` | Typical vehicle occupancy per trip |
| **Avg Duration (Mins)** | `AVERAGE(FACT_trips[trip_duration]) / 60` | Mean trip time in minutes |

### 7.3 Time Intelligence Measures

| Measure | DAX Logic | Business Purpose |
| :--- | :--- | :--- |
| **Trips YTD** | `TOTALYTD([Total_Trips], DIM_Date[Date])` | Year-to-date cumulative trip volume |
| **Trips MTD** | `TOTALMTD([Total_Trips], DIM_Date[Date])` | Month-to-date cumulative trip volume |
| **Trips YoY Growth** | `([Total_Trips] - [PrevYearTrips]) / [PrevYearTrips]` using `SAMEPERIODLASTYEAR` | Year-over-year percentage change in demand |
| **Trips QoQ Change** | Current quarter vs. previous quarter using `PREVIOUSQUARTER` | Quarter-over-quarter demand trend |

### 7.4 Operational & Categorical Measures

| Measure | DAX Logic | Business Purpose |
| :--- | :--- | :--- |
| **Credit Card %** | `DIVIDE(CALCULATE([Total_Trips], payment_type=1), [Total_Trips], 0)` | Share of trips paid by credit card |
| **Cash %** | `DIVIDE(CALCULATE([Total_Trips], payment_type=2), [Total_Trips], 0)` | Share of trips paid in cash |
| **Peak Hour Trips** | `CALCULATE([Total_Trips], DIM_Time[TimeCategory] = "Peak")` | Trip volume during high-demand windows |
| **Speed Category** | `SWITCH` on avg speed: >30 = "Fast", >15 = "Normal", else "Slow" | Traffic congestion classification per trip |

---

## 8. Dashboard Overview

The Power BI report is structured across three interactive pages, each addressing a distinct analytical lens.

### Page 1 – Trip Volume Overview

**Purpose:** Analyze how different variables drive total trip volume.

**KPI Cards:**
- Total Distance (cumulative miles)
- Total Trips (count)
- Average Ride Duration (minutes)

**Visuals:**
- **Pie / Donut Charts:** Proportional breakdown of trips by categorical dimension (e.g., payment type, time category)
- **Column Chart:** Trip volume distribution across numerical or categorical groups
- **Table:** Row-level detail with exact metrics per segment
- **Smart Narrative:** AI-generated text summary of key relationships and trends

---

### Page 2 – Demand Distribution & Behavior

**Purpose:** Deep-dive into trip behavior, temporal demand, and geographic patterns.

**Interactive Slicers:**
- Time Category (Morning, Midday, Evening Peak, Night)
- Payment Method (Credit Card, Cash, No Charge, Disputed)

**Visuals:**
- **Clustered Column Chart:** Trips by Hour × Time Category — identifies exact peak demand windows
- **Stacked Column Chart:** Passenger count distribution — reveals single- vs. multi-passenger ride ratios
- **Donut Chart:** Payment method split — customer preference at a glance
- **Azure Map:** Pickup location density — geospatial heatmap of high-activity zones
- **Matrix Table:** Neighborhood performance matrix combining demand volume with trip metrics

---

### Page 3 – Geographic & Temporal Deep Dive

**Purpose:** Origin-to-destination flow analysis and hour-by-hour demand mapping.

**KPI Cards:** Total Distance, Total Trips, Avg Duration (consistent with Page 1)

**Interactive Slicers:**
- Hours Slicer (0–23, single or multi-select)
- Payment Method Slicer

**Visuals:**
- **Column Chart:** Total Trips by Hour — pinpoints hourly demand peaks and lulls
- **Pickup Map:** Geospatial view of trip origins across neighborhoods
- **Drop-off Map:** Geospatial view of trip destinations — enables origin-destination flow comparison

**Usage Note:** Select filters in the slicers and all KPI cards, charts, and maps update dynamically. Hover over map data points or chart columns for detailed tooltips.

---

## 9. Key Findings & Insights

Based on the data model, DAX measures, and dashboard analysis, the following findings were identified:

### 9.1 Demand & Timing

- Taxi demand is strongly concentrated during **morning and evening peak hours**, reflecting commuter patterns. Off-peak hours (late night and midday) see significantly lower volume.
- **Day-of-week effects** are evident, with weekday demand exceeding weekend demand in commuter corridors, while weekend nights show elevated demand in entertainment districts.
- Seasonal variation exists across quarters, as tracked by the **Trips YoY Growth** and **QoQ Change** measures.

### 9.2 Location & Revenue

- A small number of **high-value neighborhoods** (pickup zones) generate a disproportionate share of total trip volume and distance.
- **Drop-off concentration** in outer neighborhoods creates a "deadhead mileage" problem: drivers returning empty from low-demand drop-off zones lose revenue on those return miles.
- The pickup-to-drop-off neighborhood matrix reveals dominant **commuter corridors** that represent predictable, recurring revenue flows.

### 9.3 Trip Behavior

- The majority of trips involve **1–2 passengers**, indicating significant unused vehicle capacity (vehicles seat up to 4+ passengers).
- **Trip duration** varies materially by time of day — peak-hour trips take longer for the same distance, reflecting traffic congestion and reducing per-hour driver earnings.
- **Speed Category** analysis shows a meaningful proportion of trips classified as "Slow," corresponding to congested corridors where time-based surcharges could offset profitability loss.

### 9.4 Payment Patterns

- **Credit Card** is the dominant payment method, with a measurable gap over Cash.
- **Cash transactions** correlate with higher inter-trip idle time, as cash handling adds friction to trip turnover.
- **Disputed and No-Charge** payment types represent a small but notable proportion of records, indicating potential revenue leakage that warrants monitoring.

### 9.5 Vendor Performance

- Multi-vendor data allows benchmarking: performance differences in efficiency metrics (speed, duration per mile) between vendors suggest that operational protocols differ and that best practices are not uniformly adopted across the fleet.

---

## 10. Strategic Recommendations

The following recommendations are prioritized by expected operational impact:

### Recommendation 1: Predictive Supply Rebalancing
**Finding addressed:** Peak demand concentration; driver idle time in low-demand zones.  
**Action:** Deploy algorithmic dispatching that shifts driver positioning toward neighborhoods and hours with historically high demand. Use the DIM_Date and DIM_Time models to generate forward-looking shift schedules aligned with demand curves.  
**Expected Impact:** Reduction in passenger wait times in high-traffic zones; increase in completed trips per driver per shift.

---

### Recommendation 2: Shared-Ride Tier Introduction
**Finding addressed:** Single-passenger ride dominance; underutilized vehicle capacity.  
**Action:** Introduce an optional shared-ride pricing tier for single-passenger trips in peak-demand corridors. Incentivize passengers with a fare discount while increasing revenue-per-mile for drivers through higher seat utilization.  
**Expected Impact:** Higher revenue per trip mile; improved service capacity without adding vehicles.

---

### Recommendation 3: Digital Payment Incentive Program
**Finding addressed:** Cash payment friction; lower trips-per-hour for cash-heavy drivers.  
**Action:** Offer minor fare discounts or driver commission bonuses for completed digital payment trips. Reduce the cash-handling overhead that currently increases idle time between trips.  
**Expected Impact:** Increase in overall trips-per-hour metric; faster trip turnover fleet-wide.

---

### Recommendation 4: Congestion Surcharge on Bottleneck Corridors
**Finding addressed:** Trip duration disproportionately exceeding distance on congested routes.  
**Action:** Identify corridors where the Speed Category is consistently "Slow" and apply dynamic time-based surcharges. Alternatively, establish strategic drop-off points near major transit hubs to reduce driver exposure to congested final miles.  
**Expected Impact:** Revenue protection on slow trips; reduced driver cost on congestion-heavy routes.

---

### Recommendation 5: Vendor Protocol Standardization
**Finding addressed:** Efficiency gaps between vendors for comparable trip types.  
**Action:** Conduct a structured analysis of the high-performing vendor's dispatch protocols and apply those practices fleet-wide. Use the multi-vendor efficiency benchmarks as the baseline for an operational audit.  
**Expected Impact:** System-wide improvement in average speed and trip duration efficiency, narrowing the performance gap between vendors.

---

### Recommendation 6: Deadhead Mileage Reduction
**Finding addressed:** Empty return miles from drop-offs in outer, low-demand neighborhoods.  
**Action:** Implement a dynamic return-trip incentive or "backhaul" dispatch system where drivers dropping off in low-demand zones are immediately paired with the nearest available pickup request, even if it requires a small repositioning distance.  
**Expected Impact:** Reduction in total empty miles driven; improvement in net revenue per hour for drivers serving outer zones.

---

## 11. Limitations & Next Steps

### Current Limitations

| Limitation | Detail |
| :--- | :--- |
| **Temporal Scope** | The dataset covers a defined extract window; trends may not reflect current conditions. |
| **Geolocation Quality** | Latitude/longitude precision may vary, affecting neighborhood boundary accuracy. |
| **Revenue Data Absent** | Fare amount is not directly present; revenue estimates are inferred from distance and payment type. |
| **External Factors** | Weather, public events, and transit disruptions are not included but significantly influence demand. |
| **Vendor Anonymity** | Vendor IDs are numeric codes; specific operator identities and their individual practices are unknown. |

### Recommended Next Steps

1. **Enrich with Weather Data:** Join taxi records with hourly weather data to quantify the demand uplift during rain or extreme temperatures.
2. **Event Calendar Integration:** Overlay major NYC events (concerts, sports, conferences) to model event-driven demand spikes.
3. **Real-Time Pipeline:** Transition from batch CSV analysis to a streaming data pipeline (e.g., Azure Stream Analytics) to power live operational dashboards.
4. **Predictive Modeling:** Apply machine learning (regression or classification) to predict trip demand by neighborhood and hour, enabling proactive fleet positioning.
5. **Fare Amount Integration:** Source or derive actual fare amounts to produce true revenue analytics rather than proxy metrics.

---

## 12. Conclusion

This Business Intelligence project successfully transformed over 50,000 raw NYC taxi trip records into structured, actionable insights using a rigorous five-phase approach: data exploration, ETL transformation, dimensional modeling, DAX development, and interactive visualization.

The star schema data model provides a scalable, high-performance analytical foundation. The three-page Power BI dashboard enables stakeholders to explore demand patterns, geographic concentrations, trip behavior, and payment trends through interactive filtering and geospatial mapping.

The central finding is clear: **the NYC taxi fleet's inefficiency is fundamentally a supply-demand alignment problem**, compounded by underutilized capacity and transaction friction. The six strategic recommendations presented in this report offer a practical, data-grounded roadmap for:

- Reducing passenger wait times
- Increasing driver revenue per hour
- Minimizing unprofitable "deadhead" mileage
- Improving fleet-wide operational velocity

By acting on these findings, taxi operators and city planners can move toward a smarter, more equitable, and more profitable urban mobility system.

---

## 13. References

**Dataset**  
Ramireddy, S. (2023). *New York City taxi rides* [Data set]. Kaggle.  
https://www.kaggle.com/datasets/surekharamireddy/new-york-city-taxi-rides

**Supplementary Dataset Reference**  
NYC Taxi Trip Duration Competition Dataset. Kaggle.  
https://www.kaggle.com/datasets/competitions/nyc-taxi-trip-duration/data

**Tool**  
Microsoft Power BI. https://powerbi.microsoft.com/

**Interactive Dashboard**  
[Power BI Dashboard – Group A NYC Taxi Report](https://app.powerbi.com/groups/me/reports/f9382bb8-35cc-4344-aace-fb1207f068c9/40a24eb88e3a7afbd37a?experience=power-bi)

**Presentation Slides**  
[Google Slides – NYC Taxi BI Analysis](https://docs.google.com/presentation/d/1Y9mi9wlte_jByuIBo5VOkpEVG015vHF-8lsqFSNJjXo/edit?usp=sharing)

---

*Report prepared by Group A, DSA3050A – Business Intelligence & Data Visualization.*
