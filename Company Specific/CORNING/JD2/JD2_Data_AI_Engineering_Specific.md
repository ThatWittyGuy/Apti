# JD2 — Data & AI Engineering Specific MCQs

### Role: IT Intern – Analyst, Data & AI Engineering

> Role-specific framing: AI-enabled data solutions, data delivery, automation, PoC/prototyping
> 🟢 Easy | 🟡 Medium | 🔴 Hard
> Python → Shared_Python.md | SQL → Shared_SQL.md | AI/ML → Shared_AI_ML.md
> Data Engineering → Shared_Data_Engineering.md | Visualization → Shared_Data_Visualization.md
> Cloud → Shared_Cloud_Platforms.md | Data Structures → Shared_Data_Structures.md

---

## SECTION A: AI-Enabled Data Solutions & Integration (35 Questions)

**Q1.** 🟢 What is a "proof of concept" (PoC) in a data/AI project?

- A) A finalized production-ready AI system
- B) A small-scale experiment that demonstrates the feasibility of an approach or technology before committing to full development
- C) A legal document proving ownership of a concept
- D) A test environment for production systems

**Answer: B**

---

**Q2.** 🟢 What does "data-driven decision-making" mean?

- A) Decisions made exclusively by data scientists
- B) Using data analysis and insights — rather than intuition alone — to guide business decisions
- C) Automating all business decisions with ML models
- D) Storing all business decisions in a database

**Answer: B**

---

**Q3.** 🟢 What is "data cleansing" (data cleaning)?

- A) Physically cleaning storage hardware
- B) The process of detecting and correcting inaccurate, incomplete, duplicate, or improperly formatted records in a dataset
- C) Archiving old data to cold storage
- D) Encrypting sensitive data fields

**Answer: B**

---

**Q4.** 🟢 What is "data quality" in the context of analytics and AI?

- A) The storage capacity of a database
- B) The degree to which data is accurate, complete, consistent, timely, valid, and fit for its intended use
- C) The speed at which data is processed
- D) The number of columns in a dataset

**Answer: B**

---

**Q5.** 🟢 What is a "data pipeline"?

- A) A physical cable network for data transfer
- B) An automated sequence of processes that ingests raw data from sources, transforms it, and loads it into a destination for storage or analysis
- C) A Python function that processes one row at a time
- D) A network protocol for database communication

**Answer: B**

---

**Q6.** 🟢 What does "automation" mean in a data engineering context?

- A) Replacing all data engineers with robots
- B) Using software tools, scripts, or workflows to perform repetitive data tasks (ingestion, transformation, reporting) without manual intervention
- C) Automatically generating ML models from raw data
- D) A hardware upgrade for faster processing

**Answer: B**

---

**Q7.** 🟡 What is the difference between structured, semi-structured, and unstructured data?

- A) They differ only in file size
- B) Structured data has a fixed schema (e.g., SQL tables); semi-structured has flexible schema (e.g., JSON, XML); unstructured has no predefined schema (e.g., text, images, video)
- C) Structured data is digital; unstructured is analog
- D) They are different names for the same concept depending on the tool used

**Answer: B**

---

**Q8.** 🟡 What is "data lineage" and why does it matter for AI solutions?

- A) The age of data records in a database
- B) The ability to trace data from its origin through every transformation to its final use — critical for debugging, compliance, and trusting AI model outputs
- C) The genealogy of database schema versions
- D) A log of all user queries on the database

**Answer: B**

---

**Q9.** 🟡 What is "data preparation" in the context of machine learning?

- A) Preparing a database backup before training
- B) The end-to-end process of collecting, cleaning, transforming, and formatting raw data into a form suitable for training or feeding ML models
- C) Writing SQL queries to retrieve training data
- D) Configuring GPU hardware for model training

**Answer: B**

---

**Q10.** 🟡 What is "feature selection" in data preparation for ML?

- A) Selecting the best ML framework for a project
- B) Identifying and keeping only the most relevant input variables from the dataset, removing redundant or irrelevant features to improve model performance and reduce complexity
- C) Selecting the target variable (label) for supervised learning
- D) Choosing which rows of data to include in training

**Answer: B**

---

**Q11.** 🟡 What is a "data catalog"?

- A) A printed directory of all company databases
- B) A centralized metadata repository that helps users discover, understand, and govern data assets across an organization
- C) A catalog of machine learning models
- D) A configuration file listing data pipeline steps

**Answer: B**

---

**Q12.** 🟡 What is "data profiling"?

- A) Creating user profiles from data
- B) Analyzing a dataset to understand its structure, content, and quality — including distributions, null counts, uniqueness, and patterns — before processing
- C) Profiling the performance of a database query
- D) Building a customer data profile for marketing

**Answer: B**

---

**Q13.** 🟡 What is an "API integration" in a data delivery context?

- A) Integrating multiple APIs into a single codebase
- B) Connecting two systems so that data flows between them via API calls — one system requests data and the other responds, enabling automated data exchange
- C) Installing a REST API framework on a server
- D) Documenting all APIs in a project

**Answer: B**

---

**Q14.** 🟡 What is the purpose of a "data schema"?

- A) A visual flowchart of a data pipeline
- B) A formal definition of the structure, data types, constraints, and relationships of data within a database or file — ensuring consistency and enabling validation
- C) A schema.org metadata format for web data
- D) A project planning document for data teams

**Answer: B**

---

**Q15.** 🟡 What is "batch processing" vs. "real-time (streaming) processing" in data engineering?

- A) Batch is faster; real-time is more accurate
- B) Batch processing collects and processes data in large groups at scheduled intervals; real-time/streaming processes data continuously as it arrives with low latency
- C) Batch uses Python; real-time uses SQL
- D) They are the same, just different naming conventions

**Answer: B**

---

**Q16.** 🟡 What is a "data lake"?

- A) A large on-premise database for transactional data
- B) A centralized repository that stores vast amounts of raw data in its native format (structured, semi-structured, unstructured) until needed — optimized for scale and flexibility
- C) A backup storage system for production databases
- D) A cloud-based SQL database

**Answer: B**

---

**Q17.** 🟡 What is a "data warehouse" and how does it differ from a data lake?

- A) A data warehouse stores raw data; a data lake stores only processed data
- B) A data warehouse stores structured, processed, and curated data optimized for analytics and reporting; a data lake stores raw data of any format for flexibility
- C) They are identical — just different marketing terms
- D) A data warehouse is on-premise; a data lake is always cloud-based

**Answer: B**

---

**Q18.** 🟡 What is "data governance" in a data delivery team?

- A) Governing who is allowed to install data tools
- B) The policies, standards, roles, and processes that ensure data is accurate, consistent, secure, and used appropriately across the organization
- C) A compliance audit of database configurations
- D) Managing the version control of data files

**Answer: B**

---

**Q19.** 🟡 In a data analytics context, what is a KPI (Key Performance Indicator)?

- A) A type of SQL query
- B) A quantifiable metric used to evaluate how effectively an organization or process is achieving a key business objective
- C) A Python performance benchmarking tool
- D) A machine learning evaluation metric

**Answer: B**

---

**Q20.** 🟡 What is "data integration" in an enterprise context?

- A) Integrating data science teams into IT departments
- B) Combining data from multiple disparate sources into a unified, consistent view for analysis or operational use
- C) Installing data processing software
- D) Connecting a data lake to the internet

**Answer: B**

---

**Q21.** 🟡 What does "data normalization" mean in the context of database design (not ML preprocessing)?

- A) Scaling numeric values to a 0–1 range
- B) Organizing a relational database to minimize redundancy and dependency by dividing data into related tables following normal form rules
- C) Converting all text to lowercase
- D) Removing duplicate rows from a table

**Answer: B**

---

**Q22.** 🟡 What is an "analytical output" in a business intelligence context?

- A) The output terminal of an analytics server
- B) Any deliverable generated from data analysis — such as a report, dashboard, chart, summary, or recommendation — that communicates insights to stakeholders
- C) A raw data export from a database
- D) A Python script output printed to console

**Answer: B**

---

**Q23.** 🟡 What is "data enrichment"?

- A) Increasing the file size of a dataset
- B) Enhancing existing data by supplementing it with additional information from internal or external sources to make it more complete and valuable
- C) Converting data to a richer file format (e.g., CSV to Parquet)
- D) Adding more rows to a training dataset

**Answer: B**

---

**Q24.** 🔴 What is an "ML pipeline" in a production AI system?

- A) A data cable connecting ML servers
- B) An end-to-end automated workflow covering data ingestion → preprocessing → feature engineering → model training → evaluation → deployment → monitoring
- C) A Python script that trains an ML model
- D) A CI/CD pipeline specifically for ML code

**Answer: B**

---

**Q25.** 🔴 What is "MLOps"?

- A) Machine Learning Operations — a set of practices combining ML, DevOps, and data engineering to streamline building, deploying, monitoring, and maintaining ML models in production
- B) A Python library for ML model management
- C) A cloud service for training ML models
- D) An SAP module for AI operations

**Answer: A**

---

**Q26.** 🔴 What is "model versioning" and why is it important?

- A) Tracking software version numbers for ML frameworks
- B) Keeping track of different iterations of a trained model (code, data, hyperparameters, performance metrics) to enable reproducibility, rollback, and comparison
- C) Versioning the training dataset only
- D) A feature in Python for managing script versions

**Answer: B**

---

**Q27.** 🔴 What is "data drift" vs. "concept drift" in production ML?

- A) They are the same phenomenon
- B) Data drift is when the statistical distribution of input features changes over time; concept drift is when the relationship between features and the target variable changes — both degrade model performance
- C) Data drift affects only structured data; concept drift affects unstructured
- D) Concept drift is a change in business terminology; data drift is a storage issue

**Answer: B**

---

**Q28.** 🔴 What is a "feature store" in ML engineering?

- A) A marketplace for buying ML features/models
- B) A centralized repository for storing, sharing, and serving computed features — enabling consistency between training and inference, and reuse across teams and models
- C) A database storing raw data before feature extraction
- D) A configuration store for ML hyperparameters

**Answer: B**

---

**Q29.** 🔴 What is "data mesh" architecture?

- A) A network topology for data centers
- B) A decentralized data architecture where domain teams own their data as products with defined interfaces, rather than a central data team owning all data — improving scalability and ownership
- C) A mesh network for IoT data collection
- D) A data compression algorithm

**Answer: B**

---

**Q30.** 🔴 What is "Apache Kafka" used for in data engineering?

- A) A batch processing framework like Hadoop
- B) A distributed event streaming platform used to publish, subscribe to, store, and process real-time streams of data across systems at high throughput
- C) A SQL query engine for big data
- D) A data visualization library

**Answer: B**

---

**Q31.** 🔴 What is "Apache Spark" and what problem does it solve?

- A) A web server for serving ML models
- B) A distributed in-memory data processing engine that enables fast, large-scale batch and streaming data processing across clusters — overcoming the disk I/O bottleneck of MapReduce
- C) A SQL database for big data storage
- D) A Python library for data visualization

**Answer: B**

---

**Q32.** 🔴 What is the "Medallion Architecture" (Bronze/Silver/Gold) in data engineering?

- A) An award system for data engineers
- B) A layered data lake design: Bronze stores raw ingested data, Silver stores cleaned/validated data, Gold stores business-ready aggregated data optimized for analytics
- C) A security classification for sensitive data
- D) A database indexing strategy for performance

**Answer: B**

---

**Q33.** 🔴 What is "data observability"?

- A) A visualization technique for complex datasets
- B) The ability to understand the health and state of data in a system through monitoring of freshness, distribution, volume, schema, and lineage — detecting anomalies before they impact downstream consumers
- C) Monitoring the performance of data engineers
- D) Logging all SQL queries for audit purposes

**Answer: B**

---

**Q34.** 🔴 What is "zero-ETL" or "direct query federation"?

- A) An ETL pipeline with no transformation step
- B) An approach where data is queried in place at the source without physically moving/copying it to a central store — reducing latency, storage costs, and pipeline maintenance
- C) Running ETL with zero human oversight
- D) A cloud service that auto-generates ETL code

**Answer: B**

---

**Q35.** 🔴 What is the purpose of "data contracts" in data engineering?

- A) Legal agreements between data vendors
- B) Formal agreements between data producers and consumers defining the schema, quality standards, SLAs, and semantics of data — ensuring producers don't break downstream consumers with unannounced changes
- C) GDPR compliance contracts for data usage
- D) API authentication contracts

**Answer: B**

---

_End of JD2 role-specific file. For foundational MCQs see:_
_Python → Shared_Python.md | SQL → Shared_SQL.md | AI/ML → Shared_AI_ML.md_
_Data Engineering → Shared_Data_Engineering.md | Visualization → Shared_Data_Visualization.md_
_Cloud → Shared_Cloud_Platforms.md | Data Structures → Shared_Data_Structures.md_
