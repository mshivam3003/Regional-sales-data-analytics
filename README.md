# Regional Sales Data Analytics (EDA + Power BI Dashboard)

This project analyzes **XYZ Company's regional sales data (2014-2018)** to identify key revenue/profit drivers across **products, channels, and regions**, uncover seasonal patterns/outliers, and compare performance against **budget (available for 2017)**.

## What's included

1. **Exploratory Data Analysis (EDA)**: performed in `Regional_Sales_Analysis.ipynb`
2. **Power BI Dashboard**: screenshots of the dashboard are included in this repo

## Repository contents

- `Regional_Sales_Analysis.ipynb` - data cleaning, feature engineering, and EDA
- `Regional Sales Dataset.xlsx` - source dataset (Excel)
- `requirements.txt` - Python dependencies for running the notebook
- <img width="1609" height="838" alt="Screenshot 2026-04-24 154923" src="https://github.com/user-attachments/assets/9155050e-e7fb-4077-af36-a015bc8b7123" />
 - Power BI dashboard image
- <img width="1596" height="855" alt="Screenshot 2026-04-24 154933" src="https://github.com/user-attachments/assets/09149354-afe5-41dc-a66f-1ddbd1b8df23" />
 - Power BI dashboard image
- <img width="1602" height="832" alt="Screenshot 2026-04-24 154944" src="https://github.com/user-attachments/assets/9f083160-91c5-44ff-947b-d9e0b23d973e" />
- Power BI dashboard image
- <img width="1604" height="828" alt="Screenshot 2026-04-24 155001" src="https://github.com/user-attachments/assets/0e14d091-06a3-445f-bbec-643ae5036b7b" />
- Power BI dashboard image

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

## Dashboard (Power BI)

Below are the Power BI dashboard screenshots included in this repo:

![Power BI Dashboard 1](<Screenshot 2026-04-24 154923.png>)
![Power BI Dashboard 2](<Screenshot 2026-04-24 154933.png>)
![Power BI Dashboard 3](<Screenshot 2026-04-24 154944.png>)
![Power BI Dashboard 4](<Screenshot 2026-04-24 155001.png>)

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
