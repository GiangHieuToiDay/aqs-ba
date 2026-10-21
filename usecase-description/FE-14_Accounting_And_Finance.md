# MODULE FE-14: ACCOUNTING AND FINANCE

---

### UC-62 Look up and Manage Financial Transactions

**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to search and filter cash and bank transaction ledgers, cash drawer balances, and bank account movements so that all monetary flows within the business are auditable and reconciled.  

**Preconditions**  
1. The user is logged in with accounting and financial permissions.  
2. The Financial Transactions screen is accessible in the Back-Office system.  

**Normal Flow**  
1. The Accountant navigates to the "Financial Transactions Ledger" screen.  
2. The system renders the comprehensive transaction ledger: Transaction ID (e.g., `TXN-20261013-0089`), Transaction Date/Time, Transaction Type (`Receipt / Inflow`, `Payment Voucher / Outflow`), Account Type (`Cash Drawer`, `Bank Account - Vietcombank`, `Bank Account - Techcombank`), Amount, Reference Entity (Sales Order, Purchase Invoice, Expense Voucher), Payer/Payee, and Status (`Posted`, `Voided`).  
3. The Accountant filters transactions by Date Range = "Today", Account Type = "Bank Account - Vietcombank", and Type = "Receipt".  
4. The system queries the financial ledger and updates the grid with matching transaction rows, displaying running account balance.  
5. The Accountant clicks on a transaction row to view the transaction details drawer.  
6. The system displays the full accounting breakdown: debited account, credited account, transaction description, originating cashier/staff user, and attached transaction receipts.  
7. The Accountant verifies the entries and closes the drawer.  

**Alternative Flows**  
- **Step 3 - Export Financial Transaction Journal**:  
  1. The Accountant selects a month and clicks "Export General Ledger (Excel)".  
  2. The system formats and downloads the complete double-entry journal spreadsheet.  
- **Step 5 - Filter by Specific Order or Invoice Reference**:  
  1. The Accountant enters Order Code `ORD-20261013-0045` in the search box.  
  2. The system filters all payment transactions (e.g., Initial Deposit + Final Settlement) tied to that specific order.  

**Postconditions**  
1. Financial transactions are audited without altering immutable accounting journal records.  

---

### UC-63 Manage Receipts and Payment Allocation

**Primary Actors**  
Accountant  

**Secondary Actors**  
None  

**Description**  
As an Accountant, I want to create official cash and bank receipt vouchers, allocate customer payments across multiple outstanding sales invoices or contractor debt lines, and issue formal electronic receipts so that customer balances are settled accurately.  

**Preconditions**  
1. The Accountant is authenticated with cashiering/receipt creation privileges.  
2. Customer payment has been received (via cash, bank wire, or COD remittance from UC-30).  

**Normal Flow**  
1. The Accountant opens the "Receipt Vouchers" management screen and clicks "Create Receipt Voucher".  
2. The system opens the voucher creation form with an auto-generated Voucher ID (e.g., `RCT-202610-0042`).  
3. The Accountant selects the Customer / Contractor (e.g., "Song Hong Construction Co.") and Receipt Method (`Cash Drawer` or `Bank Account`).  
4. The Accountant inputs total payment received: `50,000,000 VND`.  
5. The system automatically fetches and displays all unpaid and partially paid sales orders/invoices for that contractor, sorted by oldest due date (FIFO).  
6. The Accountant chooses payment allocation:  
   - Clicks "Auto-Allocate (FIFO)": System automatically allocates funds to clear Invoice 1 (20,000,000 VND), Invoice 2 (25,000,000 VND), and applies the remaining 5,000,000 VND towards Invoice 3.  
   - Or manually enters specific allocation amounts against individual orders.  
7. The Accountant enters the receipt description: "Debt installment payment for Riverside Villa bathrooms Phase 1".  
8. The Accountant clicks "Post Receipt Voucher".  
9. The system commits the transaction to the cash/bank ledger, updates invoice paid balances and order payment statuses, reduces the customer's outstanding accounts receivable balance, and generates a printable Electronic Receipt Voucher.  
10. The system displays MSG95: "Receipt voucher created and payments allocated successfully."  

**Alternative Flows**  
- **Step 4 - Customer Payment Exceeds Total Open Invoices (Unallocated Credit)**:  
  1. The customer pays more than their outstanding balance.  
  2. The system allocates 100% of open invoices and posts the excess amount as "Customer Advance Credit" (Prepayment).  
  3. The customer credit balance is stored and can be deducted from future project orders.  
- **Step 8 - Void Posted Receipt Voucher with Audit Justification**:  
  1. The Accountant discovers a receipt was posted against the wrong customer account.  
  2. The Store Owner and Accountant execute a "Reversal / Void Voucher" action, entering reason: "Wrong contractor selected by cashier".  
  3. The system generates a compensating reversal entry, restores the invoice balances, and logs the action in the Activity Log.  

**Postconditions**  
1. Receipt voucher is posted to the financial ledger.  
2. Linked sales order balances and customer debt ledgers are updated.  

---

### UC-64 Manage Payment Vouchers and Documents

**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As an Accountant or Store Owner, I want to create, review, approve, and record official payment vouchers (for supplier invoice payments, showroom rent, logistics freight, and technician service fees) with attached tax invoices and receipts so that store disbursements are controlled and documented.  

**Preconditions**  
1. The user has disbursement management permissions.  
2. The Payment Vouchers screen is accessible.  

**Normal Flow**  
1. The Accountant navigates to "Payment Vouchers" and clicks "Create Payment Voucher".  
2. The system initializes the form with an auto-generated Voucher ID (e.g., `PV-202610-0028`).  
3. The Accountant selects Payment Category: "Supplier Purchase Invoice Payment", "Store Operating Expense", or "Freight / Logistics".  
4. The Accountant selects Payee (e.g., "TOTO Vietnam Co., Ltd.") and Disbursement Account (e.g., "Bank - Techcombank").  
5. For supplier payment: The system lists open Purchase Orders / Goods Receipt Notes (from FE-07). The Accountant selects `GRN-TOTO-0012` with invoice balance `120,000,000 VND`.  
6. The Accountant enters disbursement amount (`120,000,000 VND`) and bank transfer reference code.  
7. The Accountant uploads the electronic VAT invoice (e-invoice XML/PDF) and bank wire confirmation slip.  
8. The system uploads files to Cloudinary and saves document URLs.  
9. If payment exceeds 20,000,000 VND, the system flags the voucher as `Pending_Owner_Approval`.  
10. The Store Owner reviews the voucher and clicks "Approve Disbursement".  
11. The system posts the payment voucher, deducts funds from the bank account balance, reduces Accounts Payable for TOTO Vietnam, and updates status to `Posted`.  
12. The system displays MSG96: "Payment voucher approved and posted successfully."  

**Alternative Flows**  
- **Step 10 - Store Owner Rejects Payment Voucher**:  
  1. The Store Owner rejects the voucher with note: "Discrepancy in supplier invoice amount. Hold payment pending vendor credit note."  
  2. The system sets status to `Rejected` and notifies the Accountant to investigate.  
- **Step 3 - Petty Cash Disbursement (Under 2,000,000 VND)**:  
  1. The Accountant logs an immediate petty cash voucher for showroom cleaning supplies (`350,000 VND`).  
  2. Because the amount is below the approval threshold, the system auto-approves and immediately posts the cash deduction.  

**Postconditions**  
1. Cash/bank ledger is reduced by the disbursement amount.  
2. Supplier accounts payable or operating expense accounts are updated.  
3. Supporting documentation is securely archived.  

---

### UC-65 Manage and Reconcile Accounts Receivable

**Primary Actors**  
Accountant  

**Secondary Actors**  
Email Service, SMS Service  

**Description**  
As an Accountant, I want to track customer aging debts, generate formal debt reconciliation statements, and dispatch periodic debt confirmation letters and reminders to contractors so that trade receivables are collected on time.  

**Preconditions**  
1. The Accountant is authenticated with debt control permissions.  
2. Customers with outstanding balances exist in the system.  

**Normal Flow**  
1. The Accountant navigates to "Accounts Receivable & Debt Reconciliation".  
2. The system renders the AR ledger table: Customer Name, Contractor Company, Total Receivables, Current (0-30 days), Overdue 31-60 days, Overdue > 60 days, and Last Reconciliation Date.  
3. The Accountant selects a contractor (e.g., "An Gia Investment Corp") and clicks "Generate Reconciliation Statement".  
4. The system aggregates all transactions within the selected period (Opening balance, Invoiced deliveries, Cash/Bank receipts, Returns/Credit memos, and Closing balance).  
5. The Accountant reviews the itemized transactions and clicks "Send Debt Reconciliation Statement".  
6. The system compiles an official Debt Reconciliation Statement (PDF) formatted with legal confirmation signature boxes.  
7. The system triggers the Email Service to deliver the statement to the contractor's accounting department and dispatches an SMS reminder.  
8. The system logs the reconciliation dispatch event with a 7-day confirmation deadline.  
9. The system displays MSG97: "Debt reconciliation statement generated and dispatched to contractor."  

**Alternative Flows**  
- **Step 8 - Contractor Confirms Statement with Zero Discrepancy**:  
  1. The contractor signs and returns the reconciliation statement.  
  2. The Accountant uploads the signed statement, sets status to `Reconciled_Agreed`, and logs the timestamp.  
- **Step 8 - Contractor Reports Discrepancy (e.g., Uncredited Returned Fixture)**:  
  1. The contractor claims an uncredited damaged faucet return of 2,500,000 VND.  
  2. The Accountant cross-checks with Warehouse Returns (FE-07 UC-37), confirms the credit memo was pending, posts the credit, and regenerates the statement.  

**Postconditions**  
1. Debt reconciliation status and audit records are updated.  
2. Official statement documents are archived in the customer dossier.  

---

### UC-66 Manage and Reconcile Accounts Payable

**Primary Actors**  
Accountant  

**Secondary Actors**  
None  

**Description**  
As an Accountant, I want to track accounts payable to sanitary ware manufacturers and distributors, review invoice payment due dates, and generate vendor debt reconciliation statements so that store debts are verified and paid accurately.  

**Preconditions**  
1. Goods receipts from suppliers (FE-07 UC-32) have been recorded.  
2. The Accounts Payable module is accessible.  

**Normal Flow**  
1. The Accountant opens the "Accounts Payable (Vendor Debt)" screen.  
2. The system displays a list of suppliers with financial balances: Supplier Code, Company Name (e.g., "Inax LIXIL Vietnam"), Total Invoiced Purchases, Total Paid, Current Outstanding Balance, and Upcoming Payment Due Dates (e.g., 30,000,000 VND due in 5 days).  
3. The Accountant selects a supplier to inspect the statement of account.  
4. The system renders the chronological vendor ledger: Goods Received Notes, Supplier Invoices, Returns & Discounts, and Payment Vouchers.  
5. The supplier emails their monthly vendor statement of account.  
6. The Accountant performs three-way matching against the system ledger (Purchase Order vs. Goods Received Note vs. Supplier Invoice).  
7. The Accountant confirms all line items and balances match.  
8. The Accountant marks the period as `Reconciled` and schedules eligible invoices for payment voucher creation (UC-64).  
9. The system displays MSG98: "Supplier accounts payable successfully reconciled."  

**Alternative Flows**  
- **Step 6 - Three-Way Matching Discrepancy Detected**:  
  1. The supplier's invoice charges 15,000,000 VND for an item, but the approved PO unit price was 14,000,000 VND.  
  2. The Accountant flags the line as `Discrepancy_Price_Mismatch` and contacts the supplier for a revised invoice before releasing payment.  

**Postconditions**  
1. Supplier payable balances are audited and confirmed.  
2. Verified invoices are queued for payment disbursement.  

---

### UC-67 View Cash Flow and Receipts/Payments Report

**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to generate and view cash flow statements, opening/closing cash drawer and bank balances, and inflow/outflow summaries so that liquidity health and working capital can be evaluated.  

**Preconditions**  
1. The user has financial reporting permissions.  
2. The Cash Flow Reports screen is accessible.  

**Normal Flow**  
1. The user navigates to the "Cash Flow & Receipts/Payments Report" screen.  
2. The user selects the reporting period (e.g., "Current Month") and account scope ("Consolidated All Accounts", "Cash Only", or "Bank Accounts Only").  
3. The user clicks "Generate Cash Flow Statement".  
4. The system aggregates all posted receipt vouchers and payment vouchers, displaying:  
   - Opening Cash Balance at start of period.  
   - Operating Inflows: Showroom POS cash sales, Online VietQR payments, Contractor debt collections, Customer deposits.  
   - Operating Outflows: Supplier inventory purchases, Showroom operating expenses, Staff logistics stipends, Delivery truck fuel.  
   - Net Cash Flow for the period.  
   - Closing Cash Balance across all vaults and bank accounts.  
5. The system renders graphical trend charts comparing daily cash inflows vs. outflows.  
6. The user clicks "Export Cash Flow Statement (PDF/Excel)".  
7. The system renders and downloads the standardized financial statement.  

**Alternative Flows**  
- **Step 4 - Drill Down to Source Vouchers**:  
  1. The user clicks on any summarized inflow category (e.g., "Contractor Debt Collections: 145,000,000 VND").  
  2. The system opens an audit drawer listing every individual receipt voucher that contributed to that aggregate number.  

**Postconditions**  
1. Real-time cash position and historical cash flow metrics are presented.  

---

### UC-68 View Revenue and Debt Report

**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to view detailed accounting revenue statements, recognized revenue vs. deferred deposits, bad debt allowances, and net customer balances across projects so that financial statements reflect GAAP/VAS standards.  

**Preconditions**  
1. The user has access to executive financial statements.  
2. Financial transactions are posted for the target period.  

**Normal Flow**  
1. The user opens the "Accounting Revenue & Debt Analysis" screen.  
2. The user selects the financial quarter or fiscal year and clicks "Generate Statement".  
3. The system computes accounting figures:  
   - Gross Invoiced Revenue.  
   - Sales Deductions: Trade discounts, combo bundle rebates, return credits.  
   - Net Realized Revenue (recognized upon physical delivery handover).  
   - Deferred Revenue: Customer project deposits for unfulfilled orders (held as liabilities until goods issue).  
   - Net Accounts Receivable and Bad Debt Provisioning Allowance (e.g., 5% provision for debts overdue > 90 days).  
4. The system renders the project-by-project financial summary matrix.  
5. The user reviews the metrics, signs off on the accounting period, and exports the statement to Excel.  
6. The system displays MSG99: "Accounting revenue and debt report generated successfully."  

**Alternative Flows**  
- **Step 3 - Recalculate Bad Debt Provisions**:  
  1. The Accountant adjusts bad debt reserve parameters based on updated risk assessments.  
  2. The system recalculates net asset valuation and updates the statement.  

**Postconditions**  
1. Accounting revenue and bad debt provisions are computed and documented.  

---

### UC-69 View Accounting Dashboard

**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to view real-time financial metrics, available cash balances across bank accounts, total pending payment verifications, and upcoming payable/receivable deadlines on an accounting dashboard so that financial control is immediate.  

**Preconditions**  
1. The user has accounting dashboard access privileges.  
2. The user accesses the Accounting module homepage.  

**Normal Flow**  
1. The user navigates to the "Accounting Dashboard".  
2. The system queries active ledgers and renders executive financial cards:  
   - Total Liquid Funds: Sum of Showroom Cash Drawer + Bank Accounts (e.g., `840,250,000 VND`).  
   - Pending Verifications: Number of unverified incoming bank wires awaiting reconciliation (e.g., 3 transactions / 24,500,000 VND).  
   - Accounts Receivable (AR) Summary: Total customer debt, including overdue portion flagged in red.  
   - Accounts Payable (AP) Summary: Total supplier debt, with invoices due within the next 7 days highlighted.  
3. The system renders interactive charts:  
   - 30-day rolling cash inflow vs. outflow velocity.  
   - Accounts Receivable Aging breakdown (0-30 days, 31-60 days, > 60 days).  
4. The user clicks on the "Pending Verifications" card to open the payment verification worklist (UC-70).  

**Alternative Flows**  
- **Step 2 - Bank Account Breakdown View**:  
  1. The user expands the "Liquid Funds" widget.  
  2. The system reveals individual balances for each bank account (Vietcombank Corporate Account: 520,000,000 VND; Techcombank Operational Account: 295,000,000 VND; Showroom Cash Vault: 25,250,000 VND).  

**Postconditions**  
1. Up-to-the-minute financial health indicators are displayed.  

---

### UC-70 Process Pending Payment Verifications

**Primary Actors**  
Accountant  

**Secondary Actors**  
None  

**Description**  
As an Accountant, I want to review unverified bank transfer alerts, VietQR transactions, and customer payment slips, match them against pending sales orders, and confirm or reject payments so that orders can proceed to packing without revenue leaks.  

**Preconditions**  
1. The Accountant is logged into the system.  
2. Unverified payments exist in status `Pending_Verification` (from FE-05 UC-18 or FE-06 UC-23).  

**Normal Flow**  
1. The Accountant opens the "Payment Verification Queue" screen.  
2. The system lists all unverified payments: Payment ID, Submission Timestamp, Customer Name, Phone, Target Order Code, Payment Method (`Bank Wire`, `VietQR`), Claimed Amount (e.g., `18,500,000 VND`), Bank Transfer Memo, and Uploaded Slip Image.  
3. The Accountant selects an unverified payment item to review.  
4. The system displays the customer's uploaded bank transaction screenshot side-by-side with the store's electronic bank statement feed.  
5. The Accountant confirms that:  
   - Bank reference number matches the bank transaction log.  
   - Credited amount matches the claimed order payment exactly (18,500,000 VND).  
   - Beneficiary account is the official AquaSuite store account.  
6. The Accountant clicks "Confirm & Post Payment".  
7. The system updates the payment status to `Verified`, sets the target Sales Order's payment status to `Paid` (or `Partially_Paid`), and posts an official cash receipt voucher in FE-14 (UC-63).  
8. The system updates the order fulfillment status to `Processing`, releasing it for warehouse goods issue (FE-08 UC-39).  
9. The system displays MSG100: "Payment verified successfully. Order released for fulfillment."  

**Alternative Flows**  
- **Step 5 - Payment Discrepancy / Fraudulent Slip (Rejection Flow)**:  
  1. The Accountant investigates the bank statement and finds no matching credit entry (or discovers an edited/fake screenshot).  
  2. The Accountant clicks "Reject Payment".  
  3. The system prompts for Rejection Reason: "No matching credit found on bank statement as of 13/10/2026", "Transferred amount is less than claimed", or "Invalid reference memo".  
  4. The Accountant submits the rejection.  
  5. The system sets payment status to `Rejected`, alerts Sales Staff to contact the customer, and keeps the order on hold.  
  6. The system sends an email notification to the customer informing them of the payment verification failure and requesting a valid bank trace.  
- **Step 5 - Transferred Amount Differs from Order Value (Partial Payment)**:  
  1. Order total is 20,000,000 VND, but customer wire transfer is verified at only 15,000,000 VND.  
  2. The Accountant verifies the 15,000,000 VND.  
  3. The system credits 15,000,000 VND, sets order payment status to `Partially_Paid` with remaining balance 5,000,000 VND, and prompts Sales Staff to collect the remainder before final delivery.  

**Postconditions**  
1. Payment record is verified and posted to the general ledger.  
2. Associated sales order transitions to the fulfillment pipeline.  
3. Verification outcome is logged in the Activity Log with the verifying Accountant's user ID.  
