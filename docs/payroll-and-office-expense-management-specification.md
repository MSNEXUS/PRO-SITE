# ProSite — Payroll, Workforce Costs & Office Expense Management Specification

**Date:** 2026-11-23  
**Product:** ProSite / M and S Construction Tech  
**Status:** Proposed functional specification — business rules and implementation status require approval  
**Scope:** Salaried staff, site workforce costs, multiple offices/branches, recurring utilities and administrative overheads.

## 1. Objective

Add a connected Payroll and Business Expense Management capability to ProSite. It must support salary administration and payment tracking while separately managing office/branch overheads, utilities and other operating expenses.

The system must distinguish:
- Employee salary earned/accrued.
- Salary paid and outstanding.
- Employee advances and their settlement.
- Project/site labour cost.
- Office payroll and administrative overhead.
- Project-specific costs.
- Office/branch operating expenses.
- Expense incurred versus cash paid.

Do not merge these values into one undifferentiated expense total.

## 2. Navigation proposal

**People & Payroll**
- Employee Register
- Salary Profiles
- Attendance & Leave
- Payroll Periods
- Payroll Runs
- Salary Advances
- Payslips
- Payroll Reports

**Business Expenses**
- Expense Register
- Offices & Branches
- Expense Categories
- Recurring Expenses
- Bills & Due Dates
- Payments
- Expense Reports

The navigation names are proposals and may be refined during UX design.

## 3. Employee and salary records

### Employee profile
Suggested fields:
- Employee ID (stable internal ID plus human-readable code)
- Full name and contact details
- Employment type and department
- Designation
- Joining date and employment status
- Primary office/branch
- Default project/site assignment where applicable
- Bank/payment details with restricted access
- Payroll profile and effective date
- Notes and employment history

Avoid storing unnecessary sensitive personal information. Bank details, salary amounts and payslips must be permission-restricted.

### Salary profile
Support configurable components rather than hardcoding one salary formula:
- Basic salary
- Fixed allowances
- Variable allowances
- Incentives/bonus
- Overtime or additional duty (if enabled)
- Deductions
- Advances/loan instalments
- Employer-side cost components, if separately required

Each component needs a name, type, calculation basis, effective date, taxable/statutory treatment (only after professional review), and whether it affects gross pay, net pay or employer cost.

Historical salary profiles must be versioned. A salary revision must not rewrite previously finalised payroll periods.

## 4. Payroll cycle

Recommended workflow:

1. Configure payroll period and pay date.
2. Select eligible employees.
3. Pull approved attendance/leave inputs where configured.
4. Review salary profile effective for that period.
5. Calculate draft payroll.
6. Review employee-wise earnings, deductions and net pay.
7. Resolve missing or unusual inputs.
8. Finalise/approve payroll according to the company's role policy.
9. Record actual salary payments, including partial payments if permitted.
10. Generate payslips and payroll reports.
11. Lock the period while preserving correction history.

Payroll statuses should distinguish Draft, Calculated, Pending Review, Approved, Partially Paid, Paid, On Hold and Reversed/Corrected. Final status vocabulary requires UX and policy review.

### Calculation safeguards
- Show each component and calculation basis.
- Use decimal-safe money arithmetic.
- Define rounding policy before production.
- Do not silently assume unpaid leave, overtime, bonus, deductions or statutory contributions.
- Do not calculate Indian statutory deductions/contributions or claim compliance until reviewed by a qualified payroll/CA professional and configured for the employer's applicable obligations.
- Reconcile payroll payment totals to actual payment records.
- A payslip generated or printed does not prove salary was paid.
- Corrections to a finalised run must use a controlled adjustment/reversal workflow, preserving the original run.

## 5. Attendance, leave and workforce costs

Support attendance inputs only to the extent required by the employer's chosen payroll policy:
- Present/absent/half-day or other configured attendance statuses.
- Paid/unpaid leave categories.
- Approved overtime/additional duty, if applicable.
- Attendance correction history and approver.
- Period cut-off and late-entry handling.

Keep office employees and site workforce distinguishable. A worker's payroll cost may be allocated to a project/site, an office/branch, a shared overhead cost centre, or split by an explicitly recorded allocation. Allocation must not duplicate the same salary cost.

## 6. Salary advances and employee loans

Track separately:
- Advance requested/issued.
- Recipient and date.
- Original amount and outstanding balance.
- Repayment/deduction schedule, if approved.
- Actual deductions and repayments.
- Settlement/closure history.

An advance paid is not automatically a salary expense and must not be counted again when recovered through payroll. Loan principal, interest and payroll deductions require separately defined treatment.

## 7. Multiple offices and branches

Create an Office/Branch register:
- Office ID and name
- Address and contact details
- Office type/status
- Opening/active date
- Responsible manager (optional)
- Cost centre code
- Default currency (INR initially)
- Notes

Every office-related expense should be attributable to one office/branch or an explicitly shared/head-office cost centre. Historical records must retain the office/cost centre used at the time, even if the office is renamed or closed.

Do not assume a branch is a separate legal entity. Legal entity and tax registration are separate master data concepts if multi-entity support is later required.

## 8. Office expense register

Suggested categories:
- Rent and lease charges
- Electricity
- Water
- Internet and broadband
- Telephone/mobile
- Software and subscriptions
- Office stationery and printing
- Furniture and equipment
- Repairs and maintenance
- Cleaning and housekeeping
- Security
- Insurance
- Local travel and transport
- Professional and administrative services
- Bank/payment charges
- Other approved overheads

Each expense record should capture:
- Expense ID
- Office/branch/cost centre
- Category and subcategory
- Supplier/payee
- Description/purpose
- Bill/invoice reference (reference text only; file storage is a separate feature)
- Expense/invoice date
- Due date
- Period covered (for recurring utilities)
- Amount and currency
- Incurred date and paid date
- Payment method/account
- Paid amount and outstanding amount
- Recurrence link, if applicable
- Created by / reviewed by
- Optional project/site link where the expense is genuinely project-related

An expense incurred and a payment made are distinct events. Do not treat an unpaid bill as paid, and do not count its settlement as a second expense.

## 9. Recurring utilities and subscriptions

Allow recurring templates for monthly, quarterly, half-yearly and annual charges:
- Utility/service type
- Office/branch
- Supplier
- Expected amount or variable amount
- Billing cycle
- Expected bill date/due date
- Start/end dates
- Reminder lead time
- Active/paused/ended status

A recurring template should generate a due item or draft expense for review; it must not automatically post a paid expense or assume the bill amount. Variable bills such as electricity and water require actual bill amount entry and review.

Prevent duplicate bill generation for the same office, supplier, billing period and reference where identifiers are available.

## 10. Reporting

Required reports:
- Employee-wise monthly payroll register
- Payroll summary by department and office
- Salary paid versus outstanding
- Salary advances outstanding
- Payroll cost by project/site/cost centre
- Office-wise expense summary
- Category-wise and utility-wise expense summary
- Recurring expense due/overdue list
- Paid versus unpaid expense register
- Monthly overhead trend
- Annual office expense comparison
- Shared overhead allocation report

Every report must show its period, filters, currency, calculation basis and source records. Do not combine payroll, project costs and office overhead without clear labels.

## 11. Data model proposal

Potential entities:
- Employee
- EmploymentProfile
- SalaryProfileVersion
- SalaryComponentDefinition
- PayrollPeriod
- PayrollRun
- PayrollLine
- PayrollLineComponent
- AttendanceRecord
- LeaveRecord
- SalaryAdvance
- AdvanceRepayment
- Office
- CostCentre
- ExpenseCategory
- ExpenseRecord
- ExpensePayment
- RecurringExpenseTemplate
- RecurringExpenseOccurrence
- CostAllocation
- AuditEvent

Use stable IDs, effective dates, explicit relationships, unique constraints and history. Sensitive payroll fields require restricted access and must not be exposed through general project reports.

## 12. Permissions and privacy

ProSite is planned as a multi-user SaaS product. Payroll access must be separately permissioned:
- Business owner/admin: configure payroll and review all authorised payroll data.
- Payroll/HR role: manage employee payroll within assigned company scope.
- Finance role: process approved payroll payments and expense settlements without unnecessary access to private employee details.
- Project manager/site roles: see only the labour cost or workforce information needed for their work, not full salary details by default.
- Employee self-service/payslip access: future feature requiring secure authentication and tenant-scoped access.

All access must be enforced server-side and at the data layer. Frontend hiding is not security. Company/tenant isolation applies to every payroll and expense query.

## 13. Accounting and cost-control rules to define

Before production implementation, decide:
- Payroll accrual date and salary payable treatment.
- When payroll becomes final and who approves it.
- Salary payment, partial payment and reversal rules.
- Salary advance and loan accounting.
- Employer contributions and statutory deductions.
- Leave, overtime, bonus, incentive and arrears calculations.
- Payroll allocation across multiple projects/offices.
- Expense incurred date versus payment date.
- Recurring bill cut-off and duplicate detection.
- Shared office overhead allocation basis.
- Treatment of deposits, rent advances, refundable security deposits and capital purchases.
- Period closing and correction policy.
- Chart-of-accounts mapping and reporting basis.

No accounting journal mapping or statutory formula is finalised by this specification.

## 14. Implementation phases

**Phase 1 — Foundation**
- Office/branch and cost-centre register
- Expense categories and manual expense entry
- Due dates and payment status
- Employee register and salary profiles
- Basic monthly payroll draft and review

**Phase 2 — Payroll operations**
- Attendance/leave inputs
- Payroll run, finalisation and payslip
- Salary advances and settlements
- Payroll payment register
- Office-wise and employee-wise reports

**Phase 3 — Automation and control**
- Recurring utility templates and due reminders
- Partial payments and reconciliation
- Shared overhead allocations
- Audit and period-lock controls
- Advanced analytics and export

**Phase 4 — SaaS hardening**
- Server-side permission enforcement
- Tenant isolation tests
- Secure employee self-service if approved
- Backup, restore and privacy verification

## 15. Open decisions

| ID | Decision | Status |
|---|---|---|
| PAY-01 | Employee categories and employment types | Open |
| PAY-02 | Monthly payroll cut-off and pay date | Open |
| PAY-03 | Salary component catalogue and calculation rules | Open |
| PAY-04 | Attendance, leave and overtime treatment | Open |
| PAY-05 | Statutory deductions/contributions and professional review | Open |
| PAY-06 | Payroll approval roles and finalisation | Open |
| PAY-07 | Salary advance/loan deduction rules | Open |
| PAY-08 | Partial salary payment and correction rules | Open |
| PAY-09 | Office/branch structure and cost centres | Open |
| PAY-10 | Recurring expense cycles and bill verification | Open |
| PAY-11 | Shared overhead allocation method | Open |
| PAY-12 | Expense recognition versus payment and period close | Open |
| PAY-13 | Payroll and expense account mappings | Open |
| PAY-14 | Payroll data visibility, retention and employee self-service | Open |

## 16. Change log
- 2026-11-23: Initial proposed payroll, multi-office expense and recurring utilities specification. No implementation or statutory compliance is claimed.
