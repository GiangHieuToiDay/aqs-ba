# MODULE FE-13: TECHNICAL SERVICES

---

### UC-59 Look up Technical Jobs and Schedule

**Primary Actors**  
Technical Staff  

**Secondary Actors**  
None  

**Description**  
As a Technical Staff, I want to view my assigned schedule of bathroom site surveys, sanitary ware installations, and warranty repair jobs along with customer site addresses, plumbing requirements, and fixture specifications so that I can prepare specialized tools and plan my itinerary.  

**Preconditions**  
1. The Technical Staff is logged into the AquaSuite mobile portal.  
2. At least one technical service job has been assigned to the technician.  

**Normal Flow**  
1. The Technical Staff opens the "My Technical Schedule" screen.  
2. The system renders a calendar and list view of assigned work orders for the current day and upcoming week: Work Order ID (e.g., `WO-TEC-20261013-0004`), Job Type (`Installation`, `Plumbing Survey`, `Warranty Repair`, `Emergency Leak Service`), Scheduled Date & Time Slot (e.g., 14:00 - 16:30, 13/10/2026), Customer Name, Site Address, and Priority (`High`, `Normal`).  
3. The technician taps on a scheduled job to inspect full work order details.  
4. The system displays:  
   - Detailed job description and service objectives (e.g., "Install TOTO Neorest Smart Toilet, verify 220V electrical socket ground, and test water supply pressure at 0.15 MPa").  
   - Linked Sales Order or Warranty Ticket (FE-09 UC-46).  
   - Sanitary ware fixtures and SKU codes to be installed/inspected.  
   - Site contact details, building gate guidelines, and map location pin.  
   - Required specialized tool checklist (Flange sealing wax, pressure gauge, pipe thread sealant tape, silicone gun).  
5. The technician reviews the job specifications and confirms tool preparedness.  

**Alternative Flows**  
- **Step 2 - Filter by Job Type or Status**:  
  1. The technician filters the calendar by "Warranty Repairs Only" or "Awaiting Acceptance".  
  2. The system updates the calendar view to highlight matching appointments.  
- **Step 2 - Schedule Conflict Alert**:  
  1. The system detects two overlapping job assignments within the same time window.  
  2. The system highlights the conflicting cards with a yellow warning badge and prompts the technician to alert the service dispatcher for rescheduling.  

**Postconditions**  
1. Assigned technical tasks and specifications are reviewed by the technician.  

---

### UC-60 Accept and Execute Technical Jobs

**Primary Actors**  
Technical Staff  

**Secondary Actors**  
None  

**Description**  
As a Technical Staff, I want to acknowledge job assignments, update my en-route and arrival status at the customer's site, inspect on-site water pressure and rough-in plumbing compatibility, and execute installation/repair work so that technical service execution is tracked in real-time.  

**Preconditions**  
1. The Technical Staff is scheduled for an assigned job (UC-59).  
2. The technician is preparing to travel to the customer's site.  

**Normal Flow**  
1. The technician opens the scheduled job on their mobile interface and taps "Accept & Depart for Site".  
2. The system updates the work order status to `En_Route` and timestamps the departure.  
3. Upon arriving at the customer's location, the technician taps "Arrived at Site".  
4. The system updates the work order status to `Arrived / In_Progress`.  
5. The technician conducts the Pre-Installation Plumbing Survey:  
   - Measures water supply pressure using a pressure gauge (verifying within 0.05 - 0.75 MPa specification).  
   - Verifies wastewater rough-in center distance from finished wall (checking 305mm dimension).  
   - Checks electrical socket grounded status for smart bidet plug.  
6. The technician logs survey measurements into the pre-installation checklist on the mobile screen.  
7. The technician proceeds with physical fixture installation / repair according to manufacturer technical guidelines:  
   - Positions mounting flange and wax ring.  
   - Mounts ceramic toilet bowl and torques closet bolts evenly.  
   - Connects water supply stop-valve with flexible braided hose.  
   - Plugs in electrical bidet seat and executes automated nozzle test cycle.  
   - Applies mildew-resistant silicone sealant along the base perimeter.  
8. The technician updates the work order progress milestone to "Installation Completed - Testing in Progress".  

**Alternative Flows**  
- **Step 5 - Plumbing / Site Incompatibility Detected**:  
  1. The rough-in pipe center is measured at 250mm (incompatible with standard 305mm fixture without offset flange), or water pressure is too low (< 0.05 MPa).  
  2. The technician halts installation and taps "Report Technical Incompatibility".  
  3. The technician inputs details and takes photos of the misaligned pipe.  
  4. The system flags the job with status `Suspended_Pending_Piping_Adjustment`, notifies Sales Staff and Customer, and recommends an offset connector or booster pump.  
- **Step 7 - Additional Replacement Parts Required**:  
  1. During a warranty repair, the technician discovers the solenoid water inlet valve is burned out.  
  2. The technician clicks "Add Spare Part Used" and enters SKU `PART-VALVE-01` from van stock.  
  3. The system logs the part consumption against the warranty ticket.  

**Postconditions**  
1. Work order status is updated to `In_Progress` with location and survey measurements recorded.  
2. Spare parts consumed are registered in the technical service log.  

---

### UC-61 Record Results and Complete Technical Jobs

**Primary Actors**  
Technical Staff  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Technical Staff, I want to record completion test results, upload high-resolution photos of the installed fixtures and clean job site, obtain the customer's digital acceptance signature, and submit the completed service ticket so that the work order is formally finalized and closed.  

**Preconditions**  
1. The physical installation or warranty repair work has been completed (UC-60).  
2. The technician and customer are present at the site for final handover.  

**Normal Flow**  
1. The technician conducts the final Handover Quality Checklist on the mobile app:  
   - Verified zero leaks at water inlet stopcock and waste trapway.  
   - Verified flush cycle and dual-flush buttons operating normally.  
   - Verified smart bidet seat heating, water temperature, and nozzle spray functions.  
   - Cleaned ceramic surface and cleared packing debris.  
2. The technician checks each item on the digital test checklist.  
3. The technician captures clear photos of the installed sanitary fixtures in operation and the clean bathroom environment.  
4. The system uploads the photos to Cloudinary and attaches the image URLs to the work order.  
5. The technician demonstrates operation of the smart fixture to the customer and presents the mobile sign-off screen.  
6. The customer inspects the installation, signs their digital signature on the screen, and rates service satisfaction (e.g., 5 Stars).  
7. The technician taps "Submit Handover & Close Job".  
8. The system updates the work order status to `Completed`, sets the linked Warranty Ticket / Order Installation milestone to `Completed`, and logs the completion timestamp.  
9. The system triggers an automated service completion notice to the customer and notifies the Store Owner.  
10. The system displays MSG94: "Technical job completed and customer sign-off recorded successfully."  

**Alternative Flows**  
- **Step 6 - Customer Notes Defect or Minor Adjustment Needed**:  
  1. Customer requests re-aligning the toilet paper holder or adjusting water spray pressure setting.  
  2. The technician performs the requested adjustment on the spot.  
  3. The customer verifies the adjustment and proceeds with signature.  
- **Step 1 - Customer Not Present for In-Person Sign-Off (Tenant / Contractor Off-Site)**:  
  1. The project superintendent is away from the site.  
  2. The technician selects "Remote Sign-Off", uploads comprehensive video of the functioning fixture, and submits.  
  3. The system emails an electronic acceptance link to the homeowner for remote sign-off within 24 hours.  

**Postconditions**  
1. Work order status is set to `Completed`.  
2. Proof of installation photos and digital signature are permanently archived.  
3. Linked warranty tickets or order installation milestones are closed.  
