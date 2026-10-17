# Data Center Site Selection & Grid Analysis for Intel

## Project Objective
Intel is planning to build a new data center and needs to identify the optimal location within the United States. The ideal site must meet energy demands of roughly 100 MW/year while adhering to Intel's sustainability goals, which require using 93% renewable energy. My objective was to analyze US energy grid data, evaluate regional power production, and recommend a state that provides an energy surplus and relies heavily on renewable sources.

## The Approach
To solve this, I analyzed three primary datasets containing daily US grid demand, regional energy generation by source, and specific power plant outputs for the year 2022. The project was broken down into three phases:

1. **Data Exploration (SQL):** I queried the raw data to calculate the net energy production (generation minus demand) for each region to find grids operating at a surplus[cite: 4, 5]. 
2. **Data Transformation (SQL):** I engineered a new dataset by categorizing dozens of specific power sources (wind, solar, coal, nuclear) into binary `renewable energy` and `fossil fuel` labels to prepare the data for visual trend analysis[cite: 8, 9].
3. **Visual Storytelling (Tableau):** I built dynamic dashboards to map out regional surpluses, track hourly generation trends, and drill down into state-level renewable output[cite: 18, 22].

## Core Competencies Demonstrated
*   **Programmatic Data Labeling:** Transformed raw energy generation metrics into binary categorical labels (`renewable energy` vs. `fossil fuel`) using SQL to enable downstream machine learning and visual analysis[cite: 8, 9].
*   **Data Integrity & Edge-Case Handling:** Prevented critical data loss during dataset merging by explicitly structuring `UNION ALL` logic to preserve valid, identical time-series instances that standard `UNION` functions would have erroneously dropped as duplicates[cite: 10].
*   **Complex Relational Mapping:** Linked independent datasets using `INNER JOIN` operations on primary keys (`plant_code`) to aggregate energy production at the individual power plant level[cite: 12].
*   **Time-Series Parsing:** Extracted granular hourly trends using `date_part` functions to compare peak renewable generation cycles between distinct geographic grids (California vs. Pacific Northwest)[cite: 16].

## Data Pipeline & Methodology
The analysis was conducted using three primary datasets:
1.  `intel.energy_data`: Daily energy production, demand, and generation by source (e.g., wind, coal, solar)[cite: 3].
2.  `intel.power_plants`: Identifier codes, regional locations, and primary technologies of US power plants[cite: 4].
3.  `intel.energy_by_plant`: Total megawatt (MW) generation per plant for the 2022 calendar year[cite: 4].

To prepare the data for visualization, I engineered a new master table. Rather than relying on disparate columns for individual energy sources, I wrote queries to aggregate sources into precise classifications. I preserved high-fidelity data by carefully selecting merging techniques; for instance, when combining the renewable and fossil fuel data frames, I utilized `UNION ALL`. This ensured that legitimate duplicate generation values across the same region and date (e.g., California generating 38 MW from multiple overlapping sources on 2022-01-01) were fully retained in the final pipeline[cite: 10].

## Key Findings & Recommendation
Using Tableau to visualize the transformed datasets, the **Pacific Northwest** emerged as the strongest candidate region, offering a 21.6 million MW surplus with 52% of its grid powered by renewable sources[cite: 24]. 

Drilling down into state-level data, **Washington State** is the definitive recommendation. Washington produces nearly 75 million MW/year from renewable sources alone—vastly exceeding the data center's requirements—while offering a naturally cool climate (56°F annual average) to reduce secondary cooling costs[cite: 24].

## SQL Code Highlight: Relational Joins & Aggregation
This query demonstrates the use of Common Table Expressions (CTEs) to join distinct power plant databases, filtering specifically for operational efficiency of Solar Photovoltaic farms[cite: 14, 27].

```sql
WITH plant_data AS (
    SELECT 
        a.*,
        b.*
    FROM intel.power_plants AS a
    INNER JOIN intel.energy_by_plant AS b
        ON a.plant_code = b.plant_code
)
SELECT 
    region,
    SUM(energy_generated_mw) AS total_energy_gen,
    COUNT(*) AS n_plants
FROM plant_data
WHERE energy_type = 'renewable_energy'
    AND primary_technology = 'Solar Photovoltaic'
GROUP BY region
HAVING COUNT(*) >= 50;
