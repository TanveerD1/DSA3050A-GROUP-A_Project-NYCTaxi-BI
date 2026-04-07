---
marp: true
---
# NYC Taxi BI Analysis 

**Data-Driven Urban Mobility**  

*DSA3050A - Business Intelligence & Data Visualization* 

**Team:** 
Tanveer Omar
Mohamed Mohamed
Mitchel Muthaura
Calvin Gacheru
Claire Mwarari
Lavender Nchagwa

---

# Problem Statement  

**1. The Strategic Gap**

New York City’s taxi ecosystem suffers from a misalignment between dynamic passenger demand and active vehicle supply.

The Result: High idle times for drivers in quiet zones and excessive wait times for passengers in high-traffic neighborhoods.

---

**2. Key Operational Variables**
To solve this, we must analyze and synchronize three critical factors:

 **Location:** Geographic hotspots for pickups vs. drop-offs.

 **Timing:** Hourly and seasonal fluctuations in ride requests.

 **Trip Behavior:** Patterns in duration, distance, and payment types that dictate profitability.

---

# Data Collection  

**Source:**
- Taxi rides website evidence
![NYC Taxi dataset website homepage displaying ride data, fare calculations, and location information for 2024 taxi analytics project](documentation/image-5.png)

---

**Executive Summary of the data**

**Executive Summary: NYC Taxi Trip Duration Analysis**

* **Dataset Scope:** Analysis of 50,000+ trip records from three distinct taxi vendors to identify urban traffic patterns and mobility trends.
* **Temporal & Spatial Variables:** Integration of precise pickup/drop-off timestamps and geographic coordinates (latitude/longitude) mapped to specific NYC neighborhoods.
* **Operational Metrics:** Tracking of passenger counts, trip distances (miles), and payment types (Credit, Cash, etc.) to evaluate service usage.
* **Primary Objective:** Modeling **trip_duration** as the target variable to optimize vehicle dispatching and predict fleet availability for subsequent trips.
* **Strategic Goal:** Leveraging dependency analysis between trip variables to minimize idle time and improve taxi assignment efficiency across high-demand zones.

---

# Data Architecture and Star schema 
**Why Star Schema?**
- Enables fast, scalable analytics
- Simplifies complex queries
- Supports flexible slicing/dicing for BI

**Visual: Star Schema**

---
![alt text](documentation/schema.png)

---
# Data Cleaning and Transformation
The dataset underwent rigorous cleaning and transformation to ensure high-quality, actionable insights:

- **Promoted Headers**: Set the first row as headers for accurate column identification.
- **Renamed Queries**: Standardized naming conventions (e.g., changing trips_1 to FACT_trips).
- **Removed Columns**: Deleted redundant latitude and longitude columns from the Fact table, as they exist in Dimension tables.
- **Fixed Data Types**: Corrected types for numerical and datetime fields.

---

- **Split Columns**: Separated pickup and drop-off datetime columns into distinct "Date Only" and "Time Only" columns.
- **Handled Nulls**: Filtered out empty values from neighborhood ID columns.
- **Filtered Outliers**: Excluded trips with a distance of 0 to prevent skewed visualizations.
- **Removed Duplicates**: Deleted duplicate records based on the unique id column.
- **Merged Queries**: Joined the Fact table with Dimension tables to pull in neighborhood names.

---

- **Replaced Values**: Decoded payment IDs into text (e.g., 1 = "credit card", 2 = "cash", 4 = "disputed").
- **Custom Speed Column**: Created speed_mph formula: = [trip_distance] / [trip_duration]/3600.
- **Conditional Time Columns**: Categorized times into text labels ("Morning Peak", "Midday", "Evening Peak", "Night")

---

## Dimensional Modeling & Optimizations
- **Star Schema Implementation**: Structured the data with one central Fact table (FACT_trips) surrounded by descriptive Dimension tables.
- **One-to-Many Relationships**: Established single-direction, one-to-many relationships connecting Dimension tables (Date, Time, Payment, Pickups, Dropoffs) to the Fact table.
- **Data Reduction**: Optimized model size and performance by dropping duplicate rows and redundant geographical coordinates early in the transformation process.

---

## Analytical DAX Measures & Tables

- **DIM_Date Table**: Created using CALENDAR and ADDCOLUMNS to extract Year, Month, Day, Weekday, Quarter, etc.
- **DIM_Payment Table**: Hardcoded using DATATABLE to map codes (1-4) to strings ("Credit Card", "Cash", etc.).
- **Total Trips**: Total_Trips = COUNTROWS(FACT_trips).
- **Total Distance**: Total_Distance = SUM(FACT_trips[trip_distance]).
- **Average Passengers**: Average_Passengers = AVERAGE(FACT_trips[passenger_count]).
- **Average Duration (Mins)**: Avg_Duration_Minutes = AVERAGE(FACT_trips[trip_duration]) / 60.
- **Trips YTD**: Trips_YTD = TOTALYTD([Total_Trips], DIM_Date[Date]).

---

- **Trips MTD**: Trips_MTD = TOTALMTD([Total_Trips], DIM_Date[Date]).
- **Credit Card %**: Credit Card % = DIVIDE(CALCULATE([Total_Trips], FACT_trips[payment_type] = 1), [Total_Trips], 0).
- **Cash %**: Cash % = DIVIDE(CALCULATE([Total_Trips], FACT_trips[payment_type] = 2), [Total_Trips], 0).
- **Peak Hour Trips**: Peak Hour Trips = CALCULATE([Total_Trips], DIM_Time[TimeCategory] = "Peak").
- **Trips YoY Growth**: Calculates current year trips minus previous year trips (using SAMEPERIODLASTYEAR), divided by previous year trips.
- **Trips QoQ Change**: Compares current quarter trips against previous quarter trips using PREVIOUSQUARTER.
- **Speed Category**: Uses SWITCH to classify average speed: > 30 as "Fast", > 15 as "Normal", and else "Slow".

---

![alt text](documentation/dash1.png)

---


























# Slide 6: Data Architecture & Star Schema

**Fact Table:**
- FactTrips (id, date_key, vendor_id, passenger_count, trip_distance, payment_type, trip_duration, pickup_neighborhood_key, dropoff_neighborhood_key)

**Dimension Tables:**
- DimDate (date_key, year, month, day, hour, etc.)
- DimPickupNeighborhood (neighborhood_key, name)
- DimDropoffNeighborhood (neighborhood_key, name)
- DimPayment (payment_key, code, description)
- DimVendor (vendor_key, id, name)

**Visual:**
![Star Schema](documentation/image-73.png)



---

# Slide 7: ETL & Data Transformation (Power Query)

- **Header Promotion:** Ensured correct column names ([image-12](documentation/image-12.png))
- **Column Pruning:** Removed redundant geolocation columns ([image-15](documentation/image-15.png))
- **Data Type Fixes:** Standardized datetime, integer, and categorical fields ([image-18](documentation/image-18.png), [image-24](documentation/image-24.png))
- **Splitting Date/Time:** Extracted date and time for temporal analysis ([image-27](documentation/image-27.png), [image-29](documentation/image-29.png))
- **Conditional Columns:** Labeled peak/off-peak hours ([image-35](documentation/image-35.png))
- **Null Handling:** Removed nulls from dimension tables ([image-38](documentation/image-38.png))
- **Outlier Filtering:** Excluded zero-distance trips ([image-41](documentation/image-41.png))
- **Duplicate Removal:** Ensured unique trip IDs ([image-43](documentation/image-43.png))
- **Custom Columns:** Calculated speed ([image-45](documentation/image-45.png))
- **Merges:** Added neighborhood names ([image-48](documentation/image-48.png), [image-50](documentation/image-50.png))
- **Payment Decoding:** Replaced codes with descriptions ([image-56](documentation/image-56.png))

---

# Slide 8: Dimensional Modeling

- **DimDate:** Created for time-based analysis ([image-63](documentation/image-63.png))
- **DimTime:** Hour-level granularity ([image-65](documentation/image-65.png))
- **DimPayment:** Decoded payment types ([image-67](documentation/image-67.png))
- **Relationships:** One-to-many from dimensions to fact ([image-68](documentation/image-68.png)–[image-72](documentation/image-72.png))

---

# Slide 9: Optimization Techniques

- **Star Schema:** Reduces query complexity and improves performance
- **Column Pruning:** Minimizes memory usage
- **Data Types:** Ensures efficient storage and calculation
- **Indexing (Power BI):** Implicit via relationships and columnar storage
- **Materialized Views:** Achieved via summary tables and DAX measures

**Why?**
- To enable fast, interactive dashboards
- To support large-scale analytics without performance bottlenecks

---

# Slide 10: Analytical DAX Measures

- **Total Trips:** [image-75](documentation/image-75.png)
- **Total Distance:** [image-77](documentation/image-77.png)
- **Average Passengers:** [image-78](documentation/image-78.png)
- **YTD/MTD/YoY Growth:** [image-79](documentation/image-79.png), [image-80](documentation/image-80.png), [image-81](documentation/image-81.png)
- **QoQ Change:** [image-82](documentation/image-82.png)
- **Payment Method %:** [image-83](documentation/image-83.png), [image-84](documentation/image-84.png)
- **Average Trip Duration:** [image-85](documentation/image-85.png)
- **Peak Hour Trips:** [image](documentation/image.png)
- **Speed Categories:** [image-1](documentation/image-1.png)

**Why DAX?**
- Enables advanced, dynamic analytics
- Supports business-focused KPIs

---

# Slide 11: Power BI Dashboard Overview

- **KPIs:** Total Distance, Total Trips, Avg. Duration ([pbix/dashboard1.jpeg](pbix/dashboard1.jpeg))
- **Visuals:** Pie, doughnut, column charts, maps ([pbix/dashboard2.png](pbix/dashboard2.png), [pbix/dashboard3.png](pbix/dashboard3.png), [pbix/dashboard4.png](pbix/dashboard4.png))
- **Interactivity:** Slicers for hour, payment method, etc.
- **Geospatial Analysis:** Pickup/dropoff maps

---

# Slide 12: Key Insights & Recommendations

- **Peak Demand:** Identified by hour and neighborhood
- **Revenue Drivers:** Top neighborhoods and payment types
- **Operational Efficiency:** Outlier and null handling improved data quality
- **Actionable Recommendations:**
  - Adjust fleet allocation to match peak demand
  - Incentivize drivers for underserved areas/times
  - Promote efficient payment methods

---

# Slide 13: Conclusion

- BI and data modeling enable smarter urban mobility decisions
- Power BI dashboards provide actionable, real-time insights
- Data-driven strategies can optimize both passenger experience and driver revenue

---

# Slide 14: References

- Kaggle Dataset: https://www.kaggle.com/datasets/surekharamireddy/new-york-city-taxi-rides
- Course: DSA3050A - Business Intelligence & Data Visualization
- All images and visuals from this repository

- **Dataset:** New York City taxi rides ([Kaggle](https://www.kaggle.com/datasets/surekharamireddy/new-york-city-taxi-rides))