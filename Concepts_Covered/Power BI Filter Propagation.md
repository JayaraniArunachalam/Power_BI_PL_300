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
- `Sales` table holds sales of an employee and their corresponding region
- `Salesperson` table holds the data of emp key and their names
- `Region` table holds data of the region (dim of region)
- One **Salesperson** can be assigned to multiple **Regions**
- One **Region** can have multiple **Salespersons**

This creates a **many-to-many relationship** between Salesperson and Region.

To model this relationship explicitly and clearly, a **bridge table (SalespersonRegion)** is used.

##  Relationship Design & Behavior

### Case 1: No Direct Relationship Between Salesperson and Sales

#### Relationships
- Region → Sales (1:* on Region)
- Salesperson → SalespersonRegion (1:* on EmpKey)
- Region ↔ SalespersonRegion (bi-directional)
- No direct relationship between Salesperson and Sales
![](https://github.com/JayaraniArunachalam/Power_BI_PL_300/blob/main/Diagrams/case%201%20filter%20propagation.jpg)

#### Visual Used
- Table visual: **Salesperson | Total Sales**
- ![](https://github.com/JayaraniArunachalam/Power_BI_PL_300/blob/main/Diagrams/case1%20filterpropagation%20visual.jpg)

#### Observed Result
- Each salesperson shows the **total sales of all regions assigned to them**
- Individual employee-level sales are not isolated

#### Explanation
Filters propagate through the following path:

Salesperson → SalespersonRegion → Region → Sales

As a result, aggregation happens at the **region level**, not the employee level.

---

### Case 2: Added a Direct Relationship

#### Additional Relationship
- Salesperson → Sales (1:* on EmpKey)
![](https://github.com/JayaraniArunachalam/Power_BI_PL_300/blob/main/Diagrams/case%202%20filter%20propagation.jpg)

#### Observed Result
- Each salesperson now shows **only their own sales**
![](https://github.com/JayaraniArunachalam/Power_BI_PL_300/blob/main/Diagrams/case2%20filterpropagation%20visual.jpg)

#### Explanation
Power BI prefers the **shortest and least ambiguous filter path**:


This enables filtering and aggregation at the **employee granularity**.

---

## Key Learnings

- Power BI always prefers the **most direct filter path**
- Bi-directional filters can significantly affect result granularity
- Many-to-many relationships must be modeled carefully
- Explicit bridge tables improve clarity and model reliability
- Relationship design is just as important as DAX

---

## Why This Matters

Understanding filter propagation is critical for:
- Accurate reporting
- Avoiding hidden aggregation bugs
- Building reliable Power BI models
- PL-300 certification preparation
- Real-world BI projects

---
