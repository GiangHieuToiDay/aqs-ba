# MODULE FE-10: CUSTOMER RELATIONSHIP MANAGEMENT (CRM)

---

### UC-49 Manage Customer Profiles and Account Linking

**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to create, update, and manage customer CRM profiles, classify customer tiers (Retail Homeowner, Interior Contractor, Real Estate Developer), and link customer records to online portal accounts so that customer identity and commercial terms are unified.  

**Preconditions**  
1. The user is authenticated with CRM management permissions.  
2. The Customer CRM screen is accessible in the Back-Office system.  

**Normal Flow**  
1. The user opens the "Customer CRM Management" screen.  
2. The system displays a paginated list of customers: Customer ID, Full Name, Phone Number, Email, Customer Group/Tier (`Retail`, `Contractor VIP`, `Architect / Designer`), Total Orders, Lifetime Spend, Outstanding Debt, and Linked Account Status (`Linked`, `Unlinked`).  
3. The user clicks "Create Customer Profile".  
4. The system opens the profile creation form with fields: Full Name, Phone, Email, Tax Code (for corporate contractors), Billing Address, Customer Classification, and Payment Terms.  
5. The user enters customer data, selects Classification = `Interior Contractor`, and assigns a default 10% wholesale discount policy.  
6. The user clicks "Save Customer".  
7. The system validates uniqueness of the Phone Number and Tax Code, creates the CRM profile record, and displays MSG87: "Customer profile created successfully."  

**Alternative Flows**  
- **Step 2 - Link CRM Profile to Existing Online User Account**:  
  1. A walk-in showroom contractor subsequently registers an online account on the storefront.  
  2. The Sales Staff clicks "Link Online Account" on the CRM profile.  
  3. The system searches for matching email/phone in the user database and binds the online user account to the existing CRM profile.  
  4. The system synchronizes order histories and discount tiers and displays MSG88: "CRM profile successfully linked to online user account."  
- **Step 2 - Update Customer Classification Tier**:  
  1. A homeowner completes multiple villa renovations and reaches the lifetime spending milestone (> 100,000,000 VND).  
  2. The Store Owner upgrades the customer tier to `VIP Gold`.  
  3. The system updates the tier and automatically applies preferential pricing rules to future quotations.  

**Postconditions**  
1. Customer master CRM record is updated in the database.  
2. Commercial discount tiers and linked online credentials are synchronized.  

---

### UC-50 Look up Customer History and Purchases

**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to review past purchase orders, quotation proposals, total lifetime spend, payment records, and service tickets of a customer so that consultations and personalized offers can be tailored accurately.  

**Preconditions**  
1. The user has CRM viewing permissions.  
2. The target customer exists in the system.  

**Normal Flow**  
1. The Sales Staff opens the Customer CRM screen and searches by Customer Name or Phone Number (e.g., "Tran Minh Tri - 0988776655").  
2. The system displays matching search results. The Sales Staff clicks "View 360° Dossier".  
3. The system renders the comprehensive Customer 360 Dashboard containing:  
   - Header Summary: Total Orders Count (8 orders), Total Lifetime Spend (420,000,000 VND), Current Outstanding Debt (15,000,000 VND), Customer Tier (`VIP Contractor`).  
   - Tab 1: Orders History (Chronological list of POS and online orders with status and line items).  
   - Tab 2: Quotations History (Active, converted, or expired project quotes).  
   - Tab 3: Construction Projects (List of registered villa/apartment job-sites).  
   - Tab 4: Warranty & Service Tickets (Active fixtures, repairs, service history).  
   - Tab 5: AI Consultation Transcripts (Past AI chatbot discussions).  
4. The Sales Staff reviews past purchases to recommend complementary fittings (e.g., customer previously purchased TOTO Neorest toilets and now requires matching wall-hung bidet washlets).  
5. The Sales Staff proceeds with the new consultation.  

**Alternative Flows**  
- **Step 3 - Export Customer Statement / Activity Report**:  
  1. The Sales Staff clicks "Export Customer Purchase History (PDF/Excel)".  
  2. The system generates and downloads a consolidated statement of all orders, deliveries, and payments.  

**Postconditions**  
1. Complete customer interaction history is presented without altering system data.  

---

### UC-51 Manage Customer Construction Projects

**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to register and track multiple construction project sites (e.g., Riverside Villa Block B, Sunrise Hotel Suites, Townhouse 42) under a single customer or contractor profile so that quotations, shipments, and warranty cards can be partitioned by job-site.  

**Preconditions**  
1. The customer profile exists in CRM (UC-49).  
2. The user has project tracking permissions.  

**Normal Flow**  
1. The user opens the customer's CRM profile and navigates to the "Construction Sites / Projects" tab.  
2. The system displays existing registered construction sites: Project Code, Project Name, Site Address, On-Site Contact Supervisor, Phone Number, Current Construction Phase (`Foundation`, `Rough-in Plumbing`, `Tiling`, `Finishing / Fixture Installation`), and Total Fixtures Delivered.  
3. The user clicks "Add Construction Site".  
4. The system opens the site registration form with fields:  
   - Project Name: "Villa 22 - Thao Dien Compound".  
   - Site Address: Street address, Ward, District, Province.  
   - Site Superintendent / Foreman Name and Direct Mobile Phone.  
   - Site Access Restrictions (e.g., "Trucks over 2.5 tons restricted between 6 AM - 8 PM; elevator access requires booking 1 day in advance").  
   - Estimated Fixture Installation Date.  
5. The user enters site details and clicks "Save Project Site".  
6. The system validates the inputs, associates the site with the customer profile, and displays MSG89: "Construction project site registered successfully."  

**Alternative Flows**  
- **Step 2 - Update Construction Phase**:  
  1. The contractor notifies that tiling is completed and the site is ready for ceramic installation (`Finishing`).  
  2. The Sales Staff updates the site phase to `Finishing / Fixture Installation`.  
  3. The system updates the phase and prompts to schedule delivery dispatch for staged fixtures (FE-08 UC-40).  
  4. The system displays MSG90: "Project construction phase updated."  

**Postconditions**  
1. Construction site record is saved and linked to the customer.  
2. Site address and access notes become available as target delivery destinations in FE-06 and FE-08.  
