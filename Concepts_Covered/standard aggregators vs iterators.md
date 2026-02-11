# Normal Aggregation vs Iterator Functions in DAX  
### Understanding SUM vs SUMX with a Practical Example

This document explains the fundamental difference between **normal aggregation functions** and **iterator (X) functions** in DAX, using `SUM` and `SUMX` as examples.

Understanding this distinction is essential for writing accurate and efficient Power BI measures.

---

## 📌 Why This Matters

In DAX, aggregation can happen in two ways:

1. **Normal Aggregation Functions** (e.g., SUM, AVERAGE)
2. **Iterator Functions** (e.g., SUMX, AVERAGEX)

The difference becomes critical when calculations involve **row-level business logic**, such as percentage discounts or multi-column arithmetic.

---

## 🔹 Example Scenario

### Sales Table

| Price | Discount % |
|-------|------------|
| 150   | 15%        |
| 180   | 18%        |

### Goal  
Calculate **Total Sales after applying discount per row**.

---

## 🔹 Approach 1 – Using Normal Aggregation (SUM)

### Step 1: Create a Calculated Column

``` NetPrice = Sales[Price] * (1 - Sales[Discount %]) ```

### Step 2: Aggregate the column
``` TotalSales = SUM(Sales[NetPrice]) ```

### Explanation
- Row-level calculation happens in the calculated column.
- SUM aggregates an existing column.
- No row-by-row evaluation occurs inside the measure.
- Increases model size due to additional stored column.

---

## 🔹 Approach 2 – Using Iterator Function (SUMX)
``` TotalSales =
SUMX(
    Sales,
    Sales[Price] * (1 - Sales[Discount %])
)
```

### Explanation
- SUMX iterates over the Sales table.
- Evaluates the expression for each row.
- Aggregates the evaluated results.
- No calculated column required.
- Fully dynamic and filter-responsive.

### Key Differences

| Normal Aggregation                    | Iterator Function                  |
| ------------------------------------- | ---------------------------------- |
| Aggregates a column                   | Iterates over a table              |
| No row context in measure             | Creates row context                |
| Faster and optimized                  | Slightly heavier on large datasets |
| Needs calculated column for row logic | Handles row-level logic directly   |
![](https://github.com/JayaraniArunachalam/Power_BI_PL_300/blob/main/Diagrams/normal%20vs%20iterator%20function%20in%20pbi.jpg)

### When to Use What?
#### Use Normal Functions When:
- Aggregating a single existing column
- No row-level expression is required
- Maximum performance is needed

#### Use Iterator Functions When:
- Calculation depends on multiple columns
- Row-by-row logic is required
- Iterating over a virtual table (FILTER, VALUES, SUMMARIZE)
- You want to avoid creating additional calculated columns

### Core Insight
- Normal functions aggregate columns.
- Iterator functions aggregate calculated row-level results.

Understanding this difference is fundamental to writing correct and scalable DAX.
