# AQUASUITE — TỔNG HỢP ĐẶC TẢ USE CASE (66 USE CASES)

> **Hệ thống**: AquaSuite — Quản lý chuỗi cửa hàng và phân phối thiết bị vệ sinh cao cấp
> **Nguồn danh mục**: AquaSuite_Final_Use_Case_Catalog_66 (1).md (66 Use Cases tinh gọn)
> **Chuẩn đặc tả**: RUP / Agile Use Case Specification Standard
> **Mẫu đặc tả**: Primary Actors, Secondary Actors, Description (User Story), Preconditions, Normal Flow, Alternative Flows, Postconditions
> **Quy cách tài liệu**: Bản gộp toàn bộ 66 Use Case theo thứ tự tuần tự (STT 1 -> 66), không phân tách theo module

---

## BẢNG DANH MỤC 66 USE CASES HỆ THỐNG

| STT | Mã UC | Tên Use Case (Tiếng Việt) | Tên Use Case (English) | UC Gốc |
|:---:|:---:|:---|:---|:---:|
| 1 | **UC-01** | Đăng ký tài khoản khách hàng | Register Customer Account | UC-001 |
| 2 | **UC-02** | Đăng nhập và xác thực người dùng | User Login and Authentication | UC-002 |
| 3 | **UC-03** | Quản lý hồ sơ cá nhân và địa chỉ | Manage Profile and Delivery Addresses | UC-003, UC-004 |
| 4 | **UC-04** | Quản lý mật khẩu và khôi phục tài khoản | Manage Password and Account Recovery | UC-005, UC-006, UC-010 |
| 5 | **UC-05** | Quản lý tài khoản và trạng thái người dùng | Manage User Accounts and Status | UC-007, UC-008 |
| 6 | **UC-06** | Quản lý vai trò và phân quyền | Manage Roles and Permissions | UC-009 |
| 7 | **UC-07** | Tra cứu và xem chi tiết nhật ký hoạt động | Search and View Activity Logs | UC-012, UC-013 |
| 8 | **UC-08** | Quản lý danh mục và thương hiệu | Manage Categories and Brands | UC-014, UC-021 |
| 9 | **UC-09** | Quản lý sản phẩm | Manage Products | UC-015, UC-017 |
| 10 | **UC-10** | Quản lý SKU, biến thể và giá bán | Manage SKUs, Variants, and Pricing | UC-016, UC-018, UC-019, UC-020 |
| 11 | **UC-11** | Nhập và xuất dữ liệu sản phẩm | Import and Export Product Data | UC-023 |
| 12 | **UC-12** | Tìm kiếm và xem thông tin sản phẩm | Search and View Product Information | UC-024, UC-025 |
| 13 | **UC-13** | Xem và quản lý gói sản phẩm | View and Manage Product Combos | UC-027 |
| 14 | **UC-14** | Tư vấn và gợi ý sản phẩm bằng AI | AI Consultation and Product Recommendation | UC-028, UC-029 |
| 15 | **UC-15** | Quản lý yêu cầu và lịch sử tư vấn | Manage Consultation Requests and History | UC-030, UC-032 |
| 16 | **UC-16** | Chuyển tư vấn thành báo giá hoặc đơn hàng | Convert Consultation to Quotation or Order | UC-033 |
| 17 | **UC-17** | Quản lý giỏ hàng | Manage Shopping Cart | UC-034 |
| 18 | **UC-18** | Đặt hàng và thanh toán trực tuyến | Place Online Order and Payment | UC-035, UC-036 |
| 19 | **UC-19** | Theo dõi đơn hàng của khách hàng | Track Customer Orders | UC-037 |
| 20 | **UC-20** | Yêu cầu thay đổi hoặc hủy đơn hàng | Request Order Modification or Cancellation | UC-038, UC-039 |
| 21 | **UC-21** | Tạo và quản lý đơn hàng bán | Create and Manage Sales Orders | UC-040, UC-041, UC-043, UC-044, UC-045, UC-052 |
| 22 | **UC-22** | Xác nhận và xử lý đơn hàng online | Confirm and Process Online Orders | UC-042 |
| 23 | **UC-23** | Quản lý thanh toán và xác minh thanh toán | Manage Payment and Payment Verification | UC-046, UC-047, UC-059 |
| 24 | **UC-24** | Quản lý công nợ khách hàng | Manage Customer Debt and Receivables | UC-048 |
| 25 | **UC-26** | Xử lý yêu cầu thay đổi hoặc hủy đơn | Process Order Modification or Cancellation Requests | UC-051 |
| 26 | **UC-27** | Quản lý báo giá dự án | Manage Project Quotations | UC-054, UC-055, UC-060 |
| 27 | **UC-28** | Chuyển báo giá thành đơn hàng | Convert Quotation to Sales Order | UC-056 |
| 28 | **UC-29** | Quản lý chiết khấu, đặt cọc và giữ hàng | Manage Discounts, Deposits, and Stock Reservation | UC-057, UC-058 |
| 29 | **UC-30** | Quản lý thu hộ COD và bàn giao tiền | Manage COD Collection and Remittance | UC-061 |
| 30 | **UC-31** | Tra cứu và quản lý thông tin tồn kho | Look up and Manage Inventory Information | UC-062, UC-071, UC-072 |
| 31 | **UC-32** | Quản lý nhập kho | Manage Goods Receipt | UC-063, UC-064, UC-065 |
| 32 | **UC-33** | Quản lý nhà cung cấp | Manage Suppliers | UC-066 |
| 33 | **UC-34** | Quản lý đơn nhập hàng | Manage Purchase Orders | UC-067, UC-075 |
| 34 | **UC-36** | Quản lý giữ hàng | Manage Stock Reservations | UC-073, UC-076 |
| 35 | **UC-37** | Xử lý hàng trả về và trả nhà cung cấp | Process Returned Goods and Vendor Returns | UC-077, UC-078 |
| 36 | **UC-38** | Nhập/xuất dữ liệu tồn kho | Import and Export Inventory Data | UC-079 |
| 37 | **UC-39** | Quản lý phiếu xuất kho | Manage Goods Issue Notes | UC-080, UC-081, UC-082 |
| 38 | **UC-40** | Lập và quản lý đợt giao hàng | Create and Manage Delivery Dispatches | UC-083, UC-084, UC-087 |
| 39 | **UC-41** | Thực hiện và cập nhật kết quả giao hàng | Execute and Update Delivery Results | UC-085, UC-086, UC-091 |
| 40 | **UC-42** | Theo dõi lịch sử và tiến độ giao hàng | Track Delivery Progress and History | UC-088 |
| 41 | **UC-43** | Tra cứu chuyến giao được phân công | Look up Assigned Delivery Trips | UC-089, UC-090, UC-092 |
| 42 | **UC-44** | Quản lý chính sách bảo hành | Manage Warranty Policies | UC-093, UC-094 |
| 43 | **UC-45** | Quản lý hồ sơ bảo hành | Manage Warranty Records | UC-095, UC-096, UC-103 |
| 44 | **UC-46** | Tiếp nhận và xử lý yêu cầu bảo hành | Receive and Process Warranty Claims | UC-097, UC-098, UC-101, UC-102 |
| 45 | **UC-47** | Gửi yêu cầu bảo hành trực tuyến | Submit Online Warranty Claim | UC-099 |
| 46 | **UC-48** | Tra cứu bảo hành của khách hàng | Look up Warranty Information | UC-100 |
| 47 | **UC-49** | Quản lý hồ sơ khách hàng và liên kết tài khoản | Manage Customer Profiles and Account Linking | UC-104, UC-105 |
| 48 | **UC-50** | Tra cứu lịch sử khách hàng và mua hàng | Look up Customer History and Purchases | UC-106, UC-108 |
| 49 | **UC-51** | Quản lý công trình/dự án khách hàng | Manage Customer Construction Projects | UC-107 |
| 50 | **UC-53** | Xem dashboard và cảnh báo vận hành | View Operations Dashboard and Alerts | UC-111, UC-120 |
| 51 | **UC-54** | Xem báo cáo kinh doanh và doanh số | View Sales and Revenue Reports | UC-112, UC-113, UC-114, UC-115, UC-116, UC-122 |
| 52 | **UC-55** | Xem báo cáo tồn kho và công nợ | View Inventory and Debt Reports | UC-117, UC-118 |
| 53 | **UC-56** | Quản lý chi phí vận hành và xuất báo cáo | Manage Operating Expenses and Export Reports | UC-119, UC-121 |
| 54 | **UC-58** | Tra cứu dữ liệu kinh doanh qua Telegram | Query Business Data via Telegram | UC-124, UC-125, UC-126 |
| 55 | **UC-59** | Tra cứu công việc và lịch kỹ thuật | Look up Technical Jobs and Schedule | UC-127, UC-128, UC-129 |
| 56 | **UC-60** | Tiếp nhận và thực hiện công việc kỹ thuật | Accept and Execute Technical Jobs | UC-130, UC-133, UC-134 |
| 57 | **UC-61** | Ghi nhận kết quả và hoàn thành công việc | Record Results and Complete Technical Jobs | UC-131, UC-132 |
| 58 | **UC-62** | Tra cứu và quản lý giao dịch tài chính | Look up and Manage Financial Transactions | UC-135, UC-142, UC-143 |
| 59 | **UC-63** | Quản lý phiếu thu và phân bổ thanh toán | Manage Receipts and Payment Allocation | UC-136, UC-137, UC-138, UC-149 |
| 60 | **UC-64** | Quản lý phiếu chi và chứng từ | Manage Payment Vouchers and Documents | UC-139, UC-140, UC-141 |
| 61 | **UC-65** | Quản lý và đối soát công nợ phải thu | Manage and Reconcile Accounts Receivable | UC-144, UC-145, UC-150 |
| 62 | **UC-66** | Quản lý và đối soát công nợ phải trả | Manage and Reconcile Accounts Payable | UC-146, UC-147, UC-148 |
| 63 | **UC-67** | Xem báo cáo thu chi và dòng tiền | View Cash Flow and Receipts/Payments Report | UC-151, UC-152 |
| 64 | **UC-68** | Xem báo cáo doanh thu và công nợ | View Revenue and Debt Report | UC-153, UC-154, UC-155 |
| 65 | **UC-69** | Xem dashboard kế toán | View Accounting Dashboard | UC-156 |
| 66 | **UC-70** | Xử lý các khoản thanh toán chờ xác minh | Process Pending Payment Verifications | UC-157 |

---

## CHI TIẾT ĐẶC TẢ 66 USE CASES

### 1. UC-01: Đăng ký tài khoản khách hàng (Register Customer Account)

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

### 2. UC-02: Đăng nhập và xác thực người dùng (User Login and Authentication)

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

### 3. UC-03: Quản lý hồ sơ cá nhân và địa chỉ (Manage Profile and Delivery Addresses)

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

### 4. UC-04: Quản lý mật khẩu và khôi phục tài khoản (Manage Password and Account Recovery)

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

### 5. UC-05: Quản lý tài khoản và trạng thái người dùng (Manage User Accounts and Status)

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

### 6. UC-06: Quản lý vai trò và phân quyền (Manage Roles and Permissions)

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

---

### 7. UC-07: Tra cứu và xem chi tiết nhật ký hoạt động (Search and View Activity Logs)

**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to search, filter, and inspect detailed system audit trails and operation logs so that all business transactions, data modifications, and security events can be audited, investigated, and verified for compliance.  

**Preconditions**  
1. The Store Owner is authenticated and authorized to access security and audit records.  
2. The Activity Log screen is accessible in the Admin Portal.  

**Normal Flow**  
1. The Store Owner navigates to the "System Activity Logs" screen under System Administration.  
2. The system renders the audit trail dashboard containing:  
   - Search filter bar (Date Range, Module, Action Type: Create, Update, Delete, Login, Export, Approve, User/Actor, Target Entity, IP Address).  
   - Paginated grid of log entries: Log ID, Timestamp, Actor Name & Role, Action Description, Module, Entity ID, Client IP, and Status (`Success`/`Failure`).  
3. The Store Owner inputs search criteria (e.g., Module = "Inventory", Action = "Stock Adjustment", Date Range = Last 7 Days) and clicks "Filter".  
4. The system queries the immutable log database and displays matching audit entries.  
5. The Store Owner clicks "View Details" on a specific log record (e.g., "Price updated for SKU-TOTO-001").  
6. The system displays a modal containing the full audit payload, including:  
   - Detailed user context (User ID, Role, Session ID, IP address, Device agent).  
   - Changed attributes showing Before vs. After values (JSON diff).  
   - Exact API endpoint, execution duration (ms), and server response code.  
7. The Store Owner inspects the changes and closes the modal.  

**Alternative Flows**  
- **Step 3 - No Matching Log Records**:  
  1. The system finds zero log entries matching the filter combination.  
  2. The system displays an empty table state with MSG29: "No activity log entries match your search criteria."  
  3. The Store Owner clicks "Reset Filters" to reload all recent logs.  
- **Step 2 - Export Audit Logs**:  
  1. The Store Owner selects a date range and clicks "Export to Excel/CSV".  
  2. The system compiles the audit entries into an encrypted CSV/Excel file.  
  3. The system downloads the file and records an "Audit Log Exported" event in the log itself.  
- **Step 5 - Security Alert Investigation**:  
  1. The Store Owner filters logs by Status = `Failure` and Action = `Login`.  
  2. The system highlights multiple consecutive failed attempts originating from a single IP address with a warning badge.  
  3. The Store Owner can inspect the failed attempts and initiate IP blocking if necessary.  

**Postconditions**  
1. Audit logs remain strictly immutable (read-only); no log entries can be modified or deleted.  
2. Inspection and export actions are themselves recorded in the system audit trail.

---

### 8. UC-08: Quản lý danh mục và thương hiệu (Manage Categories and Brands)

**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to manage a hierarchical category structure and sanitary ware brand profiles (TOTO, Inax, Caesar, Viglacera, American Standard) so that bathroom fixtures are organized consistently for browsing and inventory management.  

**Preconditions**  
1. The Store Owner is logged in and authorized to manage product classifications.  
2. The Product Categories & Brands screen is accessible in the Admin Portal.  

**Normal Flow**  
1. The Store Owner opens the Product Categories & Brands screen.  
2. The system displays a hierarchical tree of parent and child categories (e.g., Toilets -> Smart Toilets, One-piece Toilets, Wall-hung Toilets; Washbasins -> Cabinet Basins, Countertop Basins; Showers & Faucets; Bathtubs; Bathroom Accessories) alongside a Brands management tab.  
3. Each category node displays its Code, Name, Icon, linked product count, and Visibility status (`Visible`/`Hidden`).  
4. The Store Owner expands or collapses branches to inspect the category hierarchy.  
5. The Store Owner drags a category node to rearrange its display position.  
6. The system updates and saves the display order in the database.  
7. The system refreshes the tree and displays MSG134: "Category display order updated successfully."  

**Alternative Flows**  
- **Step 2 - No Categories Available**:  
  1. The system displays an empty tree and a "Create First Root Category" button.  
  2. The Store Owner clicks the button.  
  3. The system opens a category creation modal.  
  4. The Store Owner enters Category Name, Code, selects an icon, and clicks "Save".  
  5. The system creates the root category and reloads the tree.  
- **Step 3 - Create New Child Category**:  
  1. The Store Owner clicks "Add Subcategory" on an existing category node (e.g., under "Toilets").  
  2. The system displays the form requesting Name (e.g., "Wall-hung Toilets"), Code (`CAT-TOI-WH`), Description, and Category Banner.  
  3. The Store Owner uploads a banner image to Cloudinary and saves.  
  4. The system links the subcategory under the parent and displays MSG30: "Category created successfully."  
- **Step 3 - Update or Toggle Visibility**:  
  1. The Store Owner edits category metadata or toggles Visibility to `Hidden`.  
  2. The system persists changes and displays MSG31: "Category updated successfully."  
- **Step 3 - Delete Category with Validation**:  
  1. The Store Owner clicks "Delete" on a category.  
  2. The system verifies if any active products or child subcategories are linked.  
  3. If products are linked: The system blocks deletion and displays MSG32: "Cannot delete category containing active products. Reassign or delete products first."  
  4. If empty: The system prompts for confirmation, soft-deletes the category, and displays MSG33: "Category deleted successfully."  
- **Step 2 - Manage Brands Sub-flow**:  
  1. The Store Owner clicks the "Brands" tab.  
  2. The system displays a grid of sanitary ware brands (TOTO, Inax, Viglacera, etc.) showing Brand Logo, Origin (Japan, Vietnam, Germany), Website, and Status.  
  3. The Store Owner adds or updates a brand with a logo uploaded to Cloudinary.  
  4. The system saves the brand record and displays MSG34: "Brand updated successfully."  

**Postconditions**  
1. Category tree hierarchy, ordering, and brand definitions are stored in the database.  
2. Catalog navigation menus and filter bars reflect the updated categories and brands immediately.

---

### 9. UC-09: Quản lý sản phẩm (Manage Products)

**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to create, update, and manage master sanitary ware products with technical specifications, origin, installation guides, and gallery media so that bathroom fixtures are accurately presented for sale.  

**Preconditions**  
1. The Store Owner is authenticated with catalog management permissions.  
2. Relevant Categories (UC-08) and Brands have been created.  

**Normal Flow**  
1. The Store Owner navigates to the "Product Management" list in the Admin Portal.  
2. The system displays a paginated list of sanitary ware products showing Product Code, Image, Name, Category, Brand, Total Variants, Base Price Range, and Status (`Draft`, `Active`, `Discontinued`).  
3. The Store Owner clicks "Create Product".  
4. The system opens the Product Creation form with sections:  
   - Basic Info: Product Name (e.g., "TOTO Neorest Smart Toilet"), Product Code (`PROD-TOTO-NEO-01`), Category, Brand, Origin.  
   - Detailed Description & Rich-Text Features (CeFiONtect ceramic glaze, Tornado flush, auto-deodorizer).  
   - Technical Specifications (Rough-in dimension 305mm, Water pressure 0.05-0.75 MPa, Power rating 220V/850W, Trapway S-Trap).  
   - Media Gallery: Upload high-resolution product photos, dimensional blueprints, and installation specification PDFs.  
5. The Store Owner fills in all required fields and uploads media files.  
6. The system uploads image assets and specification drawings to Cloudinary and retrieves secure asset URLs.  
7. The Store Owner sets initial status to `Active` and clicks "Save Product".  
8. The system validates uniqueness of the Product Code, saves the master product, and displays MSG35: "Product created successfully. Please configure SKUs and variants."  

**Alternative Flows**  
- **Step 2 - Search, Filter, and Edit Product**:  
  1. The Store Owner filters products by Category, Brand, or searches by keyword.  
  2. The Store Owner clicks "Edit" on a product.  
  3. The system loads product data into the form.  
  4. The Store Owner modifies details (e.g., updates technical specs or replaces gallery images) and clicks "Save Changes".  
  5. The system saves changes and displays MSG36: "Product details updated successfully."  
- **Step 4 - Duplicate Product Code**:  
  1. The entered Product Code already exists in the system.  
  2. The system flags the field with MSG37: "Product Code must be unique. This code is already in use."  
  3. The Store Owner enters an alternate code.  
- **Step 2 - Discontinue / Archive Product**:  
  1. The Store Owner changes product status to `Discontinued`.  
  2. The system prompts: "Discontinuing this product will hide it from the customer storefront while preserving existing sales history."  
  3. The Store Owner confirms. The system updates status and displays MSG38: "Product marked as Discontinued."  

**Postconditions**  
1. Master product record and technical attributes are persisted.  
2. Media asset URLs are stored and ready for storefront and quote PDF rendering.

---

### 10. UC-10: Quản lý SKU, biến thể và giá bán (Manage SKUs, Variants, and Pricing)

**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to manage individual product variants, barcodes/SKUs, dimensions, finishes, retail prices, and wholesale pricing tiers so that inventory and sales operations can accurately reference specific saleable items.  

**Preconditions**  
1. The master product container has been created (UC-09).  
2. The Store Owner is on the "Variants & SKUs" tab of the product editor.  

**Normal Flow**  
1. The Store Owner opens the "Variants & Pricing" section for a product (e.g., "TOTO Neorest Smart Toilet").  
2. The system displays existing variant rows: SKU Code, Barcode, Variant Name (e.g., "White - Rough-in 305mm", "Matte Black - Rough-in 305mm"), Attributes (Color, Rough-in, Flushing Volume), Retail Selling Price, Contractor/Wholesale Price, and Cost Price.  
3. The Store Owner clicks "Add Variant".  
4. The system opens a variant configuration row/form.  
5. The Store Owner enters:  
   - SKU Code (e.g., `SKU-NEO-WHT-305`).  
   - Barcode / EAN-13.  
   - Variant Attributes: Color (`Glossy White`), Dimensions (`690 x 390 x 510 mm`), Weight (`45 kg`).  
   - Cost Price (e.g., `45,000,000 VND`).  
   - Retail Selling Price (e.g., `68,000,000 VND`).  
   - Wholesale / Project Tier Price (e.g., `58,000,000 VND`).  
6. The Store Owner clicks "Save Variants".  
7. The system validates that SKU Code and Barcode are globally unique across all products.  
8. The system saves the variant records and automatically initializes inventory ledger rows (with stock quantity 0) across all warehouses.  
9. The system displays MSG39: "SKU and variant pricing saved successfully."  

**Alternative Flows**  
- **Step 2 - Bulk Generate Variants via Attributes Matrix**:  
  1. The Store Owner clicks "Generate Variants Matrix".  
  2. The Store Owner selects Attribute 1 (Color: Chrome, Matte Black, Brushed Gold) and Attribute 2 (Spout Length: 150mm, 200mm).  
  3. The system generates all 6 combinations and pre-populates SKU templates.  
  4. The Store Owner enters pricing for each combination and clicks "Confirm".  
  5. The system batch-creates the variants and displays MSG40: "Generated 6 variants successfully."  
- **Step 7 - Duplicate SKU or Barcode**:  
  1. The entered SKU code or Barcode already exists in another product.  
  2. The system rejects saving and highlights the duplicate row with MSG41: "Duplicate SKU or Barcode detected. Each variant must have unique identifiers."  
- **Step 5 - Invalid Price Relationship**:  
  1. The Store Owner sets Retail Price lower than Cost Price without override authorization.  
  2. The system warns: "Selling price is lower than cost price. Are you sure you want to proceed?"  
  3. The Store Owner acknowledges or adjusts the price.  

**Postconditions**  
1. Distinct SKU variant records are created with pricing tiers.  
2. Inventory tracking records are initialized for the new SKUs.

---

### 11. UC-11: Nhập và xuất dữ liệu sản phẩm (Import and Export Product Data)

**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to bulk import sanitary ware products and SKUs using standardized Excel templates and export catalog datasets so that large-scale product catalogs can be rapidly initialized, audited, and maintained.  

**Preconditions**  
1. The Store Owner is authenticated and has catalog import/export permissions.  
2. The "Import / Export Products" tool is accessible in the Admin Portal.  

**Normal Flow**  
1. The Store Owner navigates to the "Product Import / Export" page.  
2. The Store Owner clicks "Download Excel Template" to obtain the standardized spreadsheet schema (containing columns for Product Code, Name, Category Code, Brand, SKU, Attributes, Cost Price, Selling Price, Specs).  
3. The Store Owner fills in product rows in the Excel file offline.  
4. The Store Owner clicks "Upload File" and selects the completed `.xlsx` file.  
5. The system parses the file and performs pre-validation:  
   - Validates column headers against schema.  
   - Verifies referenced Categories and Brands exist.  
   - Checks for duplicate SKUs within the file and against existing database records.  
   - Validates data formats (numeric prices, non-empty mandatory fields).  
6. The system displays a Preview Grid showing Total Rows, Valid Rows (e.g., 150), and Error Rows (0).  
7. The Store Owner clicks "Execute Import".  
8. The system batch-inserts products and SKU variants in a transactional database operation.  
9. The system logs the bulk import event in the Activity Log.  
10. The system displays MSG42: "Import completed successfully: 150 products and SKUs created/updated."  

**Alternative Flows**  
- **Step 5 - Validation Errors in Import File**:  
  1. The system detects validation errors (e.g., Category Code does not exist, invalid price format on Row 14 and Row 28).  
  2. The system flags the invalid rows with explicit error messages.  
  3. The system allows the Store Owner to download an Error Report spreadsheet containing highlighted issues.  
  4. The Store Owner corrects the spreadsheet and re-uploads.  
- **Step 1 - Export Products Sub-flow**:  
  1. The Store Owner filters products by Category or Brand and clicks "Export to Excel".  
  2. The system compiles all matching master products, variants, and pricing tiers into a formatted `.xlsx` workbook.  
  3. The system initiates browser download of the file.  

**Postconditions**  
1. Master products and variants are imported or updated in bulk.  
2. An import summary log entry is created with row counts and user ID.

---

### 12. UC-12: Tìm kiếm và xem thông tin sản phẩm (Search and View Product Information)

**Primary Actors**  
Guest, Customer, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a user or sales staff, I want to search and filter sanitary ware products by category, brand, price range, technical specifications, and real-time stock availability so that I can evaluate fixtures, verify compatibility, and check inventory.  

**Preconditions**  
1. The AquaSuite storefront or POS Product Search interface is loaded.  

**Normal Flow**  
1. The user navigates to the Product Catalog or enters a search term in the search bar (e.g., "TOTO smart toilet 305mm").  
2. The system performs full-text search and renders the product catalog grid with faceted filter sidebars:  
   - Category (Toilets, Basins, Showers, Bathtubs).  
   - Brand (TOTO, Inax, Caesar, Viglacera).  
   - Price Range slider.  
   - Technical Specifications (Flushing type, Rough-in dimension, Ceramic glaze technology).  
   - Availability toggle ("In Stock Only").  
3. The user applies filters (e.g., Brand = "TOTO", Category = "Smart Toilets", In Stock = Yes).  
4. The system refreshes results instantly, displaying product cards with thumbnail image, product name, brand badge, starting price, and stock status badge (`In Stock` / `Pre-order`).  
5. The user clicks on a product card to view the Product Details page.  
6. The system displays the comprehensive product page:  
   - Image gallery with zoom capabilities and technical CAD blueprints.  
   - Variant selector (Color, Rough-in dimension) dynamically updating SKU price and stock count.  
   - Real-time stock status (For Sales Staff: warehouse-level breakdown; For Customer: general availability).  
   - Complete technical specifications table and downloadable installation manual PDF.  
   - Warranty terms (e.g., 10-year ceramic warranty, 2-year electronic valve warranty).  

**Alternative Flows**  
- **Step 2 - No Products Found**:  
  1. No products match the search query or combined filters.  
  2. The system displays MSG43: "No products found matching your criteria. Try adjusting your filters or search keywords."  
  3. The system provides links to top popular categories (Smart Toilets, Washbasins).  
- **Step 6 - Out of Stock Variant Selected**:  
  1. The user selects a variant currently with zero inventory.  
  2. The system displays a "Temporarily Out of Stock - Contact Sales for Reservation" badge and disables the "Instant Buy" button while enabling "Notify Me When Available" or "Add to Quote".  

**Postconditions**  
1. Product views and search events are tracked for analytics.  
2. Selected product details are loaded for cart addition or quote generation.

---

### 13. UC-13: Xem và quản lý gói sản phẩm (View and Manage Product Combos)

**Primary Actors**  
Store Owner, Sales Staff, Customer, Guest  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, Sales Staff, or Customer, I want to configure and browse packaged bathroom fixture combos (e.g., Complete Master Bathroom Suite: Smart Toilet, Vanity Sink, Thermostatic Shower Set, and 6-piece Accessory Kit) with bundled package discounts so that customers can purchase coordinated bathroom sets at favorable pricing.  

**Preconditions**  
1. For Store Owner: Logged in with catalog permissions to create/edit combos.  
2. For Customer/Sales: Accessing the "Bathroom Combos & Bundles" section.  

**Normal Flow**  
1. The Store Owner navigates to the "Product Combos & Packages" manager in the Admin Portal.  
2. The system displays active combo packages with Package Name, Thumbnail, Linked Items Count, Original Total Price, Package Special Price, and Discount %.  
3. The Store Owner clicks "Create Combo Package".  
4. The system opens the combo creation modal.  
5. The Store Owner enters Combo Title (e.g., "Luxury Master Suite Package - TOTO Neorest"), description, and uploads package promotional banner to Cloudinary.  
6. The Store Owner searches and adds constituent SKU items to the bundle:  
   - 1x TOTO Neorest Smart Toilet (Retail: 68,000,000 VND).  
   - 1x Countertop Basin Sink (Retail: 12,000,000 VND).  
   - 1x Tall Basin Mixer Tap (Retail: 6,500,000 VND).  
   - 1x Thermostatic Rain Shower System (Retail: 18,500,000 VND).  
   - 1x 6-piece Solid Brass Accessory Set (Retail: 5,000,000 VND).  
7. The system computes total standard retail sum: `110,000,000 VND`.  
8. The Store Owner sets Package Promotional Price: `95,000,000 VND` (Saving: 15,000,000 VND / 13.6%).  
9. The Store Owner sets validity dates and stock reservation rules (whether combo checks real-time component SKU inventory).  
10. The Store Owner clicks "Publish Combo".  
11. The system saves the combo package and publishes it to the storefront and POS quote builder with MSG44: "Bathroom package created successfully."  

**Alternative Flows**  
- **Step 2 - Customer Browses and Purchases Combo**:  
  1. A Customer navigates to "Bathroom Packages" on the website.  
  2. The system renders the bundled fixtures with high-resolution bathroom layout imagery.  
  3. The Customer selects component options (e.g., choosing Chrome or Matte Black finish for shower/faucets).  
  4. The Customer clicks "Add Complete Package to Cart".  
  5. The system adds all constituent SKUs with package discount applied to the cart.  
- **Step 6 - Component SKU Depleted in Warehouse**:  
  1. One of the mandatory SKUs inside a combo runs out of stock in the warehouse.  
  2. The system flags the combo as "Partially Out of Stock" on the storefront, showing which fixture is depleted and offering similar substitute SKUs or pre-order reservation.  

**Postconditions**  
1. The combo package definition and component mappings are stored in the database.  
2. Combo pricing is recognized across POS sales, project quotations, and storefront checkout.

---

### 14. UC-14: Tư vấn và gợi ý sản phẩm bằng AI (AI Consultation and Product Recommendation)

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

### 15. UC-15: Quản lý yêu cầu và lịch sử tư vấn (Manage Consultation Requests and History)

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

### 16. UC-16: Chuyển tư vấn thành báo giá hoặc đơn hàng (Convert Consultation to Quotation or Order)

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

---

### 17. UC-17: Quản lý giỏ hàng (Manage Shopping Cart)

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

### 18. UC-18: Đặt hàng và thanh toán trực tuyến (Place Online Order and Payment)

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

### 19. UC-19: Theo dõi đơn hàng của khách hàng (Track Customer Orders)

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

### 20. UC-20: Yêu cầu thay đổi hoặc hủy đơn hàng (Request Order Modification or Cancellation)

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

---

### 21. UC-21: Tạo và quản lý đơn hàng bán (Create and Manage Sales Orders)

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

### 22. UC-22: Xác nhận và xử lý đơn hàng online (Confirm and Process Online Orders)

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

### 23. UC-23: Quản lý thanh toán và xác minh thanh toán (Manage Payment and Payment Verification)

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

### 24. UC-24: Quản lý công nợ khách hàng (Manage Customer Debt and Receivables)

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

### 25. UC-26: Xử lý yêu cầu thay đổi hoặc hủy đơn (Process Order Modification or Cancellation Requests)

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

### 26. UC-27: Quản lý báo giá dự án (Manage Project Quotations)

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

### 27. UC-28: Chuyển báo giá thành đơn hàng (Convert Quotation to Sales Order)

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

### 28. UC-29: Quản lý chiết khấu, đặt cọc và giữ hàng (Manage Discounts, Deposits, and Stock Reservation)

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

### 29. UC-30: Quản lý thu hộ COD và bàn giao tiền (Manage COD Collection and Remittance)

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

---

### 30. UC-31: Tra cứu và quản lý thông tin tồn kho (Look up and Manage Inventory Information)

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

### 31. UC-32: Quản lý nhập kho (Manage Goods Receipt)

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

### 32. UC-33: Quản lý nhà cung cấp (Manage Suppliers)

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

### 33. UC-34: Quản lý đơn nhập hàng (Manage Purchase Orders)

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

### 34. UC-36: Quản lý giữ hàng (Manage Stock Reservations)

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

### 35. UC-37: Xử lý hàng trả về và trả nhà cung cấp (Process Returned Goods and Vendor Returns)

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

### 36. UC-38: Nhập/xuất dữ liệu tồn kho (Import and Export Inventory Data)

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

---

### 37. UC-39: Quản lý phiếu xuất kho (Manage Goods Issue Notes)

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

### 38. UC-40: Lập và quản lý đợt giao hàng (Create and Manage Delivery Dispatches)

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

### 39. UC-41: Thực hiện và cập nhật kết quả giao hàng (Execute and Update Delivery Results)

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

### 40. UC-42: Theo dõi lịch sử và tiến độ giao hàng (Track Delivery Progress and History)

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

### 41. UC-43: Tra cứu chuyến giao được phân công (Look up Assigned Delivery Trips)

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

---

### 42. UC-44: Quản lý chính sách bảo hành (Manage Warranty Policies)

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

### 43. UC-45: Quản lý hồ sơ bảo hành (Manage Warranty Records)

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

### 44. UC-46: Tiếp nhận và xử lý yêu cầu bảo hành (Receive and Process Warranty Claims)

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

### 45. UC-47: Gửi yêu cầu bảo hành trực tuyến (Submit Online Warranty Claim)

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

### 46. UC-48: Tra cứu bảo hành của khách hàng (Look up Warranty Information)

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

---

### 47. UC-49: Quản lý hồ sơ khách hàng và liên kết tài khoản (Manage Customer Profiles and Account Linking)

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

### 48. UC-50: Tra cứu lịch sử khách hàng và mua hàng (Look up Customer History and Purchases)

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

### 49. UC-51: Quản lý công trình/dự án khách hàng (Manage Customer Construction Projects)

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

---

### 50. UC-53: Xem dashboard và cảnh báo vận hành (View Operations Dashboard and Alerts)

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

### 51. UC-54: Xem báo cáo kinh doanh và doanh số (View Sales and Revenue Reports)

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

### 52. UC-55: Xem báo cáo tồn kho và công nợ (View Inventory and Debt Reports)

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

### 53. UC-56: Quản lý chi phí vận hành và xuất báo cáo (Manage Operating Expenses and Export Reports)

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

---

### 54. UC-58: Tra cứu dữ liệu kinh doanh qua Telegram (Query Business Data via Telegram)

**Primary Actors**  
Store Owner  

**Secondary Actors**  
Telegram Service, OpenAI  

**Description**  
As a Store Owner, I want to query store business performance, real-time sales figures, SKU stock availability, and overdue contractor debts by sending natural language prompts via Telegram so that I can monitor operations and make quick executive decisions on-the-go without logging into the web portal.  

**Preconditions**  
1. The Store Owner has previously linked and authenticated their verified Telegram Chat ID with their AquaSuite account credentials.  
2. The AquaSuite Telegram Bot service is active and listening for incoming webhooks.  
3. Read-only constraint: Per system constraints, the Telegram Assistant performs read-only data queries and never executes data modification or deletion commands.  

**Normal Flow**  
1. The Store Owner opens the Telegram application on their mobile phone and accesses the official AquaSuite AI Bot chat.  
2. The Store Owner types a natural language query in Vietnamese or English (e.g., "Báo cáo doanh thu hôm nay và danh sách đơn hàng chưa giao" / "Today's revenue report and pending delivery orders").  
3. The Telegram Service receives the message and triggers the AquaSuite Telegram Webhook endpoint.  
4. The system validates the sender's Telegram User ID against the authorized Store Owner profile and confirms active permissions.  
5. The system forwards the query text and schema context to OpenAI to parse intent, identify query parameters (Entity: Revenue, Date: Today; Entity: Orders, Status: Pending Delivery), and generate structured SQL/GraphQL read queries.  
6. The system executes the read-only query against the database, computing:  
   - Today's Net Revenue: 85,600,000 VND (12 orders).  
   - Top Selling SKU: TOTO Neorest (2 units).  
   - Pending Delivery Dispatches: 3 orders scheduled for afternoon delivery.  
7. The system formats the result into a clean, markdown-formatted Telegram response message with emoji indicators and summary bullet points.  
8. The system calls the Telegram Service Bot API to send the reply back to the Store Owner's chat.  
9. The Store Owner receives and reviews the business metrics in their Telegram app.  
10. The system logs the query, AI prompt tokens, and response in the Telegram Bot interaction log.  

**Alternative Flows**  
- **Step 4 - Unauthorized Telegram User**:  
  1. An unregistered Telegram account or non-owner user attempts to send a query to the bot.  
  2. The system rejects the request, blocks data access, and replies: "Unauthorized access. This bot is restricted to authorized AquaSuite administrators."  
  3. The system logs the unauthorized attempt in the security audit log.  
- **Step 2 - Query Specific SKU Inventory Balance**:  
  1. The Store Owner asks: "Kho chính còn bao nhiêu bồn cầu thông minh TOTO Neorest?" (How many TOTO Neorest smart toilets are left in the main warehouse?).  
  2. The AI identifies SKU and warehouse entity, queries real-time stock (Physical: 8, Reserved: 3, Available: 5).  
  3. The bot replies with detailed breakdown and rack location.  
- **Step 2 - Query Overdue Contractor Debts**:  
  1. The Store Owner asks: "Những nhà thầu nào đang nợ quá hạn trên 30 ngày?" (Which contractors have debts overdue past 30 days?).  
  2. The bot retrieves aging debt records from FE-14 and returns a formatted list of delinquent contractors and amounts.  
- **Step 2 - User Attempts Data Modification via Telegram**:  
  1. The Store Owner types: "Xóa đơn hàng ORD-0012" (Delete order ORD-0012) or "Giảm giá 20% cho khách Nam" (Give 20% discount).  
  2. The system detects a write/mutation request and refuses execution per system constraints: "AquaSuite Telegram Assistant is strictly read-only. Data modifications, order cancellations, and price adjustments must be executed directly in the AquaSuite Web Admin Portal."  
- **Step 5 - OpenAI Service Latency / Fallback**:  
  1. If OpenAI API experiences latency, the bot recognizes standard shortcut commands (e.g., `/doanhthu`, `/tonkho`, `/congno`) and responds via deterministic pre-compiled queries.  

**Postconditions**  
1. Business query results are delivered securely to the Store Owner's Telegram chat.  
2. The interaction and audit record are logged; no underlying business database records are modified.

---

### 55. UC-59: Tra cứu công việc và lịch kỹ thuật (Look up Technical Jobs and Schedule)

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

### 56. UC-60: Tiếp nhận và thực hiện công việc kỹ thuật (Accept and Execute Technical Jobs)

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

### 57. UC-61: Ghi nhận kết quả và hoàn thành công việc (Record Results and Complete Technical Jobs)

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

---

### 58. UC-62: Tra cứu và quản lý giao dịch tài chính (Look up and Manage Financial Transactions)

**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to search and filter cash and bank transaction ledgers, cash drawer balances, and bank account movements so that all monetary flows within the business are auditable and reconciled.  

**Preconditions**  
1. The user is logged in with accounting and financial permissions.  
2. The Financial Transactions screen is accessible in the Back-Office system.  

**Normal Flow**  
1. The Accountant navigates to the "Financial Transactions Ledger" screen.  
2. The system renders the comprehensive transaction ledger: Transaction ID (e.g., `TXN-20261013-0089`), Transaction Date/Time, Transaction Type (`Receipt / Inflow`, `Payment Voucher / Outflow`), Account Type (`Cash Drawer`, `Bank Account - Vietcombank`, `Bank Account - Techcombank`), Amount, Reference Entity (Sales Order, Purchase Invoice, Expense Voucher), Payer/Payee, and Status (`Posted`, `Voided`).  
3. The Accountant filters transactions by Date Range = "Today", Account Type = "Bank Account - Vietcombank", and Type = "Receipt".  
4. The system queries the financial ledger and updates the grid with matching transaction rows, displaying running account balance.  
5. The Accountant clicks on a transaction row to view the transaction details drawer.  
6. The system displays the full accounting breakdown: debited account, credited account, transaction description, originating cashier/staff user, and attached transaction receipts.  
7. The Accountant verifies the entries and closes the drawer.  

**Alternative Flows**  
- **Step 3 - Export Financial Transaction Journal**:  
  1. The Accountant selects a month and clicks "Export General Ledger (Excel)".  
  2. The system formats and downloads the complete double-entry journal spreadsheet.  
- **Step 5 - Filter by Specific Order or Invoice Reference**:  
  1. The Accountant enters Order Code `ORD-20261013-0045` in the search box.  
  2. The system filters all payment transactions (e.g., Initial Deposit + Final Settlement) tied to that specific order.  

**Postconditions**  
1. Financial transactions are audited without altering immutable accounting journal records.

---

### 59. UC-63: Quản lý phiếu thu và phân bổ thanh toán (Manage Receipts and Payment Allocation)

**Primary Actors**  
Accountant  

**Secondary Actors**  
None  

**Description**  
As an Accountant, I want to create official cash and bank receipt vouchers, allocate customer payments across multiple outstanding sales invoices or contractor debt lines, and issue formal electronic receipts so that customer balances are settled accurately.  

**Preconditions**  
1. The Accountant is authenticated with cashiering/receipt creation privileges.  
2. Customer payment has been received (via cash, bank wire, or COD remittance from UC-30).  

**Normal Flow**  
1. The Accountant opens the "Receipt Vouchers" management screen and clicks "Create Receipt Voucher".  
2. The system opens the voucher creation form with an auto-generated Voucher ID (e.g., `RCT-202610-0042`).  
3. The Accountant selects the Customer / Contractor (e.g., "Song Hong Construction Co.") and Receipt Method (`Cash Drawer` or `Bank Account`).  
4. The Accountant inputs total payment received: `50,000,000 VND`.  
5. The system automatically fetches and displays all unpaid and partially paid sales orders/invoices for that contractor, sorted by oldest due date (FIFO).  
6. The Accountant chooses payment allocation:  
   - Clicks "Auto-Allocate (FIFO)": System automatically allocates funds to clear Invoice 1 (20,000,000 VND), Invoice 2 (25,000,000 VND), and applies the remaining 5,000,000 VND towards Invoice 3.  
   - Or manually enters specific allocation amounts against individual orders.  
7. The Accountant enters the receipt description: "Debt installment payment for Riverside Villa bathrooms Phase 1".  
8. The Accountant clicks "Post Receipt Voucher".  
9. The system commits the transaction to the cash/bank ledger, updates invoice paid balances and order payment statuses, reduces the customer's outstanding accounts receivable balance, and generates a printable Electronic Receipt Voucher.  
10. The system displays MSG95: "Receipt voucher created and payments allocated successfully."  

**Alternative Flows**  
- **Step 4 - Customer Payment Exceeds Total Open Invoices (Unallocated Credit)**:  
  1. The customer pays more than their outstanding balance.  
  2. The system allocates 100% of open invoices and posts the excess amount as "Customer Advance Credit" (Prepayment).  
  3. The customer credit balance is stored and can be deducted from future project orders.  
- **Step 8 - Void Posted Receipt Voucher with Audit Justification**:  
  1. The Accountant discovers a receipt was posted against the wrong customer account.  
  2. The Store Owner and Accountant execute a "Reversal / Void Voucher" action, entering reason: "Wrong contractor selected by cashier".  
  3. The system generates a compensating reversal entry, restores the invoice balances, and logs the action in the Activity Log.  

**Postconditions**  
1. Receipt voucher is posted to the financial ledger.  
2. Linked sales order balances and customer debt ledgers are updated.

---

### 60. UC-64: Quản lý phiếu chi và chứng từ (Manage Payment Vouchers and Documents)

**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As an Accountant or Store Owner, I want to create, review, approve, and record official payment vouchers (for supplier invoice payments, showroom rent, logistics freight, and technician service fees) with attached tax invoices and receipts so that store disbursements are controlled and documented.  

**Preconditions**  
1. The user has disbursement management permissions.  
2. The Payment Vouchers screen is accessible.  

**Normal Flow**  
1. The Accountant navigates to "Payment Vouchers" and clicks "Create Payment Voucher".  
2. The system initializes the form with an auto-generated Voucher ID (e.g., `PV-202610-0028`).  
3. The Accountant selects Payment Category: "Supplier Purchase Invoice Payment", "Store Operating Expense", or "Freight / Logistics".  
4. The Accountant selects Payee (e.g., "TOTO Vietnam Co., Ltd.") and Disbursement Account (e.g., "Bank - Techcombank").  
5. For supplier payment: The system lists open Purchase Orders / Goods Receipt Notes (from FE-07). The Accountant selects `GRN-TOTO-0012` with invoice balance `120,000,000 VND`.  
6. The Accountant enters disbursement amount (`120,000,000 VND`) and bank transfer reference code.  
7. The Accountant uploads the electronic VAT invoice (e-invoice XML/PDF) and bank wire confirmation slip.  
8. The system uploads files to Cloudinary and saves document URLs.  
9. If payment exceeds 20,000,000 VND, the system flags the voucher as `Pending_Owner_Approval`.  
10. The Store Owner reviews the voucher and clicks "Approve Disbursement".  
11. The system posts the payment voucher, deducts funds from the bank account balance, reduces Accounts Payable for TOTO Vietnam, and updates status to `Posted`.  
12. The system displays MSG96: "Payment voucher approved and posted successfully."  

**Alternative Flows**  
- **Step 10 - Store Owner Rejects Payment Voucher**:  
  1. The Store Owner rejects the voucher with note: "Discrepancy in supplier invoice amount. Hold payment pending vendor credit note."  
  2. The system sets status to `Rejected` and notifies the Accountant to investigate.  
- **Step 3 - Petty Cash Disbursement (Under 2,000,000 VND)**:  
  1. The Accountant logs an immediate petty cash voucher for showroom cleaning supplies (`350,000 VND`).  
  2. Because the amount is below the approval threshold, the system auto-approves and immediately posts the cash deduction.  

**Postconditions**  
1. Cash/bank ledger is reduced by the disbursement amount.  
2. Supplier accounts payable or operating expense accounts are updated.  
3. Supporting documentation is securely archived.

---

### 61. UC-65: Quản lý và đối soát công nợ phải thu (Manage and Reconcile Accounts Receivable)

**Primary Actors**  
Accountant  

**Secondary Actors**  
Email Service, SMS Service  

**Description**  
As an Accountant, I want to track customer aging debts, generate formal debt reconciliation statements, and dispatch periodic debt confirmation letters and reminders to contractors so that trade receivables are collected on time.  

**Preconditions**  
1. The Accountant is authenticated with debt control permissions.  
2. Customers with outstanding balances exist in the system.  

**Normal Flow**  
1. The Accountant navigates to "Accounts Receivable & Debt Reconciliation".  
2. The system renders the AR ledger table: Customer Name, Contractor Company, Total Receivables, Current (0-30 days), Overdue 31-60 days, Overdue > 60 days, and Last Reconciliation Date.  
3. The Accountant selects a contractor (e.g., "An Gia Investment Corp") and clicks "Generate Reconciliation Statement".  
4. The system aggregates all transactions within the selected period (Opening balance, Invoiced deliveries, Cash/Bank receipts, Returns/Credit memos, and Closing balance).  
5. The Accountant reviews the itemized transactions and clicks "Send Debt Reconciliation Statement".  
6. The system compiles an official Debt Reconciliation Statement (PDF) formatted with legal confirmation signature boxes.  
7. The system triggers the Email Service to deliver the statement to the contractor's accounting department and dispatches an SMS reminder.  
8. The system logs the reconciliation dispatch event with a 7-day confirmation deadline.  
9. The system displays MSG97: "Debt reconciliation statement generated and dispatched to contractor."  

**Alternative Flows**  
- **Step 8 - Contractor Confirms Statement with Zero Discrepancy**:  
  1. The contractor signs and returns the reconciliation statement.  
  2. The Accountant uploads the signed statement, sets status to `Reconciled_Agreed`, and logs the timestamp.  
- **Step 8 - Contractor Reports Discrepancy (e.g., Uncredited Returned Fixture)**:  
  1. The contractor claims an uncredited damaged faucet return of 2,500,000 VND.  
  2. The Accountant cross-checks with Warehouse Returns (FE-07 UC-37), confirms the credit memo was pending, posts the credit, and regenerates the statement.  

**Postconditions**  
1. Debt reconciliation status and audit records are updated.  
2. Official statement documents are archived in the customer dossier.

---

### 62. UC-66: Quản lý và đối soát công nợ phải trả (Manage and Reconcile Accounts Payable)

**Primary Actors**  
Accountant  

**Secondary Actors**  
None  

**Description**  
As an Accountant, I want to track accounts payable to sanitary ware manufacturers and distributors, review invoice payment due dates, and generate vendor debt reconciliation statements so that store debts are verified and paid accurately.  

**Preconditions**  
1. Goods receipts from suppliers (FE-07 UC-32) have been recorded.  
2. The Accounts Payable module is accessible.  

**Normal Flow**  
1. The Accountant opens the "Accounts Payable (Vendor Debt)" screen.  
2. The system displays a list of suppliers with financial balances: Supplier Code, Company Name (e.g., "Inax LIXIL Vietnam"), Total Invoiced Purchases, Total Paid, Current Outstanding Balance, and Upcoming Payment Due Dates (e.g., 30,000,000 VND due in 5 days).  
3. The Accountant selects a supplier to inspect the statement of account.  
4. The system renders the chronological vendor ledger: Goods Received Notes, Supplier Invoices, Returns & Discounts, and Payment Vouchers.  
5. The supplier emails their monthly vendor statement of account.  
6. The Accountant performs three-way matching against the system ledger (Purchase Order vs. Goods Received Note vs. Supplier Invoice).  
7. The Accountant confirms all line items and balances match.  
8. The Accountant marks the period as `Reconciled` and schedules eligible invoices for payment voucher creation (UC-64).  
9. The system displays MSG98: "Supplier accounts payable successfully reconciled."  

**Alternative Flows**  
- **Step 6 - Three-Way Matching Discrepancy Detected**:  
  1. The supplier's invoice charges 15,000,000 VND for an item, but the approved PO unit price was 14,000,000 VND.  
  2. The Accountant flags the line as `Discrepancy_Price_Mismatch` and contacts the supplier for a revised invoice before releasing payment.  

**Postconditions**  
1. Supplier payable balances are audited and confirmed.  
2. Verified invoices are queued for payment disbursement.

---

### 63. UC-67: Xem báo cáo thu chi và dòng tiền (View Cash Flow and Receipts/Payments Report)

**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to generate and view cash flow statements, opening/closing cash drawer and bank balances, and inflow/outflow summaries so that liquidity health and working capital can be evaluated.  

**Preconditions**  
1. The user has financial reporting permissions.  
2. The Cash Flow Reports screen is accessible.  

**Normal Flow**  
1. The user navigates to the "Cash Flow & Receipts/Payments Report" screen.  
2. The user selects the reporting period (e.g., "Current Month") and account scope ("Consolidated All Accounts", "Cash Only", or "Bank Accounts Only").  
3. The user clicks "Generate Cash Flow Statement".  
4. The system aggregates all posted receipt vouchers and payment vouchers, displaying:  
   - Opening Cash Balance at start of period.  
   - Operating Inflows: Showroom POS cash sales, Online VietQR payments, Contractor debt collections, Customer deposits.  
   - Operating Outflows: Supplier inventory purchases, Showroom operating expenses, Staff logistics stipends, Delivery truck fuel.  
   - Net Cash Flow for the period.  
   - Closing Cash Balance across all vaults and bank accounts.  
5. The system renders graphical trend charts comparing daily cash inflows vs. outflows.  
6. The user clicks "Export Cash Flow Statement (PDF/Excel)".  
7. The system renders and downloads the standardized financial statement.  

**Alternative Flows**  
- **Step 4 - Drill Down to Source Vouchers**:  
  1. The user clicks on any summarized inflow category (e.g., "Contractor Debt Collections: 145,000,000 VND").  
  2. The system opens an audit drawer listing every individual receipt voucher that contributed to that aggregate number.  

**Postconditions**  
1. Real-time cash position and historical cash flow metrics are presented.

---

### 64. UC-68: Xem báo cáo doanh thu và công nợ (View Revenue and Debt Report)

**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to view detailed accounting revenue statements, recognized revenue vs. deferred deposits, bad debt allowances, and net customer balances across projects so that financial statements reflect GAAP/VAS standards.  

**Preconditions**  
1. The user has access to executive financial statements.  
2. Financial transactions are posted for the target period.  

**Normal Flow**  
1. The user opens the "Accounting Revenue & Debt Analysis" screen.  
2. The user selects the financial quarter or fiscal year and clicks "Generate Statement".  
3. The system computes accounting figures:  
   - Gross Invoiced Revenue.  
   - Sales Deductions: Trade discounts, combo bundle rebates, return credits.  
   - Net Realized Revenue (recognized upon physical delivery handover).  
   - Deferred Revenue: Customer project deposits for unfulfilled orders (held as liabilities until goods issue).  
   - Net Accounts Receivable and Bad Debt Provisioning Allowance (e.g., 5% provision for debts overdue > 90 days).  
4. The system renders the project-by-project financial summary matrix.  
5. The user reviews the metrics, signs off on the accounting period, and exports the statement to Excel.  
6. The system displays MSG99: "Accounting revenue and debt report generated successfully."  

**Alternative Flows**  
- **Step 3 - Recalculate Bad Debt Provisions**:  
  1. The Accountant adjusts bad debt reserve parameters based on updated risk assessments.  
  2. The system recalculates net asset valuation and updates the statement.  

**Postconditions**  
1. Accounting revenue and bad debt provisions are computed and documented.

---

### 65. UC-69: Xem dashboard kế toán (View Accounting Dashboard)

**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to view real-time financial metrics, available cash balances across bank accounts, total pending payment verifications, and upcoming payable/receivable deadlines on an accounting dashboard so that financial control is immediate.  

**Preconditions**  
1. The user has accounting dashboard access privileges.  
2. The user accesses the Accounting module homepage.  

**Normal Flow**  
1. The user navigates to the "Accounting Dashboard".  
2. The system queries active ledgers and renders executive financial cards:  
   - Total Liquid Funds: Sum of Showroom Cash Drawer + Bank Accounts (e.g., `840,250,000 VND`).  
   - Pending Verifications: Number of unverified incoming bank wires awaiting reconciliation (e.g., 3 transactions / 24,500,000 VND).  
   - Accounts Receivable (AR) Summary: Total customer debt, including overdue portion flagged in red.  
   - Accounts Payable (AP) Summary: Total supplier debt, with invoices due within the next 7 days highlighted.  
3. The system renders interactive charts:  
   - 30-day rolling cash inflow vs. outflow velocity.  
   - Accounts Receivable Aging breakdown (0-30 days, 31-60 days, > 60 days).  
4. The user clicks on the "Pending Verifications" card to open the payment verification worklist (UC-70).  

**Alternative Flows**  
- **Step 2 - Bank Account Breakdown View**:  
  1. The user expands the "Liquid Funds" widget.  
  2. The system reveals individual balances for each bank account (Vietcombank Corporate Account: 520,000,000 VND; Techcombank Operational Account: 295,000,000 VND; Showroom Cash Vault: 25,250,000 VND).  

**Postconditions**  
1. Up-to-the-minute financial health indicators are displayed.

---

### 66. UC-70: Xử lý các khoản thanh toán chờ xác minh (Process Pending Payment Verifications)

**Primary Actors**  
Accountant  

**Secondary Actors**  
None  

**Description**  
As an Accountant, I want to review unverified bank transfer alerts, VietQR transactions, and customer payment slips, match them against pending sales orders, and confirm or reject payments so that orders can proceed to packing without revenue leaks.  

**Preconditions**  
1. The Accountant is logged into the system.  
2. Unverified payments exist in status `Pending_Verification` (from FE-05 UC-18 or FE-06 UC-23).  

**Normal Flow**  
1. The Accountant opens the "Payment Verification Queue" screen.  
2. The system lists all unverified payments: Payment ID, Submission Timestamp, Customer Name, Phone, Target Order Code, Payment Method (`Bank Wire`, `VietQR`), Claimed Amount (e.g., `18,500,000 VND`), Bank Transfer Memo, and Uploaded Slip Image.  
3. The Accountant selects an unverified payment item to review.  
4. The system displays the customer's uploaded bank transaction screenshot side-by-side with the store's electronic bank statement feed.  
5. The Accountant confirms that:  
   - Bank reference number matches the bank transaction log.  
   - Credited amount matches the claimed order payment exactly (18,500,000 VND).  
   - Beneficiary account is the official AquaSuite store account.  
6. The Accountant clicks "Confirm & Post Payment".  
7. The system updates the payment status to `Verified`, sets the target Sales Order's payment status to `Paid` (or `Partially_Paid`), and posts an official cash receipt voucher in FE-14 (UC-63).  
8. The system updates the order fulfillment status to `Processing`, releasing it for warehouse goods issue (FE-08 UC-39).  
9. The system displays MSG100: "Payment verified successfully. Order released for fulfillment."  

**Alternative Flows**  
- **Step 5 - Payment Discrepancy / Fraudulent Slip (Rejection Flow)**:  
  1. The Accountant investigates the bank statement and finds no matching credit entry (or discovers an edited/fake screenshot).  
  2. The Accountant clicks "Reject Payment".  
  3. The system prompts for Rejection Reason: "No matching credit found on bank statement as of 13/10/2026", "Transferred amount is less than claimed", or "Invalid reference memo".  
  4. The Accountant submits the rejection.  
  5. The system sets payment status to `Rejected`, alerts Sales Staff to contact the customer, and keeps the order on hold.  
  6. The system sends an email notification to the customer informing them of the payment verification failure and requesting a valid bank trace.  
- **Step 5 - Transferred Amount Differs from Order Value (Partial Payment)**:  
  1. Order total is 20,000,000 VND, but customer wire transfer is verified at only 15,000,000 VND.  
  2. The Accountant verifies the 15,000,000 VND.  
  3. The system credits 15,000,000 VND, sets order payment status to `Partially_Paid` with remaining balance 5,000,000 VND, and prompts Sales Staff to collect the remainder before final delivery.  

**Postconditions**  
1. Payment record is verified and posted to the general ledger.  
2. Associated sales order transitions to the fulfillment pipeline.  
3. Verification outcome is logged in the Activity Log with the verifying Accountant's user ID.

---
