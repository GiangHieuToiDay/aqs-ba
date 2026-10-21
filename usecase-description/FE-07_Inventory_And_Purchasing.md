# MODULE FE-07: INVENTORY AND PURCHASING

---

### UC-31 Look up and Manage Inventory Information

**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to inspect real-time inventory balances, warehouse storage bin locations, safety stock thresholds, and low-stock replenishment alerts across warehouses so that sanitary ware supplies remain adequate and organized.  

**Preconditions**  
1. The user is logged in with inventory lookup permissions.  
2. The Inventory Balance screen is accessible.  

**Normal Flow**  
1. The user navigates to the "Inventory Management" screen.  
2. The system displays a comprehensive inventory ledger: SKU Code, Product Name, Category, Brand, Warehouse Location (Main Warehouse, Showroom Floor), Bin/Rack Code (e.g., `RACK-A-04`), Physical On-Hand Quantity, Reserved Quantity, Available-to-Promise (ATP) Quantity, Safety Stock Threshold, and Stock Status (`Normal`, `Low Stock`, `Out of Stock`, `Overstocked`).  
3. The user searches for a specific SKU or filters by Category = "Showers & Faucets" and Status = "Low Stock".  
4. The system updates the ledger in real-time displaying matching items.  
5. The user clicks "Stock Card" (Bin Card) on a specific item (e.g., `SKU-INAX-C909`).  
6. The system displays the historical ledger of all stock movements for that SKU: Inward receipts, Outward sales deliveries, Customer returns, and Reserved holds, showing running balance and transaction reference codes.  
7. The user reviews the history and returns to the inventory overview.  

**Alternative Flows**  
- **Step 2 - Configure Minimum Safety Stock Threshold**:  
  1. The Store Owner clicks "Edit Threshold" on an item.  
  2. The Store Owner updates Minimum Safety Stock to 10 units.  
  3. The system saves the threshold. When available stock drops below 10, the system triggers automated low-stock warnings on the dashboard.  
  4. The system displays MSG69: "Inventory safety threshold updated successfully."  
- **Step 2 - Filter by Multiple Warehouses**:  
  1. The user selects a specific warehouse location from the warehouse filter dropdown (e.g., "Da Nang Regional Hub").  
  2. The system isolates stock quantities stored only within that specific facility.  

**Postconditions**  
1. Inventory balances and bin card movements are accurately displayed.  
2. Threshold changes are saved in the inventory master table.  

---

### UC-32 Manage Goods Receipt

**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to receive incoming shipments, verify physical quantities against supplier delivery notes, inspect ceramic quality/damage, and confirm goods receipts so that warehouse inventory is officially updated.  

**Preconditions**  
1. The Store Owner has warehouse receiving permissions.  
2. An active Purchase Order exists (UC-34), or an ad-hoc supplier delivery arrives.  

**Normal Flow**  
1. The Store Owner opens the "Goods Receipt" screen and clicks "Create Inward Receipt".  
2. The Store Owner selects the target Purchase Order (e.g., `PO-202610-008` from TOTO Vietnam) and destination warehouse.  
3. The system loads all ordered SKU lines with expected quantities.  
4. As warehouse staff unloads sanitary ceramics and brassware from the delivery truck, the Store Owner inspects boxes for cracks, hairline ceramic fractures, or missing accessories.  
5. The Store Owner enters actual received quantities (e.g., Expected: 20, Received: 20, Damaged: 0).  
6. The Store Owner uploads a photo of the signed supplier delivery note / bill of lading.  
7. The system uploads the image to Cloudinary and saves the proof document URL.  
8. The Store Owner enters delivery note number and clicks "Complete Goods Receipt".  
9. The system validates quantities, posts stock additions to the physical on-hand inventory, updates average cost price (WAC) for each SKU, and sets receipt status to `Completed`.  
10. The system creates a Goods Received Note (GRN) and alerts the Accountant module (FE-14) to record accounts payable.  
11. The system displays MSG70: "Goods receipt completed and inventory updated successfully."  

**Alternative Flows**  
- **Step 5 - Damaged or Defective Fixtures Detected**:  
  1. The Store Owner discovers 2 cracked toilet bowls.  
  2. The Store Owner marks: Received Good: 18, Damaged/Rejected: 2, and uploads photos of the cracked glaze.  
  3. The system records only 18 units into active saleable inventory, puts 2 units into the Defective / Quarantine location, and generates a Supplier Defect Return Claim (UC-37).  
- **Step 5 - Partial Delivery Received**:  
  1. Supplier ships only 10 out of 20 ordered items due to factory shortages.  
  2. The Store Owner records 10 units received.  
  3. The system marks the Purchase Order as `Partially_Received` and keeps 10 remaining units on back-order.  

**Postconditions**  
1. Physical inventory counts are incremented in the database.  
2. Stock valuation is recalculated.  
3. Inward receipt is logged and routed to accounting for payment processing.  

---

### UC-33 Manage Suppliers

**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to manage supplier profiles (TOTO Vietnam, Inax LIXIL, Caesar, Viglacera), contact representatives, tax codes, payment credit terms, and authorized brand lines so that procurement partnerships are organized.  

**Preconditions**  
1. The Store Owner is authenticated with procurement privileges.  
2. The Suppliers screen is accessible in the Admin Portal.  

**Normal Flow**  
1. The Store Owner opens the "Supplier Management" screen.  
2. The system displays the list of suppliers: Supplier Code, Company Name, Tax Code, Representative, Phone, Email, Payment Terms (e.g., Net 45 Days), Active Debt, and Status (`Active`, `Inactive`).  
3. The Store Owner clicks "Add Supplier".  
4. The system opens the supplier profile form.  
5. The Store Owner inputs:  
   - Supplier Code (e.g., `SUPP-TOTO-VN`).  
   - Company Name: "TOTO Vietnam Co., Ltd.".  
   - Tax Identification Number.  
   - Sales Representative Name, Phone Number, and Official Order Email.  
   - Warehouse Dispatch Address.  
   - Credit Terms (Net 30, Net 45, or Advance 100%).  
   - Linked Product Categories & Brands supplied.  
6. The Store Owner clicks "Save Supplier".  
7. The system validates that Supplier Code and Tax Code are unique, saves the supplier record, and displays MSG71: "Supplier profile saved successfully."  

**Alternative Flows**  
- **Step 2 - Edit Supplier Terms**:  
  1. The Store Owner selects an existing supplier and updates payment credit days from Net 30 to Net 45.  
  2. The system saves the revised terms and displays MSG72: "Supplier terms updated successfully."  
- **Step 2 - Deactivate Supplier**:  
  1. The Store Owner toggles status to `Inactive` for a discontinued distributor.  
  2. The system verifies there are no open pending purchase orders.  
  3. The system sets status to `Inactive` and blocks new PO generation for this vendor.  

**Postconditions**  
1. Supplier master record is stored in the database.  
2. Supplier details are available for purchase orders in UC-34.  

---

### UC-34 Manage Purchase Orders

**Primary Actors**  
Store Owner  

**Secondary Actors**  
Email Service  

**Description**  
As a Store Owner, I want to draft, approve, and dispatch purchase orders (POs) to sanitary ware manufacturers and track delivery progress so that store stock replenishment is systematically controlled.  

**Preconditions**  
1. The Store Owner is authenticated.  
2. Active Suppliers (UC-33) and SKUs (UC-10) exist.  

**Normal Flow**  
1. The Store Owner navigates to "Purchase Orders" and clicks "Create Purchase Order".  
2. The system initializes a PO form with an auto-generated PO Code (e.g., `PO-202610-0015`).  
3. The Store Owner selects Supplier (e.g., "Inax LIXIL Vietnam") and Destination Warehouse.  
4. The system automatically populates supplier payment terms and order email.  
5. The Store Owner adds SKU lines, entering purchase quantities and agreed wholesale unit costs.  
6. The system computes Line Totals, VAT (10%), and Total Purchase Value.  
7. The Store Owner specifies expected delivery arrival date (e.g., 5 business days).  
8. The Store Owner clicks "Approve & Send PO".  
9. The system validates the PO, sets status to `Sent_To_Supplier`, generates an official Purchase Order PDF, and triggers the Email Service to transmit the order directly to the vendor's sales department.  
10. The system displays MSG73: "Purchase order approved and emailed to supplier."  

**Alternative Flows**  
- **Step 5 - Auto-Generate PO based on Low Stock Suggestions**:  
  1. The Store Owner clicks "Auto-Fill Low Stock Items".  
  2. The system scans the inventory ledger for all SKUs currently below safety stock thresholds supplied by the selected vendor.  
  3. The system automatically populates recommended replenishment quantities based on reorder points.  
- **Step 8 - Save as Draft**:  
  1. The Store Owner saves the PO without sending it.  
  2. The system sets status to `Draft`, allowing future edits before formal approval.  
- **Step 2 - Cancel Open Purchase Order**:  
  1. The supplier cannot fulfill the order due to factory discontinued model.  
  2. The Store Owner cancels the PO, entering cancellation reason.  
  3. The system marks the PO as `Cancelled` and notifies the supplier.  

**Postconditions**  
1. Purchase Order is saved with status `Sent_To_Supplier`.  
2. Inventory tracking recognizes pipeline inbound stock.  

---

### UC-36 Manage Stock Reservations

**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to monitor, extend, or release reserved stock batches held for pending project quotations and sales orders so that warehouse capacity and stock are not locked unnecessarily.  

**Preconditions**  
1. Active stock reservations exist in the inventory database.  
2. The user has inventory control permissions.  

**Normal Flow**  
1. The user opens the "Stock Reservations" management screen.  
2. The system displays all active stock holds: Reservation ID, SKU Code, Product Name, Reserved Quantity, Linked Order/Quote Code, Customer/Contractor Name, Hold Expiry Date, and Deposit Status (`Deposited`, `Courtesy Hold`).  
3. The user filters by "Expiring Within 48 Hours".  
4. The system highlights reservations that have exceeded or are nearing their hold expiration.  
5. For an expired reservation where the customer did not finalize the project contract, the Store Owner clicks "Release Reserved Stock".  
6. The system prompts: "Are you sure you want to release 15 units of SKU-TOTO-001 back to available-to-promise inventory?"  
7. The Store Owner confirms.  
8. The system updates the reservation status to `Released`, returns the quantities to available saleable stock, and logs the release in the Stock Card.  
9. The system displays MSG74: "Stock reservation released successfully. Available stock restored."  

**Alternative Flows**  
- **Step 5 - Customer Paid Additional Deposit to Extend Hold**:  
  1. The contractor pays an additional milestone deposit to extend the hold by 30 days.  
  2. The Sales Staff clicks "Extend Hold", enters the new expiry date, and references the payment receipt.  
  3. The system extends the reservation and displays MSG75: "Reservation period extended successfully."  
- **Step 1 - Automated Nightly Release of Expired Courtesy Holds**:  
  1. The system runs an automated scheduled job at midnight.  
  2. The system detects courtesy holds (> 48 hours with zero deposit) that have expired.  
  3. The system automatically cancels the holds and restores inventory to the public catalog.  

**Postconditions**  
1. Reservation status is updated (`Released` or `Extended`).  
2. Available-to-promise (ATP) inventory levels are refreshed.  

---

### UC-37 Process Returned Goods and Vendor Returns

**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to inspect and process returned bathroom fixtures from customers (due to defective glaze, damaged shipment, or project excess) and manage vendor return claims so that inventory valuations and defective items are reconciled.  

**Preconditions**  
1. The customer has returned fixtures to the store/warehouse, or defective stock has been quarantined during goods receipt.  
2. The Store Owner is authorized to handle warehouse RMA (Return Merchandise Authorization).  

**Normal Flow**  
1. The Store Owner navigates to "Warehouse Returns & RMAs" and clicks "New Return Ticket".  
2. The Store Owner selects Return Type: "Customer Return" or "Supplier Return Claim".  
3. For Customer Return: The Store Owner searches and links the original Sales Order (FE-06).  
4. The system displays original order items and delivery timestamps.  
5. The Store Owner selects the returned SKU (e.g., `SKU-BASIN-01`), inputs quantity (1 unit), and specifies Reason ("Defective ceramic surface finish").  
6. The Store Owner inspects the item and categorizes Physical Disposition:  
   - `Restock to Saleable Inventory` (Brand new, unopened box).  
   - `Quarantine / Defective` (Chipped porcelain, leaking cartridge).  
7. The Store Owner uploads photos of the defect to Cloudinary.  
8. The Store Owner clicks "Approve Return".  
9. The system adjusts inventory according to disposition, updates the order return record, and generates a credit memo for the Accounting module (FE-14).  
10. The system displays MSG76: "Return ticket processed successfully."  

**Alternative Flows**  
- **Step 2 - Vendor Return Claim Sub-flow**:  
  1. The Store Owner selects "Supplier Return Claim" to return defective quarantined items back to manufacturer (e.g., TOTO).  
  2. The system lists all items in the Quarantine Bin.  
  3. The Store Owner selects items, attaches manufacturer warranty claim forms, and generates an Outward Supplier Return Note.  
  4. The system deducts the defective stock from the warehouse ledger and notifies the Accountant to debit the supplier's payable account.  

**Postconditions**  
1. Warehouse inventory registers return movements.  
2. Financial credit/debit memos are routed to FE-14.  

---

### UC-38 Import and Export Inventory Data

**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to export current inventory balance sheets and import stock count adjustments via Excel spreadsheets so that stock reconciliation can be audited efficiently.  

**Preconditions**  
1. The Store Owner is authenticated with master inventory permissions.  
2. The Inventory Import/Export tool is accessible.  

**Normal Flow**  
1. The Store Owner opens the "Inventory Data Tools" screen.  
2. The Store Owner clicks "Export Current Inventory" and selects the target warehouse.  
3. The system generates an Excel spreadsheet containing SKU Code, Barcode, Product Name, Category, Bin Location, Physical Stock, and Reserved Stock.  
4. The Store Owner conducts periodic warehouse stocktake with staff, recording physical counts in the "Actual Count" column of the sheet.  
5. The Store Owner returns to the system and clicks "Upload Stock Adjustment File".  
6. The system parses the spreadsheet, validates SKUs, and displays a Reconciliation Preview showing: SKU, System Count, Physical Count, Discrepancy (Variance), and Value Impact.  
7. The Store Owner reviews variances and clicks "Apply Stock Adjustment".  
8. The system updates the inventory ledger to reflect physical counts, logs stock adjustment entries with timestamp and user ID, and notifies the Accountant (FE-14).  
9. The system displays MSG77: "Inventory adjustments applied successfully: 24 SKUs updated."  

**Alternative Flows**  
- **Step 6 - Format or Validation Errors**:  
  1. The file contains unknown SKU codes or non-numeric values.  
  2. The system rejects the file and highlights erroneous cells.  
  3. The Store Owner corrects the file and re-uploads.  

**Postconditions**  
1. Database inventory balances are aligned with physical warehouse counts.  
2. Adjustment discrepancies are written to the immutable audit log.  
