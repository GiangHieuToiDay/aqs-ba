# MODULE FE-02: SYSTEM AUDIT LOG

---

### UC-07 Search and View Activity Logs

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
