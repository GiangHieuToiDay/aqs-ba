# AQUASUITE — DETAILED USE CASE SPECIFICATIONS (66 USE CASES)

> **Project:** AquaSuite — Sanitary Ware & Bathroom Equipment Store Management System  
> **Source Catalog:** `AquaSuite_Final_Use_Case_Catalog_66 (1).md` (Consolidated 66 Use Cases)  
> **Standard:** RUP / Agile Formal Use Case Specification Standard  
> **Actor Model:** System Context Diagram (7 Primary Actors & 6 Secondary Actors)  
> **Scope:** 14 Modules — 66 Core Use Cases (UC-01 to UC-70; UC-25, UC-35, UC-52, UC-57 reserved)

> **Quy cách tài liệu gộp**: Toàn bộ 66 Use Case đã được gộp thành một tập tin duy nhất đánh số từ 1 đến 66 tại [`usecase_descriptions.md`](./usecase_descriptions.md).

---

## 1. System Actors Catalog

Based on the system context diagram, AquaSuite defines the following actors:

### 1.1. Primary Actors (Human Actors)
| Actor | Role Category | Key Responsibilities & System Interactions |
|---|---|---|
| **Store Owner** | Internal / Management | Highest administrative authority. Manages accounts, roles, product catalog, pricing, suppliers, purchase orders, inventory, reports, and AI Telegram queries. |
| **Sales Staff** | Internal / Operations | Handles showroom/POS sales, online order verification, customer consultations, project quotes, deposits, and delivery staging requests. |
| **Technical Staff** | Internal / Field Service | Conducts bathroom site surveys, fixture installation, water pressure/piping testing, maintenance, and on-site warranty repair services. |
| **Shipper** | Internal / Field Delivery | Delivers sanitary ware fixtures to customer sites/villas/apartments, collects COD payments, captures delivery photos, and remits cash. |
| **Accountant** | Internal / Financial | Manages financial vouchers (receipts/payments), bank reconciliations, customer & supplier debts, cash flow, and financial statements. |
| **Guest** | External / Public | Unregistered visitor browsing sanitary ware catalogs, checking fixture specifications, using the AI consultation chatbot, and registering an account. |
| **Customer** | External / End-User | Registered customer/homeowner/contractor placing online orders, tracking deliveries, managing project addresses, and submitting warranty claims. |

### 1.2. Secondary Actors (External Services & Subsystems)
| Actor | Type | Interfacing Capabilities |
|---|---|---|
| **Email Service** | External Service | Transmits registration OTPs, order confirmation receipts, project quote PDFs, and password recovery emails. |
| **Cloudinary** | External Cloud Storage | Stores product gallery images, technical specification drawings, installation completion photos, and payment slips. |
| **Google Authentication Service** | External Identity Provider | Provides OAuth 2.0 single sign-on authentication for rapid customer login and registration. |
| **OpenAI** | External AI Engine | Powers the conversational AI consultation assistant (bathroom design & fixture selection) and natural language parsing. |
| **SMS Service** | External Telecom Gateway | Dispatches mobile OTPs, delivery status SMS/ZNS notifications, and debt reconciliation reminders. |
| **Telegram Service** | External Messaging Platform | Serves as the channel for Store Owner on-the-go business queries and executive KPI alerts. |

---

## 2. Master Catalog of 66 Consolidated Use Cases

| Module ID | Module Name | Use Case ID | Use Case Name | Primary Actor | Secondary Actors | File Reference |
|:---:|:---|:---:|:---|:---|:---|:---|
| **FE-01** | Accounts & Access Control | **UC-01** | Register Customer Account | Guest | Email Service, SMS Service, Google Auth | [`FE-01_Account_And_Access_Control.md`](./FE-01_Account_And_Access_Control.md) |
| | | **UC-02** | User Login and Authentication | Customer, Staff, Store Owner | Google Authentication Service | [`FE-01_Account_And_Access_Control.md`](./FE-01_Account_And_Access_Control.md) |
| | | **UC-03** | Manage Profile and Delivery Addresses | Customer, Internal Staff | Cloudinary | [`FE-01_Account_And_Access_Control.md`](./FE-01_Account_And_Access_Control.md) |
| | | **UC-04** | Manage Password and Account Recovery | Customer, Internal Staff | Email Service, SMS Service | [`FE-01_Account_And_Access_Control.md`](./FE-01_Account_And_Access_Control.md) |
| | | **UC-05** | Manage User Accounts and Status | Store Owner | Email Service | [`FE-01_Account_And_Access_Control.md`](./FE-01_Account_And_Access_Control.md) |
| | | **UC-06** | Manage Roles and Permissions | Store Owner | None | [`FE-01_Account_And_Access_Control.md`](./FE-01_Account_And_Access_Control.md) |
| **FE-02** | System Audit Log | **UC-07** | Search and View Activity Logs | Store Owner | None | [`FE-02_System_Audit_Log.md`](./FE-02_System_Audit_Log.md) |
| **FE-03** | Product Catalog | **UC-08** | Manage Categories and Brands | Store Owner | Cloudinary | [`FE-03_Product_Management.md`](./FE-03_Product_Management.md) |
| | | **UC-09** | Manage Products | Store Owner | Cloudinary | [`FE-03_Product_Management.md`](./FE-03_Product_Management.md) |
| | | **UC-10** | Manage SKUs, Variants, and Pricing | Store Owner | None | [`FE-03_Product_Management.md`](./FE-03_Product_Management.md) |
| | | **UC-11** | Import and Export Product Data | Store Owner | None | [`FE-03_Product_Management.md`](./FE-03_Product_Management.md) |
| | | **UC-12** | Search and View Product Information | Guest, Customer, Sales Staff | None | [`FE-03_Product_Management.md`](./FE-03_Product_Management.md) |
| | | **UC-13** | View and Manage Product Combos | Store Owner, Customer | Cloudinary | [`FE-03_Product_Management.md`](./FE-03_Product_Management.md) |
| **FE-04** | AI Consultation | **UC-14** | AI Consultation & Product Recommendation | Guest, Customer | OpenAI, Cloudinary | [`FE-04_AI_Consultation.md`](./FE-04_AI_Consultation.md) |
| | | **UC-15** | Manage Consultation Requests and History | Sales Staff, Store Owner | None | [`FE-04_AI_Consultation.md`](./FE-04_AI_Consultation.md) |
| | | **UC-16** | Convert Consultation to Quote or Order | Sales Staff | Email Service | [`FE-04_AI_Consultation.md`](./FE-04_AI_Consultation.md) |
| **FE-05** | Online Shopping | **UC-17** | Manage Shopping Cart | Customer, Guest | None | [`FE-05_Online_Shopping.md`](./FE-05_Online_Shopping.md) |
| | | **UC-18** | Place Online Order and Payment | Customer | Email Service, SMS Service | [`FE-05_Online_Shopping.md`](./FE-05_Online_Shopping.md) |
| | | **UC-19** | Track Customer Orders | Customer, Guest | None | [`FE-05_Online_Shopping.md`](./FE-05_Online_Shopping.md) |
| | | **UC-20** | Request Order Modification or Cancel | Customer | Email Service | [`FE-05_Online_Shopping.md`](./FE-05_Online_Shopping.md) |
| **FE-06** | Sales & Quotations | **UC-21** | Create and Manage Sales Orders | Sales Staff, Store Owner | None | [`FE-06_Sales_And_Quotations.md`](./FE-06_Sales_And_Quotations.md) |
| | | **UC-22** | Confirm and Process Online Orders | Sales Staff | Email Service, SMS Service | [`FE-06_Sales_And_Quotations.md`](./FE-06_Sales_And_Quotations.md) |
| | | **UC-23** | Manage Payment and Payment Verification | Sales Staff, Store Owner | Cloudinary | [`FE-06_Sales_And_Quotations.md`](./FE-06_Sales_And_Quotations.md) |
| | | **UC-24** | Manage Customer Debt & Receivables | Sales Staff, Store Owner | SMS Service | [`FE-06_Sales_And_Quotations.md`](./FE-06_Sales_And_Quotations.md) |
| | | **UC-26** | Process Order Change or Cancel Requests | Sales Staff, Store Owner | Email Service | [`FE-06_Sales_And_Quotations.md`](./FE-06_Sales_And_Quotations.md) |
| | | **UC-27** | Manage Project Quotations | Sales Staff, Store Owner | Email Service | [`FE-06_Sales_And_Quotations.md`](./FE-06_Sales_And_Quotations.md) |
| | | **UC-28** | Convert Quotation to Sales Order | Sales Staff | None | [`FE-06_Sales_And_Quotations.md`](./FE-06_Sales_And_Quotations.md) |
| | | **UC-29** | Manage Discounts, Deposits & Reservations | Sales Staff, Store Owner | None | [`FE-06_Sales_And_Quotations.md`](./FE-06_Sales_And_Quotations.md) |
| | | **UC-30** | Manage COD Collection and Remittance | Sales Staff, Accountant, Shipper | None | [`FE-06_Sales_And_Quotations.md`](./FE-06_Sales_And_Quotations.md) |
| **FE-07** | Inventory & Purchasing | **UC-31** | Look up and Manage Inventory Information | Store Owner, Sales Staff | None | [`FE-07_Inventory_And_Purchasing.md`](./FE-07_Inventory_And_Purchasing.md) |
| | | **UC-32** | Manage Goods Receipt | Store Owner | Cloudinary | [`FE-07_Inventory_And_Purchasing.md`](./FE-07_Inventory_And_Purchasing.md) |
| | | **UC-33** | Manage Suppliers | Store Owner | None | [`FE-07_Inventory_And_Purchasing.md`](./FE-07_Inventory_And_Purchasing.md) |
| | | **UC-34** | Manage Purchase Orders | Store Owner | Email Service | [`FE-07_Inventory_And_Purchasing.md`](./FE-07_Inventory_And_Purchasing.md) |
| | | **UC-36** | Manage Stock Reservations | Store Owner, Sales Staff | None | [`FE-07_Inventory_And_Purchasing.md`](./FE-07_Inventory_And_Purchasing.md) |
| | | **UC-37** | Process Returned Goods and Vendor Returns | Store Owner | Cloudinary | [`FE-07_Inventory_And_Purchasing.md`](./FE-07_Inventory_And_Purchasing.md) |
| | | **UC-38** | Import and Export Inventory Data | Store Owner | None | [`FE-07_Inventory_And_Purchasing.md`](./FE-07_Inventory_And_Purchasing.md) |
| **FE-08** | Warehouse & Delivery | **UC-39** | Manage Goods Issue Notes | Store Owner | None | [`FE-08_Warehouse_Issue_And_Delivery.md`](./FE-08_Warehouse_Issue_And_Delivery.md) |
| | | **UC-40** | Create and Manage Delivery Dispatches | Store Owner, Sales Staff | SMS Service | [`FE-08_Warehouse_Issue_And_Delivery.md`](./FE-08_Warehouse_Issue_And_Delivery.md) |
| | | **UC-41** | Execute and Update Delivery Results | Shipper | Cloudinary, SMS Service | [`FE-08_Warehouse_Issue_And_Delivery.md`](./FE-08_Warehouse_Issue_And_Delivery.md) |
| | | **UC-42** | Track Delivery Progress and History | Sales Staff, Customer, Owner | None | [`FE-08_Warehouse_Issue_And_Delivery.md`](./FE-08_Warehouse_Issue_And_Delivery.md) |
| | | **UC-43** | Look up Assigned Delivery Trips | Shipper | None | [`FE-08_Warehouse_Issue_And_Delivery.md`](./FE-08_Warehouse_Issue_And_Delivery.md) |
| **FE-09** | Warranty Management | **UC-44** | Manage Warranty Policies | Store Owner | None | [`FE-09_Warranty_Management.md`](./FE-09_Warranty_Management.md) |
| | | **UC-45** | Manage Warranty Records | Store Owner, Sales Staff | None | [`FE-09_Warranty_Management.md`](./FE-09_Warranty_Management.md) |
| | | **UC-46** | Receive and Process Warranty Claims | Sales Staff, Store Owner | Email Service, SMS Service | [`FE-09_Warranty_Management.md`](./FE-09_Warranty_Management.md) |
| | | **UC-47** | Submit Online Warranty Claim | Customer | Cloudinary | [`FE-09_Warranty_Management.md`](./FE-09_Warranty_Management.md) |
| | | **UC-48** | Look up Warranty Information | Customer, Guest | None | [`FE-09_Warranty_Management.md`](./FE-09_Warranty_Management.md) |
| **FE-10** | Customer CRM | **UC-49** | Manage Customer Profiles & Account Linking | Store Owner, Sales Staff | None | [`FE-10_Customer_Management.md`](./FE-10_Customer_Management.md) |
| | | **UC-50** | Look up Customer History and Purchases | Sales Staff, Store Owner | None | [`FE-10_Customer_Management.md`](./FE-10_Customer_Management.md) |
| | | **UC-51** | Manage Customer Construction Projects | Sales Staff, Store Owner | None | [`FE-10_Customer_Management.md`](./FE-10_Customer_Management.md) |
| **FE-11** | Reporting & Analytics | **UC-53** | View Operations Dashboard and Alerts | Store Owner | None | [`FE-11_Reporting_And_Analytics.md`](./FE-11_Reporting_And_Analytics.md) |
| | | **UC-54** | View Sales and Revenue Reports | Store Owner | None | [`FE-11_Reporting_And_Analytics.md`](./FE-11_Reporting_And_Analytics.md) |
| | | **UC-55** | View Inventory and Debt Reports | Store Owner, Accountant | None | [`FE-11_Reporting_And_Analytics.md`](./FE-11_Reporting_And_Analytics.md) |
| | | **UC-56** | Manage Operating Expenses & Export Reports | Store Owner, Accountant | None | [`FE-11_Reporting_And_Analytics.md`](./FE-11_Reporting_And_Analytics.md) |
| **FE-12** | AI Telegram Assistant | **UC-58** | Query Business Data via Telegram | Store Owner | Telegram Service, OpenAI | [`FE-12_AI_Telegram_Assistant.md`](./FE-12_AI_Telegram_Assistant.md) |
| **FE-13** | Technical Services | **UC-59** | Look up Technical Jobs and Schedule | Technical Staff | None | [`FE-13_Technical_Services.md`](./FE-13_Technical_Services.md) |
| | | **UC-60** | Accept and Execute Technical Jobs | Technical Staff | None | [`FE-13_Technical_Services.md`](./FE-13_Technical_Services.md) |
| | | **UC-61** | Record Results and Complete Technical Jobs | Technical Staff | Cloudinary | [`FE-13_Technical_Services.md`](./FE-13_Technical_Services.md) |
| **FE-14** | Accounting & Finance | **UC-62** | Look up and Manage Financial Transactions | Accountant, Store Owner | None | [`FE-14_Accounting_And_Finance.md`](./FE-14_Accounting_And_Finance.md) |
| | | **UC-63** | Manage Receipts and Payment Allocation | Accountant | None | [`FE-14_Accounting_And_Finance.md`](./FE-14_Accounting_And_Finance.md) |
| | | **UC-64** | Manage Payment Vouchers and Documents | Accountant, Store Owner | Cloudinary | [`FE-14_Accounting_And_Finance.md`](./FE-14_Accounting_And_Finance.md) |
| | | **UC-65** | Manage and Reconcile Accounts Receivable | Accountant | Email Service, SMS Service | [`FE-14_Accounting_And_Finance.md`](./FE-14_Accounting_And_Finance.md) |
| | | **UC-66** | Manage and Reconcile Accounts Payable | Accountant | None | [`FE-14_Accounting_And_Finance.md`](./FE-14_Accounting_And_Finance.md) |
| | | **UC-67** | View Cash Flow and Receipts/Payments Report | Accountant, Store Owner | None | [`FE-14_Accounting_And_Finance.md`](./FE-14_Accounting_And_Finance.md) |
| | | **UC-68** | View Revenue and Debt Report | Accountant, Store Owner | None | [`FE-14_Accounting_And_Finance.md`](./FE-14_Accounting_And_Finance.md) |
| | | **UC-69** | View Accounting Dashboard | Accountant, Store Owner | None | [`FE-14_Accounting_And_Finance.md`](./FE-14_Accounting_And_Finance.md) |
| | | **UC-70** | Process Pending Payment Verifications | Accountant | None | [`FE-14_Accounting_And_Finance.md`](./FE-14_Accounting_And_Finance.md) |

*(Note: Per Catalog requirements, UC-25, UC-35, UC-52, and UC-57 are intentionally omitted/reserved).*
