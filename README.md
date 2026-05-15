# International Development: Global Population Trends (SQL Analysis)

## Project Goal
The primary objective of this project is to examine and analyze global population trends from 1960 to 2017 using historical World Bank data. By leveraging SQL querying techniques, the project isolates regional data, calculates absolute growth metrics, and identifies key geographical drivers of demographic change.

## Dataset Overview
The dataset contains historical population metrics distributed by country and regional codes over a 57-year period. It includes fields for country codes, year indicators, and absolute population counts. Non-country regional aggregates (such as high-level income groups or global totals) are selectively filtered out during analysis to focus on specific geographic dynamics.

## SQL Analysis & Implementation
The analysis uses a Common Table Expression (CTE) and conditional aggregation inside SQLite to pivot and extract the baseline year (1960) and the final year (2017) populations for comparison.

```sql
WITH PopulationBounds AS (
    SELECT
        country_code,
        MAX(CASE WHEN year = 1960 THEN population END) AS pop_1960,
        MAX(CASE WHEN year = 2017 THEN population END) AS pop_2017
    FROM
        population_data
    WHERE
        country_code NOT IN ('WLD', 'IBT', 'LMY', 'MIC', 'IBD', 'HIC', 'LTE', 'EAS', 'TE')
    GROUP BY
        country_code
)
SELECT 
    country_code,
    pop_1960,
    pop_2017,
    (pop_2017 - pop_1960) AS total_growth
FROM 
    PopulationBounds
ORDER BY 
    total_growth DESC;
```

### Key Technical Approaches
* **Common Table Expressions (CTEs)**: Used to isolate the data processing layer and create clean, reusable intermediate results.
* **Conditional Aggregation**: Used `MAX(CASE WHEN...)` statements to cleanly pivot row-based annual data into structured columns for specific comparison years.
* **Data Filtering**: Excluded broad regional aggregates (`WLD`, `IBT`, `LMY`, etc.) to maintain granular accuracy in regional analysis.

## Key Insights
* **Rapid Global Expansion**: The global population more than doubled in less than 60 years, surging from approximately 3 billion in 1960 to nearly 7.6 billion by 2017, maintaining an unbroken upward trajectory.
* **Regional Growth Drivers**: Regional analysis highlights South Asia (SAS) as a primary engine of global population growth, contributing an absolute increase of over 1.2 billion people during the study period.

## Repository Structure
* `data/`: Contains references or source links to the World Bank population dataset.
* `scripts/`: Holds the SQL query files used to aggregate and clean the data.
* `visualizations/`: Includes population trend charts and query output screenshots.

