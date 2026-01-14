# ☑️📌BI & Analytics Handbook

Welcome to my personal knowledge base for **Business Intelligence and Analytics Engineering**. This repository serves as a centralized hub for data modeling patterns, advanced DAX, and end-to-end data pipeline best practices.

---

## 🧠 Data Modeling 

One of the foundational pillars of this repository is mastering the **Star Schema**.

| Topic | Answer |
| :--- | :--- |
| **How do you design a star schema?** | A star schema consists of a central **fact table** with measurable data and foreign keys, surrounded by **dimension tables** that store descriptive attributes. This structure improves query performance, simplifies DAX, and makes the model easier for users to understand. |
| **Why prefer star schema over snowflake?** | I generally prefer a star schema because it is simpler, more intuitive, and performs better in Power BI. Star schemas reduce the number of joins required during query execution, which improves performance and makes the model easier to understand for both developers and business users. From a DAX perspective, star schemas also reduce ambiguity in relationships, making calculations more predictable and easier to maintain. Snowflake schemas may be more normalized, but the added complexity often hurts usability and performance in reporting scenarios. |
| **How does star schema help DAX performance?** | Star schema helps DAX performance by simplifying filter propagation and reducing relationship complexity. In a star schema, filters flow cleanly from dimension tables to the fact table through one-to-many relationships. This allows the VertiPaq engine to efficiently resolve filter context and aggregate measures. Because there are fewer joins and no chained dimension relationships, DAX calculations execute faster, are easier to debug, and avoid unexpected results caused by ambiguous paths. |
| **When would you break star schema rules?** | I break strict star schema rules only when there is a clear technical or business justification. Common examples include handling very large dimensions, reducing model size, or supporting specific requirements such as role-playing dimensions, many-to-many relationships, or complex hierarchies. In some cases, partial snowflaking or bridge tables can be acceptable if they significantly improve performance or data governance. However, I always balance this against increased complexity and make sure the design remains understandable and well-documented. | 
| **Fact vs Dimension Tables** | Fact tables store quantitative, measurable data such as sales, revenue, quantity, or cost. They are typically large and contain foreign keys that link to dimension tables. Dimension tables store descriptive, qualitative attributes such as product, customer, date, or geography. They provide context for analysis and are relatively smaller. In Power BI, facts are where measures aggregate data, while dimensions are used for filtering, slicing, and grouping results. |
| **Handling Many-to-Many Relationships** | I handle many-to-many relationships by introducing a bridge table whenever possible. The bridge table contains distinct key combinations and allows relationships to remain one-to-many, which improves model stability and DAX behavior. While Power BI supports native many-to-many relationships, I use them cautiously because they can introduce ambiguity and performance issues. I prefer explicit modeling so filter propagation is predictable and easier to debug. |

---

## 🧮 DAX (Advanced Concepts)

This section focuses on advanced DAX concepts, calculation behavior, and performance best practices commonly asked in senior BI interviews.

| Topic | Answer |
| :--- | :--- |
| **What is CALCULATE and why is it important?** | CALCULATE modifies the filter context of a measure. It allows expressions to be evaluated under different filters than those applied by visuals. CALCULATE is essential for time intelligence, conditional logic, and implementing complex business rules by adding, removing, or overriding filters. |
| **Row Context vs Filter Context** | Row context exists when calculations are evaluated row by row, typically in calculated columns or iterator functions like SUMX. Filter context comes from slicers, filters, and visuals and determines which rows are included in a calculation. Understanding how CALCULATE transitions row context into filter context is critical for correct DAX results. |
| **Time Intelligence Patterns** | Time intelligence relies on a properly designed and marked Date table. Common patterns include YTD, MTD, QTD, Same Period Last Year, and rolling periods, using functions such as TOTALYTD, SAMEPERIODLASTYEAR, DATEADD, and DATESINPERIOD. |
| **Dynamic Measures (SWITCH, SELECTEDVALUE)** | Dynamic measures use SWITCH and SELECTEDVALUE to change logic based on user selections such as KPIs, scenarios, or currencies. This allows a single measure to support multiple analytical views while keeping the model clean and reusable. |
| **How do you design dynamic measures without hurting performance?** | I reuse base measures, minimize branching logic, avoid deeply nested SWITCH statements, and keep calculations aggregation-friendly. I also rely on a well-designed star schema so DAX can leverage efficient filter propagation instead of complex logic. |

---


## 🔄 Power Query / M (Data Preparation)

Focused on data transformation, query folding, and applying logic in the correct layer.

| Topic | Answer |
| :--- | :--- |
| **What transformations do you perform in Power Query?** | I perform data cleansing, shaping, merging, appending, deduplication, and standardization. This includes fixing data types, renaming columns, handling null values, and applying consistent business rules before loading data into the model. |
| **What is query folding and why is it important?** | Query folding occurs when Power Query pushes transformations back to the source system. It improves refresh performance, reduces memory usage, and leverages the processing power of the data source. |
| **Combining multiple data sources** | I combine multiple sources using merge or append operations while monitoring query folding. When structures differ, I normalize them into a common schema before loading into the semantic model. |
| **When do you use Power Query vs DAX?** | I use Power Query for structural, repeatable transformations that should occur at refresh time. I use DAX for calculations that must respond dynamically to filters, slicers, and user interaction. |
| **Handling data quality issues** | I handle data quality issues as early as possible by validating keys, removing duplicates, standardizing values, and flagging anomalies. This improves trust, consistency, and simplifies downstream modeling and reporting. |

 🔹 Basic Functions

| Function | Purpose | Syntax Example |
|----------|---------|----------------|
| `Text.Proper` | Converts text to proper case (first letter capitalized). | `Text.Proper("hello world")` → `"Hello World"` |
| `Text.Upper` | Converts text to uppercase. | `Text.Upper("hello")` → `"HELLO"` |
| `Text.Lower` | Converts text to lowercase. | `Text.Lower("HELLO")` → `"hello"` |
| `Number.Round` | Rounds a number to the nearest integer or specified decimals. | `Number.Round(3.14159, 2)` → `3.14` |
| `DateTime.LocalNow` | Returns the current system date and time. | `DateTime.LocalNow()` |
| `Table.RenameColumns` | Renames columns in a table. | `Table.RenameColumns(Source, {{"OldName","NewName"}})` |
| `Table.RemoveColumns` | Removes specified columns. | `Table.RemoveColumns(Source, {"Column1","Column2"})` |
| `Table.SelectRows` | Filters rows based on a condition. | `Table.SelectRows(Source, each [Sales] > 1000)` |


🔹 Advanced Functions

| Function | Purpose | Syntax Example |
|----------|---------|----------------|
| `Table.Group` | Groups rows by one or more columns and applies aggregations. | `Table.Group(Source, {"Region"}, {{"TotalSales", each List.Sum([Sales]), type number}})` |
| `Table.Join` | Joins two tables on a matching key. | `Table.Join(Table1, "ID", Table2, "ID", JoinKind.Inner)` |
| `Table.Combine` | Appends multiple tables into one. | `Table.Combine({Table1, Table2})` |
| `Table.AddColumn` | Adds a calculated column. | `Table.AddColumn(Source, "DiscountedPrice", each [Price] * 0.9)` |
| `Record.FieldValues` | Returns values from a record as a list. | `Record.FieldValues([Name="Cyrus", Age=35])` → `{"Cyrus", 35}` |
| `List.Transform` | Applies a function to each item in a list. | `List.Transform({1,2,3}, each _ * 2)` → `{2,4,6}` |
| `Table.ExpandRecordColumn` | Expands nested records into columns. | `Table.ExpandRecordColumn(Source, "Details", {"City","Country"})` |
| `Table.Buffer` | Forces evaluation and caches a table in memory (useful for performance tuning). | `Table.Buffer(Source)` |

- **Basic functions** handle text, numbers, dates, and simple table operations.  
- **Advanced functions** deal with grouping, joining, transformations, and performance optimization.  
- Always consider **query folding** when applying transformations — push logic back to the source whenever possible.  
- Use **Power Query (M)** for repeatable, refresh-time transformations; use **DAX** for dynamic, user-driven calculations.  

---

## ⚡ Power BI Performance Optimization

Focused on building efficient, scalable, and enterprise-ready models.

| Topic | Answer |
| :--- | :--- |
| **Import vs DirectQuery vs Composite Models** | Import mode provides the best performance and is preferred whenever possible. DirectQuery is used for near real-time requirements but introduces performance trade-offs. Composite models combine both to balance flexibility and performance. |
| **Cardinality and its impact** | High-cardinality columns reduce compression efficiency and increase memory usage. I reduce cardinality by removing unnecessary columns, using surrogate keys, and avoiding high-cardinality text fields in fact tables. |
| **Model size optimization** | I optimize model size by removing unused columns, selecting appropriate data types, reducing precision where possible, and pushing transformations upstream. Smaller models refresh faster and perform better. |
| **Performance Analyzer usage** | I use Performance Analyzer to identify slow visuals, expensive DAX queries, and inefficient interactions. This helps isolate whether issues originate from the model, DAX, or report design. |

---

## 🧱 Databricks & Data Pipelines

| Topic | Answer |
| :--- | :--- |
| **Role of Databricks** | Serves as the scalable **Lakehouse** layer where raw data is processed using Spark and Delta Lake before BI consumption. |
| **Databricks vs. Fabric** | Databricks is most similar to the **Fabric Lakehouse**. Both utilize **Delta Lake** and the **Medallion Architecture**. While Fabric Warehouse focuses on T-SQL, Databricks (and Fabric Lakehouse) focuses on Spark-based engineering and open-format storage. |
| **Unity Catalog vs. OneLake** | Databricks governs data through **Unity Catalog**, providing cross-cloud governance. Fabric centralizes data in **OneLake**, acting as a unified "SaaS" storage layer for the entire organization. |
| **Delta Tables** | Delta tables provide ACID transactions, schema enforcement, and versioning, making them the "gold standard" for both Databricks and Fabric environments. |

---

## 👉 Azure Synapse & Azure Data Factory (ADF)

This section compares **Azure Synapse Analytics** and **Azure Data Factory (ADF)** with **Microsoft Fabric**, highlighting their engines, tools, and languages.  
As someone certified in **PL-300 (Power BI)** and **DP-600 (Fabric)**, this helps connect the Azure data services with Fabric’s unified SaaS-first approach.

| Topic | Answer |
|-------|--------|
| **Role of Azure Synapse Analytics** | Synapse is a **cloud-based analytics service** that combines big data and data warehousing. It enables querying structured and semi-structured data at scale. |
| **Synapse Engine, Tools, Languages** | - **Engine:** Dedicated SQL Pools (MPP engine), On-demand Serverless SQL<br>- **Tools:** Synapse Studio, Azure Portal<br>- **Languages:** **T-SQL**, integration with **Spark (Python, Scala, R)** |
| **Synapse vs. Fabric** | Synapse overlaps with **Fabric Warehouse** (T-SQL focus) and **Fabric Lakehouse** (Spark focus). Both provide scalable analytics, but Fabric integrates more tightly with **Power BI** and **OneLake** for a unified SaaS experience. |
| **Role of Azure Data Factory (ADF)** | ADF is the **cloud-native ETL/ELT service**, orchestrating data movement and transformation across diverse sources. |
| **ADF Engine, Tools, Languages** | - **Engine:** ADF Integration Runtime (cloud or self-hosted)<br>- **Tools:** ADF Studio (Azure Portal)<br>- **Languages:** GUI-driven pipelines, **Power Query (M language)**, **JSON** for pipeline definitions |
| **ADF vs. Fabric** | ADF is most similar to **Fabric Data Pipelines**. Both orchestrate data ingestion and transformation, but Fabric simplifies this by embedding pipelines directly into the SaaS ecosystem with **OneLake** as the central storage. |

- **Azure Synapse** and **ADF** are Azure-native services that map closely to Fabric’s **Warehouse, Lakehouse, and Data Pipelines**.  
- **Engines:**  
  - Synapse → Dedicated SQL Pools & Spark runtime  
  - ADF → Integration Runtime  
  - Fabric → Warehouse (T-SQL), Lakehouse (Spark), Pipelines (M/Power Query)  
- **Tools:**  
  - Synapse Studio ↔ Fabric Workspaces  
  - ADF Studio ↔ Fabric Data Pipelines UI  
- **Languages:**  
  - Synapse → T-SQL + Spark (Python/Scala/R)  
  - ADF → JSON + M language  
  - Fabric → T-SQL + Spark + DAX/M  

- **Similarities in Purpose:**  
  - Synapse ↔ Fabric Warehouse/Lakehouse → **Scalable analytics & querying**  
  - ADF ↔ Fabric Data Pipelines → **Data movement & transformation**  

- If you’ve worked with **Synapse or ADF**, transitioning to **Fabric** is straightforward — the concepts are nearly identical but unified in one SaaS platform.  
- If you’re new (like shifting from Power BI only), Fabric abstracts much of the complexity, letting you focus on **analytics and storytelling** rather than infrastructure.  

---

## 👉 Deployment & Governance

Covers enterprise deployment, governance, and consulting best practices.

| Topic | Answer |
| :--- | :--- |
| **Gateways (Personal vs Standard)** | Personal gateways are intended for individual use, while standard gateways support enterprise scenarios with multiple datasets, users, and centralized management. |
| **Refresh strategies** | Refresh strategies include scheduled refresh, incremental refresh, and near real-time DirectQuery, depending on data volume and business needs. |
| **Dataflows** | Dataflows centralize data preparation logic, promote reusability, and support governance by separating data transformation from semantic modeling. |
| **Workspace architecture** | I follow a Dev / Test / Prod workspace strategy to support controlled deployments, testing, and governance. |
| **Dataset sharing strategies** | I prefer shared and certified datasets to enforce a single source of truth, reduce duplication, and improve consistency across reports. |

---

## 💼 Power BI Licensing (Consulting Scenarios)

Focused on advisory decision-making and cost optimization.

| Topic | Answer |
| :--- | :--- |
| **Pro vs Premium Per User vs Premium Capacity** | Pro is suitable for small teams. Premium Per User enables advanced features for individuals. Premium Capacity supports large-scale deployments and broad content distribution without requiring Pro licenses for viewers. |
| **When to recommend Power BI Embedded** | Power BI Embedded is ideal for external-facing applications where reports are embedded into custom apps and users do not require Power BI licenses. |
| **How would you advise a client with 300 users?** | I assess usage patterns, data volume, refresh frequency, and sharing needs. In many cases, Premium Capacity is cost-effective for large audiences, while Premium Per User may suffice if advanced features are limited to a smaller group. |

---
## 👉Technical & Architectural Interview Guide

| Question Category | Scenario / Context | Expert Consultant Answer |
| :--- | :--- | :--- |
| **Data Architecture** | **Direct Lake vs. DirectQuery** | Use **Direct Lake** (Fabric) for high-volume data to get Import-mode speed without the data copy. Use **DirectQuery** only if you need real-time T-SQL logic or the data exceeds Fabric's capacity. |
| **Data Modeling** | **Star Schema vs. Flat Tables** | Always advocate for **Star Schema**. It optimizes the VertiPaq engine, prevents double-counting on different granularities (e.g., Budget vs. Actuals), and simplifies DAX logic. |
| **DAX Optimization** | **Performance Troubleshooting** | Use **Performance Analyzer** to find the bottleneck. If DAX is slow, use **DAX Studio** to check for "CallbackDataID" or expensive iterators. Check for high-cardinality columns bloating the model. |
| **Security** | **Dynamic Row-Level Security (RLS)** | Use `USERPRINCIPALNAME()` mapped to a security bridge table. This allows one single role to filter data based on the logged-in user's credentials and organizational hierarchy. |
| **Advanced DAX** | **Calculated Columns vs. Measures** | Measures are calculated on the fly and respond to filters (CPU intensive); Columns are calculated during refresh (Memory intensive). As a rule: **"Do it in the source, then Power Query, then DAX measures."** |
| **ETL / Integration** | **Query Folding** | Ensure transformations happen at the SQL source level. If a step in Power Query turns "off" folding, the engine downloads all data to the local machine, causing significant refresh delays. |
| **Platform Knowledge** | **Microsoft Fabric Integration** | Fabric is the unified SaaS layer. Discuss **OneLake** (the "OneDrive for data") and how **Power BI** now interacts directly with Lakehouses and Warehouses without needing separate silos. |
| **Data Integrity** | **SCD Type 2 (Historical Data)** | Use surrogate keys in the fact table to link to specific versions of a dimension record. This ensures that historical sales remain tied to the territory/manager active at the *time of the sale*. |
| **Governance** | **Deployment Pipelines** | Use **Power BI Deployment Pipelines** (Dev -> Test -> Prod). This ensures consistency, allows for parameterizing connection strings, and prevents accidental deletions in production. |
| **Consulting Skills** | **Ambiguous Requirements** | Start with **KPI Discovery**. Ask: "What decision will this report help you make?" and "What does success look like in 6 months?" Build a Wireframe (low-fidelity) before touching the data. |

## 👉Key DAX Functions to Review
* **`CALCULATE()`**: The most important function; modifies filter context.
* **`REMOVEFILTERS()` / `ALL()`**: Essential for calculating percentages of totals.
* **`USERPRINCIPALNAME()`**: Core for dynamic security.
* **`SUMMARIZECOLUMNS()`**: The gold standard for querying data in modern Power BI.
* **`TREATAS()`**: Useful for virtual relationships when a physical one isn't possible.

## 👉Interview Strategy: 
* **Azure-Only Focus:** Emphasize that you are a specialist in the Microsoft ecosystem.
* **People First:** Mention how your solutions enable "Data Democracy" (making data easy for non-technical users).
* **Performance:** Always mention that a report is only good if it's fast and accurate.

---
# ☑️📌Senior BI Consultant Study Guide: DAX & Soft Skills

This guide is tailored for Senior Consultant roles at elite Microsoft partners. It focuses on architectural impact rather than just basic syntax.

## 👉1. Consultant-Grade DAX Cheat Sheet

| Function | Primary Purpose | Senior-Level "Pro-Tip" |
| :--- | :--- | :--- |
| `CALCULATE()` | The "Brain" of DAX. Modifies filter context. | Always use `KEEPFILTERS()` within CALCULATE if you want to preserve existing slicer selections rather than overwriting them. |
| `SUMX() / AVERAGEX()` | Iterative functions that perform row-by-row logic. | Avoid using these on huge tables (10M+ rows) if a standard aggregation will suffice. Iterators are CPU-intensive. |
| `ALL()` / `REMOVEFILTERS()` | Removes filters to calculate "Grand Totals." | Use `REMOVEFILTERS()` for clarity in modern DAX; it performs exactly like `ALL()` but is more descriptive for other devs. |
| `ALLSELECTED()` | Keeps the filters coming from slicers/pages but ignores visual-level filters. | This is the "Gold Standard" for calculating "% of visible total" in dynamic dashboards. |
| `USERPRINCIPALNAME()` | Returns the email of the logged-in user. | The foundation of **Dynamic RLS**. Combine with a bridge table to secure data for thousands of users with a single role. |
| `TREATAS()` | Applies the result of a table expression as filters to columns from an unrelated table. | Use this for "Virtual Relationships" when you cannot (or should not) create a physical line in the model view. |
| `SELECTEDVALUE()` | Captures what a user picked in a slicer. | Perfect for "Dynamic Titles" or switching metrics (e.g., swapping a chart between "Sales $" and "Units Sold"). |
| `COALESCE()` | Returns the first non-blank value. | Use this to replace `IF(ISBLANK(...))` for cleaner code and slightly better performance. |
| `SWITCH(TRUE()...)` | A cleaner alternative to nested IF statements. | Use the `TRUE()` pattern for complex conditional logic (e.g., `SWITCH(TRUE(), [Sales] > 1M, "Gold", [Sales] > 500k, "Silver", "Bronze")`). |

---

## 👉2. Behavioral & Soft Skills for Consultants (The STAR Method)

As a Senior Consultant, you are an advisor. Prepare stories using the **S**ituation, **T**ask, **A**ction, **R**esult format.

### Q1: "Tell me about a time you dealt with a difficult stakeholder."
* **The Scenario:** A stakeholder demands a "Flat Table" export because they "don't trust the dashboard."
* **The Answer:** Focus on **Active Listening** and **Education**.
    * *Action:* I met with them 1-on-1 to find the root of their distrust. I discovered they were comparing a "Gross" number in Excel to a "Net" number in Power BI.
    * *Result:* I aligned the definitions, added a "Data Dictionary" page to the report, and earned their buy-in, leading to a 40% increase in dashboard adoption.

### Q2: "How do you handle ambiguous requirements?"
* **The Scenario:** A client says, "I just need a dashboard for my sales team," but provides no KPIs.
* **The Answer:** Focus on **Discovery Frameworks**.
    * *Action:* I lead a "Design Thinking" session. I ask, "What is the first question you want to answer when you wake up?" and "What action will you take if this KPI turns red?" 
    * *Result:* We moved from a generic request to a "Action-Oriented" dashboard that reduced meeting times by 15 minutes daily.

### Q3: "Tell me about a time you failed or made a technical mistake."
* **The Scenario:** You deployed a report that was extremely slow in production.
* **The Answer:** Focus on **Accountability** and **Optimization**.
    * *Action:* I took ownership, used **DAX Studio** to find a poorly written `FILTER(ALL())` measure, and refactored it using `KEEPFILTERS`.
    * *Result:* The report load time dropped from 12 seconds to 2 seconds. I then created a "Performance Checklist" for the whole team to prevent it happening again.

---

## 👉3. High-Level Architectural Concepts (2026 Focus)

* **Medallion Architecture:** Be able to explain **Bronze** (Raw), **Silver** (Cleansed), and **Gold** (Aggregated/Business-Ready) layers within a Lakehouse.
* **Vibe Coding / AI:** Mention how you use **Copilot for DAX** to speed up development, but emphasize that you manually audit the code for performance.
* **Governance:** Mention **Purview** for data lineage and **Deployment Pipelines** for moving code from Dev to Prod.

---

# ☑️📌Senior Consultant: Power BI & Data Specialist Study Guide

## 👉1. Technical Deep-Dive: Power BI & Databricks Integration

Emphasizes the use of **Databricks** for data pipelines. You must be able to explain how the "Lakehouse" meets the "Dashboard."

| Scenario | Expert Answer |
| :--- | :--- |
| **Connecting Power BI to Databricks** | Use the **Databricks Connector** in Power BI. For high performance, recommend **Partner Connect**. Mention the advantage of using **Unity Catalog** for centralized governance across both platforms. |
| **Optimization: Delta Lake** | Explain **Z-Ordering** and **Data Skipping** in Databricks. As a BI consultant, explain that optimizing the Delta tables *before* Power BI connects is more effective than trying to fix performance in DAX. |
| **DirectQuery vs. Import on Databricks** | For massive datasets, use **DirectQuery** with **Databricks SQL (DSQL)** to leverage the **Photon** engine. For smaller, high-speed interaction, use **Import Mode**. |
| **Medallion Architecture** | Explain the flow: **Bronze** (Raw Landing) -> **Silver** (Cleansed/Joined) -> **Gold** (Aggregated for Power BI). Power BI should ideally only ever point to the **Gold** layer. |

---

## 👉2. The "Consultant" Edge: Licensing & Discovery

Expects you to act as a **trusted advisor** regarding Microsoft's complex licensing.

### Power BI Licensing Matrix (2026 Context)
| License Type | Best For... | Key Constraint |
| :--- | :--- | :--- |
| **Power BI Pro** | Small to mid-size teams. | 1GB model limit; 8 refreshes/day. |
| **Premium Per User (PPU)** | Small teams needing advanced AI/Paginated reports. | Content can ONLY be shared with other PPU users. |
| **Fabric / Premium Capacity (F-SKUs)** | Large enterprises (500+ users). | No "per-reader" cost for F64+ SKUs. Includes OneLake. |

**Discovery Question Scenario:**
* **Question:** "A client has 800 employees who only need to *view* reports. Which license is best?"
* **Answer:** "I would recommend **Microsoft Fabric (F64 SKU)** or higher. At 800 users, the 'Per User' cost of Pro ($10-$14/mo) exceeds the cost of a dedicated capacity. This also gives the organization access to OneLake and unified data engineering tools."

---

## 👉3. Legacy Transitions: SQL Server & SSIS to Azure

The job mentions "conventional SQL Server" (SSRS/SSIS). You may be asked how to migrate these to the cloud.

* **SSRS -> Power BI:** Explain that **Paginated Reports** in Power BI are the direct successor to SSRS. You can host `.rdl` files directly in the Power BI Service.
* **SSIS -> Databricks/Data Factory:** Explain that legacy SSIS packages can be "lifted and shifted" into **Azure Data Factory (ADF)**, but the modern approach is to refactor them into **Databricks Notebooks** or **Medallion pipelines** for better scalability.
* **SQL DW -> Synapse/Fabric:** Emphasize moving from a fixed-resource SQL warehouse to a decoupled **Storage (OneLake) + Compute (Fabric SQL)** model.

---

## 👉4. Cultural Fit (The "Core Values" Interview)

Consulting firms explicitly hires for **Accountability** and **Candid Feedback**.

* **"Roll up your sleeves":** Be ready with a story where you did "grunt work" (like cleaning a messy CSV) to help the team meet a deadline.
* **"Experiment or Fail":** Talk about a time you tried a new tool (like a Python script for data cleaning) that didn't work, what you learned, and how you pivoted back to a stable solution.
* **"Candid Feedback":** "Tell me about a time you disagreed with a manager." Focus on how you provided **data-backed** feedback respectfully to save the project from a technical error.

---

## 👉5. Required Technical Keywords for your STAR Answers
* **Power Query/M:** Mention "M-parameters" for dynamic filtering.
* **DAX:** Mention "Context Transition" (Row context to Filter context).
* **Dimensional Modeling:** Mention "Surrogate Keys" and "Bus Matrix."
* **Security:** Mention "Object-Level Security (OLS)" vs "Row-Level Security (RLS)."
---
# ☑️📌 BI Lab Setup Guide: Local & Cloud Environment

This guide outlines how to build a local "Enterprise BI" sandbox on your laptop to practice the legacy and modern tools required for the Senior Consultant role.

## 👉1. Local Software Installation (Order Matters)

Follow this sequence to ensure all components communicate correctly:

| Component | Tool to Download | Purpose |
| :--- | :--- | :--- |
| **1. Database Engine** | [SQL Server 2022 Developer Edition](https://www.microsoft.com/en-us/sql-server/sql-server-downloads) | The "Warehouse" that stores your data tables. |
| **2. Management** | [SQL Server Management Studio (SSMS)](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms) | The UI where you write T-SQL and manage databases. |
| **3. Development** | [Visual Studio 2022 Community](https://visualstudio.microsoft.com/vs/community/) | The IDE used for building SSIS (ETL) and SSRS (Reports). |
| **4. BI Extensions** | **SSDT (SQL Server Data Tools)** | Inside Visual Studio, go to *Extensions > Manage Extensions* and install: <br> 1. *Integration Services Projects* <br> 2. *Reporting Services Projects* |

---

## 👉2. Load Sample Data (AdventureWorks)

Don't create data from scratch. Use the industry-standard "AdventureWorks" data warehouse.

1.  **Download**: Get the `AdventureWorksDW2022.bak` file from the [Official Microsoft GitHub](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks).
2.  **Restore**:
    * Open **SSMS** and connect to your local server.
    * Right-click **Databases** > **Restore Database...**
    * Select **Device**, click the `...`, and add your downloaded `.bak` file.
3.  **Verify**: You should now see tables like `FactInternetSales`, `DimCustomer`, and `DimProduct`.

---

## 👉3. Practice Projects & Learning Path

### Project A: SQL Mastery (Foundation)
* **Task**: Write a T-SQL view that joins `FactInternetSales` with `DimProduct` and `DimDate`. 
* **Goal**: Ensure you can create a "Gold" layer view that Power BI can consume easily.

### Project B: SSIS (Legacy ETL)
* **Task**: Create a new Integration Services project in Visual Studio.
* **Workflow**: 
    1.  Create a **Data Flow Task**.
    2.  Use a **Flat File Source** (a CSV of your choice).
    3.  Add a **Derived Column** transformation (e.g., convert a string to Uppercase).
    4.  Use an **OLE DB Destination** to save that data into your `AdventureWorks` database.

### Project C: SSRS (Paginated Reporting)
* **Task**: Create a "Report Server Project".
* **Goal**: Build a pixel-perfect table showing *Monthly Sales by Region*. Practice adding a **Parameter** so users can select a specific year.

### Project D: Power BI (The Final Layer)
* **Task**: Connect Power BI Desktop to your local SQL Server.
* **Goal**: Compare **Import Mode** vs. **DirectQuery** performance using your local AdventureWorks tables.

---

## 👉4. Modern Cloud Practice (Databricks)

Since Databricks cannot run locally, use the cloud-based free tier:

1.  **Sign Up**: [Databricks Community Edition](https://community.cloud.databricks.com/login.html).
2.  **Practice**:
    * Upload a small CSV to the **DBFS** (Databricks File System).
    * Create a **Notebook** and write simple Spark SQL: 
        ```sql
        SELECT * FROM delta.`/path/to/your/file` WHERE column_name = 'Example'
        ```
    * Understand the **Medallion Architecture**: Try to "move" data from a raw folder (Bronze) to a cleaned table (Silver).

---

## 👉5.  SSRS, SSIS, SQL Server & Their Similarities to Fabric

This section compares **traditional SQL Server tools** with **Microsoft Fabric**, highlighting their engines, tools, and languages.  
As someone certified in **PL-300 (Power BI)** and **DP-600 (Fabric)**, this bridges the gap between legacy SQL Server technologies and Fabric’s modern cloud-native ecosystem.

---

**Comparison Table**

| Topic | Answer |
|-------|--------|
| **Role of SSIS (SQL Server Integration Services)** | SSIS is the **ETL engine** for SQL Server, designed to move and transform data. |
| **SSIS Engine, Tools, Languages** | - **Engine:** SSIS Runtime Engine (on-premises)<br>- **Tools:** Visual Studio (SQL Server Data Tools), SSIS Designer<br>- **Languages:** Primarily GUI-driven workflows, with support for **T-SQL** and **C#/.NET scripting** |
| **SSIS vs. Fabric** | Fabric replaces SSIS with **Data Pipelines** and **Dataflows**, powered by **Spark** and **Power Query (M language)**. Both aim to **move and transform data** for analytics. |

| **Role of SSRS (SQL Server Reporting Services)** | SSRS is the **reporting engine** for SQL Server, focused on paginated, operational reports. |
| **SSRS Engine, Tools, Languages** | - **Engine:** SSRS Report Server<br>- **Tools:** Report Builder, Visual Studio (SSDT)<br>- **Languages:** **RDL (Report Definition Language)**, **T-SQL** for queries |
| **SSRS vs. Fabric** | Fabric integrates reporting into **Power BI Workspaces**, using **DAX** and **M language** for interactive dashboards. Both deliver **insights to end-users**. |

| **Role of SQL Server (Database Engine)** | SQL Server is the **relational database engine**, supporting queries, transactions, and secure storage. |
| **SQL Server Engine, Tools, Languages** | - **Engine:** SQL Server Database Engine<br>- **Tools:** SQL Server Management Studio (SSMS), Azure Data Studio<br>- **Languages:** **T-SQL** (Transact-SQL) |
| **SQL Server vs. Fabric** | Fabric offers **Lakehouse** (Spark-based) and **Warehouse** (T-SQL-based) engines. Both enable **data storage and querying for analysis**. |

- **Engines:**  
  - SSIS → ETL Runtime Engine  
  - SSRS → Report Server Engine  
  - SQL Server → Relational Database Engine  
  - Fabric → **Lakehouse (Spark)** + **Warehouse (T-SQL)** + **Power BI Engine (DAX/M)**  

- **Tools:**  
  - Legacy: SSMS, SSIS Designer, Report Builder  
  - Fabric: Power BI Service, Data Pipelines, Notebooks, OneLake  

- **Languages:**  
  - Legacy: T-SQL, RDL, C#/VB.NET scripting  
  - Fabric: T-SQL (Warehouse), Spark/Scala/Python (Lakehouse), DAX & M (Power BI/Dataflows)  

- **Similarities in Purpose:**  
  - SSIS ↔ Fabric Data Pipelines → **Data movement & transformation**  
  - SSRS ↔ Fabric Power BI → **Reporting & insights delivery**  
  - SQL Server ↔ Fabric Lakehouse/Warehouse → **Data storage & querying**  

---

- Legacy SQL Server tools are **engine-specific and siloed**.  
- Fabric unifies these into a **cloud-native SaaS platform**, reducing complexity.  
- Your **Power BI + Fabric certifications (PL-300, DP-600)** already cover the modern equivalents of SSIS, SSRS, and SQL Server.  
---
* **When asked about SSIS**: *"I am familiar with the architecture of SSIS packages, including Control Flow and Data Flow tasks, and I understand how to migrate that logic into modern cloud tools like Azure Data Factory or Databricks."*
* **When asked about SQL**: *"I focus on writing SARGable (Search ARGumentable) queries and ensuring my views support Query Folding in Power BI to maximize performance."*

---

# ☑️📌 About the Author
My name is **Cyrus Baruc**, I’m 34 years old, based in Lapu-Lapu City with my wife and kids.  
My family is my daily motivation—they inspire me and keep me grounded.

I worked as an **Electronics Technician** for more than ten years.  
During that time, I also explored **stock trading** and started learning **data analysis, data visualization, Python, and Excel**.  
Those experiences gave me both a strong analytical foundation and the perspective of a **business user**—understanding how data and charts connects to strategy and decision-making.  

That combination helped me transition into **data and business intelligence**.  
Today, I specialize in **Power BI**, focusing not only on building dashboards but also on **data processing, data modeling and ETL**, ensuring the data is clean, reliable, and delivers real business value.

Outside of work, I enjoy **journaling** to capture ideas that come to me during the day, and I make sure to spend quality time with my family.

My goal is to **inspire other professionals to see the power of data**.  
I believe **data is the new oil**—it fuels innovation, efficiency, and smarter decisions.  
I want to continue growing in this field and use my success as a way to encourage others to embrace data as a driver of transformation.

# ⭐⭐⭐⭐ Projects
**Client**: D&E Air Conditioning Pty Ltd — Australia‑wide Mechanical Services & Air Conditioning solutions
**Projects Supported:**  
- Large‑scale commercial buildings (offices, hotels, shopping centres)  
- Industrial facilities and manufacturing plants  
- Hospitals, laboratories, and pharmaceutical environments  
- Data centres and technology hubs  
- Education and government infrastructure  
**Stakeholders:** Project Managers, Construction Managers, Engineering Managers, Executives, General Manager
**Projects:**  Project Tracking Dashboards | Tender Phasing & Revenue Forecasting | Dynamics 365 Model‑Driven Power Apps Support
- Gathered business requirements using a structured template, ensuring stakeholder alignment and clarity of project objectives.
- Prepared data using Dataflows Gen1 and Power Query, applying medallion architecture principles to process structured, semi‑structured, and unstructured data, consolidating diverse on‑premises sources into a staging dataflow for streamlined transformation and enterprise reporting.
- Applied snowflake and star schema principles to design optimized data models, improving query performance and reducing report load times in the reporting layer.
- Developed executive‑ready dashboards with drill‑down analytics, enabling self‑service insights through intuitive visualizations and paginated reports tailored to business needs.
- Validated data accuracy, managed workspaces, and applied security roles for secure asset sharing; performed ad‑hoc analyses to resolve data mismatches and deliver additional views and features requested by stakeholders.

---

Before D&E, I worked at Lexmark as a Finance Business Analyst. I partnered closely with finance stakeholders to gather business requirements and translate them into BI solutions for forecasting, budgeting, and performance reporting. I led multiple reporting projects, delivering dashboards for revenue, cost, OPEX, cost center analysis, extended warranty amortization, and worldwide expense bridging. I ensured data accuracy and governance and continuously optimized BI solutions to support evolving business needs.

--- 

I also worked part-time at AvantGuard, where I integrated data from IT and endpoint management applications such as Halopsa, NinjaOne, Cove, and others to build internal dashboards monitoring users, machines, and devices. Later, I delivered client-facing BI projects, performing ETL, dimensional modeling, and dashboard development to automate financial reporting and replace manual spreadsheets. This reduced manual effort, improved accuracy and scalability, and gave clients clearer operational and financial insights. 

--- 

At Moderno Plant X, I built dashboards to monitor manufacturing operational metrics, including Overall Equipment Effectiveness (OEE), in near real time, helping operations teams identify issues quickly and improve overall performance.

--- 
At Excigence, I built a Python application to manage electronics test procedures and scripts for semiconductor device production. It uses Pandas and Matplotlib to automatically analyze the test data, generate summarized logs, and create reports after each run, which really helped make the testing process faster and more accurate.

Earlier in my career, I worked as a Senior Board Repair Specialist, supporting automated test equipment (Teradyne) for electronics production. I analyzed repair and failure data to identify recurring issues and improve processes, using problem-solving and troubleshooting techniques. I also used diagnostic tools that applied statistical analysis, including Process Capability Index, to detect micro-variances and subtle differences in test results. Additionally, I built Excel-based tracking systems to monitor performance, which gave me a strong foundation in data analysis that I now apply in BI and analytics.

