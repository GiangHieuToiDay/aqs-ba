# PHÂN HỆ FE-13: QUẢN LÝ CÔNG VIỆC LẮP ĐẶT VÀ SỬA CHỮA (TECHNICAL SERVICES)

---

### UC-241 Xem danh sách công việc kỹ thuật (View Technical Job List)
**Primary Actors**  
Technical Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Technical Staff or Store Owner, I want to view, search, and filter assigned installation and repair jobs by job ID, customer, appointment date, and status so that technical work is executed on time.  

**Preconditions**  
Người dùng đã đăng nhập với vai trò `Technical Staff` hoặc `Store Owner`.  

**Normal Flow**  
*Xem và tìm kiếm danh sách công việc kỹ thuật*  
1. Nhân viên kỹ thuật nhấp chọn mục "Công việc kỹ thuật" (Technical Jobs) trên menu.  
2. Hệ thống tải và hiển thị trang danh sách công việc với các bộ lọc:  
   - Ô tìm kiếm từ khóa (Mã công việc, Tên khách hàng, Số điện thoại, Địa chỉ công trình).  
   - Bộ lọc trạng thái: `Tất cả`, `Chờ tiếp nhận`, `Đã tiếp nhận`, `Đang di chuyển`, `Đang thực hiện`, `Hoàn thành`, `Tạm hoãn/Hủy`.  
   - Bộ lọc phân loại công việc: `Lắp đặt mới`, `Sửa chữa / Bảo hành tại nhà`, `Khảo sát đường nước`.  
   - Bộ lọc theo ngày hẹn (Hôm nay, Ngày mai, Tuần này, Khoảng ngày tùy chọn).  
3. Bảng dữ liệu hiển thị các cột: Mã công việc (ví dụ: `JOB-2026-0012`), Loại công việc, Tên khách hàng, Số điện thoại, Địa chỉ, Ngày & Giờ hẹn, Trạng thái (Huy hiệu màu), Thao tác ("Xem chi tiết").  
4. Mặc định đối với Technical Staff: Hệ thống hiển thị các công việc được phân công cho chính nhân viên đó trong ngày hôm nay lên đầu danh sách.  
5. Nhân viên duyệt danh sách và nhấp vào một công việc để xem chi tiết (UC-242).  

**Alternative Flows**  
- **Không có công việc nào được phân công trong ngày:**  
  1. Ngày được lọc không có lịch làm việc kỹ thuật nào.  
  2. Hệ thống hiển thị thông báo (MSG95: "Không có công việc kỹ thuật nào được lên lịch cho khoảng thời gian này").  

**Postconditions**  
Danh sách công việc kỹ thuật được hiển thị đầy đủ, phân loại rõ ràng theo lịch phân công.  

---

### UC-242 Xem chi tiết công việc kỹ thuật (View Technical Job Detail)
**Primary Actors**  
Technical Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Technical Staff or Store Owner, I want to view comprehensive details of an installation/repair job (customer info, site address, equipment models, technical requirements, notes, history) so that I am fully prepared before arriving at the customer's site.  

**Preconditions**  
1. Người dùng đã đăng nhập.  
2. Người dùng mở một công việc kỹ thuật từ danh sách (UC-241) hoặc từ lịch hẹn (UC-243).  

**Normal Flow**  
*Xem đầy đủ thông tin chi tiết của một phiếu công việc kỹ thuật*  
1. Người dùng nhấp vào mã công việc hoặc nút "Xem chi tiết".  
2. Hệ thống điều hướng đến trang "Chi tiết công việc kỹ thuật - [Mã JOB]":  
   - Khối 1: Thông tin khách hàng & Địa điểm thi công (Họ tên chủ nhà/người liên hệ, Số điện thoại có nút bấm gọi nhanh trên di động, Địa chỉ chi tiết kèm liên kết mở Google Maps chỉ đường).  
   - Khối 2: Lịch hẹn thi công (Ngày hẹn, Khung giờ thỏa thuận, Nhân viên kỹ thuật phụ trách).  
   - Khối 3: Danh sách thiết bị & Công việc cụ thể:  
     + Tên thiết bị, Mã model/SKU, Số Serial thiết bị (nếu có).  
     + Mô tả yêu cầu: Lắp đặt mới combo bồn cầu thông minh + sen cây nhiệt độ; hoặc Sửa chữa rò rỉ nước tại van khóa góc.  
     + Yêu cầu kỹ thuật đặc biệt (Tâm xả bao nhiêu, áp lực nước yếu cần bơm tăng áp...).  
   - Khối 4: Lịch sử và Tiến trình cập nhật trạng thái (Dấu thời gian từng bước tiếp nhận, bắt đầu làm, hoàn thành).  
   - Khối 5: Các ghi chú kỹ thuật và hình ảnh hiện trường đã chụp (nếu có).  
3. Nhân viên nắm bắt thông tin và sẵn sàng di chuyển hoặc thực hiện công việc.  

**Alternative Flows**  
- **Khách hàng đổi số điện thoại hoặc đổi giờ hẹn:**  
  1. Nếu nhân viên phát hiện sai lệch, có thể nhấp nút "Thêm ghi chú kỹ thuật" (UC-247) để ghi nhận ngay lập tức vào hồ sơ.  

**Postconditions**  
Mọi thông tin liên quan đến công việc kỹ thuật được cung cấp rõ ràng, chính xác.  

---

### UC-243 Xem lịch hẹn công việc (View Technical Appointment Schedule)
**Primary Actors**  
Technical Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Technical Staff or Store Owner, I want to view all technical appointments in a calendar view (day, week, month) so that I can manage my daily route and avoid scheduling overlaps.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý kỹ thuật.  

**Normal Flow**  
*Xem lịch làm việc kỹ thuật dưới dạng Calendar trực quan*  
1. Người dùng nhấp chọn mục "Lịch hẹn kỹ thuật" (Technical Schedule/Calendar).  
2. Hệ thống tải và hiển thị giao diện Lịch theo dạng Ngày (Day View), Tuần (Week View), hoặc Tháng (Month View).  
3. Mỗi ô khung giờ hiển thị các khối thẻ lịch hẹn (Event Blocks):  
   - Màu sắc theo trạng thái công việc (Xanh lá: Đã hoàn thành; Vàng: Đang làm; Xanh dương: Sắp diễn ra; Xám: Chờ tiếp nhận).  
   - Thông tin tóm tắt trên khối: Giờ bắt đầu - kết thúc, Tên khách hàng, Phường/Quận địa chỉ, Tóm tắt công việc (ví dụ: "09:00 - 11:30 | Anh Nam - Cầu Giấy | Lắp đặt 2 bồn cầu TOTO").  
4. Người dùng nhấp vào một khối lịch hẹn để xem popup xem nhanh thông tin hoặc nhấp đúp để mở trang Chi tiết công việc (UC-242).  
5. Người dùng có thể chuyển đổi giữa các tuần hoặc lọc theo tên nhân viên kỹ thuật (dành cho Store Owner để điều phối ca).  

**Alternative Flows**  
- **Cảnh báo trùng lịch hẹn:**  
  1. Một nhân viên kỹ thuật bị xếp 2 lịch hẹn trùng khung giờ thi công.  
  2. Khối sự kiện hiển thị biểu tượng cảnh báo màu đỏ (Conflict Warning) để Store Owner kịp thời điều chuyển nhân viên khác.  

**Postconditions**  
Lịch biểu làm việc kỹ thuật được trình bày trực quan, giúp tối ưu hóa lộ trình di chuyển.  

---

### UC-244 Cập nhật trạng thái công việc kỹ thuật (Update Technical Job Status)
**Primary Actors**  
Technical Staff  

**Secondary Actors**  
SMS Service  

**Description**  
As a Technical Staff, I want to update the lifecycle status of a job (Accepted -> En Route -> In Progress -> Completed/On Hold) so that the store and customer are informed of real-time progress.  

**Preconditions**  
1. Nhân viên kỹ thuật đã đăng nhập vào hệ thống.  
2. Công việc kỹ thuật mục tiêu được phân công cho nhân viên này.  

**Normal Flow**  
*Cập nhật tuần tự trạng thái thi công công việc kỹ thuật*  
1. Tại màn hình chi tiết công việc, nhân viên nhấp vào nút chuyển trạng thái phù hợp theo quy trình nghiệp vụ:  
   - Từ `Chờ tiếp nhận` nhấp "Tiếp nhận công việc" -> Chuyển sang `Đã tiếp nhận (Accepted)`.  
   - Khi bắt đầu xuất phát đến nhà khách: Nhấp "Bắt đầu di chuyển" -> Chuyển sang `Đang di chuyển (En Route)`.  
   - Khi có mặt tại hiện trường và bắt đầu lắp đặt/sửa chữa: Nhấp "Bắt đầu làm" -> Chuyển sang `Đang thực hiện (In Progress)`.  
2. Hệ thống kiểm tra điều kiện chuyển đổi trạng thái hợp lệ (State Machine Transition Rules):  
   - Không được nhảy cóc trạng thái trái quy trình (ví dụ: từ Chờ tiếp nhận không thể nhảy thẳng sang Hoàn thành mà không qua Đang thực hiện).  
3. Hệ thống cập nhật trạng thái mới cho công việc kèm lưu dấu thời gian (Timestamp) chính xác.  
4. Hệ thống ghi nhật ký cập nhật trạng thái vào lịch sử công việc.  
5. (Tùy chọn cấu hình): Hệ thống tự động gửi tin nhắn SMS/Zalo ZNS thông báo cho khách hàng (ví dụ: "Kỹ thuật viên [Tên NV] đang trên đường đến nhà quý khách, dự kiến có mặt lúc [Giờ]").  
6. Hệ thống hiển thị thông báo thành công (MSG96: "Đã cập nhật trạng thái công việc thành công!") và đổi màu giao diện tương ứng.  

**Alternative Flows**  
- **Trường hợp tạm hoãn công việc (Put on Hold):**  
  1. Khi đến nơi, chủ nhà đi vắng hoặc hiện trạng công trình chưa đủ điều kiện (chưa có điện nước, thợ gạch chưa xong).  
  2. Nhân viên nhấp nút "Tạm hoãn công việc".  
  3. Hệ thống yêu cầu chọn lý do: "Chưa có đường điện/nước", "Chủ nhà vắng mặt", "Hiện trạng xây dựng chưa xong", "Khác" và nhập ghi chú.  
  4. Trạng thái chuyển sang `Tạm hoãn (On Hold)` và lưu vào hệ thống để lên lại lịch hẹn mới.  

**Postconditions**  
1. Trạng thái công việc kỹ thuật được cập nhật chính xác theo thực tế hiện trường.  
2. Ban quản lý và khách hàng nắm rõ tiến độ làm việc của kỹ thuật viên.  

---

### UC-245 Ghi nhận kết quả lắp đặt/sửa chữa (Record Installation/Repair Result)
**Primary Actors**  
Technical Staff  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Technical Staff, I want to record the outcome of an installation/repair job (installed items, test results, issues resolved, on-site photos) so that there is undeniable proof of completed work.  

**Preconditions**  
1. Nhân viên kỹ thuật đang ở trạng thái `Đang thực hiện (In Progress)` tại hiện trường.  
2. Công việc lắp đặt/sửa chữa đã được thao tác xong và tiến hành kiểm tra kỹ thuật (Test áp lực nước, kiểm tra rò rỉ, vận hành thử).  

**Normal Flow**  
*Ghi nhận chi tiết kết quả kỹ thuật và chụp ảnh hiện trường nghiệm thu*  
1. Nhân viên kỹ thuật mở ứng dụng/website trên điện thoại, truy cập công việc đang làm và nhấp nút "Ghi nhận kết quả thi công".  
2. Hệ thống hiển thị biểu mẫu nghiệm thu kỹ thuật:  
   - Danh sách hạng mục thiết bị đã lắp đặt/sửa chữa.  
   - Danh sách kiểm tra chất lượng (Checklist kiểm tra):  
     + [x] Đã xả thử và kiểm tra không rò rỉ tại các mối nối ren.  
     + [x] Áp lực phun nước sen tắm đạt chuẩn.  
     + [x] Bồn cầu đóng êm, xả hút triệt để, gioăng ngăn mùi đã lắp khít.  
     + [x] Đã vệ sinh sạch sẽ khu vực sàn thao tác.  
   - Ô nhập "Kết quả xử lý & Mô tả chi tiết" (ví dụ: "Đã thay van cấp nước mới cho bồn cầu TOTO, căn chỉnh lại cần gạt, xả thử 5 lần không phát sinh rò rỉ").  
   - Khu vực tải ảnh hiện trường nghiệm thu (Yêu cầu tối thiểu 2 ảnh: ảnh toàn cảnh thiết bị sau lắp và ảnh chi tiết mối nối đường nước).  
3. Nhân viên kỹ thuật tích chọn các mục checklist đã đạt, nhập kết quả xử lý và chụp ảnh trực tiếp tải lên.  
4. Nhân viên nhấp nút "Lưu kết quả kỹ thuật".  
5. Hệ thống gọi Secondary Actor `Cloudinary` tải các tệp ảnh lên lưu trữ an toàn.  
6. Hệ thống lưu kết quả nghiệm thu, danh sách checklist và đường dẫn ảnh vào hồ sơ công việc.  
7. Hệ thống hiển thị thông báo thành công (MSG97: "Đã lưu kết quả thi công kỹ thuật thành công!").  

**Alternative Flows**  
- **Step 3.1_Chưa chụp đủ ảnh hiện trường theo quy định:**  
  1. Nhân viên bấm lưu nhưng chưa tải lên ảnh chụp sản phẩm sau hoàn thiện.  
  2. Hệ thống cảnh báo (MSG98: "Quy định bắt buộc tải lên ít nhất 2 ảnh chụp hiện trường hoàn thiện để đối chiếu nghiệm thu").  
  3. Nhân viên chụp ảnh bổ sung và bấm lưu lại.  

**Postconditions**  
1. Hồ sơ kết quả nghiệm thu kèm ảnh thực tế được lưu vĩnh viễn trên hệ thống.  
2. Dữ liệu sẵn sàng cho bước ký xác nhận hoàn thành công việc (UC-246).  

---

### UC-246 Xác nhận hoàn thành công việc (Confirm Technical Job Completion)
**Primary Actors**  
Technical Staff  

**Secondary Actors**  
Customer  

**Description**  
As a Technical Staff, I want to submit customer sign-off or digital signature to complete the technical job so that the system officially marks the job as Completed and activates warranty records.  

**Preconditions**  
1. Kết quả kỹ thuật và ảnh nghiệm thu đã được lưu đầy đủ (UC-245).  
2. Khách hàng đã trực tiếp kiểm tra và hài lòng với chất lượng thiết bị.  

**Normal Flow**  
*Ký nhận nghiệm thu và hoàn tất bàn giao công việc kỹ thuật*  
1. Nhân viên kỹ thuật nhấp nút "Hoàn tất công việc & Bàn giao" trên màn hình công việc.  
2. Hệ thống hiển thị màn hình bàn giao nghiệm thu gồm: Tóm tắt các thiết bị đã xử lý, Ảnh nghiệm thu, và Ô ký chữ ký điện tử của khách hàng (Digital Signature Pad).  
3. Nhân viên đưa điện thoại cho khách hàng ký tên xác nhận nghiệm thu trực tiếp lên màn hình cảm ứng, hoặc chụp ảnh biên bản giấy đã có chữ ký tay của khách hàng.  
4. Nhân viên nhấp nút "Xác nhận hoàn thành".  
5. Hệ thống kiểm tra các điều kiện tiên quyết: Kết quả kỹ thuật đã nhập, Checklist đã hoàn thành, Chữ ký khách hàng hợp lệ.  
6. Hệ thống chuyển trạng thái công việc kỹ thuật sang `Hoàn thành (Completed)`.  
7. Hệ thống ghi nhận thời gian hoàn tất thực tế.  
8. Hệ thống tự động kích hoạt/cập nhật Thẻ bảo hành điện tử (liên kết FE-09) cho các thiết bị vừa được lắp đặt/sửa chữa.  
9. Hệ thống gửi tin nhắn cảm ơn và thông báo bảo hành điện tử tới điện thoại của khách hàng.  
10. Hệ thống hiển thị thông báo thành công (MSG99: "Công việc kỹ thuật đã hoàn thành xuất sắc! Cảm ơn bạn đã nỗ lực làm việc") và chuyển về màn hình danh sách công việc.  

**Alternative Flows**  
- **Khách hàng không có mặt để ký trực tiếp (Bàn giao qua bên thứ 3/Thợ xây):**  
  1. Chủ nhà không có mặt tại công trình.  
  2. Nhân viên chọn tùy chọn "Bàn giao cho người đại diện", nhập Họ tên và SĐT người nhận thay (ví dụ: "Bàn giao cho Bác Thợ cả trông công trình") và tải ảnh biên bản bàn giao kèm theo.  
  3. Hệ thống lưu người nhận thay và đóng công việc.  

**Postconditions**  
1. Phiếu công việc kỹ thuật chuyển sang trạng thái `Completed`.  
2. Thông tin bảo hành được kích hoạt chính thức.  

---

### UC-247 Ghi nhận ghi chú kỹ thuật (Record Technical Note)
**Primary Actors**  
Technical Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Technical Staff or Store Owner, I want to append technical field notes (water pressure warnings, pipe diameter quirks, customer requests) to a job record so that valuable site context is preserved for future visits.  

**Preconditions**  
Người dùng đã mở trang chi tiết của một công việc kỹ thuật (UC-242).  

**Normal Flow**  
*Thêm ghi chú kỹ thuật tại hiện trường vào hồ sơ công việc*  
1. Người dùng cuộn đến phần "Ghi chú kỹ thuật" (Technical Notes) và nhấp nút "Thêm ghi chú mới".  
2. Hệ thống hiển thị ô nhập ghi chú kèm các tùy chọn gắn nhãn phân loại:  
   - Nhãn loại ghi chú: `Cảnh báo đường nước`, `Kích thước phi tiêu chuẩn`, `Lưu ý điện âm tường`, `Thái độ/Yêu cầu riêng của chủ nhà`.  
   - Ô nhập nội dung chi tiết (ví dụ: "Áp lực nước tại tầng 4 rất yếu (dưới 0.05 MPa), đã khuyến nghị chủ nhà lắp thêm bơm tăng áp 200W trước khi lắp sen cây tắm đứng").  
3. Người dùng nhập nội dung và nhấp nút "Lưu ghi chú".  
4. Hệ thống kiểm tra nội dung không để trống.  
5. Hệ thống lưu bản ghi chú mới kèm Tên người tạo, Vai trò và Dấu thời gian chính xác vào dòng thời gian (Notes Timeline) của công việc.  
6. Hệ thống hiển thị thông báo (MSG100: "Đã lưu ghi chú kỹ thuật thành công!").  

**Alternative Flows**  
- **Bỏ trống nội dung ghi chú:**  
  1. Người dùng không nhập chữ nào và bấm lưu.  
  2. Hệ thống cảnh báo (MSG02: "Vui lòng nhập nội dung ghi chú").  

**Postconditions**  
Ghi chú kỹ thuật được lưu trữ an toàn trong hồ sơ công việc và không thể bị sửa chữa nhằm giữ nguyên tính trung thực của sự việc tại thời điểm ghi nhận.  

---

### UC-248 Tiếp nhận và cập nhật trạng thái đơn yêu cầu bảo hành (Receive and Update Warranty Request Status)
**Primary Actors**  
Technical Staff, Store Owner  

**Secondary Actors**  
Customer, Email Service  

**Description**  
As a Technical Staff or Store Owner, I want to receive warranty claims escalated from sales staff/customers, inspect defects, and advance their warranty lifecycle status (Received -> Inspecting -> Repairing / Replacing -> Resolved / Rejected) with explicit reasons.  

**Preconditions**  
1. Nhân viên kỹ thuật đã đăng nhập vào hệ thống.  
2. Có đơn yêu cầu bảo hành (từ UC-102 hoặc UC-154) đang chờ bộ phận kỹ thuật xử lý.  

**Normal Flow**  
*Kỹ thuật viên tiếp nhận kiểm tra và cập nhật tiến độ xử lý phiếu bảo hành*  
1. Kỹ thuật viên truy cập mục "Xử lý bảo hành kỹ thuật" trong hệ thống.  
2. Kỹ thuật viên chọn phiếu bảo hành cần xử lý (ví dụ: `BH-2026-0045` - Bồn cầu thông minh bị liệt bảng điều khiển).  
3. Kỹ thuật viên nhấp nút "Tiếp nhận kiểm định kỹ thuật".  
4. Hệ thống cập nhật trạng thái phiếu bảo hành sang `Đang kiểm tra kỹ thuật (Inspecting)`.  
5. Kỹ thuật viên thực hiện tháo kiểm tra thiết bị tại xưởng hoặc tại nhà khách:  
   - Xác định nguyên nhân hỏng hóc: Lỗi do nhà sản xuất (Mạch điện tử chập) hay Lỗi do người dùng (Làm rơi vỡ ngấm nước).  
6. Kỹ thuật viên nhấp vào "Cập nhật kết luận giám định & Phương án xử lý":  
   - Nhập Kết luận kỹ thuật.  
   - Chọn Phương án: (1) Sửa chữa miễn phí thay linh kiện chính hãng; (2) Đổi mới thiết bị tương đương (áp dụng chính sách 1 đổi 1); (3) Từ chối bảo hành (chuyển sang sửa dịch vụ tính phí do lỗi người dùng).  
7. Kỹ thuật viên cập nhật trạng thái tương ứng: `Đang xử lý sửa chữa (Repairing)` hoặc `Đã hoàn tất bảo hành (Resolved)` hoặc `Từ chối bảo hành (Rejected)`.  
8. Kỹ thuật viên nhấp "Lưu cập nhật bảo hành".  
9. Hệ thống lưu kết quả giám định, người thực hiện và cập nhật trạng thái vào cơ sở dữ liệu.  
10. Hệ thống gọi Secondary Actor `Email Service` gửi thông báo cập nhật kết quả bảo hành cho khách hàng.  
11. Hệ thống hiển thị thông báo thành công (MSG101: "Cập nhật tiến độ bảo hành kỹ thuật thành công!").  

**Alternative Flows**  
- **Từ chối bảo hành do vi phạm chính sách bảo hành:**  
  1. Thiết bị bị tháo niêm phong tự ý sửa chữa hoặc vỡ do ngoại lực.  
  2. Kỹ thuật viên chọn trạng thái `Từ chối bảo hành` và bắt buộc nhập chi tiết lý do kèm ảnh chụp bằng chứng vi phạm điều kiện bảo hành.  
  3. Hệ thống lưu hồ sơ từ chối và thông báo chính thức cho khách hàng kèm báo giá sửa chữa dịch vụ có phí (nếu khách đồng ý).  

**Postconditions**  
1. Trạng thái phiếu bảo hành được phản ánh trung thực theo tiến trình xử lý kỹ thuật.  
2. Toàn bộ lịch sử can thiệp được lưu vết trong hồ sơ bảo hành của sản phẩm.  

---
