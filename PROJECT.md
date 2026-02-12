# Phase 1 — User Stories and Requirements

## User Stories (8 total)

1. As an **auditor**, I want to **upload a PDF invoice** so that I can detect incorrect charges. (MUST)
2. As a **reviewer**, I want to **view parsed invoice fields** so that I can validate them. (MUST)
3. As a **system**, I want to **store invoices securely** so that they can be audited later. (NFR)
4. As an **auditor**, I want to **link an invoice to a contract** so that I can compare expected rates. (Nice)
5. As a **reviewer**, I want to **flag an invoice for dispute** so that AP can hold payment. (MUST)
6. As an **admin**, I want **logs for each upload** so I can debug issues. (NF)
7. As a **user**, I want to **download the original PDF** so I can send it to the carrier if needed. (Nice)
8. As an **analyst**, I want **basic metrics** (total recovered $, recovery rate) so I can measure impact. (Nice)

---

## MVP Stories

- **Story #1** → upload invoice and parse it  
- **Story #5** → flag discrepancies and generate dispute packets

---

## Acceptance Criteria (MVP)

**Story #1 — Upload Invoice**

- Accept PDF ≤ 10MB  
- Extract invoice_number, total_amount, carrier, and tracking_number  
- Store parsed data in MongoDB  
- Parsing completes ≤ 3s  
- Display uploaded invoice in reviewer UI  

**Story #5 — Flag Discrepancies**

- Compare invoice charge to contract + manifest  
- Calculate expected charge including surcharges  
- Flag invoices with mismatch > $5 or > 5%  
- Display flagged invoices with severity score in reviewer UI  
- Allow reviewer to generate dispute packet (PDF/email template)  

---

## Non-Functional Requirements (NFR)

- Files **encrypted-at-rest** (AES-256)  
- **Max upload size**: 10MB  
- **Parsing time per invoice** ≤ 3 seconds  
- **UI** protected with basic auth (dev)  
- **Logging**: user, timestamp, filename for each upload  
- **Database**: start with MongoDB local, scalable for production  
- **API responses**: JSON, consistent, with error handling  
- **Version control**: Git for all Phase 1 commits  
- **Extensibility**: architecture ready for later 3PL, marketplace, and ERP integrations  

---

## MERN Stack Notes

- MongoDB collections: `invoices`, `manifests`, `contracts`, `disputes`, `users`  
- Express routes:
  - `POST /uploadInvoice` → parse and store invoice  
  - `GET /flaggedInvoices` → retrieve invoices with mismatches  
  - `POST /generateDispute` → generate dispute PDF/email  
- React components:
  - `Uploader` → file upload form  
  - `InvoiceTable` → display parsed invoices  
  - `DisputeModal` → review and generate dispute  
- Node.js backend logic:
  - Compute expected charge (DIM weight, fuel surcharge, zone, etc.)  
  - Compare to invoice  
  - Generate severity score and store flagged invoices
