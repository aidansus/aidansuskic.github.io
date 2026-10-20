# Data Center Site Selection & Grid Analysis for Intel

## Project Objective
Intel is planning to build a new data center and needs to identify the optimal location within the United States. The ideal site must meet energy demands of roughly 100 MW/year while adhering to Intel's sustainability goals, which require using 93% renewable energy. My objective was to analyze US energy grid data, evaluate regional power production, and recommend a state that provides an energy surplus and relies heavily on renewable sources.

## The Approach
To solve this, I analyzed three primary datasets containing daily US grid demand, regional energy generation by source, and specific power plant outputs for the year 2022. The project was broken down into three phases:

1. **Data Exploration (SQL):** I queried the raw data to calculate...
2. **Data Transformation (SQL):** I engineered a new dataset by categorizing dozens of specific power sources (wind, solar, coal, nuclear) into binary `renewable energy` and `fossil fuel` labels to prepare the data for visual trend analysis[cite: 8, 9].
3. **Visual Storytelling (Tableau):** I built dynamic dashboards to map out regional surpluses, track hourly generation trends, and drill down into state-level renewable output[cite: 18, 22].

## Key Data Insights
### 1. Identifying an Energy Surplus
To find a suitable location for Intel's new data center, my first step was to identify grids operating with a power surplus. By querying national energy production against local demand, I isolated the 5 US regions that produce more energy than they consume.
```sql
--Surplus Energy Production
SELECT
  region,
  SUM(net_generation - demand) AS net_energy
FROM intel.energy_data
GROUP BY region
ORDER BY net_energy DESC;
``` 
<p align="center">
  <img width="606" height="426" alt="Screenshot 2026-10-18 at 3 16 11 PM" src="https://github.com/user-attachments/assets/6bd5eeb3-884d-4c00-b4a8-38cc51266e33" />
</p>

### 2. Identifying Renewable Leaders
Next, I queried the dataset to calculate the percentage of total energy generation that came from renewable sources for each region.. The data highlights the Northwest as the definitive leader, with over 50% of its grid powered by renewables, making it a strong candidate for Intel's green energy goals. 
```sql
--Percent Renewable Energy
SELECT
  region,
  (SUM(hydropower_and_pumped_storage + wind + solar) / SUM(net_generation)) * 100 AS percent_renewable_generation
FROM intel. energy_data
GROUP BY region
ORDER BY percent_renewable_generation DESC;
```
<p align="center">
<img width="604" height="427" alt="Screenshot 2026-10-18 at 3 34 29 PM" src="https://github.com/user-attachments/assets/2a9271a9-b538-4fd0-8fa5-637d8d66b6fe" />
</p>

### 3. Tracking Seasonal Generation Trends
To determine if renewable energy is generated consistently throughout the year, I needed to inspect time-series data for 2022. To investigate, I needed to restructure the raw data. I grouped specific energy source columns into a new energy_type column, dividing the data cleanly into 'renewable energy' and 'fossil fuel' categories.
```sql
--Union All Energy by Source
SELECT
  date, 
  region,
  (hydropower_and_pumped_storage + wind + solar) AS energy-generated_mw, 'renewable energy' AS energy_type
FROM intel. energy_data

UNION ALL

SELECT
  date,
  region,
  (all_petroleum products + coal + natural gas + nuclear + other_fuel_sources) AS energy-generated_mw, 'fossil fuel' AS energy_type
FROM intel. energy_data
ORDER BY date, region, energy_type, energy_generated_mw;
```
