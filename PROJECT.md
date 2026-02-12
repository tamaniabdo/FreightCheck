PROJECT — Phase 1: User stories

Purpose
This file lists user stories for the Phase 1 scope. These stories describe who benefits and what they need. We will mark the MVP stories in a separate commit.

## User stories (8)
1. As an auditor, I want to upload a PDF or CSV invoice so that I can detect incorrect charges. "(MUST) "
2. As a system, I want to extract invoice_number and total_amount from uploaded invoices so that we can run checks automatically.
3. As a system, I want to store the original invoice encrypted-at-rest so that we have auditable evidence.
4. As an auditor, I want to link an invoice to a shipment manifest or quote so that we can compare expected vs billed charges.
5. As a system, I want to recompute expected charges using simple contract rules (weight, zone, fuel %) so that we have an independent price. " (MUST)"
6. As a reviewer, I want a UI that shows invoice, expected charge, and the difference so that I can decide to dispute or hold payment.
7. As a reviewer, I want to flag an invoice for dispute and attach supporting evidence so that AP or vendors can act.
8. As an analyst, I want basic metrics (number flagged, potential recovered $) so that we can measure impact and ROI.

Notes
- We will mark MVP stories (2 stories) in a following commit `docs(mvp)`.
- Acceptance criteria and NFRs are recorded separately (`docs(acceptance)` and `docs(nfr)`).

## Acceptance criteria (for MVP)

MVP - Upload & Parse (story #1 & #2)
- Given a logged-in user, when they upload a machine-generated PDF or CSV (≤10MB) to `/api/invoices/upload`, then the server accepts it, stores it encrypted, parses `invoice_number` and `total_amount`, and returns JSON `{ id, invoice_number, total_amount, parse_ms }`.  
- Parsing time (parse_ms) must be ≤ 3000 ms on a local dev machine for typical machine PDFs.

MVP - Recompute & Flag (story #5)
- Given an invoice record, when the system runs a basic contract rule (example: base rate × weight + fuel %) it produces `expected_amount`.  
- If `billed_amount - expected_amount >= $5` or `>= 5%` (whichever is larger), the system creates a flag record with `reason` and `difference`.  
- The flag appears in the reviewer UI list within 5 seconds of upload.




