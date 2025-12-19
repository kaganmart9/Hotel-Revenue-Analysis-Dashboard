# Hotel Revenue Analysis Dashboard

![Hotel Revenue Dashboard](visual/dashboard_pic.png)

## Project Overview

This Power BI dashboard analyzes booking and revenue data for two hotels (a **City Hotel** and a **Resort Hotel**) between 2018 and 2020.

It's a project I built while learning and practicing Power BI. I combined data from multiple years using SQL, modeled it in Power BI, calculated some key hotel metrics, and created interactive visualizations to explore the trends.

Total revenue in the dataset is around **$10.23 million**. The dashboard shows differences between the two hotel types and highlights patterns like seasonal peaks and the big drop in 2020.

## Key Insights

Here are some of the main things I noticed while exploring the data:

- **Revenue Split**  
  - Resort Hotel: ~$5.48M (53.6%)  
  - City Hotel: ~$4.75M (46.4%)  
  - Resort earns more even though both hotels have similar total nights sold, mainly because of higher average rates.

- **Seasonality**  
  - Clear summer peaks, especially for the Resort Hotel  
  - City Hotel has more consistent demand year-round  
  - Sharp decline from early 2020 onwards

- **Average Daily Rate (ADR)**  
  - Overall average: ~$104  
  - Resort Hotel usually has higher rates

- **Parking Spaces**  
  - Guests requested parking ~8,669 times – something interesting for operations

- **Yearly Revenue**  
  - 2018: ~$4.25M  
  - 2019: ~$4.59M  
  - 2020: ~$1.40M (much lower)

These findings show how pricing, seasonality, and external events affect hotel performance.

## Data Sources

- Raw data: `data/hotel_revenue_historical_full-2.xlsx`  
  - Separate sheets for 2018, 2019, and 2020 bookings  
  - Small tables for meal costs and market segment discounts

The data includes details like booking dates, room types, ADR, cancellations, parking requests, etc.

## SQL Data Preparation

The data was split across yearly Excel sheets, so I used this SQL query to bring everything together and join the small reference tables:

```sql
    WITH hotels AS (
        SELECT * FROM dbo.['2018$']
        UNION ALL
        SELECT * FROM dbo.['2019$']
        UNION ALL
        SELECT * FROM dbo.['2020$']
    )
    SELECT 
        h.*,
        ms.Discount AS market_segment_discount,
        mc.Cost AS meal_cost
    FROM hotels h
    LEFT JOIN dbo.market_segment$ ms 
        ON h.market_segment = ms.market_segment
    LEFT JOIN dbo.meal_cost$ mc 
        ON h.meal = mc.meal;

    /* Example revenue calculation query used during exploration */
    SELECT
        arrival_date_year,
        hotel,
        ROUND(SUM((stays_in_week_nights + stays_in_weekend_nights) * adr), 0) AS revenue
    FROM hotels
    GROUP BY arrival_date_year, hotel;
```

File: `queries/hotel_revenue_query.sql`

## Power BI Part

- Loaded the combined data and reference tables
- Created measures like Total Revenue, ADR, and Parking Spaces Required
- Built cards, donut chart, line chart for trends, and a detail table
- Added basic interactivity (slicers, etc.)

Power BI file: `Hotel_Revenue_Analysis.pbix` (open with Power BI Desktop)

## Tools I Used

- SQL for combining the data
- Excel as the source
- Power BI for everything else
- GitHub to share the project

## How to Check It Out

1. Download the repo
2. Open the .pbix file in Power BI Desktop
3. Play around with the dashboard

## What I Plan to Add Next

- Year-over-year comparisons
- More filters (e.g., by country)
- Cancellation analysis
- Maybe some forecasting

## Contact

Questions or feedback are welcome!  

LinkedIn: [https://www.linkedin.com/in/kaganmart9/]  
Email: [dev.alikaganmart@gmail.com]

Thanks for taking a look!
