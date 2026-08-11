# Shared — SAP FICO MCQs
### Applies to: JD3 (SAP Finance Data), JD1 (ERP Transformation) — SAP FI & CO modules in depth
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 What does "FICO" stand for in SAP?
- A) Finance and Internal Controls Operations
- B) Financial Accounting (FI) and Controlling (CO) — the two tightly integrated core finance modules in SAP
- C) Financial Integration and Configuration Operations
- D) Fiscal and Cost Operations

**Answer: B**

---

**Q2.** 🟢 What is the primary purpose of SAP FI (Financial Accounting)?
- A) Internal cost management and profitability analysis
- B) External financial reporting — recording all business transactions and producing financial statements (balance sheet, P&L) that satisfy legal and reporting requirements
- C) Managing vendor master data only
- D) Tracking employee expense claims

**Answer: B**

---

**Q3.** 🟢 What is the primary purpose of SAP CO (Controlling)?
- A) Controlling user access to SAP
- B) Internal management accounting — tracking costs and revenues for internal decision-making, profitability analysis, budgeting, and cost center management
- C) Controlling financial postings for compliance
- D) Managing external audit processes

**Answer: B**

---

**Q4.** 🟢 What is "Accounts Payable" (AP) in SAP FI?
- A) A module tracking money owed to the company by customers
- B) The sub-module managing a company's obligations to vendors — recording vendor invoices, processing payments, and maintaining vendor master data
- C) A module for tracking fixed assets
- D) A module for managing employee expenses

**Answer: B**

---

**Q5.** 🟢 What is "Accounts Receivable" (AR) in SAP FI?
- A) A module managing money the company owes to vendors
- B) The sub-module managing amounts owed to the company by customers — recording customer invoices, tracking collections, and managing customer master data
- C) A module for managing bank accounts
- D) A module for recording payroll transactions

**Answer: B**

---

**Q6.** 🟢 What is "Asset Accounting" (AA) in SAP FI?
- A) A module for managing customer assets (receivables)
- B) The sub-module managing the company's fixed assets — tracking acquisition, depreciation, transfers, and retirement of capital assets like machinery and buildings
- C) A module for tracking financial investments
- D) A module managing intangible assets like patents only

**Answer: B**

---

**Q7.** 🟢 What is a "cost center" in SAP CO?
- A) A physical office location with high operating costs
- B) An organizational unit within CO used to collect and monitor costs — representing a department, function, or location (e.g., IT department, Marketing) responsible for its costs
- C) A vendor with high invoice volumes
- D) A financial account for recording operating costs

**Answer: B**

---

**Q8.** 🟢 What is a "profit center" in SAP CO?
- A) The most profitable product in a company's portfolio
- B) An organizational unit within CO representing a business segment responsible for both revenues and costs — used for internal P&L management and segment reporting
- C) A G/L account for recording profits
- D) A cost center that generates revenue

**Answer: B**

---

**Q9.** 🟡 What is "document splitting" in SAP S/4HANA FI?
- A) Splitting large SAP documents into multiple files for archiving
- B) A feature that automatically splits accounting document line items by defined characteristics (e.g., profit center, segment) — enabling zero-balance financial statements at a more granular level for segment reporting
- C) Dividing a vendor invoice across multiple payment runs
- D) Splitting GL postings between two company codes

**Answer: B**

---

**Q10.** 🟡 What is the "Universal Journal" (ACDOCA) in SAP S/4HANA?
- A) A user manual for SAP FI transactions
- B) A single, unified accounting table in S/4HANA that combines postings from FI, CO, AA, ML, and PS into one source of truth — eliminating reconciliation between FI and CO and enabling real-time reporting
- C) A journal that stores all SAP system logs
- D) A ledger for recording universal (global) currency transactions

**Answer: B**

---

**Q11.** 🟡 What is "cost element accounting" in SAP CO?
- A) Accounting for the cost of SAP licenses
- B) The component of CO that classifies costs using cost elements (primary: derived from P&L G/L accounts; secondary: used only in CO for allocations) — forming the basis for cost analysis
- C) Managing the cost of raw materials in production
- D) Tracking the cost of individual sales transactions

**Answer: B**

---

**Q12.** 🟡 What is "internal order" in SAP CO?
- A) A purchase order created internally without vendor involvement
- B) A CO object used to collect and monitor costs for a specific task, project, or event (e.g., a marketing campaign, office renovation) — settled to cost centers or assets at period-end
- C) An inter-company purchase order
- D) A workflow approval for internal payments

**Answer: B**

---

**Q13.** 🟡 What is "product costing" in SAP CO?
- A) Calculating the retail price of products for customers
- B) A CO component that calculates the standard cost of manufactured products — including material, labor, and overhead — used for inventory valuation and profitability analysis
- C) Tracking the cost of SAP software products
- D) Managing vendor pricing for purchased goods

**Answer: B**

---

**Q14.** 🟡 What is "Profitability Analysis" (CO-PA) in SAP CO?
- A) Analyzing the profitability of the SAP implementation project
- B) A CO component that analyzes market segment profitability (by customer, product, region, sales channel) by combining revenue and cost data — used for strategic decision-making
- C) A financial statement showing overall company profit
- D) A tool for analyzing vendor contract profitability

**Answer: B**

---

**Q15.** 🟡 What is "allocation" in SAP CO?
- A) Allocating SAP user licenses to departments
- B) The process of distributing costs from sender objects (cost centers) to receiver objects (other cost centers, orders, profit centers) — e.g., allocating IT department costs to business units that use IT services
- C) Allocating budget to capital expenditure projects
- D) Distributing vendor invoices across multiple cost centers

**Answer: B**

---

**Q16.** 🟡 What is the difference between "distribution" and "assessment" in SAP CO cost allocation?
- A) They are identical allocation methods
- B) Distribution transfers costs keeping the original primary cost elements visible in the receiver; assessment transfers costs using a secondary assessment cost element — summarizing the sender's costs into one line
- C) Distribution is for cost centers; assessment is for profit centers
- D) Assessment transfers at actual cost; distribution transfers at standard cost

**Answer: B**

---

**Q17.** 🟡 What is "budgeting" vs. "planning" in SAP CO?
- A) They are the same — SAP uses the terms interchangeably
- B) Planning sets expected cost/revenue values for analysis (often flexible); budgeting sets an authorized spending limit with availability control that blocks or warns when exceeded
- C) Budgeting is done in FI; planning is done in CO
- D) Planning is for future periods; budgeting is for historical comparison

**Answer: B**

---

**Q18.** 🟡 What is "payment terms" in SAP AP/AR?
- A) The terms of a licensing agreement with SAP
- B) Configuration that defines when a payment is due and any discount conditions (e.g., "Net 30" — full payment due in 30 days; "2/10 Net 30" — 2% discount if paid within 10 days)
- C) A condition specifying the payment method (check, wire, ACH)
- D) The terms governing which users can approve payments

**Answer: B**

---

**Q19.** 🟡 What is an "automatic payment run" (F110) in SAP AP?
- A) A report that automatically generates vendor invoices
- B) An SAP program that automatically selects open vendor invoices due for payment based on payment terms and due dates, creates payment documents, and generates payment files for the bank
- C) An automated process for clearing customer receipts
- D) A batch job that posts accrual entries automatically

**Answer: B**

---

**Q20.** 🟡 What is "tax code" in SAP FI?
- A) A code used to encrypt financial data
- B) A configuration object in SAP that defines the tax type, rate, and G/L account determination for tax-relevant transactions (e.g., input/output VAT) — applied to purchasing and sales documents
- C) A legal tax identification number for the company
- D) A code for SAP financial reporting categories

**Answer: B**

---

**Q21.** 🟡 What is "withholding tax" in SAP FI?
- A) Taxes withheld from employee salaries (handled in HR/Payroll)
- B) A tax deducted at source from certain vendor payments (e.g., professional services) and remitted directly to the tax authority — configured in SAP for automatic calculation and reporting
- C) A temporary tax hold on financial transactions
- D) VAT withheld until a vendor invoice is received

**Answer: B**

---

**Q22.** 🟡 What is "depreciation" in SAP Asset Accounting?
- A) The decrease in market value of a company's stock
- B) The systematic allocation of a fixed asset's cost over its useful life — calculated by SAP AA using depreciation keys (e.g., straight-line, declining balance) and posted to the G/L periodically
- C) Writing off a customer's uncollectible invoice
- D) The reduction in SAP license value over time

**Answer: B**

---

**Q23.** 🟡 What is a "depreciation area" in SAP Asset Accounting?
- A) A physical area of a factory where assets are stored
- B) A parallel valuation view for an asset — enabling different depreciation methods for different purposes (e.g., book depreciation for IFRS, tax depreciation for local tax reporting) simultaneously
- C) A cost center that accumulates depreciation charges
- D) An area in the system where depreciation runs are scheduled

**Answer: B**

---

**Q24.** 🔴 What is "new Asset Accounting" (new AA) in SAP S/4HANA?
- A) A new UI for asset management in Fiori
- B) The redesigned Asset Accounting in S/4HANA that posts depreciation and asset values directly in the Universal Journal (ACDOCA) in real time — eliminating the need for periodic depreciation runs to reconcile FI and AA
- C) A new module replacing Fixed Assets Management
- D) An upgrade to the classic AA module with no architectural changes

**Answer: B**

---

**Q25.** 🔴 What is "Material Ledger" (ML) in SAP and why is it mandatory in S/4HANA?
- A) A ledger that records material master data changes
- B) A CO/FI component that records all quantity and value flows for materials — enabling actual costing (revaluing inventory at actual cost at period-end) and multi-currency/multi-valuation inventory management; mandatory in S/4HANA as it underpins the Universal Journal's inventory valuation
- C) A ledger for tracking raw material purchases
- D) An MM module component for warehouse management

**Answer: B**

---

**Q26.** 🔴 What is "actual costing" in SAP Material Ledger?
- A) Tracking the actual physical count of materials
- B) A costing method where inventory is revalued at period-end using actual prices (weighted average of all price variances during the period) — providing true actual cost of goods manufactured/sold vs. standard cost
- C) Calculating the actual cost of a single production order
- D) A costing approach that uses real-time market prices

**Answer: B**

---

**Q27.** 🔴 What is "account-based CO-PA" vs. "costing-based CO-PA" in SAP?
- A) Account-based uses cost elements; costing-based uses G/L accounts
- B) Account-based CO-PA is reconciled with FI using G/L accounts and is the only option in S/4HANA (mandatory); costing-based CO-PA uses value fields for more flexible profitability analysis but is not reconciled with FI ledgers
- C) They are identical in S/4HANA
- D) Account-based is for manufacturing; costing-based is for services

**Answer: B**

---

**Q28.** 🔴 What is "variance analysis" in SAP CO Product Costing?
- A) Analyzing differences between two SAP system versions
- B) The process of calculating and categorizing the difference between the standard cost of a production order and its actual cost — variances are settled to CO-PA or stock for management analysis
- C) Analyzing budget vs. actual variances only
- D) A statistical tool for analyzing data quality in FI

**Answer: B**

---

**Q29.** 🔴 What is "special purpose ledger" (FI-SL) / "extension ledger" in SAP S/4HANA?
- A) A ledger for special one-time transactions
- B) An extension ledger in S/4HANA that stores only adjustment postings on top of a leading ledger — used for local GAAP adjustments without duplicating all transactions; inherited from the classic Special Purpose Ledger concept
- C) A ledger used only during system testing
- D) A ledger for recording intercompany eliminations

**Answer: B**

---

**Q30.** 🔴 What is the "leading ledger" vs. "non-leading ledger" in SAP S/4HANA parallel accounting?
- A) The leading ledger is the primary accounting ledger; non-leading ledgers are inactive
- B) The leading ledger (0L) is the primary IFRS/group accounting ledger used for most reporting and reconciliation; non-leading ledgers run in parallel for local GAAP (e.g., US GAAP, local statutory) with their own fiscal year variants and currency settings
- C) Leading ledger is for FI; non-leading is for CO only
- D) They are identical — the leading/non-leading distinction is cosmetic

**Answer: B**

---

**Q31.** 🟡 What is a "clearing account" in SAP FI?
- A) An account cleared of all balances at system startup
- B) A temporary transit account used to hold postings that will be matched and cleared against offsetting entries — e.g., GR/IR clearing, bank clearing accounts
- C) An account used during system testing only
- D) A balance sheet account with zero balance

**Answer: B**

---

**Q32.** 🟡 What is "document type" in SAP FI?
- A) A SAP file format classification
- B) A two-character key that classifies accounting documents (e.g., KR = vendor invoice, DZ = customer payment, SA = G/L posting) — controlling number range assignment and account types allowed
- C) A category defining the type of financial report
- D) A classification for SAP workflow approval types

**Answer: B**

---

**Q33.** 🟡 What is "posting key" in SAP FI?
- A) A key used to unlock SAP posting periods
- B) A two-digit code in SAP that determines whether a line item is a debit or credit, which account type it applies to (G/L, vendor, customer, asset), and which fields are required for entry
- C) A password required for financial posting authorization
- D) A configuration key for automatic account determination

**Answer: B**

---

**Q34.** 🔴 What is "FASB ASC 842" / "IFRS 16" and how does SAP handle it?
- A) SAP security standards for financial data
- B) Lease accounting standards requiring lessees to recognize right-of-use assets and lease liabilities on the balance sheet — SAP handles this through the SAP Lease Administration by Nakisa or RE-FX/Contract Accounting integration with FI-AA
- C) Financial reporting standards for digital assets
- D) SAP's own internal accounting policy

**Answer: B**

---

**Q35.** 🔴 What is "cash flow statement" and which SAP tool supports it?
- A) A report showing daily cash transactions from the bank
- B) A financial statement showing cash inflows and outflows from operating, investing, and financing activities — SAP supports it through Cash Flow Statement configuration in FI, SAP Analytics Cloud, or BW-based reporting
- C) A SAP CO report showing cost center cash budgets
- D) An AP report showing payment due dates

**Answer: B**

---
*35 Questions | Shared — SAP FICO deep dive.*
