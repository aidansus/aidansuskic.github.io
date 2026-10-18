# Data Center Site Selection & Grid Analysis for Intel

## Project Objective
Intel is planning to build a new data center and needs to identify the optimal location within the United States. The ideal site must meet energy demands of roughly 100 MW/year while adhering to Intel's sustainability goals, which require using 93% renewable energy. My objective was to analyze US energy grid data, evaluate regional power production, and recommend a state that provides an energy surplus and relies heavily on renewable sources.

## The Approach
To solve this, I analyzed three primary datasets containing daily US grid demand, regional energy generation by source, and specific power plant outputs for the year 2022. The project was broken down into three phases:

1. **Data Exploration (SQL):** I queried the raw data to calculate...
2. **Data Transformation (SQL):** I engineered a new dataset by categorizing dozens of specific power sources (wind, solar, coal, nuclear) into binary `renewable energy` and `fossil fuel` labels to prepare the data for visual trend analysis[cite: 8, 9].
3. **Visual Storytelling (Tableau):** I built dynamic dashboards to map out regional surpluses, track hourly generation trends, and drill down into state-level renewable output[cite: 18, 22].


## Key Data Insights
In order to find a suitable location for Intel's new data center our first step is to locate regions with surplus in energy production. A short SQL query aggregates data from a dataset containing national energy production and consumption in order to isolate potential site locations. 
```sql
--Surplus Energy Production
SELECT
region,
SUM(net_ generation - demand) AS net_energy
FROM intel. energy_data
GROUP BY region
ORDER BY net_energy DESC;
'''
