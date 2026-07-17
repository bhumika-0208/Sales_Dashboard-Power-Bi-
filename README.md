# Sales Performance Dashboard — Power BI

An interactive 2-page Power BI dashboard analyzing 1,500 sales transactions (2022–2026) across regions, product categories, customer segments, and payment methods.

## 📌 Project Overview

This dashboard was built to practice end-to-end analyst workflow: cleaning raw data, building DAX measures, designing a clear visual story, and validating every KPI against the underlying data rather than trusting default aggregations.

**Dataset:** 1,500 rows | Order_ID, Order_Date, Product_Name, Category, Region, Customer_Type, Payment_Method, Quantity, Unit_Price, Total_Sales

## 🛠️ Tools Used

- **Power BI Desktop** — report building
- **Power Query** — data cleaning (typo correction, duplicate removal, date locale fix)
- **DAX** — custom measures (DIVIDE, CALCULATE, TOPN, COUNTROWS, DISTINCTCOUNT)

## 📄 Page 1 — Overview

- KPI cards: Avg Order Value, Total Units Sold, Total Orders, Total Revenue
- Year slicer
- Total Sales by Region (bar chart)
- Revenue vs Avg Order Value by Customer Type (dual-axis combo chart)
- Total Sales by Category (donut chart)
- Key insight callout

## 📄 Page 2 — Deep Dive

- Category and Year slicers (synced with Page 1)
- Top 5 Products by Revenue (bar chart, Top N filter)
- Total Sales by Year and Category (100% stacked column — trend view)
- Sales by Payment Method (donut chart)
- Key insight callouts

Two-way page navigation buttons link both pages.

## 🔑 Key Insights

- **Category concentration:** Electronics drives ~75% of total revenue, a share that has stayed stable (72–78%) every year since 2022 — this is a structural pattern, not a recent trend.
- **Product concentration:** Laptop alone generates over 40% of revenue — more than double the next best-selling product (Tablet). The top 5 products account for ~93% of total sales.
- **Regional gap:** Pune generates over 2x the revenue of the lowest-performing region, Delhi.
- **Customer behavior:** New customers have a higher Average Order Value (₹19.8K) than Premium customers (₹18.4K) — a counterintuitive finding worth investigating for retention/upsell strategy.
- **Payment method:** Cash on Delivery leads in both revenue share (43.4%) and average order value, challenging the common assumption that COD correlates with smaller, lower-trust purchases.

## 🐞 Data Debugging Highlight

While validating the Average Order Value KPI, the number (₹56.5K) didn't reconcile with Total Revenue ÷ Total Orders (₹18.8K expected). Investigation traced the issue to the `Order_ID` column: IDs were being reused across transactions with completely different dates, meaning `DISTINCTCOUNT(Order_ID)` undercounted real orders (499 instead of 1,500).

**Fix:** Replaced `DISTINCTCOUNT(Order_ID)` with `COUNTROWS(sales_dataset_dashboard)` in the measure, since each row represents a genuinely separate transaction:

```DAX
Avg Order Value = DIVIDE(
    SUM(sales_dataset_dashboard[Total_Sales]),
    COUNTROWS(sales_dataset_dashboard)
)
```

## 🧮 Other Key DAX Measures

```DAX
Top Product = CALCULATE(
    SUM(sales_dataset_dashboard[Total_Sales]),
    TOPN(1, VALUES(sales_dataset_dashboard[Product_Name]),
         SUM(sales_dataset_dashboard[Total_Sales]), 0)
)
```

## 🧹 Data Cleaning Steps

- Corrected "Premimum" → "Premium" in Customer_Type
- Removed duplicate rows
- Set Order_Date locale to ensure correct DD-MM-YYYY parsing

## 📂 Files

- `Sales_Dashboard.pbix` — Power BI report file
- `sales_dataset_dashboard.csv` — source dataset

## 👩‍💻 About Me

Aspiring Data Analyst | BCA Graduate | Building portfolio projects in Power BI, SQL, Python, and Excel.

Connect with me on [LinkedIn : https://www.linkedin.com/in/bhumika-kumari-a92a51262/] — happy to walk through the design decisions and DAX logic behind this dashboard.
