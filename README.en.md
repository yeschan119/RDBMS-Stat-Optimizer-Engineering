# RDBMS Statistics & Optimizer Engineering
### Database Statistics Collection, Management, and Query Plan Optimization

[한국어 🇰🇷](README.Ko.md)

This repository documents engineering work related to the database statistics subsystem inside an RDBMS engine.

The work focused on improving statistics collection accuracy, enhancing optimizer plan quality, and implementing core logic for statistics storage and utilization.

---

## Project Scope

Responsible for the design, implementation, and optimization of database statistics features including:

- Table statistics collection and storage
- Index statistics collection and accuracy improvement
- Partition-level statistics management
- Histogram & skew-aware sampling logic
- Statistics utilization for query plan generation
- Optimizer integration using collected statistics

---

## Core Responsibilities

### 1️⃣ Statistics Collection & Management (PL/SQL)

- Implemented statistics collection procedures
- Designed storage schema for statistics metadata
- Developed sampling logic for large datasets
- Improved accuracy for skewed data distributions
- Implemented partition-aware statistics collection
- Built index statistics collection mechanisms

### 2️⃣ Optimizer Integration (C++)

- Integrated statistics into plan generation logic
- Enhanced cardinality estimation accuracy
- Improved cost model input quality
- Leveraged clustering factor and histogram data
- Optimized index scan vs full table scan decision logic

---

## 🔍 Key Engineering Challenges

### Index Statistics Accuracy Issue (Tmax)

Problem:
- Index statistics (especially clustering factor) showed low accuracy
- Poor clustering factor values caused severe performance degradation
- Index scan plans were misestimated

Root Cause:
- The query execution strategy used for index statistics collection was flawed
- The execution path did not reflect actual data ordering characteristics

Solution:
- Developed a dedicated statistics collection node specifically for index statistics
- Modified execution flow so that:
  - Only statistics collection queries for index operations used the new node
- Redesigned clustering factor calculation logic

Result:
- Achieved over 90% accuracy improvement
- Resolved index scan performance degradation
- Stabilized optimizer cost model decisions

---

## ⚙️ Implemented Features

### Improved Sampling Logic
- Adaptive sampling ratio
- Enhanced sampling precision for large tables
- Reduced estimation bias

### Skewed Data Handling
- Histogram refinement
- Selectivity correction logic
- Distribution-aware statistics computation

### Partition Statistics
- Partition-level statistics collection
- Global + Local stats reconciliation
- Improved partition pruning cost estimation

### Index Statistics Collection
- Clustering factor recalculation logic
- Dedicated execution node for index stats
- Enhanced selectivity estimation

---

## Architecture Perspective

Statistics Flow:

Data → Sampling → Statistics Computation → Storage → Optimizer Input → Plan Generation

Key Concepts:

- Cardinality Estimation
- Selectivity Calculation
- Cost-Based Optimization (CBO)
- Clustering Factor
- Histogram Distribution
- Partition-Level Metadata

---

## 🛠 Tech Stack

Languages:
- PL/SQL (Statistics Collection & Management)
- C++ (Optimizer & Plan Generation Logic)

Domain:
- RDBMS Engine Development
- Cost-Based Query Optimization
- Database Internals

---

## Technical Impact

- Improved clustering factor accuracy by over 90%
- Stabilized index scan performance
- Reduced optimizer misestimation
- Enhanced cost model reliability
- Improved plan selection stability under skewed workloads

---

## Engineering Focus

This work demonstrates:

- Deep understanding of database statistics mechanisms
- Practical experience with query optimizer internals
- Strong knowledge of cost-based optimization
- Ability to modify execution engine components
- End-to-end ownership from statistics collection to plan generation
