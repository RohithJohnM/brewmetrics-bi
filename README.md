# BrewMetrics Power BI Analytics

BrewMetrics is a version-controlled Power BI analytics solution for **BrewMetrics Coffee Co.** The project presents an interactive view of sales performance across time, cities, and products.

## 1. Project Overview

The BrewMetrics Power BI project demonstrates:

- A structured star-schema semantic model
- Reusable DAX measures for sales analysis
- Interactive filtering using an item slicer
- Time-based analysis using a date hierarchy
- Dashboard reporting through cards, charts, and slicers
- Version control using the Power BI Project (`.pbip`) format

The main project file is `BrewMetrics.pbip`. The report definition is stored in `BrewMetrics.Report/`, while the semantic model is stored in `BrewMetrics.SemanticModel/`.

## 2. Data Model

The solution uses a **star schema**, with `Fact_Sales` as the central fact table connected to descriptive dimension tables for date, city, and product analysis.

### Tables

| Table | Purpose |
|---|---|
| `Fact_Sales` | Central fact table containing sales transaction data, including sale date, city, item, sale ID, and sales amount. |
| `Dim_Date` | Date dimension used for time analysis. It contains `Year`, `Month Name`, and `Day` fields and supports the `Year → Month Name → Day` hierarchy. |
| `Dim_City` | City dimension used for city-level sales analysis. |
| `Dim_Product` | Product dimension used for item/product analysis. |

### Relationships

The model connects:

- `Dim_Date` to `Fact_Sales` through the date field.
- `Dim_City` to `Fact_Sales` through the city field.
- `Dim_Product` to `Fact_Sales` through the item field.

This structure allows fields from the dimension tables to filter the sales data in `Fact_Sales`.

## 3. DAX Measures

The following measures were created for the report.

### Month-over-Month Sales Growth %

Calculates the percentage change between current-month sales and the previous month's sales.

```DAX
Month-over-Month Sales Growth % =
VAR CurrentMonthSales =
    SUM ( Fact_Sales[sales_amount] )
VAR PreviousMonthSales =
    CALCULATE (
        SUM ( Fact_Sales[sales_amount] ),
        PREVIOUSMONTH ( Dim_Date[date] )
    )
RETURN
    DIVIDE (
        CurrentMonthSales - PreviousMonthSales,
        PreviousMonthSales
    )
```

### Cumulative Sales

Calculates sales accumulated through the current date in the active date context.

```DAX
Cumulative Sales =
VAR CurrentDate =
    MAX ( Dim_Date[date] )
RETURN
    CALCULATE (
        SUM ( Fact_Sales[sales_amount] ),
        FILTER (
            ALL ( Dim_Date[date] ),
            Dim_Date[date] <= CurrentDate
        )
    )
```

### Item Sales Rank

Ranks items by sales amount, with the highest-selling item receiving rank 1.

```DAX
Item Sales Rank =
RANKX (
    ALL ( Fact_Sales[item] ),
    CALCULATE ( SUM ( Fact_Sales[sales_amount] ) ),
    ,
    DESC,
    Dense
)
```

### Average Sale Amount

Calculates the average sales amount per distinct sale.

```DAX
Average Sale Amount =
DIVIDE (
    SUM ( Fact_Sales[sales_amount] ),
    DISTINCTCOUNT ( Fact_Sales[sale_id] )
)
```

## 4. Dashboard

The report page is titled **"BrewMetrics | Cold Brew Performance Dashboard"**.

### Visuals and Features

- **Cold Brew Sales Trend** — line chart showing monthly Cold Brew sales for April, May, June, and July.
- **Sales by City** — column chart showing sales across Bengaluru, Chennai, Hyderabad, and Coimbatore.
- **Sales by Item** — horizontal bar chart comparing sales across products.
- **Select Item** — slicer for interactive item filtering.
- **Average Sales Amount** — card showing the average sale amount.
- **Total Sales Amount** — card showing total sales.
- **Cumulative Sales** — card showing cumulative sales.
- **Date hierarchy** — `Year → Month Name → Day` for time-based drill-down.

### Dashboard Values

#### Cold Brew Monthly Sales

| Month | Sales Amount |
|---|---:|
| April | 276,593.46 |
| May | 301,280.66 |
| June | 185,282.81 |
| July | 8,104.77 |

#### Sales by City — All Items Selected

| City | Sales Amount |
|---|---:|
| Bengaluru | 1,119,895.31 |
| Chennai | 1,054,801.19 |
| Hyderabad | 968,966.26 |
| Coimbatore | 822,665.12 |

#### Sales by Item

| Item | Sales Amount |
|---|---:|
| Cold Brew | 771,261.70 |
| Tumbler | 628,389.11 |
| Coffee Beans Pack | 507,736.20 |
| Cappuccino | 457,148.37 |
| Mug | 403,358.87 |
| Espresso | 384,970.70 |
| Brownie | 259,016.87 |
| Croissant | 203,370.21 |
| Filter Coffee | 191,007.20 |
| Muffin | 160,073.65 |

The dashboard shows **Total Sales Amount of approximately 3.97M**.

The dashboard also shows an **Average Sales Amount of 256.19**.

Cold Brew has an **Item Sales Rank of 1**.

## 5. Key Insights

1. **Cold Brew is the highest-ranked item**, with sales of **771,261.70** and an **Item Sales Rank of 1**.

2. **Bengaluru records the highest sales among the four cities**, with **1,119,895.31**, while Coimbatore records the lowest, with **822,665.12**.

3. In the displayed Cold Brew monthly trend, **May has the highest sales** at **301,280.66**. Sales then decrease to **185,282.81 in June** and **8,104.77 in July**.

These insights describe the values displayed in the dashboard and do not infer causes or customer behavior beyond the available data.

## 6. Tools and Technologies

- **Microsoft Power BI Desktop** — report development, data modeling, DAX, and visualisation.
- **Power BI Project (`.pbip`) format** — version-controlled project structure.
- **DAX (Data Analysis Expressions)** — calculated measures and analytical logic.
- **Power Query / M** — data loading and transformation.
- **Tabular Model Definition Language (TMDL)** — semantic model definitions.
- **Git** — source control and project versioning.