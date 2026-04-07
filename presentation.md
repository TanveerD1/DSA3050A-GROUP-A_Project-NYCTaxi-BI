---
marp: true
---

# Slide 1: Title Slide

# NYC Taxi BI Analysis 

**Data-Driven Urban Mobility**  

*DSA3050A - Business Intelligence & Data Visualization* 

**Team:** 
Tanveer 762 
Mohamed 006 
Mitchel 413 
Calvin 035 
Claire 470
Lavender 647  


---

# Slide 2: Problem Statement  

**1. The Strategic Gap**

New York City’s taxi ecosystem suffers from a misalignment between dynamic passenger demand and active vehicle supply.

The Result: High idle times for drivers in quiet zones and excessive wait times for passengers in high-traffic neighborhoods.

**2. Key Operational Variables**
To solve this, we must analyze and synchronize three critical factors:

 **Location:** Geographic hotspots for pickups vs. drop-offs.

 **Timing:** Hourly and seasonal fluctuations in ride requests.

 **Trip Behavior:** Patterns in duration, distance, and payment types that dictate profitability.

---

# Slide 3: Problem Statement

NYC's taxi operations face significant challenges in balancing supply and demand. Inefficiencies lead to long passenger wait times and idle drivers. 

**Why Analytics?**
- To optimize fleet utilization and route planning.
- To reduce inefficiencies and improve passenger experience.
- To maximize driver revenue through data-driven decisions.

---

# Slide 4: Business Questions

1. What are the peak hours and days for taxi demand across neighborhoods?
2. Which neighborhoods generate the highest trip volumes and revenue?
3. How does trip distance correlate with fare amount and payment type?
4. What is the average trip duration and how does it vary by time of day?
5. Which payment methods are most common and how do they vary by neighborhood?
6. How many passengers typically ride together and does this affect trip distance?
7. What is the relationship between pickup and dropoff neighborhoods (commuter patterns)?

---

# Slide 5: Data Source and Scope

- **Dataset:** New York City taxi rides ([Kaggle](https://www.kaggle.com/datasets/surekharamireddy/new-york-city-taxi-rides))
- **Files:**
  - trips_1.csv (Fact table: 50,000+ rows)
  - pickup_neighborhoods.csv (Dimension)
  - dropoff_neighborhoods.csv (Dimension)
- **Granularity:** Trip-level, with time, location, and payment details
- **Assumptions:** Cleaned for nulls, outliers, and duplicates

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

**Why Star Schema?**
- Enables fast, scalable analytics
- Simplifies complex queries
- Supports flexible slicing/dicing for BI

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

---

# Slide 15: Appendix (Extra Visuals)

- [documentation/image-4.png](documentation/image-4.png)
- [documentation/image-5.png](documentation/image-5.png)
- [documentation/image-6.png](documentation/image-6.png)
- [documentation/image-7.png](documentation/image-7.png)
- [documentation/image-8.png](documentation/image-8.png)
- [documentation/image-9.png](documentation/image-9.png)
- [documentation/image-10.png](documentation/image-10.png)
- [documentation/image-11.png](documentation/image-11.png)
- [documentation/image-13.png](documentation/image-13.png)
- [documentation/image-14.png](documentation/image-14.png)
- [documentation/image-16.png](documentation/image-16.png)
- [documentation/image-17.png](documentation/image-17.png)
- [documentation/image-19.png](documentation/image-19.png)
- [documentation/image-20.png](documentation/image-20.png)
- [documentation/image-21.png](documentation/image-21.png)
- [documentation/image-22.png](documentation/image-22.png)
- [documentation/image-23.png](documentation/image-23.png)
- [documentation/image-25.png](documentation/image-25.png)
- [documentation/image-26.png](documentation/image-26.png)
- [documentation/image-28.png](documentation/image-28.png)
- [documentation/image-29.png](documentation/image-29.png)
- [documentation/image-31.png](documentation/image-31.png)
- [documentation/image-33.png](documentation/image-33.png)
- [documentation/image-34.png](documentation/image-34.png)
- [documentation/image-35.png](documentation/image-35.png)
- [documentation/image-36.png](documentation/image-36.png)
- [documentation/image-37.png](documentation/image-37.png)
- [documentation/image-38.png](documentation/image-38.png)
- [documentation/image-39.png](documentation/image-39.png)
- [documentation/image-40.png](documentation/image-40.png)
- [documentation/image-41.png](documentation/image-41.png)
- [documentation/image-42.png](documentation/image-42.png)
- [documentation/image-43.png](documentation/image-43.png)
- [documentation/image-44.png](documentation/image-44.png)
- [documentation/image-46.png](documentation/image-46.png)
- [documentation/image-47.png](documentation/image-47.png)
- [documentation/image-48.png](documentation/image-48.png)
- [documentation/image-49.png](documentation/image-49.png)
- [documentation/image-50.png](documentation/image-50.png)
- [documentation/image-51.png](documentation/image-51.png)
- [documentation/image-52.png](documentation/image-52.png)
- [documentation/image-53.png](documentation/image-53.png)
- [documentation/image-54.png](documentation/image-54.png)
- [documentation/image-55.png](documentation/image-55.png)
- [documentation/image-56.png](documentation/image-56.png)
- [documentation/image-61.png](documentation/image-61.png)
- [documentation/image-62.png](documentation/image-62.png)
- [documentation/image-64.png](documentation/image-64.png)
- [documentation/image-65.png](documentation/image-65.png)
- [documentation/image-66.png](documentation/image-66.png)
- [documentation/image-67.png](documentation/image-67.png)
- [documentation/image-68.png](documentation/image-68.png)
- [documentation/image-69.png](documentation/image-69.png)
- [documentation/image-70.png](documentation/image-70.png)
- [documentation/image-71.png](documentation/image-71.png)
- [documentation/image-72.png](documentation/image-72.png)
- [documentation/image-74.png](documentation/image-74.png)
- [documentation/image-75.png](documentation/image-75.png)
- [documentation/image-76.png](documentation/image-76.png)
- [documentation/image-77.png](documentation/image-77.png)
- [documentation/image-78.png](documentation/image-78.png)
- [documentation/image-79.png](documentation/image-79.png)
- [documentation/image-80.png](documentation/image-80.png)
- [documentation/image-81.png](documentation/image-81.png)
- [documentation/image-82.png](documentation/image-82.png)
- [documentation/image-83.png](documentation/image-83.png)
- [documentation/image-84.png](documentation/image-84.png)
- [documentation/image-85.png](documentation/image-85.png)
- [documentation/image.png](documentation/image.png)
- [pbix/dashboard1.jpeg](pbix/dashboard1.jpeg)
- [pbix/dashboard2.png](pbix/dashboard2.png)
- [pbix/dashboard3.png](pbix/dashboard3.png)
- [pbix/dashboard4.png](pbix/dashboard4.png)

---

*End of Presentation*