# Data Center Site Selection & Grid Analysis for Intel

## Project Objective
Intel is planning to build a new data center and needs to identify the optimal location within the United States. The ideal site must meet energy demands of roughly 100 MW/year while adhering to Intel's sustainability goals, which require using a majority renewable energy. My objective was to analyze US energy grid data, evaluate regional power production, and recommend a state that provides an energy surplus and relies heavily on renewable sources.

## The Approach
To solve this, I analyzed three primary datasets containing daily US grid demand, regional energy generation by source, and specific power plant outputs for the year 2022. The project was broken down into three phases:

1. **Data Exploration and Transformation (SQL):** I queried and restructured datasets to isolate key metrics and prepare data for visualization. 
2. **Data Visualization (Tableau):** I imported aggregated data into Tableau to create dynamic dashboards to visualize regional energy surpluses, leaders in renewable generation, and compare state level energy outputs.
3. **Identifying Business Solutions:** I analyzed dynamic data visualizations to suggest locations for a new data center that best met the required energy and sustaiability benchmarks set forth by Intel.

## Key Data Insights
### 1. Identifying an Energy Surplus
To find a suitable location for Intel's new data center, my first step was to identify grids operating with a power surplus. A dataset containing information about daily energy production and consumption for different regions in the US helped me to find answers. By querying gross energy production against demand, I isolated 5 US regions that produced an energy surplus.
```sql
--Surplus Energy Production
SELECT
  region,
  SUM(gross_generation - demand) AS net_energy
FROM intel.energy_data
GROUP BY region
ORDER BY net_energy DESC;
``` 
<p align="center">
  <img width="606" height="426" alt="Screenshot 2026-10-18 at 3 16 11 PM" src="https://github.com/user-attachments/assets/6bd5eeb3-884d-4c00-b4a8-38cc51266e33" />
</p>

### 2. Identifying Renewable Leaders
Next, I queried the dataset to calculate the percentage of total energy generation that came from renewable sources for each region. The data highlights the Northwest as the definitive leader, with over 50% of its grid powered by renewables, making it a strong candidate for Intel's green energy goals. 
```sql
--Percent Renewable Energy
SELECT
  region,
  (SUM(hydropower_and_pumped_storage + wind + solar) / SUM(gross_generation)) * 100 AS percent_renewable_generation
FROM intel.energy_data
GROUP BY region
ORDER BY percent_renewable_generation DESC;
```
<p align="center">
<img width="604" height="427" alt="Screenshot 2026-10-18 at 3 34 29 PM" src="https://github.com/user-attachments/assets/2a9271a9-b538-4fd0-8fa5-637d8d66b6fe" />
</p>

### 3. Tracking Seasonal Generation Trends
To determine if renewable energy is generated consistently throughout the year, I needed to inspect time-series data for 2022. To investigate, I needed to restructure the raw data. I grouped specific energy source columns into a new `energy_type` column, dividing the data cleanly into 'renewable energy' and 'fossil fuel' categories. Visualizing the adjusted data showed that the Northwest generated renewable energy at a consistent rate, outperforming fossil fuels for half the year while never dropping below 2 million MW in output.
```sql
--Time Series Data Adjustments
SELECT
  date, 
  region,
  (hydropower_and_pumped_storage + wind + solar) AS energy_generated_mw,
  'renewable energy' AS energy_type
FROM intel.energy_data

UNION ALL

SELECT
  date,
  region,
  (all_petroleum_products + coal + natural gas + nuclear + other_fuel_sources) AS energy_generated_mw,
  'fossil fuel' AS energy_type
FROM intel.energy_data
ORDER BY date, region, energy_type, energy_generated_mw;
```
<p align="center">
<img width="604" height="427" alt="Gemini_Generated_Image_8d3kb18d3kb18d3k" src="https://github.com/user-attachments/assets/95e85605-c472-4804-b929-12029dd05231" />
</p>

### 4. State Specific Energy Generation
From the previous queries and visualizations I was able to pinpoint the Northwest region as the top contender for Intel's new datacenter. Gaining further insight into the region, specifically state specific energy generation, took joining two new datasets containing individual power plant data. After visualizing the combined dataset and filtering data for the Northwest region, the state of Washington stood out as a clear leader generating nearly 75 million MW of renewable energy in the year of 2022.
```sql
--Combining Data
SELECT
  a.region,
  a.state,
  b.energy_type,
  b.energy_generated_mw
FROM intel.power_plants AS a
INNER JOIN intel.energy_by_plant AS b
ON a.plant_code = b.plant_code;
```
<p align="center">
<img width="556" height="438" alt="Screenshot 2026-10-22 at 4 15 31 PM" src="https://github.com/user-attachments/assets/379bc04c-88e7-4317-99fc-94925833c0e6" />
</p>

## Identifying Business Solutions
Using tools like SQL and Tableau to aggregate and visualize data allowed clear insight into which region and more specifically which state would be able to best meet Intel's energy and sustainability benchmarks. **I used my final dashboard the communicate to Intel why the state of Washington in the Northwest region was a clear choice.**
1) The Northwest ranks 2nd in surplus energy production.
2) The Northwest ranks 1st in percent renewable energy production.
3) The Northwest generates renewable energy consistently throughout the year.
4) Washington state ranks 1st both in total energy production (~100M Mw/year) and percent renewable energy production (~75%) in the Northwest.
   
<p align="center">
<img width="1396" height="780" alt="Screenshot 2026-10-26 at 5 16 02 PM" src="https://github.com/user-attachments/assets/81ddc120-b822-4b15-b215-875cb3c7aa0f" />
</p>
