# MODULE FE-06: SALES AND QUOTATIONS

---

### UC-21 Create and Manage Sales Orders

**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to create sales orders at the showroom counter/POS, add product SKU line items, apply item discounts, calculate taxes, and manage sales order lifecycles so that retail transactions and store sales are accurately processed.  

**Preconditions**  
1. The Sales Staff is authenticated and assigned an active sales shift.  
2. The POS Sales screen is accessible in the Back-Office system.  

**Normal Flow**  
1. The Sales Staff opens the POS / New Sales Order screen.  
2. The system initializes an empty order draft with an auto-generated Order ID (e.g., `ORD-POS-20261013-0045`), current date/time, and logged-in Sales Staff name.  
3. The Sales Staff looks up an existing customer by phone/name or enters walk-in customer details.  
4. The Sales Staff adds items to the order by scanning barcodes with a handheld scanner or searching by SKU/Name (e.g., "Inax C-909VN Toilet", "AquaSuite Brass Basin Faucet").  
5. The system verifies real-time available stock in the primary warehouse, retrieves unit selling prices, and appends each SKU line item with quantity 1.  
6. The Sales Staff adjusts quantities, applies line discounts (within authorized staff limit, e.g., up to 5%), and specifies delivery instructions.  
7. The system recalculates Subtotal, Total Discount, VAT (10%), and Grand Total.  
8. The Sales Staff clicks "Confirm Order".  
9. The system validates stock availability, creates the Sales Order record with status `Confirmed`, reserves the physical stock, and prompts for payment settlement (UC-23).  
10. The system displays MSG58: "Sales order created successfully."  

**Alternative Flows**  
- **Step 4 - Walk-in Retail Customer (No Profile)**:  
  1. The Sales Staff selects the system default "Retail Walk-in Customer" account.  
  2. The system continues order processing without requiring customer registration.  
- **Step 5 - Insufficient Stock Warning**:  
  1. The requested quantity exceeds available physical stock in the local warehouse.  
  2. The system warns: "Warning: Insufficient stock at local warehouse. Available: 3, Requested: 5."  
  3. The Sales Staff may split fulfillment from a central warehouse or convert the deficit items to a pre-order reservation.  
- **Step 6 - Discount Exceeds Staff Authorization Threshold**:  
  1. The Sales Staff attempts to apply an 8% discount (authorized limit is 5%).  
  2. The system locks the field and prompts: "Discount exceeds authorized threshold. Manager override PIN required."  
  3. The Store Owner enters their PIN/credentials to authorize the discount.  
  4. The system logs the manager override in the Activity Log and applies the discount.  
- **Step 2 - Filter and Manage Existing Orders**:  
  1. The Sales Staff opens "Order Management" list.  
  2. The system displays orders filterable by Status, Date, Customer, and Channel (POS, Online, Quote).  
  3. The Sales Staff can inspect order details, print invoice/delivery slips, or duplicate orders.  

**Postconditions**  
1. Sales order is persisted with status `Confirmed`.  
2. Stock reservation entries are created for the ordered SKUs.  
3. Sales performance is credited to the logged-in Sales Staff.  

---

### UC-22 Confirm and Process Online Orders

**Primary Actors**  
Sales Staff  

**Secondary Actors**  
Email Service, SMS Service  

**Description**  
As a Sales Staff, I want to review incoming online e-commerce orders, verify stock availability, confirm shipping addresses and phone contacts with customers, and approve orders so that orders can transition to warehouse packing and delivery.  

**Preconditions**  
1. The Sales Staff has access to the "Online Order Processing" queue.  
2. At least one online order with status `Pending_Confirmation` or `Payment_Confirmed` exists.  

**Normal Flow**  
1. The Sales Staff opens the "Online Orders Queue".  
2. The system displays pending online orders sorted by creation timestamp, showing Order Code, Customer Name, Phone, Delivery Address, Payment Status, and Fulfillment Status.  
3. The Sales Staff selects an order to review.  
4. The system displays full order details: ordered bathroom fixtures, customer delivery notes (e.g., "Elevator available to 4th floor", "Call 30 minutes before arrival"), delivery address, and payment confirmation status.  
5. For COD orders, the Sales Staff calls the customer's phone number to verify purchase intent and delivery schedule.  
6. The Sales Staff confirms the stock availability across warehouses and clicks "Approve Order for Fulfillment".  
7. The system transitions the order status to `Processing` (Ready for Goods Issue), locks warehouse inventory for packing, and creates a staging request for the Warehouse module (FE-08).  
8. The system triggers the Email Service and SMS Service to notify the customer that their order has been approved and is being prepared for dispatch.  
9. The system displays MSG59: "Online order approved and queued for warehouse dispatch."  

**Alternative Flows**  
- **Step 5 - Customer Unreachable (COD Order)**:  
  1. The customer does not answer the verification call.  
  2. The Sales Staff logs a call attempt note: "Call 1 - Unanswered" and keeps status as `Pending_Confirmation`.  
  3. The system schedules an automated SMS reminder via SMS Service.  
  4. If unreachable after 3 attempts across 24 hours, the Sales Staff cancels the order with reason "Customer unreachable" and releases stock reservations.  
- **Step 6 - Customer Requests Order Modifications During Call**:  
  1. The customer asks to change rough-in size or add silicone sealant tubes.  
  2. The Sales Staff initiates UC-26 to amend order line items before approving.  

**Postconditions**  
1. Order status is updated to `Processing`.  
2. Warehouse module receives the fulfillment staging task.  
3. Customer receives notification of approval.  

---

### UC-23 Manage Payment and Payment Verification

**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Sales Staff or Store Owner, I want to record customer payments (Cash, VietQR, Credit Card, Bank Wire), upload payment proofs, and verify transaction statuses so that order payment records are settled and auditable.  

**Preconditions**  
1. A valid Sales Order exists in status `Confirmed` or `Processing`.  
2. The user has payment processing permissions.  

**Normal Flow**  
1. The Sales Staff opens the "Payment Settlement" drawer for an order.  
2. The system displays total order amount, amount previously paid, and outstanding balance.  
3. The Sales Staff selects Payment Method:  
   - Cash: Enters cash received from customer. The system calculates change due.  
   - POS Card Swipe: Enters card authorization code and terminal receipt number.  
   - Bank Wire / VietQR: Enters bank transaction reference number.  
4. If payment proof is provided (e.g., wire transfer slip from mobile banking app), the Sales Staff uploads the slip image.  
5. The system uploads the payment receipt to Cloudinary and saves the image URL.  
6. The Sales Staff clicks "Record Payment".  
7. The system creates a Payment Record, updates the order's `PaidAmount`, and evaluates balance:  
   - If `PaidAmount == TotalAmount`: Sets payment status to `Paid`.  
   - If `PaidAmount < TotalAmount`: Sets payment status to `Partially_Paid` and records remaining balance.  
8. The system issues an electronic receipt, notifies Accountant module (FE-14), and displays MSG60: "Payment recorded successfully."  

**Alternative Flows**  
- **Step 7 - Customer Payment Requires Accountant Verification**:  
  1. For high-value bank transfers (> 50,000,000 VND), payment status is set to `Pending_Verification`.  
  2. The payment record is routed to Accountant queue (FE-14 UC-70) for bank ledger reconciliation.  
  3. The system displays MSG61: "Payment slip uploaded and submitted for Accountant verification."  
- **Step 3 - Split Payment Across Multiple Tender Types**:  
  1. The customer pays partially in cash (10,000,000 VND) and the remainder via Credit Card (25,000,000 VND).  
  2. The Sales Staff adds two distinct payment lines.  
  3. The system records both transactions independently and updates the consolidated balance.  

**Postconditions**  
1. Payment transaction record is written to the financial ledger.  
2. Order payment status is updated.  
3. Audit log records the cashier and payment method details.  

---

### UC-24 Manage Customer Debt and Receivables

**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
SMS Service  

**Description**  
As a Sales Staff or Store Owner, I want to track customer credit limits, outstanding balances, credit terms, and overdue aging so that sales on credit to contractors and developers are tightly controlled.  

**Preconditions**  
1. The customer is registered as a contractor or commercial client with approved credit terms.  
2. The user has credit/debt viewing permissions.  

**Normal Flow**  
1. The Sales Staff opens the "Customer Debt & Receivables" screen.  
2. The system renders the debt overview table: Customer Name, Company, Credit Limit (e.g., 200,000,000 VND), Total Outstanding Balance, Available Credit, Credit Term (e.g., Net 30 days), and Overdue Balance.  
3. The Sales Staff searches for a specific contractor (e.g., "Song Hong Construction Co.").  
4. The system displays the contractor's debt detail card:  
   - Unpaid invoices and delivery notes breakdown with aging brackets (0-30 days, 31-60 days, > 60 days).  
   - Repayment history and running statement of account.  
5. The Sales Staff reviews the balance before issuing a new credit order.  
6. If the contractor has overdue invoices, the Sales Staff clicks "Send Debt Reminder".  
7. The system triggers SMS Service to send an automated debt notification containing outstanding balance and payment bank details.  
8. The system displays MSG62: "Debt reminder dispatched successfully."  

**Alternative Flows**  
- **Step 5 - Credit Limit Exceeded on New Order Creation**:  
  1. Sales Staff attempts to create a new order on credit that exceeds the customer's Available Credit (or customer has invoices overdue > 60 days).  
  2. The system blocks credit approval and displays MSG63: "Credit limit exceeded or overdue balance detected. Order on credit is blocked."  
  3. The Sales Staff can only proceed if Store Owner enters an administrative override approval.  
- **Step 4 - Generate Statement of Account (SOA)**:  
  1. The Sales Staff clicks "Export Statement of Account (PDF)".  
  2. The system generates a formatted statement showing all debit/credit movements for reconciliation.  

**Postconditions**  
1. Debt status and aging metrics are calculated and displayed.  
2. Debt reminders and credit overrides are logged in the Activity Log.  

---

### UC-26 Process Order Modification or Cancellation Requests

**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
Email Service  

**Description**  
As a Sales Staff or Store Owner, I want to review, approve, or reject customer requests for order item changes or cancellations so that inventory reservations, warehouse packing, and payments are accurately adjusted.  

**Preconditions**  
1. An order change or cancellation request has been submitted by a Customer (UC-20) or initiated at the sales counter.  
2. The order has not completed delivery.  

**Normal Flow**  
1. The Sales Staff accesses the "Order Change Requests" list.  
2. The system displays pending requests showing Order Code, Customer Name, Request Type (`Full Cancellation`, `Address Change`, `Item Swap`), Reason, and Submission Time.  
3. The Sales Staff opens the request details to verify the order fulfillment status.  
4. The system confirms the order items have not yet departed the warehouse.  
5. For a cancellation request on a paid order, the Sales Staff clicks "Approve Cancellation".  
6. The system cancels the order, releases all reserved stock back to active inventory, and creates a refund task in the Accounting module (FE-14).  
7. The system triggers the Email Service to inform the customer of the approval and refund timeline.  
8. The system displays MSG64: "Order cancellation approved and inventory released."  

**Alternative Flows**  
- **Step 4 - Goods Already Dispatched (Rejection Flow)**:  
  1. The warehouse has already loaded the truck and dispatched the goods.  
  2. The Sales Staff contacts the delivery driver, finds delivery is already underway, and clicks "Reject Cancellation".  
  3. The Sales Staff enters rejection note: "Goods are currently out for delivery. Please refuse receipt at door or request return upon arrival."  
  4. The system sends a rejection notification email to the customer and maintains order status as `Dispatched`.  
- **Step 3 - Item Swap Request (Modification)**:  
  1. The customer requested changing rough-in dimension or color finish.  
  2. The Sales Staff adjusts the line items in the order, verifies price differences, and approves the change.  
  3. If new total is higher: System generates an additional payment invoice.  
  4. If new total is lower: System schedules a partial refund credit.  

**Postconditions**  
1. Order status is updated to `Cancelled` or amended with revised SKUs.  
2. Stock reservations are adjusted accordingly.  
3. Activity is logged in Activity Log.  

---

### UC-27 Manage Project Quotations

**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
Email Service  

**Description**  
As a Sales Staff or Store Owner, I want to create, calculate tiered project discounts, format, and dispatch formal project quotations for large villas, apartment complexes, or hotel projects so that institutional contracts can be negotiated and closed.  

**Preconditions**  
1. The Sales Staff is logged into the system.  
2. The Project Quotations module is accessible.  

**Normal Flow**  
1. The Sales Staff opens the "Project Quotations" screen and clicks "Create New Quotation".  
2. The system renders the quotation builder with fields for:  
   - Project Name (e.g., "Riverside Eco Villa - 12 Bathrooms").  
   - Customer / Contractor Selection (e.g., "An Gia Investment Corp").  
   - Quotation Validity Period (e.g., 30 days).  
   - Delivery Address & Construction Phase Notes.  
3. The Sales Staff adds fixture lines grouped by bathroom zones (e.g., Master Bath 1 & 2: Neorest Toilets; Guest Baths: One-piece Toilets).  
4. The system checks warehouse stock and pipeline incoming inventory, retrieves base list prices, and displays total line items.  
5. The Sales Staff enters special project discount rates (e.g., 12% bulk discount on sanitary ceramics, 8% on brass faucets).  
6. If discount exceeds standard sales threshold, the Store Owner reviews and signs off within the system.  
7. The Sales Staff enters terms: Payment milestones (30% deposit, 50% before dispatch, 20% post-handover) and staged delivery terms.  
8. The Sales Staff clicks "Generate Quote PDF & Send".  
9. The system generates an official branded PDF quotation document and dispatches it via the Email Service.  
10. The system saves the quotation with status `Sent` and displays MSG65: "Project quotation generated and sent to contractor."  

**Alternative Flows**  
- **Step 2 - Duplicate Existing Quotation for Revision**:  
  1. The contractor requests a revised version (e.g., substituting luxury items with mid-range items).  
  2. The Sales Staff clicks "Create Version Revision".  
  3. The system duplicates the quote as `Rev 2`, preserving the original version history.  
- **Step 8 - Export PDF for In-Person Contract Signing**:  
  1. The Sales Staff clicks "Export PDF".  
  2. The system renders and downloads a formatted commercial proposal ready for printing.  

**Postconditions**  
1. Project quotation record is saved in the database with unique Quote Code.  
2. Version tracking records are preserved.  

---

### UC-28 Convert Quotation to Sales Order

**Primary Actors**  
Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, I want to convert an approved/accepted project quotation directly into an active sales order so that goods can be reserved and fulfillment initiated without re-entering data.  

**Preconditions**  
1. A Project Quotation exists in status `Sent` or `Accepted`.  
2. The contractor has officially accepted the commercial terms.  

**Normal Flow**  
1. The Sales Staff locates the accepted quotation in the "Project Quotations" list.  
2. The Sales Staff clicks "Convert to Sales Order".  
3. The system validates quotation validity date and prompts for confirmation.  
4. The system creates a new Sales Order pre-populated with:  
   - All customer details and project construction site.  
   - Complete list of sanitary ware SKUs, agreed project discounted prices, and payment milestones.  
   - Reference link back to the originating Quotation ID.  
5. The system locks the agreed pricing and creates inventory reservations for the items.  
6. The system updates the quotation status to `Converted_To_Order`.  
7. The system redirects the Sales Staff to the newly created Sales Order and displays MSG66: "Quotation successfully converted to Sales Order."  

**Alternative Flows**  
- **Step 3 - Quotation Validity Expired**:  
  1. The quotation validity period has elapsed, and catalog prices have increased in the interim.  
  2. The system warns: "Quotation has expired. Pricing has changed for 2 items. Would you like to recalculate or request Store Owner override?"  
  3. If Store Owner overrides: Pricing is preserved and conversion proceeds.  
  4. If recalculated: Pricing is updated to current catalog rates.  

**Postconditions**  
1. A new Sales Order is created in FE-06.  
2. Quotation status is updated to `Converted_To_Order`.  
3. Stock reservations are created in FE-07.  

---

### UC-29 Manage Discounts, Deposits, and Stock Reservation

**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to apply special project discounts, record customer construction deposits, and reserve stock for long-term project timelines so that sanitary ware supplies are secured for the project.  

**Preconditions**  
1. A confirmed Sales Order or Project Contract exists.  
2. The user has financial/discount authorization permissions.  

**Normal Flow**  
1. The Sales Staff opens the Sales Order and selects the "Deposits & Reservations" tab.  
2. The Sales Staff enters the customer's project deposit amount (e.g., 30% of order value = 60,000,000 VND).  
3. The Sales Staff specifies the desired stock reservation duration (e.g., "Hold stock for 45 days pending bathroom tiling completion").  
4. The system verifies that deposit meets the minimum threshold (at least 20% required for stock reservations longer than 14 days).  
5. The Sales Staff records the deposit transaction (Cash/Bank Wire) via UC-23.  
6. The system sets the reservation status on all linked SKUs to `Reserved_Locked` with an expiration date set to 45 days from current date.  
7. The system updates available-to-promise (ATP) inventory across the warehouse without deducting physical stock.  
8. The system displays MSG67: "Deposit recorded and stock reserved for 45 days successfully."  

**Alternative Flows**  
- **Step 4 - Reservation Request with Zero Deposit**:  
  1. Customer requests holding stock without placing a deposit.  
  2. The system permits a temporary courtesy hold of maximum 48 hours only.  
  3. If deposit is not received within 48 hours, the system automatically releases reserved stock back to the general pool (UC-36).  
- **Step 3 - Extend Reservation Period**:  
  1. Contractor encounters construction delays and requests a 30-day extension.  
  2. The Store Owner approves the extension in the system.  
  3. The system extends the reservation expiry date and logs the approval.  

**Postconditions**  
1. Customer deposit is credited to the order balance.  
2. Inventory ledger marks SKUs as reserved, preventing them from being sold to other buyers.  

---

### UC-30 Manage COD Collection and Remittance

**Primary Actors**  
Sales Staff, Accountant, Shipper  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Accountant, I want to monitor Cash on Delivery (COD) amounts collected by shippers and verify cash handovers at the end of each delivery shift so that all delivered orders are financially settled without revenue leakage.  

**Preconditions**  
1. A Shipper has completed deliveries with cash collected (FE-08 UC-41).  
2. The Shipper returns to the showroom/warehouse to remit the collected cash.  

**Normal Flow**  
1. The Accountant or Sales Staff opens the "COD Settlement & Remittance" screen.  
2. The system lists all active delivery runs with pending cash collections grouped by Shipper.  
3. The Accountant selects the Shipper (e.g., "Nguyen Van A - Shift 13/10").  
4. The system displays all orders marked as delivered by the Shipper, along with individual COD amounts collected (e.g., Order 1: 3,500,000 VND; Order 2: 7,200,000 VND; Total to remit: 10,700,000 VND).  
5. The Shipper hands over physical cash to the Accountant.  
6. The Accountant counts the cash, confirms the physical total matches the system total (10,700,000 VND), and clicks "Confirm Cash Remittance".  
7. The system updates all linked orders' payment status to `Paid`, generates an official Cash Inward Receipt in FE-14, and clears the Shipper's liability ledger.  
8. The system displays MSG68: "COD remittance verified: 10,700,000 VND settled successfully."  

**Alternative Flows**  
- **Step 6 - Cash Discrepancy (Shortage or Overpayment)**:  
  1. The physical cash handed over is less than the system calculation (e.g., short by 500,000 VND due to customer short-payment).  
  2. The Accountant records the actual cash received and logs a discrepancy note.  
  3. The system sets the remittance status to `Partial_Remittance` and keeps an open debit line against the order or Shipper until resolved.  

**Postconditions**  
1. Order payment records are marked as fully settled.  
2. Financial cash receipts are automatically created in FE-14.  
3. Shipper's daily cash liability balance is reset to zero.  
