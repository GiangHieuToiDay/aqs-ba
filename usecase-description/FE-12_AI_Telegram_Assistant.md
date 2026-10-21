# MODULE FE-12: AI TELEGRAM ASSISTANT

---

### UC-58 Query Business Data via Telegram

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
