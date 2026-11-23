# ProSite — Cross-Repository Review & Consolidated Product Blueprint

**Review date:** 2026-11-23  
**Product owner:** M and S Construction Tech  
**Product:** ProSite  
**Commercial module:** QuoteFlow  
**Status:** Design and repository alignment review. This document does not claim that the ZIP source code has been extracted, audited, or tested.

## 1. Repositories reviewed

| Repository | Observed contents | Role in this review |
|---|---|---|
| [Prosite-Trial-Templates](https://github.com/MSNEXUS/Prosite-Trial-Templates) | README plus `ProSite-website By Freebuff.zip` (1,518,687 bytes) | Latest trial handoff supplied for review. ZIP binary could not be extracted through the connected GitHub text-file reader. |
| [prosite-concept](https://github.com/MSNEXUS/prosite-concept) | Product README, concept HTML, and several versioned ZIP archives | Earlier ProSite concept and prototype source. |
| [PRO-SITE](https://github.com/MSNEXUS/PRO-SITE) | Existing product README | Main ProSite destination repository; README already contains a comprehensive 26-section product brief. |
| [ICA-Template](https://github.com/MSNEXUS/ICA-Template) | Private ICA Fresh Build V1.1 README and design specifications | Cross-check reference for construction-office operational workflows and to prevent conflicting requirements. |

**Inspection limitation:** GitHub confirmed the trial ZIP's filename, size and blob SHA, but the available repository reader only returns UTF-8 text and cannot expose ZIP bytes. The concept repository also contains ZIP binaries which could not be extracted through that reader. The concept HTML and text specifications were readable and reviewed. Therefore this is a source-document and product-requirements cross-check, not a verified source-code audit of the ZIP.

## 2. Key findings

### Strong foundations to retain in ProSite
- Connected workflow from client and commercial enquiry through project delivery, site operations, costs, reporting and handover.
- QuoteFlow as an integrated commercial module, not a disconnected quotation generator.
- Project-centric records linked by stable IDs rather than duplicate disconnected copies.
- Daily logs, labour attendance, material movements, tasks, expenses and project reporting.
- Budget versus actual tracking with traceable source records and explicit anti-double-counting controls.
- Mobile-first site usability, responsive layouts, clear empty/error/loading states and functional controls.
- Versioned data, validation, data export/import and a documented path from a small prototype to a secure backend.

### Gaps and improvements required
1. **Separate prototype, MVP and production claims.** A visual prototype, browser-storage demo and secure multi-user SaaS are different delivery states. Each feature needs an implementation status and evidence.
2. **Make the project/site model explicit.** A project can have one or more sites; a site may be project-linked or independent where the business workflow permits. Daily logs, tasks, visits, materials and expenses must retain their own record identities and links.
3. **Make operational capture usable on site.** Prioritise fast mobile capture, clear date/time, project/site context, recorded-by attribution, drafts/failed-save states and later review. GPS, photo uploads, offline queueing and WhatsApp should remain clearly marked as future integrations unless implemented.
4. **Define progress semantics.** Keep stage status, milestone status, task completion and measured physical progress separate. Never average unrelated task percentages and present that as verified construction progress.
5. **Harden material stock accounting.** Treat deliveries, usage, returns and adjustments as distinct movements. Derive on-hand quantities from the movement ledger; preserve movement history and require a controlled adjustment when stock would go negative.
6. **Harden cost accounting.** Each cost must have a single authoritative source record. Linked labour/material costs must not be counted again as separate expenses. Keep estimated costs, approved budget, actual incurred cost, cash paid and commercial value distinct.
7. **Make commercial document lineage immutable.** An accepted quotation is a historical baseline. Revisions and change orders should create versioned changes rather than silently rewriting the accepted offer. Payment entries remain separate from invoices.
8. **Add auditability and recovery.** Track who changed what and when for commercially/financially important operations; validate imports; take backups before destructive migrations; test restore, not merely export.
9. **Strengthen tenancy and permissions before SaaS.** Company separation, server-side authorisation, project-level permissions, secure sessions and private file access must be enforced by the backend/database, never only by frontend visibility.
10. **Avoid premature AI and integrations.** GPS, WhatsApp, OCR, voice, AI forecasting and delay prediction need real integration design, privacy controls and success/failure handling. Do not display fake connected states.

## 3. Product scope — consolidated ProSite modules

The following is a recommended product structure for the main ProSite roadmap. It retains the product brief's functional breadth while clarifying domain boundaries.

### A. Business workspace
- Dashboard
- Business profile and company settings
- Team, roles and permissions
- Activity and audit history
- Notifications and reminders

### B. Clients and commercial pipeline — QuoteFlow
- Client and contact register
- Enquiries/leads (recommended addition; define before implementation)
- Estimates and estimate line items
- Quotations and quotation versions
- Accepted quotation conversion/link to a project
- Change orders and approval history
- Invoices and invoice line items (ProSite scope)
- Separate payment records and allocations
- Commercial statements and ageing reports

### C. Project delivery
- Project register and project workspace
- Configurable stages and milestones
- Project baseline and approved change history
- Project/site relationship management
- Project budget and cost categories
- Project status and schedule

### D. Site operations
- Site register and site profiles
- Daily site logs / site diary
- Work completed, blockers, delays and observations
- Site visits and inspection records
- Tasks, deadlines, priorities and assignments
- Labour attendance and workforce summaries
- Material inventory and stock movements
- Equipment/plant register (recommended, subject to validation)
- Site issue / snag / punch-list register (recommended, subject to validation)

### E. Financial control and reporting
- Project expenses and payment status
- Budget versus actual
- Cost commitments (recommended, subject to definition)
- Cost-to-complete forecast (future phase; define methodology)
- Cash paid and unpaid cost obligations kept distinct
- Project, site, material, labour and commercial reports

### F. Files and customer-facing outputs
- Project files, drawings and photos (requires secure file-storage design)
- Version history and access controls
- Print-ready site reports
- PDF/CSV exports
- Client-facing progress reports/portal (future phase; separate permission model)

These are proposed modules and extensions, not a claim that they are already implemented.

## 4. Core end-to-end workflows

### 4.1 Lead-to-project
Client/enquiry → estimate → quotation draft → quotation review and issue → accepted/rejected/expired outcome → project creation or linking.

Preserve the original quotation version and its acceptance state. Revisions create a new version or a linked change record; they do not overwrite the accepted baseline.

### 4.2 Project delivery
Project → stages and milestones → sites → daily logs / tasks / visits / attendance / material movements → review and issue resolution → project completion and handover.

Do not equate project stage, task status or physical progress. Each must have its own meaning and evidence.

### 4.3 Cost control
Approved budget baseline + approved budget changes → actual cost records → budget-to-actual variance → review and forecast.

Record cost events once. A material movement, attendance/payroll calculation or expense must not be duplicated in the project's actual-cost total. Define when a cost is incurred versus paid.

### 4.4 Invoice and collection
Commercial invoice → one or more linked payment records → payment allocation → outstanding amount and ageing.

Printing an invoice does not mark it paid. Partial payments are supported. Credit notes, cancellations, write-offs and overpayments require separately specified accounting rules.

## 5. Domain model and data-integrity rules

Use stable internal IDs and explicit foreign-key relationships. Human-readable codes are display identifiers, not database keys.

| Entity | Key relationships / invariant |
|---|---|
| Company | Owns clients, people, projects and company configuration in a multi-company deployment. |
| Client | Can have multiple contacts, enquiries, quotations and projects. |
| Contact | Belongs to a client; preserve history when deactivated. |
| Project | Belongs to a client; has stages, milestones, sites, budget, tasks and commercial links. |
| Site | May be independent or linked to a project; has logs, visits, tasks, labour and materials where relevant. |
| Project stage / milestone | Versioned configuration and project history; never overwrite completed history silently. |
| Estimate / estimate item | Commercial working calculation with quantities, units, rates and cost composition. |
| Quotation / quotation version | Client-facing offer; preserve issued/accepted version. |
| Change order | Project-linked request and decision; only approved changes affect the relevant baseline. |
| Invoice / invoice item | Commercial receivable document in ProSite; never conflate with payment. |
| Payment / allocation | Separate cash receipt record; one payment may be allocated according to explicit rules. |
| Budget / budget revision | Approved baseline plus separately recorded authorised changes. |
| Expense | One authoritative cost event with category, date, amount, payer/payee and optional project/site links. |
| Material / stock movement | Movement ledger; balance derived from opening quantity plus movements. |
| Worker / attendance record | Attendance event linked to worker, project/site and date; wage calculation policy remains configurable. |
| Task | Separate assigned work item; status and assignment history. |
| Site log | Dated report linked to a site and optionally project, author and stage. |
| Visit / inspection | Scheduled or completed event with purpose, attendees, outcome and related costs. |
| File / attachment | Metadata linked to owning entity; bytes stored in private controlled storage, not public URLs. |
| Audit event | Append-only record for security- and business-critical changes. |

### Financial invariants
- Estimate, quotation, approved contract/commercial value, project budget, actual costs, invoice value, payment received and outstanding amount are separate metrics.
- Actual cost totals are derived from authoritative cost sources, with explicit inclusion rules for materials, labour, subcontractors and expenses.
- Budget variance formula and sign convention must be consistent across all screens and reports.
- Every financial report states its date range, basis and source.
- Use integer minor currency units or a decimal-safe numeric type; never binary floating-point for money.
- Currency and tax treatment are configurable; statutory tax correctness must be reviewed for the jurisdiction before production use.

### Operational invariants
- Every daily log has a date, site context, author/recorder and clear status.
- Every stock movement has a type, quantity, unit, date, source and destination/project where applicable.
- Task completion does not automatically prove physical site progress.
- Historical assignments, stage changes, quotation versions, approved changes and financial corrections remain traceable.
- Failed saves, duplicate submissions and retry operations must not create duplicate business records.

## 6. Architecture recommendations

The existing ProSite README describes a React + TypeScript + Vite frontend, with local browser persistence for an initial prototype and PostgreSQL/Supabase as a future production direction. Retain that as a **prototype/roadmap distinction**, but do not market localStorage/IndexedDB as a secure multi-device or multi-user database.

Recommended target architecture when the product moves beyond a local prototype:
- React + TypeScript UI with a feature/module boundary.
- Backend API that owns validation, business rules and authorisation.
- PostgreSQL relational database with constraints and migration history.
- Object storage with private access controls for project files/photos.
- Server-side company and project scope enforcement.
- Durable audit events for important business actions.
- Automated backup and tested restoration.
- Background job/notification processing with idempotent retries.
- Observability that excludes secrets and unnecessary personal/business data.

Technology selection should be revalidated at implementation time; this document does not lock specific framework/library versions.

## 7. Design and usability standards

- Mobile-first for site capture; desktop-efficient for office and commercial work.
- Keep project/site context visible on every operational capture form.
- Provide fast create flows with progressive disclosure rather than exposing every field at once.
- Show save status, last saved time and unsaved changes.
- Support poor-connectivity workflows only after a reliable offline data-sync model is designed; do not imply offline sync already works.
- Use accessible labels, keyboard support, sufficient contrast and clear validation messages.
- Tables need useful filtering, sorting, pagination and mobile alternatives.
- Distinguish loading, empty, permission-denied, offline, validation and server-error states.
- Destructive actions need confirmation and relationship/dependency checks.

## 8. Security, privacy and trust

- Never rely on frontend route hiding as access control.
- Enforce company and project access in backend queries and database policies.
- Use secure password/session handling and role-based authorisation.
- Keep uploaded project files private; issue short-lived access links where appropriate.
- Validate file type and size; scan files where the deployment environment supports it.
- Protect exports, backups and bulk downloads.
- Log key events without logging passwords, tokens, recovery secrets or unnecessary personal data.
- Provide retention, export and deletion rules consistent with applicable obligations.
- Separate demo data from real customer records; label synthetic records prominently.
- Do not claim GPS verification, secure cloud storage, WhatsApp delivery, AI prediction or statutory compliance until implemented and independently tested.

## 9. Delivery priorities

### P0 — dependable foundation
- Data model and migrations
- Authentication and company isolation design
- Clients and projects
- Sites and project/site relationships
- Tasks, daily logs and milestones
- Data validation and audit history
- Backup, restore and test strategy
- Responsive design system

### P1 — core construction operations
- Material movement ledger
- Labour attendance
- Project expenses
- Budget versus actual
- Visit/inspection records
- Operational reports and exports

### P2 — QuoteFlow commercial workflow
- Estimate builder
- Versioned quotations
- Accepted quotation to project conversion
- Change orders
- Invoices
- Payment records and allocations
- Commercial reports

### P3 — advanced capabilities
- Client portal
- Secure document workflows
- Notifications and integrations
- GPS-assisted capture
- OCR and voice capture
- AI forecasting and advanced analytics

Priorities should be validated against pilot-customer feedback before they become delivery commitments.

## 10. Cross-check against ICA — Fresh Build V1.1

ICA is a separate offline-first internal construction-office system for Inked Corridors Atelier. ProSite is a broader SaaS product under M and S Construction Tech. Do not merge their identities, deployments, authentication, data or roadmaps.

| Topic | ProSite direction | ICA constraint to respect |
|---|---|---|
| Product purpose | Multi-company SaaS for construction SMEs | Private local office-management system for ICA |
| Deployment | Future hosted backend and multi-device access | Offline, portable, local loopback only; no cloud/LAN |
| Users | Company/team roles and possible client-facing access | GM is only business login; Developer separate technical role |
| Clients/projects/sites | Shared operational patterns are reusable | ICA's detailed client/project/site relationships and codes govern ICA |
| Finance | Full commercial, invoice and payment lifecycle | ICA excludes invoice issuance/management and GST calculations |
| Quotations/change orders | QuoteFlow commercial lifecycle | Not to be copied into ICA unless scope separately changes |
| Documents/files | Private project file storage as a ProSite design area | ICA excludes document repository and attachment storage |
| Materials/labour | Stock and attendance/cost workflows | ICA scope must follow its approved module/data specs; do not assume full inventory module |
| UI | ProSite-specific brand/design direction | ICA uses Midnight Mist visual baseline |
| Architecture | SaaS backend, tenant isolation and object storage | ICA's approved local encrypted architecture remains distinct |

### ICA cross-check outcomes
- Reusable concepts: linked client/project/site context, distinct tasks and visits, traceable visit participants/expenses, stage history, explicit financial metrics, stable IDs, controlled changes and clear UI states.
- Must not copy into ICA: cloud hosting, multi-company tenancy, client/worker portals, Supabase, public file storage, cloud sync, QuoteFlow, invoice workflows or GST.
- ICA's own approved scope and open decision register remain authoritative for ICA. This document is not an ICA scope change or approval.

## 11. Conflicts and decisions to resolve for ProSite

| ID | Question / issue | Recommended handling | Status |
|---|---|---|---|
| PS-01 | Is the initial build a clickable concept, local demo or production-ready MVP? | Name the release state explicitly and keep a feature-evidence matrix. | Open |
| PS-02 | Which customer/company roles are required at first pilot? | Define a minimum role matrix; never treat a frontend demo switcher as security. | Open |
| PS-03 | Does a site belong to exactly one project, multiple projects, or may be standalone? | Confirm cardinality and lifecycle before schema finalisation. | Open |
| PS-04 | Is site-level material stock shared across projects or project-specific? | Define warehouse/site/store model and transfer rules. | Open |
| PS-05 | How are labour attendance corrections approved? | Define who records, edits, approves and locks attendance periods. | Open |
| PS-06 | How are physical project progress and stage completion measured? | Define milestone evidence and a transparent percentage method. | Open |
| PS-07 | How are actual costs recognised versus paid? | Separate incurred, committed, paid and reimbursed amounts. | Open |
| PS-08 | What are the change-order approval and budget-baseline rules? | Preserve original baseline and version every approved adjustment. | Open |
| PS-09 | How should partial payments, advance payments, credit notes and overpayments work? | Define payment allocation, unallocated balances and correction flows. | Open |
| PS-10 | What are retention, backup, restore and deletion rules for files and records? | Define retention and test restore before production data. | Open |
| PS-11 | Which features are genuinely in the supplied trial ZIP? | Extract and inspect the archive, then map implemented routes, models and workflows to this blueprint. | Blocked — binary ZIP not inspected |
| PS-12 | What does offline use mean for field staff? | Specify read/write offline, queued sync, conflict policy and device identity separately. | Open |
| PS-13 | What is the required multi-company isolation model? | Tenant key, database policies, storage paths and cross-tenant tests required. | Open |
| PS-14 | Which customer-facing documents are in MVP? | Confirm estimates, quotations, invoices, reports and their permitted states independently. | Open |

## 12. Change log

### 2026-11-23 — Initial cross-repository review
- Inspected the supplied trial repository listing and README.
- Inspected the ProSite concept README and HTML concept page.
- Inspected the existing PRO-SITE README.
- Cross-checked relevant ICA README, client/project/site wireframes and tasks/visits/people wireframes.
- Identified product separation requirements and explicit ProSite-versus-ICA conflicts.
- Recorded unresolved ProSite decisions and ZIP source-inspection blocker.
- No production feature implementation or application test result is claimed.

## 13. Source references
- [Trial template repository](https://github.com/MSNEXUS/Prosite-Trial-Templates)
- [Trial ZIP](https://github.com/MSNEXUS/Prosite-Trial-Templates/blob/main/ProSite-website%20By%20Freebuff.zip)
- [ProSite concept repository](https://github.com/MSNEXUS/prosite-concept)
- [Concept HTML](https://github.com/MSNEXUS/prosite-concept/blob/main/ProSite%20%E2%80%94%20Construction%20&%20Commercial%20Management.html)
- [ProSite product repository](https://github.com/MSNEXUS/PRO-SITE)
- [ICA Template repository](https://github.com/MSNEXUS/ICA-Template)
- [ICA README](https://github.com/MSNEXUS/ICA-Template/blob/main/README.md)
- [ICA client/project/site wireframes](https://github.com/MSNEXUS/ICA-Template/blob/main/docs/clients-projects-site-operations-wireframes.md)
- [ICA tasks/visits/people wireframes](https://github.com/MSNEXUS/ICA-Template/blob/main/docs/tasks-visits-people-wireframes.md)
