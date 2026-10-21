# MODULE FE-11: REPORTING AND ANALYTICS

---

### UC-53 View Operations Dashboard and Alerts

**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to view high-level operations KPIs (daily sales, gross revenue, pending dispatches, critical low-stock alerts, overdue customer debts, and urgent warranty tickets) on an executive dashboard so that I can monitor overall store health and take immediate operational actions.  

**Preconditions**  
1. The Store Owner is authenticated with executive management permissions.  
2. The Dashboard screen is accessible upon logging in.  

**Normal Flow**  
1. The Store Owner accesses the "Operations Executive Dashboard".  
2. The system queries operational data across all modules and renders key metric cards:  
   - Today's Revenue: Total monetary sales (e.g., 185,400,000 VND) with percentage change vs. yesterday.  
   - Order Pipeline: Total Orders Today (18), Orders Awaiting Dispatch (5), Orders In Delivery (3).  
   - Critical Inventory Alerts: Number of SKUs below minimum safety stock threshold (12 SKUs).  
   - Outstanding Accounts Receivable: Total overdue customer receivables (> 30 days: 48,000,000 VND).  
   - Warranty & Technical Tickets: Open warranty claims (2) and today's scheduled installations (4).  
3. The system renders real-time graphical charts:  
   - Hourly sales velocity trend chart for today.  
   - Top-selling sanitary fixture categories (Pie chart: Toilets 45%, Showers 28%, Basins 18%, Others 9%).  
4. The Store Owner clicks on the "Critical Inventory Alerts" widget.  
5. The system navigates directly to the low-stock inventory list in FE-07 (UC-31) filtered to items requiring immediate purchase replenishment.  

**Alternative Flows**  
- **Step 2 - Refresh Real-Time Data**:  
  1. The Store Owner clicks the "Refresh Dashboard" button.  
  2. The system queries the latest database aggregates and updates all widgets without full page reload.  
- **Step 2 - Date Range Comparison**:  
  1. The Store Owner selects "This Month vs. Last Month" from the time period selector.  
  2. The system recalculates comparative metrics and renders side-by-side performance indicators.  

**Postconditions**  
1. Operational metrics are displayed without altering underlying transactional data.  

---

### UC-54 View Sales and Revenue Reports

**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to analyze multi-dimensional sales and revenue reports filtered by date range, product categories, brands (TOTO, Inax, Viglacera), sales channels (Showroom POS vs. Online), and individual sales staff performance so that revenue drivers and commercial growth can be evaluated.  

**Preconditions**  
1. The Store Owner has business analytics permissions.  
2. The Sales Reports screen is accessible.  

**Normal Flow**  
1. The Store Owner navigates to the "Sales & Revenue Reports" screen.  
2. The system renders filter parameters: Date Range (Daily, Weekly, Monthly, Custom Range), Sales Channel (All, POS Showroom, Online Web), Brand (All, TOTO, Inax, Caesar, Viglacera), Category (All, Toilets, Faucets, Basins), and Sales Representative.  
3. The Store Owner selects Date Range = "This Quarter", Brand = "TOTO", Channel = "All" and clicks "Generate Report".  
4. The system calculates and displays:  
   - Total Gross Sales Revenue, Total Discounts Allowed, Net Revenue, Cost of Goods Sold (COGS), and Gross Profit Margin (e.g., 28.5%).  
   - Trend Chart: Weekly revenue and profit curves.  
   - Detailed breakdown table: Product SKU, Units Sold, Net Sales Amount, Gross Profit, and % Contribution to Total Revenue.  
   - Staff Leaderboard: Revenue generated per Sales Staff member.  
5. The Store Owner reviews the analytics and clicks "Export Report to Excel".  
6. The system compiles the dataset into a styled `.xlsx` report and initiates browser download.  
7. The system displays MSG91: "Sales report exported successfully."  

**Alternative Flows**  
- **Step 3 - No Sales Data for Filter Range**:  
  1. The selected period or brand filter has zero transactions.  
  2. The system displays MSG92: "No sales transactions found for the selected criteria."  

**Postconditions**  
1. Consolidated sales figures and profitability metrics are computed and presented.  

---

### UC-55 View Inventory and Debt Reports

**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to view inventory valuation, stock turnover, slow-moving/dead stock analysis, and accounts receivable aging reports so that working capital health and inventory risks are controlled.  

**Preconditions**  
1. The user has financial/inventory reporting privileges.  
2. The Inventory & Debt Reports screen is accessible.  

**Normal Flow**  
1. The user navigates to the "Inventory & Debt Reports" section.  
2. The system presents two primary analytical views: "Inventory Valuation & Turnover" and "Accounts Receivable & Debt Aging".  
3. The user opens "Inventory Valuation & Turnover":  
   - The system displays Total Capital Tied Up in Stock (e.g., 2,450,000,000 VND), Total Physical Units On-Hand, Stock Turnover Ratio, and Average Days to Sell.  
   - Slow-Moving / Dead Stock Table: Highlights SKUs with zero sales in the past 90/180 days (e.g., slow-moving luxury bronze bathtubs) with accumulated holding cost.  
4. The user toggles to the "Debt Aging & Risk Report":  
   - The system renders the Debt Aging Matrix categorizing outstanding receivables into aging buckets: Current (0-30 days), 31-60 days, 61-90 days, and Overdue > 90 days.  
   - Risk Bad-Debt Summary: Flags high-risk contractor accounts that have exceeded their credit limits or payment deadlines.  
5. The user inspects the report and exports the summary to PDF.  

**Alternative Flows**  
- **Step 4 - Filter Debt by Contractor / Customer Tier**:  
  1. The user filters the aging report to display only "Interior Contractors".  
  2. The system filters the debt ledger and displays contractor-specific exposure.  

**Postconditions**  
1. Inventory holding capital and credit aging risk distributions are calculated and reviewed.  

---

### UC-56 Manage Operating Expenses and Export Reports

**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to record recurring store operating expenses (showroom rent, utilities, staff logistics, marketing, delivery truck fuel) and generate consolidated Net Profit & Loss (P&L) statements so that net store profitability is accurately determined.  

**Preconditions**  
1. The user is authorized to manage store expenses and executive financial reports.  
2. The Operating Expenses screen is accessible.  

**Normal Flow**  
1. The user opens the "Operating Expenses & P&L" screen.  
2. The system displays the expense ledger: Expense ID, Date, Expense Category (Rent, Electricity/Water, Logistics & Fuel, Marketing, Showroom Maintenance), Amount, Paid By, Description, and Attached Invoice Reference.  
3. The user clicks "Record New Expense".  
4. The system opens the expense entry form.  
5. The user enters Expense Date, selects Category = "Logistics & Fuel", enters Amount: `4,500,000 VND`, attaches fuel receipt note, and inputs Description: "Delivery truck diesel fuel for Week 41".  
6. The user clicks "Save Expense".  
7. The system records the expense in the financial ledger and updates the month-to-date operating expense total.  
8. The user clicks "Generate Comprehensive P&L Statement".  
9. The system aggregates Gross Revenue (from FE-06/UC-54) minus Cost of Goods Sold (COGS) to calculate Gross Profit, then subtracts Total Operating Expenses to compute Net Operating Profit (EBITDA).  
10. The user clicks "Export P&L Report (PDF/Excel)".  
11. The system downloads the official formatted financial statement and displays MSG93: "Operating expense recorded and P&L statement generated."  

**Alternative Flows**  
- **Step 5 - Expense Amount Exceeds Standard Discretionary Limit**:  
  1. An Accountant enters a major showroom maintenance expense of `35,000,000 VND` (threshold requiring Owner sign-off is > 10,000,000 VND).  
  2. The system flags the entry as `Pending_Owner_Approval`.  
  3. The Store Owner reviews and approves the expense before it is officially deducted from Net Profit.  

**Postconditions**  
1. Operating expense record is committed to the financial ledger.  
2. Net profit calculations reflect the newly logged expenditure.  
