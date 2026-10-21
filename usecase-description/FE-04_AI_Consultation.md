# MODULE FE-04: AI CONSULTATION & RECOMMENDATION

---

### UC-14 AI Consultation and Product Recommendation

**Primary Actors**  
Guest, Customer  

**Secondary Actors**  
OpenAI, Cloudinary  

**Description**  
As a Customer or Guest, I want to interact with an AI consultation assistant to describe my bathroom layout, budget, design style, and technical plumbing conditions (or upload architectural drawings/photos) so that I receive personalized sanitary ware recommendations and layout advice.  

**Preconditions**  
1. The AquaSuite storefront is accessible.  
2. The AI consultation chat widget is active and integrated with OpenAI.  

**Normal Flow**  
1. The user opens the "AI Bathroom Consultant" chat interface on the storefront.  
2. The system initiates a consultation session, prompts the user with guided questions (Bathroom Dimensions, Budget Tier: Economy, Mid-range, Luxury; Design Aesthetic: Modern Minimalist, Classic European, Japandi; Piping constraints: Floor drain distance, Water pressure).  
3. The user inputs their requirements via natural language text (e.g., "I have a 6m² master bathroom with a budget of 50 million VND. I want a one-piece toilet, black thermostatic shower, and a modern vanity cabinet").  
4. The user optionally uploads a floor plan drawing or photo of their current bathroom layout.  
5. The system saves the uploaded image to Cloudinary and retrieves the image URL.  
6. The system compiles the user prompt, image URL, and real-time product catalog context into an engineered prompt and calls the OpenAI API.  
7. OpenAI processes the prompt and returns structured recommendations:  
   - Analysis of space zoning (Wet zone vs. Dry zone).  
   - Recommended SKU fixture list matching budget and specifications (e.g., Inax AC-902VN Toilet, Grohe Thermostatic Shower, AquaSuite Custom Vanity).  
   - Detailed justification (water saving, anti-stain glaze, dimensional compatibility).  
8. The system parses the AI response and displays an interactive chat message containing product cards with thumbnail photos, specifications, real-time pricing, stock status, and "Add to Cart" or "Request Showroom Consultation" buttons.  
9. The user reviews recommendations and may ask follow-up questions or request alternative fixtures.  
10. The system logs the entire conversation thread under a unique `ConsultationSessionID`.  

**Alternative Flows**  
- **Step 6 - OpenAI Service Latency or Rate Limit**:  
  1. The OpenAI API call times out (> 15 seconds) or returns a rate-limit error.  
  2. The system falls back to a rule-based recommendation engine matching category tags and budget filters.  
  3. The system displays MSG45: "AI is currently experiencing high demand. Here are recommended fixture packages based on your criteria:" followed by pre-configured curated packages.  
- **Step 3 - Budget Insufficient for Desired High-End Fixtures**:  
  1. The user requests luxury smart bidet fixtures with a very low budget.  
  2. The AI politely clarifies the cost breakdown, recommends cost-effective alternative models (e.g., manual bidet washlet instead of full smart toilet), and explains trade-offs.  
- **Step 8 - Guest Requests Sales Staff Follow-up**:  
  1. The user clicks "Request Human Sales Expert Consultation".  
  2. The system prompts the user to enter their Name and Phone Number (or logs in if existing Customer).  
  3. The system transitions the consultation session status to `Awaiting_Staff_Review` and alerts the Sales Staff on the CRM dashboard.  

**Postconditions**  
1. The consultation session, user requirements, and recommended items are recorded in the database.  
2. If contact info is submitted, a lead is generated for Sales Staff in FE-04 / FE-10.  

---

### UC-15 Manage Consultation Requests and History

**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to review, filter, assign, and inspect past customer AI consultation transcripts and recommended product lists so that sales staff can proactively contact potential customers with personalized bathroom advice.  

**Preconditions**  
1. The Sales Staff or Store Owner is logged in with CRM/Sales permissions.  
2. The "AI Consultation Management" screen is accessible in the Back-Office Portal.  

**Normal Flow**  
1. The Sales Staff opens the "AI Consultations & Inquiries" menu.  
2. The system displays a list of consultation sessions: Session ID, Customer Name/Guest Phone, Consultation Date/Time, Budget Tier, Space Dimensions, Lead Status (`New`, `In Progress`, `Quote Sent`, `Closed - Won`, `Closed - Lost`), and Assigned Staff.  
3. The Sales Staff filters by Status = `New` or searches by Customer Phone Number.  
4. The Sales Staff selects an inquiry and clicks "View Transcript".  
5. The system displays the full conversation history, including user inputs, uploaded architectural drawings, and the AI's recommended sanitary ware items.  
6. The Sales Staff clicks "Assign to Me" or assigns the lead to a team member.  
7. The system updates the session status to `In Progress` with the assigned staff member's ID and timestamp.  
8. The system displays MSG46: "Consultation lead assigned successfully."  

**Alternative Flows**  
- **Step 5 - Add Internal Sales Notes**:  
  1. The Sales Staff calls the customer to clarify technical requirements (e.g., confirming water pipe center distance is 300mm).  
  2. The Sales Staff inputs notes into the "Staff Consultation Notes" box and clicks "Save Note".  
  3. The system logs the note with the staff member's name and timestamp.  
- **Step 2 - Mark Lead as Inactive / Closed - Lost**:  
  1. Customer unreachable or decides not to build bathroom fixtures at this time.  
  2. The Sales Staff updates status to `Closed - Lost` with a reason (e.g., "Out of budget", "Purchased elsewhere").  
  3. The system updates status and displays MSG47: "Consultation status updated to Closed - Lost."  

**Postconditions**  
1. The consultation session status, assignment, and internal notes are updated in the database.  
2. Real-time lead metrics on the CRM dashboard are updated.  

---

### UC-16 Convert Consultation to Quotation or Order

**Primary Actors**  
Sales Staff  

**Secondary Actors**  
Email Service  

**Description**  
As a Sales Staff, I want to convert an AI consultation transcript and recommended item list directly into a formal project quotation or a draft sales order so that the customer can review pricing, sign off, or complete purchase without manual data re-entry.  

**Preconditions**  
1. The Sales Staff is reviewing an active consultation session (UC-15).  
2. The customer's contact information (Name, Phone Number, Email) is recorded.  

**Normal Flow**  
1. The Sales Staff clicks the "Convert to Quotation" button on the consultation detail screen.  
2. The system retrieves customer profile info and all recommended product SKUs from the AI transcript.  
3. The system initializes a new Project Quotation draft in the Sales module (FE-06), pre-populating:  
   - Customer Name, Phone, and Email.  
   - Project Site Name (e.g., "Master Bathroom Renovation").  
   - Quotation line items matching the recommended fixtures with real-time unit prices, tax rates, and warehouse availability.  
4. The Sales Staff reviews and adjusts the draft: fine-tunes quantities, applies negotiated project discount percentages (e.g., 5% bundle discount), and adds optional installation services.  
5. The system calculates subtotal, discount, VAT, and grand total.  
6. The Sales Staff clicks "Finalize & Send Quotation".  
7. The system generates an official PDF quotation document stamped with AquaSuite store details.  
8. The system triggers the Email Service to deliver the quote PDF and an approval link directly to the customer's email.  
9. The system updates the consultation session status to `Quote Sent` and links the new `QuotationID`.  
10. The system displays MSG48: "Quotation created and emailed to customer successfully."  

**Alternative Flows**  
- **Step 1 - Direct Conversion to POS / Draft Sales Order**:  
  1. If the customer is at the showroom counter or ready to purchase immediately, the Sales Staff clicks "Convert to Sales Order".  
  2. The system imports recommended SKUs directly into a POS order draft (UC-21).  
  3. The Sales Staff proceeds with immediate payment or deposit collection.  
- **Step 3 - Recommended SKU Out of Stock**:  
  1. The system identifies that a recommended SKU has zero inventory.  
  2. The system flags the item row: "Warning: SKU is currently out of stock."  
  3. The Sales Staff clicks "Replace with Equivalent Item" and selects an in-stock alternative model.  
  4. The system replaces the line item and recalculates totals.  

**Postconditions**  
1. A formal Quotation (or Sales Order) is created in FE-06 and linked to the AI consultation session.  
2. An official quotation email is transmitted to the customer.  
3. The activity is recorded in the customer's interaction history (FE-10).  
