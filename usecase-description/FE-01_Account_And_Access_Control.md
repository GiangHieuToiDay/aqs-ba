# MODULE FE-01: ACCOUNT AND ACCESS CONTROL

---

### UC-01 Register Customer Account

**Primary Actors**  
Guest  

**Secondary Actors**  
Email Service, SMS Service, Google Authentication Service  

**Description**  
As a Guest, I want to register a new customer account using my contact credentials or Google account so that I can sign into AquaSuite, place online orders, track delivery dispatches, and request warranty services.  

**Preconditions**  
1. The Guest is not currently authenticated in the system.  
2. The AquaSuite public storefront is accessible.  

**Normal Flow**  
1. The Guest opens the Registration page by clicking the "Register" button on the storefront navigation bar.  
2. The system displays the customer registration form with input fields: Full Name, Phone Number, Email Address, Password, Password Confirmation, and a Terms of Service agreement checkbox.  
3. The Guest enters valid personal details:  
   - Full Name (2 to 50 characters).  
   - Phone Number (valid 10-digit Vietnamese phone number).  
   - Email Address (valid RFC email format).  
   - Password (minimum 8 characters, containing at least one uppercase letter, one lowercase letter, one number, and one special character).  
   - Password Confirmation (matching Password).  
   - Marks the Terms of Service checkbox.  
4. The Guest clicks the "Register Now" button.  
5. The system validates the form inputs and verifies that neither the Email Address nor the Phone Number is already registered in the database.  
6. The system generates a 6-digit numeric verification OTP (valid for 5 minutes) and dispatches an email via the Email Service.  
7. The system redirects the Guest to the OTP Verification screen with a 300-second countdown timer.  
8. The Guest enters the 6-digit OTP received in their email inbox and clicks "Verify".  
9. The system validates the OTP against the cached token.  
10. The system creates the new user account with role `Customer`, status `Active`, hashes the password using BCrypt, and initializes an associated Customer Profile record.  
11. The system records the registration event in the Activity Log.  
12. The system automatically signs the new Customer in, generates a JWT session token, and displays MSG01: "Account registered successfully! Welcome to AquaSuite."  

**Alternative Flows**  
- **Step 3 - Google OAuth Registration**:  
  1. The Guest clicks the "Sign up with Google" button.  
  2. The system redirects the Guest to the Google Authentication Service consent screen.  
  3. The Guest authenticates with Google and grants permission to share profile info (Email, Full Name).  
  4. Google Authentication Service returns an identity token.  
  5. The system verifies the token. If the email is new, the system creates an active `Customer` account and prompts the user to enter their Phone Number.  
  6. The Guest inputs their Phone Number and proceeds to Step 10.  
- **Step 5 - Validation Errors**:  
  1. Any required field is blank: System displays MSG02: "Please fill in all mandatory fields."  
  2. Phone number or email format is invalid: System displays MSG03: "Invalid phone number or email address format."  
  3. Password fails security complexity policy: System displays MSG04: "Password must be at least 8 characters long and contain uppercase, lowercase, numbers, and special characters."  
  4. Passwords do not match: System displays MSG05: "Password confirmation does not match."  
  5. The Guest corrects the errors and resubmits the form.  
- **Step 5 - Duplicate Account Credentials**:  
  1. The system detects that the Email or Phone Number already exists in the user database.  
  2. The system retains entered fields and displays MSG06: "An account with this email address or phone number already exists. Please log in or reset your password."  
  3. The Guest is offered a direct link to the Login screen.  
- **Step 8 - Resend OTP Request**:  
  1. The Guest does not receive the OTP within 60 seconds and clicks "Resend OTP".  
  2. The system invalidates the previous code, generates a fresh OTP, and triggers SMS Service as a fallback if requested.  
  3. The system restarts the verification countdown timer.  
- **Step 9 - Invalid or Expired OTP**:  
  1. The Guest enters an incorrect OTP: The system displays MSG07: "Invalid OTP code. Please check and try again (Remaining attempts: X)."  
  2. If the OTP has expired (> 5 minutes): The system displays MSG08: "The verification code has expired. Please request a new code."  

**Postconditions**  
1. A new user record is created in the database with role `Customer` and status `Active`.  
2. A corresponding Customer Profile is initialized in the CRM module.  
3. A successful registration entry is written to the Activity Log.  

---

### UC-02 User Login and Authentication

**Primary Actors**  
Customer, Sales Staff, Technical Staff, Shipper, Accountant, Store Owner  

**Secondary Actors**  
Google Authentication Service  

**Description**  
As a registered user, I want to authenticate into the system with my credentials or Google account (and safely log out) so that I can access authorized business functions corresponding to my assigned role.  

**Preconditions**  
1. The user possesses a registered account in AquaSuite with status `Active`.  
2. The user is currently not logged in, or their previous session has expired.  

**Normal Flow**  
1. The user navigates to the Login page via the storefront header or internal staff portal link.  
2. The system renders the Login interface presenting input fields for Username (Email or Phone Number), Password, "Remember Me" checkbox, and a "Sign In" button.  
3. The user inputs their registered Email/Phone and Password, then clicks "Sign In".  
4. The system validates the input fields, retrieves the user record, verifies the BCrypt password hash, and confirms that the account status is `Active`.  
5. The system generates secure JWT Access and Refresh Tokens containing role claims and permissions.  
6. The system records the successful login timestamp, IP address, and user-agent in the Activity Log.  
7. The system redirects the user to the appropriate screen according to their role:  
   - `Store Owner`, `Sales Staff`, `Accountant`: Redirected to the Back-Office Management Dashboard.  
   - `Technical Staff`: Redirected to the Technical Schedule and Work Order list.  
   - `Shipper`: Redirected to the Delivery Dispatch and Run Sheet interface.  
   - `Customer`: Redirected to the Storefront Homepage or previous browsing destination.  
8. The system displays MSG09: "Logged in successfully."  

**Alternative Flows**  
- **Step 3 - Google Single Sign-On**:  
  1. The user selects "Log in with Google".  
  2. The system redirects to Google Authentication Service.  
  3. The user logs in via Google and grants identity access.  
  4. Google returns a verified identity token. The system locates the associated account by email.  
  5. If located and active, the system issues session tokens and proceeds to Step 6.  
- **Step 4 - Invalid Credentials**:  
  1. The user inputs an incorrect username or password.  
  2. The system increments the failed login attempt counter for the account.  
  3. The system displays MSG10: "Invalid username or password. Please try again."  
- **Step 4 - Account Locked or Inactive**:  
  1. The user has exceeded 5 consecutive failed login attempts, or the account is deactivated (`Suspended`/`Inactive`).  
  2. The system blocks the authentication request.  
  3. The system displays MSG11: "This account has been locked or deactivated. Please contact the Store Owner for assistance."  
- **Step 7 - Secure Logout Flow**:  
  1. The authenticated user clicks the "Log Out" button on the profile menu.  
  2. The system revokes the current JWT refresh token and clears client cookies/local storage.  
  3. The system writes a logout event to the Activity Log.  
  4. The system redirects the user to the public login page with MSG12: "You have been logged out successfully."  

**Postconditions**  
1. A valid authenticated session is established with role-based permissions granted.  
2. Login metadata (timestamp, IP, device) is updated on the user record and logged into the Activity Log.  

---

### UC-03 Manage Profile and Delivery Addresses

**Primary Actors**  
Customer, Sales Staff, Technical Staff, Shipper, Accountant, Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As an authenticated user, I want to view and update my personal profile, contact information, avatar image, and delivery address book so that my account records and shipping information remain accurate.  

**Preconditions**  
1. The user is logged into the AquaSuite platform.  
2. The user navigates to the "My Profile" / "Account Settings" section.  

**Normal Flow**  
1. The user accesses the Account Profile screen.  
2. The system loads and displays the current profile details: Full Name, Email, Phone Number, Avatar, Role/Customer Tier, and saved Delivery Addresses.  
3. The user modifies editable details (Full Name, Phone Number, or selects a new Avatar image).  
4. If an avatar image is uploaded, the system uploads the image binary to Cloudinary, retrieves the secure CDN URL, and updates the avatar preview.  
5. The user clicks "Save Changes".  
6. The system validates the inputs, updates the User and Customer Profile records, and saves the new avatar URL.  
7. The system refreshes the screen and displays MSG13: "Profile updated successfully."  

**Alternative Flows**  
- **Step 3 - Manage Delivery Address Book (Customer)**:  
  1. The Customer selects the "Address Book" tab.  
  2. The system lists all saved delivery addresses with labels (Home, Construction Site, Villa), Recipient Name, Phone, Province/City, District, Ward, Detailed Street Address, and a "Default Address" badge.  
  3. The Customer clicks "Add New Address".  
  4. The system displays an address modal with administrative division dropdowns (Province, District, Ward).  
  5. The Customer enters recipient details, selects administrative units, and toggles "Set as Default Delivery Address".  
  6. The Customer clicks "Save Address".  
  7. The system saves the address record and displays MSG14: "Delivery address added successfully."  
- **Step 3 - Edit or Delete Delivery Address**:  
  1. The Customer clicks "Edit" or "Delete" on an existing address entry.  
  2. If editing: The Customer updates address fields and clicks "Update". The system saves changes and displays MSG15: "Address updated successfully."  
  3. If deleting: The system prompts for confirmation. Upon confirmation, the system soft-deletes the address and displays MSG16: "Address deleted successfully."  
- **Step 4 - Invalid Avatar Upload**:  
  1. The user uploads a file exceeding 5MB or an unsupported format (non-JPEG/PNG/WEBP).  
  2. The system rejects the file and displays MSG17: "Avatar must be a JPEG, PNG, or WEBP image under 5MB."  

**Postconditions**  
1. User profile data and avatar URLs are updated in the database.  
2. Address changes are immediately available for checkout and delivery dispatch.  

---

### UC-04 Manage Password and Account Recovery

**Primary Actors**  
Customer, Sales Staff, Technical Staff, Shipper, Accountant, Store Owner  

**Secondary Actors**  
Email Service, SMS Service  

**Description**  
As a user, I want to change my current password or request an OTP-based password recovery via email/SMS so that my account credentials remain secure and recoverable if forgotten.  

**Preconditions**  
1. For changing password: User is logged in and knows their current password.  
2. For password recovery: User is on the login screen and has forgotten their password.  

**Normal Flow**  
1. The user opens the "Change Password" section in Account Settings.  
2. The system renders three input fields: Current Password, New Password, and Confirm New Password.  
3. The user enters their current password, inputs a new compliant password, and re-enters the new password in the confirmation field.  
4. The user clicks "Update Password".  
5. The system verifies that the current password matches the stored BCrypt hash.  
6. The system checks that the new password meets security policies (8+ chars, uppercase, lowercase, number, symbol) and differs from the last 3 passwords.  
7. The system updates the password hash, updates the `PasswordLastChangedAt` timestamp, revokes all other active sessions, and displays MSG18: "Password changed successfully. Please log in again if required."  

**Alternative Flows**  
- **Step 1 - Forgotten Password Recovery Flow**:  
  1. From the Login page, the user clicks "Forgot Password?".  
  2. The system displays the Password Recovery screen requesting registered Email or Phone Number.  
  3. The user submits their registered Email address.  
  4. The system validates the account exists, generates a secure password reset token with a 6-digit OTP (expires in 10 minutes), and dispatches it via the Email Service.  
  5. The system displays MSG19: "A password reset code has been sent to your email."  
  6. The user enters the 6-digit OTP and their desired New Password twice.  
  7. The system verifies the token, resets the password hash, unlocks the account if previously locked, and displays MSG20: "Your password has been reset successfully. You may now sign in."  
- **Step 5 - Incorrect Current Password**:  
  1. The user inputs an incorrect current password.  
  2. The system rejects the change and displays MSG21: "Current password is incorrect."  
- **Step 6 - Weak or Mismatched New Password**:  
  1. New password does not meet complexity requirements or does not match confirmation.  
  2. The system displays MSG22: "New password does not meet security requirements or does not match confirmation."  

**Postconditions**  
1. The user's password hash is updated in the database.  
2. Old session tokens are invalidated to enforce security.  
3. Password modification event is recorded in the Activity Log.  

---

### UC-05 Manage User Accounts and Status

**Primary Actors**  
Store Owner  

**Secondary Actors**  
Email Service  

**Description**  
As a Store Owner, I want to create internal staff accounts, view account lists, edit user details, and toggle account activation status so that only authorized personnel can access the store management system.  

**Preconditions**  
1. The Store Owner is authenticated and holds administrative privileges.  
2. The "User Accounts Management" screen is accessible in the Admin Portal.  

**Normal Flow**  
1. The Store Owner navigates to the "User Management" module.  
2. The system displays a paginated list of all system accounts (Store Owner, Sales Staff, Technical Staff, Shipper, Accountant, Customer) displaying Full Name, Email, Phone, Role, Status (`Active`, `Inactive`, `Locked`), and Creation Date.  
3. The Store Owner clicks the "Create Staff Account" button.  
4. The system presents a form with fields: Full Name, Email, Phone Number, Assigned Role dropdown, Department, and an auto-generated temporary password toggle.  
5. The Store Owner fills in staff details, selects an internal role (e.g., `Sales Staff`), and clicks "Save Account".  
6. The system validates the data, creates the account, hashes the temporary password, and triggers the Email Service to send an activation email containing login credentials.  
7. The system refreshes the user grid and displays MSG23: "Staff account created successfully and credentials emailed."  

**Alternative Flows**  
- **Step 2 - Filter and Search Users**:  
  1. The Store Owner filters by Role, Status, or searches by keyword (Name, Email, Phone).  
  2. The system updates the list dynamically matching the search criteria.  
- **Step 2 - Deactivate or Reactivate Account**:  
  1. The Store Owner locates an account and clicks the status toggle (Deactivate/Activate).  
  2. If deactivating: The system prompts for confirmation: "Deactivating this account will immediately revoke all access."  
  3. The Store Owner confirms.  
  4. The system updates the status to `Inactive`, revokes all active JWT tokens for that user, and displays MSG24: "Account status updated to Inactive."  
- **Step 5 - Validation / Duplicate Email**:  
  1. The entered email or phone is already associated with an existing account.  
  2. The system displays MSG25: "This email address or phone number is already registered to another account."  

**Postconditions**  
1. Staff account is stored in the database with assigned system roles.  
2. Account status changes take effect immediately across all active sessions.  
3. Changes are written to the Activity Log.  

---

### UC-06 Manage Roles and Permissions

**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to configure Role-Based Access Control (RBAC) roles and assign granular functional permissions so that each employee operates strictly within their assigned business responsibilities.  

**Preconditions**  
1. The Store Owner is logged in with master administrative access.  
2. The "Roles & Permissions" configuration interface is open.  

**Normal Flow**  
1. The Store Owner opens the Roles & Permissions screen.  
2. The system displays the list of system roles: `Store Owner`, `Sales Staff`, `Technical Staff`, `Shipper`, `Accountant`, and `Customer`.  
3. The Store Owner selects a role (e.g., `Sales Staff`) to inspect its assigned permissions.  
4. The system displays a categorized permission matrix (Products, Orders, Quotations, Inventory, Warehousing, Technical, Accounting, Reports) with checkboxes for View, Create, Edit, Delete, and Approve operations.  
5. The Store Owner modifies specific permission flags (e.g., granting discount approval up to 10% for Sales Staff).  
6. The Store Owner clicks "Save Permissions".  
7. The system verifies security policies (preventing self-lockout of Store Owner privileges) and commits permission updates to the database.  
8. The system refreshes active role definitions and displays MSG26: "Role permissions updated successfully."  

**Alternative Flows**  
- **Step 3 - Create Custom Role**:  
  1. The Store Owner clicks "Add New Role".  
  2. The system prompts for Role Name, Code, and Description (e.g., `Warehouse Supervisor`).  
  3. The Store Owner checks the desired permissions and clicks "Create Role".  
  4. The system registers the new role and displays MSG27: "New role created successfully."  
- **Step 6 - Guarding Core System Privileges**:  
  1. The Store Owner attempts to remove administrative permissions from the master `Store Owner` role.  
  2. The system rejects the modification and displays MSG28: "The Store Owner role must retain full administrative privileges to prevent system lockout."  

**Postconditions**  
1. Permission mappings for the role are saved in the database.  
2. Active user sessions belonging to that role have their authorization claims refreshed on their next token rotation.  
