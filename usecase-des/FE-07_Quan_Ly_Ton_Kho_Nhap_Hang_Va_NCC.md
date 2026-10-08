# PHÂN HỆ FE-07: QUẢN LÝ TỒN KHO, NHẬP HÀNG & NHÀ CUNG CẤP (INVENTORY & SUPPLIER MANAGEMENT)

---

### UC-074 Xem tồn kho (View Inventory)
**Primary Actors**  
Store Owner, Sales Staff, Accountant  

**Secondary Actors**  
None  

**Description**  
As an internal user, I want to view stock levels for all product variants with a three-layer inventory breakdown (On-hand Stock, Reserved Stock, and Available-to-Sell Stock) so that sales and purchasing are accurately planned.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý kho.  

**Normal Flow**  
*Xem danh sách tồn kho theo cơ chế ba lớp số lượng*  
1. Người dùng chọn mục "Báo cáo tồn kho tức thì" (Live Inventory).  
2. Hệ thống tải và hiển thị bảng dữ liệu tồn kho theo từng biến thể sản phẩm:  
   - Thông tin nhận diện: Ảnh sản phẩm, Mã SKU, Tên biến thể, Danh mục, Thương hiệu, Đơn vị tính (Bộ/Cái).  
   - Cột **Tồn thực tế (On-hand / Physical Stock)**: Tổng số lượng hàng hóa đang nằm vật lý trong kho.  
   - Cột **Số lượng đang giữ (Reserved Stock)**: Số lượng hàng đã được khách đặt trong các đơn đã xác nhận (UC-086) nhưng chưa xuất kho đi giao.  
   - Cột **Có thể bán (Available to Sell - ATS)**: `Tồn khả dụng = Tồn thực tế - Số lượng đang giữ`.  
   - Cột **Ngưỡng tồn tối thiểu (Min Stock Level)**: Mức tồn kho an toàn đã cấu hình.  
   - Cột **Trạng thái kho**: Huy hiệu màu (Xanh lá: "Đủ hàng"; Vàng: "Chạm ngưỡng an toàn"; Đỏ: "Hết hàng khả dụng").  
3. Người dùng duyệt danh sách, có thể sắp xếp theo cột Có thể bán tăng dần để xem những mặt hàng cần bổ sung.  

**Alternative Flows**  
- **Tồn khả dụng bằng 0 hoặc âm:**  
  1. Hàng thực tế đã được giữ chỗ hết cho các đơn hàng đã duyệt.  
  2. Cột Có thể bán hiển thị số 0 màu đỏ; nút bán hàng cho mặt hàng này sẽ bị cảnh báo hoặc yêu cầu đặt trước từ NCC.  

**Postconditions**  
Số liệu tồn kho 3 lớp được phản ánh chính xác theo thời gian thực.  

---

### UC-075 Lập phiếu nhập hàng (Create Goods Receipt)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to create a Goods Receipt from a supplier (selecting supplier, variants, received quantities, cost prices) so that physical incoming stock is officially verified and accounted for.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập vào hệ thống.  
2. Nhà cung cấp và các biến thể sản phẩm đã có trên hệ thống.  

**Normal Flow**  
*Lập và hoàn tất phiếu nhập hàng từ nhà cung cấp*  
1. Chủ cửa hàng nhấp nút "Tạo phiếu nhập hàng mới" (Create Goods Receipt).  
2. Hệ thống hiển thị biểu mẫu lập phiếu nhập:  
   - Mã phiếu nhập: Tự động sinh `PN-YYYYMMDD-XXXXX`.  
   - Chọn Nhà cung cấp (Dropdown NCC đang hoạt động).  
   - Chọn Đơn đặt hàng NCC liên kết (Purchase Order - nếu có kế thừa từ UC-182).  
   - Bảng kê danh sách mặt hàng nhập: Ô tìm kiếm và chọn SKU, Tên biến thể, ĐVT, Ô nhập "Số lượng nhập", Ô nhập "Đơn giá nhập" (Cost Price), Thành tiền từng dòng.  
   - Bảng tổng tiền: Tổng tiền hàng nhập, Chiết khấu NCC (nếu có), Thuế VAT nhập khẩu, Tổng tiền phải trả nhà cung cấp.  
   - Ô nhập Ghi chú đợt nhập (ví dụ: "Nhập lô bồn cầu TOTO tháng 10/2026 kèm hóa đơn VAT số 001234").  
3. Chủ cửa hàng chọn NCC, thêm các biến thể và nhập số lượng, đơn giá nhập thực tế.  
4. Chủ cửa hàng nhấp nút "Lưu và Hoàn tất nhập kho".  
5. Hệ thống kiểm tra dữ liệu: Đã chọn NCC, danh sách mặt hàng có ít nhất 1 dòng, số lượng và giá nhập > 0.  
6. Nếu các biến thể có cấu hình Serial (UC-030), hệ thống yêu cầu quét/nhập danh sách mã Serial cho từng thiết bị nhập kho.  
7. Hệ thống tự động cộng tăng số lượng Tồn thực tế (On-hand Stock) của từng biến thể tương ứng.  
8. Hệ thống cập nhật Giá vốn bình quân gia quyền (Weighted Average Cost) của sản phẩm trong kho.  
9. Hệ thống tự động ghi nhận công nợ phải trả Nhà cung cấp (liên kết ACC-F35).  
10. Hệ thống ghi nhật ký sự kiện vào Audit Log.  
11. Hệ thống hiển thị thông báo thành công (MSG165: "Nhập hàng thành công! Tồn kho thực tế đã được cộng thêm").  

**Alternative Flows**  
- **Step 4.1_Lưu dưới dạng Phiếu tạm / Nháp (Draft):**  
  1. Hàng đang bốc dỡ chưa kiểm đếm xong hoặc chờ kiểm tra hóa đơn.  
  2. Chủ cửa hàng chọn "Lưu bản nháp". Hệ thống tạo phiếu nhập ở trạng thái `Bản nháp (Draft)` và chưa cộng tồn kho (Kích hoạt UC-179, UC-181).  
- **Step 5_Số lượng hoặc đơn giá nhập <= 0:**  
  1. Có dòng sản phẩm có số lượng hoặc đơn giá bằng 0.  
  2. Hệ thống cảnh báo (MSG166: "Số lượng và đơn giá nhập của tất cả các dòng phải lớn hơn 0").  

**Postconditions**  
1. Phiếu nhập hàng được lưu vĩnh viễn ở trạng thái Hoàn tất.  
2. Tồn kho thực tế tăng lên ngay lập tức.  
3. Công nợ nhà cung cấp được cập nhật tự động.  

---

### UC-076 Xem chi tiết phiếu nhập (View Goods Receipt Detail)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to view complete details of a goods receipt (supplier, line items, received quantities, cost prices, total payable, receiving staff, serial list) for financial audit.  

**Preconditions**  
Người dùng đã mở danh sách phiếu nhập và nhấp vào một phiếu cụ thể.  

**Normal Flow**  
*Xem đầy đủ chi tiết chứng từ của một phiếu nhập hàng*  
1. Hệ thống mở màn hình "Chi tiết phiếu nhập kho - [Mã PN]":  
   - Khối Thông tin chung: Mã phiếu nhập, Ngày giờ nhập hàng, Người lập phiếu, Trạng thái (Hoàn tất / Nháp / Đã hủy).  
   - Khối Nhà cung cấp: Tên công ty/nhà phân phối, Mã số thuế, Địa chỉ, SĐT người liên hệ.  
   - Bảng kê sản phẩm: STT, Mã SKU, Tên biến thể, ĐVT, Số lượng nhập, Đơn giá nhập, Thành tiền, Danh sách mã Serial đã nhập của từng sản phẩm.  
   - Khối Tổng kết tài chính: Tổng tiền hàng, Tiền thuế, Số tiền đã thanh toán cho NCC, Dư nợ còn lại của phiếu nhập này.  
   - Lịch sử thao tác: Ai đã duyệt hoàn tất, thời gian phê duyệt.  
2. Người dùng kiểm tra thông tin hoặc nhấp nút "In phiếu nhập kho".  

**Alternative Flows**  
- **Không tìm thấy phiếu:**  
  1. Mã phiếu không tồn tại. Hệ thống hiển thị thông báo lỗi và quay lại danh sách.  

**Postconditions**  
Mọi thông tin chứng từ nhập kho được cung cấp minh bạch, đầy đủ.  

---

### UC-077 Cập nhật/Hủy phiếu nhập hàng (Update/Cancel Goods Receipt)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to edit or cancel a mistaken goods receipt with mandatory reason logging so that inventory counts and supplier payables are systematically reversed.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập với vai trò `Store Owner`.  
2. Phiếu nhập hàng mục tiêu đang ở trạng thái Hoàn tất và số lượng hàng nhập chưa bị xuất bán vượt quá tồn kho hiện tại.  

**Normal Flow**  
*Hủy phiếu nhập hàng đã hoàn tất do sai sót nghiêm trọng*  
1. Tại chi tiết phiếu nhập kho, Chủ cửa hàng nhấp nút "Hủy phiếu nhập kho" (Cancel Goods Receipt).  
2. Hệ thống kiểm tra điều kiện an toàn kho:  
   - Kiểm tra số lượng tồn kho thực tế hiện tại của các sản phẩm trong phiếu: Tồn hiện tại phải >= Số lượng đã nhập của phiếu này (đảm bảo chưa bị bán âm kho nếu trừ ngược lại).  
3. Nếu thỏa mãn, hệ thống hiển thị hộp thoại cảnh báo:  
   - Cảnh báo: "Khi hủy phiếu nhập kho [Mã PN], toàn bộ số lượng tồn kho tương ứng sẽ bị TRỪ NGƯỢC LẠI và công nợ nhà cung cấp sẽ bị HỦY BỎ. Hành động này không thể hoàn tác!".  
   - Ô bắt buộc nhập: "Lý do hủy phiếu nhập kho" (tối thiểu 10 ký tự).  
4. Chủ cửa hàng nhập lý do chi tiết (ví dụ: "Nhập nhầm nhà cung cấp và sai số lượng toàn bộ lô sen tắm") và nhấp nút "Xác nhận hủy phiếu".  
5. Hệ thống chuyển trạng thái phiếu nhập sang `Đã hủy (Cancelled)`.  
6. Hệ thống thực hiện trừ lùi số lượng Tồn thực tế tương ứng trong kho.  
7. Hệ thống giải phóng hoặc thu hồi các mã Serial đã khai báo trong phiếu đó.  
8. Hệ thống đảo ngược khoản công nợ phải trả NCC liên quan trong sổ kế toán.  
9. Hệ thống ghi nhận sự kiện vào Audit Log với đầy đủ thông tin trước/sau và lý do.  
10. Hệ thống hiển thị thông báo thành công (MSG167: "Đã hủy phiếu nhập kho và điều chỉnh tồn kho thành công!").  

**Alternative Flows**  
- **Hàng trong phiếu đã bị xuất bán (Không đủ tồn để trừ ngược):**  
  1. Một số sản phẩm trong lô hàng nhập đã được xuất bán cho khách, tồn kho hiện tại nhỏ hơn số lượng cần trừ.  
  2. Hệ thống chặn thao tác hủy và hiển thị thông báo lỗi (MSG168: "Không thể hủy phiếu nhập kho vì một số sản phẩm đã được xuất bán cho khách hàng. Vui lòng thực hiện Kiểm kê & Điều chỉnh tồn kho thủ công").  
- **Không nhập lý do hủy:**  
  1. Chủ cửa hàng bỏ trống ô lý do.  
  2. Hệ thống báo lỗi (MSG40: "Bắt buộc nhập lý do hủy phiếu nhập").  

**Postconditions**  
1. Phiếu nhập bị hủy hoàn toàn.  
2. Tồn kho và công nợ nhà cung cấp được hoàn nguyên chính xác.  
3. Audit Log ghi nhận đầy đủ chi tiết kiểm toán.  

---

### UC-078 Xem lịch sử nhập hàng (View Import History)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to review historical goods receipts filtered by supplier, variant, date range, and creator to audit receiving trends and supplier cost fluctuations.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý kho.  

**Normal Flow**  
*Xem và trích xuất lịch sử các đợt nhập hàng*  
1. Người dùng chọn mục "Lịch sử nhập kho" (Import History).  
2. Hệ thống hiển thị giao diện tra cứu lịch sử:  
   - Bộ lọc: Lọc theo Nhà cung cấp, Lọc theo Khoảng ngày nhập, Lọc theo Trạng thái (Hoàn tất / Đã hủy), Lọc theo Người lập phiếu.  
   - Bảng danh sách: Mã phiếu, Ngày nhập, Nhà cung cấp, Tổng số mặt hàng, Tổng giá trị tiền hàng nhập (VNĐ), Người nhận hàng, Trạng thái.  
3. Người dùng lọc theo tháng hiện tại để kiểm tra tổng giá trị hàng nhập trong kỳ.  
4. Người dùng có thể nhấp nút "Xuất file Excel" để phục vụ đối soát chi phí đầu vào.  

**Alternative Flows**  
- **Không tìm thấy bản ghi:**  
  1. Không có phiếu nhập nào trong khoảng thời gian lọc.  
  2. Hệ thống hiển thị thông báo không có dữ liệu.  

**Postconditions**  
Toàn bộ lịch sử nhập kho được kết xuất chính xác, minh bạch.  

---

### UC-079 Quản lý nhà cung cấp (Manage Suppliers)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to manage supplier profiles (business name, tax code, contacts, address, bank accounts, payment terms) so that procurement workflows run smoothly.  

**Preconditions**  
Chủ cửa hàng đã đăng nhập và truy cập trang "Quản lý nhà cung cấp".  

**Normal Flow**  
*Xem danh mục và cập nhật hồ sơ các nhà cung cấp thiết bị vệ sinh*  
1. Hệ thống hiển thị danh sách các nhà cung cấp đang hợp tác: Mã NCC (ví dụ: `NCC-TOTO-VN`), Tên công ty phân phối, Mã số thuế, Đại diện bán hàng, Số điện thoại, Địa chỉ kho, Tổng tiền hàng đã nhập lũy kế, Dư nợ hiện tại, Trạng thái hoạt động.  
2. Chủ cửa hàng có thể thực hiện: Thêm mới NCC (UC-187), Cập nhật thông tin (UC-188), Xem chi tiết lịch sử giao dịch (UC-190), Khóa/Mở khóa NCC (UC-191).  
3. Chủ cửa hàng nhấp vào nhà cung cấp mong muốn để kiểm tra điều khoản công nợ (ví dụ: Công nợ gối đầu 30 ngày, hạn mức 200 triệu đồng).  

**Alternative Flows**  
- **Danh sách NCC trống:**  
  1. Chưa có NCC nào. Hệ thống hiển thị nút "Thêm nhà cung cấp đầu tiên".  

**Postconditions**  
Danh bạ nhà cung cấp được quản lý tập trung và an toàn.  

---

### UC-080 Thực hiện kiểm kê kho (Perform Stock Count)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to conduct physical stock counts by scanning or counting actual units on warehouse shelves and entering physical counts to identify variances against system records.  

**Preconditions**  
1. Người dùng đã đăng nhập vào hệ thống kho.  
2. Cửa hàng tiến hành kiểm kê định kỳ (cuối tuần, cuối tháng hoặc đột xuất).  

**Normal Flow**  
*Mở phiếu kiểm kê và nhập số lượng thực tế tại kho*  
1. Người dùng nhấp nút "Tạo đợt kiểm kê mới" (New Stock Count).  
2. Hệ thống hiển thị biểu mẫu tạo phiếu kiểm kê:  
   - Mã phiếu kiểm kê: Tự động sinh `KK-YYYYMMDD-XXXXX`.  
   - Phạm vi kiểm kê: `Toàn bộ kho` hoặc `Theo danh mục cụ thể` (ví dụ: chỉ kiểm kê nhóm Sen cây nhiệt độ).  
   - Nhân viên tham gia kiểm kê (chọn từ danh sách nhân viên).  
   - Ghi chú đợt kiểm kê.  
3. Người dùng chọn phạm vi kiểm kê và nhấp "Bắt đầu kiểm kê".  
4. Hệ thống tải danh sách toàn bộ các biến thể thuộc phạm vi kiểm kê:  
   - Cột Mã SKU, Tên biến thể, Tồn hệ thống ghi nhận tại thời điểm bắt đầu (`SystemQuantity`).  
   - Cột nhập liệu "Số lượng thực kiểm" (`ActualQuantity`).  
   - Cột tính toán chênh lệch tự động (`Difference = ActualQuantity - SystemQuantity`).  
5. Người dùng đi kiểm đếm thực tế từng kệ hàng và nhập số lượng đếm được vào cột "Số lượng thực kiểm" (hoặc dùng máy quét mã vạch quét từng sản phẩm để tự động cộng dồn).  
6. Hệ thống tự động tính toán chênh lệch theo thời gian thực:  
   - Chênh lệch = 0: Hiển thị màu xanh lá (Khớp).  
   - Chênh lệch < 0: Hiển thị màu đỏ (Thiếu hàng / Thất thoát).  
   - Chênh lệch > 0: Hiển thị màu vàng (Thừa hàng).  
7. Người dùng kiểm tra lại toàn bộ danh sách và nhấp nút "Lưu biên bản kiểm kê".  
8. Hệ thống lưu phiếu kiểm kê ở trạng thái `Đã kiểm đếm - Chờ đối soát và điều chỉnh`.  
9. Hệ thống hiển thị thông báo thành công (MSG169: "Lưu biên bản kiểm kê thành công! Bạn có thể chuyển sang bước Điều chỉnh tồn kho sau kiểm kê").  

**Alternative Flows**  
- **Tạm dừng lưu nháp kiểm kê:**  
  1. Hết giờ làm việc kiểm kê chưa xong toàn bộ kho.  
  2. Người dùng nhấp "Lưu tạm". Hệ thống lưu các số liệu đã đếm được để tiếp tục kiểm vào ca hôm sau mà không làm mất dữ liệu.  

**Postconditions**  
Biên bản đối soát kiểm kê kho được xác lập với đầy đủ số liệu thực tế và chênh lệch.  

---

### UC-081 Điều chỉnh tồn kho (Adjust Stock)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to approve stock adjustments from a completed stock count with mandatory reason logging so that system on-hand balances match physical counts and variance losses are recorded.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập với vai trò `Store Owner`.  
2. Đã có biên bản kiểm kê kho (UC-080) có phát sinh chênh lệch (Difference != 0).  

**Normal Flow**  
*Cân bằng số liệu tồn kho theo biên bản kiểm kê thực tế*  
1. Chủ cửa hàng mở chi tiết biên bản kiểm kê kho cần điều chỉnh.  
2. Hệ thống hiển thị danh sách các sản phẩm có chênh lệch giữa thực kiểm và hệ thống (ví dụ: SKU-TOTO-001 thiếu 1 cái; SKU-INAX-002 thừa 2 cái).  
3. Chủ cửa hàng nhấp nút "Cân bằng & Điều chỉnh tồn kho" (Apply Stock Adjustment).  
4. Hệ thống mở cửa sổ xác nhận điều chỉnh kèm yêu cầu:  
   - Bắt buộc nhập "Lý do điều chỉnh kiểm kê" (ví dụ: "Bù trừ sau kiểm kê định kỳ cuối tháng 9; 1 bồn cầu bị vỡ trong lúc bốc xếp đã lập biên bản hủy; thừa 2 sen vòi do bỏ sót chưa nhập phiếu đợt trước").  
   - Checkbox xác nhận: "Tôi xác nhận ghi nhận khoản hao hụt/thừa này vào chi phí tổn thất kho".  
5. Chủ cửa hàng nhập lý do và bấm "Xác nhận điều chỉnh".  
6. Hệ thống cập nhật số lượng Tồn thực tế của từng SKU về đúng bằng Số lượng thực kiểm (`On-hand = ActualQuantity`).  
7. Hệ thống tự động ghi nhận các bản ghi biến động kho tương ứng (Stock Movement Logs - UC-082) với loại giao dịch `ADJUSTMENT`.  
8. Nếu có thiết bị thiếu quản lý theo Serial: Hệ thống đánh dấu mã Serial bị thiếu là `Lost/Damaged`.  
9. Hệ thống tự động ghi nhận sự kiện toàn diện vào Audit Log (FE-02).  
10. Hệ thống hiển thị thông báo thành công (MSG170: "Điều chỉnh cân bằng tồn kho thành công! Số lượng tồn kho hệ thống đã được cập nhật chính xác").  

**Alternative Flows**  
- **Chủ cửa hàng từ chối điều chỉnh (Yêu cầu đếm lại):**  
  1. Phát hiện chênh lệch quá lớn, nghi ngờ nhân viên đếm nhầm.  
  2. Chủ cửa hàng bấm "Yêu cầu kiểm đếm lại". Hệ thống chuyển trạng thái phiếu kiểm kê về cho nhân viên đếm lại lần 2.  
- **Không nhập lý do điều chỉnh:**  
  1. Bỏ trống ô lý do.  
  2. Hệ thống chặn lưu (MSG40: "Bắt buộc nhập lý do điều chỉnh tồn kho").  

**Postconditions**  
1. Số liệu tồn kho thực tế của hệ thống hoàn toàn trùng khớp với hiện vật trong kho.  
2. Biến động được ghi vết kiểm toán chi tiết.  

---

### UC-082 Xem lịch sử biến động tồn kho (View Stock Movement History)
**Primary Actors**  
Store Owner, Accountant, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As an internal user, I want to trace the complete chronological transaction history of a specific SKU (sales exports, purchases, returns, scrap, adjustments) with running balances to investigate discrepancies.  

**Preconditions**  
Người dùng đã mở màn hình chi tiết tồn kho của một biến thể (UC-186) hoặc mục Nhật ký kho tổng hợp.  

**Normal Flow**  
*Xem sổ chi tiết thẻ kho và diễn biến số lượng của một sản phẩm*  
1. Người dùng chọn xem "Thẻ kho / Lịch sử biến động" của một mã SKU cụ thể.  
2. Hệ thống truy vấn toàn bộ các giao dịch tác động đến SKU này và hiển thị bảng thẻ kho theo thứ tự thời gian:  
   - Cột Thời gian: Ngày, giờ, phút, giây.  
   - Cột Loại giao dịch: `Nhập kho từ NCC` (IMPORT), `Xuất bán đơn hàng` (SALE_EXPORT), `Khách trả hàng đổi mới` (CUSTOMER_RETURN), `Trả hàng về NCC` (SUPPLIER_RETURN), `Điều chỉnh kiểm kê` (ADJUSTMENT).  
   - Cột Mã chứng từ liên quan: Mã phiếu nhập (PN-xxx), Mã đơn hàng (ORD-xxx), Mã phiếu xuất (PX-xxx), Mã phiếu kiểm kê (KK-xxx) có liên kết bấm vào xem chi tiết.  
   - Cột Số lượng thay đổi: Nhập (+X), Xuất (-Y).  
   - Cột Tồn lũy kế sau giao dịch (`Running Balance`): Số tồn chính xác ngay sau khi giao dịch đó diễn ra.  
   - Cột Người thực hiện & Ghi chú lý do.  
3. Người dùng đối chiếu từng dòng giao dịch để truy vết tại sao số lượng tăng/giảm.  

**Alternative Flows**  
- **Lọc theo khoảng ngày:**  
  1. Người dùng chọn lọc thẻ kho trong tuần này. Hệ thống hiển thị số dư đầu kỳ, biến động trong kỳ và số dư cuối kỳ chuẩn xác.  

**Postconditions**  
Dòng chảy hàng hóa của từng mặt hàng được minh bạch hóa hoàn toàn.  

---

### UC-083 Cấu hình mức tồn kho tối thiểu (Set Minimum Stock Level)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to define safety stock threshold levels (Min Stock Level) for each variant so that the system triggers early low-stock warnings before items sell out.  

**Preconditions**  
Chủ cửa hàng đang ở biểu mẫu chỉnh sửa biến thể hoặc bảng cấu hình định mức tồn kho.  

**Normal Flow**  
*Thiết lập ngưỡng tồn kho an toàn cho biến thể*  
1. Chủ cửa hàng truy cập mục "Cấu hình định mức tồn kho".  
2. Hệ thống hiển thị bảng các sản phẩm kèm ô nhập "Định mức tồn tối thiểu" (Min Stock).  
3. Chủ cửa hàng nhập số lượng tối thiểu an toàn cho từng SKU (ví dụ: Đối với Bồn cầu TOTO bán chạy, đặt Min Stock = 5 bộ; đối với Phụ kiện van xả đặt Min Stock = 20 cái).  
4. Chủ cửa hàng nhấp nút "Lưu định mức".  
5. Hệ thống kiểm tra: Mức tồn tối thiểu là số nguyên >= 0.  
6. Hệ thống lưu trường `MinStockLevel` vào bản ghi biến thể trong cơ sở dữ liệu.  
7. Hệ thống hiển thị thông báo thành công (MSG171: "Cập nhật định mức tồn kho tối thiểu thành công!").  

**Alternative Flows**  
- **Nhập số âm:**  
  1. Người dùng nhập số lượng âm.  
  2. Hệ thống cảnh báo lỗi (MSG172: "Định mức tồn kho tối thiểu phải lớn hơn hoặc bằng 0").  

**Postconditions**  
Hệ thống có căn cứ định mức để tự động kích hoạt cảnh báo tồn kho thấp (UC-085).  

---

### UC-084 Xem danh sách hàng dưới mức tồn (View Low Stock List)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to view a prioritized list of all variants currently at or below their configured safety stock thresholds so that replenishment orders can be placed proactively.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý kho.  

**Normal Flow**  
*Xem danh sách các mặt hàng sắp hết để lên kế hoạch nhập hàng*  
1. Người dùng chọn mục "Hàng chạm/dưới mức an toàn" (Low Stock Alert List).  
2. Hệ thống truy vấn các biến thể có `AvailableStock <= MinStockLevel`:  
   - Bảng danh sách: Ảnh, Mã SKU, Tên biến thể, Tồn khả dụng hiện tại, Mức tồn tối thiểu, Số lượng đề xuất nhập bổ sung (`SuggestedRestock = MinStockLevel * 2 - AvailableStock`), Nhà cung cấp chính, Đơn giá nhập gần nhất.  
   - Các dòng được bôi màu cam cảnh báo đối với hàng sắp hết và màu đỏ đối với hàng đã hết sạch (Tồn = 0).  
3. Người dùng tích chọn các mặt hàng cần nhập và nhấp nút "Tạo đề xuất nhập hàng nhanh" (kích hoạt UC-138).  

**Alternative Flows**  
- **Kho đầy đủ hàng (Không có mặt hàng nào dưới định mức):**  
  1. Tất cả sản phẩm đều có tồn kho an toàn.  
  2. Màn hình hiển thị: "Tất cả các sản phẩm đều đang ở mức tồn kho an toàn!".  

**Postconditions**  
Danh sách hàng cần bổ sung được cung cấp nhanh chóng, tránh đứt gãy nguồn cung.  

---

### UC-085 Nhận cảnh báo tồn kho thấp (Receive Low Stock Alert)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
Email Service  

**Description**  
As a Store Owner or Sales Staff, I want to receive automated system notifications (dashboard toast, notification center badge, email) as soon as any variant drops below its safety stock threshold so that out-of-stock incidents are prevented.  

**Preconditions**  
1. Sản phẩm đã được cấu hình mức tồn tối thiểu (UC-083).  
2. Một đơn hàng vừa được xác nhận hoặc hàng vừa được xuất kho khiến tồn khả dụng rơi xuống dưới ngưỡng tối thiểu.  

**Normal Flow**  
*Hệ thống phát cảnh báo tự động khi tồn kho xuống thấp*  
1. Ngay khi giao dịch xuất kho hoặc xác nhận đơn hàng thành công, hệ thống kiểm tra tồn kho khả dụng mới: `AvailableStock <= MinStockLevel`.  
2. Hệ thống tự động tạo một thông báo cảnh báo vận hành khẩn cấp trong hệ thống:  
   - Tiêu đề: `Cảnh báo tồn kho thấp: [Tên sản phẩm] - [Mã SKU]`.  
   - Nội dung: `Số lượng khả dụng chỉ còn [X] (Ngưỡng an toàn là [Y]). Đề xuất nhập thêm hàng từ NCC [Tên NCC]`.  
3. Hệ thống hiển thị huy hiệu thông báo đỏ trên thanh tiêu đề của Store Owner và Sales Staff đang đăng nhập.  
4. Hệ thống gọi Secondary Actor `Email Service` gửi thư cảnh báo định kỳ tới hòm thư của Store Owner.  
5. Người dùng nhấp vào thông báo để chuyển thẳng đến màn hình Tạo đề xuất nhập hàng (UC-138).  

**Alternative Flows**  
- **Cấu hình tắt thông báo email:**  
  1. Store Owner chọn chỉ nhận cảnh báo trên giao diện web.  
  2. Hệ thống chỉ hiển thị trên web, không gửi email.  

**Postconditions**  
Cảnh báo tồn kho được gửi kịp thời đến người có trách nhiệm.  

---

### UC-086 Giữ chỗ tồn kho (Reserve Stock)
**Primary Actors**  
System  

**Secondary Actors**  
None  

**Description**  
As a System, I want to automatically increment 'Reserved Stock' and decrement 'Available-to-Sell Stock' whenever an order is confirmed so that the same physical unit cannot be sold to another customer.  

**Preconditions**  
1. Đơn hàng online vừa được tạo hoặc nhân viên bấm xác nhận đơn hàng (UC-049).  
2. Số lượng tồn kho khả dụng của biến thể >= Số lượng đặt trong đơn hàng.  

**Normal Flow**  
*Tự động giữ chỗ tồn kho khi đơn hàng được xác nhận*  
1. Hệ thống tiếp nhận sự kiện xác nhận đơn hàng.  
2. Đối với từng dòng sản phẩm trong đơn hàng:  
   - Hệ thống tăng trường `ReservedQuantity` lên thêm một lượng bằng đúng số lượng đặt mua: `ReservedQuantity = ReservedQuantity + OrderItemQuantity`.  
   - Hệ thống tính toán lại `AvailableQuantity = PhysicalQuantity - ReservedQuantity`.  
3. Hệ thống kiểm tra: `AvailableQuantity >= 0`.  
4. Hệ thống cập nhật bảng dữ liệu tồn kho trong cùng một Transaction với đơn hàng.  
5. Hệ thống hoàn tất giữ chỗ thành công; sản phẩm này không còn hiển thị cho khách khác mua với số lượng vượt quá số lượng khả dụng mới.  

**Alternative Flows**  
- **Tồn kho khả dụng không đủ để giữ chỗ (Race condition):**  
  1. Trong tích tắc, khách hàng khác vừa đặt mua hết hàng.  
  2. `AvailableQuantity < 0`.  
  3. Hệ thống chặn đứng giao dịch, rollback trạng thái đơn hàng và thông báo lỗi: "Không đủ tồn kho khả dụng để xác nhận đơn hàng".  

**Postconditions**  
Lượng hàng tương ứng được khóa giữ chỗ an toàn cho đơn hàng mục tiêu.  

---

### UC-087 Hoàn lại tồn kho giữ chỗ (Release Stock)
**Primary Actors**  
System  

**Secondary Actors**  
None  

**Description**  
As a System, I want to automatically decrement 'Reserved Stock' and restore 'Available-to-Sell Stock' whenever an order is cancelled or modified downwards so that held stock is instantly freed for other buyers.  

**Preconditions**  
Đơn hàng đã được xác nhận (đã có giữ chỗ tồn kho) bị hủy bỏ (UC-044, UC-053) hoặc bị giảm số lượng sản phẩm.  

**Normal Flow**  
*Giải phóng tồn kho giữ chỗ khi đơn hàng bị hủy*  
1. Hệ thống tiếp nhận sự kiện hủy đơn hàng hoặc sửa giảm số lượng sản phẩm trong đơn.  
2. Đối với từng sản phẩm bị hủy/giảm trong đơn:  
   - Hệ thống giảm trường `ReservedQuantity`: `ReservedQuantity = ReservedQuantity - CancelledQuantity`.  
   - Hệ thống tính lại `AvailableQuantity = PhysicalQuantity - ReservedQuantity`.  
3. Hệ thống cập nhật lại bảng tồn kho trong cơ sở dữ liệu.  
4. Số lượng vừa được giải phóng ngay lập tức xuất hiện trở lại trên website để các khách hàng khác có thể đặt mua bình thường.  

**Alternative Flows**  
- **Đơn hàng chưa từng giữ chỗ:**  
  1. Đơn hàng bị hủy khi chưa từng được xác nhận giữ kho.  
  2. Hệ thống bỏ qua không cần giải phóng tồn kho.  

**Postconditions**  
Lượng tồn kho giữ chỗ được trả về trạng thái khả dụng cho toàn hệ thống.  

---

### UC-088 Trừ tồn kho thực tế khi xuất hàng (Deduct Stock on Export)
**Primary Actors**  
System  

**Secondary Actors**  
None  

**Description**  
As a System, I want to automatically deduct both 'On-hand Stock' and 'Reserved Stock' when goods are physically handed over/dispatched (Delivery Note completed or Counter Sale delivered) so that physical counts match records.  

**Preconditions**  
Phiếu xuất kho/giao hàng được xác nhận giao thành công (UC-095) hoặc đơn bán tại quầy nhận hàng ngay được hoàn tất (UC-048).  

**Normal Flow**  
*Trừ tồn kho thực tế và giảm giữ chỗ khi hàng rời kho thành công*  
1. Hệ thống tiếp nhận sự kiện giao hàng thành công.  
2. Đối với từng sản phẩm và số lượng đã xuất giao thực tế (`ExportQuantity`):  
   - Hệ thống trừ Tồn thực tế: `PhysicalQuantity = PhysicalQuantity - ExportQuantity`.  
   - Hệ thống trừ Số lượng đang giữ: `ReservedQuantity = ReservedQuantity - ExportQuantity`.  
   - `AvailableQuantity` được giữ nguyên ổn định (vì trước đó đã trừ lúc giữ chỗ).  
3. Nếu sản phẩm theo dõi theo Serial: Hệ thống chuyển trạng thái các mã Serial tương ứng sang `Đã xuất bán (Sold)`.  
4. Hệ thống ghi nhận bản ghi biến động kho (Stock Movement Log - UC-082) với loại `SALE_EXPORT`.  
5. Hệ thống hoàn tất cập nhật tồn kho an toàn trong cơ sở dữ liệu.  

**Alternative Flows**  
- **Lỗi đồng bộ tồn kho:**  
  1. Cơ sở dữ liệu phát hiện bất thường về số lượng.  
  2. Hệ thống ghi log cảnh báo để quản trị viên đối soát ngay lập tức.  

**Postconditions**  
Số lượng tồn kho thực tế của cửa hàng giảm đi chính xác theo lượng hàng đã bàn giao cho khách.  

---

### UC-138 Tạo đề xuất nhập hàng (Create Restock Request)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to submit a formal Restock Request when customer demand exceeds current stock so that warehouse management and the store owner can approve purchasing from suppliers.  

**Preconditions**  
1. Khách hàng muốn mua thiết bị số lượng lớn hoặc sản phẩm đang chạm ngưỡng tồn kho thấp (UC-084).  

**Normal Flow**  
*Lập phiếu đề xuất nhập thêm hàng*  
1. Nhân viên nhấp nút "Tạo đề xuất nhập hàng" (Restock Request).  
2. Hệ thống hiển thị biểu mẫu:  
   - Mã đề xuất: Tự động sinh `YC-NHAP-XXXXX`.  
   - Danh sách mặt hàng cần nhập: Chọn SKU, Tên biến thể, Tồn hiện tại, Số lượng đề xuất nhập thêm, Nhà cung cấp dự kiến, Đơn giá dự kiến.  
   - Mức độ ưu tiên: `Khẩn cấp (Khách công trình đang chờ)`, `Bình thường (Bổ sung tồn định kỳ)`.  
   - Lý do đề xuất (ví dụ: "Khách nhà thầu chốt lấy 10 bộ sen cây Toto trong tuần tới, kho hiện chỉ còn 2 bộ").  
3. Nhân viên nhập các thông tin và nhấp nút "Gửi đề xuất".  
4. Hệ thống lưu phiếu đề xuất ở trạng thái `Chờ phê duyệt (Pending Approval)`.  
5. Hệ thống gửi thông báo nhắc duyệt đến Store Owner.  
6. Hệ thống hiển thị thông báo thành công (MSG173: "Đã gửi đề xuất nhập hàng thành công!").  

**Alternative Flows**  
- **Chủ cửa hàng trực tiếp lập đề xuất:**  
  1. Chủ cửa hàng lập đề xuất.  
  2. Hệ thống cho phép duyệt trực tiếp và chuyển đổi ngay thành Đơn đặt hàng NCC (UC-182).  

**Postconditions**  
Phiếu đề xuất nhập hàng được ghi nhận, sẵn sàng để xem xét phê duyệt.  

---

### UC-139 Yêu cầu soạn hàng tại kho (Request Stock Picking)
**Primary Actors**  
Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, I want to send an immediate stock picking request to warehouse workers for in-store customers waiting to take items home immediately so that goods are picked and inspected without delay.  

**Preconditions**  
Đơn hàng bán tại quầy được tạo lập với tùy chọn "Khách nhận hàng ngay tại quầy" (UC-048).  

**Normal Flow**  
*Gửi lệnh soạn hàng nhanh xuống kho cho khách lấy ngay*  
1. Tại màn hình bán hàng POS, sau khi khách thanh toán/chốt đơn, nhân viên nhấp nút "Gửi yêu cầu soạn hàng tại kho" (Request Picking).  
2. Hệ thống tạo phiếu soạn hàng nhanh với mã `PICK-XXXXX` và gửi thông báo hiển thị trên màn hình máy tính bảng của nhân viên kho.  
3. Phiếu soạn hàng hiển thị: Tên khách hàng đang chờ tại quầy, Danh sách sản phẩm, Vị trí kệ kho (Kệ A1, Kệ B3), Số lượng cần lấy.  
4. Nhân viên kho tiếp nhận, nhặt hàng từ kệ, kiểm tra nguyên vẹn và mang ra quầy giao dịch bàn giao cho nhân viên bán hàng.  
5. Nhân viên kho bấm "Đã soạn xong" trên hệ thống.  
6. Màn hình bán hàng của nhân viên quầy đổi sang trạng thái "Hàng đã sẵn sàng tại quầy".  

**Alternative Flows**  
- **Hàng trên kệ bị móp vỡ khi thủ kho nhặt:**  
  1. Thủ kho phát hiện thùng hàng bị vỡ sứ.  
  2. Thủ kho báo ngay trên hệ thống "Đổi thùng hàng khác" và lấy thùng dự phòng đạt chuẩn.  

**Postconditions**  
Quy trình soạn hàng lấy ngay diễn ra trơn tru, rút ngắn thời gian chờ đợi của khách tại cửa hàng.  

---

### UC-140 Bàn giao hàng đổi trả về kho (Handover Returned Goods to Warehouse)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, I want to generate a return-to-warehouse handover slip when a customer exchanges/returns products at the counter so that items are inspected and restored to inventory.  

**Preconditions**  
Đã có phiếu đổi trả hàng được chấp thuận tại quầy (UC-060).  

**Normal Flow**  
*Bàn giao thiết bị khách đổi trả về khu vực kho*  
1. Nhân viên bán hàng mở phiếu đổi trả hàng đã duyệt và nhấp nút "Bàn giao về kho".  
2. Hệ thống hiển thị biểu mẫu bàn giao:  
   - Danh sách thiết bị khách trả lại: Tên, SKU, Số lượng, Mã Serial (nếu có).  
   - Phân loại tình trạng hàng hóa:  
     + `Hàng nguyên vẹn 100%`: Hộp chưa bóc, đầy đủ phụ kiện -> Nhập lại tồn kho để bán tiếp.  
     + `Hàng lỗi do nhà sản xuất`: Chờ làm thủ tục trả lại NCC (UC-194).  
     + `Hàng trầy xước nhẹ do khách`: Chuyển sang khu vực hàng thanh lý giảm giá.  
   - Người bàn giao (Nhân viên bán hàng) và Người nhận (Thủ kho).  
3. Nhân viên chọn phân loại tình trạng và nhấp "Xác nhận bàn giao về kho".  
4. Thủ kho ký nhận trên hệ thống.  
5. Hệ thống tự động cộng lại số lượng Tồn thực tế vào kho tương ứng với phân loại đã chọn.  
6. Hệ thống hiển thị thông báo thành công (MSG174: "Đã bàn giao hàng đổi trả về kho và cập nhật tồn kho thành công!").  

**Alternative Flows**  
- **Thủ kho phát hiện hàng thiếu phụ kiện:**  
  1. Kiểm đếm phát hiện thiếu dây cấp hoặc nắp bồn cầu.  
  2. Thủ kho từ chối nhận nguyên vẹn, yêu cầu nhân viên bán hàng trừ tiền hoàn của khách theo chính sách đổi trả.  

**Postconditions**  
Hàng đổi trả được kiểm soát chặt chẽ, cập nhật đúng phân loại tồn kho.  

---

### UC-178 Xem chi tiết phiếu nhập kho (View Goods Receipt Detail)
**Primary Actors**  
Store Owner, Accountant, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As an internal user, I want to view the full details of a goods receipt (supplier, products, quantities, prices, receiving notes, status, history) so that purchasing documentation is transparent.  

*(Chi tiết xem tại UC-076 - Chuẩn hóa giao diện xem chi tiết chứng từ nhập kho).*  

---

### UC-179 Tạo phiếu nhập hàng (chọn NCC, biến thể, số lượng, giá nhập) (Create Goods Receipt Draft)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to create a draft goods receipt (specifying supplier, variants, expected quantities, cost prices) before final stock counting so that staff can verify physical goods upon arrival.  

**Preconditions**  
Chủ cửa hàng đã đăng nhập và truy cập màn hình tạo phiếu nhập.  

**Normal Flow**  
*Tạo phiếu nhập hàng ở trạng thái Nháp (Draft)*  
1. Chủ cửa hàng mở biểu mẫu tạo phiếu nhập kho.  
2. Chủ cửa hàng chọn Nhà cung cấp và thêm danh sách các mặt hàng dự kiến nhập từ hóa đơn của hãng.  
3. Chủ cửa hàng nhập Số lượng dự kiến và Đơn giá nhập.  
4. Chủ cửa hàng nhấp nút "Lưu phiếu nháp" (Save as Draft).  
5. Hệ thống lưu phiếu nhập với mã `PN-XXXXX` ở trạng thái `Bản nháp (Draft)`.  
6. Tồn kho thực tế **CHƯA ĐƯỢC CỘNG** vào hệ thống tại bước này.  
7. Hệ thống hiển thị thông báo (MSG175: "Đã lưu bản nháp phiếu nhập kho. Hãy kiểm đếm hàng thực tế và xác nhận hoàn tất để cộng tồn kho").  

**Alternative Flows**  
- **Hủy bỏ tạo nháp:**  
  1. Người dùng bấm Hủy. Hệ thống đóng biểu mẫu không lưu.  

**Postconditions**  
Phiếu nhập nháp được lưu trữ sẵn sàng cho khâu kiểm đếm thực tế (UC-181).  

---

### UC-180 Cập nhật / hủy phiếu nhập (khi còn nháp hoặc chưa xác nhận) (Update/Cancel Draft Receipt)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to update line items or cancel an unconfirmed/draft goods receipt without affecting live inventory so that mistakes are corrected effortlessly.  

**Preconditions**  
Phiếu nhập kho đang ở trạng thái `Bản nháp (Draft)`.  

**Normal Flow**  
*Chỉnh sửa hoặc hủy bỏ phiếu nhập nháp*  
1. Chủ cửa hàng mở chi tiết phiếu nhập đang ở trạng thái `Draft`.  
2. Chủ cửa hàng có thể:  
   - Thêm/bớt mặt hàng, sửa số lượng, sửa đơn giá nhập.  
   - Hoặc nhấp nút "Hủy phiếu nháp".  
3. Nếu chọn Hủy: Hệ thống hiển thị xác nhận và chuyển trạng thái phiếu sang `Đã hủy (Cancelled)` ngay lập tức mà không cần quy trình đảo ngược phức tạp vì tồn kho chưa từng bị tác động.  
4. Hệ thống hiển thị thông báo thành công (MSG176: "Đã hủy phiếu nhập nháp thành công!").  

**Alternative Flows**  
- **Phiếu đã xác nhận hoàn tất:**  
  1. Phiếu đã ở trạng thái Hoàn tất.  
  2. Hệ thống ẩn nút xóa nháp và yêu cầu thực hiện qua quy trình UC-077.  

**Postconditions**  
Phiếu nhập nháp được cập nhật hoặc loại bỏ hoàn toàn khỏi hàng đợi.  

---

### UC-181 Xác nhận / hoàn tất phiếu nhập (lúc này tồn kho mới được cộng) (Confirm/Complete Goods Receipt)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to confirm and finalize a draft goods receipt after physical verification so that inventory balances are officially incremented and purchase journals are posted.  

**Preconditions**  
1. Phiếu nhập kho đang ở trạng thái `Draft` (UC-179).  
2. Hàng hóa thực tế đã được bốc dỡ và kiểm đếm đủ số lượng, quét đủ mã Serial (nếu có).  

**Normal Flow**  
*Xác nhận hoàn tất phiếu nhập kho để chính thức cộng tồn kho*  
1. Chủ cửa hàng mở chi tiết phiếu nhập nháp.  
2. Chủ cửa hàng đối chiếu số lượng thực tế với số lượng trên phiếu, điều chỉnh lại số lượng nếu có chênh lệch thực nhận.  
3. Chủ cửa hàng nhập/quét mã Serial cho các thiết bị bắt buộc Serial (UC-030).  
4. Chủ cửa hàng nhấp nút "Xác nhận & Hoàn tất nhập kho" (Confirm Goods Receipt).  
5. Hệ thống kiểm tra: Các mã Serial đã quét đủ theo số lượng và không trùng lặp.  
6. Hệ thống chuyển trạng thái phiếu nhập từ `Draft` sang `Hoàn tất (Completed)`.  
7. Hệ thống tự động **CỘNG TĂNG TỒN THỰC TẾ** của các biến thể tương ứng trong kho.  
8. Hệ thống kích hoạt ghi sổ Công nợ nhà cung cấp (ACC-F35).  
9. Hệ thống ghi nhận sự kiện vào Audit Log.  
10. Hệ thống hiển thị thông báo thành công (MSG177: "Xác nhận nhập kho thành công! Tồn kho đã được cộng vào hệ thống").  

**Alternative Flows**  
- **Chưa quét đủ mã Serial theo số lượng nhập:**  
  1. Hàng quản lý Serial nhưng chưa quét đủ mã.  
  2. Hệ thống chặn hoàn tất và yêu cầu quét đủ Serial (MSG110).  

**Postconditions**  
1. Phiếu nhập chính thức hoàn tất.  
2. Tồn kho thực tế tăng lên chính xác.  

---

### UC-182 Tạo đơn đặt hàng NCC (Purchase Order) (Create Purchase Order)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Email Service  

**Description**  
As a Store Owner, I want to create and send a Purchase Order (PO) to a supplier before goods arrive so that procurement orders are tracked and pre-scheduled.  

**Preconditions**  
Chủ cửa hàng đã đăng nhập và truy cập phân hệ Đặt hàng NCC.  

**Normal Flow**  
*Lập đơn đặt hàng chính thức gửi nhà cung cấp*  
1. Chủ cửa hàng nhấp nút "Tạo đơn đặt hàng NCC" (Create Purchase Order).  
2. Hệ thống hiển thị biểu mẫu PO:  
   - Mã đơn PO: Tự động sinh `PO-YYYYMMDD-XXXXX`.  
   - Nhà cung cấp mục tiêu.  
   - Danh sách mặt hàng cần đặt: SKU, Tên biến thể, Số lượng đặt, Đơn giá thỏa thuận, Ngày dự kiến giao hàng đến cửa hàng.  
   - Điều khoản giao hàng & Thanh toán (Thanh toán 100% khi nhận hàng hoặc công nợ 30 ngày).  
3. Chủ cửa hàng nhập các thông tin và nhấp nút "Lưu và Gửi PO".  
4. Hệ thống kiểm tra dữ liệu hợp lệ.  
5. Hệ thống lưu PO ở trạng thái `Đã gửi NCC (Sent to Supplier)`.  
6. Hệ thống gọi `Email Service` gửi file PDF bản Đơn đặt hàng đến địa chỉ email của đại diện bán hàng hãng cung cấp.  
7. Hệ thống hiển thị thông báo thành công (MSG178: "Đã tạo và gửi đơn đặt hàng PO cho nhà cung cấp thành công!").  

**Alternative Flows**  
- **Kế thừa từ Phiếu đề xuất nhập hàng (UC-138):**  
  1. Chủ cửa hàng bấm "Tạo PO từ đề xuất". Hệ thống tự động điền danh sách sản phẩm từ đề xuất đã duyệt.  

**Postconditions**  
Đơn đặt hàng PO được lưu trữ, sẵn sàng để đối soát khi hàng về (UC-075).  

---

### UC-183 Điều chỉnh tồn kho thủ công (kèm lý do: hỏng, mất, thất thoát) (Manual Stock Adjustment with Reason)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to manually adjust stock levels for specific items (due to damage, shrinkage, display sample loss) with mandatory reason logging so that discrepancies are resolved immediately without full inventory counts.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập với vai trò `Store Owner`.  
2. Phát hiện sự cố hàng hóa cục bộ (ví dụ: vỡ 1 bồn cầu khi dọn kho).  

**Normal Flow**  
*Điều chỉnh số lượng tồn kho thủ công đột xuất*  
1. Chủ cửa hàng truy cập chi tiết biến thể sản phẩm, nhấp nút "Điều chỉnh tồn kho" (Manual Adjust).  
2. Hệ thống hiển thị hộp thoại:  
   - Tên biến thể, SKU, Tồn thực tế hiện tại.  
   - Chọn loại điều chỉnh: `Tăng số lượng (+)` hoặc `Giảm số lượng (-)`.  
   - Ô nhập "Số lượng điều chỉnh" (ví dụ: giảm 1 cái).  
   - Dropdown chọn "Lý do điều chỉnh": `Hàng vỡ/hỏng trong kho`, `Thất thoát/mất mát`, `Hàng xuất làm mẫu trưng bày (Showroom Sample)`, `Kiểm đếm bù trừ`.  
   - Ô nhập "Mô tả chi tiết lý do" (bắt buộc, tối thiểu 10 ký tự).  
3. Chủ cửa hàng chọn Giảm 1 cái, chọn lý do "Hàng vỡ/hỏng trong kho" và nhập mô tả "Vỡ két nước do va chạm xe nâng".  
4. Chủ cửa hàng nhấp nút "Xác nhận điều chỉnh".  
5. Hệ thống kiểm tra: Lý do hợp lệ, số lượng điều chỉnh giảm không vượt quá tồn thực tế hiện có.  
6. Hệ thống cập nhật số tồn thực tế mới trong cơ sở dữ liệu.  
7. Hệ thống ghi nhận biến động kho và Audit Log chi tiết (FE-02).  
8. Hệ thống hiển thị thông báo thành công (MSG179: "Điều chỉnh tồn kho thủ công thành công!").  

**Alternative Flows**  
- **Không nhập mô tả lý do:**  
  1. Bỏ trống mô tả. Hệ thống chặn lưu (MSG40).  
- **Điều chỉnh giảm vượt quá tồn hiện có:**  
  1. Tồn hiện tại là 0 nhưng chọn giảm 1.  
  2. Hệ thống báo lỗi (MSG180: "Không thể điều chỉnh giảm vượt quá số lượng tồn kho thực tế hiện tại").  

**Postconditions**  
Số lượng tồn kho được cập nhật chính xác kèm lý do kiểm toán minh bạch.  

---

### UC-184 Tìm kiếm / lọc tồn kho (Search/Filter Inventory)
**Primary Actors**  
Store Owner, Sales Staff, Accountant  

*(Chi tiết xem tại UC-074 và UC-168 - Bộ công cụ tìm kiếm và lọc tồn kho theo SKU, tên, danh mục và trạng thái còn/hết hàng).*  

---

### UC-185 Tìm kiếm / lọc lịch sử nhập (Search/Filter Import History)
**Primary Actors**  
Store Owner, Accountant  

*(Chi tiết xem tại UC-078 - Bộ lọc nâng cao lịch sử các đợt nhập hàng theo NCC, ngày và trạng thái).*  

---

### UC-186 Xem chi tiết tồn kho của một biến thể (View Variant Stock Detail)
**Primary Actors**  
Store Owner, Sales Staff, Technical Staff  

**Secondary Actors**  
None  

**Description**  
As an internal user, I want to view detailed inventory metrics of a specific variant (on-hand, reserved, available, incoming POs, damaged, serial list) so that I have absolute clarity on stock availability.  

**Preconditions**  
Người dùng nhấp vào một biến thể từ danh sách tồn kho (UC-074).  

**Normal Flow**  
*Xem hồ sơ chi tiết tồn kho của một biến thể hàng hóa*  
1. Hệ thống hiển thị trang "Chi tiết tồn kho - [Mã SKU]":  
   - Thẻ số: Tồn thực tế, Đang giữ chỗ cho đơn, Có thể bán ngay, Số lượng đang trên đường về từ đơn PO.  
   - Bảng danh sách các mã Serial đang nằm trong kho (nếu có theo dõi Serial): Mã Serial, Ngày nhập kho, Vị trí kệ, Trạng thái (Mới 100%).  
   - Danh sách các đơn hàng đang giữ chỗ mặt hàng này (Mã đơn hàng, Tên khách hàng, Số lượng giữ).  
   - Bảng Thẻ kho tóm tắt 10 giao dịch xuất nhập gần nhất.  
2. Người dùng xem thông tin đối soát.  

**Alternative Flows**  
- **Không có hàng trong kho:**  
  1. Tồn bằng 0. Hệ thống hiển thị gợi ý tạo đơn nhập hàng.  

**Postconditions**  
Toàn cảnh dữ liệu tồn kho của biến thể được cung cấp chuẩn xác.  

---

### UC-187 Tạo NCC (Create Supplier)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to create a new supplier profile with business info, tax ID, contacts, address, and payment terms so that the store can purchase items from this vendor.  

**Preconditions**  
Chủ cửa hàng đang ở màn hình Quản lý nhà cung cấp (UC-079).  

**Normal Flow**  
*Tạo mới hồ sơ nhà cung cấp*  
1. Chủ cửa hàng nhấp nút "Thêm nhà cung cấp mới" (Add Supplier).  
2. Hệ thống hiển thị biểu mẫu:  
   - Tên nhà cung cấp (ví dụ: "Công ty TNHH TOTO Việt Nam - Chi nhánh Hà Nội").  
   - Mã NCC (ví dụ: `NCC-TOTO-HN`).  
   - Mã số thuế doanh nghiệp (10 hoặc 13 chữ số).  
   - Người liên hệ / Quản lý kinh doanh (Họ tên, SĐT, Email).  
   - Địa chỉ văn phòng & Địa chỉ kho giao nhận.  
   - Tài khoản ngân hàng (Số TK, Ngân hàng, Tên thụ hưởng).  
   - Điều khoản công nợ mặc định: Số ngày được nợ (ví dụ: 30 ngày), Hạn mức công nợ tối đa (ví dụ: 500.000.000 VNĐ).  
3. Chủ cửa hàng nhập các thông tin và nhấp nút "Lưu nhà cung cấp".  
4. Hệ thống kiểm tra: Tên và Mã NCC không trùng lặp, Mã số thuế và SĐT đúng định dạng.  
5. Hệ thống lưu bản ghi NCC vào cơ sở dữ liệu ở trạng thái `Active`.  
6. Hệ thống hiển thị thông báo thành công (MSG181: "Tạo nhà cung cấp mới thành công!").  

**Alternative Flows**  
- **Mã NCC hoặc Mã số thuế bị trùng:**  
  1. Mã số thuế đã thuộc về một NCC khác trên hệ thống.  
  2. Hệ thống báo lỗi (MSG182: "Mã số thuế hoặc Mã NCC này đã tồn tại trên hệ thống").  

**Postconditions**  
Nhà cung cấp mới sẵn sàng để chọn trong các phiếu nhập kho và đơn PO.  

---

### UC-188 Cập nhật NCC (Update Supplier)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to edit supplier contact info, bank accounts, or credit terms so that vendor records remain accurate.  

**Preconditions**  
Chủ cửa hàng mở chi tiết một nhà cung cấp hiện có.  

**Normal Flow**  
*Chỉnh sửa thông tin nhà cung cấp*  
1. Chủ cửa hàng nhấp nút "Sửa" tại dòng nhà cung cấp cần cập nhật.  
2. Hệ thống mở biểu mẫu với dữ liệu hiện tại.  
3. Chủ cửa hàng cập nhật thông tin: SĐT người liên hệ mới, Đổi số tài khoản ngân hàng, Nâng hạn mức công nợ.  
4. Chủ cửa hàng nhấp nút "Lưu thay đổi".  
5. Hệ thống kiểm tra dữ liệu hợp lệ và lưu cập nhật vào cơ sở dữ liệu.  
6. Hệ thống ghi nhận sự kiện vào Audit Log.  
7. Hệ thống hiển thị thông báo thành công (MSG183: "Cập nhật thông tin nhà cung cấp thành công!").  

**Alternative Flows**  
- **Hủy bỏ thay đổi:**  
  1. Người dùng bấm Hủy. Hệ thống giữ nguyên thông tin cũ.  

**Postconditions**  
Hồ sơ nhà cung cấp được cập nhật chính xác.  

---

### UC-189 Xem danh sách NCC (View Supplier List)
**Primary Actors**  
Store Owner, Accountant  

*(Chi tiết xem tại UC-079 - Xem và tìm kiếm danh sách các nhà cung cấp).*  

---

### UC-190 Xem chi tiết NCC (kèm lịch sử nhập từ NCC đó) (View Supplier Detail)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to view detailed supplier profiles along with historical goods receipts and outstanding debt ledgers so that supplier relationships and payables are evaluated.  

**Preconditions**  
Người dùng nhấp vào một nhà cung cấp từ danh sách (UC-079).  

**Normal Flow**  
*Xem hồ sơ chi tiết và lịch sử giao dịch với nhà cung cấp*  
1. Hệ thống hiển thị trang "Chi tiết nhà cung cấp - [Tên NCC]":  
   - Khối Thông tin doanh nghiệp: Tên, Mã số thuế, Người liên hệ, SĐT, Email, Địa chỉ, Tài khoản ngân hàng.  
   - Khối Chỉ số tài chính: Tổng giá trị hàng đã nhập từ trước đến nay, Tổng số tiền đã thanh toán, Dư nợ hiện tại còn phải trả, Hạn mức công nợ cho phép.  
   - Tab 1: "Lịch sử các đợt nhập hàng" (Danh sách tất cả các phiếu nhập kho từ NCC này kèm trạng thái).  
   - Tab 2: "Sổ công nợ & Lịch sử thanh toán" (Các phiếu chi tiền, ngày thanh toán, số tiền trả nợ).  
   - Tab 3: "Danh mục sản phẩm cung cấp" (Các mặt hàng cửa hàng thường xuyên nhập từ NCC này).  
2. Người dùng kiểm tra thông tin để lên kế hoạch thanh toán hoặc đàm phán chiết khấu lô mới.  

**Alternative Flows**  
- **Nhà cung cấp mới chưa phát sinh giao dịch:**  
  1. Các tab lịch sử hiển thị thông báo "Chưa có giao dịch nhập hàng nào".  

**Postconditions**  
Bức tranh hợp tác toàn diện với nhà cung cấp được hiển thị rõ ràng.  

---

### UC-191 Khóa/mở khóa NCC (Lock/Unlock Supplier)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to lock or unlock a supplier so that staff cannot create new POs or goods receipts from inactive or problematic vendors.  

**Preconditions**  
Chủ cửa hàng đang ở danh sách nhà cung cấp.  

**Normal Flow**  
*Khóa hoặc mở khóa quyền giao dịch của nhà cung cấp*  
1. Tại dòng nhà cung cấp cần khóa, Chủ cửa hàng gạt công tắc Trạng thái sang `Locked`.  
2. Hệ thống hiển thị hộp thoại xác nhận: "Bạn có chắc muốn tạm dừng hợp tác / khóa nhà cung cấp [Tên NCC]? Khi bị khóa, nhân viên sẽ không thể tạo đơn đặt hàng mới từ nhà cung cấp này".  
3. Chủ cửa hàng nhập lý do khóa (ví dụ: "NCC thường xuyên giao hàng trễ hẹn và tỷ lệ hàng lỗi cao") và nhấp "Xác nhận khóa".  
4. Hệ thống cập nhật trạng thái `IsActive = False` trong cơ sở dữ liệu.  
5. Hệ thống ghi nhận thao tác vào Audit Log.  
6. Hệ thống hiển thị thông báo thành công (MSG184: "Đã khóa nhà cung cấp thành công!"). Nhà cung cấp này bị ẩn khỏi danh sách lựa chọn tạo phiếu nhập mới.  

**Alternative Flows**  
- **Mở khóa nhà cung cấp:**  
  1. Gạt công tắc từ Locked sang Active.  
  2. Hệ thống mở khóa và cho phép tạo giao dịch nhập hàng trở lại.  

**Postconditions**  
Trạng thái hoạt động của nhà cung cấp được cập nhật chính xác.  

---

### UC-192 Export/ Import kho (Export/Import Inventory Data)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to export full inventory data to Excel and batch-import stock initialization files so that stock audits and data migrations are executed efficiently.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản trị kho.  

**Normal Flow**  
*Xuất và nhập dữ liệu tồn kho hàng loạt qua Excel*  
1. **Xuất dữ liệu kho (Export):**  
   - Người dùng nhấp nút "Xuất file tồn kho Excel".  
   - Hệ thống kết xuất file `Ton_Kho_[YYYYMMDD].xlsx` chứa đầy đủ: SKU, Tên biến thể, Tồn thực tế, Tồn giữ chỗ, Tồn khả dụng, Giá vốn, Thành tiền giá trị tồn.  
   - File tự động tải về máy tính.  
2. **Nhập dữ liệu kho khởi tạo (Import):**  
   - Người dùng nhấp nút "Nhập tồn kho từ Excel".  
   - Người dùng tải lên file Excel đã điền số lượng kiểm kê ban đầu.  
   - Hệ thống kiểm tra hợp lệ dữ liệu, hiển thị kết quả đối soát trước.  
   - Người dùng bấm "Xác nhận Import".  
   - Hệ thống cập nhật số tồn kho hàng loạt và ghi nhận Audit Log.  
3. Hệ thống hiển thị thông báo thành công (MSG185: "Xử lý dữ liệu tồn kho Excel thành công!").  

**Alternative Flows**  
- **File Import bị lỗi dữ liệu:**  
  1. Có dòng sai mã SKU hoặc số lượng không phải số nguyên.  
  2. Hệ thống từ chối tệp và báo lỗi chi tiết từng dòng (MSG159).  

**Postconditions**  
Dữ liệu tồn kho được sao lưu hoặc cập nhật hàng loạt an toàn.  

---

### UC-193 Xem danh sách phiếu nhập (View Goods Receipt List)
**Primary Actors**  
Store Owner, Accountant  

*(Chi tiết xem tại UC-078 - Danh sách các phiếu nhập kho cùng bộ lọc tra cứu).*  

---

### UC-194 Trả hàng NCC (Return Goods to Supplier)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to record goods returned to a supplier (for defective batches or unsold consignment returns), deduct inventory, and reduce accounts payable so that vendor balances remain accurate.  

**Preconditions**  
1. Có hàng lỗi từ nhà máy hoặc thỏa thuận trả lại hàng tồn cho nhà phân phối.  
2. Hàng hóa còn tồn thực tế trong kho.  

**Normal Flow**  
*Lập phiếu xuất trả hàng cho nhà cung cấp*  
1. Người dùng nhấp nút "Tạo phiếu xuất trả hàng NCC" (Return to Supplier).  
2. Hệ thống hiển thị biểu mẫu:  
   - Mã phiếu trả: Tự động sinh `TH-NCC-YYYYMMDD-XXXXX`.  
   - Chọn Nhà cung cấp nhận lại hàng.  
   - Danh sách mặt hàng trả lại: Chọn SKU, Tên sản phẩm, Số lượng trả, Đơn giá nhập ban đầu, Lý do trả (Hàng lỗi men sứ, Hàng lỗi van xả, Hàng thừa theo hợp đồng).  
   - Nếu có quản lý Serial: Quét danh sách mã Serial của các thiết bị xuất trả.  
3. Người dùng nhập đầy đủ thông tin và nhấp nút "Xác nhận xuất trả hàng".  
4. Hệ thống kiểm tra: Số lượng trả <= Số lượng tồn thực tế hiện có trong kho.  
5. Hệ thống trừ số lượng Tồn thực tế trong kho tương ứng.  
6. Hệ thống tự động giảm trừ số tiền công nợ phải trả cho nhà cung cấp đó trong sổ kế toán (liên kết ACC-F38).  
7. Hệ thống ghi nhận sự kiện vào Audit Log.  
8. Hệ thống hiển thị thông báo thành công (MSG186: "Lập phiếu xuất trả hàng NCC thành công! Tồn kho và công nợ đã được giảm trừ").  

**Alternative Flows**  
- **Số lượng trả lớn hơn tồn kho thực tế:**  
  1. Người dùng nhập số lượng trả vượt quá hàng hiện có.  
  2. Hệ thống chặn lưu và thông báo lỗi (MSG187: "Số lượng xuất trả không được lớn hơn số lượng tồn kho thực tế hiện có").  

**Postconditions**  
1. Hàng hóa được trừ khỏi kho.  
2. Công nợ với nhà cung cấp được khấu trừ chính xác.  

---
