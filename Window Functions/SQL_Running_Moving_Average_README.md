
# 📘 SQL Window Functions - Running & Moving Averages

This guide explains **running average** and **moving average**, including real-world use cases, syntax breakdown, and SQL examples with expected outputs.

---

## 🧠 What is a Running Average?

A **running average** (aka cumulative average) is the average of all previous rows up to the current row.

### 🔍 Syntax

```sql
AVG(column) OVER (
  ORDER BY <column>
  ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
)
```

- `UNBOUNDED PRECEDING` → start from the first row
- `CURRENT ROW` → up to the current row

---

## 🧠 What is a Moving Average?

A **moving average** (aka rolling average) calculates the average of a defined **range** of rows around the current row.

### 🔍 Syntax

```sql
AVG(column) OVER (
  ORDER BY <column>
  ROWS BETWEEN N PRECEDING AND N FOLLOWING
)
```

- E.g., 1 PRECEDING to 1 FOLLOWING → 3-row average (previous, current, next)

---

## 📄 Sample Table: Sales Data

| day | sales |
|-----|-------|
| 1   | 100   |
| 2   | 150   |
| 3   | 200   |
| 4   | 180   |
| 5   | 130   |

---

## 📊 Running Average

**Use Case:** Track how average sales evolve cumulatively over days.

```sql
SELECT day, sales,
       AVG(sales) OVER (ORDER BY day 
                        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_avg
FROM sales;
```

**Expected Output:**

| day | sales | running_avg |
|-----|-------|-------------|
| 1   | 100   | 100.0       |
| 2   | 150   | 125.0       |
| 3   | 200   | 150.0       |
| 4   | 180   | 157.5       |
| 5   | 130   | 152.0       |

🟢 Running average is often used in dashboards to analyze growth/progress over time.

---

## 📈 Moving Average (3-day window)

**Use Case:** Smooth fluctuations in daily sales using a rolling 3-day window.

```sql
SELECT day, sales,
       AVG(sales) OVER (ORDER BY day 
                        ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS moving_avg
FROM sales;
```

**Expected Output:**

| day | sales | moving_avg |
|-----|-------|------------|
| 1   | 100   | 125.0      |
| 2   | 150   | 150.0      |
| 3   | 200   | 176.7      |
| 4   | 180   | 170.0      |
| 5   | 130   | 155.0      |

🟢 Moving averages are used in:
- Stock price smoothing
- Sales trends
- Weather trend analysis
- Sensor data smoothing

---

## 📦 Real-world Use Cases

| Industry | Use Case |
|----------|----------|
| Finance | Moving average of stock prices for trend detection |
| Retail  | Running average of weekly sales |
| Marketing | Average daily website traffic trends |
| IoT | Sensor reading smoothing |

---

## 📝 Summary

| Type           | Frame Syntax                                       | Use Case                         |
|----------------|----------------------------------------------------|----------------------------------|
| Running Avg    | UNBOUNDED PRECEDING TO CURRENT ROW                 | Cumulative metrics               |
| Moving Avg     | N PRECEDING TO N FOLLOWING                         | Trend smoothing                  |

---

✅ Use `ORDER BY` carefully to define time or sequence  
✅ Use ROWS clause to define window behavior

