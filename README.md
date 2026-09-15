# 🏢 ImoUni — Real Estate Management System

![Microsoft SQL Server](https://img.shields.io/badge/Database-MS%20SQL%20Server-CC292B?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)
![Data Modeling](https://img.shields.io/badge/Design-Conceptual%20%26%20Relational-blue?style=for-the-badge)
![Institution](https://img.shields.io/badge/IPCA-EST-005A36?style=for-the-badge)

A comprehensive relational database solution developed for **ImoUni**, a real estate mediation agency. Designed and implemented in **Microsoft SQL Server** as part of the *Data Storage and Access* (*Armazenamento e Acesso a Dados*) curriculum.

---

## 📑 Table of Contents
- [Project Overview](#-project-overview)
- [System Architecture & Data Modeling](#-system-architecture--data-modeling)
- [Key Business Rules](#-key-business-rules)
- [Business Workflow](#-business-workflow)
- [Database Schema & Data Volume](#-database-schema--data-volume)
- [Analytical SQL Queries](#-analytical-sql-queries)
- [Project Setup](#-project-setup)
- [Academic Information](#-academic-information)
- [Team Members](#-team-members)

---

## 📖 Project Overview

**ImoUni** aims to digitize and optimize its day-to-day real estate operations. This project provides a robust, normalized relational database engine designed to:

* **Centralize Operations:** Efficiently manage properties, clients, and real estate agents.
* **Streamline Contracts:** Log and track proposals, property acquisitions, and final transaction contracts (sales and leases).
* **Automate Financial Control:** Ensure transparent fee calculations and commission payout workflows.
* **Deliver Business Intelligence:** Provide analytical queries supporting management and strategic decision-making.

---

## 📐 System Architecture & Data Modeling

The database is built on relational database design principles and normalized up to the **Third Normal Form (3NF)** to eliminate redundancy and prevent update anomalies:

* **Conceptual Model:** Entity-Relationship diagram modeling core entities (`Agent`, `Client`, `Property`, `Proposal`, `Transaction`, `CommissionPayment`, `Document`).
* **Relational Model:** Normalized logical schema enforcing primary keys, foreign keys, and integrity constraints.

*(Tip: You can add your diagram image inside an `assets/` folder and link it here)*
