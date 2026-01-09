# BI & Analytics Handbook

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

## 🚀 Deployment & Governance

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
## Technical & Architectural Interview Guide

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

## Key DAX Functions to Review
* **`CALCULATE()`**: The most important function; modifies filter context.
* **`REMOVEFILTERS()` / `ALL()`**: Essential for calculating percentages of totals.
* **`USERPRINCIPALNAME()`**: Core for dynamic security.
* **`SUMMARIZECOLUMNS()`**: The gold standard for querying data in modern Power BI.
* **`TREATAS()`**: Useful for virtual relationships when a physical one isn't possible.

## Interview Strategy: 
* **Azure-Only Focus:** Emphasize that you are a specialist in the Microsoft ecosystem.
* **People First:** Mention how your solutions enable "Data Democracy" (making data easy for non-technical users).
* **Performance:** Always mention that a report is only good if it's fast and accurate.

---
# Senior BI Consultant Study Guide: DAX & Soft Skills

This guide is tailored for Senior Consultant roles at elite Microsoft partners. It focuses on architectural impact rather than just basic syntax.

## 1. Consultant-Grade DAX Cheat Sheet

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

## 2. Behavioral & Soft Skills for Consultants (The STAR Method)

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

## 3. High-Level Architectural Concepts (2026 Focus)

* **Medallion Architecture:** Be able to explain **Bronze** (Raw), **Silver** (Cleansed), and **Gold** (Aggregated/Business-Ready) layers within a Lakehouse.
* **Vibe Coding / AI:** Mention how you use **Copilot for DAX** to speed up development, but emphasize that you manually audit the code for performance.
* **Governance:** Mention **Purview** for data lineage and **Deployment Pipelines** for moving code from Dev to Prod.
---
# 3Cloud (Cognizant) Senior Consultant: Power BI & Data Specialist Study Guide

## 1. Technical Deep-Dive: Power BI & Databricks Integration

Emphasizes the use of **Databricks** for data pipelines. You must be able to explain how the "Lakehouse" meets the "Dashboard."

| Scenario | Expert Answer |
| :--- | :--- |
| **Connecting Power BI to Databricks** | Use the **Databricks Connector** in Power BI. For high performance, recommend **Partner Connect**. Mention the advantage of using **Unity Catalog** for centralized governance across both platforms. |
| **Optimization: Delta Lake** | Explain **Z-Ordering** and **Data Skipping** in Databricks. As a BI consultant, explain that optimizing the Delta tables *before* Power BI connects is more effective than trying to fix performance in DAX. |
| **DirectQuery vs. Import on Databricks** | For massive datasets, use **DirectQuery** with **Databricks SQL (DSQL)** to leverage the **Photon** engine. For smaller, high-speed interaction, use **Import Mode**. |
| **Medallion Architecture** | Explain the flow: **Bronze** (Raw Landing) -> **Silver** (Cleansed/Joined) -> **Gold** (Aggregated for Power BI). Power BI should ideally only ever point to the **Gold** layer. |

---

## 2. The "Consultant" Edge: Licensing & Discovery

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

## 3. Legacy Transitions: SQL Server & SSIS to Azure

The job mentions "conventional SQL Server" (SSRS/SSIS). You may be asked how to migrate these to the cloud.

* **SSRS -> Power BI:** Explain that **Paginated Reports** in Power BI are the direct successor to SSRS. You can host `.rdl` files directly in the Power BI Service.
* **SSIS -> Databricks/Data Factory:** Explain that legacy SSIS packages can be "lifted and shifted" into **Azure Data Factory (ADF)**, but the modern 3Cloud approach is to refactor them into **Databricks Notebooks** or **Medallion pipelines** for better scalability.
* **SQL DW -> Synapse/Fabric:** Emphasize moving from a fixed-resource SQL warehouse to a decoupled **Storage (OneLake) + Compute (Fabric SQL)** model.

---

## 4. 3Cloud Cultural Fit (The "Core Values" Interview)

3Cloud explicitly hires for **Accountability** and **Candid Feedback**.

* **"Roll up your sleeves":** Be ready with a story where you did "grunt work" (like cleaning a messy CSV) to help the team meet a deadline.
* **"Experiment or Fail":** Talk about a time you tried a new tool (like a Python script for data cleaning) that didn't work, what you learned, and how you pivoted back to a stable solution.
* **"Candid Feedback":** "Tell me about a time you disagreed with a manager." Focus on how you provided **data-backed** feedback respectfully to save the project from a technical error.

---

## 5. Required Technical Keywords for your STAR Answers
* **Power Query/M:** Mention "M-parameters" for dynamic filtering.
* **DAX:** Mention "Context Transition" (Row context to Filter context).
* **Dimensional Modeling:** Mention "Surrogate Keys" and "Bus Matrix."
* **Security:** Mention "Object-Level Security (OLS)" vs "Row-Level Security (RLS)."
---
## 🛠 Repository Structure

* `/Modeling`: Deep dives into Star Schema vs. Snowflake, Data Vault, and normalization.
* `/SQL-Snippets`: Optimized queries for common BI reporting needs.
* `/Power-BI-DAX`: Patterns for time intelligence and complex measures.
* `/Pipelines`: Transformations and ETL logic using Databricks and Spark.

---

## 🚀 About the Author
I am a Data Enthusiast focused on bridging the gap between raw data and executive insights. I use this repository to document my learning journey and professional growth in the cloud-scale analytics space.
