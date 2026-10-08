# PHÂN HỆ FE-10: QUẢN LÝ KHÁCH HÀNG & LỊCH SỬ MUA HÀNG

---

### UC-107 Tự tạo hồ sơ khách hàng (Create Customer Profile)
**Primary Actors**  
System, Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a System, I want to automatically create a centralized Customer Profile whenever a new customer places their first online order or counter order so that the store can maintain a continuous purchase history and contact record.  

**Preconditions**  
1. Đơn hàng online đầu tiên được đặt bởi khách vãng lai (UC-040), hoặc Nhân viên bán hàng tạo đơn tại quầy cho khách mới mà số điện thoại chưa từng tồn tại trên hệ thống.  

**Normal Flow**  
*Hệ thống tự động khởi tạo hồ sơ khách hàng từ giao dịch đầu tiên*  
1. Người dùng (hoặc Nhân viên) thực hiện lưu đơn hàng mới có chứa thông tin khách hàng (Họ tên, Số điện thoại, Địa chỉ giao hàng).  
2. Hệ thống kiểm tra số điện thoại của người mua trong bảng `CustomerProfiles`: Số điện thoại chưa tồn tại trong cơ sở dữ liệu.  
3. Hệ thống tự động tạo một bản ghi Hồ sơ khách hàng mới:  
   - Mã khách hàng: Tự động sinh theo mẫu `KH-XXXXX`.  
   - Họ và tên: Lấy từ tên người đặt đơn hàng.  
   - Số điện thoại: Số điện thoại đặt hàng (khóa định danh chính).  
   - Địa chỉ giao hàng đầu tiên: Lưu vào danh sách địa chỉ của khách hàng.  
   - Nhóm khách hàng mặc định: `Khách lẻ` (Retail Customer).  
   - Tổng chi tiêu tích lũy: Khởi tạo bằng giá trị đơn hàng đầu tiên (hoặc 0 nếu đơn chưa thanh toán).  
   - Ngày tạo hồ sơ: Ngày giờ tạo đơn hàng đầu tiên.  
4. Hệ thống liên kết Khóa ngoại `CustomerID` của đơn hàng với Hồ sơ khách hàng vừa tạo.  
5. Hệ thống hoàn tất việc tạo hồ sơ trong cùng một giao dịch (Transaction) của đơn hàng.  

**Alternative Flows**  
- **Số điện thoại đã tồn tại trong hệ thống:**  
  1. Hệ thống phát hiện số điện thoại đã có hồ sơ từ trước.  
  2. Hệ thống không tạo hồ sơ mới mà tự động gắn đơn hàng mới vào hồ sơ khách hàng đã có.  
  3. Hệ thống cập nhật thêm địa chỉ giao hàng mới vào sổ địa chỉ của khách nếu địa chỉ này chưa từng được lưu.  

**Postconditions**  
1. Hồ sơ khách hàng mới được lưu vĩnh viễn trong cơ sở dữ liệu.  
2. Đơn hàng được liên kết chặt chẽ với hồ sơ khách hàng.  

---

### UC-108 Liên kết tài khoản khách hàng (Link Customer Account)
**Primary Actors**  
System, Customer  

**Secondary Actors**  
SMS Service  

**Description**  
As a Customer, I want the system to link my new online user account with my existing in-store purchase profile based on verified phone number so that I can see all my past orders, warranties, and project quotes in one unified place.  

**Preconditions**  
1. Khách hàng đã từng mua hàng tại quầy và đã có hồ sơ khách hàng lưu tại cửa hàng (dựa trên Số điện thoại).  
2. Khách hàng thực hiện đăng ký tài khoản trực tuyến trên website bằng chính số điện thoại đó (UC-001).  

**Normal Flow**  
*Tự động đồng bộ và liên kết tài khoản website với hồ sơ mua hàng tại quầy*  
1. Khách hàng đăng ký tài khoản người dùng trực tuyến trên website và xác thực số điện thoại qua OTP thành công.  
2. Hệ thống kiểm tra trong bảng `CustomerProfiles` xem đã có hồ sơ nào trùng với Số điện thoại vừa xác thực hay chưa.  
3. Hệ thống tìm thấy hồ sơ khách hàng cũ tại quầy (trước đây chưa có `AccountID` liên kết).  
4. Hệ thống thực hiện liên kết trường `AccountID` của tài khoản web mới với bản ghi `CustomerProfile` cũ.  
5. Hệ thống đồng bộ toàn bộ lịch sử đơn hàng tại quầy, lịch sử báo giá công trình và thẻ bảo hành cũ vào tài khoản web của khách hàng.  
6. Hệ thống hiển thị thông báo chào mừng (MSG85: "Chào mừng quý khách trở lại! Toàn bộ lịch sử mua hàng và bảo hành trước đây tại quầy đã được đồng bộ vào tài khoản của bạn").  

**Alternative Flows**  
- **Không tìm thấy hồ sơ cũ:**  
  1. Số điện thoại chưa từng mua hàng tại cửa hàng.  
  2. Hệ thống tạo mới đồng thời cả Tài khoản đăng nhập lẫn Hồ sơ khách hàng mới.  
- **Hồ sơ cũ đã liên kết với một tài khoản email khác:**  
  1. Số điện thoại này trước đó đã được liên kết với một tài khoản trực tuyến khác.  
  2. Hệ thống thông báo cảnh báo và hướng dẫn khách hàng đăng nhập bằng tài khoản đã liên kết trước đó (MSG86: "Số điện thoại này đã được liên kết với tài khoản khác. Vui lòng đăng nhập lại").  

**Postconditions**  
Tài khoản người dùng và hồ sơ khách hàng được tích hợp 1-1, cho phép khách hàng tra cứu toàn bộ dữ liệu đa kênh.  

---

### UC-109 Tìm kiếm khách hàng (Search Customers)
**Primary Actors**  
Sales Staff, Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, Store Owner, or Accountant, I want to search and filter customers by name, phone number, customer tier, or building project name so that I can quickly access their file during sales consultation or payment collection.  

**Preconditions**  
Người dùng nội bộ đã đăng nhập vào hệ thống và đang ở trang "Quản lý khách hàng".  

**Normal Flow**  
*Tìm kiếm và tra cứu thông tin khách hàng nhanh chóng*  
1. Người dùng nhập từ khóa vào ô tìm kiếm: Số điện thoại (ví dụ: `0987654321`), Họ tên khách (ví dụ: `Anh Hùng`), hoặc Tên công trình (ví dụ: `Villa Ecopark`).  
2. Người dùng có thể chọn thêm bộ lọc:  
   - Phân loại nhóm: `Tất cả`, `Khách lẻ`, `Nhà thầu xây dựng`, `Thợ điện nước / Lắp đặt`, `Đại lý cấp dưới`.  
   - Tình trạng công nợ: `Tất cả`, `Có nợ quá hạn`, `Đang có dư nợ`, `Không có nợ`.  
3. Hệ thống thực hiện tìm kiếm tức thì và trả về danh sách khách hàng thỏa mãn:  
   - Cột Mã khách hàng, Tên khách hàng, Số điện thoại, Nhóm khách hàng, Số lượng công trình, Tổng chi tiêu lũy kế, Dư nợ hiện tại, Nút "Xem chi tiết".  
4. Người dùng nhấp vào khách hàng mong muốn để xem hồ sơ chi tiết hoặc chọn khách hàng vào đơn hàng mới.  

**Alternative Flows**  
- **Không tìm thấy khách hàng:**  
  1. Từ khóa tìm kiếm không trùng khớp với bất kỳ bản ghi nào.  
  2. Hệ thống hiển thị thông báo (MSG87: "Không tìm thấy khách hàng nào với thông tin trên") kèm nút "Tạo khách hàng mới" (kích hoạt UC-133).  

**Postconditions**  
Thông tin khách hàng cần tìm được hiển thị chính xác và nhanh chóng.  

---

### UC-110 Xem chi tiết & lịch sử khách hàng (View Customer Detail)
**Primary Actors**  
Sales Staff, Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, Store Owner, or Accountant, I want to view a 360-degree profile of a customer (personal info, building projects, full order history, quotes, active warranties, and debt ledger) so that I can provide personalized care and manage financial risk.  

**Preconditions**  
Người dùng đã đăng nhập và nhấp vào xem chi tiết của một khách hàng từ danh sách khách hàng (UC-109).  

**Normal Flow**  
*Xem hồ sơ toàn diện và lịch sử giao dịch của khách hàng*  
1. Hệ thống hiển thị trang "Hồ sơ chi tiết khách hàng - [Tên khách hàng] - [Mã KH]":  
   - Khối thông tin chung: Họ tên, Số điện thoại, Email, Địa chỉ chính, Nhóm khách hàng (Khách lẻ / Thợ / Nhà thầu), Chiết khấu áp dụng, Ghi chú đặc biệt.  
   - Tab 1: "Lịch sử đơn hàng" (Tất cả đơn hàng online và tại quầy, trạng thái giao, trạng thái thanh toán, tổng giá trị).  
   - Tab 2: "Báo giá công trình" (Danh sách các bản báo giá thiết bị vệ sinh đã lập, trạng thái duyệt).  
   - Tab 3: "Danh sách công trình" (Các địa điểm xây dựng/công trình của khách hàng - liên kết UC-112).  
   - Tab 4: "Sổ công nợ & Thanh toán" (Chi tiết lịch sử mua nợ, các đợt thanh toán, hạn nợ, số dư nợ hiện tại).  
   - Tab 5: "Thẻ bảo hành" (Tất cả sản phẩm và thiết bị đã mua còn hạn bảo hành kèm mã Serial).  
   - Tab 6: "Nhật ký chăm sóc" (Lịch sử các lần gọi điện, trao đổi, hẹn gặp - liên kết UC-141).  
2. Người dùng nhấp qua lại giữa các tab để tra cứu dữ liệu nghiệp vụ theo nhu cầu.  

**Alternative Flows**  
- **Khách hàng mới chưa có lịch sử giao dịch:**  
  1. Khách hàng vừa được tạo thủ công chưa phát sinh đơn hàng.  
  2. Các tab lịch sử hiển thị thông báo "Chưa có dữ liệu đơn hàng/báo giá".  

**Postconditions**  
Toàn bộ dữ liệu lịch sử liên quan đến khách hàng được cung cấp đầy đủ và minh bạch.  

---

### UC-111 Cập nhật thông tin khách hàng (Update Customer Information)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to edit a customer's contact details, shipping addresses, tax ID, or notes so that customer records remain accurate.  

**Preconditions**  
1. Người dùng đã đăng nhập và đang ở trang chi tiết khách hàng (UC-110).  

**Normal Flow**  
*Chỉnh sửa thông tin hồ sơ khách hàng*  
1. Người dùng nhấp vào nút "Chỉnh sửa thông tin" (Edit Profile).  
2. Hệ thống hiển thị biểu mẫu cho phép sửa các trường: Họ và tên, Số điện thoại phụ, Email, Mã số thuế doanh nghiệp (nếu có xuất hóa đơn VAT), Địa chỉ nhận hàng, Phân loại nhóm khách hàng, Ghi chú sở thích/thói quen mua hàng.  
3. Người dùng cập nhật các trường thông tin cần thiết.  
4. Người dùng nhấp nút "Lưu thay đổi".  
5. Hệ thống kiểm tra dữ liệu: Họ tên không để trống, Mã số thuế đúng định dạng 10 hoặc 13 số (nếu có nhập).  
6. Hệ thống cập nhật dữ liệu vào cơ sở dữ liệu.  
7. Hệ thống ghi nhận hành động chỉnh sửa hồ sơ vào Audit Log.  
8. Hệ thống hiển thị thông báo thành công (MSG88: "Cập nhật thông tin khách hàng thành công!") và tải lại màn hình chi tiết.  

**Alternative Flows**  
- **Chỉnh sửa số điện thoại chính:**  
  1. Số điện thoại chính là khóa định danh. Nếu đổi số điện thoại chính, hệ thống kiểm tra số mới không được trùng với bất kỳ khách hàng nào khác.  
  2. Nếu bị trùng -> Báo lỗi (MSG33: "Số điện thoại đã tồn tại cho một khách hàng khác").  

**Postconditions**  
Thông tin khách hàng được lưu mới chính xác trong hệ thống.  

---

### UC-112 Quản lý công trình của khách hàng (Manage Customer Projects)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to create and track construction projects linked to a customer (project name, site address, supervisor contact, stage of completion) so that the store can accurately organize multi-batch deliveries and project quotes.  

**Preconditions**  
1. Người dùng đã mở hồ sơ chi tiết của khách hàng (UC-110) và chọn tab "Công trình".  

**Normal Flow**  
*Thêm mới và quản lý địa điểm công trình xây dựng của khách hàng*  
1. Người dùng nhấp vào nút "Thêm công trình mới" (Add Project).  
2. Hệ thống hiển thị biểu mẫu bao gồm:  
   - Tên công trình (ví dụ: "Nhà phố 4 tầng - Lô B2", "Biệt thự Vinhome Ocean Park").  
   - Địa chỉ công trình chi tiết (Số nhà, Phường/Xã, Quận/Huyện, Tỉnh/Thành phố).  
   - Người giám sát / Phụ trách nhận hàng tại công trình (Họ tên, SĐT).  
   - Loại công trình (Nhà dân dụng, Khách sạn mini, Tòa văn phòng, Chung cư).  
   - Số lượng phòng tắm / phòng vệ sinh (ví dụ: 4 phòng vệ sinh).  
   - Giai đoạn thi công hiện tại (Đang xây thô, Đang đi ống nước âm tường, Đang ốp lát gạch, Chuẩn bị lắp thiết bị hoàn thiện).  
   - Ghi chú thi công (đặc điểm đường xe tải vào công trình, thời gian được phép bốc dỡ hàng).  
3. Người dùng nhập đầy đủ thông tin và nhấp nút "Lưu công trình".  
4. Hệ thống kiểm tra dữ liệu: Tên công trình và Địa chỉ không để trống, Số điện thoại người giám sát hợp lệ.  
5. Hệ thống lưu bản ghi công trình mới gắn liền với Khách hàng.  
6. Hệ thống hiển thị thông báo thành công (MSG89: "Thêm công trình cho khách hàng thành công!") và cập nhật danh sách công trình trong hồ sơ.  

**Alternative Flows**  
- **Chỉnh sửa thông tin công trình:**  
  1. Tại danh sách công trình, người dùng nhấp nút "Sửa".  
  2. Hệ thống mở biểu mẫu cập nhật giai đoạn thi công (ví dụ: chuyển từ "Đang xây thô" sang "Chuẩn bị lắp thiết bị").  
  3. Người dùng lưu lại -> Hệ thống cập nhật trạng thái mới.  
- **Xóa công trình:**  
  1. Người dùng bấm "Xóa" công trình không còn thực hiện.  
  2. Hệ thống kiểm tra: Nếu công trình chưa có đơn hàng hoặc báo giá nào phát sinh -> Cho phép xóa; Nếu đã có đơn hàng -> Không cho xóa mà chỉ cho phép đánh dấu "Đã hoàn thành thi công".  

**Postconditions**  
1. Bản ghi công trình được lưu trữ và sẵn sàng để chọn làm địa điểm giao hàng hoặc lập báo giá theo công trình.  

---

### UC-113 Khách xem lịch sử mua hàng (View My Purchase History)
**Primary Actors**  
Customer  

**Secondary Actors**  
None  

**Description**  
As a Customer, I want to view my aggregated purchase history (online and counter purchases, total spending, active warranties) in my personal account so that I can keep track of my equipment investments and warranties.  

**Preconditions**  
Khách hàng đã đăng nhập vào tài khoản trên website.  

**Normal Flow**  
*Khách hàng xem tổng hợp lịch sử giao dịch mua sắm của bản thân*  
1. Khách hàng truy cập vào mục "Lịch sử mua hàng" (Purchase History) trong tài khoản cá nhân.  
2. Hệ thống truy vấn toàn bộ dữ liệu đơn hàng đã hoàn tất (bao gồm cả đơn đặt qua website và đơn mua trực tiếp tại quầy đã được đồng bộ qua SĐT).  
3. Hệ thống hiển thị giao diện tổng quan:  
   - Khối thống kê: Tổng số đơn hàng đã mua, Tổng số tiền đã thanh toán, Số lượng thiết bị đang được bảo hành, Hạng thành viên hiện tại (ví dụ: Thành viên Thân thiết - Chiết khấu 5% cho lần mua kế tiếp).  
   - Bảng danh sách các đơn hàng đã mua thành công sắp xếp theo thời gian mới nhất: Mã đơn hàng, Ngày mua, Kênh mua (Online / Tại quầy), Danh sách thiết bị, Tổng tiền, Trạng thái (Đã giao hàng và thanh toán đủ).  
   - Cột thao tác: "Xem chi tiết đơn", "Tải hóa đơn", "Yêu cầu bảo hành nhanh" (UC-154).  
4. Khách hàng có thể tìm kiếm theo tên thiết bị đã từng mua hoặc khoảng thời gian.  

**Alternative Flows**  
- **Khách hàng chưa có đơn hàng hoàn tất nào:**  
  1. Khách hàng chưa mua thành công đơn hàng nào.  
  2. Hệ thống hiển thị thông báo rỗng kèm nút "Khám phá danh mục thiết bị vệ sinh".  

**Postconditions**  
Dữ liệu mua sắm đa kênh của khách hàng được trình bày rõ ràng, minh bạch.  

---

### UC-133 Tạo mới khách hàng thủ công (Create Customer Manually)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to manually create a new customer record along with initial project details at the counter or via hotline inquiry so that we can prepare quotes and manage interactions before an actual sale occurs.  

**Preconditions**  
1. Người dùng đã đăng nhập vào hệ thống bán hàng.  
2. Khách hàng liên hệ qua điện thoại, mạng xã hội hoặc đến trực tiếp tại cửa hàng để tham khảo và chưa có trên hệ thống.  

**Normal Flow**  
*Nhân viên chủ động tạo hồ sơ khách hàng mới*  
1. Tại màn hình Bán hàng tại quầy hoặc Quản lý khách hàng, nhân viên nhấp nút "Thêm khách hàng" (New Customer).  
2. Hệ thống hiển thị biểu mẫu "Thêm mới khách hàng":  
   - Mã khách hàng (Tự động sinh: `KH-XXXXX`).  
   - Họ và tên khách hàng (bắt buộc).  
   - Số điện thoại chính (bắt buộc, 10 số).  
   - Phân loại nhóm: `Khách lẻ`, `Chủ thầu xây dựng`, `Thợ điện nước / Lắp đặt`, `Kiến trúc sư`.  
   - Địa chỉ liên hệ.  
   - Tên công trình dự kiến (không bắt buộc, nếu có thì hệ thống tự tạo một Project liên kết luôn).  
   - Ghi chú ban đầu (ví dụ: "Khách đang hoàn thiện nhà ở Long Biên, dự kiến mua tháng sau").  
3. Nhân viên nhập các thông tin do khách hàng cung cấp.  
4. Nhân viên nhấp nút "Lưu hồ sơ khách hàng".  
5. Hệ thống kiểm tra dữ liệu: Số điện thoại không bị trùng với khách hàng đã có, Họ tên hợp lệ.  
6. Hệ thống lưu bản ghi khách hàng mới vào cơ sở dữ liệu.  
7. Hệ thống hiển thị thông báo thành công (MSG90: "Tạo mới khách hàng thành công!") và tự động chọn khách hàng này vào đơn hàng hoặc báo giá đang thao tác.  

**Alternative Flows**  
- **Số điện thoại bị trùng lặp:**  
  1. Số điện thoại nhập vào đã thuộc về một khách hàng khác.  
  2. Hệ thống cảnh báo (MSG50) và hiển thị tên khách hàng đang sở hữu số điện thoại đó, cho phép nhân viên chọn dùng lại hồ sơ cũ hoặc sửa lại số điện thoại.  

**Postconditions**  
Hồ sơ khách hàng mới sẵn sàng cho các quy trình tư vấn, báo giá và lập đơn hàng tiếp theo.  

---

### UC-141 Ghi nhật ký chăm sóc & tương tác khách hàng (Log Customer Interaction)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to record logs of interactions (phone calls, showroom visits, site surveys, follow-up reminders) with customers or building contractors so that the sales team maintains continuity of service.  

**Preconditions**  
1. Người dùng đã mở hồ sơ chi tiết của khách hàng (UC-110).  

**Normal Flow**  
*Ghi nhật ký tương tác và đặt lịch nhắc hẹn chăm sóc khách hàng*  
1. Người dùng chọn tab "Nhật ký chăm sóc" trong hồ sơ khách hàng và nhấp nút "Ghi nhận tương tác mới" (Add Interaction).  
2. Hệ thống hiển thị biểu mẫu ghi nhận:  
   - Hình thức tương tác: `Gọi điện thoại`, `Gặp trực tiếp tại cửa hàng`, `Khảo sát thực tế công trình`, `Trao đổi qua Zalo/Email`.  
   - Thời gian tương tác: Mặc định là thời gian hiện tại (cho phép sửa).  
   - Tóm tắt nội dung trao đổi (ví dụ: "Đã tư vấn lại về kích thước tâm xả 300mm của bồn cầu Toto, khách chốt đổi sang mẫu bồn cầu treo tường").  
   - Mức độ quan tâm của khách: `Rất tiềm năng (Hot)`, `Cân nhắc (Warm)`, `Chưa vội (Cold)`.  
   - Checkbox "Đặt lịch nhắc liên hệ lại" (Follow-up Reminder): Chọn Ngày hẹn gọi lại, Giờ hẹn và Nội dung cần trao đổi.  
3. Người dùng nhập đầy đủ nội dung và nhấp nút "Lưu nhật ký".  
4. Hệ thống kiểm tra nội dung trao đổi không để trống.  
5. Hệ thống lưu bản ghi tương tác vào lịch sử của khách hàng.  
6. Nếu có đặt lịch nhắc liên hệ lại, hệ thống tự động tạo một công việc nhắc nhở trong Lịch công việc của nhân viên vào đúng ngày giờ đã hẹn.  
7. Hệ thống hiển thị thông báo thành công (MSG91: "Đã lưu nhật ký tương tác thành công!").  

**Alternative Flows**  
- **Bỏ trống nội dung ghi chú:**  
  1. Người dùng chưa nhập tóm tắt trao đổi.  
  2. Hệ thống cảnh báo (MSG02: "Vui lòng nhập nội dung tóm tắt tương tác").  

**Postconditions**  
1. Lịch sử chăm sóc được lưu trữ đầy đủ.  
2. Lịch hẹn nhắc nhở được kích hoạt trong hệ thống để nhân viên không bỏ lỡ khách hàng.  

---

### UC-142 Quản lý nhóm khách hàng & thợ lắp đặt (Manage Customer Tiers & Contractors)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to categorize customers into specific tiers (Retail, Contractors, Plumbers/Electricians, Wholesale Dealers) and configure default discount rates so that orders automatically apply appropriate pricing policies.  

**Preconditions**  
Người dùng đã đăng nhập với vai trò `Store Owner` hoặc `Sales Staff` (có quyền quản trị chính sách khách hàng).  

**Normal Flow**  
*Thiết lập và quản lý phân hạng khách hàng kèm chính sách chiết khấu*  
1. Người dùng truy cập mục "Cấu hình nhóm khách hàng" trong Phân hệ Quản lý khách hàng.  
2. Hệ thống hiển thị danh sách các nhóm khách hàng hiện tại:  
   - `Khách lẻ`: Chiết khấu mặc định 0%.  
   - `Thợ điện nước / Lắp đặt`: Chiết khấu mặc định 8% trên giá niêm yết thiết bị, tích điểm hoa hồng giới thiệu.  
   - `Chủ thầu xây dựng`: Chiết khấu mặc định 12%, cho phép mua công nợ tối đa 30 ngày.  
   - `Đại lý cấp 2`: Chiết khấu mặc định 15%, hạn mức công nợ riêng.  
3. Người dùng có thể:  
   - Sửa mức chiết khấu và hạn mức công nợ của một nhóm.  
   - Hoặc gán một khách hàng cụ thể vào nhóm thợ/nhà thầu từ hồ sơ của họ.  
4. Người dùng cập nhật thông tin và nhấp nút "Lưu chính sách".  
5. Hệ thống kiểm tra tỷ lệ chiết khấu hợp lệ (từ 0% đến 50%).  
6. Hệ thống lưu cấu hình và ghi nhận Audit Log.  
7. Hệ thống hiển thị thông báo (MSG92: "Cập nhật chính sách nhóm khách hàng thành công!"). Từ thời điểm này, khi tạo đơn hàng mới cho khách hàng thuộc nhóm này, hệ thống sẽ tự động gợi ý mức chiết khấu tương ứng.  

**Alternative Flows**  
- **Tỷ lệ chiết khấu vượt ngưỡng an toàn:**  
  1. Người dùng nhập chiết khấu quá cao (> 50%).  
  2. Hệ thống cảnh báo nguy cơ lỗ vốn (MSG93: "Tỷ lệ chiết khấu vượt quá ngưỡng an toàn cho phép (50%). Vui lòng kiểm tra lại hoặc yêu cầu phê duyệt đặc biệt").  

**Postconditions**  
Phân loại nhóm khách hàng và tỷ lệ chiết khấu được cập nhật, sẵn sàng áp dụng tự động trong lập báo giá và đơn hàng (UC-048, UC-144).  

---
