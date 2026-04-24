# Regional Sales Data Analytics (EDA + Power BI Dashboard)

This project analyzes **XYZ Company's regional sales data (2014-2018)** to identify key revenue/profit drivers across **products, channels, and regions**, uncover seasonal patterns/outliers, and compare performance against **budget (available for 2017)**.

## What's included

1. **Exploratory Data Analysis (EDA)**: performed in `Regional_Sales_Analysis.ipynb`
2. **Power BI Dashboard**: screenshots of the dashboard are included in this repo

## Repository contents

- `Regional_Sales_Analysis.ipynb` - data cleaning, feature engineering, and EDA
- `Regional Sales Dataset.xlsx` - source dataset (Excel)
- `requirements.txt` - Python dependencies for running the notebook
- Power BI dashboard screenshots (see [Dashboard](#dashboard-power-bi))

## Tech stack

- Python (pandas, numpy, matplotlib, seaborn)
- Power BI (dashboard)
- Excel dataset (`.xlsx`)

## EDA overview (Notebook)

The notebook covers:

- Data loading (Excel), relationship/merge checks
- Data cleaning (drop redundant columns, normalize column names, handle nulls)
- Feature engineering
- Visual EDA including:
  - monthly sales trend (per year + combined)
  - top products by revenue
  - sales distribution by channel
  - average order value distribution
  - unit price distribution
  - sales by US region and top states
  - top/bottom customers by revenue

## EDA walkthrough (Detailed)

This section explains the EDA workflow implemented in `Regional_Sales_Analysis.ipynb` step-by-step.

### 1) Problem statement & objectives

The goal is to use historical sales data (2014-2018) to:

- Identify key **revenue/profit drivers** across products, channels, and regions
- Understand **seasonality** (month-wise trends) and spot potential outliers
- Align performance with **2017 budgets** (budget sheet is available only for 2017)

### 2) Data sources (Excel workbook)

The dataset is provided as `Regional Sales Dataset.xlsx` with multiple sheets that represent a simple data model:

- `Sales Orders` - transaction-level order lines (facts)
- `Customers` - customer attributes (dimension)
- `Products` - product attributes and unit costs (dimension)
- `Regions` - delivery region mapping (dimension)
- `State Regions` - state-to-region mapping (dimension)
- `2017 Budgets` - budget by product for the year 2017

In the notebook, all sheets are loaded at once via `pd.read_excel(..., sheet_name=None)` and then assigned to separate DataFrames (`df_sales`, `df_customer`, `df_products`, etc.).

### 3) Initial data quality checks (nulls / missing values)

Before joining tables, missing values are checked for each sheet using `.isnull().sum()` to understand:

- which fields have gaps,
- whether the gaps are expected (e.g., optional fields), and
- whether cleaning or filtering is needed before analysis.

### 4) Relationships (ER) and building the analysis dataset

To perform EDA effectively, the notebook creates one consolidated analysis table by merging:

- `Sales Orders` + `Customers` (customer information)
  - `left_on="Customer Name Index"` and `right_on="Customer Index"`
- + `Products` (product name, pricing/cost fields)
  - `left_on="Product Description Index"` and `right_on="Index"`
- + `Regions` (delivery region labels)
  - `left_on="Delivery Region Index"` and `right_on="id"`
- + `State Regions` (map `state_code` to `Region`)
  - join on `state_code` and `State Code`
- + `2017 Budgets` (budget values by product)
  - join `on="Product Name"`

All merges use `how="left"` so that sales rows are retained even if some dimension attributes are missing.

### 5) Data cleaning (post-merge)

After merges, the notebook cleans up duplicated keys and duplicate columns created by joins:

- Drops redundant identifier columns such as `Customer Index`, `Index`, `id`, and duplicate region/state fields.
- Renames duplicate columns like `Region_x` / `Region_y` into a single consistent `Region` column.
- Normalizes column names to lowercase using `df.columns = df.columns.str.lower()` for consistent referencing.

### 6) Keep only the analysis-ready columns

To keep the dataset focused on the business questions, the notebook retains a curated set of columns:

- Order fields: `ordernumber`, `orderdate`
- Customer fields: `customer name index`, `channel`
- Product fields: `product name`, `order quantity`, `unit price`, `total unit cost`
- Revenue fields: `line total`
- Geography fields: `county`, `state_code`, `state`, `region`, `latitude`, `longitude`
- Budget field: `2017 budgets`

`line total` is treated as the primary **revenue** metric throughout the EDA.

### 7) Budget handling (2017 only)

Because budgets are available only for 2017, the notebook blanks out budgets for non-2017 rows:

- For rows where `orderdate.year != 2017`, `2017 budgets` is set to missing (`NA`).

This avoids incorrectly comparing non-2017 performance to a 2017-only target.

For budget-focused analysis, a 2017-only subset is also created (`df_2017 = df[df['orderdate'].dt.year == 2017]`).

### 8) Feature engineering

The notebook engineers additional fields used in analysis:

- `totalcost = order quantity * total unit cost`
- `profit = line total - totalcost`
- `profit_pct = (profit / line total) * 100`

It also derives time features from `orderdate` for trend/seasonality analysis:

- `order month` as a monthly period (`.dt.to_period("M")`) for time-series aggregation
- `Month` and `Month_Num` for month-of-year seasonality plots

### 9) Visual EDA (plots and what they answer)

The notebook answers common business questions with grouped aggregations and charts:

- **Monthly sales trend (time series)**  
  Group by `order month`, sum `line total`, and plot a line chart to understand trend + year-end dips.

- **Overall monthly trend (seasonality across all years)**  
  Extract month name/number from `orderdate`, group by `Month_Num` + `Month`, sum `line total`, and plot to see which months typically perform best.

- **Top 10 products by revenue**  
  Group by `product name`, sum `line total`, sort descending, and plot a horizontal bar chart to find top contributors.

- **Sales distribution by channel**  
  Group by `channel`, sum `line total`, and visualize with a pie chart to see channel mix.

- **Order value distribution**  
  Aggregate revenue per `ordernumber` (sum of `line total`) and plot a histogram (with KDE) to understand typical order sizes and the long tail of high-value orders.

- **Unit price distribution per product**  
  Use boxplots of `unit price` by `product name` to compare pricing spread and spot outliers across products.

- **Total sales by US region**  
  Group by `region`, sum `line total`, and plot a bar chart to compare regional performance.

- **Top states by revenue and by order count**  
  Group by `state` and compute:
  - total revenue (sum of `line total`)
  - order volume (nunique of `ordernumber`)
  Then visualize top 10 for each metric.

- **Top and bottom customers by revenue**  
  Group by `customer name index`, sum `line total`, sort, and plot top 10 and bottom 10 customers to understand concentration and low-value accounts.

> Tip: The notebook uses `.dt` accessors on `orderdate`. If your environment reads that column as text, convert it once with `pd.to_datetime(df['orderdate'])` before running the time-based analysis cells.

## Dashboard (Power BI)

Below are the Power BI dashboard screenshots included in this repo:

- [Power BI Dashboard 1](https://github.com/user-attachments/assets/9155050e-e7fb-4077-af36-a015bc8b7123)
- [Power BI Dashboard 2](https://github.com/user-attachments/assets/09149354-afe5-41dc-a66f-1ddbd1b8df23)
- [Power BI Dashboard 3](https://github.com/user-attachments/assets/9f083160-91c5-44ff-947b-d9e0b23d973e)
- [Power BI Dashboard 4](https://github.com/user-attachments/assets/0e14d091-06a3-445f-bbec-643ae5036b7b)

## Key insights (from the EDA)

- Monthly revenue is largely stable (roughly **INR 23M-26M**) with a recurring **dip toward year-end**.
- **January** appears as a peak sales month; sales fall after January and recover around **May/August**.
- Most orders are **small-to-medium value**, with a smaller number of high-value/bulk purchases.

## How to run

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
# source .venv/bin/activate

pip install -r requirements.txt
jupyter notebook
```

Open `Regional_Sales_Analysis.ipynb` and run the cells.

> Note: If the notebook uses an absolute path for the Excel file on your machine, update it to a relative path like `Regional Sales Dataset.xlsx` after cloning.
