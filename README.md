# Exploring Production Performance & Quality Optimization in Manufacturing Environment | Power BI

**Author:** Bùi Xuân Bảo Duy (Kelvin)  
**Date:** April 2025  
**Tools Used:** Power BI  

## 🗂️ Table of Contents
1️⃣ [Context](#context)  
2️⃣ [Dataset Description & Data Structure (DD & DS)](#dataset-description--data-structure-dd--ds)  
3️⃣ [Design Thinking Framework](#design-thinking-framework)  
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

<details>
<summary><b>Dim_date</b></summary>

| Column Name | Data Type | Description         |
|-------------|-----------|---------------------|
| Date        | DATETIME  | Full date value     |
| Year        | INT       | Year number         |
| Month       | TEXT      | Month with year     |
| Quarter     | TEXT      | Quarter with year   |
| Week        | INT       | Week number of year |
| Day         | INT       | Day number of month |

</details>

<details>
<summary><b>Product_Inventory</b></summary>

| Column Name  | Data Type        | Description              |
|--------------|------------------|--------------------------|
| ProductID    | INT              | Product identifier       |
| LocationID   | INT              | Location identifier      |
| Shelf        | INT              | Storage shelf            |
| Bin          | INT              | Storage bin              |
| Quantity     | INT              | Inventory quantity       |
| rowguid      | UNIQUEIDENTIFIER | Unique identifier (GUID) |
| ModifiedDate | DATETIME         | Lastest modified date    |
| SafetyStock  | INT              | Safety stock level       |
| Status       | TEXT             | Quantity Status          |
| ReorderPoint | INT              | Reorder Point            |

</details>

<details>
<summary><b>Production_ScrapReason</b></summary>

| Column Name   | Data Type | Description              |
|---------------|-----------|--------------------------|
| ScrapReasonID | INT       | Scrap reason identifier  |
| Name          | TEXT      | Scrap reason description |
| ModifiedDate  | DATETIME  | Lastest update date      |

</details>

<details>
<summary><b>Production_Location</b></summary>

| Column Name  | Data Type | Description                    |
|--------------|-----------|--------------------------------|
| LocationID   | INT       | Unique location identifier     |
| Name         | TEXT      | Location name                  |
| CostRate     | DECIMAL   | Cost rate per unit resource    |
| Availability | INT       | Location availability capacity |
| ModifiedDate | DATETIME  | Lastest update date            |

</details>

<details>
<summary><b>Production_WorkOrder</b></summary>

| Column Name   | Data Type | Description                                               |
|---------------|-----------|-----------------------------------------------------------|
| WorkOrderID   | INT       | Work order identifier                                     |
| ProductID     | INT       | Product identifier                                        |
| OrderQty      | INT       | Ordered quantity                                          |
| StockedQty    | INT       | Quantity stocked after production                         |
| ScrappedQty   | INT       | Scrapped quantity                                         |
| StartDate     | DATETIME  | Planned start date                                        |
| EndDate       | DATETIME  | Planned end date                                          |
| DueDate       | DATETIME  | Required completion date                                  |
| ScrapReasonID | INT       | Scrap reason identifier                                   |
| ModifiedDate  | DATETIME  | Latest update date                                        |
| Status        | TEXT      | Work order status (e.g., completed on time/late/in progress/open) |
| IsDelayed     | BIN       | 1 if delayed, else 0                                      |
| ScrapReason   | TEXT      | Scrap reason                                              |

</details>

<details>
<summary><b>Production_WorkOrderRouting</b></summary>

| Column Name           | Data Type | Description                    |
|-----------------------|-----------|--------------------------------|
| WorkOrderID           | INT       | Work order identifier          |
| ProductID             | INT       | Product identifier             |
| OperationSequence     | INT       | Operation order                |
| LocationID            | INT       | Production location identifier |
| ScheduledStartDate    | DATETIME  | Planned start time             |
| ScheduledEndDate      | DATETIME  | Planned end time               |
| ActualStartDate       | DATETIME  | Actual start time              |
| ActualEndDate         | DATETIME  | Actual end time                |
| ActualResourceHrs     | DECIMAL   | Actual resource hours consumed |
| PlannedCost           | DECIMAL   | Planned production cost        |
| ActualCost            | DECIMAL   | Actual production cost         |
| ModifiedDate          | DATETIME  | Latest update date             |
| LocationName          | TEXT      | Location name                  |
| ScheduledProducingDays| INT       | Planned production days        |
| ActualProducingDays   | INT       | Actual production days         |

</details>

<details>
<summary><b>Product_Product</b></summary>

| Column Name           | Data Type        | Description                              |
|-----------------------|------------------|------------------------------------------|
| ProductID             | INT              | Product identifier (primary key)         |
| Name                  | TEXT             | Product name                             |
| ProductNumber         | TEXT             | Unique product number/code               |
| MakeFlag              | BIT              | 1 = manufactured in-house, 0 = purchased externally |
| FinishedGoodsFlag     | BIT              | 1 = sold as finished product, 0 = component only |
| Color                 | TEXT             | Product color                            |
| SafetyStockLevel      | INT              | Minimum inventory to maintain            |
| ReorderPoint          | INT              | Inventory level that triggers reordering |
| StandardCost          | DECIMAL          | Standard manufacturing cost              |
| ListPrice             | DECIMAL          | Selling price                            |
| Size                  | TEXT             | Product size (e.g., S, M, L, XL)         |
| SizeUnitMeasureCode   | TEXT             | Unit of measure for size                 |
| WeightUnitMeasureCode | TEXT             | Unit of measure for weight               |
| Weight                | DECIMAL          | Product weight                           |
| DaysToManufacture     | INT              | Estimated days required to manufacture   |
| ProductLine           | TEXT             | Product line code                        |
| Class                 | TEXT             | Product class                            |
| Style                 | TEXT             | Product style                            |
| ProductSubcategoryID  | INT              | Linked product subcategory identifier    |
| ProductModelID        | INT              | Linked product model identifier          |
| SellStartDate         | DATETIME         | Date when product was first available    |
| SellEndDate           | DATETIME         | Date when product was no longer available|
| DiscontinuedDate      | DATETIME         | Date product was officially discontinued |
| rowguid               | UNIQUEIDENTIFIER | Globally unique identifier               |
| ModifiedDate          | DATETIME         | Last modification date                   |
| PriceLevel            | TEXT             | Custom field for price level/category    |

</details>


- **Data Relationship**  

![image](https://github.com/kelvinduybui/Exploring-Production-Performance-Quality-Optimization-in-Manufacturing-Environment-Power_BI/blob/main/Pictures/Data%20Model.png?raw=true)


## 3️⃣ Design Thinking Framework  

### 🤝 Empathize  
![Image](https://github.com/kelvinduybui/Exploring-Production-Performance-Quality-Optimization-in-Manufacturing-Environment-Power_BI/blob/main/Pictures/Stage%201%20p1.jpg?raw=true)
![Image](https://github.com/kelvinduybui/Exploring-Production-Performance-Quality-Optimization-in-Manufacturing-Environment-Power_BI/blob/main/Pictures/Stage%201%20p2.jpg?raw=true)

### 🎯 Define Northstar Metric & Point of View  
![Image](https://github.com/kelvinduybui/Exploring-Production-Performance-Quality-Optimization-in-Manufacturing-Environment-Power_BI/blob/main/Pictures/Stage%202%20-%20POV.jpg?raw=true)

### 💡 Ideate  
![Image](https://github.com/kelvinduybui/Exploring-Production-Performance-Quality-Optimization-in-Manufacturing-Environment-Power_BI/blob/main/Pictures/Stage%203%20-%20Ideate.jpg?raw=true)

### 🛠️ Prototype & Review  
Conducted through cross-departmental discussions  

## 4️⃣ Visualization  
### Page 1: Overview Dashboard  
![Image](https://github.com/kelvinduybui/Exploring-Production-Performance-Quality-Optimization-in-Manufacturing-Environment-Power_BI/blob/main/Pictures/overview.png?raw=true)  

#### Key Takeaways:  
From this dashboard, the following key aspects can be **viewed** and **monitored** :

- **Production Volume**: Track **total work orders** and **total products** to understand overall **production scale**.  
- **Inventory Status**: Monitor **stockouts**, **overstock**, and **reorder needs**.  
- **Product Quality & Issues**: See **product status** (*normal vs. issue*) and track **scrap rates**.  
- **Operational Efficiency by Process**: Follow **average actual cost** and **hours** by **production stage**.  
- **Trends & Alerts**: Observe **delay trends** and flag **alerts** over time.  
- **Key Products**: Identify **top produced products** and their **scrap rates**.  

### Page 2: Production Dashboard  
![Image](https://github.com/kelvinduybui/Exploring-Production-Performance-Quality-Optimization-in-Manufacturing-Environment-Power_BI/blob/main/Pictures/production.png?raw=true)

#### Key Takeaways:  
From this production dashboard, the following aspects can be **viewed** and **monitored**:

- **Production Efficiency**: **Average manufacturing time**, **delay time**, and **on-time performance** for both **quantity** and **work orders**.  
- **Work Order Status**: Breakdown of **completed orders** (*on time vs. late*).  
- **Planned vs. Actual Performance**: Comparison of **scheduled producing days** vs. **actual producing days** over time.  
- **Operational Insights**: Trends in **lead time**, **delays**, and **completion performance**.  

### Page 3: Quality Control Dashboard  
![Image](https://github.com/kelvinduybui/Exploring-Production-Performance-Quality-Optimization-in-Manufacturing-Environment-Power_BI/blob/main/Pictures/qc.png?raw=true)

#### Key Takeaways:  
From this quality control dashboard, the following aspects can be **viewed** and **monitored**:

- **Scrap Performance**: **Total scrap orders**, **scrap quantity**, **scrap rate**, and **scrap cost**.  
- **Scrap by Location**: Distribution of **scrap** across different **production stages**.  
- **Scrap Trends**: **Scrap rate** and **cost changes** over time.  
- **Root Causes**: **Scrap reasons** and their corresponding **rates** and **costs**.  
- **Product Impact**: **Scrap rate** and **cost** linked to specific **products**.  


## 5️⃣ Insights & Recommendations
### 🔎 Overview Insights  

- **Production Volume**: A total of **72.59K work orders** were processed, covering **295 products** — reflecting a **broad production portfolio**.  
- **Inventory Health**: There are **4 stockouts** (*low but critical*), **50 overstock cases**, and **848 items needing reorder** — indicating **imbalance in stock management**.  
- **Product Status**: The majority of products (**95.23%**) fall under **reorder needed**, **overstock**, or **stockout** status, while only a small portion is **normal** — showing **serious inventory flow imbalance** rather than **quality issues**.  
- **Cost & Time by Location**:  
  - **Frame Welding** → **Low actual hours (3.5)** but **very high cost (88)** → **cost inefficiency**.  
  - **Final Assembly** → **High actual hours (7.8)** but **relatively low cost (35)** → **time inefficiency**.  
- **Top Products**:  
  - **BB Ball Bearing** → **Largest production volume (0.91M units)** with a **very low scrap rate (0.11%)** → **efficient and reliable**.  
  - **Fork End** → **Smaller output (0.23M units)** but a **much higher scrap rate (0.58%)** → **quality concern** despite lower scale.  
- **Delays**: The **percent delayed** has been **rising steadily** from ~**28% (2011)** to nearly **33% (2014)** → showing a **growing risk in delivery reliability**.

### 💡 Overview Recommendations  

- **Inventory Management**:  
  - Use **demand forecasting** and **buffer stock rules** to reduce both **reorder needs (848)** and **overstock (50)**.  
  - Apply **ABC/XYZ analysis** to better align stock with **demand variability**.

- **Production Efficiency**:
  - **Standardize best practices** across locations to **minimize variation** in costs and hours.
  - **Investigate** high-cost or time-consuming processes for **automation** or **process improvement**.

- **Delay Mitigation**:  
  - Strengthen **production scheduling** and apply **early-warning alerts** for **at-risk orders**.  
  - Explore **load balancing** across locations to relieve **bottlenecks**.  

- **Product Focus**:  
  - Maintain **high standards** for **volume leaders** like **BB Ball Bearing**.  
  - Improve **quality control** on **Fork End** and other products with **disproportionately high scrap rates**.  

### 🔎 Production Insights  
- **Manufacturing & Delay Times**:  
  - **Average manufacturing time**: **13.24 days**.  
  - **Average delay time**: **8.44 days**.  
  - → **Production cycles** are **relatively long** with **notable delays**.  

- **On-Time Performance**:  
  - **Qty On Time %**: **78.56%**.  
  - **WO On Time %**: **68.60%**.  
  - → While most **quantities** are delivered on time, the percentage of **work orders** completed on time is **relatively low**.  

- **Work Order Status**:  
  - **68.6% completed on time**, **31.4% late** (≈**22.8K late work orders**).  

- **Scheduled vs. Actual Producing Days**:  
  - **Actual producing days** consistently **exceed scheduled days** → frequent **overruns**.  

- **By Location**:  
  - **Scheduled average**: ~**11 days**.  
  - **Actual average**: **12–12.7 days**.  
  - **Debur and Polish (12.70)** and **Specialized Paint (12.76)** show the **largest overruns** → main **bottlenecks** are in **assembly stages**.  

### 💡 Production Recommendations  

- **Efficiency Improvement**:  
  - Focus on improving **Debur and Polish** & **Specialized Paint** through **automation**, **better resource allocation**, and **workforce training**.  
  - Adjust **time standards** or add **buffer capacity** to reduce **gaps** between **scheduled** and **actual**.  

- **On-Time Delivery**:  
  - Implement **detailed milestone tracking** for each **work order**.  
  - Set up **early-warning alerts** for **at-risk orders**.  

- **Scheduling Optimization**:  
  - Align **production schedules** closer to the **actual average (~12.3 days instead of 11)** to make **planning** more **realistic** and reduce **lateness**.  

### 🔎 Quality Control Insights  

- **Overall Scrap Performance**:  
  - **Scrap rate**: **24%** → **relatively high**.  
  - **Total scrap cost**: ~**$359.95K**.  
  - **729 Work Orders** affected, generating **11K+ scrap products**.  

- **By Year**:  
  - **Scrap rate** rose from ~**12% (2011)** → ~**29% (2013)**, then dropped to ~**20% (2014)**.  
  - **Scrap cost** followed the same trend → **peaked in 2013**, then **declined in 2014**.  
  - → Indicates **improvement after 2013**, but **performance remains unstable**.  

- **By Location**:  
  - **Highest scrap** in **Subassembly (21%)**, followed by **Frame Welding (18%)** and **Frame Forming (18%)**.  
  - Most **scrap originates** from **early-stage production processes** → driving both **quantity** and **cost**.  

- **By Scrap Reason**:  
  - **Top reasons**: **Seat assembly not as ordered (3.1%)**, **Drill size too large (2.9%)**, **Paint process failed (2.9%)**, **Trim length too long (2.9%)**.  
  - **Costs don’t fully align with rates**: e.g., **Trim length too long** → **high scrap rate** but **cost only ~$18K** → tied to **lower-value parts**.  
  - → Issues linked to **machine setup** and **quality inspection processes**.  

- **By Product**:  
  - **Scrap rates** across products are **relatively high**, often **>50%**.  
  - **Critical cases**: **ML Road Frame – Red (75%)**, **Mountain-100 Black (63%)**.  
  - **Scrap cost varies**: **expensive frames** drive **high cost**, while **high-rate low-value items** generate **less cost**.  
  - → Suggests **quality issues are widespread**, but **financial impact** is concentrated in **high-value SKUs**.  

### 💡 Quality Control Recommendations  

- **Target High-Scrap Products**:  
  - Prioritize **process review** for **top 3 products** with the **highest scrap rates** and **costs**.  
  - Apply **root cause analysis** (*Fishbone, 5 Whys*) to identify **design**, **material**, or **process issues**.  

- **Improve Subassembly & Welding**:  
  - Enhance **machine calibration** and **operator training** in **high-scrap stages**.  
  - Implement **early-stage quality checkpoints** to prevent **downstream defects** and **extra cost**.  

- **Cost-Focused Monitoring**:  
  - Track both **scrap rate** and **scrap cost** in **KPIs**.  
  - Focus on processes with **high cost + high scrap rate**.  
