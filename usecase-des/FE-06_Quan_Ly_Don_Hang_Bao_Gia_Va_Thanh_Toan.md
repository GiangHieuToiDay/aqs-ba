# PHÂN HỆ FE-06: QUẢN LÝ ĐƠN HÀNG, BÁO GIÁ CÔNG TRÌNH & THANH TOÁN (ORDERS, QUOTES & POS)

---

### UC-046 Xem danh sách đơn hàng (View Order List)
**Primary Actors**  
Sales Staff, Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, Store Owner, or Accountant, I want to search and filter orders across all channels (Counter, Online, Project Contracts) by status, channel, date, customer, and payment state so that order fulfillment is coordinated smoothly.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý bán hàng.  

**Normal Flow**  
*Xem và lọc danh sách đơn hàng đa kênh*  
1. Người dùng chọn mục "Quản lý đơn hàng" trên menu.  
2. Hệ thống tải và hiển thị trang danh sách đơn hàng dạng bảng lưới:  
   - Thanh công cụ bộ lọc: Ô tìm kiếm từ khóa (Mã đơn, Tên khách, SĐT), Lọc theo Kênh bán (`Tất cả`, `Tại quầy`, `Website`, `Báo giá công trình`), Lọc theo Trạng thái đơn (`Chờ xác nhận`, `Đã xác nhận`, `Chuẩn bị hàng`, `Đang giao`, `Hoàn tất`, `Đã hủy`), Lọc theo Trạng thái thanh toán (`Chưa thanh toán`, `Thanh toán 1 phần`, `Đã thanh toán đủ`), Lọc theo Khoảng ngày tạo đơn.  
   - Bảng dữ liệu: Mã đơn hàng (ví dụ: `ORD-20261008-0012`), Ngày tạo, Kênh bán, Khách hàng & SĐT, Tổng giá trị đơn hàng, Số tiền đã trả, Dư nợ còn lại, Trạng thái đơn (Huy hiệu màu), Trạng thái thanh toán, Thao tác ("Xem chi tiết", "In hóa đơn", "Cập nhật").  
   - Phân trang dữ liệu (Mặc định 20 đơn/trang, sắp xếp theo thời gian mới nhất).  
3. Người dùng chọn các tiêu chí lọc để tìm kiếm đơn hàng cần thao tác.  
4. Người dùng nhấp vào một dòng đơn hàng để xem chi tiết (UC-047).  

**Alternative Flows**  
- **Không tìm thấy đơn hàng:**  
  1. Tiêu chí lọc không trả về kết quả nào.  
  2. Hệ thống hiển thị thông báo "Không tìm thấy đơn hàng nào phù hợp" kèm nút "Đặt lại bộ lọc".  

**Postconditions**  
Danh sách đơn hàng được hiển thị chính xác theo quyền hạn và tiêu chí lọc.  

---

### UC-047 Xem chi tiết đơn hàng (View Order Detail)
**Primary Actors**  
Sales Staff, Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As an internal user, I want to view comprehensive details of an order (line items, payment history, debt balances, delivery batches, status timeline, and staff audit log) so that I have complete operational visibility.  

**Preconditions**  
Người dùng nhấp vào một đơn hàng từ danh sách đơn hàng (UC-046).  

**Normal Flow**  
*Xem đầy đủ thông tin chi tiết của một đơn hàng*  
1. Hệ thống điều hướng đến trang "Chi tiết đơn hàng - [Mã đơn]":  
   - Khối 1: Thông tin khách hàng & Địa chỉ giao hàng (Tên, SĐT, Địa chỉ công trình, Ghi chú giao hàng).  
   - Khối 2: Trạng thái quy trình (Thanh timeline tiến độ: Đặt hàng -> Xác nhận -> Xuất kho -> Giao hàng -> Hoàn tất).  
   - Khối 3: Danh sách sản phẩm chi tiết: Ảnh, Tên thiết bị, SKU, Số lượng mua, Đơn giá, Chiết khấu từng dòng, Thành tiền, Danh sách Serial đã gán (nếu có).  
   - Khối 4: Tổng kết tài chính: Tổng tiền hàng, Chiết khấu đơn hàng/Voucher, Phí vận chuyển, Tổng tiền thanh toán, Số tiền khách đã trả (kèm danh sách các đợt thanh toán), Dư nợ còn lại, Hạn nợ.  
   - Khối 5: Tiến độ giao hàng theo đợt (Liên kết FE-08: Bảng ma trận số lượng đã giao và còn lại).  
   - Khối 6: Nhật ký lịch sử thay đổi (Audit trail: ai tạo đơn, ai duyệt, ai sửa giá, ai hủy).  
2. Người dùng xem thông tin và thực hiện các tác vụ quản lý tương ứng (In hóa đơn, Ghi nhận tiền, Lập phiếu giao).  

**Alternative Flows**  
- **Không tìm thấy đơn hàng:**  
  1. Mã đơn không tồn tại hoặc bị xóa. Hệ thống báo lỗi và quay lại danh sách.  

**Postconditions**  
Toàn cảnh thông tin đơn hàng được cung cấp đầy đủ, chính xác.  

---

### UC-048 Lập đơn hàng tại quầy (Create Counter Order)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
Cloudinary, Email Service  

**Description**  
As a Sales Staff or Store Owner, I want to create a direct Counter Sale (POS) order (selecting/creating customer, checking live stock, adding variants, applying counter discounts, choosing immediate take-away or scheduled delivery) so that in-store sales are processed quickly.  

**Preconditions**  
1. Nhân viên đã đăng nhập và đang mở ca bán hàng (UC-149).  
2. Khách hàng đang có mặt tại quầy showroom hoặc chốt đơn qua hotline.  

**Normal Flow**  
*Lập đơn bán hàng tại quầy trực tiếp*  
1. Nhân viên mở giao diện Bán hàng tại quầy (POS Screen).  
2. Nhân viên tìm kiếm và chọn Khách hàng (bằng Số điện thoại hoặc Tên; nếu khách mới thì bấm "Thêm nhanh khách hàng" - UC-133).  
3. Nhân viên thêm sản phẩm vào giỏ hàng quầy:  
   - Sử dụng máy quét mã vạch quét mã trên hộp sản phẩm, hoặc gõ tên/SKU vào ô tìm kiếm nhanh.  
   - Hệ thống hiển thị tồn kho thực tế và tồn khả dụng tức thì ngay dưới tên sản phẩm.  
   - Điều chỉnh số lượng mua.  
4. Nhân viên chọn hình thức nhận hàng:  
   - Lựa chọn 1: `Khách lấy hàng ngay tại quầy` (Kích hoạt yêu cầu soạn hàng - UC-139; trừ tồn thực tế ngay khi hoàn tất - UC-088).  
   - Lựa chọn 2: `Cửa hàng giao tận nhà / công trình sau` (Kích hoạt giữ chỗ tồn kho - UC-086; tạo đơn chờ xuất kho - UC-089).  
5. Nhân viên áp dụng chính sách chiết khấu (nếu có khách thợ/nhà thầu - UC-144) hoặc nhập mã voucher giảm giá.  
6. Nhân viên chọn phương thức thanh toán: `Tiền mặt tại quầy`, `Chuyển khoản VietQR` (UC-041), `Thanh toán một phần / Mua nợ` (UC-054).  
7. Nhân viên nhấp nút "Thanh toán & Hoàn tất đơn hàng".  
8. Hệ thống kiểm tra: Tồn kho khả dụng đủ, Số tiền thanh toán hợp lệ.  
9. Hệ thống lưu đơn hàng mới với Kênh bán = `Counter`, sinh mã `ORD-POS-YYYYMMDD-XXXXX`.  
10. Hệ thống cập nhật trừ kho / giữ chỗ theo hình thức nhận hàng đã chọn.  
11. Hệ thống in hóa đơn bán hàng tức thì qua máy in nhiệt (UC-063).  
12. Hệ thống hiển thị thông báo thành công (MSG188: "Tạo đơn hàng tại quầy thành công!").  

**Alternative Flows**  
- **Sản phẩm hết hàng khả dụng trong kho:**  
  1. Số lượng mua vượt quá số hàng có thể bán.  
  2. Hệ thống cảnh báo đỏ (MSG72: "Không đủ tồn kho khả dụng").  
  3. Nhân viên tư vấn khách chuyển sang hình thức Đặt cọc giữ hàng chờ nhập (UC-145).  
- **Khách không lấy hóa đơn giấy:**  
  1. Khách muốn nhận hóa đơn điện tử qua Zalo/Email.  
  2. Nhân viên bấm gửi hóa đơn điện tử tới email/SĐT khách.  

**Postconditions**  
1. Đơn hàng tại quầy được tạo lập và lưu trữ.  
2. Tồn kho và két tiền bán hàng được cập nhật chuẩn xác.  

---

### UC-049 Xác nhận đơn hàng online (Confirm Online Order)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
SMS Service, Email Service  

**Description**  
As a Sales Staff or Store Owner, I want to review customer details and confirm an online order placed via website so that inventory reservation is verified and the warehouse is notified to prepare goods.  

**Preconditions**  
1. Đơn hàng online đang ở trạng thái `Chờ xác nhận (Unconfirmed)` (từ UC-040).  
2. Nhân viên đã gọi điện thoại cho khách để xác nhận địa chỉ và thời gian nhận hàng.  

**Normal Flow**  
*Xác nhận đơn hàng trực tuyến của khách hàng*  
1. Nhân viên mở chi tiết đơn hàng online cần duyệt.  
2. Nhân viên kiểm tra tính chính xác của địa chỉ giao hàng, thông tin liên hệ và phương thức thanh toán (COD hoặc đã chuyển khoản VietQR).  
3. Nhân viên nhấp nút "Xác nhận đơn hàng" (Confirm Order).  
4. Hệ thống kiểm tra điều kiện an toàn: Tồn kho giữ chỗ của các mặt hàng trong đơn vẫn hợp lệ và khả dụng.  
5. Hệ thống chuyển trạng thái đơn hàng từ `Unconfirmed` sang `Đã xác nhận (Confirmed)`.  
6. Hệ thống kích hoạt tạo tác vụ Đơn chờ xuất kho (UC-089) cho bộ phận kho.  
7. Hệ thống tự động gửi tin nhắn SMS/Email thông báo cho khách hàng: "Đơn hàng [Mã đơn] của bạn đã được xác nhận thành công và đang được chuẩn bị đóng gói!".  
8. Hệ thống ghi nhận sự kiện vào Audit Log.  
9. Hệ thống hiển thị thông báo thành công (MSG189: "Xác nhận đơn hàng thành công!").  

**Alternative Flows**  
- **Khách hàng yêu cầu hủy khi nhân viên gọi điện xác nhận:**  
  1. Khách báo đổi ý không mua nữa.  
  2. Nhân viên chuyển sang chức năng Từ chối/Hủy đơn hàng online (UC-050).  

**Postconditions**  
Đơn hàng chính thức chuyển sang giai đoạn chuẩn bị hàng và xuất kho.  

---

### UC-050 Từ chối đơn hàng online (Reject Online Order)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
Email Service, SMS Service  

**Description**  
As a Sales Staff or Store Owner, I want to reject an online order (due to out-of-stock items, fraudulent customer, unreachable phone, or delivery outside serviceable radius) with mandatory reason entry so that held inventory is released.  

**Preconditions**  
Đơn hàng online đang ở trạng thái `Unconfirmed`.  

**Normal Flow**  
*Từ chối đơn hàng online và giải phóng tồn kho*  
1. Tại chi tiết đơn hàng online, nhân viên nhấp nút "Từ chối đơn hàng" (Reject Order).  
2. Hệ thống hiển thị hộp thoại yêu cầu:  
   - Chọn lý do từ chối (Dropdown): `Không liên lạc được khách hàng (gọi 3 lần)`, `Hết hàng trong kho không thể cung cấp`, `Địa chỉ giao hàng ngoài vùng phục vụ`, `Nghi vấn đơn hàng ảo/gian lận`, `Khách hàng yêu cầu hủy khi gọi xác nhận`.  
   - Ô nhập mô tả chi tiết lý do (bắt buộc, tối thiểu 10 ký tự).  
3. Nhân viên chọn lý do, nhập mô tả và nhấp "Xác nhận từ chối".  
4. Hệ thống kiểm tra: Lý do không để trống.  
5. Hệ thống chuyển trạng thái đơn hàng sang `Đã từ chối / Đã hủy (Cancelled)`.  
6. Hệ thống tự động giải phóng toàn bộ số lượng tồn kho đã giữ chỗ cho đơn này (UC-087).  
7. Hệ thống gửi email thông báo giải thích lý do từ chối đơn hàng cho khách hàng.  
8. Hệ thống ghi nhận sự kiện vào Audit Log.  
9. Hệ thống hiển thị thông báo thành công (MSG190: "Đã từ chối đơn hàng và hoàn lại tồn kho thành công!").  

**Alternative Flows**  
- **Không nhập lý do từ chối:**  
  1. Bỏ trống ô mô tả lý do.  
  2. Hệ thống báo lỗi (MSG40: "Bắt buộc nhập lý do từ chối đơn hàng").  

**Postconditions**  
Đơn hàng bị đóng lại, hàng hóa giữ chỗ được trả về kho cho khách khác mua.  

---

### UC-051 Cập nhật trạng thái đơn hàng (Update Order Status)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
SMS Service  

**Description**  
As a Sales Staff or Store Owner, I want to advance order status through its lifecycle (Confirmed -> Processing -> Shipping -> Delivered -> Completed) in accordance with physical fulfillment.  

**Preconditions**  
Đơn hàng đang ở trạng thái có thể chuyển tiếp theo quy trình máy trạng thái (State Machine).  

**Normal Flow**  
*Cập nhật trạng thái tiến độ thực hiện đơn hàng*  
1. Người dùng mở chi tiết đơn hàng (UC-047).  
2. Người dùng nhấp vào nút chuyển trạng thái phù hợp (ví dụ: Chuyển từ `Chuẩn bị hàng` sang `Đang giao hàng` khi xe tải bắt đầu chạy).  
3. Hệ thống kiểm tra điều kiện chuyển đổi: Các điều kiện về phiếu xuất kho và phân công vận chuyển đã thỏa mãn.  
4. Hệ thống cập nhật trạng thái mới cho đơn hàng kèm dấu thời gian thực hiện.  
5. Hệ thống ghi nhật ký cập nhật vào lịch sử đơn hàng.  
6. Hệ thống thông báo thành công (MSG191: "Cập nhật trạng thái đơn hàng thành công!").  

**Alternative Flows**  
- **Chuyển trạng thái trái quy trình:**  
  1. Cố tình chuyển từ Chờ xác nhận thẳng sang Đã hoàn tất mà chưa qua giao hàng.  
  2. Hệ thống ngăn chặn thao tác và cảnh báo vi phạm quy trình nghiệp vụ.  

**Postconditions**  
Trạng thái đơn hàng được cập nhật đồng bộ trên hệ thống và ứng dụng khách hàng.  

---

### UC-052 Chỉnh sửa thông tin đơn hàng (Edit Order)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to edit line items, quantities, shipping addresses, or discounts of an unfulfilled order with mandatory reason logging so that customer changes are accommodated before dispatch.  

**Preconditions**  
1. Đơn hàng chưa xuất kho giao hàng (trước trạng thái `Shipping`).  
2. Người dùng có quyền chỉnh sửa đơn hàng.  

**Normal Flow**  
*Chỉnh sửa nội dung đơn hàng và ghi nhận lý do kiểm toán*  
1. Tại chi tiết đơn hàng, người dùng nhấp nút "Chỉnh sửa đơn hàng" (Edit Order).  
2. Hệ thống mở giao diện chỉnh sửa: Cho phép sửa danh sách sản phẩm, tăng/giảm số lượng, đổi biến thể, sửa địa chỉ giao hàng, hoặc điều chỉnh chiết khấu.  
3. Người dùng thực hiện các thay đổi (ví dụ: khách đổi từ bồn cầu mã MS885 sang mã MS688 và đổi địa chỉ giao sang tầng 3).  
4. Người dùng nhấp nút "Lưu thay đổi đơn hàng".  
5. Hệ thống mở popup yêu cầu: Bắt buộc nhập "Lý do chỉnh sửa đơn hàng".  
6. Người dùng nhập lý do (ví dụ: "Khách gọi hotline yêu cầu đổi mẫu bồn cầu và đổi địa chỉ nhận hàng") và nhấp "Xác nhận lưu".  
7. Hệ thống kiểm tra lại tồn kho cho các sản phẩm mới thêm/đổi: Đủ hàng khả dụng.  
8. Hệ thống tính toán lại tổng tiền đơn hàng, điều chỉnh lượng tồn kho giữ chỗ tương ứng (tăng hoặc giảm giữ chỗ).  
9. Hệ thống ghi nhận toàn bộ sai khác trước/sau vào Audit Log (UC-016).  
10. Hệ thống hiển thị thông báo thành công (MSG192: "Chỉnh sửa đơn hàng thành công! Dữ liệu kiểm toán đã được ghi nhận").  

**Alternative Flows**  
- **Sản phẩm đổi sang bị thiếu hàng:**  
  1. Mẫu bồn cầu mới khách muốn đổi không đủ tồn kho khả dụng.  
  2. Hệ thống cảnh báo thiếu hàng và không cho phép lưu thay đổi.  

**Postconditions**  
Nội dung đơn hàng, công nợ và tồn kho giữ chỗ được điều chỉnh đồng bộ.  

---

### UC-053 Hủy đơn hàng (Cancel Order)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to cancel an active order with mandatory cancellation reason entry so that all reserved stock is immediately released and payment refund workflows are initiated if prepaid.  

**Preconditions**  
Đơn hàng đang ở trạng thái chưa hoàn tất (chưa ở trạng thái `Completed`).  

**Normal Flow**  
*Hủy đơn hàng và tự động giải phóng tồn kho giữ chỗ*  
1. Người dùng mở chi tiết đơn hàng và nhấp nút "Hủy đơn hàng" (Cancel Order).  
2. Hệ thống hiển thị hộp thoại xác nhận hủy đơn:  
   - Cảnh báo: "Khi hủy đơn hàng [Mã đơn], toàn bộ hàng hóa đang giữ chỗ sẽ được giải phóng ngay lập tức".  
   - Bắt buộc nhập "Lý do hủy đơn hàng" (ví dụ: "Khách công trình hủy hợp đồng thi công").  
3. Người dùng nhập lý do và nhấp "Xác nhận hủy".  
4. Hệ thống chuyển trạng thái đơn hàng sang `Đã hủy (Cancelled)`.  
5. Hệ thống tự động kích hoạt chức năng giải phóng tồn kho giữ chỗ (UC-087).  
6. Nếu đơn hàng đã có khoản tiền đặt cọc/thanh toán trước: Hệ thống tạo một khoản phải hoàn tiền và nhắc người dùng thực hiện Ghi nhận hoàn tiền (UC-061).  
7. Hệ thống ghi nhận sự kiện vào Audit Log.  
8. Hệ thống hiển thị thông báo thành công (MSG193: "Đơn hàng đã được hủy thành công! Tồn kho đã được giải phóng").  

**Alternative Flows**  
- **Không nhập lý do hủy đơn:**  
  1. Người dùng để trống ô lý do. Hệ thống chặn lưu (MSG40).  

**Postconditions**  
Đơn hàng bị hủy bỏ hoàn toàn, hàng hóa được hoàn trả về tồn kho có thể bán.  

---

### UC-054 Ghi nhận thanh toán một phần (Record Partial Payment)
**Primary Actors**  
Sales Staff, Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, Store Owner, or Accountant, I want to record a partial payment or deposit for an order (specifying amount, payment method, payment due date) so that remaining balances and receivables are tracked.  

**Preconditions**  
1. Đơn hàng đang có dư nợ (Số tiền đã thanh toán < Tổng giá trị đơn hàng).  
2. Khách hàng đặt cọc trước một phần hoặc thanh toán theo đợt công trình.  

**Normal Flow**  
*Ghi nhận đợt thanh toán cọc hoặc trả góp tiền hàng*  
1. Tại chi tiết đơn hàng, người dùng nhấp nút "Ghi nhận thanh toán" (Record Payment).  
2. Hệ thống hiển thị biểu mẫu thanh toán:  
   - Tổng tiền đơn hàng: `50.000.000 VNĐ`.  
   - Đã thanh toán trước đó: `0 VNĐ`.  
   - Số tiền còn nợ: `50.000.000 VNĐ`.  
   - Ô nhập "Số tiền thanh toán đợt này" (ví dụ khách cọc `15.000.000 VNĐ`).  
   - Phương thức thanh toán: `Tiền mặt`, `Chuyển khoản VietQR`, `Thẻ POS`.  
   - Ô nhập "Hạn thanh toán số tiền còn lại" (Due Date: ví dụ sau 15 ngày).  
   - Ô nhập Ghi chú đợt thanh toán (ví dụ: "Khách đặt cọc 30% giữ hàng công trình").  
3. Người dùng nhập số tiền cọc và chọn phương thức thanh toán.  
4. Người dùng nhấp nút "Xác nhận ghi nhận tiền".  
5. Hệ thống kiểm tra: Số tiền thanh toán > 0 và < Số tiền còn nợ.  
6. Hệ thống tạo phiếu thu tiền mới trong sổ quỹ (liên kết ACC-F10).  
7. Hệ thống cập nhật trạng thái thanh toán của đơn hàng sang `Thanh toán một phần (Partially Paid)`.  
8. Hệ thống lưu số dư nợ còn lại (`RemainingAmount = 35.000.000 VNĐ`) và ngày đến hạn nợ.  
9. Hệ thống ghi vết vào Audit Log và in biên lai thu tiền cọc cho khách.  
10. Hệ thống hiển thị thông báo thành công (MSG194: "Ghi nhận thanh toán một phần thành công! Dư nợ còn lại: 35.000.000 VNĐ").  

**Alternative Flows**  
- **Khách thanh toán bằng hoặc vượt số tiền nợ:**  
  1. Người dùng nhập số tiền >= Số tiền nợ.  
  2. Hệ thống tự động chuyển sang luồng Ghi nhận thanh toán đủ (UC-055).  

**Postconditions**  
1. Khoản thanh toán một phần được ghi vào quỹ.  
2. Đơn hàng ghi nhận dư nợ chính xác và theo dõi hạn nợ.  

---

### UC-055 Ghi nhận thanh toán đủ (Record Full Payment)
**Primary Actors**  
Sales Staff, Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, Store Owner, or Accountant, I want to record full payment settling the entire order balance so that the order is marked as Fully Paid.  

**Preconditions**  
Khách hàng thanh toán toàn bộ giá trị đơn hàng hoặc thanh toán nốt phần dư nợ còn lại.  

**Normal Flow**  
*Ghi nhận thanh toán tất toán 100% đơn hàng*  
1. Người dùng nhấp nút "Thanh toán toàn bộ" trên đơn hàng.  
2. Hệ thống tự động điền số tiền bằng đúng số dư nợ còn lại của đơn hàng.  
3. Người dùng chọn phương thức thanh toán (`Tiền mặt` hoặc `Chuyển khoản`) và nhấp "Xác nhận tất toán".  
4. Hệ thống kiểm tra số tiền khớp đúng nghĩa vụ thanh toán.  
5. Hệ thống tạo phiếu thu tất toán vào sổ quỹ.  
6. Hệ thống cập nhật trạng thái thanh toán của đơn hàng sang `Đã thanh toán đủ (Fully Paid)`.  
7. Dư nợ còn lại của đơn hàng trở về 0 VNĐ.  
8. Hệ thống hiển thị thông báo thành công (MSG195: "Đơn hàng đã được thanh toán toàn bộ 100%!").  

**Alternative Flows**  
- **Hủy thao tác tất toán:**  
  1. Người dùng bấm Hủy. Hệ thống giữ nguyên trạng thái nợ cũ.  

**Postconditions**  
Đơn hàng được tất toán hoàn toàn về mặt tài chính.  

---

### UC-056 Xác nhận tiền chuyển khoản (Confirm Bank Transfer Payment)
**Primary Actors**  
Sales Staff, Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As an internal staff member, I want to verify bank transfer notifications against our bank account statement and confirm the payment so that pending VietQR orders become verified.  

**Preconditions**  
Đơn hàng online hoặc đơn tại quầy có giao dịch chuyển khoản VietQR đang ở trạng thái `Chờ xác nhận tiền (Pending Verification)` (từ UC-041).  

**Normal Flow**  
*Đối soát và xác nhận tiền chuyển khoản đã về tài khoản ngân hàng*  
1. Người dùng mở mục "Xác nhận chuyển khoản ngân hàng" hoặc mở trực tiếp đơn hàng đang chờ xác nhận tiền.  
2. Hệ thống hiển thị thông tin đối soát:  
   - Mã đơn hàng, Tên khách hàng.  
   - Số tiền cần chuyển: ví dụ `18.500.000 VNĐ`.  
   - Mã nội dung chuyển khoản chuẩn: `DH ORD2026100801 0987654321`.  
   - Ảnh chụp biên lai chuyển tiền do khách tải lên (nếu có).  
3. Người dùng kiểm tra ứng dụng ngân hàng / tin nhắn biến động số dư SMS Banking của cửa hàng: Xác nhận đã nhận được đúng số tiền và nội dung.  
4. Người dùng nhấp nút "Xác nhận đã nhận đủ tiền" (Confirm Payment Received).  
5. Hệ thống chuyển trạng thái thanh toán của đơn hàng sang `Đã thanh toán đủ (Fully Paid)`.  
6. Hệ thống tự động tạo phiếu thu tiền gửi ngân hàng vào sổ quỹ (ACC-F10).  
7. Hệ thống gửi thông báo cảm ơn và xác nhận thanh toán thành công tới điện thoại/email của khách hàng.  
8. Hệ thống hiển thị thông báo thành công (MSG196: "Đã xác nhận tiền chuyển khoản thành công!").  

**Alternative Flows**  
- **Khách chuyển thiếu tiền:**  
  1. Số tiền nhận thực tế nhỏ hơn số tiền đơn hàng (ví dụ: chuyển 10 triệu thay vì 18.5 triệu).  
  2. Người dùng nhấp "Xác nhận chuyển thiếu", nhập số tiền thực nhận.  
  3. Hệ thống ghi nhận là Thanh toán một phần (UC-054) và lưu lại dư nợ còn thiếu.  
- **Chưa nhận được tiền trong tài khoản:**  
  1. Tài khoản ngân hàng chưa báo có (nghẽn mạng Napas hoặc khách chưa chuyển thật).  
  2. Người dùng giữ đơn ở trạng thái chờ và liên hệ lại với khách để đối chiếu mã giao dịch ngân hàng.  

**Postconditions**  
Trạng thái thanh toán điện tử của đơn hàng được xác thực chính xác.  

---

### UC-057 Thu hồi công nợ khách hàng (Collect Outstanding Debt)
**Primary Actors**  
Sales Staff, Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As an internal staff member, I want to record debt collection payments from a debtor customer and allocate the received money across their open invoices so that outstanding balances are reduced.  

**Preconditions**  
Khách hàng (chủ thầu/thợ hoặc khách lẻ) đang có số dư nợ trên hệ thống (UC-058).  

**Normal Flow**  
*Thu nợ khách hàng và giảm trừ công nợ lũy kế*  
1. Người dùng mở hồ sơ công nợ của khách hàng (hoặc truy cập mục Thu hồi công nợ).  
2. Hệ thống hiển thị tổng dư nợ hiện tại của khách (ví dụ: `45.000.000 VNĐ`) và danh sách các đơn hàng còn nợ.  
3. Khách hàng thanh toán tiền nợ (ví dụ: trả trước `30.000.000 VNĐ`).  
4. Người dùng nhấp nút "Thu hồi nợ" (Collect Debt).  
5. Người dùng nhập: Số tiền thu (`30.000.000 VNĐ`), Phương thức thanh toán (Tiền mặt/Chuyển khoản), Chọn cơ chế phân bổ:  
   - `Tự động phân bổ theo đơn nợ cũ nhất trước` (FIFO - First In First Out).  
   - Hoặc `Chỉ định gạch nợ cho đơn hàng cụ thể`.  
6. Người dùng nhấp nút "Xác nhận thu nợ".  
7. Hệ thống tạo phiếu thu nợ, giảm trừ số dư nợ của khách hàng xuống còn `15.000.000 VNĐ`.  
8. Các đơn hàng được gạch nợ tự động chuyển trạng thái thanh toán tương ứng.  
9. Hệ thống ghi vết vào Sổ công nợ và in biên lai thu nợ cho khách.  
10. Hệ thống hiển thị thông báo thành công (MSG197: "Thu nợ thành công! Dư nợ mới của khách hàng là 15.000.000 VNĐ").  

**Alternative Flows**  
- **Khách trả thừa tiền so với tổng nợ:**  
  1. Số tiền trả lớn hơn tổng số nợ hiện có.  
  2. Hệ thống kích hoạt xử lý tiền trả thừa (ACC-F41).  

**Postconditions**  
Số dư công nợ của khách hàng được giảm trừ chính xác theo số tiền thực thu.  

---

### UC-058 Xem danh sách công nợ khách hàng (View Debt List)
**Primary Actors**  
Accountant, Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As an Accountant, Store Owner, or Sales Staff, I want to view an aggregated summary of all customers with outstanding debts, their total balances, and nearest payment due dates so that credit risks are controlled.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý.  

**Normal Flow**  
*Xem danh sách tổng hợp công nợ khách hàng*  
1. Người dùng chọn mục "Danh sách công nợ khách hàng" trên menu Tài chính.  
2. Hệ thống hiển thị bảng tổng hợp công nợ:  
   - Cột: Tên khách hàng / Nhà thầu, Số điện thoại, Nhóm khách hàng, Số lượng đơn hàng đang nợ, Tổng dư nợ hiện tại (in đỏ), Ngày nợ gần nhất, Ngày đến hạn thanh toán gần nhất, Trạng thái tuổi nợ (Huy hiệu xanh: Trong hạn; Huy hiệu đỏ: Quá hạn), Nút "Thu nợ", Nút "Nhắc nợ" (UC-147).  
   - Thống kê tóm tắt trên đầu trang: Tổng công nợ toàn bộ khách hàng phải trả cho cửa hàng; Số tiền nợ đã quá hạn.  
3. Người dùng lọc theo nhóm "Nợ quá hạn" để ưu tiên xử lý thu hồi dòng tiền.  

**Alternative Flows**  
- **Không có khách hàng nào nợ:**  
  1. Tất cả đơn hàng đã được tất toán 100%.  
  2. Bảng hiển thị thông báo: "Cửa hàng hiện không có công nợ khách hàng tồn đọng!".  

**Postconditions**  
Bức tranh công nợ phải thu của toàn bộ khách hàng được hiển thị rõ ràng.  

---

### UC-059 Cảnh báo công nợ quá hạn (Receive Overdue Debt Alert)
**Primary Actors**  
Accountant, Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As an internal user, I want to receive proactive system notifications when a customer's debt balance reaches its due date or becomes overdue so that credit terms are promptly enforced.  

**Preconditions**  
Một hoặc nhiều khoản nợ đơn hàng vượt quá ngày hẹn thanh toán (Due Date < Ngày hiện tại).  

**Normal Flow**  
*Hệ thống tự động phát cảnh báo nợ quá hạn trên giao diện quản trị*  
1. Hệ thống chạy tiến trình quét hạn nợ định kỳ vào đầu mỗi ngày làm việc.  
2. Phát hiện các khách hàng có khoản nợ quá hạn thanh toán.  
3. Hệ thống sinh cảnh báo màu đỏ trên thanh Thông báo (Notification Center) và khối Cảnh báo vận hành Dashboard (UC-123):  
   - "Cảnh báo nợ quá hạn: Khách hàng [Tên khách] có khoản nợ [X triệu] quá hạn [Y] ngày!".  
4. Khi nhân viên bán hàng mở đơn hàng mới cho khách hàng này tại quầy POS, hệ thống hiển thị ngay một Popup cảnh báo:  
   "Khách hàng này hiện đang có khoản nợ quá hạn [X triệu] chưa thanh toán. Bạn có chắc chắn muốn tiếp tục cho mua công nợ?".  
5. Nhân viên phải xin ý kiến Store Owner nếu muốn tiếp tục duyệt bán nợ tiếp.  

**Alternative Flows**  
- **Khách hàng đã thanh toán xong:**  
  1. Sau khi khách trả nợ, cảnh báo tự động biến mất ngay lập tức.  

**Postconditions**  
Cảnh báo nợ quá hạn được truyền đạt kịp thời, chặn đứng nguy cơ phát sinh thêm nợ xấu.  

---

### UC-060 Xử lý đổi trả hàng (Process Return/Exchange)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to process a customer's return or exchange request at the counter (verifying return policy, item condition, calculating refund or difference amount) so that inventory and ledgers are adjusted.  

**Preconditions**  
1. Khách hàng mang sản phẩm đã mua đến cửa hàng yêu cầu đổi mẫu khác hoặc trả hàng lấy lại tiền.  
2. Đơn hàng gốc đã mua trong vòng 7 ngày (hoặc theo chính sách đổi trả của cửa hàng).  

**Normal Flow**  
*Lập phiếu xử lý đổi trả thiết bị vệ sinh*  
1. Nhân viên tìm đơn hàng gốc đã mua bằng SĐT hoặc Mã đơn.  
2. Nhân viên nhấp nút "Tạo yêu cầu Đổi / Trả hàng" (Create Return/Exchange).  
3. Hệ thống hiển thị biểu mẫu đổi trả:  
   - Danh sách mặt hàng đã mua trong đơn gốc: Nhân viên tích chọn mặt hàng khách muốn trả, nhập số lượng trả, chọn lý do trả (Đổi sang mẫu màu khác, Mua nhầm kích thước tâm xả, Lỗi kỹ thuật nhà sản xuất).  
   - Kiểm tra điều kiện ngoại quan: Hộp nguyên đai nguyên kiện, đủ phụ kiện đi kèm.  
   - Lựa chọn hình thức:  
     + `Đổi hàng (Exchange)`: Chọn sản phẩm/biến thể mới khách muốn lấy thay thế.  
     + `Trả hàng hoàn tiền (Return & Refund)`: Khách trả hàng và lấy lại tiền.  
4. Nhân viên chọn đổi sang bồn cầu mới có giá trị cao hơn.  
5. Hệ thống tính toán chênh lệch tài chính:  
   - Giá trị hàng trả lại: `8.000.000 VNĐ`.  
   - Giá trị hàng mới lấy: `10.500.000 VNĐ`.  
   - Tiền khách cần đóng thêm: `+2.500.000 VNĐ`.  
6. Khách hàng thanh toán số tiền chênh lệch 2.500.000 VNĐ.  
7. Nhân viên nhấp nút "Hoàn tất đổi hàng".  
8. Hệ thống tạo phiếu đổi trả hàng `DT-YYYYMMDD-XXXXX`.  
9. Hệ thống kích hoạt chức năng bàn giao hàng trả về kho (UC-140) và trừ kho hàng mới xuất.  
10. Hệ thống in phiếu đổi trả hàng giao cho khách.  
11. Hệ thống hiển thị thông báo thành công (MSG198: "Xử lý đổi trả hàng thành công!").  

**Alternative Flows**  
- **Khách đổi sang sản phẩm giá thấp hơn hoặc Trả hàng hoàn tiền:**  
  1. Giá trị hàng trả lớn hơn hàng lấy (hoặc trả hàng hoàn toàn).  
  2. Hệ thống tính ra số tiền cửa hàng cần hoàn lại cho khách.  
  3. Kích hoạt chức năng Ghi nhận hoàn tiền (UC-061).  
- **Sản phẩm bị trầy xước không đủ điều kiện đổi trả:**  
  1. Hàng đã lắp đặt có vết xi măng bẩn hoặc sứ bị mẻ.  
  2. Nhân viên từ chối tiếp nhận đổi trả theo chính sách và giải thích cho khách hàng.  

**Postconditions**  
1. Giao dịch đổi trả được ghi nhận vào hệ thống.  
2. Hàng hóa trong kho và tài chính két tiền được cân bằng chính xác.  

---

### UC-061 Ghi nhận hoàn tiền (Record Refund)
**Primary Actors**  
Sales Staff, Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As an internal staff member, I want to record an outbound refund payment to a customer resulting from an order cancellation or return so that the refunded cash or bank outflow is documented.  

**Preconditions**  
Có một giao dịch hủy đơn hàng có cọc (UC-053) hoặc phiếu đổi trả hàng (UC-060) phát sinh số tiền phải trả lại cho khách.  

**Normal Flow**  
*Ghi nhận chi tiền hoàn trả cho khách hàng*  
1. Người dùng mở phiếu đổi trả/hủy đơn có trạng thái "Chờ hoàn tiền".  
2. Hệ thống hiển thị số tiền cần hoàn lại cho khách (ví dụ: `3.000.000 VNĐ`).  
3. Người dùng nhấp nút "Thực hiện hoàn tiền" (Process Refund).  
4. Người dùng chọn phương thức hoàn tiền:  
   - `Hoàn tiền mặt từ két bán hàng tại quầy`.  
   - `Chuyển khoản trả lại vào tài khoản ngân hàng của khách` (Nhập STK, Tên ngân hàng, Tên người nhận).  
5. Người dùng nhập nội dung hoàn tiền và nhấp "Xác nhận chi tiền hoàn".  
6. Hệ thống tạo phiếu chi tiền hoàn lại trong sổ quỹ (ACC-F16).  
7. Số dư két tiền hoặc tài khoản ngân hàng của cửa hàng giảm đi tương ứng.  
8. Hệ thống chuyển trạng thái phiếu sang `Đã hoàn tiền (Refunded)`.  
9. Hệ thống in phiếu chi hoàn tiền và đưa khách hàng ký nhận.  
10. Hệ thống hiển thị thông báo thành công (MSG199: "Ghi nhận hoàn tiền thành công!").  

**Alternative Flows**  
- **Két tiền tại quầy không đủ tiền mặt để hoàn:**  
  1. Tiền mặt trong két ca trực nhỏ hơn số tiền cần hoàn.  
  2. Hệ thống cảnh báo thiếu tiền mặt trong két và gợi ý chọn phương thức chuyển khoản ngân hàng.  

**Postconditions**  
Số tiền hoàn lại được hạch toán minh bạch trong hệ thống tài chính cửa hàng.  

---

### UC-062 Xử lý yêu cầu hủy/thay đổi đơn (Handle Cancellation Request)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
Email Service  

**Description**  
As a Sales Staff or Store Owner, I want to review customer cancellation or modification requests for confirmed orders and approve or reject them based on actual logistics picking progress.  

**Preconditions**  
Đơn hàng đang có cờ `HasPendingChangeRequest = True` (yêu cầu do khách gửi từ UC-045).  

**Normal Flow**  
*Tiếp nhận và xử lý yêu cầu thay đổi/hủy đơn từ khách hàng*  
1. Nhân viên mở chi tiết đơn hàng đang có cảnh báo yêu cầu từ khách.  
2. Hệ thống hiển thị nội dung yêu cầu của khách: Loại yêu cầu (Hủy đơn hoàn toàn / Đổi địa chỉ / Đổi mặt hàng) và Lý do khách viết.  
3. Nhân viên kiểm tra tình trạng thực tế của đơn hàng tại kho:  
   - Hàng chưa bốc lên xe tải -> Có thể chấp thuận yêu cầu.  
4. Nhân viên nhấp nút "Chấp thuận yêu cầu" (Approve Request).  
5. Hệ thống kích hoạt quy trình tương ứng:  
   - Nếu là yêu cầu hủy: Chuyển sang hủy đơn và giải phóng tồn kho (UC-053).  
   - Nếu là yêu cầu đổi địa chỉ/đổi món: Mở giao diện Chỉnh sửa đơn hàng (UC-052).  
6. Hệ thống gỡ bỏ cờ cảnh báo `HasPendingChangeRequest` trên đơn hàng.  
7. Hệ thống tự động gửi email thông báo kết quả chấp thuận cho khách hàng.  
8. Hệ thống hiển thị thông báo thành công (MSG200: "Đã xử lý yêu cầu thay đổi đơn hàng thành công!").  

**Alternative Flows**  
- **Từ chối yêu cầu do hàng đã trên đường đi giao:**  
  1. Hàng đã được Shipper chở đi giao trên đường.  
  2. Nhân viên nhấp "Từ chối yêu cầu", nhập lý do: "Hàng đã xuất kho và đang trên đường giao, vui lòng nhận hàng hoặc hỗ trợ trực tiếp với tài xế".  
  3. Hệ thống gửi thông báo từ chối cho khách hàng và tiếp tục hành trình giao hàng.  

**Postconditions**  
Yêu cầu của khách hàng được xử lý thỏa đáng theo tiến độ vận hành thực tế.  

---

### UC-063 In hóa đơn bán hàng (Print Invoice)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to print physical sales invoices/receipts for counter orders or online orders with store branding, VAT details, and return terms.  

**Preconditions**  
Đơn hàng đã được tạo lập thành công trên hệ thống.  

**Normal Flow**  
*In hóa đơn bán hàng ra máy in nhiệt hoặc máy in A4/A5*  
1. Người dùng nhấp nút "In hóa đơn" (Print Invoice) trên màn hình chi tiết đơn hàng hoặc màn hình POS bán hàng.  
2. Hệ thống mở cửa sổ xem trước bản in hóa đơn theo mẫu chuẩn:  
   - Thông tin cửa hàng: Logo, Tên cửa hàng, Địa chỉ, Hotline, Mã số thuế, Tài khoản ngân hàng VietQR.  
   - Tiêu đề: HÓA ĐƠN BÁN HÀNG KIÊM PHIẾU BẢO HÀNH.  
   - Mã đơn hàng (kèm mã vạch Barcode để quét tra cứu nhanh).  
   - Thông tin khách hàng: Tên, SĐT, Địa chỉ.  
   - Bảng kê sản phẩm: Tên thiết bị, Biến thể, ĐVT, Số lượng, Đơn giá, Thành tiền.  
   - Tổng kết: Tổng tiền hàng, Giảm giá/Chiết khấu, Phí ship, Tổng thanh toán, Đã trả, Còn nợ.  
   - Chính sách đổi trả và bảo hành tóm tắt dưới chân hóa đơn.  
3. Người dùng chọn khổ in: Máy in nhiệt K80 (80mm cuộn giấy nhỏ) hoặc Máy in khổ A4/A5.  
4. Người dùng bấm lệnh "In".  
5. Hóa đơn được in ra đưa cho khách hàng.  

**Alternative Flows**  
- **Xuất hóa đơn ra file PDF:**  
  1. Khách hàng yêu cầu gửi file hóa đơn điện tử.  
  2. Người dùng nhấp "Tải file PDF". Hệ thống tạo tệp PDF hóa đơn lưu về máy tính.  

**Postconditions**  
Hóa đơn bán hàng vật lý được in ra phục vụ đối soát và lưu giữ chứng từ.  

---

### UC-064 Cấu hình cửa hàng & TK thanh toán (Configure Store Info)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to configure store master data (store name, branch address, hotline, tax ID, store logo) and the official beneficiary bank account used to generate dynamic VietQR payment codes.  

**Preconditions**  
Chủ cửa hàng đã đăng nhập với vai trò `Store Owner` và vào mục Cài đặt hệ thống.  

**Normal Flow**  
*Thiết lập thông tin cửa hàng và tài khoản ngân hàng nhận tiền VietQR*  
1. Chủ cửa hàng truy cập mục "Cài đặt cửa hàng & Tài khoản thanh toán".  
2. Hệ thống hiển thị biểu mẫu thiết lập:  
   - Tên thương hiệu cửa hàng (ví dụ: "Thiết Bị Vệ Sinh Cao Cấp AQS").  
   - Địa chỉ showroom & Kho hàng chính.  
   - Số điện thoại Hotline & Chăm sóc khách hàng.  
   - Mã số thuế công ty / Hộ kinh doanh.  
   - Logo cửa hàng (Tải ảnh lên).  
   - Cấu hình tài khoản ngân hàng sinh mã VietQR:  
     + Chọn Ngân hàng thụ hưởng (Dropdown danh sách hơn 50 ngân hàng NAPAS: Vietcombank, MB Bank, Techcombank, BIDV...).  
     + Số tài khoản ngân hàng chính thức.  
     + Tên chủ tài khoản (In hoa không dấu, ví dụ: `NGUYEN VAN A`).  
3. Chủ cửa hàng nhập thông tin và tải lên logo.  
4. Chủ cửa hàng nhấp nút "Kiểm tra kết nối VietQR". Hệ thống gọi API kiểm tra số tài khoản và hiển thị mã QR mẫu thử nghiệm.  
5. Chủ cửa hàng nhấp nút "Lưu cấu hình".  
6. Hệ thống lưu cấu hình vào cơ sở dữ liệu.  
7. Kể từ thời điểm này, toàn bộ các mã VietQR sinh ra trên website và tại quầy thu ngân (UC-041) sẽ tự động liên kết với tài khoản ngân hàng mới này.  
8. Hệ thống hiển thị thông báo thành công (MSG201: "Cập nhật thông tin cửa hàng và tài khoản thanh toán VietQR thành công!").  

**Alternative Flows**  
- **Số tài khoản ngân hàng không hợp lệ:**  
  1. Nhập sai số tài khoản hoặc tên ngân hàng.  
  2. Hệ thống báo lỗi kiểm tra VietQR và yêu cầu đối chiếu lại.  

**Postconditions**  
Thông tin nhận diện cửa hàng và cổng thanh toán chuyển khoản VietQR được kích hoạt chuẩn xác.  

---

### UC-065 Tạo báo giá công trình (Create Project Quote)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to create itemized construction project quotes grouped by areas/phases (e.g., Ground Floor Bath, Master Bath, Floor 2-3) with tailored bulk pricing so that contractors receive professional bids.  

**Preconditions**  
1. Đã có hồ sơ khách hàng và công trình xây dựng tương ứng (FE-10).  

**Normal Flow**  
*Lập bảng báo giá thiết bị vệ sinh theo từng hạng mục công trình*  
1. Người dùng nhấp nút "Tạo báo giá mới" (Create Project Quote).  
2. Hệ thống hiển thị biểu mẫu lập báo giá:  
   - Mã báo giá: Tự động sinh `BG-YYYYMMDD-XXXXX` (Phiên bản V1).  
   - Chọn Khách hàng & Chọn Công trình liên kết (ví dụ: "Công trình Villa Ecopark - Khách hàng Anh Tuấn").  
   - Hiệu lực báo giá: Ngày báo giá và Thời hạn hiệu lực (mặc định 30 ngày).  
   - Phân nhóm hạng mục công trình (Phòng tắm Master tầng 2, Phòng tắm khách tầng 1, Phòng vệ sinh sân vườn...).  
   - Thêm thiết bị vào từng hạng mục: Chọn SKU, Tên thiết bị, Số lượng, Đơn giá niêm yết, % Chiết khấu công trình riêng, Thành tiền từng món.  
   - Bảng tổng kết: Tổng giá trị thiết bị, Tổng chiết khấu công trình, Phí vận chuyển & Bốc xếp lên tầng, Tổng giá trị báo giá trọn gói.  
   - Điều khoản hợp đồng tóm tắt (Tiến độ giao hàng nhiều đợt, Tỷ lệ đặt cọc 30%, Tiến độ thanh toán).  
3. Người dùng nhập đầy đủ các hạng mục và thiết lập mức giá ưu đãi theo quy mô công trình.  
4. Người dùng nhấp nút "Lưu báo giá".  
5. Hệ thống kiểm tra: Danh sách thiết bị hợp lệ, Tên công trình hợp lệ.  
6. Hệ thống lưu bản báo giá ở trạng thái `Bản thảo / Đã gửi khách (Sent to Client)`.  
7. Hệ thống ghi nhận sự kiện vào Audit Log.  
8. Hệ thống hiển thị thông báo thành công (MSG202: "Tạo báo giá công trình thành công!") kèm tùy chọn "In/Xuất báo giá" (UC-072).  

**Alternative Flows**  
- **Kế thừa từ Yêu cầu tư vấn online (UC-134):**  
  1. Người dùng bấm chuyển từ tư vấn sang báo giá. Toàn bộ thiết bị đã chọn trong tư vấn được tự động đưa vào bảng báo giá.  

**Postconditions**  
Bảng báo giá công trình chuyên nghiệp được tạo lập, sẵn sàng gửi khách hàng phê duyệt.  

---

### UC-066 Cập nhật báo giá công trình (Update Project Quote)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to modify quantities, items, or discounts in a project quote while saving a version history (V1, V2, V3) so that negotiations are tracked across iterations.  

**Preconditions**  
Báo giá đang ở trạng thái đàm phán (chưa được khách duyệt thành đơn hàng chính thức).  

**Normal Flow**  
*Chỉnh sửa báo giá và lưu vết lịch sử phiên bản*  
1. Người dùng mở chi tiết báo giá công trình và nhấp nút "Tạo phiên bản điều chỉnh" (Revise Quote).  
2. Hệ thống tự động nhân bản báo giá hiện tại sang phiên bản mới (ví dụ: từ `BG-20261008-01-V1` thành `BG-20261008-01-V2`).  
3. Người dùng điều chỉnh: Thay đổi mẫu sen tắm sang mẫu cao cấp hơn, tăng số lượng bồn cầu, điều chỉnh tỷ lệ chiết khấu theo yêu cầu của chủ nhà.  
4. Người dùng nhập ghi chú phiên bản (ví dụ: "V2: Đổi sang sen cây phím đàn nhiệt độ theo yêu cầu chị vợ").  
5. Người dùng nhấp nút "Lưu phiên bản mới".  
6. Hệ thống lưu bản V2 và đánh dấu bản V1 là phiên bản cũ (Archived Version).  
7. Hệ thống hiển thị thông báo thành công (MSG203: "Đã tạo phiên bản báo giá mới V2 thành công!").  

**Alternative Flows**  
- **Báo giá đã được khách duyệt thành đơn hàng (Approved):**  
  1. Báo giá đã chuyển thành đơn hàng chính thức.  
  2. Nút chỉnh sửa bị khóa; hệ thống yêu cầu thao tác qua chỉnh sửa đơn hàng (UC-052).  

**Postconditions**  
Phiên bản báo giá mới được thiết lập, bảo toàn toàn bộ lịch sử đàm phán cũ.  

---

### UC-067 Xem danh sách báo giá (View Quote List)
**Primary Actors**  
Sales Staff, Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As an internal user, I want to search and filter project quotes by customer, project, status (Draft, Sent, Approved, Rejected, Expired), and sales rep so that project pipelines are managed.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý bán hàng.  

**Normal Flow**  
*Xem danh sách các bản báo giá thiết bị công trình*  
1. Người dùng chọn mục "Báo giá công trình" trên menu.  
2. Hệ thống hiển thị bảng danh sách các báo giá:  
   - Mã báo giá & Phiên bản, Ngày lập, Khách hàng, Tên công trình, Tổng giá trị báo giá (VNĐ), Người lập, Trạng thái (Huy hiệu màu: `Đang gửi khách`, `Đã duyệt`, `Đã từ chối`, `Đã chuyển đơn hàng`), Ngày hết hiệu lực.  
   - Bộ lọc: Tìm kiếm theo tên khách/công trình, Lọc theo trạng thái báo giá, Lọc theo khoảng ngày.  
3. Người dùng duyệt danh sách và nhấp vào một báo giá để xem chi tiết (UC-068).  

**Alternative Flows**  
- **Không có báo giá nào:**  
  1. Danh sách trống. Hệ thống hiển thị nút tạo báo giá mới.  

**Postconditions**  
Toàn bộ danh mục báo giá dự án được hiển thị rõ ràng.  

---

### UC-068 Xem chi tiết & lịch sử báo giá (View Quote Detail)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to view itemized breakdown and audit history of a quote (comparing V1 vs V2, seeing client interactions) so that I understand proposal evolution.  

**Preconditions**  
Người dùng nhấp vào một báo giá từ danh sách (UC-067).  

**Normal Flow**  
*Xem chi tiết các hạng mục và đối chiếu các phiên bản báo giá*  
1. Hệ thống hiển thị trang "Chi tiết báo giá - [Mã BG]":  
   - Danh sách các hạng mục thiết bị chi tiết theo từng phòng.  
   - Điều khoản hợp đồng và tổng giá trị.  
   - Tab "Lịch sử phiên bản" (Version Comparison): Cho phép chọn so sánh giữa bản V1 và V2 để xem rõ sản phẩm nào đã được thêm, bớt hoặc đổi giá.  
2. Người dùng kiểm tra thông tin để chuẩn bị trao đổi với khách hàng.  

**Alternative Flows**  
- **Báo giá đã hết hạn hiệu lực:**  
  1. Ngày hiện tại lớn hơn Ngày hết hạn của báo giá.  
  2. Hệ thống hiển thị cảnh báo: "Báo giá này đã hết hiệu lực. Vui lòng gia hạn hoặc tạo phiên bản mới trước khi chuyển thành đơn hàng".  

**Postconditions**  
Toàn bộ nội dung và lịch sử thay đổi của bản báo giá được hiển thị minh bạch.  

---

### UC-069 Ghi nhận khách chấp thuận báo giá (Record Quote Approval)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to record customer acceptance of a project quote so that the proposal transitions to approved status and is ready for conversion into a sales order.  

**Preconditions**  
Khách hàng đồng ý với phương án báo giá (ký hợp đồng hoặc đồng ý qua điện thoại/Zalo).  

**Normal Flow**  
*Ghi nhận khách hàng đã đồng ý với bản báo giá*  
1. Tại chi tiết báo giá, nhân viên nhấp nút "Ghi nhận khách duyệt" (Record Approval).  
2. Hệ thống hiển thị biểu mẫu xác nhận:  
   - Ngày khách chấp thuận (mặc định hôm nay).  
   - Hình thức xác nhận: `Ký hợp đồng trực tiếp`, `Xác nhận qua tin nhắn Zalo/Email`, `Đồng ý qua điện thoại`.  
   - Đính kèm ảnh hợp đồng có chữ ký (nếu có).  
   - Ghi chú thỏa thuận (ví dụ: "Khách đồng ý chốt phương án V2, hẹn đặt cọc 30% vào ngày mai").  
3. Nhân viên nhập thông tin và nhấp "Xác nhận duyệt".  
4. Hệ thống chuyển trạng thái báo giá sang `Đã chấp thuận (Approved)`.  
5. Hệ thống ghi nhật ký sự kiện vào Audit Log.  
6. Hệ thống hiển thị thông báo thành công (MSG204: "Ghi nhận khách hàng duyệt báo giá thành công! Bạn có thể chuyển báo giá thành đơn hàng ngay bây giờ") kèm nút kích hoạt UC-073.  

**Alternative Flows**  
- **Hủy thao tác duyệt:**  
  1. Người dùng bấm Hủy. Trạng thái báo giá giữ nguyên.  

**Postconditions**  
Báo giá chuyển sang trạng thái `Approved`, sẵn sàng để sinh đơn hàng công trình chính thức.  

---

### UC-070 Ghi nhận khách từ chối báo giá (Reject Quote)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to record customer rejection of a quote with mandatory reason capture (price too high, competitor selected, cancelled build) so that lost-deal analytics can be audited.  

**Preconditions**  
Khách hàng không đồng ý với phương án báo giá.  

**Normal Flow**  
*Ghi nhận lý do khách hàng từ chối bản báo giá*  
1. Tại chi tiết báo giá, nhân viên nhấp nút "Ghi nhận từ chối" (Record Rejection).  
2. Hệ thống hiển thị hộp thoại:  
   - Chọn lý do từ chối (Dropdown): `Giá quá cao so với ngân sách`, `Chọn nhà cung cấp đối thủ`, `Công trình tạm hoãn thi công`, `Không đúng sở thích mẫu mã`, `Khác`.  
   - Ô nhập mô tả chi tiết lý do (bắt buộc, ví dụ: "Khách chọn bên Toto khác do được chiết khấu thêm 3%").  
3. Nhân viên nhập lý do và nhấp "Xác nhận từ chối".  
4. Hệ thống chuyển trạng thái báo giá sang `Đã từ chối (Rejected)`.  
5. Hệ thống ghi nhận dữ liệu phục vụ báo cáo tỷ lệ chốt thầu.  
6. Hệ thống hiển thị thông báo (MSG205: "Đã ghi nhận khách hàng từ chối báo giá thành công!").  

**Alternative Flows**  
- **Không nhập lý do từ chối:**  
  1. Bỏ trống ô mô tả. Hệ thống báo lỗi (MSG40).  

**Postconditions**  
Báo giá được đóng với lý do minh bạch, phục vụ phân tích rút kinh nghiệm kinh doanh.  

---

### UC-071 Hủy báo giá (Cancel Quote)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to void an obsolete or mistaken quote so that it is closed and removed from active pipeline tracking.  

**Preconditions**  
Báo giá chưa chuyển thành đơn hàng và không còn hiệu lực thực hiện.  

**Normal Flow**  
*Hủy bỏ bản báo giá không còn giá trị*  
1. Người dùng nhấp nút "Hủy báo giá" (Cancel Quote).  
2. Hệ thống hiển thị hộp thoại xác nhận hủy kèm ô nhập lý do.  
3. Người dùng nhập lý do và nhấp "Xác nhận hủy".  
4. Hệ thống chuyển trạng thái báo giá sang `Đã hủy (Cancelled)`.  
5. Hệ thống hiển thị thông báo thành công (MSG206: "Hủy báo giá thành công!").  

**Alternative Flows**  
- **Báo giá đã sinh đơn hàng:**  
  1. Báo giá đã chuyển thành đơn hàng. Hệ thống khóa nút hủy và thông báo lỗi.  

**Postconditions**  
Báo giá bị vô hiệu hóa hoàn toàn trên hệ thống.  

---

### UC-072 In/Xuất báo giá (Print/Export Quote)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to print physical quotes or export them to branded PDF/Excel files with product thumbnail images so that professional proposals can be presented to contractors.  

**Preconditions**  
Báo giá đã được tạo trên hệ thống.  

**Normal Flow**  
*Xuất bản báo giá công trình chuyên nghiệp ra PDF/Excel*  
1. Người dùng nhấp nút "In/Xuất báo giá" (Export Quote).  
2. Người dùng chọn định dạng xuất:  
   - `Xuất file PDF chuyên nghiệp` (Trang bìa sang trọng, logo cửa hàng, ảnh sản phẩm từng dòng, bảng chi tiết theo phòng, điều khoản bảo hành và chữ ký).  
   - `Xuất file Excel (.xlsx)` (Phục vụ nhà thầu bóc tách khối lượng và dự toán công trình).  
3. Người dùng nhấp "Xác nhận tải về".  
4. Hệ thống kết xuất tài liệu và tự động tải file `Bao_Gia_[MaBG]_[TenCongTrinh].pdf` về máy tính.  
5. Người dùng gửi file cho khách hàng qua Zalo/Email hoặc in ra giấy khổ A4 kẹp hồ sơ dự án.  

**Alternative Flows**  
- **Tùy chọn ẩn giá vốn hoặc ẩn hình ảnh:**  
  1. Người dùng có thể tích chọn ẩn/hiển thị cột ảnh sản phẩm để file PDF gọn gàng hơn.  

**Postconditions**  
Bản báo giá tiêu chuẩn thương mại được kết xuất hoàn chỉnh.  

---

### UC-073 Chuyển báo giá thành đơn hàng (Convert Quote to Order)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to convert an approved project quote into an active project sales order (inheriting all items, project address, pricing, and multi-batch delivery schedule) so that execution begins without data re-entry.  

**Preconditions**  
Báo giá đang ở trạng thái `Đã chấp thuận (Approved)` (UC-069).  

**Normal Flow**  
*Chuyển đổi bản báo giá đã duyệt thành đơn hàng công trình chính thức*  
1. Tại màn hình chi tiết báo giá đã duyệt, người dùng nhấp nút "Chuyển thành Đơn hàng" (Convert to Sales Order).  
2. Hệ thống hiển thị hộp thoại xác nhận chuyển đổi:  
   - Kế thừa toàn bộ thông tin khách hàng, địa chỉ công trình và người giám sát nhận hàng.  
   - Kế thừa toàn bộ danh sách thiết bị và mức giá đã thỏa thuận trong báo giá.  
   - Kênh bán hàng: Đánh dấu là `Bán công trình / Dự án (Project Sale)`.  
   - Bật cờ cho phép giao hàng nhiều đợt (Multi-batch Delivery Allowed - FE-08).  
3. Người dùng nhấp "Xác nhận tạo đơn hàng".  
4. Hệ thống tạo một Đơn hàng mới với mã `ORD-PRJ-YYYYMMDD-XXXXX`.  
5. Hệ thống kích hoạt quy trình giữ chỗ tồn kho (UC-086) cho toàn bộ các thiết bị trong đơn hàng.  
6. Hệ thống cập nhật trạng thái báo giá gốc sang `Đã chuyển đơn hàng (Converted)`.  
7. Hệ thống chuyển hướng người dùng thẳng tới màn hình chi tiết Đơn hàng công trình vừa tạo (UC-047).  
8. Hệ thống hiển thị thông báo thành công (MSG207: "Chuyển báo giá thành Đơn hàng công trình thành công! Đơn hàng đã sẵn sàng để lập đợt giao và thu tiền cọc").  

**Alternative Flows**  
- **Tồn kho trong kho không đủ để giữ chỗ toàn bộ dự án:**  
  1. Một số thiết bị có số lượng vượt quá tồn hiện có trong kho.  
  2. Hệ thống vẫn cho phép tạo đơn công trình nhưng gắn nhãn "Chờ nhập hàng bổ sung" và tự động gợi ý tạo đề xuất nhập hàng (UC-138) cho lượng hàng còn thiếu.  

**Postconditions**  
1. Đơn hàng công trình mới được khởi tạo hoàn chỉnh.  
2. Báo giá gốc được đánh dấu đã chuyển đổi.  

---

### UC-144 Áp dụng chiết khấu & chương trình khuyến mãi (Apply Discount & Promo)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to apply voucher codes, tier discounts (plumbers, contractors), or line-item/order-level discounts (percentage or lump-sum) with permission controls so that sales incentives are accurately applied.  

**Preconditions**  
Người dùng đang ở màn hình tạo hoặc chỉnh sửa đơn hàng (POS hoặc Quản trị).  

**Normal Flow**  
*Áp dụng mã giảm giá và chiết khấu linh hoạt cho đơn hàng*  
1. Tại khu vực thanh toán đơn hàng, người dùng chọn áp dụng chiết khấu:  
   - **Tự động theo nhóm khách hàng**: Nếu khách là Thợ/Nhà thầu (UC-142), hệ thống tự động áp mức chiết khấu quy định (ví dụ: Giảm 8% tiền hàng).  
   - **Áp dụng Mã Voucher khuyến mãi**: Người dùng nhập mã giảm giá (ví dụ: `CHAOMUNG2026`) -> Hệ thống kiểm tra điều kiện voucher (thời hạn, đơn tối thiểu) và trừ tiền.  
   - **Chiết khấu trực tiếp theo quyết định của nhân viên**: Nhập giảm theo % (ví dụ: giảm thêm 3%) hoặc giảm số tiền mặt cụ thể (ví dụ: bớt 200.000 VNĐ tiền lẻ).  
2. Người dùng nhấp "Áp dụng chiết khấu".  
3. Hệ thống kiểm tra hạn mức thẩm quyền:  
   - Sales Staff chỉ được phép chiết khấu tối đa 10%.  
   - Nếu chiết khấu > 10%: Hệ thống yêu cầu Store Owner nhập mã PIN xác nhận phê duyệt (Manager Override).  
4. Hệ thống tính toán lại Tổng tiền đơn hàng ngay lập tức.  
5. Hệ thống hiển thị thông báo thành công (MSG208: "Áp dụng chiết khấu thành công!").  

**Alternative Flows**  
- **Voucher không hợp lệ hoặc hết hạn:**  
  1. Mã voucher hết hiệu lực.  
  2. Hệ thống báo lỗi (MSG209: "Mã voucher không hợp lệ hoặc đã hết lượt sử dụng").  
- **Nhân viên vượt quá thẩm quyền chiết khấu không có Store Owner duyệt:**  
  1. Nhân viên nhập giảm 15% nhưng không có Store Owner nhập mã PIN.  
  2. Hệ thống chặn thao tác và cảnh báo quyền hạn.  

**Postconditions**  
Tổng số tiền đơn hàng được giảm trừ hợp lệ theo đúng chính sách bán hàng.  

---

### UC-145 Tạo phiếu đặt cọc giữ hàng công trình (Create Reservation / Layaway)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to create a Reservation Order locking in prices and reserving stock for customers waiting for home construction completion so that promotional prices and physical units are guaranteed.  

**Preconditions**  
Khách hàng muốn mua thiết bị vệ sinh vào đợt khuyến mãi nhưng nhà đang xây thô, 2 tháng sau mới hoàn thiện để lắp đặt.  

**Normal Flow**  
*Lập phiếu đặt cọc giữ hàng và khóa giá khuyến mãi*  
1. Nhân viên chọn hình thức đơn hàng: `Đặt cọc giữ hàng & giữ giá (Reservation Order)`.  
2. Nhân viên chọn khách hàng, danh sách thiết bị và áp dụng giá ưu đãi hiện tại.  
3. Nhân viên nhập:  
   - Ngày hẹn lấy hàng dự kiến (ví dụ: sau 60 ngày).  
   - Số tiền đặt cọc bắt buộc (Tối thiểu 30% giá trị đơn hàng, ví dụ cọc 10.000.000 VNĐ trên tổng đơn 30.000.000 VNĐ).  
   - Cam kết giữ giá: "Cửa hàng cam kết giữ nguyên mức giá và giữ hàng trong kho cho đến ngày [DD/MM/YYYY]".  
4. Khách hàng thanh toán tiền cọc 10.000.000 VNĐ.  
5. Nhân viên nhấp "Lưu phiếu đặt cọc giữ hàng".  
6. Hệ thống tạo đơn hàng ở trạng thái `Đặt cọc giữ hàng (Reserved)`.  
7. Hệ thống tự động khóa giữ chỗ tồn kho (UC-086) cho toàn bộ các sản phẩm trong đơn trong suốt thời hạn giữ hàng.  
8. Hệ thống in Phiếu cam kết giữ hàng và giao biên nhận cho khách.  
9. Hệ thống hiển thị thông báo thành công (MSG210: "Tạo phiếu đặt cọc giữ hàng thành công! Hàng hóa đã được khóa giữ chỗ an toàn").  

**Alternative Flows**  
- **Tiền đặt cọc nhỏ hơn mức tối thiểu 30%:**  
  1. Khách cọc dưới 30% giá trị đơn.  
  2. Hệ thống cảnh báo và yêu cầu phê duyệt đặc biệt từ Store Owner nếu cho phép cọc mức thấp hơn.  

**Postconditions**  
Hàng hóa được giữ chỗ dài hạn, giá bán được cố định cho khách hàng.  

---

### UC-146 Gia hạn / Hủy phiếu đặt cọc giữ hàng (Extend / Cancel Reservation)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to extend the holding deadline or cancel an expired reservation (forfeiting/refunding deposit and releasing reserved inventory) so that inventory is not blocked indefinitely.  

**Preconditions**  
Phiếu đặt cọc giữ hàng đã đến hạn nhận hàng nhưng khách chưa lấy hoặc xin lùi lịch.  

**Normal Flow**  
*Xử lý gia hạn thời gian giữ hàng hoặc hủy cọc giải phóng hàng*  
1. Người dùng mở phiếu đặt cọc giữ hàng sắp hoặc đã quá hạn.  
2. **Nếu khách xin gia hạn (Extend):**  
   - Nhân viên nhấp nút "Gia hạn thời gian giữ hàng".  
   - Chọn ngày hẹn mới (tối đa thêm 30 ngày) và nhập lý do khách hoãn thi công.  
   - Hệ thống cập nhật hạn giữ hàng mới.  
3. **Nếu khách bỏ cọc hoặc quá hạn quy định (Cancel & Forfeit):**  
   - Nhân viên nhấp nút "Hủy phiếu giữ hàng do quá hạn".  
   - Hệ thống hiển thị điều khoản xử lý tiền cọc: Tịch thu tiền cọc đưa vào thu nhập khác của cửa hàng hoặc hoàn lại một phần theo thỏa thuận.  
   - Hệ thống chuyển trạng thái phiếu sang `Đã hủy cọc (Cancelled)`.  
   - Hệ thống tự động giải phóng toàn bộ số lượng tồn kho đang giữ chỗ (UC-087) về trạng thái có thể bán ngay.  
4. Hệ thống ghi nhận sự kiện vào Audit Log.  
5. Hệ thống hiển thị thông báo thành công (MSG211: "Xử lý phiếu giữ hàng thành công!").  

**Alternative Flows**  
- **Hủy cọc có hoàn lại tiền cho khách:**  
  1. Cửa hàng đồng ý hoàn trả tiền cọc vì lý do bất khả kháng.  
  2. Hệ thống tạo phiếu chi hoàn cọc (UC-061).  

**Postconditions**  
Trạng thái phiếu cọc được cập nhật, tồn kho bị khóa được giải phóng kịp thời.  

---

### UC-147 Gửi thông báo / Nhắc nợ khách hàng (Send Debt Reminder)
**Primary Actors**  
Sales Staff, Accountant, Store Owner  

**Secondary Actors**  
SMS Service, Email Service  

**Description**  
As an internal staff member, I want to generate and dispatch polite debt reminder notices (via SMS, Zalo, or Email) with payment QR codes to debtor customers so that overdue receivables are settled.  

**Preconditions**  
Khách hàng đang có khoản nợ đến hạn hoặc quá hạn thanh toán (UC-058).  

**Normal Flow**  
*Tạo và gửi tin nhắn nhắc nợ kèm mã VietQR thanh toán nhanh*  
1. Người dùng nhấp vào nút "Nhắc nợ" tại dòng khách hàng trong Sổ công nợ.  
2. Hệ thống hiển thị biểu mẫu gửi thông báo nhắc nợ:  
   - Kênh gửi: `Tin nhắn SMS`, `Zalo ZNS`, `Email`.  
   - Mẫu tin nhắn (Dropdown): `Nhắc nợ lịch sự sắp đến hạn`, `Thông báo khoản nợ quá hạn lần 1`, `Cảnh báo tạm dừng cấp hàng do nợ quá hạn`.  
   - Nội dung tin nhắn tự động điền:  
     "Kính gửi Quý khách [Tên KH], Cửa hàng AQS xin thông báo quý khách có khoản thanh toán đơn hàng [Mã đơn] số tiền [X VNĐ] đến hạn ngày [DD/MM/YYYY]. Quý khách có thể quét mã VietQR thanh toán nhanh tại: [Link_VietQR]. Hotline: 0988xxx. Xin cảm ơn!".  
3. Người dùng kiểm tra nội dung và nhấp nút "Gửi thông báo nhắc nợ".  
4. Hệ thống gọi Secondary Actor `SMS Service` / `Email Service` gửi tin nhắn tức thì tới số điện thoại của khách hàng.  
5. Hệ thống lưu vết sự kiện nhắc nợ vào Nhật ký chăm sóc khách hàng (UC-141) kèm dấu thời gian.  
6. Hệ thống hiển thị thông báo thành công (MSG212: "Đã gửi tin nhắn nhắc nợ thành công!").  

**Alternative Flows**  
- **Gửi thất bại do số điện thoại không hợp lệ:**  
  1. Cổng SMS báo lỗi số thuê bao.  
  2. Hệ thống báo lỗi và gợi ý liên hệ trực tiếp qua điện thoại.  

**Postconditions**  
Thông điệp nhắc nợ được gửi đến khách hàng, tạo cơ sở đôn đốc thanh toán.  

---

### UC-148 In phiếu hẹn giao hàng công trình (Print Delivery Schedule Slip)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to print physical Delivery Schedule Slips for project contractors detailing phase timetables and site unloading agreements so that site coordination is clear.  

**Preconditions**  
Đơn hàng công trình đã được lên lịch trình các đợt giao (UC-073, UC-135).  

**Normal Flow**  
*In phiếu xác nhận lịch trình các đợt giao hàng công trình*  
1. Người dùng mở đơn hàng công trình và nhấp nút "In lịch trình giao hàng".  
2. Hệ thống hiển thị bản in chuẩn:  
   - Tên công trình, Địa chỉ thi công, Tên cai thầu nhận hàng, SĐT.  
   - Bảng tiến độ từng đợt giao:  
     + Đợt 1 (Ngày DD/MM): Giao thiết bị âm tường (Van xả âm, đế bồn cầu, ống nối).  
     + Đợt 2 (Ngày DD/MM): Giao thân sứ bồn cầu, chậu lavabo, bồn tắm.  
     + Đợt 3 (Ngày DD/MM): Giao phụ kiện sen vòi mạ crom, gương led, phụ kiện kính.  
   - Quy định bốc dỡ hàng: Giao tại chân công trình hay bốc vác lên tầng; xe tải được vào khung giờ nào.  
   - Chữ ký xác nhận của Đại diện cửa hàng và Đại diện nhà thầu xây dựng.  
3. Người dùng in ra giấy và đưa cho khách hàng kẹp vào hồ sơ thi công.  

**Alternative Flows**  
- **Xuất file PDF:**  
  1. Người dùng chọn lưu file PDF để gửi qua Zalo nhóm công trình.  

**Postconditions**  
Biên bản lịch trình giao nhận được xác lập, tránh xung đột về tiến độ thi công.  

---

### UC-149 Mở ca bán hàng (Open Sales Shift)
**Primary Actors**  
Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, I want to count and record beginning cash float in the POS cash drawer before starting sales transactions so that daily counter cash flow is audited.  

**Preconditions**  
1. Nhân viên bắt đầu ca làm việc (ca sáng hoặc ca chiều).  
2. Hiện tại chưa có ca bán hàng nào đang mở trên máy POS này.  

**Normal Flow**  
*Kiểm đếm tiền mặt đầu ca và mở ca bán hàng*  
1. Nhân viên đăng nhập vào giao diện Bán hàng tại quầy (POS).  
2. Hệ thống nhận diện chưa có ca làm việc nào đang mở và hiển thị màn hình "Mở ca bán hàng mới":  
   - Tên nhân viên trực ca (tự động điền).  
   - Thời gian bắt đầu ca (thời gian hiện tại).  
   - Bảng đếm mệnh giá tiền mặt đầu ca (Bảng kiểm đếm chi tiết từ tờ 500k, 200k, 100k, 50k, 20k, 10k...).  
   - Ô tổng tiền mặt đầu ca trong két (Beginning Cash Float, ví dụ: `2.000.000 VNĐ`).  
   - Ghi chú đầu ca.  
3. Nhân viên đếm tiền lẻ chuẩn bị trong két và nhập số tiền thực tế.  
4. Nhân viên nhấp nút "Xác nhận mở ca".  
5. Hệ thống kiểm tra: Số tiền đầu ca >= 0.  
6. Hệ thống tạo bản ghi ca bán hàng mới với trạng thái `Đang mở (Open)` và mã ca `CA-YYYYMMDD-XXXXX`.  
7. Hệ thống mở khóa toàn bộ các chức năng bán hàng POS (UC-048) cho nhân viên.  
8. Hệ thống hiển thị thông báo thành công (MSG213: "Mở ca bán hàng thành công! Chúc bạn một ca làm việc may mắn!").  

**Alternative Flows**  
- **Ca trước chưa kết ca:**  
  1. Nhân viên ca trước quên chưa bấm đóng ca.  
  2. Hệ thống cảnh báo: "Ca làm việc trước chưa được đóng. Vui lòng kết ca trước hoặc yêu cầu quản trị viên can thiệp".  

**Postconditions**  
Ca bán hàng được kích hoạt với số dư đầu ca được ghi nhận chính xác.  

---

### UC-150 Ghi nhận thu/chi tiền mặt trong ca (Record Petty Cash In/Out)
**Primary Actors**  
Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, I want to log ad-hoc cash inflows/outflows directly from the cash drawer during a shift (e.g., paying delivery drivers, petty office water, cash top-ups) so that closing cash balances remain balanced.  

**Preconditions**  
Ca bán hàng hiện tại đang ở trạng thái `Open` (UC-149).  

**Normal Flow**  
*Ghi nhận chi tiêu vặt hoặc thu tiền phụ từ két bán hàng*  
1. Nhân viên nhấp nút "Thu/Chi tiền mặt trong ca" (Petty Cash) trên màn hình POS.  
2. Hệ thống hiển thị biểu mẫu:  
   - Chọn loại giao dịch: `Chi tiền khỏi két (Cash Out)` hoặc `Thu thêm tiền vào két (Cash In)`.  
   - Số tiền (VNĐ): ví dụ chi `150.000 VNĐ`.  
   - Lý do thu/chi: Chọn danh mục (`Trả tiền xăng xe/bốc vác cho shipper`, `Mua nước uống tiếp khách`, `Mua băng dính đóng hàng`, `Khác`).  
   - Nội dung chi tiết (ví dụ: "Chi tiền bốc 2 bồn cầu lên xe ba gác").  
3. Nhân viên nhập thông tin và nhấp nút "Lưu giao dịch tiền mặt".  
4. Hệ thống kiểm tra: Số tiền > 0; nếu là Chi tiền thì Số tiền chi <= Số tiền mặt hiện có trong két.  
5. Hệ thống tự động trừ/cộng tiền trong két ca hiện tại và lưu bản ghi thu chi vặt.  
6. Hệ thống in phiếu thu/chi nhỏ từ máy in nhiệt kẹp cùng hóa đơn trong két.  
7. Hệ thống hiển thị thông báo thành công (MSG214: "Ghi nhận thu/chi tiền két thành công!").  

**Alternative Flows**  
- **Chi vượt quá số tiền mặt có trong két:**  
  1. Tiền mặt trong két chỉ còn 100k nhưng nhập chi 200k.  
  2. Hệ thống báo lỗi (MSG215: "Số tiền chi vượt quá số tiền mặt hiện có trong két quầy").  

**Postconditions**  
Các biến động tiền mặt phát sinh trong ca được ghi chép tức thì, phục vụ đối soát kết ca.  

---

### UC-151 Kết ca và in báo cáo ca (Close Shift & Z-Report)
**Primary Actors**  
Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, I want to count closing cash, compare physical totals with system totals, record any cash discrepancy, and print a Z-Report at the end of my shift so that cashier accountability is finalized.  

**Preconditions**  
Ca bán hàng đang mở và nhân viên chuẩn bị kết thúc thời gian trực ca.  

**Normal Flow**  
*Kiểm đếm tiền cuối ca, đối soát chênh lệch và in phiếu Z-Report*  
1. Nhân viên nhấp nút "Kết thúc ca bán hàng" (Close Shift) trên màn hình POS.  
2. Hệ thống hiển thị màn hình Kiểm đếm kết ca (Z-Report Closing):  
   - Tiền đầu ca: `2.000.000 VNĐ`.  
   - Doanh thu tiền mặt phát sinh trong ca: `15.500.000 VNĐ`.  
   - Thu tiền vặt: `0 VNĐ` - Chi tiền vặt trong ca: `150.000 VNĐ`.  
   - Tiền mặt hệ thống tính toán phải có trong két (`Expected Cash`): `17.350.000 VNĐ`.  
   - Doanh thu chuyển khoản VietQR trong ca: `25.000.000 VNĐ` (đối chiếu riêng).  
   - Bảng nhập liệu "Số tiền mặt thực tế kiểm đếm cuối ca" (`Actual Cash`).  
3. Nhân viên đếm toàn bộ tiền mặt trong két và nhập số tiền thực đếm (ví dụ đếm được đúng `17.350.000 VNĐ`).  
4. Hệ thống tính toán chênh lệch tự động:  
   - `Chênh lệch = Actual Cash - Expected Cash = 0 VNĐ` (Khớp 100%).  
5. Nhân viên nhập ghi chú kết ca và nhấp nút "Xác nhận đóng ca".  
6. Hệ thống chuyển trạng thái ca bán hàng sang `Đã đóng (Closed)` và khóa chức năng tạo đơn của ca này.  
7. Hệ thống tự động kích hoạt máy in nhiệt in Báo cáo kết ca (Phiếu Z-Report) gồm: Tổng doanh thu, Tổng số đơn hàng, Phân loại tiền mặt/chuyển khoản, Chi phí vặt và Chênh lệch két.  
8. Nhân viên ký tên vào phiếu Z-Report và kẹp cùng tiền mặt bàn giao.  
9. Hệ thống hiển thị thông báo thành công (MSG216: "Đóng ca bán hàng thành công! Số liệu hoàn toàn trùng khớp").  

**Alternative Flows**  
- **Phát hiện chênh lệch tiền két (Thừa hoặc Thiếu tiền mặt):**  
  1. Tiền thực đếm lệch so với hệ thống (ví dụ: thiếu 50.000 VNĐ do trả nhầm tiền thừa cho khách).  
  2. Cột chênh lệch hiển thị màu đỏ (-50.000 VNĐ).  
  3. Hệ thống bắt buộc nhân viên nhập "Giải trình lý do chênh lệch tiền két" trước khi cho phép bấm đóng ca.  
  4. Hệ thống ghi nhận khoản chênh lệch vào Audit Log để Kế toán và Chủ cửa hàng kiểm tra đối soát.  

**Postconditions**  
1. Ca bán hàng được đóng chính thức.  
2. Phiếu Z-Report được in ra phục vụ bàn giao ca (UC-152).  

---

### UC-152 Bàn giao ca trực (Handover Shift)
**Primary Actors**  
Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, I want to transfer the counted cash float, physical store keys, and ongoing customer orders to the incoming shift cashier with co-signatures so that handovers are seamless.  

**Preconditions**  
Ca hiện tại đã được đóng (UC-151) và nhân viên ca tiếp theo đã có mặt tại quầy.  

**Normal Flow**  
*Bàn giao tiền két và công việc dang dở cho nhân viên ca sau*  
1. Nhân viên ca trước nhấp chọn chức năng "Bàn giao ca" (Handover Shift).  
2. Hệ thống hiển thị biên bản bàn giao:  
   - Nhân viên bàn giao (Ca trước) và Chọn Nhân viên nhận bàn giao (Ca sau).  
   - Số tiền mặt thực tế bàn giao lại trong két (để làm tiền đầu ca cho ca sau).  
   - Danh sách các đơn hàng hoặc yêu cầu tư vấn đang xử lý dở dang cần ca sau theo dõi tiếp (ví dụ: "Có đơn hàng của chị Mai hẹn chiều đến lấy bồn cầu").  
3. Nhân viên ca sau kiểm đếm lại tiền mặt trong két, đọc các ghi chú và nhập mật khẩu tài khoản của mình để xác nhận đồng thuận nhận bàn giao (Digital Handover Signature).  
4. Nhân viên bấm "Xác nhận hoàn tất bàn giao ca".  
5. Hệ thống lưu biên bản bàn giao vào cơ sở dữ liệu.  
6. Hệ thống hiển thị thông báo thành công (MSG217: "Bàn giao ca thành công!") và tự động chuyển sang màn hình Đăng nhập của nhân viên mới.  

**Alternative Flows**  
- **Nhân viên ca sau không đồng ý nhận do thiếu tiền:**  
  1. Tiền trong két thực tế không khớp với biên bản.  
  2. Nhân viên ca sau từ chối nhập mật khẩu và yêu cầu nhân viên ca trước kiểm đếm lại.  

**Postconditions**  
Trách nhiệm quản lý két tiền và đơn hàng được chuyển giao minh bạch sang nhân viên ca tiếp theo.  

---

### UC-253 Ghi nhận tiền thu hộ (Record COD Collection)
**Primary Actors**  
Shipper  

**Secondary Actors**  
None  

**Description**  
As a Shipper, I want to record the actual cash amount collected from the customer upon delivering an order so that unsettled cash holdings are registered to my account.  

**Preconditions**  
1. Shipper vừa hoàn tất giao đơn hàng thành công tại công trình (UC-095).  
2. Đơn hàng có hình thức thanh toán là Thu hộ tiền mặt khi nhận hàng (COD).  

**Normal Flow**  
*Shipper ghi nhận số tiền mặt COD thực tế đã thu từ khách hàng*  
1. Trên màn hình cập nhật kết quả giao hàng, hệ thống hiển thị ô "Số tiền COD cần thu": ví dụ `12.500.000 VNĐ`.  
2. Shipper kiểm đếm tiền mặt nhận từ khách và nhập "Số tiền thực thu": `12.500.000 VNĐ`.  
3. Shipper nhấp nút "Xác nhận đã thu đủ tiền COD".  
4. Hệ thống kiểm tra: Số tiền thu khớp với số tiền cần thu của đợt giao.  
5. Hệ thống cập nhật trạng thái đơn hàng sang `Đã thu COD qua Shipper`.  
6. Hệ thống tự động ghi nhận số tiền này vào tài khoản công nợ giữ hộ tiền của Shipper (`Shipper Holding Balance`).  
7. Hệ thống hiển thị thông báo thành công (MSG218: "Đã ghi nhận thu tiền COD thành công! Số tiền đang được lưu giữ trong danh sách chờ bàn giao").  

**Alternative Flows**  
- **Khách không trả đủ tiền mặt (Thu thiếu):**  
  1. Khách xin bớt hoặc hẹn chuyển khoản phần còn thiếu.  
  2. Shipper nhập số tiền thực thu nhỏ hơn số tiền cần thu kèm ghi chú giải thích.  
  3. Hệ thống ghi nhận số tiền thực tế và cập nhật dư nợ còn lại của khách.  

**Postconditions**  
Khoản tiền COD được Shipper nắm giữ và theo dõi trong danh sách chờ nộp về cửa hàng (UC-254).  

---

### UC-254 Xem các khoản thu hộ chưa bàn giao (View Unsettled COD Collections)
**Primary Actors**  
Shipper  

**Secondary Actors**  
None  

**Description**  
As a Shipper, I want to view all collected COD amounts pending settlement and my total holding balance so that I can remit funds accurately to the store cashier/accountant.  

**Preconditions**  
Shipper đã đăng nhập vào hệ thống ứng dụng di động.  

**Normal Flow**  
*Shipper theo dõi tổng số tiền mặt COD đang cầm trên người chưa nộp về cửa hàng*  
1. Shipper chọn mục "Tiền thu hộ COD" (COD Balances) trên menu cá nhân.  
2. Hệ thống hiển thị giao diện quản lý tiền thu hộ:  
   - Thẻ số nổi bật: **Tổng tiền COD đang giữ chờ nộp** (ví dụ: `32.400.000 VNĐ` từ 3 đơn hàng).  
   - Danh sách các khoản thu chi tiết: Mã phiếu xuất, Mã đơn hàng, Tên khách hàng, Thời gian thu tiền, Số tiền đã thu.  
   - Nút "Nộp tiền về cửa hàng" (Submit Handover - UC-255).  
3. Shipper kiểm tra đối chiếu tiền mặt trong ví với con số hiển thị trên ứng dụng.  

**Alternative Flows**  
- **Không có tiền COD nào đang giữ:**  
  1. Shipper đã nộp hết tiền các đơn trước đó.  
  2. Hệ thống hiển thị: "Bạn không có khoản tiền COD nào chờ bàn giao".  

**Postconditions**  
Shipper nắm bắt chính xác nghĩa vụ nộp tiền mặt của mình.  

---

### UC-255 Ghi nhận bàn giao tiền thu hộ (Submit COD Handover)
**Primary Actors**  
Shipper, Accountant, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Shipper, I want to select collected orders and submit a cash remittance request to the cashier/accountant so that my holding liability is cleared upon cashier confirmation.  

**Preconditions**  
Shipper đang giữ tiền COD (UC-254) và quay trở về cửa hàng sau chuyến đi giao hàng.  

**Normal Flow**  
*Shipper lập phiếu nộp tiền COD cho Kế toán / Thu ngân quầy*  
1. Shipper nhấp nút "Nộp tiền bàn giao" (Submit Handover).  
2. Hệ thống hiển thị danh sách các đơn hàng COD đang giữ: Shipper tích chọn các đơn muốn nộp tiền đợt này (mặc định chọn tất cả).  
3. Hệ thống hiển thị tổng số tiền nộp: `32.400.000 VNĐ`.  
4. Shipper nhấp "Xác nhận tạo phiếu nộp tiền".  
5. Hệ thống tạo một Phiếu nộp tiền COD với mã `NOP-COD-XXXXX` ở trạng thái `Chờ thủ quỹ/kế toán xác nhận (Pending Cashier Confirmation)`.  
6. Shipper mang tiền mặt đến quầy giao cho Kế toán / Thu ngân kiểm đếm.  
7. Kế toán kiểm đếm tiền mặt thực nhận: Đủ 32.400.000 VNĐ.  
8. Kế toán nhấp nút "Xác nhận đã nhận đủ tiền từ Shipper" trên hệ thống quản trị.  
9. Hệ thống chuyển trạng thái phiếu nộp sang `Đã hoàn tất nộp tiền (Settled)`.  
10. Hệ thống tự động trừ sạch số dư nợ giữ hộ tiền của Shipper (`Shipper Holding Balance = 0 VNĐ`).  
11. Hệ thống tự động ghi tăng tiền mặt vào két quầy / quỹ tiền mặt cửa hàng (ACC-F10).  
12. Hệ thống in biên nhận nộp tiền COD cho Shipper giữ.  
13. Hệ thống hiển thị thông báo thành công (MSG219: "Bàn giao tiền thu hộ COD thành công!").  

**Alternative Flows**  
- **Kế toán phát hiện tiền rách hoặc thiếu tiền khi đếm:**  
  1. Tiền mặt thực tế thiếu 200.000 VNĐ hoặc có tiền giả/rách.  
  2. Kế toán từ chối bấm duyệt, yêu cầu Shipper bù tiền mặt đủ mới bấm xác nhận.  

**Postconditions**  
1. Trách nhiệm giữ tiền của Shipper được giải phóng hoàn toàn.  
2. Dòng tiền được nhập chính thức vào sổ quỹ cửa hàng.  

---

### UC-256 Xem lịch sử và trạng thái bàn giao tiền (View COD Handover History)
**Primary Actors**  
Shipper, Accountant, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Shipper, Accountant, or Store Owner, I want to view historical COD remittance batches and cashier approval timestamps so that remittance discrepancies are easily reconciled.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống.  

**Normal Flow**  
*Xem lịch sử các lần bàn giao và nộp tiền COD*  
1. Người dùng chọn mục "Lịch sử nộp tiền COD" (COD Settlement History).  
2. Hệ thống tải và hiển thị danh sách các đợt nộp tiền:  
   - Bảng danh sách: Mã phiếu nộp, Shipper nộp, Ngày giờ nộp, Số tiền nộp (VNĐ), Danh sách các đơn hàng cấu thành, Kế toán ký nhận, Thời gian duyệt, Trạng thái (`Đã tất toán` / `Đang chờ duyệt`).  
   - Bộ lọc: Lọc theo Shipper, Lọc theo khoảng ngày.  
3. Người dùng nhấp vào một phiếu để xem chi tiết biên nhận và danh sách đơn hàng đã gạch nợ.  

**Alternative Flows**  
- **Không tìm thấy dữ liệu:**  
  1. Bộ lọc không có giao dịch nào. Hệ thống thông báo không có dữ liệu.  

**Postconditions**  
Lịch sử dòng tiền thu hộ được đối soát minh bạch giữa Shipper và Kế toán cửa hàng.  

---
