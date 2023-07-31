# DataCamp Project: Analysis of High-Growth Companies in Top-Performing Industries

## Overview
This project aims to analyze trends in high-growth companies and identify the top-performing industries based on the number of new unicorns created in 2019, 2020, and 2021 combined. The analysis provides valuable insights to an investment firm, assisting them in understanding industry trends and structuring their portfolio for the future.

## Dataset
The project utilizes a database containing the following tables:

1. **dates**: Contains information about company join dates, company IDs, and the year of foundation.
2. **funding**: Includes data about company valuations, funding amounts, and key investors.
3. **industries**: Contains information about the industries that each company operates in.
4. **companies**: Provides details about each company, including their names, headquarters' city, country, and continent.

## Project Steps

### Step 1: Finding the Top Industries
Objective: Identify the top-performing industries based on the volume of new unicorns created in 2019, 2020, and 2021 combined.

```sql
SELECT i.industry, COUNT(i.*) AS num_unicorns
FROM dates d
JOIN industries i ON d.company_id = i.company_id
WHERE EXTRACT(YEAR FROM d.date_joined) BETWEEN 2019 AND 2021
GROUP BY i.industry
ORDER BY num_unicorns DESC
LIMIT 3;
```

### Step 2: Gathering Yearly Rankings Data
Objective: Build a second Common Table Expression (CTE) named "yearly_rankings" to gather data on the number of unicorns, industry, year, and average valuation for each industry in the years 2019, 2020, and 2021.

```sql
SELECT COUNT(i.company_id) AS num_unicorns, i.industry, EXTRACT(YEAR FROM d.date_joined) AS year, ROUND(AVG(f.valuation), 2) AS average_valuation
FROM industries AS i 
JOIN dates AS d ON i.company_id = d.company_id
JOIN funding AS f ON d.company_id = f.company_id
WHERE EXTRACT(YEAR FROM d.date_joined) BETWEEN 2019 AND 2021
GROUP BY i.industry, year
ORDER BY average_valuation DESC, num_unicorns DESC;
```

### Step 3: Returning the Final Results
Objective: Combine the results from Step 1 and Step 2 into a final query that returns a table containing industry, year, number of unicorns, and average valuation (in billions of dollars, rounded to two decimal places). Sort the results by year and the number of unicorns in descending order.

```sql
WITH top_industries AS (
    -- Step 1 code (Identify top industries)
),
yearly_rankings AS (
    -- Step 2 code (Gather yearly rankings data)
)
SELECT industry, year, num_unicorns, ROUND(AVG(average_valuation) / 1000000000, 2) AS average_valuation_billions
FROM yearly_rankings
WHERE industry IN (SELECT industry FROM top_industries)
GROUP BY industry, year, num_unicorns
ORDER BY year DESC, num_unicorns DESC;
```

## Execution and Results
To execute the final query, run the SQL code in your preferred SQL environment or database interface. The output will provide valuable insights into the top-performing industries and their high-growth companies in the years 2019, 2020, and 2021. The results are sorted by year and the number of unicorns, helping the investment firm make informed decisions regarding its portfolio structure.

## Conclusion
This project offers a comprehensive analysis of high-growth companies in top-performing industries. By identifying the trends in new unicorns and their average valuations, the investment firm gains a competitive edge, enabling them to capitalize on opportunities and achieve successful investment outcomes.
