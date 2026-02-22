---
title: "Understanding The AI Ladder Part 1: Turning Data Into Actionable Insights and Enterprise Value"
subtitle: "A walkthrough of IBM’s AI Ladder and how organizations move from collecting data to infusing AI into an enterprise"
author: "Henry Xiao"
date: 2026-01-31
tags:
  - AI
  - Data
  - IBM
  - Enterprise AI
  - watsonx
---

*The AI Ladder: Accelerate Your Journey to AI*, authored by Rob Thomas, IBM Senior Vice President, offers a practical roadmap for business leaders navigating rapid technological change. In this post, I will share some of my key takeaways and draw on my experience helping Financial Services enterprises implement Data and AI solutions to illustrate what this journey looks like in practice.


## Table of Contents
- [Overview](#-overview)
- [What is the ai ladder](#what-is-the-ai-ladder)
    - [Collect: Building a Strong Data Foundation](#1-collect-building-a-strong-data-foundation)
    - [Organize: Making Data Usable Trusted and Accessible](#2-organize-making-data-usable-trusted-and-accessible)
    - [Analyze: Extracting Insight Through AI, ML](#3-analyze-extracting-insight-through-ai--ml)
    - [Infuse: Operationalizing AI Across the Business](#4-infuse-operationalizing-ai-across-the-business)
- [Conclusion](#-conclusion)
- [Additional Resources](#-additional-resources)

---

# What Is the AI Ladder?
The AI Ladder is a structured framework that navigates organizations from basic data collection to full-scale AI integration across four key stages: **collect**, **organize**, **analyze**, and **infuse**. 

The AI Ladder enables the transition from **+AI** to **AI+**: moving beyond using AI as a supplemental tool toward embedding it holistically across the enterprise. Instead of adding AI onto existing workflows, organizations begin re-imagining those business process with AI as a foundational capability.

---

# 1. Collect: Building a Strong Data Foundation

### What this stage is about
Collecting and storing high‑quality data from diverse sources so AI efforts have something reliable to work with.

In the highly regulated financial service industry, this foundational step carries additional weight: data may include Personally Identifiable Information (PII) or sensitive information and subject to strict audit, privacy, and security requirements.

### Common Issues to address
- Siloed data across databases and systems. 
    - Consider siloed legacy systems including Mainframes and other core banking platforms. 
- Inconsistent data formats. 
- Unstructured data under-utilized in the enterprise. 
    - Examples: 
        - Loan applications and forms PDFs. 
        - Bank statements. 
- Low quality data. 

### High Value Use Cases
1. Provide a Single Point of Access to data to break down data silos
    - Data may exist on various environments.
        - On-Premise
        - Cloud
        - Hybrid Cloud
    - Data may exist in various systems/technologies.
        - CRM (Customer Relationship Managements): including Salesforce, HubSpot, etc 
        - Customer Support/Service Platforms including Zendesk, ServiceNow, etc
        - Human Resources platforms including Workday, SAP, etc. 
        - Relational Databases including Oracle, MySQL, PostgreSQL, etc.
        - Data warehouses including Teradata, Snowflake, etc
        - Cloud Object Storage
    - Data may exist across different business units: Retail, Wealth, Risk, Treasury, Payments, Trading
    - A shared and open metadata layer providing a single pane of glass view to all of these data eliminate data silos, eliminating data duplication and ETL(Extract, Transform, Load)/ELT sprawl
    - Ensure front-office, middle-office, and back-office operate from the same governed data
2. Ensuring Interoperability
    - Supports open formats like Apache Iceberg to allow different analytics/AI engines to access data
    - Remove data vendor lock-in
    - Make cross-domain Data & AI more feasible
3. Workload-optimization:
    - Multi-engine architecture for price-performance
    - Medallion architecture: Bronze (Raw Data), Silver (Clean Data), Gold (Enriched/Business ready data) 
4. Unstructured Data:
    - Access, process, and deliver unstructured data to maximize return of investment (ROI) of enterprise data. 
    - Some examples: 
        - Use call center recordings to perform customer sentiment and satisfaction analysis
        - Entity extraction from contracts, loan documents, Know Your Customer (KYC) files to perform risk assessment


### IBM Data Platform Solutions:
- **watsonx.data**
- **watsonx.data integration**
- Comparable solutions:
    - AWS Glue
    - Ab Initio
    - Informatica PowerCenter

---


# 2. Organize: Making Data Usable, Trusted, and Accessible

### What this stage is about
Cleaning, structuring, governing, and centralizing data so it becomes a trusted asset.

In financial services, this step is integral for regulatory compliance, model risk management, operational resilience, and customer protection. This ensures data can be safely used for analytics, underwriting, fraud detection, risk modeling, and AI without creating regulatory or reputational exposure.

### Key Actions
- Cleansing data for accuracy
- Categorizing data for easy retrieval across business domains
- Implementing governance and security policies
- Data Productization for accessible analytics and AI

### High Value Use cases
- Regulatory Compliance for Financial Services:
    - **Basel Committee on Banking Supervision (BCBS 239**): Risk data aggregation and reporting
    - **General Data Protection Regulation (GDPR)**: Personal data management 
    - **Digital Operational Resilience Act (DORA)**: Information and technology risk management 
    - **Payment Card Industry Data Security Standard** (PCI DSS): Credit card protection requirements 
- Data Governance:
    - Data Classification: Identifying Personal Identifiable Information/Sensitive Information (Automatic Data Profiling and Data Classification)
    - Data Catalog: Mapping Business Terms to Data and generating Data Assets Names and Descriptions
    - Data Catalog: Industry Specific Business Glossary and Terms, Metadata management. 
- Data Lineage: 
    - Database migration: Large-scale data upgrade cycles
    - Impact Analysis/Root Cause Analysis: Understanding upstream data origin and downstream data impacts
- Data Quality: 
    - Data Class: Automatic Assignment of Data Classes, such as loan attributes, customer identity, transaction types
    - Data Quality Checks: Detecting inaccuracies, inconsistencies, or missing data - Capitalization Check, Completeness, Data Classes Check, Data Type Check, Format Check, Missing Values, Length Check, Range Check, Uniqueness Check
    - Service Level Agreement (SLA): Data Quality Remediation 
- Data Productization: 
    - Accessible data: Data Democratization and Data Mesh - decentralized data architecture that treats data as a product and shift responsibility from central teams to business domains, improving data reusability 

### IBM Data Platform Solutions: 
- **IBM watsonx.data Intelligence** 
- **IBM watsonx.governance**
- Comparable solutions: 
    - Atlan Data Catalog
    - Azure Data Catalog 
    - Credo AI


---

# 3. Analyze: Extracting Insight Through AI & ML

### What this stage is about
Using statistical analysis, machine learning, and artificial intelligence to derive patterns, predictions, and insights.

### Key Actions
- Discover Patterns by applying statistical analysis or ML algorithms
    - Use statistical modeling (regression, clustering, etc) to reveal relationships, correlations, and underlying trends
    - Use AI/Gen-AI to discover semantic patterns across documents or datasets
- Developing predictive/generative models
    - ML Model (decision trees, logistical regression, neural network) training 
    - Foundational Model tuning
- Insights and Visualization
    - Communicate findings via charts, tables, visualization or statistical summaries 
    - Utilizing Foundational Models for summaries, recommendations. 

### Financial Service Enterprises Usecases
- Risk and Compliance: 
    - Build statistical and predictive models to identify risk drivers, validate assumptions
    - Use foundational models to detect patterns in transaction/claims data and generate summarizations for regulatory documents and risk reports. 
- Fraud Detection and Financial Crime
    - Classification models to flag suspicious activities 
    - Generative AI to detect behavior patterns and summarize case files for financial analysts. 
- Customer Insights and Personalization
    - Predictive models to support market segmentation and churn prediction
    - LLM to generate personalized recommendation and extract insights from customer interactions
- Claims and Loss Forecasting
- Payment Pattern Analysis

### Notes
The Analyze stage is often where organizations see their first tangible AI wins: clear patterns discovered, accurate predictions generated, and actionable insights delivered to the business. But these wins only happen when earlier rungs of the AI Ladder—Collect (clean, integrated data) and Organize (trusted, governed data)—have been done correctly. 

### IBM Data Platform Solutions:
- **IBM SPSS**
- **watsonx.ai**
- Comparable solutions: 
    - Amazon Bedrock
    - DataRobot
    - Microsoft Azure Machine Learning 


---

# 4. Infuse: Operationalizing AI Across the Business

### What this stage is about
Making AI an active part of business operations, automating tasks, augmenting decision‑making, and embedding intelligence into applications.

This is where AI becomes operationalized and requires the strongest alignment across technology, risk, compliance, and business teams.

### Key Actions
- Orchestrating end-to-end workflows
- Automating high-volume and repetitive business processes
- Maintaining Governance, Observability and Regulatory Compliance
- Training employees to use AI responsibly and confidently

### Financial Service Enterprises Usecases
- Orchestrating workflows: 
    - KYC (Know Your Customer)/AML (anti-money laundering): Orchestrating workflows to gather/screen documents, update data system, and push fraud alerts
    - Claims processing: Gather evidence, check policy data, and route approval. 
- Automating business processes: 
    - Banking: daily account reconciliation, loan document ingestion
    - Insurance: Policy data updates, loss-run reporting
- "Prepare a claims summary for this policyholder and flag suspicious items" 
- "Update the loan file with the customer's new income document" 

### Project to Capability
**+AI** to **AI+**

At the earlier rungs of the AI Ladder (Collect -> Organize -> Analyze), AI typically exists as isolated experiments, proofs‑of‑concept, and pilot use cases. The Infuse stage changes that. This is where AI stops being something you add to a process (“+AI”) and becomes the core operating model of the enterprise (“AI+”).

### IBM Data Platform Solutions:
- **IBM RPA**  
- **watsonx.orchestrate**
- Comparable solutions:
    - Microsoft Power Automate 
    - UiPath Agentic Automation 
    - Salesforce Agentforce 

---

# Conclusion
The AI Ladder is effective because it gives enterprises a clear, structured path to adopting AI in a way that’s scalable, sustainable, and business-focused

As a Data & AI specialist, I’ve seen firsthand how organizations can unlock real operational transformation when they approach AI with the right foundation. 

---

# Additional Resources
- [An Example Applying the AI Ladder. Pt 2.](https://henry-xiao-hx.github.io/henry_xiao_blogs/2026/02/01/Understanding-The-AI-Ladder-Part-2.html)
- [The AI Ladder by Rob Thomas](https://www.oreilly.com/library/view/the-ai-ladder/9781492073420/)
- [IBM watsonx](https://www.ibm.com/products/watsonx)
- [IBM watsonx.data intelligence](https://www.ibm.com/solutions/data-intelligence)
- [IBM watsonx.data](https://www.ibm.com/products/watsonx-data)
