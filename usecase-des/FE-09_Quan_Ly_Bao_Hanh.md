# PHÂN HỆ FE-09: QUẢN LÝ BẢO HÀNH (WARRANTY MANAGEMENT)

---

### UC-099 Tự động ghi nhận thông tin bảo hành (Record Warranty Info)
**Primary Actors**  
System  

**Secondary Actors**  
SMS Service, Email Service  

**Description**  
As a System, I want to automatically initialize electronic warranty records (customer info, product variant, order reference, serial numbers, warranty duration, start and end dates) upon successful delivery or counter sale so that warranty coverage is guaranteed without manual data entry.  

**Preconditions**  
1. Đơn hàng được xác nhận giao thành công (từ UC-095) hoặc đơn hàng tại quầy được thanh toán hoàn tất (UC-048).  
2. Sản phẩm trong đơn có áp dụng chính sách bảo hành (thời hạn bảo hành > 0 tháng).  

**Normal Flow**  
*Hệ thống tự động kích hoạt thẻ bảo hành điện tử*  
1. Hệ thống tiếp nhận sự kiện giao hàng thành công hoặc thanh toán đơn hàng thành công.  
2. Đối với từng sản phẩm có bảo hành trong đơn hàng:  
   - Hệ thống đọc Chính sách bảo hành áp dụng (UC-199) của biến thể sản phẩm: Số tháng bảo hành (ví dụ: Bồn cầu 10 năm phần sứ, 3 năm phụ kiện xả; Sen cây 3 năm).  
   - Tính toán Ngày bắt đầu bảo hành = Ngày giao hàng/Ngày mua.  
   - Tính toán Ngày hết hạn bảo hành = Ngày bắt đầu + Số tháng bảo hành.  
   - Lấy mã Serial của thiết bị (đã quét ở UC-093, nếu có).  
3. Hệ thống tạo một bản ghi Thẻ bảo hành điện tử (E-Warranty Record) mới trong cơ sở dữ liệu:  
   - Mã thẻ bảo hành: Tự động sinh `BH-YYYYMMDD-XXXXX`.  
   - Khóa ngoại liên kết: `CustomerID`, `OrderID`, `ProductID`, `VariantID`, `SerialNo`.  
   - Trạng thái bảo hành: `Đang có hiệu lực (Active)`.  
4. Hệ thống kích hoạt Secondary Actor `SMS Service` hoặc `Email Service` gửi thông báo kích hoạt bảo hành điện tử kèm đường dẫn tra cứu tới khách hàng.  
5. Hệ thống hoàn tất kích hoạt bảo hành nền trong cùng giao dịch cơ sở dữ liệu.  

**Alternative Flows**  
- **Sản phẩm không có bảo hành (Hàng tiêu hao/linh kiện bán lẻ):**  
  1. Sản phẩm có thời hạn bảo hành = 0 tháng.  
  2. Hệ thống bỏ qua không tạo thẻ bảo hành cho sản phẩm đó.  

**Postconditions**  
1. Thẻ bảo hành điện tử được lưu chính thức trong hệ thống.  
2. Khách hàng và nhân viên có thể tra cứu thông tin bảo hành bất cứ lúc nào.  

---

### UC-100 Tra cứu hồ sơ bảo hành (Search Warranty Record)
**Primary Actors**  
Customer, Guest, Sales Staff, Technical Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As any user, I want to search and look up warranty records by customer phone number, order number, or product serial number so that warranty coverage and expiration dates can be verified instantly.  

**Preconditions**  
Người dùng truy cập vào trang tra cứu bảo hành (trang công khai đối với khách hoặc trang nội bộ đối với nhân viên).  

**Normal Flow**  
*Tra cứu thẻ bảo hành theo tiêu chí linh hoạt*  
1. Người dùng nhập một trong các thông tin tra cứu vào ô tìm kiếm:  
   - Số điện thoại của khách hàng (ví dụ: `0987654321`).  
   - Mã đơn hàng (ví dụ: `ORD-20261008-001`).  
   - Số Serial in trên thân thiết bị vệ sinh (ví dụ: `SN-TOTO-885-9921`).  
2. Người dùng nhấp nút "Tra cứu bảo hành".  
3. Hệ thống truy vấn cơ sở dữ liệu và hiển thị kết quả:  
   - Thông tin khách hàng (được ẩn bớt số điện thoại nếu là trang công khai để bảo mật).  
   - Tên sản phẩm, hình ảnh, mã biến thể, số Serial.  
   - Ngày mua hàng & Ngày kích hoạt bảo hành.  
   - Thời hạn bảo hành và Ngày hết hạn bảo hành.  
   - Trạng thái hiệu lực bảo hành (Huy hiệu xanh: "Còn hạn bảo hành [X] ngày"; Huy hiệu xám: "Đã hết hạn bảo hành").  
   - Lịch sử các lần bảo hành/sửa chữa trước đây của sản phẩm này.  
4. Người dùng xem thông tin đối soát.  

**Alternative Flows**  
- **Không tìm thấy dữ liệu bảo hành:**  
  1. Thông tin nhập không khớp với bất kỳ bản ghi nào trong hệ thống.  
  2. Hệ thống hiển thị thông báo (MSG117: "Không tìm thấy thông tin bảo hành phù hợp. Quý khách vui lòng kiểm tra lại Số điện thoại, Mã đơn hàng hoặc Số Serial").  

**Postconditions**  
Thông tin bảo hành điện tử được hiển thị chính xác và nhanh chóng.  

---

### UC-101 Xác minh điều kiện bảo hành (Verify Warranty Eligibility)
**Primary Actors**  
Sales Staff, Technical Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Technical Staff, I want to verify warranty eligibility (purchase date, active duration, damage condition according to manufacturer warranty policy) before accepting a product for repair.  

**Preconditions**  
1. Khách hàng mang sản phẩm đến cửa hàng hoặc gửi yêu cầu bảo hành.  
2. Nhân viên đã mở thông tin thẻ bảo hành của sản phẩm (UC-100).  

**Normal Flow**  
*Kiểm tra tính hợp lệ về mặt chính sách và điều kiện bảo hành*  
1. Nhân viên đối chiếu thông tin sản phẩm thực tế với thông tin trên Thẻ bảo hành điện tử:  
   - So khớp mã Serial dập trên thân sản phẩm với Serial trên hệ thống.  
   - Kiểm tra ngày hết hạn: Ngày hiện tại <= Ngày hết hạn bảo hành.  
   - Kiểm tra tem niêm phong và tình trạng vật lý của thiết bị.  
2. Nhân viên nhấp vào nút "Xác minh điều kiện bảo hành".  
3. Hệ thống hiển thị bảng kiểm tra điều kiện (Eligibility Checklist):  
   - [x] Sản phẩm còn trong thời hạn bảo hành.  
   - [ ] Tem bảo hành / tem niêm phong còn nguyên vẹn, không bị cạo sửa.  
   - [ ] Hỏng hóc do lỗi kỹ thuật của nhà sản xuất (không phải do rơi vỡ, chuột cắn, nguồn nước phèn chua ăn mòn).  
   - [ ] Thiết bị chưa từng bị tháo dỡ sửa chữa bởi đơn vị thứ 3 không ủy quyền.  
4. Nhân viên tích chọn các tiêu chí thỏa mãn.  
5. Nếu tất cả các tiêu chí hợp lệ: Hệ thống ghi nhận sản phẩm "Đủ điều kiện bảo hành miễn phí" và cho phép chuyển tiếp sang bước Tạo yêu cầu bảo hành (UC-102).  

**Alternative Flows**  
- **Sản phẩm đã hết hạn bảo hành:**  
  1. Ngày hiện tại lớn hơn Ngày hết hạn bảo hành.  
  2. Hệ thống cảnh báo màu đỏ (MSG118: "Sản phẩm đã hết hạn bảo hành chính hãng từ ngày [DD/MM/YYYY]").  
  3. Nhân viên tư vấn cho khách hàng chuyển sang hình thức Sửa chữa dịch vụ có tính phí.  
- **Sản phẩm bị từ chối do vi phạm điều kiện (Lỗi ngoại lực/nguồn nước):**  
  1. Sản phẩm bị nứt vỡ sứ do va đập hoặc ngập nước bẩn.  
  2. Nhân viên bỏ chọn tiêu chí lỗi kỹ thuật, nhập lý do từ chối cụ thể.  
  3. Hệ thống ghi nhận kết luận không đủ điều kiện bảo hành miễn phí.  

**Postconditions**  
Kết quả thẩm định điều kiện bảo hành được xác lập làm căn cứ mở phiếu dịch vụ.  

---

### UC-102 Tạo yêu cầu bảo hành (Create Warranty Request)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
Customer, Cloudinary  

**Description**  
As a Sales Staff or Store Owner, I want to create a formal Warranty Claim Ticket for an eligible product so that the technical department can inspect and repair it.  

**Preconditions**  
1. Sản phẩm đã được xác minh đủ điều kiện bảo hành (UC-101).  
2. Khách hàng yêu cầu bảo hành tại quầy hoặc nhân viên tiếp nhận qua hotline.  

**Normal Flow**  
*Lập phiếu yêu cầu tiếp nhận bảo hành thiết bị vệ sinh*  
1. Nhân viên nhấp nút "Tạo phiếu tiếp nhận bảo hành" (Create Warranty Ticket).  
2. Hệ thống tự động kế thừa thông tin từ Thẻ bảo hành: Tên khách hàng, SĐT, Tên sản phẩm, Mã SKU, Mã Serial, Ngày hết hạn bảo hành.  
3. Nhân viên nhập các thông tin hiện trường:  
   - Hiện tượng lỗi do khách mô tả (ví dụ: "Bồn cầu không bơm nước lên két, van xả bị rò nước liên tục").  
   - Hiện trạng thiết bị lúc tiếp nhận (ví dụ: "Thân vỏ còn mới, đủ phụ kiện nắp và dây cấp").  
   - Hình thức bảo hành: `Tại cửa hàng` (khách mang đến quầy) hoặc `Tại nhà khách / Công trình` (cần cử kỹ thuật viên đến tận nơi).  
   - Ảnh chụp thiết bị lỗi và tem bảo hành.  
4. Nhân viên nhấp nút "Lưu phiếu tiếp nhận".  
5. Hệ thống gọi Secondary Actor `Cloudinary` tải ảnh chụp hiện trạng lên kho lưu trữ.  
6. Hệ thống tạo phiếu bảo hành mới với mã `BH-TICKET-XXXXX`, trạng thái ban đầu: `Tiếp nhận (Received)`.  
7. Hệ thống tự động gửi thông báo SMS/Email xác nhận mã phiếu tiếp nhận tới khách hàng.  
8. Hệ thống ghi nhận sự kiện vào Audit Log.  
9. Hệ thống hiển thị thông báo thành công (MSG119: "Tạo phiếu tiếp nhận bảo hành thành công!") kèm tùy chọn "In phiếu tiếp nhận bảo hành" (UC-136).  

**Alternative Flows**  
- **Bỏ trống mô tả hiện tượng lỗi:**  
  1. Nhân viên không nhập mô tả lỗi của thiết bị.  
  2. Hệ thống cảnh báo (MSG02: "Vui lòng nhập mô tả hiện tượng hư hỏng").  

**Postconditions**  
1. Phiếu tiếp nhận bảo hành được lưu trữ trong hệ thống ở trạng thái `Received`.  
2. Công việc được đưa vào danh sách chờ xử lý của phòng kỹ thuật (UC-248).  

---

### UC-103 Cập nhật trạng thái bảo hành (Update Warranty Status)
**Primary Actors**  
Technical Staff, Sales Staff, Store Owner  

**Secondary Actors**  
SMS Service, Email Service  

**Description**  
As a Technical Staff, Sales Staff, or Store Owner, I want to advance the warranty claim through its lifecycle states (Received -> Inspecting -> In Progress -> Ready for Handover -> Completed / Rejected) with timestamps and technical conclusions.  

**Preconditions**  
Người dùng có thẩm quyền mở một phiếu yêu cầu bảo hành đang hoạt động.  

**Normal Flow**  
*Cập nhật tiến độ xử lý quy trình bảo hành*  
1. Người dùng mở trang chi tiết phiếu yêu cầu bảo hành (UC-104).  
2. Người dùng nhấp chọn trạng thái tiếp theo theo chu trình:  
   - `Đang kiểm tra (Inspecting)`: Kỹ thuật viên bắt đầu đo đạc, kiểm tra linh kiện.  
   - `Đang xử lý (In Progress)`: Đang thay thế linh kiện hoặc chờ phụ tùng từ hãng sản xuất.  
   - `Chờ bàn giao (Ready for Handover)`: Thiết bị đã sửa xong, test đạt chuẩn, sẵn sàng trả khách.  
   - `Hoàn tất (Completed)`: Đã bàn giao lại thiết bị cho khách (thực hiện qua UC-137).  
3. Người dùng nhập ghi chú nội dung công việc tương ứng với trạng thái mới.  
4. Người dùng nhấp nút "Lưu cập nhật trạng thái".  
5. Hệ thống kiểm tra tính hợp lệ của luồng chuyển đổi trạng thái (State Machine Validation).  
6. Hệ thống cập nhật trạng thái mới và lưu dấu thời gian thực hiện.  
7. Hệ thống kích hoạt gửi tin nhắn SMS/Email tự động thông báo tiến độ cho khách hàng (đặc biệt khi chuyển sang `Ready for Handover` để khách đến nhận lại đồ).  
8. Hệ thống hiển thị thông báo thành công (MSG120: "Cập nhật trạng thái bảo hành thành công!").  

**Alternative Flows**  
- **Chuyển sang trạng thái Từ chối (Rejected):**  
  1. Sau khi tháo máy, phát hiện thiết bị bị can thiệp trái phép.  
  2. Người dùng chọn trạng thái `Từ chối bảo hành (Rejected)` kèm bắt buộc nhập lý do và bằng chứng ảnh chụp.  

**Postconditions**  
Trạng thái phiếu bảo hành được đồng bộ và phản ánh chính xác tiến độ thực tế.  

---

### UC-104 Xem chi tiết yêu cầu bảo hành (View Warranty Request Detail)
**Primary Actors**  
Technical Staff, Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Technical Staff, Sales Staff, or Store Owner, I want to view all details of a warranty claim (customer info, product, reported issue, inspection results, replaced parts, timeline, technician notes) to coordinate service fulfillment.  

**Preconditions**  
Người dùng đã đăng nhập và nhấp vào một phiếu bảo hành trong danh sách (UC-105).  

**Normal Flow**  
*Xem hồ sơ chi tiết toàn diện của một phiếu bảo hành*  
1. Hệ thống điều hướng đến trang "Chi tiết phiếu bảo hành - [Mã BH]":  
   - Khối 1: Thông tin khách hàng & Thông tin liên hệ (Tên, SĐT, Địa chỉ).  
   - Khối 2: Thông tin thiết bị vệ sinh (Tên thiết bị, SKU, Serial, Thẻ bảo hành gốc, Đơn hàng gốc).  
   - Khối 3: Mô tả lỗi ban đầu & Ảnh chụp lúc tiếp nhận.  
   - Khối 4: Kết quả kiểm tra kỹ thuật (Kỹ thuật viên phụ trách, Nguyên nhân lỗi, Linh kiện đã thay thế, Chi phí bảo hành cửa hàng chịu).  
   - Khối 5: Lịch sử tiến trình thời gian (Timeline: Ngày tiếp nhận -> Ngày kiểm tra -> Ngày hoàn tất sửa chữa -> Ngày trả hàng).  
   - Khối 6: Ghi chú nội bộ giữa nhân viên bán hàng và nhân viên kỹ thuật.  
2. Người dùng xem thông tin và có thể nhấp các nút tác vụ (Cập nhật trạng thái, In biên nhận, Bàn giao).  

**Alternative Flows**  
- **Không tìm thấy phiếu bảo hành:**  
  1. Mã phiếu bị xóa hoặc URL không tồn tại.  
  2. Hệ thống hiển thị thông báo lỗi và quay về danh sách phiếu bảo hành.  

**Postconditions**  
Thông tin chi tiết phiếu bảo hành được hiển thị trực quan và đầy đủ.  

---

### UC-105 Xem danh sách yêu cầu bảo hành (View Warranty Request List)
**Primary Actors**  
Technical Staff, Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Technical Staff, Sales Staff, or Store Owner, I want to view, search, and filter warranty claims by status, customer phone, serial number, and handling technician so that no claim is left overdue.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống.  

**Normal Flow**  
*Xem và quản lý danh sách toàn bộ các phiếu yêu cầu bảo hành*  
1. Người dùng chọn menu "Quản lý bảo hành" -> "Danh sách phiếu bảo hành".  
2. Hệ thống tải và hiển thị trang danh sách dạng bảng:  
   - Bộ lọc: Ô tìm kiếm (Mã phiếu, SĐT khách, Số Serial), Bộ lọc Trạng thái (`Tất cả`, `Tiếp nhận`, `Đang kiểm tra`, `Đang xử lý`, `Chờ bàn giao`, `Đã xong`, `Từ chối`), Bộ lọc Kỹ thuật viên phụ trách, Bộ lọc Theo ngày tiếp nhận.  
   - Bảng danh sách: Mã phiếu, Ngày nhận, Khách hàng, Thiết bị & Serial, Hiện tượng lỗi, Trạng thái (Huy hiệu màu), Kỹ thuật viên, Thời gian xử lý dự kiến, Thao tác ("Xem chi tiết").  
   - Cảnh báo trực quan: Đánh dấu màu đỏ các phiếu bị chậm tiến độ (quá 48 giờ chưa xử lý xong).  
3. Người dùng lọc danh sách và nhấp vào phiếu cần thao tác để xử lý.  

**Alternative Flows**  
- **Danh sách trống:**  
  1. Không có phiếu bảo hành nào đang tồn đọng.  
  2. Hệ thống thông báo: "Hiện không có phiếu bảo hành nào cần xử lý".  

**Postconditions**  
Danh sách phiếu bảo hành được cung cấp đầy đủ và minh bạch.  

---

### UC-106 Khách xem thông tin bảo hành (View My Warranty Info)
**Primary Actors**  
Customer  

**Secondary Actors**  
None  

**Description**  
As a Customer, I want to view my electronic warranty cards in my personal account (active products, duration, expiry dates, serials) so that I can monitor coverage and easily request service when needed.  

**Preconditions**  
Khách hàng đã đăng nhập vào tài khoản trên website.  

**Normal Flow**  
*Khách hàng xem danh mục thẻ bảo hành các thiết bị đã mua*  
1. Khách hàng truy cập trang tài khoản cá nhân và chọn mục "Bảo hành của tôi" (My Warranties).  
2. Hệ thống hiển thị danh sách các Thẻ bảo hành điện tử của khách hàng:  
   - Bộ lọc tab: `Còn hạn bảo hành` (Active) và `Đã hết hạn` (Expired).  
   - Mỗi thẻ bảo hành hiển thị: Hình ảnh sản phẩm, Tên thiết bị & biến thể, Mã Serial, Mã đơn hàng đã mua, Ngày mua, Thời hạn bảo hành, Ngày hết hạn bảo hành chính xác.  
   - Thanh đếm ngược thời gian bảo hành còn lại (ví dụ: "Còn 1 năm 4 tháng").  
   - Nút thao tác: "Gửi yêu cầu bảo hành" (UC-154) đối với sản phẩm còn hạn.  
3. Khách hàng xem thông tin hoặc nhấp nút để tạo yêu cầu sửa chữa.  

**Alternative Flows**  
- **Khách hàng chưa có sản phẩm nào có bảo hành:**  
  1. Chưa có thẻ bảo hành nào gắn với tài khoản.  
  2. Hệ thống hiển thị thông báo rỗng: "Bạn chưa có thẻ bảo hành điện tử nào".  

**Postconditions**  
Thông tin bảo hành điện tử của khách hàng được hiển thị rõ ràng, tạo niềm tin cho người tiêu dùng.  

---

### UC-136 In phiếu tiếp nhận bảo hành (Print Warranty Receipt)
**Primary Actors**  
Sales Staff, Technical Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Technical Staff, I want to print a physical Warranty Acceptance Receipt to hand to the customer when they leave a product at the store for inspection/repair.  

**Preconditions**  
Phiếu tiếp nhận bảo hành đã được lập trên hệ thống (UC-102).  

**Normal Flow**  
*In biên bản bàn giao tiếp nhận bảo hành thiết bị*  
1. Người dùng nhấp nút "In biên nhận bảo hành" (Print Warranty Receipt) trên phiếu bảo hành.  
2. Hệ thống sinh bản in khổ A5/A4 chuẩn:  
   - Thông tin cửa hàng thiết bị vệ sinh: Tên, địa chỉ, số hotline bảo hành.  
   - Tiêu đề: PHIẾU TIẾP NHẬN SẢN PHẨM BẢO HÀNH.  
   - Mã phiếu bảo hành (dưới dạng mã vạch Barcode để quét nhận hàng khi trả).  
   - Thông tin khách hàng: Tên, Số điện thoại.  
   - Thông tin sản phẩm: Tên thiết bị, Hãng, Mã Serial, Hiện trạng ngoại quan, Phụ kiện kèm theo.  
   - Hiện tượng lỗi khách mô tả.  
   - Ngày tiếp nhận & Ngày hẹn trả hàng dự kiến.  
   - Điều khoản biên nhận: "Khách hàng vui lòng mang theo biên nhận này khi nhận lại sản phẩm...".  
   - Chữ ký của Nhân viên tiếp nhận và Khách hàng gửi máy.  
3. Hệ thống kích hoạt lệnh in của trình duyệt.  
4. Người dùng bấm "In" ra giấy và giao 1 liên cho khách hàng giữ, 1 liên dán cùng thiết bị.  

**Alternative Flows**  
- **Gửi biên nhận điện tử qua Email/SMS:**  
  1. Khách hàng không muốn lấy giấy in.  
  2. Người dùng chọn "Gửi biên nhận qua Email/SMS". Hệ thống gửi liên kết xem biên nhận điện tử vào số điện thoại của khách.  

**Postconditions**  
Biên nhận tiếp nhận bảo hành được in ra, tạo căn cứ pháp lý minh bạch giữa cửa hàng và khách.  

---

### UC-137 Bàn giao sản phẩm bảo hành cho khách (Handover Repaired Product)
**Primary Actors**  
Sales Staff, Technical Staff, Store Owner  

**Secondary Actors**  
Customer  

**Description**  
As a Sales Staff or Technical Staff, I want to return the repaired or replaced product to the customer at the counter, obtain customer acceptance confirmation, and mark the warranty claim as Completed.  

**Preconditions**  
1. Phiếu bảo hành đang ở trạng thái `Chờ bàn giao (Ready for Handover)`.  
2. Khách hàng đến cửa hàng để nhận lại sản phẩm (hoặc kỹ thuật viên mang đến lắp lại tại nhà).  

**Normal Flow**  
*Bàn giao sản phẩm đã bảo hành/đổi mới cho khách hàng*  
1. Nhân viên tìm phiếu bảo hành bằng cách quét mã vạch trên phiếu biên nhận (UC-136) hoặc tìm theo SĐT khách.  
2. Hệ thống mở chi tiết phiếu bảo hành. Nhân viên nhấp nút "Bàn giao trả khách".  
3. Nhân viên cùng khách hàng đồng kiểm tra thiết bị:  
   - Kiểm tra thiết bị đã hoạt động hoàn hảo sau sửa chữa (hoặc kiểm tra thiết bị mới 100% nếu là đổi mới).  
   - Đầy đủ phụ kiện bàn giao kèm theo.  
4. Khách hàng đồng ý nhận lại sản phẩm và ký xác nhận vào biên bản trả hàng.  
5. Nhân viên nhấp "Xác nhận bàn giao thành công".  
6. Hệ thống chuyển trạng thái phiếu bảo hành sang `Đã hoàn tất (Completed)`.  
7. Nếu là trường hợp đổi mới thiết bị: Hệ thống tự động cập nhật mã Serial mới vào Thẻ bảo hành điện tử của khách hàng và tính lại thời hạn bảo hành theo quy định.  
8. Hệ thống ghi nhật ký sự kiện vào Audit Log.  
9. Hệ thống hiển thị thông báo thành công (MSG121: "Đã bàn giao sản phẩm bảo hành cho khách thành công!").  

**Alternative Flows**  
- **Khách hàng không hài lòng với kết quả sửa chữa:**  
  1. Thiết bị sau sửa chữa vẫn bị lỗi cũ khi test thử.  
  2. Nhân viên không bấm bàn giao mà chuyển trạng thái phiếu quay lại `Đang kiểm tra kỹ thuật` kèm ghi chú yêu cầu kỹ thuật xử lý triệt để lại.  

**Postconditions**  
1. Phiếu bảo hành chính thức đóng ở trạng thái `Completed`.  
2. Thiết bị được trao trả an toàn cho khách hàng.  

---

### UC-154 Gửi yêu cầu bảo hành online (Submit Online Warranty Claim)
**Primary Actors**  
Customer  

**Secondary Actors**  
Cloudinary, Email Service  

**Description**  
As a Customer, I want to submit an online warranty claim directly from my account (selecting an active product, describing the problem, and uploading photos/videos of the defect) so that I do not have to travel to the store unnecessarily.  

**Preconditions**  
1. Khách hàng đã đăng nhập vào tài khoản trên website.  
2. Sản phẩm muốn yêu cầu bảo hành đang trong thời hạn bảo hành hợp lệ.  

**Normal Flow**  
*Khách hàng tự tạo phiếu yêu cầu bảo hành trực tuyến*  
1. Khách hàng vào mục "Bảo hành của tôi" (UC-106) và nhấp nút "Yêu cầu bảo hành" tại sản phẩm bị sự cố.  
2. Hệ thống hiển thị biểu mẫu "Gửi yêu cầu bảo hành trực tuyến":  
   - Tên sản phẩm, Mã đơn hàng, Mã Serial (tự động điền).  
   - Địa chỉ hiện tại của thiết bị (Địa chỉ nhà riêng/công trình cần hỗ trợ).  
   - Mô tả chi tiết sự cố hỏng hóc gặp phải (ví dụ: "Bộ xả bồn cầu nhấn không nảy, nước chảy rò rỉ liên tục vào lòng bồn").  
   - Tải lên hình ảnh / video minh chứng lỗi (tối thiểu 1 ảnh chụp hiện trạng hoặc video ngắn quay lỗi hoạt động).  
   - Lựa chọn hình thức: `Hỗ trợ kỹ thuật tại nhà` hoặc `Tôi sẽ gửi sản phẩm đến cửa hàng`.  
3. Khách hàng nhập mô tả, tải ảnh/video và chọn hình thức mong muốn.  
4. Khách hàng nhấp nút "Gửi yêu cầu bảo hành".  
5. Hệ thống kiểm tra dữ liệu: Mô tả không để trống, đã tải lên ít nhất 1 tệp đa phương tiện hợp lệ.  
6. Hệ thống gọi Secondary Actor `Cloudinary` lưu tệp ảnh/video lỗi.  
7. Hệ thống tạo phiếu bảo hành mới ở trạng thái `Chờ tiếp nhận trực tuyến (Online Pending)`.  
8. Hệ thống gửi email xác nhận cho khách hàng kèm mã phiếu bảo hành.  
9. Hệ thống gửi thông báo cho đội ngũ kỹ thuật/bán hàng để tiếp nhận xử lý.  
10. Hệ thống hiển thị thông báo thành công (MSG122: "Gửi yêu cầu bảo hành thành công! Kỹ thuật viên sẽ xem xét video/hình ảnh và liên hệ hỗ trợ bạn trong vòng 24 giờ").  

**Alternative Flows**  
- **Sản phẩm đã hết hạn bảo hành:**  
  1. Sản phẩm đã quá ngày hết hạn bảo hành.  
  2. Nút "Yêu cầu bảo hành miễn phí" bị vô hiệu hóa; hệ thống hiển thị tùy chọn "Đặt dịch vụ sửa chữa có tính phí".  
- **Tệp video/ảnh quá dung lượng cho phép (> 20MB):**  
  1. Khách hàng tải tệp dung lượng quá lớn.  
  2. Hệ thống báo lỗi (MSG123: "Dung lượng tệp tải lên vượt quá 20MB. Vui lòng nén hoặc chọn tệp nhỏ hơn").  

**Postconditions**  
1. Yêu cầu bảo hành trực tuyến được ghi nhận vào hệ thống.  
2. Nhân viên cửa hàng có thể xem video/ảnh để chẩn đoán trước lỗi từ xa.  

---

### UC-155 Xem danh sách yêu cầu bảo hành của tôi (View My Warranty Claims)
**Primary Actors**  
Customer  

**Secondary Actors**  
None  

**Description**  
As a Customer, I want to view the list of all warranty claims I have submitted and see their current progress so that I stay informed on repairs.  

**Preconditions**  
Khách hàng đã đăng nhập vào tài khoản trên website.  

**Normal Flow**  
*Xem danh sách các phiếu bảo hành đã gửi của cá nhân*  
1. Khách hàng truy cập vào mục "Phiếu bảo hành của tôi" (My Warranty Claims).  
2. Hệ thống hiển thị danh sách các phiếu bảo hành cá nhân theo dạng thẻ:  
   - Mã phiếu bảo hành (ví dụ: `BH-2026-0032`).  
   - Tên sản phẩm & Serial.  
   - Ngày gửi yêu cầu.  
   - Trạng thái hiện tại: `Đã tiếp nhận`, `Đang kiểm tra kỹ thuật`, `Đang sửa chữa`, `Đã xong - Chờ nhận hàng`, `Đã hoàn tất`.  
   - Nút "Theo dõi chi tiết" (Track Claim).  
3. Khách hàng nhấp vào nút "Theo dõi chi tiết" để xem tiến trình cụ thể (UC-156).  

**Alternative Flows**  
- **Khách hàng chưa từng gửi yêu cầu bảo hành nào:**  
  1. Danh sách trống.  
  2. Hệ thống hiển thị: "Bạn chưa có yêu cầu bảo hành nào đang xử lý".  

**Postconditions**  
Khách hàng nắm bắt được trạng thái tổng thể các yêu cầu bảo hành của mình.  

---

### UC-156 Xem chi tiết và theo dõi yêu cầu bảo hành (Track Warranty Claim Detail)
**Primary Actors**  
Customer  

**Secondary Actors**  
None  

**Description**  
As a Customer, I want to view detailed progress, technician feedback, resolution notes, or rejection reasons for a claim under my account so that I understand exactly what was done to fix my equipment.  

**Preconditions**  
Khách hàng đã mở một phiếu bảo hành từ danh sách (UC-155).  

**Normal Flow**  
*Xem chi tiết tiến độ và kết quả sửa chữa bảo hành*  
1. Hệ thống hiển thị màn hình "Theo dõi tiến độ bảo hành - [Mã BH]":  
   - Thanh tiến trình trực quan với các mốc thời gian:  
     + Gửi yêu cầu -> Tiếp nhận -> Kiểm tra kỹ thuật -> Hoàn thành xử lý -> Bàn giao.  
   - Thông tin kỹ thuật viên phụ trách (Tên, Số điện thoại hỗ trợ).  
   - Kết luận kiểm tra từ kỹ thuật viên (ví dụ: "Lỗi hỏng phao cấp nước, đã thay thế cụm phao TOTO chính hãng mới").  
   - Ảnh chụp thiết bị sau khi khắc phục xong.  
   - Hướng dẫn nhận lại thiết bị (đến cửa hàng nhận hoặc lịch hẹn kỹ thuật đến lắp lại tại nhà).  
2. Khách hàng theo dõi và nắm bắt thông tin rõ ràng.  

**Alternative Flows**  
- **Phiếu bảo hành bị từ chối:**  
  1. Yêu cầu bảo hành không được duyệt do vi phạm chính sách.  
  2. Hệ thống hiển thị lý do từ chối chi tiết bằng chữ màu đỏ kèm hình ảnh bằng chứng vi phạm kỹ thuật và hotline khiếu nại.  

**Postconditions**  
Thông tin tiến độ bảo hành được cung cấp minh bạch đến người tiêu dùng.  

---

### UC-195 Tạo chính sách bảo hành (Create Warranty Policy)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to define new warranty policies (duration in months, coverage conditions, terms, manufacturer backed) so that products can be assigned appropriate warranty packages.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập với vai trò `Store Owner`.  
2. Đang ở trang "Cấu hình chính sách bảo hành" trong Quản trị hệ thống.  

**Normal Flow**  
*Tạo mới chính sách bảo hành thiết bị vệ sinh*  
1. Chủ cửa hàng nhấp nút "Thêm chính sách bảo hành mới" (New Policy).  
2. Hệ thống hiển thị biểu mẫu bao gồm:  
   - Tên chính sách (ví dụ: "Bảo hành thân sứ bồn cầu 10 năm", "Bảo hành linh kiện điện tử thông minh 24 tháng").  
   - Mã chính sách (viết hoa không dấu, ví dụ: `POL-SU-10Y`).  
   - Thời hạn bảo hành: Nhập số tháng (ví dụ: 120 tháng = 10 năm).  
   - Loại hình bảo hành: `Đổi mới trong 30 ngày`, `Sửa chữa thay linh kiện miễn phí`, `Bảo hành tận nơi tại công trình`.  
   - Điều kiện áp dụng chi tiết (văn bản quy định điều kiện hợp lệ và các trường hợp loại trừ).  
   - Hãng sản xuất liên kết (TOTO, INAX, Caesar, Viglacera...).  
3. Chủ cửa hàng nhập các thông tin quy định.  
4. Chủ cửa hàng nhấp nút "Lưu chính sách".  
5. Hệ thống kiểm tra: Tên và Mã chính sách không trùng lặp, Thời hạn > 0.  
6. Hệ thống lưu chính sách mới vào cơ sở dữ liệu ở trạng thái `Kích hoạt (Active)`.  
7. Hệ thống ghi nhận sự kiện vào Audit Log.  
8. Hệ thống hiển thị thông báo thành công (MSG124: "Tạo chính sách bảo hành mới thành công!") và đưa vào danh sách chính sách khả dụng.  

**Alternative Flows**  
- **Mã chính sách bị trùng lặp:**  
  1. Mã chính sách đã tồn tại trong hệ thống.  
  2. Hệ thống báo lỗi (MSG125: "Mã chính sách bảo hành đã tồn tại. Vui lòng nhập mã khác").  

**Postconditions**  
Chính sách bảo hành mới được khởi tạo và sẵn sàng gán cho sản phẩm.  

---

### UC-196 Cập nhật chính sách bảo hành (Update Warranty Policy)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to edit existing warranty terms, duration, or conditions when manufacturer warranties change so that future sales inherit updated rules.  

**Preconditions**  
Chủ cửa hàng đã đăng nhập và đang mở một chính sách bảo hành hiện có.  

**Normal Flow**  
*Chỉnh sửa điều khoản chính sách bảo hành*  
1. Chủ cửa hàng nhấp nút "Sửa" tại chính sách bảo hành cần cập nhật.  
2. Hệ thống mở biểu mẫu với dữ liệu hiện tại của chính sách.  
3. Chủ cửa hàng chỉnh sửa các trường thông tin (thời hạn, điều kiện bảo hành, văn bản điều khoản).  
4. Chủ cửa hàng nhấp nút "Lưu thay đổi".  
5. Hệ thống kiểm tra dữ liệu hợp lệ.  
6. Hệ thống cập nhật bản ghi chính sách bảo hành trong cơ sở dữ liệu.  
7. Quy tắc nghiệp vụ bảo đảm: Các đơn hàng và thẻ bảo hành đã phát hành trước đây vẫn giữ nguyên quyền lợi cũ; chính sách mới chỉ áp dụng cho các giao dịch bán mới kể từ thời điểm này.  
8. Hệ thống ghi nhận sự kiện vào Audit Log.  
9. Hệ thống hiển thị thông báo thành công (MSG126: "Cập nhật chính sách bảo hành thành công!").  

**Alternative Flows**  
- **Hủy bỏ thay đổi:**  
  1. Người dùng bấm "Hủy bỏ". Hệ thống đóng biểu mẫu và giữ nguyên chính sách cũ.  

**Postconditions**  
Nội dung chính sách bảo hành được cập nhật cho các đợt phát hành thẻ bảo hành trong tương lai.  

---

### UC-197 Bật/Tắt chính sách bảo hành (Activate/Deactivate Warranty Policy)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to toggle the active status of a warranty policy so that retired policies cannot be erroneously attached to newly listed products.  

**Preconditions**  
Chủ cửa hàng đang ở danh sách chính sách bảo hành.  

**Normal Flow**  
*Kích hoạt hoặc vô hiệu hóa một chính sách bảo hành*  
1. Tại danh sách chính sách, Chủ cửa hàng gạt công tắc Trạng thái (Toggle switch `Active` / `Inactive`) tại dòng chính sách tương ứng.  
2. Hệ thống hiển thị hộp thoại xác nhận: "Bạn có chắc muốn vô hiệu hóa chính sách [Tên chính sách]? Chính sách này sẽ không thể chọn khi tạo sản phẩm mới".  
3. Chủ cửa hàng nhấp "Xác nhận".  
4. Hệ thống cập nhật trạng thái `IsActive = False` trong cơ sở dữ liệu.  
5. Hệ thống ghi vết thao tác vào Audit Log.  
6. Hệ thống hiển thị thông báo (MSG127: "Đã tắt kích hoạt chính sách bảo hành thành công!").  

**Alternative Flows**  
- **Bật lại chính sách đang tắt:**  
  1. Gạt công tắc từ Inactive sang Active.  
  2. Hệ thống kích hoạt lại chính sách ngay lập tức mà không cần xác nhận phức tạp.  

**Postconditions**  
Chính sách bảo hành bị ẩn khỏi danh sách lựa chọn khi tạo sản phẩm mới.  

---

### UC-198 Xem danh sách chính sách bảo hành (View Warranty Policy List)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to view all available warranty policies, active statuses, and summary terms so that I understand product warranty offerings.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý.  

**Normal Flow**  
*Xem danh sách các gói chính sách bảo hành của cửa hàng*  
1. Người dùng chọn mục "Chính sách bảo hành" trong menu Quản lý bảo hành.  
2. Hệ thống hiển thị bảng danh sách các chính sách:  
   - Bảng gồm: Mã chính sách, Tên chính sách, Thời hạn (Số tháng), Hãng sản xuất, Số lượng sản phẩm đang áp dụng, Trạng thái (Đang dùng / Ngừng dùng), Thao tác (Sửa, Bật/Tắt).  
3. Người dùng xem thông tin và duyệt danh sách.  

**Alternative Flows**  
- **Danh sách trống:**  
  1. Chưa có chính sách nào.  
  2. Hệ thống hiển thị thông báo gợi ý tạo chính sách ban đầu.  

**Postconditions**  
Toàn bộ danh mục chính sách bảo hành được hiển thị rõ ràng.  

---

### UC-199 Gán chính sách bảo hành cho sản phẩm (Assign Warranty Policy to Product)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to map specific warranty policies to product variants so that when those products are sold, correct warranty records are automatically created.  

**Preconditions**  
1. Người dùng đang ở màn hình tạo mới hoặc cập nhật sản phẩm/biến thể (UC-020, UC-022).  
2. Đã có ít nhất một chính sách bảo hành đang hoạt động (UC-195).  

**Normal Flow**  
*Liên kết gói bảo hành tương ứng với sản phẩm hoặc biến thể*  
1. Tại biểu mẫu thông tin sản phẩm, người dùng cuộn đến mục "Chính sách bảo hành áp dụng".  
2. Hệ thống hiển thị Dropdown danh sách các chính sách bảo hành đang kích hoạt (`Active`).  
3. Người dùng chọn chính sách phù hợp (ví dụ: đối với Bồn cầu TOTO chọn chính sách `Bảo hành thân sứ 10 năm & phụ kiện 24 tháng`).  
4. Hệ thống tự động điền tóm tắt quyền lợi bảo hành vào trường hiển thị thông tin sản phẩm trên website.  
5. Người dùng lưu thông tin sản phẩm.  
6. Hệ thống lưu khóa ngoại `WarrantyPolicyID` vào bản ghi sản phẩm/biến thể trong cơ sở dữ liệu.  
7. Hệ thống hiển thị thông báo thành công (MSG128: "Đã gán chính sách bảo hành cho sản phẩm thành công!").  

**Alternative Flows**  
- **Sản phẩm không có bảo hành:**  
  1. Người dùng chọn "Không áp dụng bảo hành".  
  2. Hệ thống lưu giá trị null, khi bán hàng sẽ không tự sinh thẻ bảo hành.  

**Postconditions**  
Sản phẩm được liên kết với chính sách bảo hành, sẵn sàng tự động kích hoạt thẻ bảo hành khi xuất bán (UC-099).  

---

### UC-200 Tìm kiếm lọc xem lịch sử bảo hành (Search/Filter Warranty History)
**Primary Actors**  
Technical Staff, Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Technical Staff, Sales Staff, or Store Owner, I want to search and filter global warranty repair histories by customer, equipment model, serial, technician, date range, or resolution type to audit technical performance and defect trends.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống và có quyền xem báo cáo bảo hành.  

**Normal Flow**  
*Tìm kiếm và phân tích toàn diện lịch sử các ca bảo hành sửa chữa*  
1. Người dùng truy cập mục "Lịch sử xử lý bảo hành" (Warranty Resolution History).  
2. Hệ thống hiển thị giao diện báo cáo lịch sử với các bộ lọc nâng cao:  
   - Lọc theo Hãng sản xuất (TOTO, INAX, Caesar...).  
   - Lọc theo Nhóm lỗi kỹ thuật (Lỗi xả nước, Lỗi rò rỉ van, Lỗi nứt vỡ sứ, Lỗi mạch điện tử thông minh).  
   - Lọc theo Hình thức xử lý (Sửa chữa miễn phí / Đổi mới thiết bị / Từ chối).  
   - Lọc theo Khoảng thời gian tiếp nhận.  
3. Bảng dữ liệu hiển thị: Mã phiếu, Thiết bị, Serial, Khách hàng, Kỹ thuật viên, Lỗi xác định, Linh kiện đã thay, Kết quả xử lý, Ngày hoàn tất.  
4. Người dùng lọc theo tiêu chí mong muốn và nhấp "Xuất báo cáo Excel" để tải về phục vụ đối soát hoàn tiền/đổi mới linh kiện với hãng sản xuất.  

**Alternative Flows**  
- **Không tìm thấy dữ liệu:**  
  1. Bộ lọc không trả về kết quả nào.  
  2. Hệ thống hiển thị bảng trống kèm thông báo thích hợp.  

**Postconditions**  
Dữ liệu lịch sử bảo hành được kết xuất đầy đủ, hỗ trợ đánh giá chất lượng sản phẩm của từng nhà sản xuất.  

---
