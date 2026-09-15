# 📘 Practical Project — Data Storage and Access (AAD)
**Academic Year:** 2025/2026 – 1st Semester  
**Course:** Bachelor's Degree in Computer Systems Engineering (LESI)  
**School:** School of Technology (EST) — IPCA (Polytechnic Institute of Cávado and Ave)  
**Instructor:** Prof. Hélder Quintela  

---

## 🏢 ImoUni Real Estate Management System

### 1. Introduction
**ImoUni** is a real estate brokerage company that aims to computerize its internal operations regarding:

- Property portfolio management
- Clients (Owners, Buyers, and Tenants)
- Real estate agents
- Contract execution and transaction records

The objective of this project is to develop a robust **relational database** using **Microsoft SQL Server**, applying core principles of data storage, normalization, referential integrity, and SQL analytics to support operational processes and executive decision-making.

---

### 2. Objectives

#### General Objectives
Design and implement a relational database that:
- Centralizes information about properties, clients, and agents.
- Facilitates the logging, monitoring, and execution of contracts and proposals.
- Enhances efficiency in tracking commission splits and payment installments.
- Enables the generation of analytical reports and commercial performance KPIs.

#### Specific Business Objectives

##### 🏠 Property Management
For each property:
- Unique ID (`id_imovel`)
- Type (Apartment, House, Land, Commercial unit, etc.)
- Location details (Address, City/Municipality, District, Postal Code)
- Technical features (Number of bedrooms, area, bathrooms, garage, swimming pool, etc.)
- Operational status (Available, Reserved, Sold, Rented, Maintenance)
- Asking price or monthly rental value
- Date of entry into portfolio (`datacarteira`)
- Association with a designated real estate agent and a legal owner

##### 👤 Client Management
Client categories:
- **Owners** (Sellers / Landlords)
- **Buyers / Tenants**

Required records:
- Name, Tax Identification Number (NIF), phone contact, email, fiscal address
- History of submitted offers, acquired properties, or lease agreements
- Purchasing preferences and search criteria (optional)
- *Note:* A client can act simultaneously as an owner of one property and a buyer/tenant of another without record duplication.

##### 🧑‍💼 Real Estate Agent Management
- Name, Tax Identification Number (NIF), phone contact
- Contracted commission percentage rate
- Historical record of contracts managed
- Total revenue and commission volume generated

##### 📄 Contract Management
- Transaction type (Sale or Lease)
- Contract execution date (`datacontrato`)
- Agreed transaction amount or monthly rent
- Duration (applicable to lease agreements)
- Transacted property identifier
- Associated buyer/tenant and property owner
- Responsible agent

##### 💰 Payments and Commissions
- Log of payment installments (Date, amount, payment method)
- Direct link to the contract/transaction
- Dynamic calculation of agent commissions:
  $$\text{Commission} = \text{Transaction Value} \times \text{Agent Commission Rate}$$
- Financial reconciliation and monthly/annual cash-flow tracking

##### 📊 Analytical Queries and Reports
- Properties available filtered by municipality and price range
- Active lease contracts with recurring monthly rent schedules
- Sales performance and commissions earned per agent per month
- Identification of recurring investor clients
- Geographic distribution and total valuation of properties under management

---

### 3. 🔁 Business Workflow

```
[1. Property Listing] ➔ [2. Portfolio Management] ➔ [3. Client Servicing]
          │
          ▼
[4. Offers & Negotiation] ➔ [5. Contract Execution] ➔ [6. Payments & Commissions]
          │
          ▼
[7. Business Intelligence & Reporting]
```

1. **Stage 1 — Property Listing:** The real estate agent registers the property specifications, its legal owner, and portfolio listing date.
2. **Stage 2 — Marketing & Portfolio Management:** Continuous maintenance of listing availability and multidimensional querying (by type, municipality, price, area).
3. **Stage 3 — Client Servicing & Profiling:** Registration of prospective buyers/tenants and their specific property preferences (budget boundaries, minimum rooms, locations).
4. **Stage 4 — Offers & Negotiation:** Formal logging of buyer proposals (value, date, status). Accepted proposals transition into formal contracts.
5. **Stage 5 — Contract Execution:** Formalization of the sale or lease agreement, recording parties, financial terms, and updating property status.
6. **Stage 6 — Payments & Commission Settlement:** Recording payment receipts (single-sum or installments) and dynamic commission calculations.
7. **Stage 7 — Strategic Reporting:** Multi-table analytical queries monitoring commercial health, negotiation variances, and time-on-market metrics.

---

### 4. 📐 Database Modeling and Architecture

The database was designed following standard relational database principles, normalized up to the **Third Normal Form (3NF)**:

* **Conceptual Model:** Abstract representation of domain entities (`Agente`, `Cliente`, `Imovel`, `Proposta`, `Transacao`, `PagamentoComissao`, `Documentos`, `Preferencias`).
* **Relational Model:** Schema mapping with strictly defined Primary Keys (`PK`) and Foreign Keys (`FK`), enforcing referential integrity across operational tables.

#### Business Rules Implemented at Database Level (Constraints)
- **Multi-Role Clients:** Normalized so that a single record in `Cliente` can be referenced as an owner or a buyer/tenant across different transactions.
- **Preventing Self-Dealing:** Restricts a client from being both the buyer and seller in the same transaction:
  ```sql
  CHECK (id_comprador_arrendatario <> id_vendedor_arrendador)
  ```
- **Commission Rate Integrity:** Constrains agent commission percentage within the interval $[0, 1]$:
  ```sql
  CHECK (percentagemcomissao BETWEEN 0 AND 1)
  ```
- **Temporal Consistency:** Ensures contract completion dates occur chronologically after start dates:
  ```sql
  CHECK (data_fim IS NULL OR data_fim > data_inicio)
  ```
- **Property Attribute Bounds:** Guarantees non-negative integer values for structural attributes and positive areas:
  ```sql
  CHECK (nquartos >= 0)
  CHECK (nwc >= 0)
  CHECK (area > 0)
  ```
- **Geographic Validation:** Restricts coordinate attributes to valid real-world bounds:
  ```sql
  CHECK (latitude BETWEEN -90 AND 90)
  CHECK (longitude BETWEEN -180 AND 180)
  ```
- **Uniqueness Guarantees:** Strict `UNIQUE` constraints applied to tax numbers (`nifcliente`, `nifagente`), property registry IDs (`matriz`), and postal codes (`codigopostal`).

---

### 5. 🗄️ Database Volume & Seeded Data

To validate relational constraints, index performance, and complex queries, synthetic data was generated and populated across all operational entities:

| Table | Description | Record Count |
| :--- | :--- | :---: |
| `Distrito` | Administrative districts | 20 |
| `Concelho` | Municipalities | 16 |
| `CodigoPostal` | Postal code entities | 13 |
| `Estado` | Operational status of properties | 4 |
| `TipoImovel` | Property typology categories | 7 |
| `Modalidade` | Business operational modes (Sale / Rent) | 2 |
| `TipoDocumento` | Identification and legal document types | 4 |
| `TipoTransacao` | Transaction nature | 2 |
| `MetodoPagamento` | Accepted financial payment channels | 3 |
| `EstadoProposta` | Workflow states for proposals | 3 |
| `Agente` | Real estate commercial agents | 15 |
| `Cliente` | Client master records | 40 |
| `CatalogoImovel` | Property catalog entries | 100 |
| `Proposta` | Formal property purchase/lease proposals | 20 |
| `Transacao` | Executed transaction contracts | 17 |
| `PagamentoComissao`| Commission settlement receipts | 17 |
| `Documentos` | Legal documents linked to transactions | 17 |
| `Preferencias` | Client criteria and matching requirements | 10 |

---

### 6. 📊 Analytical SQL Queries

A suite of analytical queries was implemented to extract insights from the data:

| # | Business Question / Requirement | Query Focus | SQL Complexity & Techniques |
| :-: | :--- | :--- | :--- |
| **1** | Which agents generate the highest revenue for ImoUni? | Agent ranking by total commission value | `SUM`, `GROUP BY`, temporal filters |
| **2** | Is the agency collecting commissions correctly? | Theoretical vs. collected commissions reconciliation | `LEFT JOIN`, `ISNULL`, `CASE`, `ROUND` |
| **3** | Which property types take longest to sell? | Average time between listing and transaction | `DATEDIFF`, `GROUP BY`, `IS NOT NULL` |
| **4** | Which properties are stagnant without market demand? | Active properties listed for $> X$ days with no offers | `NOT EXISTS`, `DATEDIFF`, `DECLARE`, `GETDATE` |
| **5** | How many proposals convert into closed transactions? | Proposal-to-transaction conversion rate | `COUNT`, `RIGHT JOIN`, ratio arithmetic |
| **6** | Which agents negotiate closest to the asking price? | Average spread between listing price and closing price | `JOIN`, `AVG`, percentage calculations |
| **7** | Which clients are recurring investors? | Clients with multiple purchase transactions | `GROUP BY`, `HAVING`, `COUNT(DISTINCT)` |
| **8** | Which clients accumulate multiple roles in the system? | Identification of cross-role clients (owners & buyers) | Multi-table `JOIN`, `DISTINCT`, `HAVING`, `COUNT` |
| **9** | How is business volume trending over time? | Monthly transaction volume grouped by transaction type | `GROUP BY`, temporal aggregation functions |
| **10** | Where is ImoUni's portfolio geographically concentrated? | Total managed property valuation grouped by municipality | `JOIN`, `SUM`, `NOT EXISTS`, active status filters |
| **11** | Which active properties match client preferences? | Automated matching of preferences against available stock | `CTE`, `ROW_NUMBER() OVER (PARTITION BY)`, dynamic filters |
| **12** | Are there overdue or unpaid commissions? | Variance analysis between owed and settled commission fees | `HAVING`, `DATEDIFF`, `LEFT JOIN`, `ISNULL`, `GETDATE` |

---

### 7. 🛠️ Technical Requirements & Deliverables

- **Conceptual & Logical Modeling:** Entity-Relationship diagrams and normalized relational schema.
- **SQL Server Implementation:** DDL scripts, constraints, referential integrity, and seed data.
- **Analytical Queries:** Set of advanced business intelligence queries.
- **Academic Report:** Full project documentation covering architecture, constraints, queries, and project conclusions.

---

### 8. 📏 Grading Criteria

| Criterion | Weight |
| :--- | :---: |
| Conceptual and logical modeling | 30% |
| SQL implementation and constraints | 30% |
| SQL queries (business relevance and technical correctness) | 20% |
| Report structure, clarity, and analysis | 20% |

---

## 👥 Project Contributors

- **Institution:** Instituto Politécnico do Cávado e do Ave (IPCA)  
- **School:** Escola Superior de Tecnologia (EST)  
- **Course Unit:** Armazenamento e Acesso a Dados (AAD) — Academic Year 2025/2026  
- **Supervisor:** Prof. Hélder Quintela  

**Working Group:**
* **Ricardo Gomes Pinto** — Nº 14865
* **Daniel Alves Batista** — Nº 20472
* **Marina Maria Maia Silva** — Nº 31029
* **Nuno Miguel Fernandes Oliveira** — Nº 31550
