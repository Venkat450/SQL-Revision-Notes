# Amazon Marketplace Analytics: Seller Fee and Sales Analysis

## Objective

As a Data Analyst on the Amazon Marketplace Analytics team, the goal is to analyze how transaction amounts and fee percentages impact seller performance. This involves identifying top sales, summarizing weekly trends, and calculating cumulative transaction counts to guide strategic fee adjustments and enhance marketplace efficiency.

---

## Table Structures

- **fct\_seller\_sales(sale\_id, seller\_id, sale\_amount, fee\_amount\_percentage, sale\_date)**
- **dim\_seller(seller\_id, seller\_name)**

---

## Query 1: Identify Top Sale Transaction Per Seller (April 2024)

### Problem:

Find the **top sale transaction** for each seller in April 2024. If multiple transactions have the same sale amount, pick the most recent sale\_date.

### Code:

```sql
WITH cte AS (
    SELECT
        sale_id,
        seller_id,
        RANK() OVER (PARTITION BY seller_id ORDER BY sale_amount DESC, sale_date DESC) AS rnk
    FROM
        fct_seller_sales
    WHERE
        sale_date BETWEEN '2024-04-01' AND '2024-04-30'
)

SELECT sale_id, seller_id
FROM cte
WHERE rnk = 1;
```

### Explanation:

- **RANK() OVER (PARTITION BY seller\_id ORDER BY sale\_amount DESC, sale\_date DESC)** ranks each seller's sales by amount (highest first). Ties are broken by latest sale\_date.
- We select rows where rank = 1 to get the top sale per seller.

### Example:

| sale\_id | seller\_id | sale\_amount | sale\_date |
| -------- | ---------- | ------------ | ---------- |
| 101      | A          | 500          | 2024-04-25 |
| 102      | A          | 500          | 2024-04-20 |
| 103      | B          | 600          | 2024-04-10 |

Output:

| sale\_id | seller\_id |
| -------- | ---------- |
| 101      | A          |
| 103      | B          |

---

## Query 2: Weekly Summary of Sales & Fee Percentage (May 2024)

### Problem:

For each seller, provide a **weekly summary** in May 2024 that shows:

- Total number of sales transactions.
- Fee percentage of the **most recent sale** in that week.

### Code:

```sql
WITH sales_with_week AS (
    SELECT
        seller_id,
        sale_id,
        sale_date,
        sale_amount,
        fee_amount_percentage,
        EXTRACT(WEEK FROM sale_date) AS wk
    FROM
        fct_seller_sales
    WHERE
        sale_date BETWEEN TO_DATE('2024-05-01', 'YYYY-MM-DD') AND TO_DATE('2024-05-31', 'YYYY-MM-DD')
),
rnk AS (
    SELECT
        seller_id,
        sale_id,
        fee_amount_percentage,
        wk,
        sale_date,
        ROW_NUMBER() OVER (PARTITION BY seller_id, wk ORDER BY sale_date DESC) AS last_day
    FROM
        sales_with_week
),
cnt AS (
    SELECT
        seller_id,
        COUNT(sale_id) AS csd,
        wk
    FROM
        sales_with_week
    GROUP BY
        seller_id, wk
),
wk_sum AS (
    SELECT
        seller_id,
        fee_amount_percentage,
        wk
    FROM
        rnk
    WHERE
        last_day = 1
)
SELECT
    w.seller_id,
    c.csd AS total_sales_transactions,
    w.fee_amount_percentage AS latest_fee_percentage
FROM
    cnt c
JOIN
    wk_sum w ON w.seller_id = c.seller_id AND w.wk = c.wk;
```

### Explanation:

1. **sales\_with\_week**: Extract week numbers and prepare sales data.
2. **rnk**: Rank sales per seller per week to find the latest sale.
3. **cnt**: Count total number of transactions per seller per week.
4. **wk\_sum**: Filter to get the latest fee percentage per week per seller.
5. Final SELECT joins counts and fee percentages for the summary.

### Example:

| seller\_id | wk | total\_sales\_transactions | latest\_fee\_percentage |
| ---------- | -- | -------------------------- | ----------------------- |
| A          | 18 | 3                          | 10%                     |
| B          | 18 | 2                          | 8%                      |

---

## Query 3: Daily Cumulative Transaction Count (June 2024)

### Problem:

Create a daily report for June 2024 that shows a **cumulative count of transactions** up to that day for each seller.

### Code:

```sql
WITH cte AS (
    SELECT generate_series('2024-06-01'::date, '2024-06-30'::date, interval '1 day') AS sale_date
),
cte1 AS (
    SELECT
        d.seller_id,
        c.sale_date
    FROM
        dim_seller d,
        cte c
)
SELECT
    c1.seller_id,
    c1.sale_date,
    COUNT(f.sale_id) OVER (PARTITION BY c1.seller_id ORDER BY c1.sale_date) AS nt
FROM
    fct_seller_sales f
RIGHT JOIN
    cte1 c1 ON c1.seller_id = f.seller_id AND c1.sale_date = f.sale_date;
```

### Explanation:

1. **cte**: Generates all dates in June 2024.
2. **cte1**: Cross joins each seller with every date, ensuring each seller has a row per day.
3. **RIGHT JOIN** with sales to bring in actual sales per day.
4. **COUNT() OVER (PARTITION BY seller\_id ORDER BY sale\_date)**: Computes cumulative transaction count up to that date.

### Example:

| seller\_id | sale\_date | nt |
| ---------- | ---------- | -- |
| A          | 2024-06-01 | 1  |
| A          | 2024-06-02 | 1  |
| A          | 2024-06-03 | 2  |
| B          | 2024-06-01 | 0  |
| B          | 2024-06-02 | 1  |

---

## Summary of Key Concepts:

- **RANK() / ROW\_NUMBER()**: For ordering transactions to pick top/latest sales.
- **EXTRACT(WEEK FROM date)**: For weekly aggregations.
- **generate\_series()**: To create a calendar table ensuring all dates are present.
- **Cumulative Counts (COUNT() OVER)**: For running totals by date.

This workflow builds a foundation for correlating fee changes with seller activity trends, which is crucial for strategic fee structuring in a marketplace environment.

---

## Future Enhancements:

- Include cumulative sales amounts alongside counts.
- Track week-over-week growth rates per seller.
- Use LAG() to compare fee changes from previous sales.
- Automate dynamic date ranges instead of hardcoding months.

