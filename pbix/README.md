
## Page 1

### Purpose
This page is designed to analyze how different variables affect the total trips variable, providing insights into the key drivers of taxi demand.

### Key Performance Indicators (KPIs)
Located at the top of the dashboard, these cards provide a quick snapshot of overall performance:
* **Total Distance:** The cumulative distance traveled across all selected trips.
* **Total Trips:** The absolute count of individual taxi rides within the filtered parameters.
* **Average Duration Of Rides:** The mean time taken per trip, measured in minutes.

### Visualizations
* **Pie Chart:** Displays the proportional breakdown of total trips across different categorical variables, allowing you to see the relative contribution of each segment to the overall trip volume.
* **Doughnut Chart:** Similar to the pie chart but with an alternative visual representation, showing how different categories share the total trips metric.
* **Column Chart:** Illustrates the distribution of trips across numerical or categorical dimensions, making it easy to identify which categories have the highest trip volumes.
* **Table:** Provides detailed, row-level data with specific metrics for each category, enabling users to see exact numbers and perform detailed analysis.
* **Narrative:** Offers a text-based summary and insights about the relationships between variables and their impact on total trips, helping to contextualize the visual findings.

---
## Page 2: Trip Behavior & Operational Insights

### Purpose
My dashboard page focuses on analyzing trip behavior, demand distribution, and operational patterns within the NYC Taxi dataset. The goal is to move beyond high-level summaries and provide data-driven insights into how trips vary across time, passenger composition, payment methods, and geographic locations.

### Interactive Slicers
#### **Time Category Slicer**
  This slicer enables users to filter the dashboard based on different periods of the day (e.g., morning, afternoon, evening, night).

#### **Payment Method Slicer**
  This slicer allows users to isolate trips by payment type.

### Visuals

#### 1. **Trips by Hour and Time Category (Clustered Column Chart)**
  This visual captures demand patterns by hour of the day, sectioned into broader time categories.

#### 2. **Passenger Count Distribution (Stacked Column Chart)**
  This chart analyzes how trips are distributed based on the number of passengers.

#### 3. **Payment Method Distribution (Donut Chart)**
  This visual highlights customer payment preferences.

#### 4. **Pickup Location Demand (Azure Map)**
  The map provides a geospatial view of trip demand, identifying high-activity pickup zones.

#### 5. **Neighborhood Performance Matrix (Matrix Table)**
  This matrix shows a combines demand and trip characteristics.


---
## Page 3

## Key Performance Indicators (KPIs)
Located at the top of the dashboard, these cards provide a quick snapshot of overall performance based on the selected filters:
* **Total Distance:** The cumulative distance traveled across all selected trips.
* **Total Trips:** The absolute count of individual taxi rides within the filtered parameters.
* **Average Duration Of Rides:** The mean time taken per trip, measured in minutes.

## Interactive Filters (Slicers)
The left panel contains slicers that allow users to drill down into specific data subsets. All visuals on the page will cross-filter interactively when these are adjusted:
* **Hours Slicer:** Filters data by specific hours of the day (e.g., 0 for midnight, 1 for 1 AM). You can select single or multiple hours to isolate time-based trends and shift performance.
* **Payment Method Slicer:** Filters rides based on how the transaction was settled (e.g., Cash, Credit Card, Dispute, No Charge).

## Visualizations
* **Total Trips by hour (Column Chart):** Displays the distribution of ride volume across the selected hours. This is useful for identifying peak demand times and hourly lulls.
* **Pickup locations (Map):** A geospatial view highlighting where trips originated. The data points represent the concentration of pickups in specific neighborhoods across the city.
* **Dropoff locations (Map):** A geospatial view showing the final destinations of the trips. Comparing this with the pickup map allows for origin-to-destination flow analysis.

## Usage Instructions
To use this dashboard effectively, begin by selecting your desired parameters in the **Hours Slicer** or **Payment Method Slicer**. The KPI cards, column chart, and maps will dynamically update to reflect the specific subset of data you have chosen. You can also hover over map data points or chart columns to view detailed, localized tooltips.
