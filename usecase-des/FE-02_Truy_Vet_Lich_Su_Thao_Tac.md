# PHÂN HỆ FE-02: TRUY VẾT LỊCH SỬ THAO TÁC (AUDIT LOG)

---

### UC-016 Ghi nhật ký hệ thống (Record Audit Log)
**Primary Actors**  
Store Owner, Sales Staff, Technical Staff, Shipper, Accountant  

**Secondary Actors**  
None  

**Description**  
As a System, I want to automatically capture and record critical user actions (such as price updates, stock adjustments, order modifications, order cancellations, and role assignments) with full metadata and state diffs so that all sensitive operational changes are transparently auditable.  

**Preconditions**  
1. Một người dùng thuộc một trong các vai trò nội bộ đã đăng nhập thành công.  
2. Người dùng thực hiện một hành động làm thay đổi trạng thái dữ liệu thuộc danh mục nghiệp vụ trọng yếu (Đổi giá niêm yết, điều chỉnh số lượng tồn kho, duyệt/hủy/sửa đơn hàng, phân quyền tài khoản, nhập/xuất kho).  

**Normal Flow**  
*Ghi nhận tự động bản ghi Audit Log nền khi có hành động nghiệp vụ*  
1. Người dùng thực hiện thao tác gửi yêu cầu (HTTP Request) thực hiện nghiệp vụ trọng yếu (ví dụ: Thay đổi giá bán biến thể sản phẩm tại UC-024).  
2. Hệ thống xử lý nghiệp vụ chính thành công trong cơ sở dữ liệu (Transaction Commit).  
3. Bộ chặn sự kiện kiểm toán (Audit Interceptor / Middleware) của hệ thống tự động trích xuất các thông tin ngữ cảnh:  
   - `UserID` và `Username` của người thực hiện.  
   - `Role` của người thực hiện tại thời điểm đó.  
   - Địa chỉ `IP Address` và thông tin trình duyệt `User-Agent`.  
   - Dấu thời gian chính xác theo chuẩn UTC/GMT+7 (`Timestamp`).  
   - Loại hành vi (`ActionType`: CREATE, UPDATE, DELETE, CANCEL, ADJUST).  
   - Thực thể bị tác động (`EntityName` và `EntityID`: ví dụ `ProductVariant` - SKU1029).  
   - Dữ liệu trước khi thay đổi (`OldValue` dạng JSON).  
   - Dữ liệu sau khi thay đổi (`NewValue` dạng JSON).  
   - Lý do thay đổi nghiệp vụ do người dùng nhập từ giao diện (`Reason`).  
4. Hệ thống tạo bản ghi mới và ghi vào bảng nhật ký bất biến `AuditLogs` trong cơ sở dữ liệu.  
5. Hệ thống hoàn tất việc lưu vết mà không làm gián đoạn hoặc suy giảm thời gian phản hồi của yêu cầu chính.  

**Alternative Flows**  
- **Step 4.1_Ghi nhật ký bất đồng bộ gặp sự cố (Fail-safe):**  
  1. Hàng đợi ghi Audit Log hoặc dịch vụ lưu trữ gặp sự cố kết nối cơ sở dữ liệu tạm thời.  
  2. Hệ thống ghi bản ghi kiểm toán vào tệp nhật ký dự phòng cục bộ (Local Fallback Log File) trên máy chủ để tiến hành đồng bộ lại tự động ngay khi cơ sở dữ liệu phục hồi.  
  3. Tiến trình nghiệp vụ của người dùng không bị gián đoạn.  
- **Step 1.1_Hành động nghiệp vụ bị từ chối hoặc lỗi:**  
  1. Yêu cầu của người dùng vi phạm kiểm tra nghiệp vụ hoặc bị từ chối do không đủ quyền hạn.  
  2. Nghiệp vụ bị hủy bỏ (Rollback).  
  3. Hệ thống ghi nhận sự kiện vi phạm an ninh / truy cập trái phép vào Security Log riêng biệt, không ghi nhận là thao tác thay đổi dữ liệu thành công trong Audit Log.  

**Postconditions**  
1. Bản ghi nhật ký kiểm toán mới được lưu trữ vĩnh viễn và không thể bị chỉnh sửa hay xóa bởi bất kỳ người dùng nào.  
2. Bản ghi sẵn sàng để tra cứu tại màn hình danh sách nhật ký hệ thống (UC-017).  

---

### UC-017 Xem danh sách nhật ký hệ thống (View Audit Log List)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to view, search, and filter the system audit logs by actor, action type, affected entity, and date range so that I can trace operational incidents and hold staff accountable.  

**Preconditions**  
Chủ cửa hàng đã đăng nhập thành công và có quyền truy cập vào chức năng "Nhật ký kiểm toán hệ thống".  

**Normal Flow**  
*Xem và lọc danh sách nhật ký hệ thống*  
1. Chủ cửa hàng nhấp chọn mục "Nhật ký hệ thống" (Audit Log) trên thanh điều hướng quản trị.  
2. Hệ thống tải và hiển thị giao diện danh sách nhật ký kiểm toán:  
   - Thanh công cụ bộ lọc: Lọc theo người thực hiện (Dropdown chọn nhân viên), Lọc theo phân hệ/thực thể (Sản phẩm, Kho, Đơn hàng, Tài khoản), Lọc theo loại thao tác (Thêm mới, Cập nhật, Hủy bỏ, Điều chỉnh), Lọc theo khoảng ngày (Từ ngày - Đến ngày).  
   - Bảng dữ liệu: Cột Thời gian, Cột Người thực hiện (Tên & Vai trò), Cột Hành động, Cột Đối tượng tác động (Entity & ID), Cột Tóm tắt thay đổi, Cột Lý do, Cột Thao tác ("Xem chi tiết").  
   - Thanh phân trang dữ liệu (Mặc định 20 bản ghi/trang, sắp xếp theo thời gian mới nhất giảm dần).  
3. Chủ cửa hàng nhập hoặc chọn các tiêu chí lọc (ví dụ: Lọc tất cả thao tác "Điều chỉnh tồn kho" của nhân viên "Nguyễn Văn A" trong tháng hiện tại).  
4. Chủ cửa hàng nhấp nút "Tìm kiếm/Áp dụng bộ lọc".  
5. Hệ thống truy vấn và tải lại bảng danh sách với các bản ghi nhật ký phù hợp.  
6. Chủ cửa hàng có thể duyệt danh sách và nhấp nút "Xem chi tiết" tại bất kỳ dòng nào để chuyển sang xem chi tiết bản ghi (UC-018).  

**Alternative Flows**  
- **Step 4.1_Khoảng thời gian lọc không hợp lệ:**  
  1. Ngày bắt đầu lớn hơn ngày kết thúc.  
  2. Hệ thống hiển thị thông báo lỗi (MSG61: "Khoảng thời gian không hợp lệ. Ngày bắt đầu phải nhỏ hơn hoặc bằng ngày kết thúc").  
  3. Hệ thống giữ nguyên bảng dữ liệu hiện tại để người dùng chọn lại ngày.  
- **Step 5.1_Không tìm thấy bản ghi phù hợp:**  
  1. Không có dữ liệu kiểm toán nào thỏa mãn điều kiện lọc.  
  2. Hệ thống hiển thị thông báo (MSG62: "Không có bản ghi nhật ký nào thỏa mãn điều kiện tìm kiếm") kèm nút "Xóa bộ lọc".  
  3. Người dùng nhấp "Xóa bộ lọc" để hiển thị lại toàn bộ nhật ký.  

**Postconditions**  
Danh sách các sự kiện kiểm toán được hiển thị chính xác theo tiêu chí tìm kiếm của Store Owner.  

---

### UC-018 Xem chi tiết bản ghi nhật ký (View Audit Log Detail)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to view the granular details of a specific audit log record (including before-and-after data comparisons and the operator's stated reason) so that I can understand the exact nature and rationale of the change.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập vào hệ thống.  
2. Chủ cửa hàng đang ở màn hình Danh sách nhật ký kiểm toán (UC-017).  

**Normal Flow**  
*Xem chi tiết so sánh dữ liệu trước và sau khi thay đổi*  
1. Tại một dòng nhật ký trên bảng danh sách, Chủ cửa hàng nhấp vào nút "Xem chi tiết" (Icon chi tiết).  
2. Hệ thống hiển thị cửa sổ hộp thoại mở rộng (Modal) hoặc trang chi tiết "Chi tiết bản ghi kiểm toán - [Log ID]".  
3. Hệ thống hiển thị các thông tin:  
   - Thông tin chung: Mã bản ghi log, Thời gian thực hiện chính xác, Người thực hiện (Mã NV, Họ tên, Vai trò), Địa chỉ IP, Thiết bị thực hiện.  
   - Thông tin nghiệp vụ: Phân hệ, Hành động, Đối tượng và Khóa chính đối tượng (ví dụ: Biến thể SP mã SKU-TOTO-001).  
   - Lý do thao tác: Nội dung lý do do nhân viên nhập vào khi thực hiện hành động.  
   - Bảng so sánh dữ liệu trực quan (Side-by-side Diff View):  
     + Cột Tên trường dữ liệu (Field Name).  
     + Cột Giá trị cũ (Old Value - hiển thị nền đỏ nhạt).  
     + Cột Giá trị mới (New Value - hiển thị nền xanh nhạt).  
     + Các trường không đổi được thu gọn hoặc hiển thị màu trung tính.  
4. Chủ cửa hàng xem đối soát toàn bộ chi tiết biến động của dữ liệu.  
5. Chủ cửa hàng nhấp nút "Đóng" để quay lại danh sách nhật ký.  

**Alternative Flows**  
- **Step 2.1_Bản ghi thuộc thao tác Tạo mới (CREATE):**  
  1. Thao tác tạo mới không có giá trị cũ.  
  2. Bảng so sánh chỉ hiển thị Cột Giá trị mới (New Value), Cột Giá trị cũ được hiển thị nhãn "N/A (Tạo mới)".  
- **Step 2.2_Bản ghi thuộc thao tác Xóa (DELETE):**  
  1. Thao tác xóa không có giá trị mới sau thay đổi.  
  2. Bảng so sánh hiển thị toàn bộ giá trị đã bị xóa ở Cột Giá trị cũ, Cột Giá trị mới hiển thị nhãn "N/A (Đã xóa)".  
- **Step 2.3_Bản ghi không tồn tại hoặc lỗi tải dữ liệu JSON:**  
  1. Mã log ID không hợp lệ hoặc dữ liệu JSON bị lỗi cú pháp.  
  2. Hệ thống hiển thị thông báo lỗi (MSG63: "Không thể tải chi tiết bản ghi nhật ký này") và tự động đóng modal.  

**Postconditions**  
Chủ cửa hàng nắm bắt được toàn bộ chi tiết sai khác dữ liệu trước và sau khi thay đổi cùng lý do của nhân viên.  

---
