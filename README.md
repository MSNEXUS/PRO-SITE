# Build ProSite — Construction & Commercial Management SaaS

**Developed by:** M and S Construction Tech  
**Product:** ProSite  
**Commercial Module:** QuoteFlow  
**Target Market:** Indian construction SMEs, contractors, developers, architects, site engineers and project managers.

---

# 1. PRODUCT VISION AND OVERVIEW

Act as a senior full-stack developer, SaaS product architect, database architect and UI/UX designer.

Build a complete, responsive, mobile-first construction and commercial management web application named **ProSite**, developed by **M and S Construction Tech**.

ProSite is designed to digitise and streamline construction project management and commercial operations for small and medium-sized construction businesses in India.

It combines construction site management with an integrated commercial management module called **QuoteFlow**.

## The problem

Many Indian construction SMEs still depend on manual processes such as:

- WhatsApp groups for project communication.
- Paper-based daily site diaries.
- Excel spreadsheets for expenses and budgets.
- Manual labour attendance registers.
- Separate records for materials and procurement.
- Word documents and spreadsheets for quotations and invoices.
- Disconnected tools for client communication and progress reporting.

These practices create several problems:

- Poor accountability.
- Cost overruns.
- Delayed project completion.
- Inaccurate material and labour records.
- Difficulty tracking project profitability.
- Poor visibility into site progress.
- Delayed invoicing and payment collection.
- Difficulty maintaining organised project documentation.

## The proposed solution

ProSite will provide one connected platform to manage the complete construction business workflow.

It should cover:

- Construction projects and milestones.
- Clients and project teams.
- Estimates and quotations.
- Daily site progress logs.
- Labour attendance and payroll-related tracking.
- Material inventory and procurement.
- Tasks and deadlines.
- Project budgets and expenses.
- Invoices and payment records.
- Documents, drawings and photographs.
- Client-facing progress reports.
- Business analytics and project performance.

The product must be affordable, practical, easy to learn and suitable for businesses that do not have dedicated IT teams.

## Product philosophy

Prioritise:

1. Practical functionality over unnecessary complexity.
2. Accurate financial calculations.
3. Connected records rather than disconnected modules.
4. Mobile usability for people working on construction sites.
5. Clear accountability and traceability.
6. Affordable implementation for Indian SMEs.
7. A scalable architecture that can support multiple companies in the future.

The owner has basic coding knowledge and a zero-rupee starting budget. Prefer free and open-source technologies wherever practical.

---

# 2. PRODUCT OBJECTIVE AND CORE WORKFLOW

Create one connected system for managing the following:

- Construction projects.
- Clients and project teams.
- Estimates and quotations.
- Daily site logs and progress.
- Worker attendance and labour costs.
- Material inventory and usage.
- Tasks and deadlines.
- Budgets and expenses.
- Invoices and payment records.
- Reports and project documentation.

The key business workflow is:

**Client → Estimate → Quotation → Accepted Quotation → Project → Site Operations → Actual Expenses → Invoice and Payment Records → Project Reports → Handover**

Every related record must be connected through stable project IDs, customer IDs and other appropriate relationships.

Do not create disconnected copies of the same records.

For example:

- A quotation must belong to a client.
- An accepted quotation can create or link to a project.
- A project must contain its site operations and financial records.
- Material usage should be traceable to the relevant project.
- Expenses should be linked to the relevant project and category.
- Invoices should be linked to clients and, where applicable, projects.
- Payments should be recorded separately and linked to invoices.
- Reports should calculate their information from actual stored records.

---

# 3. TECHNOLOGY AND ARCHITECTURE

Use the following technology stack for the initial MVP:

| Layer | Technology |
|---|---|
| Frontend | React |
| Language | TypeScript |
| Build tool | Vite |
| Styling | Tailwind CSS |
| Navigation | React Router |
| Icons | Lucide React |
| Charts | Recharts or a suitable maintained alternative |
| PDF | Browser printing or a maintained PDF library |
| Initial database | localStorage |
| Larger local files | IndexedDB |
| Future database | PostgreSQL |
| Future authentication | Supabase Auth |
| Future storage | Supabase Storage or compatible private storage |
| Future hosting | Suitable free-tier hosting |

## Architecture requirements

- Use a modular frontend architecture.
- Separate UI components, business logic, validation and data services.
- Create a repository/data-service layer so browser storage can eventually be replaced with PostgreSQL without rewriting the entire UI.
- Use reusable components and consistent design tokens.
- Implement a versioned local data schema.
- Use stable unique IDs.
- Use timestamps and preserve relationships.
- Handle missing, malformed or outdated data safely.
- Use decimal-safe monetary calculations.
- Avoid hardcoding dashboard metrics after records begin being created.

## Initial prototype limitations

The first version may use local browser storage.

However:

- Do not pretend that local records are securely shared between devices.
- Do not implement fake authentication.
- Do not claim that browser storage is a cloud database.
- Clearly explain that clearing browser data may cause data loss.
- Provide JSON export and validated import.
- Provide a clear future migration path to Supabase and PostgreSQL.

## Future production architecture

Plan for:

- Supabase authentication.
- PostgreSQL database.
- Row-level security.
- Company-level and project-level access control.
- Private cloud file storage.
- Secure backend validation.
- Database constraints.
- Backups and recovery.
- Multi-device synchronisation.

Do not require paid APIs for the initial version.

---

# 4. DESIGN SYSTEM AND USER EXPERIENCE

Create a premium, professional construction SaaS interface inspired by the clarity and usability of Monday.com and Asana, without copying their branding.

## Brand colours

| Element | Colour |
|---|---|
| Primary blue | #2196F3 |
| Success green | #4CAF50 |
| Warning orange | #FF9800 |
| Background | #F5F7FA |
| Main text | #333333 |
| Borders | #E0E0E0 |

Use Poppins-style headings and Inter-style body text.

## Interface requirements

Provide:

- Collapsible desktop sidebar.
- Mobile-first navigation.
- Responsive tables and cards.
- Search, sorting and filtering.
- Status badges.
- Accessible forms.
- Dark mode.
- Loading states.
- Empty states.
- Error states.
- Clear confirmation dialogs for destructive actions.
- Consistent spacing, typography and icons.
- Useful tooltips and form validation.
- Responsive layouts for desktop, tablet and mobile.

The interface must look and feel like a professional SaaS product, not a static dashboard template.

Every visible navigation item and button must perform its advertised action.

---

# 5. MAIN NAVIGATION AND MODULES

Create the following main navigation:

1. Dashboard
2. Projects
3. Clients

**QuoteFlow — Commercial Management**
- Estimates
- Quotations
- Invoices
- Change Orders
- Payment Records

**Site Operations**
- Daily Logs
- Tasks
- Attendance
- Materials

**Financial Management**
- Budgets
- Expenses
- Cost Variance

**Workspace**
- Documents & Photos
- Reports
- Team & Permissions
- Settings
- Business Profile

Each section must have its own functional interface and connect to the shared data model.

---

# 6. DASHBOARD

Create a central business dashboard.

Display:

## Project overview
- Active projects.
- Completed projects.
- Overdue projects.
- Overall project progress.
- Upcoming milestones.
- Projects requiring attention.

## Financial overview
- Total approved project budgets.
- Estimated contract value.
- Actual recorded project costs.
- Budget remaining.
- Over-budget projects.
- Total invoices issued.
- Outstanding invoice balances.
- Payments received.

## Site operations
- Recent site logs.
- Pending tasks.
- Low-stock materials.
- Attendance summary.
- Upcoming deadlines.

## Additional functionality
- Project search.
- Date filters.
- Status filters.
- Quick actions.
- Recent activity.
- Recent documents.

All metrics must be calculated from actual records.

Clearly distinguish:

- Estimated contract value.
- Approved project budget.
- Recorded expenses.
- Invoices issued.
- Payments received.
- Outstanding balances.

Never treat these financial values as interchangeable.

---

# 7. PROJECT MANAGEMENT

Create a complete project management module.

## Project fields

- Project name.
- Unique project ID.
- Client.
- Site address.
- Start date.
- Target completion date.
- Contract value.
- Approved baseline budget.
- Current status.
- Construction stage.
- Progress percentage.
- Project manager.
- Description.
- Notes.

## Functionality

Support:

- Creating projects.
- Editing projects.
- Viewing project details.
- Archiving projects.
- Searching projects.
- Filtering by status, client and manager.
- Viewing linked quotations.
- Viewing project expenses.
- Viewing site logs.
- Viewing tasks.
- Viewing materials.
- Viewing documents.
- Viewing financial performance.

Each project should have a dedicated project workspace containing all related information.

---

# 8. CLIENT MANAGEMENT

Create a client database.

## Client fields

- Individual or business name.
- Client type.
- Contact person.
- Phone.
- Email.
- Address.
- Optional GSTIN.
- Notes.
- Linked projects.
- Linked estimates.
- Linked quotations.
- Linked invoices.

Support creating, editing, searching, viewing and archiving clients.

Preserve relationships when client records are edited.

---

# 9. QUOTEFLOW — COMMERCIAL MANAGEMENT

QuoteFlow is an integrated commercial management module within ProSite.

It should manage the commercial lifecycle from initial estimation to quotation, approval, change orders, invoicing and payment records.

## 9.1 Estimates

Allow users to create detailed project estimates.

Support:

- Estimate number.
- Client.
- Optional project.
- Estimate date.
- Description.
- Construction work items.
- Materials.
- Labour.
- Machinery.
- Transport.
- Other costs.
- Quantities.
- Units.
- Rates.
- Line totals.
- Discounts.
- Optional tax.
- Notes.
- Terms and conditions.

Automatically calculate line totals and document totals.

## 9.2 Quotations

Allow users to convert estimates into quotations.

Features:

- Unique quotation numbers.
- Professional A4 layout.
- Company branding.
- Client details.
- Itemised work descriptions.
- Quantities and units.
- Rates and totals.
- Configurable discounts.
- Configurable tax.
- Payment terms.
- Validity dates.
- Terms and conditions.
- Save, edit and duplicate.
- Preview and print.
- PDF output.

Quotation statuses:

- Draft.
- Sent.
- Accepted.
- Rejected.
- Expired.

Allow an accepted quotation to be converted into a project
