# PHÂN HỆ FE-08: XUẤT KHO & GIAO HÀNG THEO ĐỢT (DELIVERY & LOGISTICS)

---

### UC-089 Xem danh sách đơn chờ xuất kho (View Orders Awaiting Export)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to view and filter all confirmed orders waiting for warehouse picking and delivery note creation so that orders can be fulfilled in a timely manner.  

**Preconditions**  
Người dùng đã đăng nhập với quyền quản lý kho/bán hàng và có đơn hàng ở trạng thái `Đã xác nhận (Confirmed)`.  

**Normal Flow**  
*Xem danh sách các đơn hàng chờ soạn hàng xuất kho*  
1. Người dùng chọn mục "Đơn chờ xuất kho" (Orders Awaiting Export) trong Phân hệ Giao nhận & Xuất kho.  
2. Hệ thống truy vấn và hiển thị bảng các đơn hàng có trạng thái `Confirmed` hoặc `Partially Delivered` (còn mặt hàng chưa xuất hết):  
   - Bộ lọc: Ô tìm kiếm (Mã đơn, Tên khách, SĐT), Bộ lọc Kênh (Tại quầy / Online / Công trình), Lọc theo Ngày giao hẹn ước.  
   - Bảng dữ liệu gồm: Mã đơn hàng, Ngày đặt, Khách hàng, Địa chỉ giao hàng, Tổng số mặt hàng, Số lượng cần xuất, Trạng thái chuẩn bị, Cột Thao tác ("Lập phiếu xuất", "Xem chi tiết đơn").  
3. Người dùng lọc theo ngày hẹn giao hàng gần nhất.  
4. Người dùng nhấp vào nút "Lập phiếu xuất kho" (UC-090) tại đơn hàng cần xuất giao.  

**Alternative Flows**  
- **Không có đơn nào chờ xuất:**  
  1. Toàn bộ đơn hàng đã được lập phiếu xuất hoặc chưa có đơn mới.  
  2. Hệ thống hiển thị bảng trống kèm thông báo (MSG102: "Hiện tại không có đơn hàng nào chờ xuất kho").  

**Postconditions**  
Danh sách các đơn hàng cần soạn kho được hiển thị chính xác theo thời gian thực.  

---

### UC-090 Lập phiếu xuất/giao hàng (Create Delivery Note)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to create a Delivery Note for a confirmed order (specifying items, quantities to export, and assigned shipper) so that the warehouse can release the items and the driver can begin transport.  

**Preconditions**  
1. Đơn hàng mục tiêu đang ở trạng thái `Confirmed` hoặc đang giao dở dang (Partially Delivered).  
2. Hàng hóa trong kho có đủ số lượng giữ chỗ tương ứng.  

**Normal Flow**  
*Lập phiếu xuất kho giao hàng cho đơn hàng*  
1. Tại danh sách đơn chờ xuất, người dùng nhấp nút "Lập phiếu xuất kho".  
2. Hệ thống mở màn hình "Tạo phiếu xuất & giao hàng" gắn với Mã đơn hàng mục tiêu:  
   - Thông tin người nhận, Số điện thoại và Địa chỉ giao hàng (mặc định lấy từ đơn hàng).  
   - Danh sách mặt hàng trong đơn: Mã SKU, Tên sản phẩm, Số lượng đã đặt, Số lượng đã xuất trước đó, Số lượng còn lại cần xuất.  
   - Ô nhập "Số lượng xuất đợt này" (Mặc định điền bằng Số lượng còn lại).  
   - Dropdown chọn "Nhân viên giao hàng (Shipper)" phụ trách chuyển phát.  
   - Ô nhập "Ghi chú giao hàng" (ví dụ: "Giao trước tầng 1 và tầng 2 cho thợ lắp trước").  
3. Người dùng điều chỉnh số lượng xuất đợt này (nếu là giao từng phần cho công trình) và chọn Shipper được phân công.  
4. Người dùng nhấp nút "Lưu phiếu xuất".  
5. Hệ thống kiểm tra: Số lượng xuất mỗi dòng > 0 và <= Số lượng còn lại của đơn hàng; Đã chọn Shipper hợp lệ.  
6. Hệ thống tạo phiếu xuất kho mới với mã `PX-YYYYMMDD-XXXXX` ở trạng thái `Chờ giao (Pending Delivery)`.  
7. Nếu có sản phẩm quản lý theo Serial (FE-03), hệ thống chuyển hướng hoặc mở popup yêu cầu quét mã Serial (kích hoạt UC-093).  
8. Hệ thống cập nhật trạng thái đơn hàng sang `Đang chuẩn bị hàng / Đang giao`.  
9. Hệ thống ghi nhận sự kiện vào Audit Log.  
10. Hệ thống hiển thị thông báo thành công (MSG103: "Lập phiếu xuất kho thành công!") kèm tùy chọn "In phiếu xuất/giao hàng" (UC-091).  

**Alternative Flows**  
- **Step 5_Số lượng xuất vượt quá số lượng còn lại của đơn:**  
  1. Người dùng nhập số lượng xuất lớn hơn số lượng chưa giao của đơn.  
  2. Hệ thống báo lỗi (MSG104: "Số lượng xuất không được vượt quá số lượng còn lại của đơn hàng").  
- **Step 5.1_Chưa chọn nhân viên giao hàng:**  
  1. Bỏ trống ô phân công Shipper.  
  2. Hệ thống cảnh báo (MSG105: "Vui lòng chọn nhân viên giao hàng phụ trách đợt giao này").  

**Postconditions**  
1. Phiếu xuất kho được tạo lập và lưu vào cơ sở dữ liệu.  
2. Đợt giao hàng xuất hiện trên ứng dụng của nhân viên Shipper được phân công (UC-249).  

---

### UC-091 In phiếu xuất/giao hàng (Print Delivery Note)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to print physical Delivery Slips so that the warehouse worker can pick items and the shipper can hand them over to the customer with required delivery acknowledgments.  

**Preconditions**  
Phiếu xuất kho đã được tạo thành công trên hệ thống.  

**Normal Flow**  
*In biên bản giao hàng kiêm phiếu xuất kho ra giấy*  
1. Người dùng nhấp vào nút "In phiếu giao hàng" (Print Delivery Note) từ màn hình chi tiết phiếu xuất hoặc danh sách phiếu xuất.  
2. Hệ thống sinh bản in theo định dạng chuẩn khổ A4 hoặc A5:  
   - Tiêu đề: PHIẾU XUẤT KHO KIÊM BIÊN BẢN BÀN GIAO HÀNG HÓA.  
   - Thông tin cửa hàng: Logo, Tên cửa hàng, Địa chỉ, Hotline.  
   - Mã phiếu xuất, Mã đơn hàng, Ngày lập, Người lập, Nhân viên giao hàng (Shipper).  
   - Thông tin khách hàng & Địa chỉ công trình nhận hàng, SĐT người nhận.  
   - Bảng kê chi tiết sản phẩm: STT, Mã SKU, Tên thiết bị & biến thể, Đơn vị tính, Số lượng giao, Danh sách số Serial (nếu có), Ghi chú tình trạng hàng.  
   - Số tiền cần thu hộ (COD) (nếu có).  
   - Phần ký nhận: Người lập phiếu, Thủ kho xuất hàng, Nhân viên giao hàng, Người nhận hàng (Ký và ghi rõ họ tên).  
3. Hệ thống mở hộp thoại in của trình duyệt (Browser Print Preview).  
4. Người dùng chọn máy in và bấm "In" (Print).  
5. Phiếu được in ra giấy phục vụ công tác bốc dỡ và ký biên bản giao nhận.  

**Alternative Flows**  
- **Xuất phiếu ra file PDF:**  
  1. Người dùng chọn lưu dạng file PDF thay vì in trực tiếp ra máy in.  
  2. Trình duyệt lưu tệp `Phieu_Xuat_Kho_[MaPX].pdf` về máy tính.  

**Postconditions**  
Phiếu giao hàng vật lý được in ra phục vụ vận chuyển và kiểm đếm thực tế.  

---

### UC-092 Cập nhật/Hủy phiếu xuất hàng (Update/Cancel Delivery Note)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to edit or cancel a Delivery Note if it was created mistakenly before dispatching so that inventory holds and delivery tasks remain accurate.  

**Preconditions**  
1. Phiếu xuất kho đang ở trạng thái `Chờ giao (Pending Delivery)` (Shipper chưa ký nhận xuất kho hoặc xe chưa rời kho).  

**Normal Flow**  
*Hủy phiếu xuất hàng do sai sót thông tin trước khi đi giao*  
1. Người dùng mở chi tiết phiếu xuất kho cần hủy.  
2. Nhận thấy phiếu còn ở trạng thái `Pending Delivery`, hệ thống hiển thị nút "Hủy phiếu xuất".  
3. Người dùng nhấp nút "Hủy phiếu xuất".  
4. Hệ thống hiển thị hộp thoại xác nhận kèm ô nhập "Lý do hủy phiếu xuất" (bắt buộc).  
5. Người dùng nhập lý do (ví dụ: "Nhầm địa chỉ công trình", "Khách hẹn lùi lịch sang tuần sau") và nhấp "Xác nhận hủy".  
6. Hệ thống chuyển trạng thái phiếu xuất sang `Đã hủy (Cancelled)`.  
7. Nếu phiếu xuất đã gán mã Serial (UC-093), hệ thống tự động giải phóng các mã Serial này về trạng thái còn trong kho.  
8. Hệ thống điều chỉnh lại trạng thái tiến độ của đơn hàng gốc.  
9. Hệ thống ghi nhật ký hành động vào Audit Log.  
10. Hệ thống hiển thị thông báo thành công (MSG106: "Hủy phiếu xuất kho thành công!").  

**Alternative Flows**  
- **Phiếu xuất đã được Shipper nhận hoặc đã giao xong:**  
  1. Phiếu xuất đang ở trạng thái `Đang giao (In Transit)` hoặc `Đã giao (Delivered)`.  
  2. Nút "Hủy phiếu xuất" bị vô hiệu hóa. Hệ thống thông báo cảnh báo (MSG107: "Hàng đã xuất kho và đang đi giao, không thể hủy phiếu. Vui lòng xử lý qua luồng Đợt giao thất bại hoặc Đổi trả").  

**Postconditions**  
1. Phiếu xuất kho bị hủy bỏ, các mã Serial và tồn kho giữ chỗ được khôi phục.  
2. Audit Log ghi nhận lý do hủy phiếu xuất.  

---

### UC-093 Nhập số Serial khi xuất hàng (Record Serial Numbers)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to scan or input serial numbers for serialized items (e.g., smart bidet toilet, thermostatic shower valve) during picking so that exact units are tracked for warranties.  

**Preconditions**  
1. Phiếu xuất kho có ít nhất một sản phẩm được cấu hình theo dõi Serial (UC-030).  
2. Sản phẩm đang trong bước chuẩn bị xuất kho.  

**Normal Flow**  
*Quét mã vạch/nhập số Serial của thiết bị xuất kho*  
1. Trên giao diện phiếu xuất kho, tại các dòng sản phẩm bắt buộc Serial, hệ thống hiển thị ô nhập liệu: "Số Serial cần nhập: [X] / Đã nhập: [Y]".  
2. Người dùng sử dụng máy quét mã vạch (Barcode/QR Scanner) hoặc bàn phím để nhập số Serial của từng sản phẩm thực tế được lấy ra khỏi kệ kho.  
3. Người dùng nhấn Enter sau mỗi mã Serial.  
4. Hệ thống kiểm tra:  
   - Mã Serial có cấu trúc hợp lệ.  
   - Mã Serial này hiện đang có trong kho và thuộc đúng biến thể sản phẩm đang xuất.  
   - Mã Serial chưa từng được xuất bán trong bất kỳ đơn hàng nào khác.  
   - Không bị nhập trùng lặp trong cùng phiếu xuất này.  
5. Hệ thống thêm mã Serial vào danh sách đã gán của dòng sản phẩm và tăng biến đếm "Đã nhập".  
6. Người dùng quét đủ số lượng Serial theo số lượng cần xuất của mặt hàng.  
7. Người dùng nhấp "Xác nhận danh sách Serial".  
8. Hệ thống lưu liên kết các mã Serial với Phiếu xuất kho và Đơn hàng.  
9. Hệ thống hiển thị thông báo (MSG108: "Đã gán đầy đủ số Serial cho phiếu xuất kho!").  

**Alternative Flows**  
- **Step 4_Số Serial không tồn tại hoặc đã bán:**  
  1. Người dùng quét nhầm mã Serial đã xuất bán trước đó hoặc sai mã.  
  2. Hệ thống phát âm thanh cảnh báo lỗi (Beep) và hiển thị thông báo lỗi (MSG109: "Mã Serial [SerialNo] không hợp lệ hoặc đã được xuất bán trước đó!").  
  3. Mã lỗi bị loại bỏ, người dùng quét lại mã trên thân máy chính xác.  
- **Chưa nhập đủ số Serial theo số lượng hàng xuất:**  
  1. Số lượng xuất là 3 cái nhưng người dùng mới quét 2 số Serial và bấm xác nhận.  
  2. Hệ thống chặn lưu và thông báo (MSG110: "Chưa nhập đủ số lượng Serial theo số lượng xuất kho (Thiếu [X] mã)").  

**Postconditions**  
Từng thiết bị cụ thể được định danh chính xác bằng mã Serial gắn với phiếu xuất và đơn hàng.  

---

### UC-094 Tạo đợt giao hàng bổ sung (Create Additional Delivery Batch)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to create subsequent delivery batches for orders delivered in stages (especially large building projects) until all items are fully delivered.  

**Preconditions**  
1. Đơn hàng là đơn công trình hoặc đơn số lượng lớn có thỏa thuận giao nhiều đợt.  
2. Đơn hàng còn các mặt hàng chưa xuất giao đủ (Số lượng còn lại > 0).  

**Normal Flow**  
*Lên lịch và lập phiếu giao cho đợt tiếp theo của đơn hàng*  
1. Người dùng mở trang chi tiết đơn hàng (UC-047).  
2. Nhận thấy đơn hàng có trạng thái `Partially Delivered`, hệ thống hiển thị bảng tiến độ giao hàng và nút "Tạo đợt giao tiếp theo" (Create Next Batch).  
3. Người dùng nhấp vào nút "Tạo đợt giao tiếp theo".  
4. Hệ thống tự động tạo một đợt giao mới (Đợt 2, Đợt 3...) và hiển thị danh sách các sản phẩm kèm số lượng còn thiếu chưa giao.  
5. Người dùng nhập số lượng xuất cho đợt này và chỉ định Shipper.  
6. Người dùng nhấp nút "Xác nhận tạo đợt giao".  
7. Hệ thống sinh phiếu xuất kho mới gắn với đợt giao tiếp theo này.  
8. Hệ thống cập nhật bảng theo dõi đợt giao của đơn hàng.  
9. Hệ thống hiển thị thông báo (MSG111: "Đã tạo đợt giao hàng [Số đợt] thành công!").  

**Alternative Flows**  
- **Hàng trong kho chưa về đủ để giao đợt tiếp theo:**  
  1. Tồn kho khả dụng cho mặt hàng đợt 2 chưa đủ (chờ nhập hàng từ NCC).  
  2. Hệ thống cảnh báo thiếu tồn kho và gợi ý tạo đề xuất nhập hàng (UC-138).  

**Postconditions**  
Đợt giao hàng tiếp theo được kích hoạt và phân bổ cho nhân viên vận chuyển.  

---

### UC-095 Cập nhật kết quả giao hàng (Update Delivery Result)
**Primary Actors**  
Shipper, Sales Staff, Store Owner  

**Secondary Actors**  
Cloudinary, Customer  

**Description**  
As a Shipper, Sales Staff, or Store Owner, I want to update the delivery outcome (Delivered Successfully or Failed) with timestamp, customer signature, or proof-of-delivery photos so that the order reaches its final state.  

**Preconditions**  
1. Đợt giao hàng đang ở trạng thái `Đang giao hàng (In Transit)`.  
2. Nhân viên giao hàng đã đến địa chỉ khách hàng và thực hiện giao nhận.  

**Normal Flow**  
*Ghi nhận giao hàng thành công tại công trình*  
1. Shipper mở ứng dụng trên điện thoại, truy cập chi tiết đợt giao hàng đang thực hiện.  
2. Shipper nhấp nút "Cập nhật kết quả giao hàng".  
3. Hệ thống hiển thị biểu mẫu kết quả:  
   - Chọn kết quả: `Giao thành công (Delivered)` hoặc `Giao thất bại (Failed)`.  
   - Khu vực chụp ảnh bằng chứng giao hàng (Proof of Delivery - POD): Bắt buộc chụp ảnh kiện hàng đã hạ tại công trình/nhà khách hoặc ảnh khách ký biên bản giấy.  
   - Ô nhập Họ tên người thực nhận hàng.  
   - Ô nhập ghi chú giao hàng (ví dụ: "Đã giao đủ 2 bồn cầu lên tầng 2 cho anh Hùng, hàng nguyên đai nguyên kiện").  
4. Shipper chọn "Giao thành công", chụp ảnh tải lên và nhập tên người nhận.  
5. Shipper nhấp nút "Xác nhận giao thành công".  
6. Hệ thống gọi Secondary Actor `Cloudinary` lưu trữ ảnh bằng chứng giao hàng an toàn.  
7. Hệ thống cập nhật trạng thái đợt giao sang `Giao thành công (Delivered)`.  
8. Hệ thống tự động trừ số lượng tồn kho thực tế tương ứng (Deduct Stock on Export - liên kết UC-088).  
9. Hệ thống tự động kích hoạt tạo Hồ sơ bảo hành điện tử (Record Warranty Info - liên kết UC-099) cho các sản phẩm đã giao.  
10. Hệ thống cập nhật tiến độ giao tổng thể của đơn hàng: Nếu tất cả các món đã giao xong -> chuyển đơn hàng sang `Hoàn tất (Completed)`.  
11. Hệ thống ghi nhận sự kiện vào Audit Log.  
12. Hệ thống hiển thị thông báo thành công (MSG112: "Đã ghi nhận giao hàng thành công!").  

**Alternative Flows**  
- **Step 4.1_Giao hàng thất bại:**  
  1. Shipper chọn kết quả "Giao thất bại".  
  2. Kích hoạt luồng Xử lý đợt giao thất bại (UC-096).  
- **Step 5.1_Chưa chụp ảnh bằng chứng giao hàng:**  
  1. Shipper bấm xác nhận giao thành công nhưng chưa có ảnh chụp.  
  2. Hệ thống cảnh báo (MSG113: "Bắt buộc chụp ảnh bằng chứng giao hàng (POD) để xác nhận hoàn tất giao hàng").  

**Postconditions**  
1. Đợt giao hàng chuyển sang trạng thái `Delivered`.  
2. Tồn kho thực tế bị trừ.  
3. Hồ sơ bảo hành điện tử được tự động khởi tạo.  

---

### UC-096 Xử lý đợt giao thất bại (Handle Failed Delivery)
**Primary Actors**  
Shipper, Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Shipper or Sales Staff, I want to record delivery failure reasons (Customer unreachable, Refused acceptance due to damage, Wrong address) and decide whether to reschedule a re-delivery or return items back to warehouse stock.  

**Preconditions**  
Đợt giao hàng không thể hoàn tất thành công tại hiện trường.  

**Normal Flow**  
*Ghi nhận lý do giao thất bại và chuyển hàng về kho*  
1. Tại biểu mẫu cập nhật kết quả giao hàng (UC-095), Shipper chọn "Giao thất bại".  
2. Hệ thống hiển thị các trường yêu cầu bắt buộc:  
   - Lý do giao hàng thất bại (Dropdown): `Không liên lạc được khách hàng (gọi 3 lần)`, `Khách từ chối nhận (hàng trầy xước/vỡ)`, `Sai địa chỉ giao hàng`, `Đường công trình cấm tải không vào được`, `Khách dời lịch giao`.  
   - Ô nhập mô tả chi tiết lý do.  
   - Ảnh chụp hiện trường / ảnh sản phẩm bị sự cố (nếu do vỡ hỏng).  
   - Lựa chọn hướng xử lý tiếp theo: (1) "Lên lịch giao lại đợt sau (Reschedule)"; (2) "Trả hàng về kho và giải phóng tồn (Return to warehouse)".  
3. Shipper nhập đầy đủ lý do và chọn phương án "Trả hàng về kho".  
4. Shipper nhấp "Xác nhận kết quả".  
5. Hệ thống chuyển trạng thái đợt giao sang `Giao thất bại (Delivery Failed)`.  
6. Hệ thống tạo phiếu nhập hoàn hàng về kho (Warehouse Return Slip) để thủ kho kiểm đếm khi Shipper mang hàng về đến cửa hàng.  
7. Tồn kho thực tế không bị trừ; lượng giữ chỗ được giải phóng hoặc bảo lưu tùy phương án.  
8. Hệ thống gửi thông báo cho Nhân viên bán hàng phụ trách đơn hàng để liên hệ ngay với khách hàng.  
9. Hệ thống ghi nhật ký sự kiện vào Audit Log.  

**Alternative Flows**  
- **Chọn phương án "Lên lịch giao lại":**  
  1. Shipper chọn giao lại vào ngày hôm sau.  
  2. Hệ thống giữ nguyên trạng thái giữ chỗ kho và tạo một lịch hẹn giao mới vào ngày hôm sau trên hệ thống.  

**Postconditions**  
Đợt giao thất bại được ghi nhận minh bạch, bảo vệ tài sản hàng hóa của cửa hàng và phân định rõ trách nhiệm.  

---

### UC-097 Theo dõi số lượng đã giao & còn lại (Track Delivered Quantity)
**Primary Actors**  
Sales Staff, Store Owner, Customer  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, Store Owner, or Customer, I want to track item-level delivery progress (ordered, dispatched, delivered, remaining) across multiple batches so that no project equipment is omitted or double-delivered.  

**Preconditions**  
Đơn hàng đã phát sinh ít nhất một đợt giao hàng.  

**Normal Flow**  
*Theo dõi bảng tiến độ giao hàng chi tiết theo từng sản phẩm*  
1. Người dùng mở trang Chi tiết đơn hàng và chọn tab "Tiến độ giao hàng" (Delivery Tracking).  
2. Hệ thống hiển thị Bảng ma trận giao hàng (Delivery Matrix Table) gồm các cột:  
   - Tên sản phẩm & Biến thể.  
   - Số lượng đặt mua (Ordered).  
   - Số lượng đã xuất kho (Dispatched).  
   - Số lượng đã giao thành công (Delivered).  
   - Số lượng còn lại phải giao (Remaining = Ordered - Delivered).  
   - Tỷ lệ phần trăm hoàn thành (Thanh Progress Bar trực quan).  
3. Bên dưới bảng, hệ thống hiển thị danh sách các đợt giao hàng (Batch 1, Batch 2...) kèm thông tin: Ngày giao, Phiếu xuất kho liên kết, Người giao, Trạng thái đợt giao.  
4. Người dùng theo dõi và nhấp vào từng đợt giao để xem chi tiết biên bản bàn giao.  

**Alternative Flows**  
- **Tất cả các sản phẩm đã giao 100%:**  
  1. Mọi dòng sản phẩm đều có Remaining = 0.  
  2. Bảng hiển thị huy hiệu xanh lá "Đã hoàn thành bàn giao toàn bộ 100%".  

**Postconditions**  
Số liệu giao hàng hiển thị chuẩn xác, ngăn ngừa hoàn toàn nguy cơ thất thoát hoặc giao thiếu/thừa thiết bị.  

---

### UC-098 Xem lịch sử giao hàng (View Delivery History)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to view the consolidated chronological dispatch log of an order (all delivery slips, drivers, timestamps, and signed proofs) so that I can resolve disputes.  

**Preconditions**  
Người dùng đã đăng nhập và đang mở chi tiết một đơn hàng.  

**Normal Flow**  
*Xem dòng thời gian lịch sử tất cả các lần xuất giao của một đơn hàng*  
1. Người dùng nhấp vào tab "Lịch sử giao nhận" (Dispatch History Timeline) của đơn hàng.  
2. Hệ thống truy vấn và hiển thị dòng thời gian tuần tự của toàn bộ các sự kiện giao hàng:  
   - Ngày giờ lập phiếu xuất, người lập phiếu xuất.  
   - Thời điểm Shipper nhận hàng rời kho.  
   - Thời điểm và tọa độ giao hàng tại công trình.  
   - Ảnh chụp biên bản giao nhận có chữ ký người nhận.  
   - Người ký nhận và số điện thoại đối chiếu.  
3. Người dùng có thể nhấp vào ảnh để phóng to xem chi tiết chữ ký và hiện trạng hàng hóa lúc bàn giao.  

**Alternative Flows**  
- **Đơn hàng chưa từng xuất giao:**  
  1. Đơn hàng mới tạo, chưa có phiếu xuất.  
  2. Hệ thống hiển thị thông báo "Đơn hàng chưa có lịch sử xuất giao nào".  

**Postconditions**  
Toàn bộ căn cứ pháp lý và lịch sử vận chuyển được hiển thị minh bạch.  

---

### UC-135 Yêu cầu xuất/giao hàng theo đợt công trình (Request Delivery Batch)
**Primary Actors**  
Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, I want to schedule and submit a warehouse dispatch request for a specific construction batch based on the agreed progress timeline with the contractor/homeowner.  

**Preconditions**  
1. Đơn hàng công trình đang có hiệu lực.  
2. Chủ thầu hoặc khách hàng gọi điện thông báo hạng mục công trình đã sẵn sàng nhận thiết bị (ví dụ: đã ốp lát xong phòng tắm, cần giao bồn cầu và chậu lavabo).  

**Normal Flow**  
*Nhân viên bán hàng đặt lịch yêu cầu xuất hàng theo tiến độ công trình*  
1. Nhân viên mở chi tiết đơn hàng công trình, nhấp nút "Yêu cầu giao hàng theo đợt".  
2. Hệ thống hiển thị biểu mẫu lên lịch giao:  
   - Chọn ngày hẹn giao mong muốn và khung giờ (Sáng / Chiều).  
   - Danh sách thiết bị công trình còn tồn đọng: Nhân viên tích chọn các sản phẩm và số lượng cần giao cho đợt này.  
   - Thông tin người nhận tại công trình (Tên cai thầu, SĐT, Ghi chú bốc xếp lên tầng).  
3. Nhân viên nhấp nút "Gửi yêu cầu xuất kho".  
4. Hệ thống kiểm tra số lượng yêu cầu hợp lệ và lưu bản ghi Yêu cầu xuất kho ở trạng thái `Đã lên lịch (Scheduled)`.  
5. Hệ thống gửi thông báo nhắc việc đến Bộ phận Kho để chuẩn bị đóng gói hàng hóa trước giờ giao.  
6. Hệ thống hiển thị thông báo thành công (MSG114: "Đã lên lịch yêu cầu xuất hàng đợt mới thành công!").  

**Alternative Flows**  
- **Chọn ngày giao trong quá khứ:**  
  1. Nhân viên chọn ngày giao nhỏ hơn ngày hiện tại.  
  2. Hệ thống cảnh báo (MSG115: "Ngày hẹn giao hàng phải từ hôm nay trở đi").  

**Postconditions**  
Lịch giao hàng đợt mới được xếp vào hàng đợi điều phối kho và giao vận.  

---

### UC-201 Xem lịch sử xuất hàng (View Export History)
**Primary Actors**  
Store Owner, Accountant, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, Accountant, or Sales Staff, I want to review the global export history log across all orders (items exported, quantities, timestamps, handlers) so that warehouse outflows are fully reconcilable with stock levels.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống và có quyền xem báo cáo kho.  

**Normal Flow**  
*Xem và trích xuất lịch sử xuất kho tổng thể toàn cửa hàng*  
1. Người dùng chọn mục "Lịch sử xuất hàng" (Export Log) trong Quản lý kho.  
2. Hệ thống hiển thị bảng dữ liệu lịch sử xuất kho tổng hợp:  
   - Bộ lọc: Khoảng ngày xuất hàng, Mã sản phẩm/SKU, Nhân viên xuất kho, Mã đơn hàng liên quan.  
   - Bảng danh sách: Mã phiếu xuất, Ngày giờ xuất, Mã đơn hàng, Tên sản phẩm, Biến thể, Số lượng xuất, Thủ kho xuất, Shipper nhận, Trạng thái.  
3. Người dùng lọc theo tháng hiện tại để đối soát.  
4. Người dùng có thể nhấp nút "Xuất file Excel" để tải toàn bộ bảng lịch sử ra máy tính phục vụ kế toán kiểm kho.  

**Alternative Flows**  
- **Lọc không có dữ liệu:**  
  1. Không có phiếu xuất nào trong khoảng thời gian lọc.  
  2. Hệ thống hiển thị bảng trống kèm thông báo thích hợp.  

**Postconditions**  
Dữ liệu xuất kho toàn hệ thống được đối soát minh bạch.  

---

### UC-249 Xem danh sách đợt giao được phân công (View Assigned Deliveries)
**Primary Actors**  
Shipper  

**Secondary Actors**  
None  

**Description**  
As a Shipper, I want to view, search, and filter deliveries assigned to me by date and status so that I can organize my delivery routes efficiently.  

**Preconditions**  
Nhân viên giao hàng đã đăng nhập bằng tài khoản `Shipper` trên điện thoại di động hoặc trình duyệt.  

**Normal Flow**  
*Shipper xem các đơn hàng và đợt giao thuộc trách nhiệm của mình*  
1. Shipper mở màn hình "Đợt giao của tôi" (My Deliveries).  
2. Hệ thống truy vấn các phiếu giao hàng được phân công cho Shipper hiện tại:  
   - Các tab trạng thái: `Cần giao hôm nay` (Today's Tasks), `Đang giao` (In Transit), `Đã hoàn thành` (Completed), `Giao thất bại` (Failed).  
   - Danh sách thẻ đơn giao: Mã phiếu xuất, Tên khách hàng, Số điện thoại người nhận, Địa chỉ giao hàng, Số tiền cần thu COD (in đậm màu đỏ), Số lượng kiện hàng.  
3. Mặc định hệ thống sắp xếp các đơn cần giao theo tuyến đường gần nhau hoặc theo giờ hẹn giao ưu tiên.  
4. Shipper nhấp vào một đợt giao để xem chi tiết lộ trình và sản phẩm (UC-250).  

**Alternative Flows**  
- **Không có đợt giao nào được phân công:**  
  1. Hôm nay Shipper chưa được giao chuyến nào.  
  2. Màn hình hiển thị: "Hôm nay bạn chưa có chuyến giao hàng nào. Hãy nghỉ ngơi hoặc liên hệ điều phối kho!".  

**Postconditions**  
Shipper nắm rõ toàn bộ nhiệm vụ giao hàng trong ngày của mình.  

---

### UC-250 Xem chi tiết đợt giao (View Delivery Details)
**Primary Actors**  
Shipper  

**Secondary Actors**  
None  

**Description**  
As a Shipper, I want to view recipient name, phone number, destination address, items, quantities, special delivery notes, and COD amount so that I can deliver accurately.  

**Preconditions**  
Shipper đã mở một đợt giao từ danh sách (UC-249).  

**Normal Flow**  
*Xem chi tiết đợt giao hàng trên thiết bị di động*  
1. Shipper nhấp vào thẻ đợt giao hàng.  
2. Hệ thống hiển thị chi tiết đợt giao:  
   - Khối thông tin khách hàng: Tên người nhận, Nút bấm gọi điện thoại nhanh (`Call Now`), Địa chỉ chi tiết kèm nút bấm mở ứng dụng Google Maps chỉ đường (`Navigate`).  
   - Khối tiền thu hộ: Số tiền COD chính xác cần thu từ khách (hoặc "Đã thanh toán trước 100% - Không thu tiền").  
   - Khối danh sách sản phẩm: Tên thiết bị, số lượng kiện, lưu ý dễ vỡ (Đồ sứ vệ sinh bồn cầu, kính buồng tắm).  
   - Ghi chú giao hàng: Lối vào ngõ nhỏ xe ba gác, giờ chủ nhà có nhà.  
   - Nút hành động: "Xác nhận nhận hàng rời kho" (UC-251) và "Cập nhật kết quả giao hàng" (UC-095).  
3. Shipper kiểm tra và bắt đầu hành trình giao hàng.  

**Alternative Flows**  
- **Khách hàng thanh toán chuyển khoản thay vì tiền mặt COD:**  
  1. Khách muốn chuyển khoản khi Shipper giao đến.  
  2. Shipper bấm nút "Hiển thị mã VietQR thu tiền" trên màn hình để khách quét chuyển tiền trực tiếp vào tài khoản cửa hàng.  

**Postconditions**  
Toàn bộ thông tin cần thiết phục vụ giao hàng được cung cấp đầy đủ trên thiết bị của Shipper.  

---

### UC-251 Xác nhận nhận hàng để giao (Confirm Goods Handover)
**Primary Actors**  
Shipper  

**Secondary Actors**  
None  

**Description**  
As a Shipper, I want to reconcile physical goods with the delivery note and confirm item custody before leaving the store/warehouse so that transport liability is clearly established.  

**Preconditions**  
1. Phiếu xuất kho đang ở trạng thái `Chờ giao (Pending Delivery)`.  
2. Shipper và Thủ kho đang có mặt tại khu vực xuất hàng của kho để bốc hàng lên xe.  

**Normal Flow**  
*Shipper kiểm đếm và ký nhận bàn giao hàng hóa từ kho*  
1. Shipper kiểm tra thực tế số lượng thùng hàng, mã sản phẩm trên xe đối chiếu với danh sách trong phiếu giao hàng trên ứng dụng.  
2. Kiểm tra hàng hóa nguyên đai nguyên kiện, không nứt vỡ sứ hay móp méo hộp phụ kiện.  
3. Shipper nhấp nút "Xác nhận nhận hàng từ kho" (Confirm Goods Handover).  
4. Hệ thống hiển thị hộp thoại xác nhận: "Bạn xác nhận đã kiểm tra đủ [X] kiện hàng nguyên vẹn và chịu trách nhiệm vận chuyển?".  
5. Shipper nhấp "Đồng ý nhận hàng".  
6. Hệ thống chuyển trạng thái đợt giao sang `Đang vận chuyển (In Transit)`.  
7. Hệ thống ghi nhận thời gian nhận hàng và người nhận chịu trách nhiệm vào nhật ký vận chuyển.  
8. Hệ thống hiển thị thông báo thành công (MSG116: "Đã xác nhận nhận hàng! Chúc bạn có chuyến đi an toàn") và chuyển sang giao diện điều hướng giao hàng.  

**Alternative Flows**  
- **Phát hiện hàng bị móp méo hoặc thiếu hàng khi kiểm kho:**  
  1. Hàng hóa thực tế bị nứt vỡ hoặc thiếu số lượng.  
  2. Shipper từ chối bấm nhận hàng và báo thủ kho đổi thùng hàng khác đạt chuẩn trước khi ký nhận.  

**Postconditions**  
Trách nhiệm bảo quản hàng hóa trên đường đi chính thức thuộc về Shipper cho đến khi bàn giao cho khách hàng.  

---

### UC-252 Xem lịch sử giao hàng cá nhân (View My Delivery History)
**Primary Actors**  
Shipper  

**Secondary Actors**  
None  

**Description**  
As a Shipper, I want to view my historical delivery records, results, completed trips, and timestamps so that I can verify my delivery counts and performance allowance.  

**Preconditions**  
Shipper đã đăng nhập vào hệ thống.  

**Normal Flow**  
*Xem thống kê và lịch sử các chuyến giao hàng đã hoàn thành của cá nhân*  
1. Shipper chọn mục "Lịch sử giao hàng" trong ứng dụng.  
2. Hệ thống hiển thị trang tổng hợp thành tích giao hàng cá nhân:  
   - Thống kê tóm tắt: Tổng số chuyến đã giao thành công trong tháng, Tỷ lệ giao thành công (ví dụ: 98%), Tổng số tiền COD đã thu hộ.  
   - Danh sách các đợt giao đã hoàn tất sắp xếp theo ngày: Mã phiếu xuất, Khách hàng, Địa chỉ, Thời gian hoàn thành, Kết quả giao (Thành công / Thất bại).  
3. Shipper lọc theo tuần hoặc tháng để xem lại các chuyến đi phục vụ tính lương và phụ cấp vận chuyển.  

**Alternative Flows**  
- **Xem chi tiết một chuyến giao cũ:**  
  1. Shipper nhấp vào một dòng lịch sử để xem lại ảnh bằng chứng giao hàng (POD) và chữ ký khách hàng đã lưu.  

**Postconditions**  
Thông tin thành tích giao hàng của Shipper được hiển thị minh bạch và chính xác.  

---
