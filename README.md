# Manufacturing-Performance-Operational-Optimization-at-AdventureWorks-Power-BI  

**Author:** Bùi Xuân Bảo Duy (Kelvin)  
**Date:** April 2025  
**Tools Used:** Power BI  

## 🗂️ Table of Contents
1️⃣ [Context](#context)  
2️⃣ [Dataset Description & Data Structure (DD & DS)](#dataset-description--data-structure-dd--ds)  
3️⃣ [Design Thinking Framework]  
4️⃣ [Visualization](#visualization)  
5️⃣ [Insights & Recommendations](#insights--recommendations)  

---

## 1️⃣ Context

📘 **Main Context**  
- **Production requests** come from Planning → after **Procurement** secures raw materials, production starts.  
- **Finished goods** stored across warehouses; completion time may not always match plan.  
- **Quality checks** at warehouse entry remove defective products.  
- Task: Build a **Power BI Operations Dashboard** for the **Production Director** to get clear insights, make better decisions, and improve operations.  

👥 **Target Audience**  
- **Primary Stakeholder**: Head of Production
- **Secondary Stakeholders**: Warehouse Department, Quality Control Department, Planning Department

🎯 **Objective**   
Provide the stakeholders with a clear and interactive dashboard to monitor manufacturing performance, track inventory and quality, and support data-driven operational decisions.  

🛠️ **Tools Used**  
- **Power BI**: For data integration, production & inventory monitoring, quality control tracking, and interactive operational dashboards  

---

## 2️⃣ Dataset Description & Data Structure (DD & DS)

### 📊 Data Description  
- **Size:** Up to 72,839 rows & 25 columns  
- **Format:** .pbix  
- **Time range:** 2011 - 2014    

### 🏷️ **Data Structure**  
#### Table Used:  
- 🏷️ **Product_Inventory** – Inventory details  

| Column Name  | Data Type        | Description                  |
|--------------|------------------|------------------------------|
| ProductID    | INT              | Product identifier           |
| LocationID   | INT              | Location identifier          |
| Shelf        | INT              | Storage shelf                |
| Bin          | INT              | Storage bin                  |
| Quantity     | INT              | Inventory quantity           |
| rowguid      | TEXT             | Unique identifier (GUID)     |
| ModifiedDate | DATETIME         | Lastest modified date        |
| SafetyStock  | INT              | Safety stock level           |
| Status       | TEXT             | Quantity Status              |
| ReorderPoint | INT              | Reorder Point                |

- 🧾 **Production_ScrapReason** – Lookup for scrap reasons  

| Column Name    | Data Type    | Description               |
|----------------|--------------|---------------------------|
| ScrapReasonID  | INT          | Scrap reason identifier   |
| Name           | TEXT         | Scrap reason description  |
| ModifiedDate   | DATETIME     | Lastest update date       |

- 📄 **Production_Location** – Production site information

| Column Name   | Data Type | Description                    |
|---------------|-----------|--------------------------------|
| LocationID    | INT       | Unique location identifier     |
| Name          | TEXT      | Location name                  |
| CostRate      | DECIMAL   | Cost rate per unit resource    |
| Availability  | INT       | Location availability capacity |
| ModifiedDate  | DATETIME  | Lastest update date            |

# 🗂️ Production_WorkOrder – Core fact table containing production order details

| Column Name     | Data Type | Description                       |
|-----------------|-----------|-----------------------------------|
| WorkOrderID     | INT       | Work order identifier             |
| ProductID       | INT       | Product identifier                |
| OrderQty        | INT       | Ordered quantity                  |
| StockedQty      | INT       | Quantity stocked after production |
| ScrappedQty     | INT       | Scrapped quantity                 |
| StartDate       | DATETIME  | Planned start date                |
| EndDate         | DATETIME  | Planned end date                  |
| DueDate         | DATETIME  | Required completion date          |
| ScrapReasonID   | INT       | Scrap reason identifier           |
| ModifiedDate    | DATETIME  | Latest update date                |
| Status          | TEXT  | Latest update date                |
| IsDelayed    | BIN  | Latest update date                |
| Scrap Reason    | DATETIME  | Latest update date                |


