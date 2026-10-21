# MODULE FE-08: WAREHOUSE ISSUE AND DELIVERY

---

### UC-39 Manage Goods Issue Notes

**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to create, verify, and confirm goods issue notes (GINs) against confirmed sales orders so that sanitary ware items are officially deducted from the warehouse inventory and staged for delivery.  

**Preconditions**  
1. A Sales Order is in status `Processing` / ready for fulfillment.  
2. The Store Owner has warehouse dispatch authorization.  

**Normal Flow**  
1. The Store Owner opens the "Goods Issue Notes" screen and clicks "Create Goods Issue Note".  
2. The Store Owner selects the target Sales Order (e.g., `ORD-20261013-0045`) and Warehouse origin.  
3. The system loads all ordered SKUs and quantities that have not yet been issued.  
4. The Store Owner inspects the physical packaging in the warehouse, verifies that serial numbers or batch numbers match, and enters the quantities to be issued.  
5. The Store Owner assigns a packing technician and selects staging bay (e.g., `BAY-02`).  
6. The Store Owner clicks "Confirm Goods Issue".  
7. The system permanently deducts the issued physical stock from the inventory ledger, clears corresponding stock reservations, and updates the order fulfillment progress.  
8. The system generates an official Goods Issue Note (GIN) with printable barcode and staging tags.  
9. The system displays MSG78: "Goods Issue Note confirmed and warehouse inventory deducted."  

**Alternative Flows**  
- **Step 4 - Staged Partial Goods Issue (Split Delivery)**:  
  1. For large construction orders, the contractor only requires 5 out of 15 toilets for Phase 1.  
  2. The Store Owner enters quantity 5 for this issue note.  
  3. The system deducts 5 units and keeps 10 units in the order's unfulfilled balance.  
  4. The system marks the order fulfillment status as `Partially_Fulfilled`.  
- **Step 4 - Physical Stock Damaged During Staging**:  
  1. A ceramic toilet bowl is cracked while being moved to the staging bay.  
  2. The Store Owner reports damage in the system.  
  3. The system quarantines the damaged unit, logs a warehouse shrinkage entry, and requests a replacement unit from another bin.  

**Postconditions**  
1. Physical warehouse inventory is decremented in the database.  
2. Goods Issue Note record is marked as `Issued` and ready for dispatch assignment.  

---

### UC-40 Create and Manage Delivery Dispatches

**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
SMS Service  

**Description**  
As a Store Owner or Sales Staff, I want to organize delivery dispatches, bundle multiple customer orders into efficient vehicle routes, and assign delivery runs to internal Shippers so that job-site delivery schedules are fulfilled on time.  

**Preconditions**  
1. Goods Issue Notes are staged in the warehouse.  
2. Active Shippers and transport vehicles are registered in the system.  

**Normal Flow**  
1. The user navigates to the "Delivery Dispatch & Trip Planning" screen.  
2. The system displays all staged Goods Issue Notes awaiting delivery, showing Customer Name, Destination Address, District, Weight/Volume, Fragile Porcelain Warning, and COD Amount.  
3. The user selects orders located in the same geographical zone (e.g., District 2 & District 9).  
4. The user clicks "Create Delivery Trip".  
5. The system generates a Trip Sheet (e.g., `TRIP-20261013-01`) and prompts to assign a driver.  
6. The user selects an available Shipper (e.g., "Nguyen Van A"), vehicle type (1.5-ton Truck), and sets planned departure time.  
7. The user specifies delivery sequence (Stop 1, Stop 2, Stop 3).  
8. The user clicks "Dispatch Trip".  
9. The system transitions the Trip status to `Dispatched`, marks all linked orders as `Dispatched` (Out for Delivery), and pushes the run sheet to the Shipper's mobile app.  
10. The system triggers the SMS Service to notify each recipient customer of their estimated delivery window and driver contact.  
11. The system displays MSG79: "Delivery trip created and dispatched to Shipper."  

**Alternative Flows**  
- **Step 6 - Driver Already Assigned to Overlapping Trip**:  
  1. The selected Shipper is currently on an active, unfinished delivery run.  
  2. The system warns: "Driver is currently on Trip #04 (2 stops remaining). Assign anyway as next queued trip?"  
  3. The user confirms to queue the trip or chooses another driver.  
- **Step 2 - Reassign or Cancel Delivery Trip**:  
  1. Vehicle breakdown occurs before departure.  
  2. The user reassigns the trip to another driver or cancels the trip sheet to re-group packages.  
  3. The system resets the staged packages back to `Awaiting_Dispatch`.  

**Postconditions**  
1. Delivery trip sheet is created in the database.  
2. Linked orders transition to `Dispatched` status.  
3. Shippers and customers receive real-time schedule notifications.  

---

### UC-41 Execute and Update Delivery Results

**Primary Actors**  
Shipper  

**Secondary Actors**  
Cloudinary, SMS Service  

**Description**  
As a Shipper, I want to update delivery milestone progress, capture customer sign-off signatures and site delivery photos, collect COD cash payments, or log failed delivery attempts so that delivery completion is verified in real-time.  

**Preconditions**  
1. The Shipper is logged into the mobile web interface.  
2. A delivery trip is currently assigned to the Shipper (UC-43).  

**Normal Flow**  
1. The Shipper arrives at the customer's delivery address (e.g., "Villa 14, Riverside Project") and taps "Arrived at Site" on the mobile interface.  
2. The system updates trip tracking to `Arrived` and timestamps the location.  
3. The Shipper unloads the bathroom fixtures, assists the customer in inspecting ceramic surfaces for cracks, and taps "Proceed to Handover".  
4. The system presents the Handover checklist: Verified all boxes intact, inspected glaze and brass accessories.  
5. For COD orders, the Shipper collects the cash payment (e.g., 8,500,000 VND) and confirms "Cash Collected".  
6. The Shipper requests the customer's digital signature directly on the mobile screen and captures a photo of the delivered fixtures placed at the site.  
7. The system uploads the proof-of-delivery photo and signature to Cloudinary.  
8. The Shipper taps "Confirm Delivery Complete".  
9. The system updates the order status to `Delivered`, sets payment status to `Paid` (if COD), and logs the cash into the Shipper's daily remittance ledger.  
10. The system triggers SMS Service to send an electronic delivery confirmation and e-warranty activation notice to the customer.  
11. The system displays MSG80: "Delivery completed successfully!" and presents navigation to the next stop.  

**Alternative Flows**  
- **Step 3 - Delivery Failed (Customer Not Home / Site Inaccessible)**:  
  1. Customer cannot be reached by phone, or site gate is locked.  
  2. The Shipper taps "Report Failed Delivery".  
  3. The system prompts for Failure Reason: "Customer unreachable after 3 calls", "Construction site closed", "Customer requested rescheduling".  
  4. The Shipper captures a photo of the closed site gate and submits.  
  5. The system sets the delivery stop status to `Delivery_Failed`, alerts Sales Staff, and retains items on the truck for return to warehouse.  
- **Step 3 - Customer Rejects Damaged Fixture at Doorstep**:  
  1. Customer detects a hairline crack on a toilet tank during inspection.  
  2. The Shipper selects "Partial Rejection / Damaged Item", photographs the crack, and hands over only the undamaged items.  
  3. The system updates the order with a partial return note and prompts Sales Staff to initiate an expedited replacement delivery.  

**Postconditions**  
1. Order status is marked as `Delivered` with timestamp, photo, and signature.  
2. COD cash collected is logged against the Shipper's liability ledger.  
3. Customer receives delivery confirmation and warranty activation.  

---

### UC-42 Track Delivery Progress and History

**Primary Actors**  
Sales Staff, Store Owner, Customer  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, Store Owner, or Customer, I want to track the overall shipment status, delivery timestamps, driver locations, and proof-of-delivery photos so that delivery inquiries can be monitored and verified accurately.  

**Preconditions**  
1. The user has access to the Order or Dispatch tracking view.  
2. The delivery trip has been created.  

**Normal Flow**  
1. The user opens the "Delivery Tracking" tab on an order or dispatch record.  
2. The system renders the delivery tracking card:  
   - Trip Code, Assigned Shipper Name, Vehicle Plate Number, Shipper Phone Number.  
   - Milestone Timeline: Dispatched at Warehouse -> Arrived at Site -> Successfully Delivered.  
   - Handover timestamp (e.g., 14:35, 13/10/2026).  
   - Proof of Delivery: Embedded high-resolution photos of fixtures at the site and customer's digital signature.  
   - Financial Summary: Shipping fee, COD amount collected.  
3. The user inspects the proof and timestamps.  

**Alternative Flows**  
- **Step 2 - Failed Delivery History**:  
  1. The delivery previously failed on Attempt 1.  
  2. The system displays the historical attempt log: Failure Reason ("Site gate locked"), photo evidence, and reschedule timestamp for Attempt 2.  

**Postconditions**  
1. Delivery audit history is presented to the user without altering database state.  

---

### UC-43 Look up Assigned Delivery Trips

**Primary Actors**  
Shipper  

**Secondary Actors**  
None  

**Description**  
As a Shipper, I want to look up my daily assigned delivery runs, package quantities, destination addresses, customer contact hotlines, route sequence, and COD cash amounts so that I can organize my delivery route efficiently.  

**Preconditions**  
1. The Shipper is logged into the system.  
2. At least one delivery run has been assigned to the Shipper.  

**Normal Flow**  
1. The Shipper opens the "My Delivery Runs" screen on their mobile device.  
2. The system loads today's active trips assigned to the logged-in Shipper: Trip Code, Departure Time, Total Stops (e.g., 4 stops), Total Packages, and Total COD to Collect (e.g., 25,400,000 VND).  
3. The Shipper taps on an active trip to view the full itinerary.  
4. The system displays sequential stops in order:  
   - Stop 1: Mr. Nam - 12 Mai Chi Tho, District 2 (1 Smart Toilet, 1 Basin; COD: 12,000,000 VND; "Call 15 mins ahead").  
   - Stop 2: Mrs. Lan - 45 Thao Dien, District 2 (2 Showers; COD: 0 VND - Paid Online).  
   - One-tap button to call customer phone number.  
   - One-tap button to open Google Maps navigation to destination.  
5. The Shipper reviews the manifest, loads boxes onto the truck, and taps "Start Trip".  
6. The system marks the trip as `In_Transit` and updates all linked orders.  

**Alternative Flows**  
- **Step 2 - No Assigned Trips Available**:  
  1. The Shipper has no active trips assigned for the day.  
  2. The system displays MSG81: "You have no assigned delivery trips at this time. Check back later or contact the dispatch coordinator."  
- **Step 4 - Re-order Route Stops**:  
  1. The Shipper encounters local road construction and prefers to deliver Stop 2 before Stop 1.  
  2. The Shipper drags and reorders stops on the run sheet.  
  3. The system saves the revised sequence.  

**Postconditions**  
1. The Shipper accesses route and customer details.  
2. Trip status transitions to `In_Transit` upon departure.  
