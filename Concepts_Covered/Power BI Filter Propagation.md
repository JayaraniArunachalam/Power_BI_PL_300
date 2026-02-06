# Power BI Filter Propagation

This repository captures a small but powerful Power BI data model I used to understand **filter propagation**deeply, 
along with **many-to-many scenarios involving bi-directional filters and bridge tables**.

The goal of this exercise is not dashboard design, but **data modeling clarity** — understanding *why* numbers change based on relationship design.

---

##  Problem Statement

In Power BI, filter propagation can behave unexpectedly when:
- Many-to-many relationships exist
- Bi-directional filters are used
- Direct and indirect filter paths coexist

This repository demonstrates how **relationship design directly impacts aggregation behavior** in visuals.

---

##  Data Model Overview

### Tables Used

#### 1. Sales
- EmpKey  
- Sales  
- Region  

#### 2. Salesperson
- EmpKey  
- Name  

#### 3. Region
- Region  

#### 4. SalespersonRegion (Bridge Table)
- EmpKey  
- Region  

---

##  Business Scenario
- 'Sales' table holds sales of an employee and their corresponding region
- One **Salesperson** can be assigned to multiple **Regions**
- One **Region** can have multiple **Salespersons**

This creates a **many-to-many relationship** between Salesperson and Region.

To model this relationship explicitly and clearly, a **bridge table (SalespersonRegion)** is used.

> While Power BI supports many-to-many relationships directly, an explicit bridge table makes filter propagation more predictable and easier to reason about.

---

## 🔄 Relationship Design & Behavior

### Case 1: No Direct Relationship Between Salesperson and Sales

#### Relationships
- Region → Sales (1:* on Region)
- Salesperson → SalespersonRegion (1:* on EmpKey)
- Region ↔ SalespersonRegion (bi-directional)
- ❌ No direct relationship between Salesperson and Sales

#### Visual Used
- Table visual: **Salesperson | Total Sales**

#### Observed Result
- Each salesperson shows the **total sales of all regions assigned to them**
- Individual employee-level sales are not isolated

#### Explanation
Filters propagate through the following path:

