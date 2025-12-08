# Power BI Storage & Connectivity Modes

Power BI offers multiple storage and semantic model configurations that impact performance, refresh strategy, and cost for enterprise BI workloads.

---

## 1) Import Mode
- Most common mode; imports data into Power BI.
- Stored on disk and loaded into memory when queried.
- Powered by the **VertiPaq (in-memory columnar storage engine)**.
- High compression (up to 10x), very fast query performance.
- Full support for **Power Query (M)**, **DAX**, **Calculated tables + columns**, **Q&A**, and **Quick Insights**.

### Advantages
- Fast query performance.
- Rich modeling flexibility.
- Best for low-medium data volumes.

### Limitations
- Requires memory to load full dataset.
- Data only as fresh as last refresh.
- Full refresh can be costly (solved with Incremental Refresh).

---

## 2) DirectQuery Mode
- Data stays in the **source system**; only metadata is stored in Power BI.
- Power BI sends **native queries** to the source at runtime.
- Useful for **very large datasets** and **real-time dashboards**.

### Data Sources
| Source | Supports DQ |
|--------|-------------|
| SQL Server | ✔ |
| Azure SQL | ✔ |
| Snowflake | ✔ |
| Databricks | ✔ |
| SAP HANA | ✔ |
| BigQuery | ✔ |

### Advantages
- No model size limits.
- No scheduled refresh required.
- Near real-time support (with auto page refresh).

### Limitations
- Restricted M transformations and DAX capabilities.
- No calculated tables.
- Performance depends on the data source + query folding.

---

## 3) Composite Models
- Mix **Import + DirectQuery** in the same model.
- Table-level storage modes: **Import**, **DirectQuery**, and **Dual**.
- Dual mode allows Power BI to **choose either Import or DirectQuery based on query context**.

### Typical Configuration
| Table Type | Suggested Storage |
|------------|------------------|
| Dimensions | Import or Dual |
| Facts | DirectQuery |

---

## 4) Hybrid Tables (Premium)
- A specialized fact table using:
  - **Import partitions** (historical data)
  - **DirectQuery partition** (latest real-time data)
- Created via **Incremental Refresh + Real-time (Premium)**.

### When to Use
- High-volume fact tables with frequent data changes.
- Enterprise pipelines requiring real-time + historical reporting.

---

### Summary Comparison

| Mode | Performance | Data Freshness | Flexibility | Best Use |
|------|-------------|----------------|-------------|----------|
| Import | ⭐⭐⭐⭐ | Scheduled | ⭐⭐⭐⭐ | Small/Medium Models |
| DirectQuery | ⭐⭐ | Real-Time | ⭐ | Very Large Data |
| Composite | ⭐⭐⭐ | Mixed | ⭐⭐⭐ | Enterprise Mix |
| Hybrid | ⭐⭐⭐⭐ | Real-Time + Historical | ⭐⭐⭐ | Premium Enterprise |

---

### Key Takeaway
Semantic model selection is a strategic decision impacting:
✔ Performance  
✔ Cost & Memory  
✔ Real-time needs  
✔ BI Scalability

