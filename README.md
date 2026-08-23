# Madhav Ecommerce Sales Dashboard

![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=flat&logo=powerbi&logoColor=black)
![Data](https://img.shields.io/badge/Data-2018%20Sales-2563EB?style=flat)
![Records](https://img.shields.io/badge/Orders-500-10B981?style=flat)
![Detail Rows](https://img.shields.io/badge/Detail%20Rows-1500-8B5CF6?style=flat)

## 1. Project Overview

**Madhav Ecommerce Sales Dashboard** is an interactive Power BI report designed to analyze ecommerce sales performance across **time, geography, customers, product categories, sub-categories, and payment modes**.

The dashboard converts transactional order data into a management-friendly view of:

- Revenue / sales amount
- Profit and monthly profit trend
- Quantity sold
- Average order value
- State-wise sales contribution
- Category-wise quantity distribution
- Sub-category profitability
- Customer-level sales contribution
- Payment-mode mix
- Quarter and state filtering

The report is designed as a **single-page executive dashboard** with a 1280 × 720 canvas, KPI cards, charts, and interactive slicers.

---

## 2. Business Objective

The primary objective is to answer five business questions:

1. **How much are we selling and how profitable are those sales?**
2. **Which states and customers contribute most to sales?**
3. **Which product categories and sub-categories drive volume and profit?**
4. **How does profitability change month by month?**
5. **Which payment modes are most frequently used?**

This makes the dashboard suitable for **sales performance review, product analysis, regional analysis, and management reporting**.

---

## 3. Dashboard Preview

![Madhav Ecommerce Sales Dashboard](Dashboard_Screenshot.png)
<img width="1750" height="747" alt="Screenshot" src="https://github.com/user-attachments/assets/a34ac423-9095-457a-acd7-802701af47eb" />


### Dashboard layout

The report contains:

| Component | Purpose |
|---|---|
| **4 KPI Cards** | Amount, Profit, Quantity and AOV |
| **Quarter Slicer** | Qtr 1–Qtr 4 filtering |
| **State Slicer** | State-level filtering |
| **Profit by Month** | Monthly profitability trend |
| **Profit by Sub-Category** | Top profitable sub-categories |
| **Amount by State** | Regional sales contribution |
| **Quantity by Category** | Product category volume mix |
| **Amount by Customer** | Top customer sales contribution |
| **Quantity by Payment Mode** | Payment behavior mix |

---

## 4. Data Sources

The project uses two logical datasets:

### Ordersa

Contains order-level context:

- `Order ID`
- `Order Date`
- `CustomerName`
- `State`
- `City`

**Profile:** 500 unique orders, 19 states, 25 cities, covering **01 Jan 2018 to 31 Dec 2018**.

### Details

Contains order-line/product-level metrics:

- `Order ID`
- `Amount`
- `Profit`
- `Quantity`
- `Category`
- `Sub-Category`
- `PaymentMode`

**Profile:** 1,500 detail rows, 500 unique orders, 3 categories, 17 sub-categories and 5 payment modes.

### Data quality checks

The supplied datasets contain:

- **0 null values** across the two primary tables
- **0 exact duplicate rows** in `Details`
- All 1,500 detail rows successfully match an `Order ID` in `Ordersa`
- Detail records are intentionally at a finer grain than orders; therefore, aggregations should be performed from `Details` for sales metrics and from `Ordersa` for order/customer/geography context.

---

## 5. Data Model

![Data Model](Data_Model.png)

The report uses an **Order ID-based relationship** between the order context table and the transaction-detail table.

### Logical grain

```text
Ordersa
    1
    |
    | Order ID
    |
    *
Details
```

- `Ordersa` = order/customer/geography/time context
- `Details` = sales/product/payment metrics

This separation allows the dashboard to combine customer/state/date attributes with transaction-level measures.

> **Modeling note:** In a production-grade Power BI implementation, a dedicated Date dimension and explicit star-schema design would be preferable for larger datasets and more advanced time intelligence.

---

## 6. KPI Definitions

### Total Sales Amount

**As displayed in the dashboard:**

`SUM(Details[Amount])`

Current dataset result: **₹437,771**

### Total Profit

`SUM(Details[Profit])`

Current dataset result: **₹36,963**

### Total Quantity

`SUM(Details[Quantity])`

Current dataset result: **5,615 units**

### AOV — important metric note

The PBIX visual references an `AOV` field in the `Details` table. Based on the source values and the dashboard's displayed magnitude, this corresponds to a **line-level AOV calculation** of approximately:

`Amount / Quantity`

The sum of these line-level AOV values is approximately **120,899**.

However, for a business-facing ecommerce dashboard, the more standard **Average Order Value** is:

```DAX
AOV =
DIVIDE(
    SUM(Details[Amount]),
    DISTINCTCOUNT(Ordersa[Order ID])
)
```

For the supplied data, this gives approximately **₹875.54 per order**.

**Recommendation:** use the second definition for a production dashboard because it represents the average revenue generated per order rather than the sum of row-level AOVs.

---

## 7. Dashboard Visual Analysis

### 7.1 Profit by Month

The monthly column chart compares positive and negative profit.

Key observations:

- **November** is the strongest month with profit of **₹10,253**.
- **January** is the second strongest month at **₹9,684**.
- **May** is the weakest month at **₹-3,730**.
- July, September and December also show negative monthly profitability.
- The chart uses contrasting positive/negative data colors, making loss periods immediately visible.

This visual is particularly useful for identifying **seasonality, weak trading periods and profitability volatility**.

---

### 7.2 Amount by State

The dashboard shows the four highest-sales states.

| Rank | State | Sales Amount |
|---:|---|---:|
| 1 | Maharashtra | ₹102,498 |
| 2 | Madhya Pradesh | ₹87,463 |
| 3 | Uttar Pradesh | ₹38,362 |
| 4 | Delhi | ₹22,957 |

Maharashtra and Madhya Pradesh together contribute approximately **43.4%** of total sales.

This indicates a relatively concentrated regional sales contribution.

---

### 7.3 Quantity by Category

| Category | Quantity | Share |
|---|---:|---:|
| Clothing | 3,516 | 62.62% |
| Electronics | 1,154 | 20.55% |
| Furniture | 945 | 16.83% |

**Clothing dominates unit volume**, contributing approximately **62.6%** of total quantity.

This is a strong example of why **sales amount and quantity should be analyzed separately**: a category can dominate volume without necessarily producing the highest revenue or profit.

---

### 7.4 Profit by Sub-Category

Top profitable sub-categories:

| Rank | Sub-Category | Profit |
|---:|---|---:|
| 1 | Printers | ₹8,606 |
| 2 | Bookcases | ₹6,516 |
| 3 | Saree | ₹4,057 |
| 4 | Accessories | ₹3,353 |
| 5 | Tables | ₹3,139 |

The dashboard correctly emphasizes **profitability rather than only sales volume**, which makes this visual more useful for margin-oriented decision-making.

There are also loss-making sub-categories:

- **Leggings**: ₹-130
- **Skirt**: ₹-315
- **Kurti**: ₹-401
- **Electronic Games**: ₹-644
- **Furnishings**: ₹-806

These loss-making sub-categories collectively contribute **₹2,296 of negative profit**, making them candidates for pricing, discount, sourcing or assortment review.

---

### 7.5 Amount by Customer

Top customers by sales amount:

| Rank | Customer | Sales |
|---:|---|---:|
| 1 | Harivansh | ₹9,902 |
| 2 | Madhav | ₹9,365 |
| 3 | Madan Mohan | ₹7,766 |
| 4 | Shiva | ₹6,339 |

The visual focuses attention on the highest-value customers and can support **customer segmentation and retention analysis**.

---

### 7.6 Quantity by Payment Mode

| Payment Mode | Quantity | Share |
|---|---:|---:|
| COD | 2,456 | 43.74% |
| UPI | 1,157 | 20.61% |
| Debit Card | 741 | 13.20% |
| Credit Card | 672 | 11.97% |
| EMI | 589 | 10.49% |

**COD is the dominant payment mode**, representing approximately **43.7%** of units.

UPI is the second-largest mode at approximately **20.6%**.

---

## 8. Executive Insights

### Sales performance

- Total sales: **₹437,771**
- Total profit: **₹36,963**
- Total quantity: **5,615 units**
- Orders: **500**
- Overall profit margin: approximately **8.44%**
- Standard order-level AOV: approximately **₹875.54**

### Regional performance

- Maharashtra is the largest sales state.
- Madhya Pradesh is the second-largest.
- The top two states together account for approximately **43.4%** of sales.

### Product performance

- Clothing dominates unit volume.
- Printers are the strongest profit-generating sub-category.
- Several sub-categories are loss-making and should be reviewed separately rather than being hidden inside overall profit.

### Time performance

- November is the strongest profitability month.
- May is the largest negative-profit month.
- Quarterly results show that **Q1 is the strongest profit quarter**, while **Q3 is loss-making**.

### Payment behavior

- COD is the dominant payment method.
- Digital payments, particularly UPI, represent a significant secondary channel.

---

## 9. Quarter-Level Performance

| Quarter | Sales | Profit | Quantity |
|---|---:|---:|---:|
| Q1 | ₹161,288 | ₹25,942 | 2,008 |
| Q2 | ₹87,081 | ₹882 | 1,181 |
| Q3 | ₹71,741 | ₹-1,469 | 1,017 |
| Q4 | ₹117,661 | ₹11,608 | 1,409 |

**Interpretation:**

- **Q1** generates the highest profit at approximately **₹25,942**.
- **Q3** is the weakest quarter and records approximately **₹-1,469** in losses.
- Q4 recovers strongly, supported by November's high profitability.

---

## 10. Power BI Features Demonstrated

This project demonstrates practical Power BI capabilities including:

- Data loading from CSV/Excel sources
- Multi-table data modeling
- Relationship through `Order ID`
- Aggregation of sales metrics
- Date hierarchy and month-level analysis
- Quarter slicer
- State slicer
- KPI cards
- Column charts
- Bar charts
- Donut charts
- Conditional formatting for positive/negative profit
- Top-N style visual filtering
- Interactive cross-filtering
- Executive dashboard layout and visual hierarchy
- Custom background/theme styling

---

## 11. Recommended Improvements

The current dashboard is visually strong for a portfolio project, but the following improvements would make it more production-ready.

### Data model

1. Add a dedicated `DimDate` table.
2. Use a proper star schema.
3. Keep dimension attributes such as Customer, State and Category in dimension tables where appropriate.
4. Avoid relying on Power BI auto date hierarchy for long-term reporting.

### Measures

Use explicit DAX measures rather than relying heavily on implicit `SUM` aggregations.

Example:

```DAX
Total Sales =
SUM(Details[Amount])

Total Profit =
SUM(Details[Profit])

Total Quantity =
SUM(Details[Quantity])

Total Orders =
DISTINCTCOUNT(Ordersa[Order ID])

Profit Margin % =
DIVIDE([Total Profit], [Total Sales])

AOV =
DIVIDE([Total Sales], [Total Orders])
```

### KPI design

Replace the current line-level AOV aggregation with the order-level AOV definition described above.

### Dashboard usability

- Add a **Reset Filters** button.
- Add KPI cards for **Profit Margin %** and **Total Orders**.
- Consider a tooltip page for detailed state/category analysis.
- Add dynamic titles showing the selected quarter/state.
- Add a small variance indicator such as **MoM Profit %**.
- Consider a dedicated detail page for loss-making products/sub-categories.

### Visual design

The dark gradient theme is visually distinctive, but some axis labels have low contrast against the background. Improving axis/title contrast and reducing unnecessary visual text would improve accessibility and readability.

---

## 12. Suggested Portfolio Description

> Built an interactive Power BI ecommerce sales dashboard to analyze revenue, profitability, quantity, customer contribution, regional sales, product performance and payment behavior. Integrated order-level and transaction-detail datasets using Order ID, implemented KPI-driven reporting and interactive Quarter/State filtering, and designed an executive-style single-page dashboard for sales performance analysis.

---

## 13. Project Structure

```text
Madhav-Ecommerce-Sales-Dashboard/
│
├── MADHAV SALES DASHBOARD.pbix
├── Orders1.csv
├── Details.csv
├── Orders.xlsm
├── Screenshot.png
└── README.md
```

---

## 14. Key Takeaway

This project demonstrates the complete flow from **raw ecommerce transaction data → data modeling → KPI calculation → business analysis → interactive Power BI visualization**.

The strongest portfolio aspect is not only the dashboard's visual design, but the ability to use the dashboard to identify:

- where sales are concentrated,
- which products generate profit,
- where losses occur,
- when profitability changes,
- which customers contribute most,
- and how customers pay.

That turns the report from a collection of charts into a **business decision-support dashboard**.

---

## Author

**Neeraj Sharma**

**Focus:** Data Analytics | Power BI | SQL | Python | Data Engineering

