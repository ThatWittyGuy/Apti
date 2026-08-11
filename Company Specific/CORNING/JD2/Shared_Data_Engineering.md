# Shared — Data Engineering MCQs

### Applies to: JD2 (Data & AI Engineering), any JD mentioning pipelines, ETL, data analytics

> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What does ETL stand for?

- A) Extract, Transform, Load
- B) Export, Transfer, Log
- C) Execute, Test, Launch
- D) Encode, Translate, Layer

**Answer: A**

---

**Q2.** 🟢 What is the difference between ETL and ELT?

- A) ETL is for cloud; ELT is for on-premise
- B) ETL transforms data before loading into the target; ELT loads raw data first and transforms it inside the target system (common in cloud data warehouses)
- C) They are identical processes
- D) ELT is the reverse of ETL — it loads, then extracts

**Answer: B**

---

**Q3.** 🟢 What is a "data source" in a data pipeline?

- A) The destination where processed data is stored
- B) Any system or file that provides raw data to be ingested — e.g., databases, APIs, files, IoT sensors, ERP systems
- C) The power source for a data center server
- D) A Python library that generates sample data

**Answer: B**

---

**Q4.** 🟢 What is "data ingestion"?

- A) Processing and transforming raw data
- B) The process of collecting and importing raw data from various sources into a storage or processing system for further use
- C) Archiving old data to long-term storage
- D) Visualizing data in a dashboard

**Answer: B**

---

**Q5.** 🟢 What is a "data warehouse"?

- A) A physical building where servers are stored
- B) A centralized, structured repository of integrated, historical data from multiple sources, optimized for analytical querying and reporting
- C) A NoSQL database for unstructured data
- D) A temporary staging area for raw data

**Answer: B**

---

**Q6.** 🟢 What is "data quality"? Name two dimensions.

- A) Database query speed; two dimensions: CPU and memory
- B) The fitness of data for its intended use; dimensions include accuracy (correctness), completeness (no missing values), consistency, timeliness, and uniqueness
- C) The format of a CSV file; dimensions: rows and columns
- D) A data governance policy; dimensions: security and privacy

**Answer: B**

---

**Q7.** 🟡 What is a "staging area" in an ETL pipeline?

- A) The production database
- B) A temporary intermediate storage area where raw data is loaded before transformation and loading into the final target — used for validation, deduplication, and error handling
- C) A QA environment for testing pipelines
- D) A cache for frequently queried data

**Answer: B**

---

**Q8.** 🟡 What is "incremental loading" vs. "full load" in ETL?

- A) Incremental loads all data every run; full load loads only changes
- B) Full load replaces all target data with the complete source data each run; incremental loading extracts and loads only new or changed data since the last run — more efficient for large datasets
- C) They are the same approach with different tool names
- D) Incremental loading is for streaming; full load is for batch only

**Answer: B**

---

**Q9.** 🟡 What is a "slowly changing dimension" (SCD) in data warehousing?

- A) A dimension that loads slowly due to performance issues
- B) A dimension table whose attribute values change slowly over time (e.g., customer address, employee department) — with different SCD types defining how historical changes are handled
- C) A dimension with infrequently queried columns
- D) A dimension that changes with each business day

**Answer: B**

---

**Q10.** 🟡 What is SCD Type 2?

- A) Overwriting the old value with the new value
- B) Adding a new row for each change, with start/end date flags to preserve full history while marking the current record
- C) Storing only the most recent two values
- D) A two-column dimension with current and previous values

**Answer: B**

---

**Q11.** 🟡 What is "data deduplication"?

- A) Deleting all backup copies of data
- B) The process of identifying and removing duplicate records from a dataset to ensure each entity appears only once
- C) Compressing data to reduce storage usage
- D) Creating redundant copies of critical data

**Answer: B**

---

**Q12.** 🟡 What is "data transformation" in an ETL pipeline?

- A) Converting a database from SQL to NoSQL
- B) Applying rules, calculations, and mappings to convert raw source data into the required format, structure, and quality for the target system
- C) Translating data documentation into another language
- D) Migrating data from one cloud provider to another

**Answer: B**

---

**Q13.** 🟡 What is "Apache Airflow" used for?

- A) A real-time streaming engine like Kafka
- B) A workflow orchestration tool that schedules, monitors, and manages data pipeline DAGs (Directed Acyclic Graphs) — defining task dependencies and execution order
- C) A distributed SQL engine for big data
- D) A machine learning training framework

**Answer: B**

---

**Q14.** 🟡 What is a DAG (Directed Acyclic Graph) in the context of data pipelines?

- A) A type of database index
- B) A graph of tasks where edges define dependencies and there are no cycles — ensuring each task runs only after its dependencies complete, used in tools like Airflow and Spark
- C) A network topology for distributed systems
- D) A visualization of data relationships

**Answer: B**

---

**Q15.** 🟡 What is "schema-on-read" vs. "schema-on-write"?

- A) Schema-on-read validates data before writing; schema-on-write validates when reading
- B) Schema-on-write enforces structure when data is stored (traditional databases); schema-on-read applies structure only when data is queried (data lakes) — enabling flexibility in storage
- C) They are identical concepts in different tools
- D) Schema-on-read is for JSON; schema-on-write is for CSV

**Answer: B**

---

**Q16.** 🟡 What is "Apache Parquet" and why is it preferred for analytics?

- A) A Python library for data parsing
- B) A columnar storage file format that is highly efficient for analytical queries — enabling compression, column pruning, and predicate pushdown — widely used in data lakes
- C) A row-based file format for transactional data
- D) A data serialization format like JSON or XML

**Answer: B**

---

**Q17.** 🟡 What is a "data mart"?

- A) An online marketplace for data products
- B) A subset of a data warehouse focused on a specific business domain or department (e.g., finance mart, sales mart) — optimized for that domain's analytical needs
- C) A data storage tier between hot and cold
- D) A lightweight version of a data lake

**Answer: B**

---

**Q18.** 🟡 What is "change data capture" (CDC)?

- A) Capturing screenshots of data changes for audits
- B) A technique that tracks and captures changes (inserts, updates, deletes) in a source database in real or near-real time, enabling incremental data synchronization
- C) A version control system for database schemas
- D) A data backup strategy

**Answer: B**

---

**Q19.** 🟡 What is "data partitioning" in big data processing?

- A) Dividing a dataset into training and test sets
- B) Splitting large datasets into smaller, manageable chunks based on a key (e.g., date, region) to enable parallel processing and efficient query pruning
- C) Partitioning a hard drive for data storage
- D) Separating structured from unstructured data

**Answer: B**

---

**Q20.** 🟡 What is a "lookup table" in data transformation?

- A) A table used to look up database errors
- B) A reference table mapping codes or IDs to descriptive values, used during transformation to enrich or decode data (e.g., mapping product_id to product_name)
- C) A temporary cache table for query optimization
- D) A table of all SQL keywords for reference

**Answer: B**

---

**Q21.** 🔴 What is "data lakehouse" architecture?

- A) A physical data center built near a lake
- B) An architecture combining the flexibility and scale of a data lake with the data management, ACID transactions, and query performance features of a data warehouse — e.g., Delta Lake, Apache Iceberg
- C) A hybrid cloud and on-premise data warehouse
- D) A data lake with a graphical user interface

**Answer: B**

---

**Q22.** 🔴 What is "Apache Iceberg"?

- A) A cold storage solution for archived data
- B) An open table format for huge analytic datasets that adds ACID transactions, schema evolution, time travel, and partition pruning on top of data lake storage (S3, ADLS, GCS)
- C) A distributed SQL query engine
- D) A streaming data processing framework

**Answer: B**

---

**Q23.** 🔴 What is "data skew" in distributed processing (e.g., Spark)?

- A) Data that is incorrectly formatted
- B) An imbalance where some partitions contain significantly more data than others, causing certain tasks to take much longer and becoming a bottleneck in distributed jobs
- C) A statistical bias in training data
- D) Data that is stored in the wrong format

**Answer: B**

---

**Q24.** 🔴 What is "pushdown optimization" in data pipelines?

- A) Physically pushing data down to lower storage tiers
- B) Moving computation (filters, aggregations) down to the data source level so less data is transferred over the network — reducing I/O and improving pipeline efficiency
- C) Optimizing a machine learning model's gradient descent
- D) A caching strategy for database queries

**Answer: B**

---

**Q25.** 🔴 What is "data serialization" and why does it matter in pipelines?

- A) Ordering data in serial (sequential) fashion for processing
- B) Converting data structures into a format (JSON, Avro, Protobuf, Parquet) that can be stored or transmitted and later deserialized — important for compatibility, performance, and schema evolution in pipelines
- C) Encrypting data before transmission
- D) Converting SQL data types to Python types

**Answer: B**

---

**Q26.** 🔴 What is "Apache Avro" and when is it used over Parquet?

- A) A Java build tool used in data pipelines
- B) A row-based, schema-embedded serialization format ideal for write-heavy streaming use cases (Kafka messages) where Parquet's columnar format would be inefficient
- C) A columnar format for analytics, similar to Parquet
- D) A Python library for data validation

**Answer: B**

---

**Q27.** 🔴 What is "watermarking" in stream processing?

- A) Adding metadata branding to data files
- B) A mechanism in streaming systems that tracks event time progress, allowing the system to handle late-arriving data and know when a time window is complete enough to compute results
- C) A data lineage tagging strategy
- D) A security feature for data access control

**Answer: B**

---

**Q28.** 🔴 What is "exactly-once semantics" in stream processing?

- A) Processing each record at least once to avoid data loss
- B) A guarantee that each record is processed exactly once — no duplicates and no data loss — the strongest and most complex delivery guarantee in streaming systems
- C) Processing records in exactly the correct order
- D) Sending exactly one message per event in a pipeline

**Answer: B**

---

**Q29.** 🔴 What is "data vault" modeling?

- A) A secure storage system for sensitive data
- B) A database modeling methodology designed for enterprise data warehouses using three types of tables — Hubs (unique business keys), Links (relationships), and Satellites (descriptive attributes) — optimized for auditability and agile loading
- C) An encrypted data warehouse architecture
- D) A version control system for data warehouse schemas

**Answer: B**

---

**Q30.** 🔴 What is "data quality monitoring" in production pipelines?

- A) Manually reviewing data files before each pipeline run
- B) Automated, continuous checking of data against defined rules (completeness, freshness, distribution, referential integrity) at each pipeline stage, with alerting when anomalies are detected
- C) A one-time data profiling activity before go-live
- D) Running SQL COUNT queries to check row numbers

**Answer: B**

---

**Q31.** 🟡 What is a "surrogate key" in data warehousing?

- A) A key copied from the source system
- B) A system-generated, meaningless integer key assigned to each row in a dimension table — decoupling the warehouse from source system key changes and enabling SCD management
- C) A secondary index on a dimension column
- D) A foreign key that references another dimension

**Answer: B**

---

**Q32.** 🟡 What is "data masking" and when is it used?

- A) Hiding data files from unauthorized file system users
- B) Replacing sensitive data values with fictitious but realistic values — used in non-production environments so developers can test with realistic data without exposing real PII
- C) Encrypting data at rest in a data warehouse
- D) Filtering sensitive columns from dashboard views

**Answer: B**

---

**Q33.** 🟡 What is "orchestration" vs. "choreography" in data pipelines?

- A) Orchestration uses a central controller; choreography is a type of complex data dance
- B) Orchestration uses a central coordinator (like Airflow) that directs all tasks; choreography has each service react to events independently with no central controller
- C) They are identical concepts in different tools
- D) Orchestration is for batch; choreography is for streaming

**Answer: B**

---

**Q34.** 🔴 What does "idempotency" mean in the context of data pipelines?

- A) A pipeline that runs only once
- B) The property that running a pipeline or operation multiple times produces the same result as running it once — critical for safe retries after failures
- C) A pipeline that processes data in identical batches
- D) A data deduplication strategy

**Answer: B**

---

**Q35.** 🔴 What is "DBT (Data Build Tool)" used for?

- A) A database backup tool
- B) A SQL-based transformation framework that runs inside a data warehouse — allowing analysts to write modular, version-controlled, tested SQL models with dependency management
- C) A data pipeline orchestration tool like Airflow
- D) A NoSQL database management tool

**Answer: B**

---

_35 Questions | Shared across data-focused JDs._
