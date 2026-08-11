# Shared — Databricks MCQs
### Applies to: JD5 (Business Engagement), any JD mentioning Databricks, Spark, lakehouse
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What is Databricks?
- A) A database management system competing with Oracle
- B) A unified data analytics and AI platform built on Apache Spark — providing a collaborative environment for data engineering, data science, machine learning, and SQL analytics on cloud infrastructure
- C) A BI tool for creating dashboards and reports
- D) A cloud database service competing with Snowflake

**Answer: B**

---

**Q2.** 🟢 What is the core distributed processing engine underlying Databricks?
- A) Apache Hadoop MapReduce
- B) Apache Spark — an in-memory distributed data processing engine enabling fast batch and streaming analytics
- C) Apache Kafka
- D) Apache Flink

**Answer: B**

---

**Q3.** 🟢 What is a "notebook" in Databricks?
- A) A physical notepad for data scientists to take notes
- B) An interactive, collaborative document combining code (Python, Scala, SQL, R), markdown text, and visualizations — the primary development interface in Databricks for exploration and prototyping
- C) A Databricks configuration file
- D) A scheduled batch job definition

**Answer: B**

---

**Q4.** 🟢 What is a "cluster" in Databricks?
- A) A group of related Databricks workspaces
- B) A set of virtual machines (driver + worker nodes) that Databricks provisions to run notebooks and jobs — providing the distributed compute power for data processing
- C) A grouping of related Delta tables
- D) A network of Databricks tenants in the same region

**Answer: B**

---

**Q5.** 🟢 What cloud providers does Databricks run on?
- A) AWS only
- B) AWS, Microsoft Azure, and Google Cloud Platform (GCP)
- C) Azure and GCP only
- D) AWS and Azure only

**Answer: B**

---

**Q6.** 🟢 What is "Delta Lake" in Databricks?
- A) A cloud data lake storage service by Databricks
- B) An open-source storage layer that brings ACID transactions, schema enforcement, time travel (versioning), and efficient upserts to data lake files (Parquet) — making them reliable for analytics and ML
- C) A data lake architecture pattern (Bronze/Silver/Gold)
- D) A competitor to Apache Iceberg

**Answer: B** *(Note: Delta Lake is also open-source and distinct from general cloud storage, though C is a concept built with Delta Lake)*

---

**Q7.** 🟡 What does "ACID" stand for in the context of Delta Lake?
- A) Advanced Columnar Indexing Design
- B) Atomicity, Consistency, Isolation, Durability — guarantees ensuring database transactions are processed reliably; Delta Lake brings these to data lake files which traditionally lacked transactional support
- C) Automated CI/CD for Data
- D) Aggregated Columnar Index Design

**Answer: B**

---

**Q8.** 🟡 What is "time travel" in Delta Lake?
- A) A feature for scheduling jobs at future timestamps
- B) The ability to query previous versions of a Delta table using `VERSION AS OF` or `TIMESTAMP AS OF` — enabling data auditing, rollback of accidental changes, and reproducible ML experiments
- C) A method for converting between time zones in Delta tables
- D) A Databricks feature for running historical data backfills

**Answer: B**

---

**Q9.** 🟡 What is the "Databricks Lakehouse Platform"?
- A) A physical data center by Databricks
- B) Databricks' unified architecture combining the flexibility and scale of a data lake with the reliability and performance of a data warehouse — enabling BI, data science, and ML on the same platform and data
- C) A SaaS version of Apache Hadoop
- D) A managed Snowflake competitor

**Answer: B**

---

**Q10.** 🟡 What is a "Databricks workspace"?
- A) A physical office space for Databricks employees
- B) An environment in Databricks where users access notebooks, clusters, jobs, data, and libraries — serving as the collaboration space for a team or project, isolated by access controls
- C) A shared folder for storing Databricks notebooks
- D) A billing account unit in Databricks

**Answer: B**

---

**Q11.** 🟡 What is "Databricks Jobs" (workflows)?
- A) A job board for Databricks-related employment
- B) A feature for scheduling and orchestrating notebooks, Python scripts, Spark JARs, or dbt tasks as automated, recurring pipelines — with dependency management, retries, and alerting
- C) A task management tool integrated into Databricks
- D) A CI/CD feature for deploying Databricks notebooks

**Answer: B**

---

**Q12.** 🟡 What is "MLflow" in the Databricks ecosystem?
- A) A machine learning data pipeline tool
- B) An open-source ML lifecycle management platform — tracking experiments (parameters, metrics, artifacts), packaging models, managing a model registry, and enabling reproducible ML workflows; natively integrated into Databricks
- C) A Databricks feature for streaming ML predictions
- D) A Python library for building neural networks

**Answer: B**

---

**Q13.** 🟡 What is the "MLflow Model Registry" used for?
- A) Registering ML models with regulatory authorities
- B) A centralized model store in MLflow/Databricks for managing model versions through lifecycle stages (Staging, Production, Archived) — with approval workflows, annotations, and lineage tracking
- C) A marketplace for downloading pre-trained ML models
- D) A configuration store for ML hyperparameters

**Answer: B**

---

**Q14.** 🟡 What is "Databricks AutoML"?
- A) An automated system for generating Databricks notebooks
- B) A feature in Databricks that automatically trains and tunes multiple ML models on a dataset, generates a baseline notebook with the best model's code, and provides a leaderboard — accelerating ML experimentation
- C) An automated data quality tool in Databricks
- D) A no-code ML tool for business users without coding experience

**Answer: B**

---

**Q15.** 🟡 What is "Unity Catalog" in Databricks?
- A) A catalog of Databricks features and pricing plans
- B) Databricks' unified governance solution providing centralized access control, auditing, lineage, and data discovery across all data assets (tables, files, ML models, notebooks) in a Databricks account — enabling fine-grained permissions
- C) A data catalog tool competing with AWS Glue
- D) A catalog of pre-built Databricks connectors

**Answer: B**

---

**Q16.** 🟡 What is a "DataFrame" in PySpark (Databricks)?
- A) A Python pandas DataFrame stored in Databricks
- B) A distributed, tabular data structure in Spark (analogous to a SQL table or pandas DataFrame) that can hold large datasets across a cluster — supporting SQL-like operations and optimized execution via Catalyst optimizer
- C) A data frame for plotting visualizations in Databricks
- D) A configuration frame defining Spark cluster settings

**Answer: B**

---

**Q17.** 🟡 What is the difference between a "transformation" and an "action" in Apache Spark?
- A) Transformations write data; actions read data
- B) Transformations (e.g., `filter`, `select`, `join`) are lazy — they build an execution plan without running; actions (e.g., `count`, `show`, `write`) trigger actual computation across the cluster — Spark's lazy evaluation optimizes the full plan before executing
- C) Actions are distributed; transformations run on the driver only
- D) They are identical — just different terminology for the same operations

**Answer: B**

---

**Q18.** 🟡 What is "Databricks SQL" (DBSQL)?
- A) A variant of SQL specific to Databricks syntax
- B) A serverless SQL analytics service in Databricks allowing business analysts and data engineers to run SQL queries on Delta Lake and other data sources using SQL warehouses — with a BI-tool-friendly interface and dashboarding
- C) A tool for managing SQL server databases in Databricks
- D) A Databricks connector for querying external SQL databases

**Answer: B**

---

**Q19.** 🟡 What is "Delta Live Tables" (DLT) in Databricks?
- A) A real-time data visualization feature in Databricks
- B) A declarative ETL framework in Databricks where users define data pipeline logic as SQL or Python, and Databricks manages orchestration, data quality enforcement, dependency resolution, and error handling automatically
- C) A feature for creating live, real-time dashboards in Databricks SQL
- D) A streaming ingestion tool for Delta Lake

**Answer: B**

---

**Q20.** 🟡 What is "Databricks Feature Store"?
- A) A marketplace for purchasing Databricks add-on features
- B) A centralized repository in Databricks for storing, discovering, and serving ML features — ensuring consistency between training-time and inference-time feature computation and enabling reuse across models and teams
- C) A store for sharing Databricks notebooks across workspaces
- D) A configuration store for Databricks cluster settings

**Answer: B**

---

**Q21.** 🔴 What is "photon" in Databricks?
- A) A light-based data transfer protocol used by Databricks
- B) A native vectorized query engine in Databricks written in C++ that accelerates SQL and DataFrame operations on Delta Lake — significantly outperforming standard Spark for analytical workloads
- C) Databricks' GPU computing framework for deep learning
- D) A real-time data streaming component in Databricks

**Answer: B**

---

**Q22.** 🔴 What is "Databricks Model Serving"?
- A) A feature for serving Databricks notebooks to users
- B) A managed, scalable REST API endpoint service that deploys MLflow-registered models for real-time inference — handling auto-scaling, versioning, and A/B testing without managing infrastructure
- C) A tool for serving ML training jobs on a schedule
- D) A feature for serving SQL query results via API

**Answer: B**

---

**Q23.** 🔴 What is "Structured Streaming" in Apache Spark/Databricks?
- A) A Databricks feature for structured (tabular) batch processing
- B) A scalable, fault-tolerant stream processing engine built on Spark's batch API — treating a live data stream as an unbounded table and enabling the same DataFrame/SQL operations on streaming data as on batch data
- C) A structured file format for streaming data storage
- D) A Databricks tool for monitoring data pipeline health

**Answer: B**

---

**Q24.** 🔴 What is "Databricks Connect"?
- A) A network connector for linking Databricks to on-premise databases
- B) A client library allowing developers to run Spark code from their local IDE (VS Code, PyCharm) against a remote Databricks cluster — enabling local development and debugging without using the web notebook UI
- C) A Databricks API for connecting to external data sources
- D) A Databricks feature for connecting multiple workspaces

**Answer: B**

---

**Q25.** 🔴 What is "OPTIMIZE" and "ZORDER" in Delta Lake?
- A) SQL commands for optimizing Databricks cluster performance
- B) `OPTIMIZE` compacts small Delta Lake files into larger ones for better read performance; `ZORDER BY` co-locates related data within those files based on specified columns — dramatically improving query performance by reducing files scanned for filtered queries
- C) Databricks commands for optimizing ML model hyperparameters
- D) Data quality commands for enforcing schema constraints

**Answer: B**

---

**Q26.** 🔴 What is "data skipping" in Delta Lake?
- A) Skipping data quality checks for faster pipeline runs
- B) A performance optimization where Delta Lake maintains statistics (min/max values) for each file and uses them to skip reading files that cannot contain data matching a query's filter — dramatically reducing I/O
- C) A feature for skipping duplicate data during ingestion
- D) A Delta Lake technique for skipping schema validation

**Answer: B**

---

**Q27.** 🔴 What is "Databricks Asset Bundles" (DABs)?
- A) A collection of Databricks pre-built assets for ML
- B) A framework for defining, testing, and deploying Databricks resources (notebooks, jobs, pipelines, clusters) as code using YAML configuration files — enabling CI/CD and infrastructure-as-code practices for Databricks projects
- C) A bundle of Databricks features packaged for enterprise licensing
- D) A Python package manager for Databricks libraries

**Answer: B**

---

**Q28.** 🔴 What is "Mosaic AI" in Databricks?
- A) A Databricks visualization library for mosaic-style charts
- B) Databricks' suite of AI/ML tools and capabilities — encompassing model training, fine-tuning LLMs, model serving, vector search, AI-powered data engineering, and governance — unified under the Databricks platform
- C) An open-source AI framework developed by Databricks
- D) A Databricks tool for creating AI-generated data visualizations

**Answer: B**

---

**Q29.** 🔴 What is "Vector Search" in Databricks?
- A) A search tool for finding vectors in Delta Lake tables
- B) A managed vector database capability in Databricks that stores embeddings and enables fast similarity search — used to build RAG (Retrieval-Augmented Generation) applications by finding semantically similar documents for LLM context
- C) A Databricks SQL feature for searching array columns
- D) A feature for searching across multiple Databricks workspaces

**Answer: B**

---

**Q30.** 🔴 What is "LakehouseIQ" / "Databricks AI/BI" (formerly Databricks Genie)?
- A) A Databricks IQ test for data engineers
- B) An AI-powered natural language query interface in Databricks that lets business users ask questions about their data in plain English and receive SQL-generated answers and visualizations — without writing code
- C) A Databricks feature for AI-powered cluster optimization
- D) An AI assistant for writing Databricks notebooks

**Answer: B**

---

**Q31.** 🟡 What is "Repos" in Databricks?
- A) A file storage system for Databricks artifacts
- B) A Git integration feature in Databricks that allows users to connect notebooks and code to Git repositories (GitHub, GitLab, Azure DevOps) — enabling version control, branching, and CI/CD workflows for Databricks projects
- C) A repository of pre-built Databricks connectors
- D) A feature for managing Databricks cluster configurations

**Answer: B**

---

**Q32.** 🟡 What is the difference between an "all-purpose cluster" and a "job cluster" in Databricks?
- A) All-purpose clusters run SQL; job clusters run Python
- B) All-purpose clusters are long-running, interactive clusters for development and exploration (shared, always on, more expensive); job clusters are ephemeral clusters spun up and terminated automatically for a specific job run (cost-efficient for production workloads)
- C) Job clusters are larger; all-purpose clusters are smaller
- D) They are identical — just different naming conventions

**Answer: B**

---

**Q33.** 🔴 What is "cluster auto-scaling" in Databricks?
- A) Automatically upgrading the Databricks cluster software version
- B) A feature that dynamically adds or removes worker nodes from a cluster based on current workload — reducing costs during idle periods and ensuring sufficient resources during peak processing
- C) Automatically scaling storage attached to a Databricks cluster
- D) A feature that automatically restarts clusters after failures

**Answer: B**

---

**Q34.** 🔴 What is "schema enforcement" in Delta Lake?
- A) Enforcing that all data engineers follow the same naming conventions
- B) A feature that automatically validates incoming data against the Delta table's defined schema on write — rejecting data with mismatched columns or data types, preventing data corruption and maintaining reliability
- C) A Delta Lake feature enforcing data partitioning by schema
- D) A governance rule enforcing schema documentation standards

**Answer: B**

---

**Q35.** 🔴 What is "Databricks Lakehouse Monitoring"?
- A) A tool for monitoring Databricks cluster CPU and memory usage
- B) A feature that automatically profiles and monitors Delta Lake tables over time — tracking data quality metrics, statistical distributions, and model prediction drift, alerting when anomalies are detected
- C) A network monitoring tool for Databricks connectivity
- D) A Databricks admin tool for monitoring workspace usage

**Answer: B**

---
*35 Questions | Shared — Databricks.*
