# MODULE FE-05: ONLINE SHOPPING & ORDER TRACKING

---

### UC-17 Manage Shopping Cart

**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
None  

**Description**  
As a Customer or Guest, I want to add sanitary ware fixtures to my shopping cart, update quantities, choose product variants, and inspect price summaries so that I can prepare my purchase before checkout.  

**Preconditions**  
1. The user is browsing the AquaSuite storefront.  
2. Active products and SKU variants are available in the catalog.  

**Normal Flow**  
1. The user selects a sanitary ware item (e.g., "Wall-hung Basin Sink", Variant: "Matte Black") and clicks "Add to Cart".  
2. The system checks real-time available inventory and adds the item to the cart (stored in LocalStorage for Guests, or synced with DB cart for Customers).  
3. The system opens the mini-cart drawer showing the newly added fixture, unit price, quantity, and updated subtotal.  
4. The user clicks "View Cart" to access the full Shopping Cart page.  
5. The system displays all cart items with thumbnail image, SKU name, variant attributes, unit price, quantity increment/decrement controls, total line amount, and order subtotal.  
6. The user adjusts the quantity of an item (e.g., changes from 1 to 2).  
7. The system verifies that the requested quantity does not exceed available warehouse stock, recalculates line totals and grand total, and updates the cart state.  
8. The system displays MSG49: "Shopping cart updated successfully."  

**Alternative Flows**  
- **Step 2 - Requested Quantity Exceeds Available Stock**:  
  1. The user attempts to add more units than currently in stock.  
  2. The system sets the quantity to the maximum available and displays MSG50: "Only X units are currently available in stock. Quantity adjusted."  
- **Step 5 - Remove Item from Cart**:  
  1. The user clicks the trash icon / "Remove" on an item.  
  2. The system prompts for confirmation, removes the SKU from the cart, recalculates subtotal, and displays MSG51: "Item removed from cart."  
- **Step 5 - Apply Promotional Voucher**:  
  1. The user enters a coupon code (e.g., `SUMMERBATH10`) and clicks "Apply".  
  2. The system verifies coupon validity (minimum order value, validity dates, category restrictions).  
  3. The system applies the discount, updates the grand total, and displays MSG52: "Promotional discount applied successfully."  
- **Step 5 - Empty Cart State**:  
  1. All items are removed from the cart.  
  2. The system displays an empty cart banner with a "Continue Shopping" button directing to top categories.  

**Postconditions**  
1. Cart contents and selected quantities are saved in the user's session or database profile.  

---

### UC-18 Place Online Order and Payment

**Primary Actors**  
Customer  

**Secondary Actors**  
Email Service, SMS Service  

**Description**  
As a registered Customer, I want to checkout my shopping cart, select a delivery address and shipping method, choose a payment method (VietQR / Bank Transfer or Cash on Delivery - COD), and submit my order so that my sanitary ware purchase is placed and confirmed.  

**Preconditions**  
1. The Customer is logged into their account.  
2. The shopping cart contains at least one valid item.  

**Normal Flow**  
1. The Customer clicks "Proceed to Checkout" from the shopping cart.  
2. The system renders the Checkout screen with three main sections:  
   - Shipping Address: Displays default address from address book with option to choose another or add a new address.  
   - Shipping & Installation Options: Standard delivery, Scheduled delivery for construction site, Optional installation request.  
   - Order Review & Payment Method: VietQR Instant Transfer, Direct Bank Transfer, Cash on Delivery (COD - applicable for orders under 20,000,000 VND).  
3. The Customer selects their delivery address (e.g., "Villa 14, Riverside Project").  
4. The Customer selects "VietQR Instant Transfer" as the payment method.  
5. The system calculates shipping fee based on delivery district and heavy sanitary ware weight, summarizes VAT, subtotal, and total amount payable.  
6. The Customer clicks "Place Order & Pay".  
7. The system creates a new sales order with status `Pending_Payment`, generates a unique Order Code (e.g., `ORD-20261013-0012`), and locks/reserves the required inventory quantities.  
8. The system renders a dynamic VietQR payment modal featuring a standardized EMVCo QR code containing the exact order amount, store bank account, and transfer memo format (`ORD202610130012`).  
9. The Customer scans the QR code via their mobile banking app and authorizes the transfer.  
10. The system detects the incoming bank transaction via webhook / verification service, updates the order status to `Payment_Confirmed`, and transitions the order to the sales queue (FE-06).  
11. The system triggers the Email Service and SMS Service to dispatch order confirmation and receipt details.  
12. The system clears the Customer's cart, displays MSG53: "Order placed and payment confirmed successfully!", and redirects to the Order Tracking page.  

**Alternative Flows**  
- **Step 4 - Select Cash on Delivery (COD)**:  
  1. The Customer selects "Cash on Delivery (COD)".  
  2. The system validates that total order value is within the COD limit (≤ 20,000,000 VND per store policy for high-value bathroom fixtures).  
  3. The Customer clicks "Place Order".  
  4. The system creates the order with status `Pending_Confirmation` and payment status `Unpaid`.  
  5. The system notifies Sales Staff to contact the customer for phone verification before dispatch (UC-22).  
- **Step 7 - Inventory Depleted During Checkout Race Condition**:  
  1. Another customer purchased the last units of an item while this user was on the checkout screen.  
  2. The system halts order creation and displays MSG54: "Some items in your cart have just sold out. Please review your cart before proceeding."  
  3. The system returns the Customer to the cart page with updated stock levels.  
- **Step 9 - Payment Timeout or Incomplete Transfer**:  
  1. The Customer does not complete VietQR payment within 15 minutes.  
  2. The system preserves the order with status `Pending_Payment` and provides a "Retry Payment" button in order history, holding inventory reservation for up to 60 minutes before auto-cancellation.  

**Postconditions**  
1. An online sales order record is created in the database.  
2. Inventory reservation records are placed on the relevant SKUs.  
3. Order confirmation email and SMS notifications are transmitted.  

---

### UC-19 Track Customer Orders

**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
None  

**Description**  
As a Customer or Guest, I want to track the real-time processing, payment, warehouse staging, dispatch, and delivery progress of my orders so that I know when my sanitary ware fixtures will arrive at my site.  

**Preconditions**  
1. For Customer: Logged in and has placed at least one order.  
2. For Guest: Has a valid Order Code and registered phone number.  

**Normal Flow**  
1. The Customer navigates to "My Orders" in their account portal.  
2. The system displays a list of orders with Order Code, Order Date, Items Summary, Grand Total, and Current Status (`Pending_Payment`, `Confirmed`, `Processing`, `Dispatched`, `Delivered`, `Cancelled`).  
3. The Customer clicks "View Order Details" on a specific order.  
4. The system displays a comprehensive tracking view:  
   - Visual progress timeline: Order Placed -> Payment Verified -> Goods Issued at Warehouse -> Out for Delivery -> Successfully Delivered.  
   - Detailed fixture list with SKU codes, unit prices, and warranty links.  
   - Assigned Shipper Name and contact hotline (when status is `Dispatched`).  
   - Delivery address, scheduled delivery date, and payment receipt summary.  
5. The Customer inspects the progress details and delivery notes.  

**Alternative Flows**  
- **Step 1 - Guest Order Tracking Flow**:  
  1. A Guest accesses the public "Track Order" link in the website header.  
  2. The system prompts for Order Code (e.g., `ORD-20261013-0012`) and Billing Phone Number.  
  3. The Guest enters credentials and clicks "Track".  
  4. The system validates the credentials and renders the tracking view without requiring an account login.  
- **Step 2 - Filter Orders by Status**:  
  1. The Customer filters orders by tabs: "All", "Processing", "Delivered", "Cancelled".  
  2. The system filters and refreshes the list instantly.  

**Postconditions**  
1. Real-time delivery and fulfillment status is presented to the user.  

---

### UC-20 Request Order Modification or Cancellation

**Primary Actors**  
Customer  

**Secondary Actors**  
Email Service  

**Description**  
As a Customer, I want to submit a formal request to cancel or modify an order before it has been dispatched from the warehouse so that ordering errors or changed construction schedules can be accommodated.  

**Preconditions**  
1. The Customer is logged in and viewing an active order in "My Orders".  
2. The order status is `Pending_Payment`, `Confirmed`, or `Processing` (has not yet reached `Dispatched`).  

**Normal Flow**  
1. The Customer clicks the "Request Cancellation / Change" button on the order detail page.  
2. The system checks order fulfillment status to ensure the goods have not left the warehouse.  
3. The system opens the Request Form asking for:  
   - Request Type: "Cancel Entire Order" or "Change Delivery Address / Contact".  
   - Reason for Request (e.g., "Ordered wrong rough-in size 305mm instead of 400mm", "Plumbing rough-in delayed by contractor", "Change in site delivery address").  
   - Additional details.  
4. The Customer selects "Cancel Entire Order", inputs the reason, and clicks "Submit Request".  
5. The system logs the change request with status `Pending_Review` on the order record and alerts Sales Staff in FE-06.  
6. If the order was unpaid (`Pending_Payment`): The system immediately cancels the order, releases reserved inventory back to warehouse stock, updates status to `Cancelled`, and displays MSG55: "Your order has been cancelled successfully."  
7. If the order was already paid: The system displays MSG56: "Your cancellation request has been submitted. Our sales staff will review and process your refund within 24 hours."  
8. The system sends an email confirmation of the request via the Email Service.  

**Alternative Flows**  
- **Step 2 - Order Already Dispatched**:  
  1. The warehouse has already packed and dispatched the order (`Dispatched`).  
  2. The system hides the cancellation button and displays MSG57: "This order is already out for delivery and cannot be cancelled online. Please contact our support hotline for return assistance."  
- **Step 3 - Request Address Modification**:  
  1. The Customer selects "Change Delivery Address", inputs new delivery address details, and submits.  
  2. The system updates the order's pending address changes and alerts Sales Staff to confirm before delivery dispatch (UC-26).  

**Postconditions**  
1. The cancellation/modification request is queued for Sales Staff handling (FE-06 UC-26).  
2. Unpaid orders are cancelled immediately with stock reservations released.  
