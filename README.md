# Manufacturing-Performance-Operational-Optimization-at-AdventureWorks-Power-BI  

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

- 🔗 **Dim_date** – 

| Column Name | Data Type | Description            |
|-------------|-----------|------------------------|
| Date        | DATETIME  | Full date value        |
| Year        | INT       | Year number            |
| Month       | TEXT      | Month with year        |
| Quarter     | TEXT      | Quarter with year      |
| Week        | INT       | Week number of year    |
| Day         | INT       | Day number of month    |

- 🏷️ **Product_Inventory**

| Column Name  | Data Type        | Description                  |
|--------------|------------------|------------------------------|
| ProductID    | INT              | Product identifier           |
| LocationID   | INT              | Location identifier          |
| Shelf        | INT              | Storage shelf                |
| Bin          | INT              | Storage bin                  |
| Quantity     | INT              | Inventory quantity           |
| rowguid      | UNIQUEIDENTIFIER | Unique identifier (GUID)     |
| ModifiedDate | DATETIME         | Lastest modified date        |
| SafetyStock  | INT              | Safety stock level           |
| Status       | TEXT             | Quantity Status              |
| ReorderPoint | INT              | Reorder Point                |

- 🧾 **Production_ScrapReason**

| Column Name    | Data Type    | Description               |
|----------------|--------------|---------------------------|
| ScrapReasonID  | INT          | Scrap reason identifier   |
| Name           | TEXT         | Scrap reason description  |
| ModifiedDate   | DATETIME     | Lastest update date       |

- 📄 **Production_Location**

| Column Name   | Data Type | Description                    |
|---------------|-----------|--------------------------------|
| LocationID    | INT       | Unique location identifier     |
| Name          | TEXT      | Location name                  |
| CostRate      | DECIMAL   | Cost rate per unit resource    |
| Availability  | INT       | Location availability capacity |
| ModifiedDate  | DATETIME  | Lastest update date            |

- 🗂️ **Production_WorkOrder**

| Column Name     | Data Type | Description                                               |
|-----------------|-----------|-----------------------------------------------------------|
| WorkOrderID     | INT       | Work order identifier                                     |
| ProductID       | INT       | Product identifier                                        |
| OrderQty        | INT       | Ordered quantity                                          |
| StockedQty      | INT       | Quantity stocked after production                         |
| ScrappedQty     | INT       | Scrapped quantity                                         |
| StartDate       | DATETIME  | Planned start date                                        |
| EndDate         | DATETIME  | Planned end date                                          |
| DueDate         | DATETIME  | Required completion date                                  |
| ScrapReasonID   | INT       | Scrap reason identifier                                   |
| ModifiedDate    | DATETIME  | Latest update date                                        |
| Status          | TEXT      | Work order status (e.g., completed on time/late/in progress/open) |
| IsDelayed       | BIN       | 1 if delayed, else 0                                      |
| ScrapReason     | TEXT      | Scrap reason                                              |

- 🔗 **Production_WorkOrderRouting**

| Column Name          | Data Type | Description                          |
|----------------------|-----------|--------------------------------------|
| WorkOrderID          | INT       | Work order identifier                |
| ProductID            | INT       | Product identifier                   |
| OperationSequence    | INT       | Operation order                      |
| LocationID           | INT       | Production location identifier       |
| ScheduledStartDate   | DATETIME  | Planned start time                   |
| ScheduledEndDate     | DATETIME  | Planned end time                     |
| ActualStartDate      | DATETIME  | Actual start time                    |
| ActualEndDate        | DATETIME  | Actual end time                      |
| ActualResourceHrs    | DECIMAL   | Actual resource hours consumed       |
| PlannedCost          | DECIMAL   | Planned production cost              |
| ActualCost           | DECIMAL   | Actual production cost               |
| ModifiedDate         | DATETIME  | Latest update date                   |
| LocationName         | TEXT      | Location name                        |
| ScheduledProducingDays | INT     | Planned production days              |
| ActualProducingDays  | INT       | Actual production days               |

- **Product_Product**  
| Column Name           | Data Type        | Description                                                                 |  
|-----------------------|------------------|-----------------------------------------------------------------------------|  
| ProductID             | INT              | Product identifier (primary key)                                            |  
| Name                  | TEXT             | Product name                                                                |  
| ProductNumber         | TEXT             | Unique product number/code                                                  |  
| MakeFlag              | BIT              | 1 = manufactured in-house, 0 = purchased externally                         |  
| FinishedGoodsFlag     | BIT              | 1 = sold as finished product, 0 = component only                            |  
| Color                 | TEXT             | Product color                                                               |  
| SafetyStockLevel      | INT              | Minimum inventory to maintain                                               |  
| ReorderPoint          | INT              | Inventory level that triggers reordering                                    |  
| StandardCost          | DECIMAL          | Standard manufacturing cost                                                 |  
| ListPrice             | DECIMAL          | Selling price                                                               |  
| Size                  | TEXT             | Product size (e.g., S, M, L, XL)                                            |  
| SizeUnitMeasureCode   | TEXT             | Unit of measure for size (e.g., CM, IN)                                     |  
| WeightUnitMeasureCode | TEXT             | Unit of measure for weight (e.g., LB, KG)                                   |  
| Weight                | DECIMAL          | Product weight                                                              |  
| DaysToManufacture     | INT              | Estimated days required to manufacture                                      |  
| ProductLine           | TEXT             | Product line code (R = Road, M = Mountain, T = Touring, S = Standard)       |  
| Class                 | TEXT             | Product class (H = High, M = Medium, L = Low)                               |  
| Style                 | TEXT             | Product style (W = Women’s, M = Men’s, U = Universal)                       |  
| ProductSubcategoryID  | INT              | Linked product subcategory identifier (foreign key)                         |  
| ProductModelID        | INT              | Linked product model identifier (foreign key)                               |  
| SellStartDate         | DATETIME         | Date when product was first available for sale                              |  
| SellEndDate           | DATETIME         | Date when product was no longer available for sale                          |  
| DiscontinuedDate      | DATETIME         | Date product was officially discontinued                                    |  
| rowguid               | UNIQUEIDENTIFIER | Globally unique identifier for replication/support                          |  
| ModifiedDate          | DATETIME         | Last modification date                                                      |  
| PriceLevel            | TEXT             | Custom field for price level/category (not standard in AdventureWorks)      |  


## 3️⃣ Design Thinking Framework  

### 🤝 Empathize  
![Image](https://github.com/kelvinduybui/Manufacturing-Performance-Operational-Optimization-at-AdventureWorks-Power-BI/blob/main/Pictures/Empathy%201_Manu.jpg?raw=true)
![Image](https://github.com/kelvinduybui/Manufacturing-Performance-Operational-Optimization-at-AdventureWorks-Power-BI/blob/main/Pictures/Empathy%202_Manu.jpg?raw=true)

### 🎯 Define Northstar Metric & Point of View  
![Image](https://github.com/kelvinduybui/Manufacturing-Performance-Operational-Optimization-at-AdventureWorks-Power-BI/blob/main/Pictures/POV_manu.jpg?raw=true)

### 💡 Ideate  
![Image](https://github.com/kelvinduybui/Manufacturing-Performance-Operational-Optimization-at-AdventureWorks-Power-BI/blob/main/Pictures/Ideate_manu.jpg?raw=true)

### 🛠️ Prototype & Review  
Conducted through cross-departmental discussions  

## 4️⃣ Visualization  
### Page 1: Production Dashboard  
![Image](https://github.com/kelvinduybui/Manufacturing-Performance-Operational-Optimization-at-AdventureWorks-Power-BI/blob/main/Pictures/Page%201.png?raw=true)  

#### Key Takeaways:  
- **Production Overview**: Number of **Work Orders**, number of **products**, and **Work Order status** (completed on time, completed late).
- **Progress and Time**: **Average manufacturing time**, **average delay time**, and **actual production days** compared to scheduled days by year.
- **Quality and Efficiency**: **Actual cost** and **actual hours** at each production stage/location.
- **Product Analysis**: **Production volume by product type**, helping identify which products are produced the most.
- **Delay Monitoring & Alerts**: Track **delay rates** and **alert flags** across different time levels (year, quarter, month, week, day), providing insights into **delivery trends** and fluctuations.
- **Detail information** Ability to view detailed **status of each Work Order ID** and **Product ID**.
- **Detailed Filters**: Ability to filter by **year, Work Order ID, product, location, and reason** to drill down into specific aspects.


### Page 2: Inventory Dashboard  
![Image](https://github.com/kelvinduybui/Manufacturing-Performance-Operational-Optimization-at-AdventureWorks-Power-BI/blob/main/Pictures/Page%202.png?raw=true)

#### Key Takeaways:  
- **Inventory Overview**: Monitor **total stock**, number of **overstock cases**, number of **stockouts**, and products that need **reorder**.
- **Location Analysis**: Inventory distributed across different **production sites/warehouses**, highlighting **disparities between locations**.
- **Category Analysis**: Total stock divided by **product categories** (components, bikes, accessories, clothing), with the ability to drill down into **subcategories** and **product names**, helping identify which category or product **holds the most inventory**.
- **Product-Level Tracking**: List of **products** with **stock quantity, storage location, safety stock level**, and **status** (e.g., needs reorder, low stock, normal), along with **PriceLevel** (Low, Normal, High).
- **Proactive Alerts**: Identify **products requiring reorder** to avoid **production disruption** or **shortages**.
- **Detailed Filters**: Filter by **year, Work Order ID, product, location, or reason** to analyze inventory conditions in more detail.


### Page 3: Quality Control Dashboard  
![Image](https://github.com/kelvinduybui/Manufacturing-Performance-Operational-Optimization-at-AdventureWorks-Power-BI/blob/main/Pictures/Page%203.png?raw=true)

#### Key Takeaways:  
- **Scrap Overview**: Track the number of **Work Orders with scrap**, **total scrapped products**, **scrap rate (%)**, and **total scrap cost**.
- **Location Analysis**: Scrap rate by **production stage/location**, helping identify areas with **higher defect rates**.
- **Time Analysis**: Scrap rate (%) by **year**, allowing observation of **trends and fluctuations** over different periods.
- **Root Cause Analysis**: Scrap rate by **reason** (e.g., drill size too large, paint process failed, assembly errors), supporting the identification of **root causes**.
- **Product Analysis**: Scrap rate by **product type**, highlighting which products are **more prone to defects**.
- **Detailed Scrap Records**: A detailed list of **scrap cases** with **reasons and quantities**, enabling easier **tracking and control** of specific issues.
- **Detailed Filters**: Filter by **year, Work Order ID, product, location, or reason** for deeper analysis.

## 5️⃣ Insights & Recommendations
### Production insights  
- **Work Orders & Status**:  
  - A total of **72.59K Work Orders** were recorded.  
  - **31.4% (22.79K)** were delayed → a relatively high percentage, significantly impacting **production efficiency**.  

- **Production & Delay Time**:  
  - **Average manufacturing time**: 13.24 days.  
  - **Average delay time**: 8.44 days (≈ 64% of total production time) → delays are **severe**.  

- **Products**:  
  - **BB Ball Bearing** is the leading product with **0.91M units**, nearly double the second-highest (**Seat Stays**).  
  - Other key products include **Seat Stays, Blade, Fork End, and Chain Stays**.  

- **Cost & Labor Hours by Location**:  
  - **Frame** and **Final Assembly** incur the **highest costs and labor hours**.  
  - **Paint** and **Deburr** require the least.  
  - Bottlenecks are concentrated in **Frame** and **Final Assembly** stages.  

- **Delay Trend by Year**:  
  - Delay rates increased from **~28% in 2011** to **~34% in 2014**.  
  - Late deliveries are **worsening over time** and not yet effectively resolved.  

- **Scheduled vs Actual Producing Days**:  
  - **Actual production days** consistently exceed **scheduled days**, highlighting **inaccurate planning**.   

### Production recommendation  
- **Improve Planning & Scheduling**  
  - Enhance **forecasting accuracy** for production time, especially in **Frame** and **Final Assembly**.  
  - Consider applying **Production Scheduling Optimization** or **Simulation models** to minimize planning gaps.  

- **Reduce Delay Rate**  
  - Investigate root causes of delays (e.g., **material shortages, machine downtime, labor shortages**).  
  - Prioritize **process improvements** for high-volume products such as **BB Ball Bearing**.  

- **Optimize Costs by Location**  
  - Since **Frame** and **Final Assembly** are cost/time-intensive, evaluate **automation** or **line balancing** to reduce bottlenecks.  

- **Develop Long-term KPIs**  
  - Target reducing the **delay rate** from **31.4% → below 20%** within 1–2 years.  
  - Target reducing the **gap between Scheduled vs Actual Days** to **<10%**.

### Inventory insights  
- **Overall Inventory**:  
  - **Total Inventory**: 335.97K units.  
  - **50 Overstock cases** and **4 Stockouts**, with **848 Reorders** needed.  

- **Inventory by Location**:  
  - Inventory is heavily concentrated in **Subassembly** and **Miscellaneous Storage**, holding the **highest stock levels**.  
  - Downstream locations like **Paint Shop & Storage** hold significantly less inventory → **imbalance across the production chain**.  

- **Inventory by Category**:  
  - **Components** dominate inventory with **40K+ units**, much higher than other categories.  
  - **Bikes** and **Accessories** follow, while **Clothing** contributes the least.  

- **Stockout vs. Overstock**:  
  - Both **Stockouts (4)** and **Overstock (50)** cases fall under **PriceLevel = Normal**.  
  - Pricing is not the main driver → issues stem from **planning, forecasting, and supply chain execution gaps**.
    
### Inventory recommendations  
- **Balance Inventory Across Locations**  
  - Reduce excessive stock at **Subassembly/Miscellaneous Storage** and redistribute to **downstream stages**.  
  - Apply **line balancing** or **Kanban systems** for smoother inventory flow.  

- **Reduce Overstock**  
  - Analyze the **50 Overstock cases** (e.g., forecasting errors, overproduction, low sales).  
  - Apply **ABC/XYZ analysis** to better align stock with **demand variability**.  

- **Set KPI Targets**  
  - **Reduce Stockouts**: from **4 cases → 0** (short-term).  
  - **Cut Overstock**: by **30–40% within 1 year**.  
  - **Improve Reordering**: keep **“Reorder Needed” < 5% of total SKUs** through better **forecasting** and **supplier lead time control**.
  
### Quality Control insights  
- **Overall Scrap Performance**:  
  - **Scrap rate**: 24% → relatively high.  
  - **Total scrap cost**: ~$359.95K.  
  - **729 Work Orders** affected, generating **11K+ scrap products**.  

- **By Year**:  
  - Scrap rate rose from **~12% in 2011 → ~29% in 2013**, then dropped to **~20% in 2014**.  
  - Indicates **improvement after 2013**, but performance remains **unstable**.  

- **By Location**:  
  - Highest scrap in **Subassembly (21%)**, followed by **Frame Welding (18%)** and **Frame Forming (18%)**.  
  - Most scrap originates from **early-stage production processes**.  

- **By Scrap Reason**:  
  - Top reasons: **Seat assembly not as ordered (3.1%)**, **Drill size too large (2.9%)**, **Paint process failed (2.9%)**, **Trim length too long (2.9%)**.  
  - Issues mainly tied to **machine setup** and **quality inspection processes**.  

- **By Product**:  
  - Scrap rates across products are **relatively high**, often **>50%**.  
  - Critical cases: **ML Road Frame – Red (75%)** and **Mountain-100 Black (63%)**.  
  - Suggests quality issues are **widespread across multiple SKUs**.  

### Quality Control recommendations  
- **Target High-Scrap Products**  
  - Prioritize process review for **top 3 products** with highest scrap rates.  
  - Apply **root cause analysis** (Fishbone, 5 Whys) to identify design, material, or process issues.  

- **Improve Subassembly & Welding**  
  - Enhance **machine calibration** and **operator training** in high-scrap stages.  
  - Implement **early-stage quality checkpoints** to prevent downstream defects.  

- **Cost Reduction Strategy**  
  - A **10% scrap reduction** could save **~$36K**.  
  - Focus on processes with **high cost + high scrap rate**.  

- **Sustain Quality Improvement**  
  - Develop a **Scrap KPI dashboard** with **weekly/monthly tracking**.  
  - Adopt **Kaizen / Lean Six Sigma** to maintain scrap levels **<15%**.  
