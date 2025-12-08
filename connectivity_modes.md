# Power BI Storage & Connectivity Modes

## 1) Import Mode
- Uses **VertiPaq (in-memory analytics engine)** to compress & store data.
- Data is **cached**, so visuals load fast.
- Ideal for **small–medium models**, periodic refresh.
- **Supports:** Excel, CSV, SharePoint, SQL Server, APIs, flat files, cloud sources.

### Pros
- Fast performance
- Good compression (10x–100x)
- Full DAX capabilities

### Cons
- Memory usage increases
- No real-time data

---

## 2) DirectQuery Mode
- Data remains in the **source system**.
- Queries executed **live (near real-time)**.
- Supports **query folding**, where Power BI pushes transformations back to the data source.

### Data Sources
| Source | Supports DQ |
|--------|-------------|
| SQL Server | ✔ |
| Azure SQL | ✔ |
| Snowflake | ✔ |
| Databricks | ✔ |
| SAP HANA | ✔ |
| BigQuery | ✔ |

### Pros
- Near real-time data
- No heavy RAM usage

### Cons
- Slower visuals
- Limited DAX in large models
- Performance depends on source

---

## 3) Composite Model
- Mixture of **Import + DirectQuery**.
- Power BI **switches** storage mode depending on query behavior.
- Best for hybrid business cases.

---

## 4) Hybrid Model
- Stores **historical data in Import** + **frequent data in DirectQuery**.
- Typically used for enterprise warehouses & incremental refresh.

---

### 📌 Key Takeaway
| Mode | Speed | Data Freshness | Best Use |
|------|-------|----------------|----------|
| Import | 🔥 Fast | ❌ Scheduled | Small–Medium Stable Data |
| DirectQuery | 🕒 Depends on DB | ✔ Real-Time | Large Models, Frequent Updates |
| Composite | ⚖ Balanced | ⚖ Balanced | Large Mixed Models |
| Hybrid | ⚡ Enterprise Optimized | ⚡ Optimized | Real-Time + Historical Storage |

