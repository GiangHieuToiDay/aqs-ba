# PHÂN HỆ ACC: KẾ TOÁN THU - CHI, CÔNG NỢ & BÁO CÁO TÀI CHÍNH (ACCOUNTING & FINANCIALS)

---

### UC-202 Xem danh sách giao dịch (View Transactions)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to view the consolidated chronological ledger of cash and bank transactions (receipts and payment vouchers) so that daily financial flows are easily audited.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý tài chính.  

**Normal Flow**  
*Xem danh sách các giao dịch thu - chi trong sổ quỹ*  
1. Người dùng chọn mục "Sổ quỹ & Nhật ký giao dịch" (Transactions Ledger).  
2. Hệ thống tải và hiển thị bảng dữ liệu các giao dịch thu - chi:  
   - Bảng phân loại: Tab `Tất cả giao dịch`, Tab `Phiếu thu (Inflows)`, Tab `Phiếu chi (Outflows)`.  
   - Cột dữ liệu: Mã phiếu (PT-xxxx cho phiếu thu, PC-xxxx cho phiếu chi), Thời gian phát sinh, Loại giao dịch (Bán hàng, Thu nợ, Nhập hàng, Chi phí vận hành, Hoàn tiền), Đối tượng nộp/nhận tiền (Tên KH / NCC / Nhân viên), Phương thức (Tiền mặt / Chuyển khoản), Số tiền (Thu in màu xanh lá `+`, Chi in màu đỏ `-`), Số dư quỹ tức thời sau giao dịch (`Running Balance`), Trạng thái (Đã xác nhận / Đã hủy), Thao tác ("Xem chi tiết", "In phiếu").  
   - Thống kê tóm tắt trên đầu: Tổng thu trong kỳ, Tổng chi trong kỳ, Chênh lệch dòng tiền thuần.  
3. Người dùng duyệt danh sách và kiểm tra số dư quỹ.  

**Alternative Flows**  
- **Danh sách trống:**  
  1. Kỳ chưa phát sinh giao dịch. Hệ thống hiển thị số liệu 0 VNĐ.  

**Postconditions**  
Sổ quỹ thu - chi tổng thể được hiển thị đầy đủ và minh bạch.  

---

### UC-203 Tìm kiếm giao dịch (Search Transactions)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to search and filter transactions by code, customer/supplier, transaction category, payment method, and amount range so that specific payment vouchers are quickly located.  

**Preconditions**  
Người dùng đang ở trên màn hình Danh sách giao dịch (UC-202).  

**Normal Flow**  
*Tìm kiếm và lọc phiếu thu chi theo tiêu chí chi tiết*  
1. Người dùng sử dụng các bộ lọc:  
   - Ô tìm kiếm: Nhập Mã phiếu (PT-..., PC-...), Tên khách hàng, Tên NCC, hoặc Tên nhân viên.  
   - Bộ lọc Loại giao dịch: `Tất cả`, `Thu tiền bán hàng`, `Thu nợ khách`, `Chi trả tiền NCC`, `Chi vận hành`, `Chi lương`, `Chi hoàn tiền`.  
   - Bộ lọc Phương thức: `Tiền mặt`, `Ngân hàng / Chuyển khoản VietQR`.  
   - Bộ lọc Khoảng tiền: Từ [X] triệu đến [Y] triệu đồng.  
   - Bộ lọc Khoảng thời gian: Chọn Từ ngày - Đến ngày.  
2. Người dùng nhấp nút "Tìm kiếm".  
3. Hệ thống trả về bảng danh sách các giao dịch thỏa mãn toàn bộ tiêu chí lọc.  
4. Người dùng kiểm tra các bản ghi tìm thấy.  

**Alternative Flows**  
- **Không tìm thấy kết quả:**  
  1. Bộ lọc không có dữ liệu. Hệ thống báo không tìm thấy bản ghi phù hợp.  

**Postconditions**  
Giao dịch cần tìm kiếm được lọc chính xác.  

---

### UC-204 Xem chi tiết giao dịch (View Transaction Detail)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to view full details of a payment voucher or receipt (originating order, payment allocation, attached invoices, audit trail) so that the validity of money movements is verified.  

**Preconditions**  
Người dùng nhấp vào một giao dịch từ danh sách (UC-202).  

**Normal Flow**  
*Xem hồ sơ chi tiết của một phiếu thu hoặc phiếu chi*  
1. Hệ thống hiển thị trang "Chi tiết giao dịch - [Mã phiếu]":  
   - Khối 1: Thông tin chứng từ: Loại phiếu (Thu / Chi), Mã phiếu, Ngày lập, Người lập phiếu, Trạng thái chứng từ.  
   - Khối 2: Số tiền và Tài khoản quỹ: Số tiền chính xác bằng số và bằng chữ (ví dụ: `Mười hai triệu năm trăm nghìn đồng chẵn`), Phương thức (Tiền mặt / Tài khoản ngân hàng số...).  
   - Khối 3: Đối tượng giao dịch: Tên khách hàng / Nhà cung cấp, Địa chỉ, Mã số thuế / SĐT.  
   - Khối 4: Căn cứ giao dịch & Phân bổ: Mã đơn hàng liên kết (ORD-xxx) hoặc Mã phiếu nhập liên kết (PN-xxx) kèm bảng phân bổ tiền (UC-208).  
   - Khối 5: Chứng từ gốc đính kèm: Ảnh chụp hóa đơn VAT, UNC ngân hàng, phiếu chi tay (UC-212).  
   - Khối 6: Lịch sử thay đổi (Audit trail: ai tạo, ai duyệt, ai điều chỉnh).  
2. Người dùng kiểm tra thông tin hoặc nhấp nút "In phiếu thu/chi".  

**Alternative Flows**  
- **Giao dịch đã bị hủy:**  
  1. Phiếu đã bị hủy trước đó.  
  2. Hệ thống hiển thị con dấu đỏ "ĐÃ HỦY CHỨNG TỪ" kèm người hủy và lý do hủy.  

**Postconditions**  
Mọi căn cứ kế toán của giao dịch được cung cấp đầy đủ, chuẩn mực.  

---

### UC-205 Tạo phiếu thu (Create Receipt)
**Primary Actors**  
Accountant, Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As an internal staff member, I want to create a formal Cash/Bank Receipt (PT) recording inbound funds (customer advance, counter cash, debt payment, scrap sale) so that company funds are legally documented.  

**Preconditions**  
Cửa hàng nhận được một khoản tiền vào tài khoản ngân hàng hoặc tiền mặt.  

**Normal Flow**  
*Lập phiếu thu tiền vào quỹ cửa hàng*  
1. Người dùng nhấp nút "Lập phiếu thu" (Create Receipt).  
2. Hệ thống hiển thị biểu mẫu tạo phiếu thu:  
   - Mã phiếu thu: Tự động sinh `PT-YYYYMMDD-XXXXX`.  
   - Ngày lập phiếu: Mặc định là ngày giờ hiện tại.  
   - Người nộp tiền: Chọn Khách hàng (UC-206) hoặc nhập người nộp vãng lai.  
   - Lý do nộp tiền: Dropdown chọn (`Thu tiền bán hàng`, `Thu hồi công nợ khách hàng`, `Thu tiền cọc công trình`, `Thu hồi tiền tạm ứng`, `Thu nhập khác`).  
   - Số tiền thu (VNĐ): Nhập số tiền > 0.  
   - Bằng chữ: Hệ thống tự động dịch số tiền thành chữ tiếng Việt chuẩn.  
   - Phương thức thu: `Tiền mặt vào quỹ` hoặc `Chuyển khoản vào tài khoản ngân hàng`.  
   - Chứng từ gốc kèm theo: Mã đơn hàng tham chiếu, Hợp đồng kinh tế.  
3. Người dùng nhập đầy đủ thông tin và chọn phân bổ khoản thu vào đơn hàng tương ứng (UC-208).  
4. Người dùng nhấp nút "Lưu và Xác nhận phiếu thu".  
5. Hệ thống kiểm tra: Số tiền > 0, Người nộp và Lý do không để trống.  
6. Hệ thống lưu phiếu thu vào cơ sở dữ liệu ở trạng thái `Hoàn tất (Confirmed)`.  
7. Hệ thống tự động cộng tăng số dư tiền mặt hoặc tài khoản ngân hàng tương ứng.  
8. Hệ thống tự động giảm trừ công nợ phải thu của khách hàng (nếu là thu nợ).  
9. Hệ thống in phiếu thu khổ A5 (2 liên: 1 liên giao người nộp, 1 liên lưu cuống sổ quỹ).  
10. Hệ thống hiển thị thông báo thành công (MSG220: "Lập phiếu thu thành công! Số dư quỹ đã được cập nhật").  

**Alternative Flows**  
- **Số tiền thu <= 0:**  
  1. Nhập số tiền không hợp lệ.  
  2. Hệ thống báo lỗi (MSG131).  

**Postconditions**  
1. Phiếu thu được ghi nhận vĩnh viễn trong sổ quỹ kế toán.  
2. Số dư tài sản tiền của cửa hàng tăng lên chính xác.  

---

### UC-206 Chọn khách hàng/đơn hàng khi thu tiền (Select Customer and Order)
**Primary Actors**  
Accountant, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Sales Staff, I want to search and attach a customer profile and their open orders when creating a receipt so that payment is tied to legal obligations.  

**Preconditions**  
Người dùng đang ở biểu mẫu lập phiếu thu (UC-205).  

**Normal Flow**  
*Tra cứu và liên kết khách hàng cùng đơn hàng vào phiếu thu*  
1. Tại trường "Người nộp tiền", người dùng gõ SĐT hoặc Tên khách hàng.  
2. Hệ thống hiển thị danh sách gợi ý thông minh (Auto-complete dropdown).  
3. Người dùng nhấp chọn khách hàng mong muốn.  
4. Hệ thống tự động hiển thị danh sách các đơn hàng còn dư nợ của khách hàng này ngay bên dưới biểu mẫu:  
   - Mã đơn hàng, Ngày đặt, Tổng tiền đơn, Đã thanh toán, Còn phải thu.  
5. Người dùng tích chọn đơn hàng mà khoản tiền này dùng để thanh toán.  
6. Hệ thống tự động liên kết đơn hàng với phiếu thu.  

**Alternative Flows**  
- **Thu tiền từ đối tượng vãng lai (không gắn đơn hàng):**  
  1. Khoản thu từ bán thanh lý phế liệu hoặc bồi thường.  
  2. Người dùng chọn "Khách vãng lai" và nhập trực tiếp tên người nộp mà không cần gắn đơn hàng.  

**Postconditions**  
Căn cứ nghĩa vụ thanh toán được liên kết chuẩn xác vào phiếu thu.  

---

### UC-207 Ghi nhận phương thức thanh toán (Record Payment Method)
**Primary Actors**  
Accountant, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Sales Staff, I want to specify whether funds were received via physical Cash, VietQR Bank Transfer, or POS Card Swipe and specify the exact receiving bank account so that bank reconciliations balance.  

**Preconditions**  
Người dùng đang tạo phiếu thu hoặc phiếu chi.  

**Normal Flow**  
*Xác định kênh dòng tiền thanh toán và tài khoản thụ hưởng*  
1. Tại mục "Phương thức thanh toán", người dùng chọn một trong các hình thức:  
   - `Tiền mặt (Cash)`: Chọn quỹ tiền mặt tại quầy showroom.  
   - `Chuyển khoản ngân hàng (Bank Transfer)`: Chọn tài khoản ngân hàng thụ hưởng (ví dụ: `Vietcombank - STK 001100...`).  
   - `Thẻ ngân hàng (POS Machine)`: Chọn máy POS thanh toán.  
2. Nếu là chuyển khoản: Người dùng nhập mã tham chiếu giao dịch ngân hàng (Mã GD / FT number).  
3. Hệ thống lưu phương thức và phân luồng hạch toán vào đúng Sổ chi tiết tiền mặt hoặc Sổ tiền gửi ngân hàng.  

**Alternative Flows**  
- **Không chọn phương thức:**  
  1. Bỏ trống phương thức. Hệ thống cảnh báo bắt buộc chọn kênh thanh toán.  

**Postconditions**  
Kênh thanh toán được phân bổ chính xác vào tài khoản tài chính tương ứng.  

---

### UC-208 Phân bổ khoản thu (Allocate Receipt)
**Primary Actors**  
Accountant  

**Secondary Actors**  
None  

**Description**  
As an Accountant, I want to allocate a lump-sum customer payment across multiple open orders/invoices (by oldest first or manual allocation) so that order balances are reduced correctly.  

**Preconditions**  
Khách hàng thanh toán một khoản tiền lớn (ví dụ: 100 triệu) để trả nợ cho nhiều đơn hàng công trình khác nhau.  

**Normal Flow**  
*Phân bổ số tiền thu vào nhiều đơn hàng mở của khách*  
1. Sau khi nhập số tiền thu là `100.000.000 VNĐ`, hệ thống hiển thị bảng danh sách 3 đơn hàng đang nợ của khách:  
   - Đơn 1: Nợ `40.000.000 VNĐ`.  
   - Đơn 2: Nợ `50.000.000 VNĐ`.  
   - Đơn 3: Nợ `30.000.000 VNĐ`.  
2. Kế toán chọn chế độ phân bổ:  
   - Lựa chọn A: `Tự động phân bổ theo đơn cũ nhất (FIFO)` -> Hệ thống tự động gạch nợ 40tr cho Đơn 1, 50tr cho Đơn 2, và 10tr còn lại cho Đơn 3.  
   - Lựa chọn B: `Phân bổ thủ công` -> Kế toán tự gõ số tiền muốn gạch vào từng dòng đơn hàng.  
3. Kế toán chọn phương án mong muốn.  
4. Hệ thống kiểm tra: Tổng số tiền phân bổ = Đúng số tiền thu của phiếu thu (100.000.000 VNĐ).  
5. Kế toán nhấp "Xác nhận phân bổ".  
6. Hệ thống lưu cấu trúc phân bổ liên kết 1-n giữa Phiếu thu và các Đơn hàng.  
7. Trạng thái thanh toán của từng đơn hàng được cập nhật tương ứng.  

**Alternative Flows**  
- **Tổng tiền phân bổ không khớp với số tiền phiếu thu:**  
  1. Phân bổ thủ công bị thừa hoặc thiếu tiền.  
  2. Hệ thống báo lỗi (MSG221: "Tổng số tiền phân bổ cho các đơn hàng phải bằng chính xác số tiền trên phiếu thu").  

**Postconditions**  
Số tiền thanh toán được phân bổ minh bạch, đúng từng đơn hàng.  

---

### UC-209 Gửi xác nhận thanh toán (Send Payment Confirmation)
**Primary Actors**  
Accountant, Sales Staff  

**Secondary Actors**  
SMS Service, Email Service  

**Description**  
As an Accountant or Sales Staff, I want to send an instant digital payment receipt (SMS or Email) to the customer upon payment confirmation so that the customer has instant proof of payment.  

**Preconditions**  
Phiếu thu đã được xác nhận hoàn tất thành công (UC-205).  

**Normal Flow**  
*Gửi biên lai xác nhận thu tiền điện tử cho khách hàng*  
1. Sau khi lưu phiếu thu, hệ thống hiển thị tùy chọn: "Gửi biên nhận điện tử cho khách".  
2. Người dùng nhấp nút "Gửi xác nhận qua SMS/Email".  
3. Hệ thống tạo nội dung thông báo chuẩn:  
   "Cửa hàng AQS xác nhận đã nhận thanh toán số tiền [X VNĐ] cho đơn hàng [Mã đơn] qua hình thức [Tiền mặt/Chuyển khoản]. Dư nợ còn lại: [Y VNĐ]. Xem biên lai: [Link_E_Receipt]. Xin cảm ơn quý khách!".  
4. Hệ thống gọi Secondary Actor `SMS Service` / `Email Service` gửi thông điệp tới số điện thoại/email của khách.  
5. Hệ thống hiển thị thông báo thành công (MSG222: "Đã gửi biên nhận thanh toán cho khách hàng thành công!").  

**Alternative Flows**  
- **Khách không đăng ký email hoặc SĐT:**  
  1. Thiếu thông tin liên hệ. Hệ thống chỉ in biên nhận giấy trực tiếp.  

**Postconditions**  
Khách hàng nhận được bằng chứng giao dịch điện tử minh bạch.  

---

### UC-210 Tạo phiếu chi (Create Payment Voucher)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to create a formal Cash/Bank Payment Voucher (PC) for disbursements (supplier payments, rent, utility bills, courier fees, customer refunds) so that money outflows are authorized.  

**Preconditions**  
Người dùng đã đăng nhập với vai trò `Accountant` hoặc `Store Owner`.  

**Normal Flow**  
*Lập phiếu chi tiền thanh toán các hoạt động kinh doanh*  
1. Người dùng nhấp nút "Lập phiếu chi" (Create Payment Voucher).  
2. Hệ thống hiển thị biểu mẫu lập phiếu chi:  
   - Mã phiếu chi: Tự động sinh `PC-YYYYMMDD-XXXXX`.  
   - Người nhận tiền: Chọn Nhà cung cấp (nếu trả nợ hàng - UC-230), Nhân viên (nếu chi lương/tạm ứng), hoặc Đơn vị dịch vụ.  
   - Phân loại khoản chi (UC-211): Chọn nhóm chi phí.  
   - Số tiền chi (VNĐ): Nhập số tiền > 0.  
   - Bằng chữ: Hệ thống tự động dịch số tiền thành chữ.  
   - Phương thức chi: `Tiền mặt từ két quầy` hoặc `Chuyển khoản từ tài khoản ngân hàng`.  
   - Nội dung chi tiết: Ghi rõ mục đích chi.  
   - Đính kèm hóa đơn chứng từ gốc (UC-212).  
3. Người dùng nhập đầy đủ thông tin và đính kèm chứng từ.  
4. Người dùng nhấp nút "Lưu và Phê duyệt phiếu chi".  
5. Hệ thống kiểm tra: Số tiền chi <= Số dư hiện có trong quỹ tiền mặt hoặc tài khoản ngân hàng tương ứng (ngăn chặn xuất âm quỹ).  
6. Hệ thống lưu phiếu chi vào cơ sở dữ liệu ở trạng thái `Hoàn tất (Confirmed)`.  
7. Hệ thống tự động trừ giảm số dư quỹ tương ứng.  
8. Hệ thống in phiếu chi khổ A5 cho người nhận tiền ký tên.  
9. Hệ thống ghi nhận sự kiện vào Audit Log.  
10. Hệ thống hiển thị thông báo thành công (MSG223: "Lập phiếu chi thành công! Số dư quỹ đã được giảm trừ").  

**Alternative Flows**  
- **Số dư quỹ không đủ để chi (Âm quỹ):**  
  1. Tiền mặt trong két nhỏ hơn số tiền muốn chi.  
  2. Hệ thống chặn lưu và báo lỗi (MSG224: "Số dư quỹ tiền mặt hiện tại không đủ để thực hiện khoản chi này. Vui lòng kiểm tra lại quỹ hoặc chọn kênh thanh toán khác").  

**Postconditions**  
1. Phiếu chi được ghi sổ chính thức.  
2. Số dư quỹ tiền của cửa hàng giảm đi tương ứng.  

---

### UC-211 Phân loại khoản chi (Categorize Expense)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to categorize payments into standardized chart-of-accounts expense buckets (Goods Purchases, Freight, Rent, Utilities, Petty Cash, Marketing, Refunds) for accurate cost reporting.  

**Preconditions**  
Người dùng đang ở biểu mẫu tạo phiếu chi (UC-210).  

**Normal Flow**  
*Gán mã phân loại nhóm chi phí cho chứng từ chi*  
1. Tại trường "Nhóm chi phí", người dùng chọn một trong các danh mục chi chuẩn:  
   - `6411 - Chi phí vận chuyển & Giao nhận hàng hóa`.  
   - `6412 - Chi phí hoa hồng & Bán hàng showroom`.  
   - `6421 - Chi phí tiền thuê mặt bằng & Kho bãi`.  
   - `6422 - Chi phí điện, nước, internet, vệ sinh`.  
   - `6423 - Chi phí văn phòng phẩm & Công cụ dụng cụ`.  
   - `331 - Thanh toán tiền hàng cho Nhà cung cấp`.  
   - `521 - Chi tiền hoàn trả lại cho khách hàng`.  
2. Hệ thống tự động gán mã tài khoản chi phí tương ứng để đưa vào Báo cáo Lãi/Lỗ (UC-116).  

**Alternative Flows**  
- **Thêm nhóm chi phí mới:**  
  1. Nếu phát sinh nhóm chi phí mới chưa có trong danh mục.  
  2. Store Owner có quyền bấm "Thêm nhóm chi phí" để tạo thêm danh mục mới.  

**Postconditions**  
Khoản chi được phân loại chuẩn mực theo nguyên tắc kế toán tài chính.  

---

### UC-212 Đính kèm chứng từ chi (Attach Expense Document)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As an Accountant or Store Owner, I want to upload image or PDF proofs (VAT tax invoices, delivery slips, bank transfer slips, hand receipts) linked to a payment voucher for legal audit compliance.  

**Preconditions**  
Người dùng đang ở biểu mẫu phiếu chi hoặc chi tiết phiếu chi.  

**Normal Flow**  
*Tải lên và lưu trữ chứng từ gốc đính kèm phiếu chi*  
1. Tại mục "Chứng từ gốc kèm theo", người dùng nhấp nút "Tải tệp đính kèm" (Upload Attachment) hoặc kéo thả tệp ảnh/PDF vào khung.  
2. Người dùng chọn tệp ảnh hóa đơn VAT hoặc ảnh ủy nhiệm chi ngân hàng từ máy tính/điện thoại.  
3. Hệ thống kiểm tra: Tệp hợp lệ (JPG, PNG, PDF) và dung lượng < 10MB.  
4. Hệ thống gọi Secondary Actor `Cloudinary` tải tệp lên lưu trữ bảo mật.  
5. Hệ thống lưu đường dẫn URL tệp vào danh sách chứng từ đính kèm của phiếu chi.  
6. Hệ thống hiển thị hình ảnh thu nhỏ của chứng từ kèm nút phóng to xem lại hoặc nút xóa.  

**Alternative Flows**  
- **Tệp sai định dạng:**  
  1. Người dùng chọn file thực thi (.exe, .zip).  
  2. Hệ thống từ chối tệp và báo lỗi định dạng (MSG18).  

**Postconditions**  
Chứng từ kế toán gốc được số hóa và liên kết vĩnh viễn với phiếu chi.  

---

### UC-213 Scan hóa đơn/chứng từ (Scan Expense Invoice)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
OpenAI, Cloudinary  

**Description**  
As an Accountant or Store Owner, I want to scan physical invoices using AI Optical Character Recognition (OCR) to automatically extract seller name, tax code, invoice number, items, and total amount into voucher fields without manual typing.  

**Preconditions**  
Người dùng mở màn hình lập phiếu chi trên điện thoại hoặc máy tính có webcam/tệp ảnh hóa đơn giấy.  

**Normal Flow**  
*Quét hóa đơn tự động trích xuất thông tin bằng AI OCR*  
1. Người dùng nhấp nút "Quét hóa đơn bằng AI" (Scan Invoice with AI) trên biểu mẫu phiếu chi.  
2. Người dùng chụp ảnh trực tiếp hóa đơn giấy hoặc tải ảnh chụp hóa đơn lên.  
3. Hệ thống gọi Secondary Actor `Cloudinary` lưu tạm ảnh và chuyển hình ảnh tới Secondary Actor `OpenAI` (Vision Model) để nhận diện ký tự quang học chuyên sâu.  
4. Secondary Actor `OpenAI` phân tích cấu trúc hóa đơn và trích xuất dữ liệu JSON có cấu trúc:  
   - Tên đơn vị bán hàng / NCC.  
   - Mã số thuế đơn vị bán.  
   - Số hóa đơn & Ngày hóa đơn.  
   - Tổng tiền thanh toán (bao gồm VAT).  
   - Nội dung tóm tắt hàng hóa/dịch vụ mua vào.  
5. Hệ thống tự động điền các trường thông tin vừa trích xuất vào biểu mẫu Tạo phiếu chi.  
6. Hệ thống hiển thị thông báo (MSG225: "Đã trích xuất dữ liệu hóa đơn thành công! Vui lòng kiểm tra lại các trường trước khi lưu").  
7. Người dùng kiểm tra đối chiếu dữ liệu nhận diện với ảnh gốc, chỉnh sửa nếu cần và bấm Lưu phiếu chi.  

**Alternative Flows**  
- **Ảnh chụp quá mờ không nhận diện được:**  
  1. Hóa đơn bị rách, nhăn hoặc ảnh chụp thiếu sáng.  
  2. Hệ thống thông báo cảnh báo: "Không thể nhận diện rõ các trường thông tin do ảnh chụp mờ. Vui lòng chụp lại rõ nét hơn hoặc nhập thủ công".  

**Postconditions**  
Thông tin hóa đơn được số hóa nhanh chóng, tiết kiệm 90% thời gian nhập liệu kế toán.  

---

### UC-214 Điều chỉnh giao dịch (Adjust Transaction)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to adjust an erroneous transaction with mandatory diff auditing and justification so that petty mistakes are corrected transparently.  

**Preconditions**  
1. Người dùng có thẩm quyền kế toán.  
2. Giao dịch cần sửa thuộc kỳ kế toán hiện tại (chưa khóa sổ).  

**Normal Flow**  
*Điều chỉnh thông tin phiếu thu hoặc phiếu chi bị sai sót*  
1. Người dùng mở chi tiết giao dịch cần điều chỉnh và nhấp nút "Điều chỉnh phiếu" (Adjust Transaction).  
2. Hệ thống mở biểu mẫu cho phép sửa: Số tiền, Đối tượng nộp/nhận, Phân loại chi phí, Nội dung.  
3. Người dùng cập nhật các trường sai sót.  
4. Người dùng nhấp nút "Lưu điều chỉnh".  
5. Hệ thống mở popup bắt buộc: Nhập "Lý do điều chỉnh giao dịch" (tối thiểu 10 ký tự, ví dụ: "Sửa lại đối tượng nhận tiền từ nhân viên A sang nhà xe B").  
6. Người dùng nhập lý do và bấm "Xác nhận lưu".  
7. Hệ thống tính toán chênh lệch số tiền và điều chỉnh lại số dư quỹ tương ứng.  
8. Hệ thống ghi nhận chi tiết giá trị cũ, giá trị mới, người sửa và lý do vào Audit Log (UC-216).  
9. Hệ thống hiển thị thông báo thành công (MSG226: "Điều chỉnh giao dịch thành công! Dữ liệu kiểm toán đã được ghi nhận").  

**Alternative Flows**  
- **Không nhập lý do điều chỉnh:**  
  1. Bỏ trống lý do. Hệ thống chặn lưu (MSG40).  

**Postconditions**  
Giao dịch được sửa đổi an toàn, số dư quỹ được cập nhật chuẩn xác.  

---

### UC-215 Hủy giao dịch (Cancel Transaction)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to void an invalid transaction without deleting it from the database so that an immutable audit trail is preserved and ledger balances are reversed.  

**Preconditions**  
Phiếu thu hoặc phiếu chi bị lập sai hoàn toàn hoặc trùng lặp và cần hủy bỏ.  

**Normal Flow**  
*Hủy bỏ phiếu thu hoặc phiếu chi và đảo ngược dòng tiền*  
1. Người dùng mở chi tiết giao dịch và nhấp nút "Hủy chứng từ" (Void Transaction).  
2. Hệ thống hiển thị hộp thoại cảnh báo:  
   - Cảnh báo: "Khi hủy phiếu [Mã phiếu], số dư quỹ sẽ bị ĐẢO NGƯỢC tương ứng. Bản ghi sẽ được giữ lại trong hệ thống với trạng thái ĐÃ HỦY và không thể xóa bỏ".  
   - Bắt buộc nhập "Lý do hủy chứng từ" (tối thiểu 10 ký tự).  
3. Người dùng nhập lý do (ví dụ: "Lập trùng phiếu thu tiền khách hàng hai lần") và nhấp "Xác nhận hủy".  
4. Hệ thống chuyển trạng thái giao dịch sang `Đã hủy (Cancelled)`.  
5. Hệ thống tự động đảo ngược dòng tiền:  
   - Nếu hủy Phiếu thu: Trừ ngược lại số dư quỹ và hoàn nguyên công nợ của khách.  
   - Nếu hủy Phiếu chi: Cộng ngược lại số tiền vào số dư quỹ.  
6. Hệ thống ghi nhận sự kiện vào Audit Log.  
7. Hệ thống hiển thị thông báo thành công (MSG227: "Đã hủy chứng từ thành công! Dòng tiền đã được đảo ngược an toàn").  

**Alternative Flows**  
- **Không nhập lý do hủy:**  
  1. Bỏ trống lý do. Hệ thống chặn thao tác (MSG40).  

**Postconditions**  
1. Giao dịch được đánh dấu Đã hủy nhưng không bị xóa khỏi cơ sở dữ liệu.  
2. Số dư sổ quỹ được hoàn nguyên chính xác.  

---

### UC-216 Xem lịch sử thay đổi giao dịch (View Transaction Audit History)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to view the chronological modification history of a financial transaction (creator, editor, timestamps, field-level before/after diffs, reasons) to detect tampering.  

**Preconditions**  
Người dùng đang ở trên màn hình Chi tiết giao dịch (UC-204).  

**Normal Flow**  
*Xem nhật ký thay đổi và kiểm toán chứng từ tài chính*  
1. Người dùng nhấp chọn tab "Lịch sử kiểm toán" (Audit Trail) của giao dịch.  
2. Hệ thống truy vấn bảng Audit Log và hiển thị dòng thời gian của giao dịch:  
   - Sự kiện 1: Tạo lập ban đầu (Ngày giờ, Người tạo, Giá trị gốc).  
   - Sự kiện 2: Điều chỉnh thông tin (Ngày giờ, Người sửa, Trường thay đổi: `Số tiền: 5.000.000 -> 5.500.000`, Lý do: "Bổ sung thuế VAT").  
   - Sự kiện 3: Duyệt / Hủy chứng từ (nếu có).  
3. Người dùng kiểm tra đối chiếu tính minh bạch của chứng từ.  

**Alternative Flows**  
- **Giao dịch chưa từng bị sửa đổi:**  
  1. Phiếu giữ nguyên từ lúc tạo.  
  2. Lịch sử hiển thị: "Chứng từ nguyên bản, chưa qua điều chỉnh".  

**Postconditions**  
Tính toàn vẹn và trách nhiệm cá nhân đối với số liệu kế toán được bảo đảm 100%.  

---

### UC-217 Xem danh sách khoản phải thu (View Accounts Receivable)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to view all open accounts receivable arising from orders (total order amount, paid amount, remaining balance, due dates) so that unpaid sales are monitored.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý tài chính.  

**Normal Flow**  
*Xem danh mục các khoản nợ phải thu của khách hàng*  
1. Người dùng chọn mục "Khoản phải thu khách hàng" (Accounts Receivable - AR).  
2. Hệ thống hiển thị bảng các khoản phải thu:  
   - Cột: Mã đơn hàng, Khách hàng & SĐT, Ngày bán, Tổng giá trị đơn hàng, Số tiền đã thu, Số tiền còn phải thu (`Remaining Receivable`), Hạn thanh toán (Due Date), Trạng thái nợ (Chưa thu / Đã thu một phần / Quá hạn), Thao tác ("Thu tiền", "Xem chi tiết").  
   - Thống kê tóm tắt trên đầu: Tổng số tiền phải thu toàn cửa hàng; Tổng số tiền trong hạn; Tổng số tiền quá hạn.  
3. Người dùng theo dõi và lên kế hoạch thu tiền.  

**Alternative Flows**  
- **Không có khoản nợ nào:**  
  1. Tất cả đơn đã trả đủ tiền. Bảng hiển thị thông báo không có nợ tồn đọng.  

**Postconditions**  
Các khoản phải thu được liệt kê chi tiết, đầy đủ.  

---

### UC-218 Tìm kiếm khoản phải thu (Search Accounts Receivable)
**Primary Actors**  
Accountant, Store Owner  

*(Chi tiết xem tại UC-217 - Tìm kiếm và lọc khoản phải thu theo khách hàng, đơn hàng, trạng thái thanh toán và ngày đến hạn).*  

---

### UC-219 Xem chi tiết khoản phải thu (View Receivable Detail)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to view the detailed payment history of a specific receivable item (order value, previous installments, remaining dues, payment receipts, due dates) so that debt status is clear.  

**Preconditions**  
Người dùng mở một khoản phải thu cụ thể từ danh sách (UC-217).  

**Normal Flow**  
*Xem chi tiết lịch sử thanh toán của một khoản phải thu*  
1. Hệ thống hiển thị trang "Chi tiết khoản phải thu - [Mã đơn]":  
   - Tổng giá trị đơn hàng: `60.000.000 VNĐ`.  
   - Bảng kê các đợt thanh toán đã nhận:  
     + Đợt 1 (Ngày 01/10): Thu cọc `20.000.000 VNĐ` (Phiếu thu PT-0012, Tiền mặt).  
     + Đợt 2 (Ngày 05/10): Thu đợt 2 `20.000.000 VNĐ` (Phiếu thu PT-0034, Chuyển khoản VietQR).  
   - Số tiền còn lại phải thu: `20.000.000 VNĐ`.  
   - Ngày hẹn thanh toán đợt cuối: `15/10/2026`.  
   - Danh sách thiết bị trong đơn hàng tương ứng.  
2. Người dùng kiểm tra thông tin để đối chiếu với khách hàng.  

**Alternative Flows**  
- **Khoản phải thu đã tất toán đủ:**  
  1. Khách đã trả xong 100%. Khoản mục hiển thị trạng thái "Đã tất toán đủ" và chuyển sang danh mục lưu trữ.  

**Postconditions**  
Lịch sử từng đồng thanh toán của khoản phải thu được cung cấp rõ ràng.  

---

### UC-220 Theo dõi trạng thái thanh toán (Track Payment Status)
**Primary Actors**  
Accountant, Sales Staff, Store Owner  

*(Chi tiết xem tại UC-046 và UC-217 - Giám sát trạng thái Chưa thanh toán, Thanh toán một phần hoặc Đã thanh toán đủ của từng đơn hàng).*  

---

### UC-221 Theo dõi ngày đến hạn (Track Due Date)
**Primary Actors**  
Accountant, Store Owner  

*(Chi tiết xem tại UC-059 và UC-217 - Theo dõi đồng hồ đếm ngược và hạn chót thanh toán đối với khách hàng mua công nợ).*  

---

### UC-222 Xem chi tiết công nợ khách hàng (View Customer Debt Detail)
**Primary Actors**  
Accountant, Store Owner  

*(Chi tiết xem tại UC-110 (Tab 4) - Xem toàn bộ lịch sử phát sinh nợ, thanh toán, điều chỉnh và số dư nợ hiện tại của một khách hàng).*  

---

### UC-223 Xem công nợ quá hạn (View Overdue Debts)
**Primary Actors**  
Accountant, Store Owner  

*(Chi tiết xem tại UC-058 và UC-121 - Bảng lọc danh sách các khoản nợ đã vượt quá ngày thanh toán thỏa thuận để ưu tiên xử lý).*  

---

### UC-224 Lập đối chiếu công nợ (Create Debt Reconciliation)
**Primary Actors**  
Accountant  

**Secondary Actors**  
None  

**Description**  
As an Accountant, I want to create a Debt Reconciliation Statement summarizing opening balance, new purchases, payments made, adjustments, and closing balance for a customer so that accounts can be formally settled.  

**Preconditions**  
Khách hàng công trình hoặc thợ thi công yêu cầu đối soát số liệu công nợ định kỳ (cuối tháng hoặc cuối công trình).  

**Normal Flow**  
*Lập bảng đối chiếu công nợ chi tiết với khách hàng*  
1. Kế toán chọn mục "Lập đối chiếu công nợ" (Create Reconciliation).  
2. Kế toán chọn Khách hàng mục tiêu và chọn Kỳ đối chiếu (ví dụ: Từ ngày `01/09/2026` đến ngày `30/09/2026`).  
3. Kế toán nhấp nút "Tổng hợp số liệu đối chiếu".  
4. Hệ thống tự động tính toán và trích xuất bảng số liệu:  
   - **(A) Số dư nợ đầu kỳ**: Dư nợ tính đến trước ngày 01/09.  
   - **(B) Số phát sinh tăng (Mua hàng trong kỳ)**: Danh sách các đơn hàng xuất trong tháng 9 kèm mã đơn, ngày mua, tổng tiền hàng.  
   - **(C) Số phát sinh giảm (Đã thanh toán / Trả hàng trong kỳ)**: Danh sách các phiếu thu tiền và giá trị hàng đổi trả trong tháng 9.  
   - **(D) Số dư nợ cuối kỳ**: `D = A + B - C` (Số tiền khách hàng thực tế còn nợ đến ngày 30/09).  
5. Kế toán kiểm tra lại các số liệu và nhấp "Lưu biên bản đối chiếu".  
6. Hệ thống lưu bản đối chiếu với mã `DC-KH-YYYYMM-XXXXX` ở trạng thái `Chờ khách ký xác nhận`.  
7. Hệ thống hiển thị thông báo thành công (MSG228: "Lập bảng đối chiếu công nợ thành công!") kèm nút "Xuất bảng đối chiếu" (UC-225).  

**Alternative Flows**  
- **Khách không có phát sinh giao dịch trong kỳ:**  
  1. Trong tháng khách không mua thêm và không trả tiền.  
  2. Bảng đối chiếu hiển thị phát sinh trong kỳ bằng 0, số dư cuối kỳ bằng số dư đầu kỳ.  

**Postconditions**  
Biên bản đối chiếu công nợ chính thức được xác lập, sẵn sàng để gửi khách hàng.  

---

### UC-225 Xuất bảng đối chiếu công nợ (Export Debt Reconciliation)
**Primary Actors**  
Accountant  

**Secondary Actors**  
None  

**Description**  
As an Accountant, I want to export the debt reconciliation statement to PDF or Excel with company signature blocks so that the customer can sign and confirm their balance.  

**Preconditions**  
Biên bản đối chiếu công nợ đã được lập (UC-224).  

**Normal Flow**  
*Xuất biên bản đối chiếu công nợ ra file PDF/Excel gửi khách ký*  
1. Kế toán nhấp nút "Xuất biên bản đối chiếu công nợ" (Export Statement).  
2. Kế toán chọn định dạng: `File PDF có mẫu ký duyệt` hoặc `File Excel (.xlsx)`.  
3. Hệ thống tạo tài liệu với tiêu đề chuẩn:  
   - BIÊN BẢN ĐỐI CHIẾU VÀ XÁC NHẬN CÔNG NỢ.  
   - Thông tin bên Bán (Cửa hàng AQS) và bên Mua (Khách hàng/Nhà thầu).  
   - Bảng kê chi tiết các chứng từ mua bán và thanh toán trong kỳ.  
   - Kết luận số dư nợ cuối kỳ bằng số và bằng chữ.  
   - Phần ký tên: Đại diện bên Bán (Kế toán & Giám đốc ký, đóng dấu) và Đại diện bên Mua (Ký và ghi rõ họ tên).  
4. File tự động tải về máy tính.  
5. Kế toán gửi file PDF qua Zalo/Email cho khách hoặc in ra giấy mang đi đối soát trực tiếp.  

**Alternative Flows**  
- **Gửi tự động qua Email:**  
  1. Kế toán bấm "Gửi trực tiếp qua Email khách". Hệ thống gửi file đính kèm đến email của khách hàng.  

**Postconditions**  
Tài liệu đối chiếu công nợ pháp lý được kết xuất hoàn chỉnh.  

---

### UC-226 Xem lịch sử đối chiếu công nợ (View Reconciliation History)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to view all past reconciliation statements generated for a customer and their signed confirmation statuses to track formal balance agreements.  

**Preconditions**  
Người dùng đã mở hồ sơ công nợ của khách hàng.  

**Normal Flow**  
*Xem danh sách các lần lập và chốt biên bản đối chiếu công nợ*  
1. Người dùng chọn tab "Lịch sử đối chiếu công nợ" (Reconciliation Logs).  
2. Hệ thống hiển thị danh sách các kỳ đối chiếu:  
   - Bảng gồm: Mã biên bản, Kỳ đối chiếu (Tháng/Năm), Ngày lập, Số dư chốt cuối kỳ, Trạng thái (`Đã gửi khách`, `Khách đã ký xác nhận`, `Có khiếu nại`), File đính kèm biên bản có chữ ký của khách.  
3. Người dùng nhấp vào một kỳ để xem chi tiết hoặc tải lại file biên bản cũ.  

**Alternative Flows**  
- **Chưa từng lập đối chiếu:**  
  1. Danh sách trống. Hệ thống hiển thị nút "Lập đối chiếu kỳ đầu tiên".  

**Postconditions**  
Lịch sử chốt công nợ định kỳ được lưu trữ đầy đủ.  

---

### UC-227 Xem khoản phải trả nhà cung cấp (View Accounts Payable)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to track all outstanding accounts payable owed to suppliers from goods receipt purchases so that payables are scheduled responsibly.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý tài chính.  

**Normal Flow**  
*Xem tổng hợp các khoản tiền nợ phải trả cho các hãng và nhà cung cấp*  
1. Người dùng chọn mục "Khoản phải trả nhà cung cấp" (Accounts Payable - AP).  
2. Hệ thống tải và hiển thị danh mục các khoản nợ NCC:  
   - Bảng danh sách: Mã phiếu nhập (PN-xxx), Nhà cung cấp, Ngày nhập hàng, Tổng tiền hàng nhập, Số tiền đã thanh toán, Số tiền còn phải trả (`Remaining Payable`), Hạn thanh toán theo hợp đồng, Trạng thái (Chưa trả / Đã trả 1 phần / Quá hạn), Thao tác ("Thanh toán", "Xem chi tiết").  
   - Thống kê tóm tắt trên đầu: Tổng số tiền phải trả toàn bộ NCC; Số tiền nợ sắp đến hạn trong tuần này.  
3. Người dùng theo dõi để chuẩn bị nguồn tiền chi trả đúng hẹn.  

**Alternative Flows**  
- **Không có khoản nợ NCC:**  
  1. Toàn bộ các lô nhập hàng đã được thanh toán ngay 100%.  
  2. Bảng hiển thị thông báo không có dư nợ phải trả.  

**Postconditions**  
Các khoản nợ nhà cung cấp được phản ánh chuẩn xác.  

---

### UC-228 Xem chi tiết công nợ nhà cung cấp (View Supplier Debt Detail)
**Primary Actors**  
Accountant, Store Owner  

*(Chi tiết xem tại UC-190 (Tab 2) - Xem các hóa đơn nhập, số đã thanh toán, số còn nợ và lịch sử chi tiền cho một nhà cung cấp cụ thể).*  

---

### UC-229 Theo dõi khoản phải trả đến hạn (Track Supplier Due Payments)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to monitor upcoming and past-due supplier payments with timeline alerts so that creditworthiness with manufacturers (TOTO, INAX...) is maintained.  

**Preconditions**  
Có các khoản nợ nhập hàng sắp đến hạn thanh toán trong 7 ngày tới hoặc đã quá hạn.  

**Normal Flow**  
*Theo dõi lịch trình các khoản nợ NCC sắp đến hạn*  
1. Người dùng chọn bộ lọc "Sắp đến hạn trong 7 ngày tới" tại bảng Khoản phải trả (UC-227).  
2. Hệ thống lọc và hiển thị danh sách các phiếu nhập cần thanh toán trong tuần:  
   - Cột Hạn thanh toán được đánh dấu màu vàng (Sắp đến hạn) hoặc màu đỏ (Đã quá hạn).  
   - Số tiền cần chuẩn bị thanh toán cho từng NCC.  
3. Hệ thống hiển thị cảnh báo trên Dashboard tài chính để Store Owner chủ động dòng tiền.  
4. Kế toán nhấp nút "Thanh toán ngay" để lập phiếu chi trả tiền cho NCC (UC-230).  

**Alternative Flows**  
- **Không có khoản nào sắp đến hạn:**  
  1. Toàn bộ các khoản nợ đều có hạn xa (> 15 ngày).  
  2. Hệ thống hiển thị thông báo an toàn dòng tiền.  

**Postconditions**  
Lịch trình nghĩa vụ thanh toán nhà cung cấp được giám sát chặt chẽ.  

---

### UC-230 Ghi nhận thanh toán nhà cung cấp (Record Supplier Payment)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to create a payment voucher linked to specific supplier goods receipts/payables and reduce outstanding accounts payable so that vendor balances are reconciled.  

**Preconditions**  
1. Có khoản nợ phải trả nhà cung cấp (UC-227).  
2. Cửa hàng thực hiện chuyển khoản hoặc chi tiền mặt thanh toán cho NCC.  

**Normal Flow**  
*Lập phiếu chi thanh toán tiền hàng cho nhà cung cấp*  
1. Người dùng nhấp nút "Thanh toán cho NCC" tại dòng nhà cung cấp hoặc phiếu nhập hàng mục tiêu.  
2. Hệ thống hiển thị biểu mẫu thanh toán:  
   - Tên nhà cung cấp thụ hưởng & Tài khoản ngân hàng của NCC.  
   - Danh sách các phiếu nhập kho đang nợ của NCC này: Người dùng chọn các phiếu muốn thanh toán.  
   - Nhập "Số tiền chi trả" (ví dụ: `80.000.000 VNĐ`).  
   - Phương thức thanh toán: `Chuyển khoản ngân hàng` (chọn tài khoản trích tiền của cửa hàng).  
   - Mã giao dịch ủy nhiệm chi ngân hàng (Mã GD/FT).  
   - Đính kèm ảnh chụp Ủy nhiệm chi đã chuyển khoản thành công.  
3. Người dùng kiểm tra thông tin và nhấp nút "Xác nhận chi tiền thanh toán".  
4. Hệ thống kiểm tra: Số tiền chi <= Số dư tài khoản ngân hàng của cửa hàng; Số tiền chi <= Tổng nợ của NCC.  
5. Hệ thống tạo phiếu chi tiền (PC) trong sổ quỹ (UC-210).  
6. Hệ thống giảm trừ số dư tiền ngân hàng của cửa hàng.  
7. Hệ thống tự động giảm trừ công nợ phải trả tương ứng của Nhà cung cấp trong Sổ công nợ NCC.  
8. Các phiếu nhập kho được gạch nợ tự động chuyển trạng thái sang `Đã thanh toán đủ`.  
9. Hệ thống hiển thị thông báo thành công (MSG229: "Ghi nhận thanh toán cho nhà cung cấp thành công! Dư nợ NCC đã được khấu trừ").  

**Alternative Flows**  
- **Thanh toán một phần tiền cho NCC:**  
  1. Trả trước một phần số tiền lô hàng.  
  2. Hệ thống cập nhật số dư nợ còn lại của phiếu nhập đó.  

**Postconditions**  
1. Nghĩa vụ nợ với nhà cung cấp được khấu trừ chính xác.  
2. Dòng tiền chi ngân hàng được phản ánh vào sổ cái kế toán.  

---

### UC-231 Đối chiếu công nợ nhà cung cấp (Supplier Debt Reconciliation)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to generate and export reconciliation data between internal payable records and the supplier's billing statements so that discrepancies in invoices or returns are resolved.  

**Preconditions**  
Nhà cung cấp gửi bảng sao kê công nợ định kỳ tháng để đối chiếu với cửa hàng.  

**Normal Flow**  
*Đối soát công nợ nhập hàng với bảng kê của nhà cung cấp*  
1. Kế toán chọn mục "Đối chiếu công nợ NCC" (Supplier Reconciliation).  
2. Kế toán chọn Nhà cung cấp và Khoảng thời gian đối chiếu (Tháng này).  
3. Hệ thống kết xuất bảng dữ liệu đối soát:  
   - Số dư đầu kỳ.  
   - Danh sách toàn bộ các Phiếu nhập kho trong kỳ (+ Tiền hàng nhập).  
   - Danh sách toàn bộ các Phiếu xuất trả hàng NCC trong kỳ (- Tiền hàng trả - UC-194).  
   - Danh sách các lần chuyển khoản thanh toán cho NCC (- Tiền đã trả).  
   - Số dư nợ cuối kỳ trên hệ thống AQS.  
4. Kế toán đối chiếu từng dòng với bảng sao kê từ hãng gửi sang.  
5. Kế toán nhấp nút "Xuất file Excel đối chiếu" để gửi lại cho kế toán bên hãng xác nhận số dư khớp.  

**Alternative Flows**  
- **Phát hiện chênh lệch (Lệch hóa đơn hoặc chiết khấu thưởng quý):**  
  1. Hãng có khoản chiết khấu thương mại giảm trừ nợ mà cửa hàng chưa nhập.  
  2. Kế toán thực hiện Điều chỉnh công nợ NCC (UC-233) để cân bằng số liệu.  

**Postconditions**  
Số liệu công nợ giữa cửa hàng và hãng sản xuất hoàn toàn khớp nhau.  

---

### UC-232 Xử lý thanh toán thừa (Handle Overpayment)
**Primary Actors**  
Accountant  

**Secondary Actors**  
None  

**Description**  
As an Accountant, I want to manage situations where a customer transfers more money than the order value (either refunding the excess or holding it as customer credit balance for future orders) so that balances remain clean.  

**Preconditions**  
Khách hàng chuyển khoản thanh toán nhiều hơn số tiền phải trả của đơn hàng (ví dụ: đơn hàng 14.800.000 VNĐ nhưng chuyển chẵn 15.000.000 VNĐ).  

**Normal Flow**  
*Xử lý số tiền khách hàng thanh toán dư thừa*  
1. Khi đối soát thanh toán đơn hàng (UC-056), hệ thống phát hiện số tiền thực nhận lớn hơn giá trị đơn hàng (Thừa `200.000 VNĐ`).  
2. Hệ thống hiển thị hộp thoại lựa chọn phương án xử lý số tiền thừa:  
   - Phương án 1: `Lưu vào Số dư có của khách hàng (Customer Credit Balance)`: Giữ lại 200k trong tài khoản khách để tự động trừ vào đơn hàng tiếp theo.  
   - Phương án 2: `Hoàn tiền thừa lại cho khách hàng`: Chuyển khoản trả lại 200k ngay lập tức vào số tài khoản của khách.  
3. Kế toán liên hệ khách và chọn Phương án 1 (Khách đồng ý để lại trừ vào lần mua sau).  
4. Kế toán nhấp "Xác nhận lưu số dư có".  
5. Hệ thống hoàn tất đơn hàng gốc (Fully Paid) và cộng `200.000 VNĐ` vào trường `CreditBalance` trong hồ sơ khách hàng (FE-10).  
6. Hệ thống hiển thị thông báo thành công (MSG230: "Đã ghi nhận số tiền thanh toán thừa vào số dư có của khách hàng thành công!").  

**Alternative Flows**  
- **Khách yêu cầu chuyển khoản hoàn lại tiền thừa:**  
  1. Khách muốn nhận lại tiền thừa.  
  2. Kế toán chọn Phương án 2 -> Kích hoạt Ghi nhận hoàn tiền (UC-061) chi trả lại 200k.  

**Postconditions**  
Khoản tiền thừa được xử lý minh bạch, không để xảy ra tranh chấp tài chính.  

---

### UC-233 Điều chỉnh công nợ (Adjust Debt)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to apply approved debt adjustments (writing off bad debts, recording supplier volume rebates, or fixing posting errors) with mandatory justifications and audit logs.  

**Preconditions**  
1. Người dùng có thẩm quyền kế toán/chủ cửa hàng.  
2. Có biên bản phê duyệt điều chỉnh công nợ (xóa nợ xấu không đòi được, hoặc hãng thưởng doanh số giảm trừ nợ).  

**Normal Flow**  
*Điều chỉnh số dư công nợ khách hàng hoặc nhà cung cấp*  
1. Người dùng mở hồ sơ công nợ cần điều chỉnh và nhấp nút "Điều chỉnh số dư nợ" (Adjust Debt Balance).  
2. Hệ thống hiển thị biểu mẫu:  
   - Đối tượng điều chỉnh: `Khách hàng` hoặc `Nhà cung cấp`.  
   - Dư nợ hiện tại: ví dụ nợ `5.000.000 VNĐ`.  
   - Loại điều chỉnh: `Giảm trừ nợ (-)` hoặc `Tăng nợ (+)`.  
   - Số tiền điều chỉnh: ví dụ giảm `5.000.000 VNĐ`.  
   - Lý do điều chỉnh (Dropdown): `Xóa sổ nợ xấu đã được phê duyệt`, `Nhà cung cấp thưởng doanh số trừ nợ`, `Bù trừ sai sót số liệu`, `Khác`.  
   - Ô bắt buộc nhập: "Mô tả chi tiết và căn cứ phê duyệt" (tối thiểu 10 ký tự).  
   - Đính kèm biên bản phê duyệt xóa nợ / công văn từ hãng (UC-212).  
3. Người dùng nhập thông tin và tải lên biên bản.  
4. Người dùng nhấp nút "Xác nhận điều chỉnh nợ".  
5. Hệ thống kiểm tra: Lý do và căn cứ đầy đủ.  
6. Hệ thống cập nhật số dư công nợ mới trong cơ sở dữ liệu.  
7. Hệ thống tự động ghi nhận biến động kiểm toán chi tiết vào Audit Log (FE-02).  
8. Hệ thống hiển thị thông báo thành công (MSG231: "Điều chỉnh số dư công nợ thành công! Dữ liệu đã được lưu vết kiểm toán").  

**Alternative Flows**  
- **Không nhập căn cứ lý do:**  
  1. Bỏ trống mô tả. Hệ thống chặn lưu (MSG40).  

**Postconditions**  
Số dư công nợ được điều chỉnh hợp lệ theo quy chế tài chính.  

---

### UC-234 Xem báo cáo thu - chi (View Income and Expense Report)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to view a categorized Income and Expense report across custom timeframes and payment methods to analyze cash flow structure.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý tài chính.  

**Normal Flow**  
*Xem báo cáo tổng hợp các khoản thu và chi theo nhóm nghiệp vụ*  
1. Người dùng chọn mục "Báo cáo Thu - Chi" (Income & Expense Report).  
2. Hệ thống hiển thị bảng báo cáo thu chi phân loại đa cấp:  
   - Phần I: **CÁC KHOẢN THU (INFLOWS)**:  
     + Thu bán hàng tại quầy.  
     + Thu đơn hàng online VietQR / COD.  
     + Thu nợ từ khách hàng công trình.  
     + Thu nhập tài chính và thu khác.  
     + **Tổng thu trong kỳ**: (VNĐ).  
   - Phần II: **CÁC KHOẢN CHI (OUTFLOWS)**:  
     + Chi trả tiền hàng nhà cung cấp.  
     + Chi phí vận hành (Điện, nước, mặt bằng, bốc vác).  
     + Chi lương và thưởng nhân viên.  
     + Chi hoàn tiền cho khách.  
     + **Tổng chi trong kỳ**: (VNĐ).  
   - Phần III: **THU - CHI THUẦN (NET CASH MOVEMENT)**: Tổng thu - Tổng chi.  
3. Người dùng lọc theo tháng/quý để xem biến động và xuất ra file Excel.  

**Alternative Flows**  
- **Kỳ có dòng tiền âm (Chi nhiều hơn thu):**  
  1. Tháng nhập nhiều lô hàng lớn. Cột chênh lệch hiển thị số âm màu đỏ cảnh báo.  

**Postconditions**  
Báo cáo cơ cấu thu - chi được trình bày trực quan, chuẩn mực.  

---

### UC-235 Xem báo cáo dòng tiền (View Cash Flow Report)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want to view the Cash Flow Statement tracking operating, investing, and financing cash movements to ensure store liquidity.  

**Preconditions**  
Người dùng có quyền xem báo cáo tài chính cấp cao.  

**Normal Flow**  
*Xem báo cáo lưu chuyển tiền tệ (Cash Flow Statement)*  
1. Người dùng chọn mục "Báo cáo Lưu chuyển tiền tệ" (Cash Flow Report).  
2. Hệ thống tổng hợp các dòng tiền theo chuẩn kế toán:  
   - Dòng tiền từ hoạt động kinh doanh (Bán hàng, nhập kho, chi phí vận hành).  
   - Dòng tiền từ hoạt động đầu tư (Mua sắm máy móc POS, sửa sang showroom).  
   - Dòng tiền từ hoạt động tài chính (Vốn góp chủ sở hữu, trả lãi vay nếu có).  
   - Tiền và tương đương tiền đầu kỳ.  
   - Lưu chuyển tiền thuần trong kỳ.  
   - Tiền và tương đương tiền cuối kỳ (Khớp đúng số dư tài khoản ngân hàng + Tiền mặt thực tế).  
3. Biểu đồ thác nước (Waterfall Chart) thể hiện sự tăng/giảm của dòng tiền qua các tuần.  
4. Người dùng theo dõi để đảm bảo không bị mất thanh khoản.  

**Alternative Flows**  
- **Xuất file báo cáo:**  
  1. Người dùng bấm "Xuất PDF". Hệ thống tạo bản in báo cáo lưu chuyển tiền tệ.  

**Postconditions**  
Bức tranh lưu chuyển tiền tệ được phác họa toàn diện.  

---

### UC-236 Xem báo cáo doanh thu (View Revenue Report)
**Primary Actors**  
Accountant, Store Owner  

*(Chi tiết xem tại UC-116 và UC-117 - Báo cáo doanh thu thuần, giảm trừ doanh thu và doanh thu theo từng kênh bán).*  

---

### UC-237 Xem báo cáo công nợ nhà cung cấp (View Supplier Debt Report)
**Primary Actors**  
Accountant, Store Owner  

*(Chi tiết xem tại UC-121 (Tab 2) - Báo cáo tổng hợp số dư nợ phải trả, hạn nợ và tình trạng thanh toán cho các hãng thiết bị vệ sinh).*  

---

### UC-238 Xem báo cáo khoản phải thu (View Receivable Report)
**Primary Actors**  
Accountant, Store Owner  

*(Chi tiết xem tại UC-121 (Tab 1) và UC-217 - Báo cáo chi tiết các khoản nợ phải thu của khách hàng phân bổ theo tuổi nợ).*  

---

### UC-239 Xem báo cáo khoản phải trả (View Payable Report)
**Primary Actors**  
Accountant, Store Owner  

*(Chi tiết xem tại UC-227 và UC-229 - Báo cáo nghĩa vụ phải trả nhà cung cấp theo thời hạn thanh toán).*  

---

### UC-240 Xem số liệu tổng quan kế toán (View Accounting Dashboard)
**Primary Actors**  
Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As an Accountant or Store Owner, I want an executive Accounting Dashboard displaying instant liquidity (cash on hand, bank balance, net receivables, net payables, overdue debt indicators) so that financial health is assessed in seconds.  

**Preconditions**  
Kế toán hoặc Chủ cửa hàng đăng nhập vào phân hệ Kế toán.  

**Normal Flow**  
*Xem bảng điều khiển tổng quan tài chính kế toán*  
1. Người dùng truy cập trang chủ Phân hệ Kế toán (Accounting Dashboard).  
2. Hệ thống tổng hợp và hiển thị các khối chỉ số tài chính tức thì:  
   - **(1) Quỹ tiền mặt tại cửa hàng**: Số tiền mặt đang có trong các két quầy và két chính.  
   - **(2) Tiền gửi ngân hàng**: Số dư tài khoản ngân hàng nhận tiền VietQR chính thức.  
   - **(3) Tổng tài sản tiền tức thời**: (1) + (2).  
   - **(4) Tổng công nợ phải thu (AR)**: Tiền khách hàng còn nợ (kèm nhãn cảnh báo nợ quá hạn).  
   - **(5) Tổng công nợ phải trả (AP)**: Tiền còn nợ các hãng cung cấp (kèm nhãn nợ sắp đến hạn).  
   - **(6) Vốn lưu động ròng tức thời**: (Tài sản tiền + Phải thu) - Phải trả.  
3. Biểu đồ so sánh thu chi hàng ngày trong tháng và lịch nhắc hẹn các khoản thanh toán trong 7 ngày tới.  
4. Người dùng xem đối soát và bấm vào từng thẻ số để chuyển thẳng đến màn hình nghiệp vụ chi tiết.  

**Alternative Flows**  
- **Cảnh báo mất cân đối thanh khoản:**  
  1. Tổng nợ phải trả sắp đến hạn lớn hơn Tài sản tiền hiện có.  
  2. Khối Vốn lưu động hiển thị cảnh báo đỏ nhấp nháy: "Cảnh báo thiếu hụt dòng tiền thanh toán trong tuần này. Cần đẩy mạnh thu hồi nợ khách hàng!".  

**Postconditions**  
Toàn bộ bức tranh sức khỏe tài chính của cửa hàng được cung cấp chính xác, tức thì.  

---
