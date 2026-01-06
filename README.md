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

Demonstrates experience working beyond Power BI and collaborating with data engineering teams.

| Topic | Answer |
| :--- | :--- |
| **Role of Databricks in BI architecture** | Databricks serves as a scalable data engineering and transformation layer where raw data is processed and optimized before being consumed by Power BI. |
| **ETL vs ELT pipelines** | I typically follow an ELT approach, loading raw data first and performing transformations within Databricks using Spark, which improves scalability and flexibility. |
| **Delta Tables** | Delta tables provide ACID transactions, schema enforcement, versioning, and performance optimizations such as data skipping, making them ideal for analytical workloads. |
| **Optimization techniques** | Common techniques include partitioning, Z-ordering, caching, and managing file sizes to improve query performance and downstream BI consumption. |
| **Databricks integration with Power BI** | Power BI connects to Databricks using Import or DirectQuery, depending on latency and performance requirements. Databricks typically provides curated, analytics-ready datasets. |

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

## 🛠 Repository Structure

* `/Modeling`: Deep dives into Star Schema vs. Snowflake, Data Vault, and normalization.
* `/SQL-Snippets`: Optimized queries for common BI reporting needs.
* `/Power-BI-DAX`: Patterns for time intelligence and complex measures.
* `/Pipelines`: Transformations and ETL logic using Databricks and Spark.

---

## 🚀 About the Author
I am a Data Enthusiast focused on bridging the gap between raw data and executive insights. I use this repository to document my learning journey and professional growth in the cloud-scale analytics space.
