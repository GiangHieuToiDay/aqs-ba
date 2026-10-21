# MODULE FE-09: WARRANTY MANAGEMENT

---

### UC-44 Manage Warranty Policies

**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to define and update warranty policies, warranty periods (e.g., 10-year ceramic glaze warranty, 2-year electronic toilet bidet seat warranty, 5-year solid brass faucet valve warranty), and coverage conditions for each brand and category so that warranty terms are transparent and standardized.  

**Preconditions**  
1. The Store Owner is authenticated with administrative permissions.  
2. The Warranty Policy Configuration screen is accessible.  

**Normal Flow**  
1. The Store Owner opens the "Warranty Policies" management screen.  
2. The system displays active warranty policies grouped by Category and Brand: Policy Code, Policy Name, Brand (TOTO, Inax, Viglacera), Coverage Duration (Months), Coverage Scope, Exclusions, and Status (`Active`, `Inactive`).  
3. The Store Owner clicks "Create Warranty Policy".  
4. The system opens the policy builder form.  
5. The Store Owner inputs:  
   - Policy Code (e.g., `POL-TOTO-CERAMIC-10Y`).  
   - Policy Name: "TOTO 10-Year Ceramic Glaze Warranty".  
   - Linked Category: "Toilets", "Washbasins".  
   - Linked Brand: "TOTO".  
   - Duration: 120 Months (10 Years).  
   - Coverage Scope: "Cracking, crazing, or peeling of CeFiONtect ceramic glaze under normal domestic usage."  
   - Exclusions: "Impact fractures caused by heavy objects, chemical acid corrosion, or unauthorized alterations."  
6. The Store Owner clicks "Save Policy".  
7. The system validates the inputs, saves the policy, links it to corresponding product SKUs, and displays MSG82: "Warranty policy created and linked successfully."  

**Alternative Flows**  
- **Step 2 - Edit Warranty Policy**:  
  1. The Store Owner modifies the warranty duration or coverage text for an existing policy.  
  2. The system updates the policy for all future warranty issuances while preserving historical warranty terms for previously issued orders.  
  3. The system displays MSG83: "Warranty policy updated successfully."  

**Postconditions**  
1. Warranty policy master record is stored in the database.  
2. Newly created sales orders inherit these terms when generating e-warranty records.  

---

### UC-45 Manage Warranty Records

**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to create, search, and manage electronic warranty cards (e-warranty records) linked to product serial numbers, order IDs, and customer phone numbers so that warranty eligibility can be audited and proven.  

**Preconditions**  
1. A Sales Order has been delivered to a Customer, or the staff is creating an ad-hoc e-warranty card.  
2. The user has warranty administration permissions.  

**Normal Flow**  
1. The user navigates to the "E-Warranty Records" screen.  
2. The system displays a paginated list of electronic warranty cards: Warranty Code (e.g., `WAR-202610-0089`), Product SKU & Name, Serial Number / Barcode, Customer Name, Phone Number, Order Code, Activation Date, Expiry Date, and Status (`Active`, `Expired`, `Claimed`, `Voided`).  
3. The user searches for a warranty record by Customer Phone Number or Serial Number.  
4. The system displays matching warranty records with remaining days of coverage.  
5. The user clicks on a record to view its full dossier:  
   - Customer profile details and installation site address.  
   - Associated order invoice and delivery receipt.  
   - Applicable warranty policy terms.  
   - Service and repair history log.  
6. The user verifies the record details and status.  

**Alternative Flows**  
- **Step 1 - Automated E-Warranty Creation upon Delivery**:  
  1. When an order transitions to `Delivered` in FE-08 (UC-41), the system automatically initializes e-warranty records for each qualifying fixture.  
  2. The system calculates expiration dates based on the active policy (e.g., Delivery Date + 120 months for ceramics).  
  3. The system activates the e-warranty card and attaches it to the customer's portal account.  
- **Step 5 - Manually Void Warranty Card**:  
  1. An inspection reveals that a fixture was maliciously damaged or installed in violation of electrical/water safety guidelines.  
  2. The Store Owner clicks "Void Warranty", inputs reason: "Voided due to unauthorized modification / acid wash damage", and confirms.  
  3. The system updates the status to `Voided` and logs the action in the Activity Log.  

**Postconditions**  
1. E-warranty records are managed in the database.  
2. Claim eligibility is systematically determined by the record status.  

---

### UC-46 Receive and Process Warranty Claims

**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
Email Service, SMS Service  

**Description**  
As a Sales Staff or Store Owner, I want to receive customer warranty claims, evaluate policy eligibility, dispatch technical staff for on-site inspection/repair, and update resolution records so that customer issues are handled professionally.  

**Preconditions**  
1. A warranty claim has been submitted by a Customer (UC-47) or reported by phone at the showroom.  
2. The staff user has warranty service handling permissions.  

**Normal Flow**  
1. The Sales Staff opens the "Warranty Claims & Support Tickets" queue.  
2. The system displays pending claims: Ticket ID (e.g., `TCK-WAR-202610-0012`), Customer Name, Phone, Fixture Description, Reported Issue (e.g., "Smart toilet remote control unresponsive and auto-flush failing"), Submission Date, and Status (`New`, `Inspecting`, `In_Repair`, `Resolved`, `Rejected`).  
3. The Sales Staff opens the claim details and reviews the linked e-warranty record, proof photos, and video of the malfunction.  
4. The system verifies that the warranty period is currently `Active`.  
5. The Sales Staff clicks "Accept Claim & Dispatch Technical Staff".  
6. The system prompts to assign a Technical Staff member (FE-13 UC-60) and schedule an on-site visit appointment (e.g., 14/10/2026 at 10:00 AM).  
7. The Sales Staff confirms the assignment.  
8. The system creates a Technical Work Order linked to the claim, updates claim status to `Inspecting`, and dispatches an appointment confirmation to the customer via SMS Service and Email Service.  
9. The system displays MSG84: "Warranty claim accepted and technical staff dispatched."  

**Alternative Flows**  
- **Step 4 - Warranty Expired or Excluded Damage (Reject Claim)**:  
  1. The system detects that the warranty expired 3 months ago, or the uploaded photos clearly show exterior physical impact crack.  
  2. The Sales Staff clicks "Reject Claim" or "Offer Paid Repair Service".  
  3. The Sales Staff inputs rejection rationale and optional quotation for replacement parts.  
  4. The system sends a formal notification to the customer explaining the policy grounds.  
- **Step 2 - Phone Intake of Warranty Claim**:  
  1. A customer calls the showroom hotline to report a leaking thermostatic shower mixer.  
  2. The Sales Staff clicks "Create Manual Warranty Ticket", looks up customer phone number, selects the purchased shower SKU, types the problem description, and proceeds to Step 5.  

**Postconditions**  
1. Warranty ticket status is updated to `Inspecting` or `Rejected`.  
2. A technical service job is generated in FE-13.  
3. Customer receives appointment confirmation.  

---

### UC-47 Submit Online Warranty Claim

**Primary Actors**  
Customer  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Customer, I want to submit an online warranty claim through my account portal by selecting my registered fixture, describing the defect, and uploading photos/videos of the malfunction so that the store can provide warranty repair support.  

**Preconditions**  
1. The Customer is logged into the AquaSuite customer portal.  
2. The Customer has at least one active e-warranty record.  

**Normal Flow**  
1. The Customer navigates to "My Warranties" in their portal.  
2. The system displays all registered fixtures with their active warranty badges and expiration dates.  
3. The Customer clicks "Request Warranty Support" on a specific fixture (e.g., "TOTO Neorest Smart Toilet").  
4. The system opens the Warranty Claim Submission form pre-populating: Product Name, Serial Number, Purchase Order Date, and Customer Contact Details.  
5. The Customer enters:  
   - Detailed Description of the Issue: "Bidet nozzle fails to extend when wash button is pressed; red indicator light flashes."  
   - Preferred Appointment Time for site visit (e.g., Morning, 15/10/2026).  
   - Current Site Address.  
6. The Customer uploads photo and short video files demonstrating the fault.  
7. The system uploads media assets to Cloudinary and validates file formats.  
8. The Customer clicks "Submit Warranty Claim".  
9. The system creates the warranty ticket with status `New`, generates Ticket Code `TCK-WAR-202610-0014`, and logs the claim.  
10. The system displays MSG85: "Your warranty request has been submitted. Our technical team will contact you within 4 business hours."  

**Alternative Flows**  
- **Step 2 - Warranty Expired**:  
  1. The Customer selects a fixture whose warranty has expired.  
  2. The system flags: "Warranty has expired on 01/08/2026. Would you like to request Out-of-Warranty Paid Repair Service?"  
  3. The Customer accepts, and the system submits an out-of-warranty service request.  

**Postconditions**  
1. A new warranty ticket record is registered in FE-09.  
2. Media files are stored in Cloudinary and linked to the ticket.  

---

### UC-48 Look up Warranty Information

**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
None  

**Description**  
As a Customer or Guest, I want to look up warranty coverage, active duration, and terms using my phone number, order code, or product serial number so that I can verify warranty status without needing to find paper warranty certificates.  

**Preconditions**  
1. The public "E-Warranty Lookup" page on the storefront is accessible.  

**Normal Flow**  
1. The user navigates to the public "Check Warranty" page.  
2. The system renders the lookup form offering search criteria: Phone Number, Order Code, or Fixture Serial Number.  
3. The user inputs their Phone Number (e.g., `0901234567`) and clicks "Search Warranty".  
4. The system searches active e-warranty records matching the phone number.  
5. The system displays a sanitized summary of all registered sanitary fixtures:  
   - Product Name (e.g., "Inax Smart One-Piece Toilet AC-1035").  
   - Serial Number (masked: `SN-INX-***-882`).  
   - Installation Date: 15/01/2026.  
   - Warranty Period: 10 Years Ceramic / 2 Years Fittings.  
   - Current Status: `Active - 9 Years Remaining`.  
   - Coverage Scope: Official factory warranty coverage summary.  
6. The user reviews their warranty coverage.  

**Alternative Flows**  
- **Step 4 - No Warranty Records Found**:  
  1. No matching records are found for the entered query.  
  2. The system displays MSG86: "No warranty records found for this information. Please verify your phone number or contact our hotline for assistance."  

**Postconditions**  
1. Warranty status is presented to the user without exposing sensitive private details.  
