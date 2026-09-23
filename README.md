# Sales Performance & Profitability Analysis (Power BI)

A Power BI project analyzing sales and profitability across regions, products, and customer segments — including a real data quality investigation that changed the business conclusion entirely.

## 📊 Project Overview

This project analyzes ~1,952 orders across 4 regions, 3 product categories, and 4 customer segments, using two source files:

- **Orders.xlsx** — transactional order-level data (sales, profit, discount, shipping, dates, etc.)
- **Users.xlsx** — region-to-manager mapping

The dashboard is built across **4 pages**:

### 1. Overview
- 5 KPI cards: Total Sales, Total Profit, Profit Margin %, Total Orders, Total Quantity
- Sales & Profit Trend (combo column + line chart, by month)
- Sales by Region (bar chart)
- Sales by Category (donut chart)
- Avg Shipping Days by Order Priority (bar chart)

### 2. Regions & Managers
- Region Performance table (Region, Manager, Total Sales, Total Profit, Profit Margin %, Total Orders) with colored data bars, so each manager's name sits directly next to their numbers
- Profit by Region × Category (matrix)
- Sales vs. Profit by Region (clustered column chart)
- Quantity Sold by Region (area chart)

### 3. Products
- Category/Sub-Category performance table with colored data bars
- Sales Share by Category (treemap)
- Top 5 / Bottom 5 Products by Profit (bar chart, toggled with bookmarks)

### 4. Customers & Shipping
- 5 KPI cards: Total Customers, Avg Order Value, Avg Shipping Cost, Total Shipping Cost, Avg Shipping Days
- Customer Segment performance table
- Ship Mode × Category (matrix, showing order counts)
- Orders by Ship Mode (bar chart)
- Avg Shipping Cost by Ship Mode (bar chart), and a Profit Contribution by Ship Mode waterfall chart

## 🔍 The Key Finding: A Data Quality Issue That Changed the Story

Early in the analysis, the **South region** appeared to be losing money (-$14.4K profit, -4.0% margin), which could easily have led to a decision to replace its manager.

Instead of accepting the number at face value, I drilled down:

- Broke profit down by Region × Category in a matrix → found **93% of South's loss came from the Technology category alone**
- Isolated the Technology orders in South → found **a single order with $1,486 in sales but -$16,477 in recorded profit** — a loss 11x larger than the sale itself, which is not commercially possible
- Built a calculated column (`Profit ÷ Sales` ratio) to scan the **entire dataset** for similarly impossible values (ratio > 100% or < -100%)
- Found **439 out of 1,952 rows (22.5% of the data)** had unrealistic profit values in both directions (extreme losses and extreme, implausible gains)

**After filtering out the 439 suspicious rows:**

| Metric | Before | After |
|---|---|---|
| Total Profit | $224K | **$377K** |
| Profit Margin | 11.6% | **20.7%** |
| South Region Profit | -$14.4K (loss) | **+$457 (break-even)** |

The conclusion: South was never actually losing money. The original numbers were distorted by a systemic data quality issue, not a performance problem. Any management decision based on the uncleaned data (e.g., reassigning the regional manager) would have been wrong.

## 🛠️ Other Insights

- **Products:** A single product ("High Speed Automatic Electric Letter Opener") accounted for ~97% of the loss in the Scissors/Rulers/Trimmers sub-category — the rest of the sub-category was roughly break-even.
- **Shipping:** Delivery Truck has the highest average shipping cost ($44 vs. $8–$9 for air modes) — not because the mode is inherently expensive, but because 73% of its usage is tied to shipping Furniture.
- **Operations:** Orders marked "Critical" priority averaged 1.43 days to ship — statistically no faster than "High" (1.38 days) or "Medium" (1.41 days) priority orders, suggesting the priority tagging isn't translating into actual operational prioritization.

## 🧰 Tools & Techniques Used

- **Power Query:** data cleaning (Trim/Clean), duplicate key resolution, custom columns, row filtering
- **Data Modeling:** star-schema relationships (Orders ↔ Users ↔ Calendar), a dedicated Calendar table for time intelligence
- **DAX:** Measures for sales/profit/margin, `DIVIDE`, `CALCULATE`, `DATEADD`, calculated columns for row-level ratio analysis, plus Top N visual-level filtering for the Top 5 / Bottom 5 Products chart
- **Report Design:** bookmarks (Top 5 / Bottom 5 toggle), conditional formatting (data bars, rule-based coloring), waterfall charts, treemaps, matrix visuals, slicers

## 📁 Files

- `Orders.xlsx` — raw order-level data
- `Users.xlsx` — region-to-manager mapping
- `Sales Performance & Profitability.pbix` — the Power BI report

## 📌 Key Takeaway

Data quality checks aren't an optional extra step in analysis — they're what separates a correct conclusion from a confidently wrong one. A 22.5% data quality issue was hiding inside numbers that otherwise looked completely reasonable at a glance.
