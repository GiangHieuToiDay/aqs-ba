# PHÂN HỆ FE-01: QUẢN LÝ TÀI KHOẢN VÀ PHÂN QUYỀN

---

### UC-001 Khách đăng ký tài khoản (Register Account)
**Primary Actors**  
Guest  

**Secondary Actors**  
SMS Service  

**Description**  
As a Guest, I want to register a new customer account using my contact details and password so that I can log into the system, place online orders, track delivery progress, and manage warranty records.  

**Preconditions**  
1. Người dùng đang ở trạng thái chưa đăng nhập (Guest) và có kết nối Internet ổn định.  
2. Người dùng truy cập vào trang chủ hoặc bất kỳ trang công khai nào trên hệ thống website cửa hàng.  

**Normal Flow**  
*Đăng ký tài khoản khách hàng bằng số điện thoại*  
1. Người dùng nhấp vào nút "Đăng ký" (Register) trên thanh tiêu đề (Header) của website.  
2. Hệ thống hiển thị biểu mẫu gồm Số điện thoại, Mã xác minh OTP, Họ và tên, Mật khẩu và ô "Đồng ý với điều khoản sử dụng"; không có trường Email tại bước đăng ký.  
3. Người dùng nhập Số điện thoại di động Việt Nam gồm 10 chữ số, bắt đầu bằng 03, 05, 07, 08 hoặc 09 và nhấp "Gửi Mã xác minh".  
4. Hệ thống kiểm tra định dạng và tính duy nhất của Số điện thoại, sinh OTP gồm 6 chữ số, hiệu lực **180 giây**, rồi gửi qua `SMS Service`.  
5. Người dùng nhập OTP, Họ và tên (2–100 ký tự), Mật khẩu (tối thiểu 8 ký tự, có chữ hoa, chữ thường, số và ký tự đặc biệt), đánh dấu đồng ý điều khoản rồi nhấp "Đăng ký". Mật khẩu chỉ nhập một lần; ô mật khẩu có chức năng ẩn/hiện.  
6. Hệ thống kiểm tra dữ liệu, xác thực OTP còn hạn và chưa sử dụng, đồng thời kiểm tra lại tính duy nhất của Số điện thoại trước khi tạo tài khoản.  
7. Hệ thống tạo tài khoản `Customer` ở trạng thái `Active`, đánh dấu Số điện thoại đã xác thực, khởi tạo hồ sơ và mã khách hàng, ghi Activity Log, hủy OTP.  
8. Hệ thống cấp phiên đăng nhập, thông báo thành công (MSG01: "Đăng ký tài khoản thành công!") và chuyển về Trang chủ.  

**Alternative Flows**  
- **Step 3_Số điện thoại không hợp lệ hoặc đã đăng ký:** Không gửi SMS; hiển thị lỗi tại trường Số điện thoại và cho phép sửa.  
- **Step 4_Gửi OTP thất bại:** Không chuyển sang bước xác thực; thông báo gửi thất bại và cho phép thử lại theo giới hạn gửi mã.  
- **Step 4.1_Gửi lại OTP:** Sau 60 giây, người dùng được yêu cầu mã mới; mã cũ bị hủy. Tổng cộng tối đa 3 lần gửi trong 10 phút cho cùng Số điện thoại và mục đích đăng ký, tính cả lần gửi đầu tiên.  
- **Step 6_Dữ liệu không hợp lệ:** Hiển thị lỗi tương ứng nếu thiếu trường bắt buộc, Họ tên hoặc Mật khẩu sai chuẩn, hoặc chưa đồng ý điều khoản; giữ thông tin hợp lệ đã nhập để người dùng sửa.  
- **Step 6.1_OTP sai hoặc hết hạn:** Cho phép nhập sai tối đa 5 lần; sai lần thứ 5 hoặc quá 180 giây thì hủy mã và yêu cầu gửi mã mới.  
- **Step 6.2_Số điện thoại bị đăng ký trong lúc xác thực:** Từ chối tạo bản ghi mới, thông báo số đã được sử dụng và hướng dẫn đăng nhập hoặc khôi phục mật khẩu.  

**Postconditions**  
1. Tài khoản khách hàng mới được tạo thành công với vai trò `Customer` và trạng thái `Active`.  
2. Hồ sơ khách hàng (Customer Profile) tương ứng được khởi tạo trong hệ thống.  
3. Sự kiện đăng ký được ghi vết vào Activity Log.  

---

### UC-002 Đăng nhập (Log In)
**Primary Actors**  
Customer, Store Owner, Sales Staff, Technical Staff, Shipper, Accountant  

**Secondary Actors**  
SMS Service, Email Service  

**Description**  
As a registered user, I want to log into the system with my credentials so that I can access features, dashboards, and data corresponding to my assigned role.  

**Preconditions**  
1. Người dùng đã có tài khoản trên hệ thống: Customer ở trạng thái `Active`; nhân viên ở trạng thái `Active` hoặc `Pending` để đổi mật khẩu lần đầu.  
2. Người dùng chưa đăng nhập hoặc phiên làm việc trước đó đã hết hạn.  

**Normal Flow**  
*Đăng nhập bằng tài khoản và mật khẩu*  
1. Người dùng nhấp vào nút "Đăng nhập" (Log In) trên Header hoặc truy cập trực tiếp vào một đường dẫn yêu cầu xác thực.  
2. Hệ thống hiển thị cổng đăng nhập tương ứng: Customer dùng Số điện thoại và Mật khẩu; nhân viên dùng Email công việc và Mật khẩu.  
3. Người dùng nhập định danh và Mật khẩu hợp lệ.  
4. Người dùng nhấp nút "Đăng nhập".  
5. Hệ thống kiểm tra dữ liệu và đối soát thông tin đăng nhập trong cơ sở dữ liệu:  
   - Kiểm tra định dạng tài khoản và sự tồn tại của người dùng.  
   - Xác thực mật khẩu đã được mã hóa (Hash verification).  
   - Kiểm tra trạng thái tài khoản theo điều kiện tiên quyết; từ chối `Locked` và `Inactive`.  
6. Hệ thống sinh OTP 6 chữ số, hiệu lực **180 giây**; gửi qua SMS cho Customer hoặc Email công việc cho nhân viên. Chỉ OTP mới nhất có hiệu lực.  
7. Người dùng nhập đúng OTP còn hạn. Hệ thống hủy OTP sau khi xác thực thành công, cấp Access Token và Refresh Token, rồi ghi thời gian, IP và thiết bị vào Activity Log.  
8. Hệ thống điều hướng người dùng dựa theo vai trò và trạng thái:  
   - `Store Owner`, `Sales Staff`, `Accountant`: Chuyển hướng tới Trang quản trị (Admin/Staff Portal Dashboard).  
   - `Technical Staff`: Chuyển hướng tới Danh sách công việc kỹ thuật/bảo hành.  
   - `Shipper`: Chuyển hướng tới Danh sách đợt giao hàng được phân công.  
   - `Customer`: Chuyển hướng về Trang chủ (hoặc trang trước đó người dùng đang truy cập dở dang).  
   - Nhân viên `Pending` đăng nhập lần đầu: Chỉ cấp quyền tạm để đổi mật khẩu lần đầu; sau khi đổi thành công mới cấp quyền theo vai trò.  

**Alternative Flows**  
- **Step 5_Xác thực không thành công (Lỗi đăng nhập):**  
  1. Để trống định danh hoặc Mật khẩu: Hệ thống thông báo lỗi (MSG10: "Vui lòng nhập đầy đủ thông tin đăng nhập").  
  2. Sai tài khoản hoặc mật khẩu: Hệ thống hiển thị thông báo chung (MSG11: "Thông tin đăng nhập hoặc mật khẩu không chính xác").  
  3. Hệ thống tăng bộ đếm số lần đăng nhập sai liên tiếp lên 1 đơn vị.  
  4. Nếu số lần đăng nhập sai đạt ngưỡng 5 lần liên tiếp: Hệ thống tạm khóa đăng nhập trong **15 phút** và hiển thị cảnh báo (MSG12: "Bạn đã nhập sai 5 lần liên tiếp. Chức năng đăng nhập tạm khóa trong 15 phút vì lý do bảo mật").  
- **Step 5.1_Tài khoản bị khóa hoặc vô hiệu hóa:**  
  1. Người dùng nhập đúng thông tin tài khoản nhưng trạng thái tài khoản là `Locked` hoặc `Inactive`.  
  2. Hệ thống từ chối đăng nhập và hiển thị thông báo lỗi (MSG13: "Tài khoản của bạn đã bị khóa hoặc ngừng hoạt động. Vui lòng liên hệ Quản trị viên để được hỗ trợ").  
- **Step 6_Gửi OTP thất bại hoặc yêu cầu gửi lại:** Hệ thống chưa cấp phiên. Cho phép gửi lại sau 60 giây; tối đa 3 lần gửi trong 10 phút cho cùng định danh và mục đích đăng nhập, tính cả lần đầu. Mã mới hủy mã cũ.  
- **Step 7_OTP sai hoặc hết hạn:** Sai tối đa 5 lần; sai lần thứ 5 hoặc quá 180 giây thì hủy mã, yêu cầu gửi mã mới theo giới hạn ở bước 6.  

**Postconditions**  
1. Phiên đăng nhập hợp lệ được thiết lập và lưu trên trình duyệt người dùng.  
2. Người dùng truy cập các chức năng theo vai trò; nhân viên `Pending` chỉ được truy cập bước đổi mật khẩu lần đầu.  
3. Nhật ký đăng nhập thành công được lưu vào Activity Log.  

---

### UC-003 Đăng xuất (Log Out)
**Primary Actors**  
Customer, Store Owner, Sales Staff, Technical Staff, Shipper, Accountant  

**Secondary Actors**  
None  

**Description**  
As an authenticated user, I want to safely log out of the system so that I can terminate my session and prevent unauthorized access on the current device.  

**Preconditions**  
Người dùng hiện đang đăng nhập vào hệ thống trên trình duyệt/thiết bị hiện tại.  

**Normal Flow**  
*Đăng xuất an toàn khỏi hệ thống*  
1. Người dùng nhấp vào biểu tượng ảnh đại diện (Avatar) hoặc tên tài khoản tại góc trên bên phải màn hình.  
2. Hệ thống hiển thị menu người dùng thả xuống (Dropdown menu).  
3. Người dùng nhấp chọn mục "Đăng xuất" (Log Out).  
4. Hệ thống hiển thị hộp thoại xác nhận: "Bạn có chắc chắn muốn đăng xuất khỏi hệ thống?".  
5. Người dùng nhấp "Xác nhận".  
6. Hệ thống thực hiện thu hồi Token xác thực (Revoke JWT Refresh Token / Hủy Session ở phía máy chủ) và xóa sạch thông tin xác thực lưu tại Local Storage / Cookies của trình duyệt.  
7. Hệ thống ghi vết hành động đăng xuất của người dùng vào Activity Log.  
8. Hệ thống điều hướng người dùng về Trang chủ (đối với khách hàng) hoặc Trang đăng nhập chung (đối với nhân viên/quản trị viên), kèm thông báo nhẹ (MSG14: "Bạn đã đăng xuất thành công").  

**Alternative Flows**  
- **Step 5.1_Người dùng hủy bỏ đăng xuất:**  
  1. Người dùng nhấp vào nút "Hủy" hoặc nhấp ra ngoài hộp thoại xác nhận.  
  2. Hệ thống đóng hộp thoại xác nhận và giữ nguyên trạng thái làm việc hiện tại của người dùng.  
- **Step 6.1_Token đã hết hạn từ trước:**  
  1. Nếu phiên làm việc đã tự hết hạn trên server trước khi bấm đăng xuất.  
  2. Hệ thống vẫn dọn dẹp bộ nhớ đệm xác thực ở trình duyệt và chuyển hướng về màn hình đăng nhập mà không báo lỗi.  

**Postconditions**  
1. Phiên làm việc của người dùng bị hủy hoàn toàn trên cả máy chủ và máy khách.  
2. Người dùng không còn quyền truy cập vào các trang và chức năng bảo mật nếu chưa đăng nhập lại.  

---

### UC-004 Cập nhật hồ sơ cá nhân (Update Customer Profile)
**Primary Actors**  
Customer  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Customer, I want to update my personal information (full name, phone number, email, avatar, date of birth, gender) so that the store has my accurate details for order fulfillment and personalized customer service.  

**Preconditions**  
Khách hàng đã đăng nhập thành công và đang ở trang "Hồ sơ cá nhân".  

**Normal Flow**  
*Cập nhật thông tin hồ sơ khách hàng*  
1. Khách hàng nhấp vào biểu tượng tài khoản và chọn "Hồ sơ của tôi".  
2. Hệ thống hiển thị thông tin hồ sơ hiện tại: Ảnh đại diện, Họ và tên, Số điện thoại, Email, Ngày sinh, Giới tính.  
3. Khách hàng nhấp vào nút "Chỉnh sửa hồ sơ".  
4. Hệ thống mở các trường dữ liệu ở chế độ cho phép chỉnh sửa.  
5. Khách hàng thực hiện các thay đổi mong muốn:  
   - Cập nhật Họ và tên.  
   - Chọn tải lên ảnh đại diện mới (nếu có nhu cầu).  
   - Cập nhật Ngày sinh và Giới tính.  
6. Khách hàng nhấp nút "Lưu thay đổi".  
7. Hệ thống kiểm tra tính hợp lệ của các dữ liệu nhập (Họ tên không rỗng, định dạng ngày sinh hợp lệ và tuổi >= 16).  
8. (Nếu có tải ảnh mới): Hệ thống gọi Secondary Actor `Cloudinary` để tải tệp ảnh lên đám mây, nhận về URL ảnh an toàn.  
9. Hệ thống cập nhật thông tin hồ sơ mới vào cơ sở dữ liệu.  
10. Hệ thống ghi nhật ký cập nhật hồ sơ vào Activity Log.  
11. Hệ thống hiển thị thông báo thành công (MSG15: "Cập nhật hồ sơ cá nhân thành công!") và tải lại giao diện hiển thị thông tin mới.  

**Alternative Flows**  
- **Step 5.1_Khách hàng cập nhật Số điện thoại hoặc Email mới:**  
  1. Nếu khách hàng sửa Số điện thoại hoặc Email, hệ thống yêu cầu xác minh bảo mật.  
  2. Hệ thống hiển thị popup yêu cầu nhập mã OTP gửi tới Email/SĐT mới để xác thực quyền sở hữu.  
  3. Khách hàng nhập OTP hợp lệ -> Hệ thống ghi nhận cập nhật thông tin liên hệ mới.  
  4. Nếu OTP sai quá 3 lần -> Hệ thống hủy yêu cầu đổi thông tin liên hệ và giữ nguyên dữ liệu cũ.  
- **Step 7_Dữ liệu không hợp lệ:**  
  1. Họ tên để trống: Hệ thống hiển thị lỗi (MSG16: "Họ và tên không được để trống").  
  2. Ngày sinh không hợp lệ (ngày tương lai): Hệ thống hiển thị lỗi (MSG17: "Ngày sinh không hợp lệ").  
  3. Ảnh đại diện sai định dạng (không phải JPG, PNG, WEBP) hoặc dung lượng vượt quá 5MB: Hệ thống báo lỗi (MSG18: "Ảnh đại diện phải thuộc định dạng JPG/PNG/WEBP và dung lượng dưới 5MB").  
  4. Khách hàng chỉnh sửa lại và tiếp tục từ bước 6 của Normal Flow.  
- **Step 8.1_Lỗi tải ảnh lên Cloudinary:**  
  1. Dịch vụ Cloudinary gặp sự cố đường truyền hoặc hết hạn ngạch.  
  2. Hệ thống bắt lỗi ngoại lệ, thông báo cho người dùng (MSG19: "Không thể tải ảnh lên máy chủ lưu trữ lúc này, vui lòng thử lại sau"). Giữ nguyên ảnh cũ.  

**Postconditions**  
1. Dữ liệu hồ sơ của khách hàng trong hệ thống được cập nhật chính xác.  
2. Thông tin mới được áp dụng ngay lập tức cho các giao dịch và hiển thị tiếp theo.  

---

### UC-005 Quản lý địa chỉ giao hàng (Manage Delivery Addresses)
**Primary Actors**  
Customer  

**Secondary Actors**  
None  

**Description**  
As a Customer, I want to add, edit, delete, and set default shipping addresses so that I can quickly select delivery destinations during checkout.  

**Preconditions**  
Khách hàng đã đăng nhập thành công và truy cập vào mục "Sổ địa chỉ nhận hàng" trong trang tài khoản.  

**Normal Flow**  
*Thêm địa chỉ giao hàng mới và đặt làm mặc định*  
1. Khách hàng chọn tab "Sổ địa chỉ" trong trang tài khoản cá nhân.  
2. Hệ thống hiển thị danh sách các địa chỉ hiện có kèm nhãn "Mặc định" (nếu có) và nút "Thêm địa chỉ mới".  
3. Khách hàng nhấp vào nút "Thêm địa chỉ mới".  
4. Hệ thống hiển thị biểu mẫu bao gồm: Họ tên người nhận, Số điện thoại người nhận, Tỉnh/Thành phố, Quận/Huyện, Phường/Xã, Địa chỉ cụ thể (số nhà, ngõ/ngách, tên đường), Ghi chú giao hàng và checkbox "Đặt làm địa chỉ mặc định".  
5. Khách hàng nhập đầy đủ thông tin địa chỉ và tích chọn "Đặt làm địa chỉ mặc định".  
6. Khách hàng nhấp nút "Lưu địa chỉ".  
7. Hệ thống kiểm tra dữ liệu: Tên người nhận không rỗng, SĐT đủ 10 số hợp lệ, Tỉnh/Quận/Xã được chọn hợp lệ từ danh mục hành chính, địa chỉ chi tiết không để trống.  
8. Hệ thống lưu địa chỉ mới vào bảng địa chỉ giao hàng của khách hàng. Do được chọn là mặc định, hệ thống tự động gỡ bỏ cờ mặc định của địa chỉ cũ (nếu có).  
9. Hệ thống hiển thị thông báo thành công (MSG20: "Thêm địa chỉ nhận hàng thành công!") và cập nhật danh sách địa chỉ trên màn hình.  

**Alternative Flows**  
- **Step 3.1_Chỉnh sửa địa chỉ đã có:**  
  1. Tại danh sách địa chỉ, khách hàng nhấp nút "Sửa" tại một địa chỉ mong muốn.  
  2. Hệ thống hiển thị popup với dữ liệu cũ của địa chỉ đó.  
  3. Khách hàng thay đổi thông tin và nhấp "Cập nhật".  
  4. Hệ thống kiểm tra dữ liệu hợp lệ và lưu cập nhật vào cơ sở dữ liệu.  
- **Step 3.2_Xóa địa chỉ giao hàng:**  
  1. Tại danh sách địa chỉ, khách hàng nhấp nút "Xóa" tại một địa chỉ (không phải địa chỉ mặc định duy nhất).  
  2. Hệ thống hiển thị hộp thoại xác nhận: "Bạn có chắc chắn muốn xóa địa chỉ nhận hàng này?".  
  3. Khách hàng nhấp "Đồng ý xóa".  
  4. Hệ thống thực hiện xóa mềm (Soft delete) địa chỉ khỏi danh sách và hiển thị thông báo (MSG21: "Đã xóa địa chỉ thành công").  
- **Step 3.3_Thiết lập địa chỉ mặc định trực tiếp:**  
  1. Tại một địa chỉ phụ trong danh sách, khách hàng nhấp "Thiết lập mặc định".  
  2. Hệ thống cập nhật địa chỉ này thành mặc định và hủy trạng thái mặc định của địa chỉ cũ.  
- **Step 7_Dữ liệu nhập không hợp lệ:**  
  1. Số điện thoại người nhận không hợp lệ hoặc để trống: Hệ thống hiển thị cảnh báo (MSG22: "Số điện thoại người nhận không hợp lệ").  
  2. Chưa chọn đầy đủ Tỉnh/Thành phố, Quận/Huyện, Phường/Xã: Hệ thống hiển thị cảnh báo (MSG23: "Vui lòng chọn đầy đủ thông tin khu vực giao hàng").  
- **Step 3.2.1_Xóa địa chỉ đang là mặc định duy nhất:**  
  1. Khách hàng bấm xóa địa chỉ duy nhất đang có trên hệ thống hoặc địa chỉ mặc định khi chưa có địa chỉ thay thế.  
  2. Hệ thống từ chối xóa và hiển thị cảnh báo (MSG24: "Không thể xóa địa chỉ mặc định duy nhất. Vui lòng thiết lập một địa chỉ khác làm mặc định trước").  

**Postconditions**  
1. Danh sách địa chỉ giao hàng của khách hàng được cập nhật.  
2. Địa chỉ mặc định mới sẵn sàng tự động điền khi tạo đơn hàng mới.  

---

### UC-006 Đổi mật khẩu (Change Password)
**Primary Actors**  
Customer, Store Owner, Sales Staff, Technical Staff, Shipper, Accountant  

**Secondary Actors**  
None  

**Description**  
As an authenticated user, I want to change my account password by providing my current password and a new password so that I can secure my account periodically.  

**Preconditions**  
Người dùng đã đăng nhập thành công vào hệ thống và đang ở trang cài đặt bảo mật tài khoản.  

**Normal Flow**  
*Đổi mật khẩu tài khoản người dùng*  
1. Người dùng chọn mục "Bảo mật & Đổi mật khẩu" trong cài đặt tài khoản.  
2. Hệ thống hiển thị biểu mẫu đổi mật khẩu gồm 3 trường: "Mật khẩu hiện tại", "Mật khẩu mới", "Xác nhận mật khẩu mới".  
3. Người dùng nhập mật khẩu hiện tại đang dùng.  
4. Người dùng nhập mật khẩu mới và nhập lại chính xác mật khẩu mới vào ô xác nhận.  
5. Người dùng nhấp nút "Cập nhật mật khẩu".  
6. Hệ thống thực hiện kiểm tra nghiệp vụ:  
   - Kiểm tra mật khẩu hiện tại trùng khớp với mật khẩu đã mã hóa lưu trong cơ sở dữ liệu.  
   - Kiểm tra mật khẩu mới tuân thủ chính sách bảo mật (>= 8 ký tự, chữ hoa, chữ thường, số, ký tự đặc biệt).  
   - Kiểm tra mật khẩu mới không được trùng với mật khẩu hiện tại.  
   - Kiểm tra xác nhận mật khẩu mới trùng khớp 100%.  
7. Hệ thống thực hiện băm mật khẩu mới (Bcrypt/Argon2 hashing) và cập nhật vào cơ sở dữ liệu.  
8. Hệ thống hủy toàn bộ các phiên đăng nhập khác của tài khoản này trên các thiết bị khác (nếu có) để đảm bảo an toàn.  
9. Hệ thống ghi nhận sự kiện đổi mật khẩu thành công vào Activity Log.  
10. Hệ thống hiển thị thông báo thành công (MSG25: "Đổi mật khẩu thành công! Vui lòng sử dụng mật khẩu mới cho lần đăng nhập tiếp theo").  

**Alternative Flows**  
- **Step 6_Mật khẩu hiện tại không chính xác:**  
  1. Người dùng nhập sai mật khẩu hiện tại.  
  2. Hệ thống hiển thị thông báo lỗi (MSG26: "Mật khẩu hiện tại không chính xác").  
  3. Hệ thống không lưu thay đổi và người dùng phải nhập lại.  
- **Step 6.1_Mật khẩu mới trùng với mật khẩu cũ:**  
  1. Người dùng nhập mật khẩu mới giống hệt mật khẩu hiện tại.  
  2. Hệ thống hiển thị thông báo lỗi (MSG27: "Mật khẩu mới không được trùng với mật khẩu đang sử dụng").  
- **Step 6.2_Mật khẩu mới không đạt chuẩn bảo mật hoặc không khớp xác nhận:**  
  1. Hệ thống hiển thị lỗi tương ứng (MSG04 hoặc MSG05).  
  2. Người dùng nhập lại mật khẩu hợp lệ.  

**Postconditions**  
1. Mật khẩu của tài khoản được cập nhật giá trị mới.  
2. Các phiên làm việc cũ trên thiết bị khác bị thu hồi quyền truy cập.  

---

### UC-007 Khách hàng yêu cầu đặt lại mật khẩu (Reset Customer Password Request)
**Primary Actors**  
Customer  

**Secondary Actors**  
SMS Service  

**Description**  
As a Customer who forgot my password, I want to verify my registered phone number with an SMS OTP so that I can securely set a new password and regain access to my account.  

**Preconditions**  
Người dùng không nhớ mật khẩu và đang ở màn hình Đăng nhập của hệ thống.  

**Normal Flow**  
*Khôi phục mật khẩu Customer qua Số điện thoại và SMS OTP*  
1. Người dùng nhấp vào liên kết "Quên mật khẩu?" trên màn hình Đăng nhập.  
2. Hệ thống hiển thị màn hình "Khôi phục mật khẩu" yêu cầu Số điện thoại đã đăng ký và Mã xác minh OTP.  
3. Người dùng nhập Số điện thoại rồi nhấp "Gửi mã".  
4. Hệ thống kiểm tra định dạng Số điện thoại; nếu có tài khoản Customer `Active` tương ứng, hệ thống sinh OTP 6 chữ số, hiệu lực **180 giây**, gửi qua `SMS Service`. Phản hồi giao diện không tiết lộ Số điện thoại có tồn tại hay không.  
5. Người dùng nhập OTP rồi nhấp "Tiếp tục". Hệ thống xác thực mã còn hạn, chưa sử dụng và hủy mã sau khi xác thực thành công.  
6. Hệ thống cấp `RESET_PASSWORD_TOKEN` dùng một lần, hiệu lực **15 phút**, rồi mở màn "Thiết lập mật khẩu mới".  
7. Người dùng nhập Mật khẩu mới một lần và nhấp "Xác nhận thay đổi".  
8. Hệ thống kiểm tra token, chuẩn mật khẩu và mật khẩu mới không trùng mật khẩu hiện tại bằng hàm verify/compare; sau đó băm mật khẩu mới, lưu vào cơ sở dữ liệu, hủy token và thu hồi các phiên đăng nhập cũ.  
9. Hệ thống ghi Activity Log, thông báo thành công (MSG29: "Đặt lại mật khẩu thành công! Bạn có thể đăng nhập ngay với mật khẩu mới") và chuyển về màn Đăng nhập.  

**Alternative Flows**  
- **Step 4_Số điện thoại không tồn tại hoặc tài khoản không hoạt động:** Không gửi OTP; hiển thị cùng thông báo chung với trường hợp đã tiếp nhận yêu cầu để không lộ tình trạng tài khoản.  
- **Step 4.1_Gửi lại OTP:** Cho phép gửi lại sau 60 giây, tối đa 3 lần gửi/10 phút cho cùng Số điện thoại và mục đích khôi phục, tính cả lần đầu; mã mới hủy mã cũ.  
- **Step 5_OTP sai hoặc hết hạn:** Sai lần thứ 5 hoặc quá 180 giây thì hủy mã; yêu cầu mã mới theo giới hạn gửi mã.  
- **Step 6.1_Token hết hạn hoặc không hợp lệ:** Quá 15 phút hoặc đã sử dụng thì từ chối cập nhật mật khẩu, hướng dẫn thực hiện lại từ bước nhập Số điện thoại.  
- **Step 4.2_SMS Service gặp sự cố:** Ghi log lỗi, thông báo chưa thể gửi mã và cho phép thử lại theo giới hạn gửi mã.  

**Postconditions**  
1. Mật khẩu tài khoản người dùng được cập nhật mới.  
2. Token khôi phục mật khẩu bị hủy bỏ.  
3. Người dùng có thể sử dụng mật khẩu mới để đăng nhập.  

---

### UC-008 Tạo tài khoản nhân viên (Create Staff Account)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Email Service  

**Description**  
As a Store Owner, I want to create a new staff account and assign an initial role (Sales Staff, Technical Staff, Shipper, Accountant) so that the new employee can access the management system to perform their job duties.  

**Preconditions**  
1. Người dùng đã đăng nhập với vai trò `Store Owner`.  
2. Người dùng đang ở trang "Quản lý nhân viên" trong Phân hệ Quản trị.  

**Normal Flow**  
*Chủ cửa hàng tạo tài khoản nhân viên mới*  
1. Chủ cửa hàng nhấp vào nút "Thêm nhân viên mới" (Create Staff).  
2. Hệ thống hiển thị biểu mẫu tạo tài khoản gồm các trường: Mã nhân viên (tự động sinh theo định dạng `NV-XXXX`), Họ và tên, Số điện thoại, Email công việc, Vai trò ban đầu (chọn từ danh sách vai trò: `Sales Staff`, `Technical Staff`, `Shipper`, `Accountant`), Ghi chú chức vụ.  
3. Chủ cửa hàng nhập các thông tin của nhân viên và chọn vai trò tương ứng.  
4. Chủ cửa hàng nhấp nút "Lưu và tạo tài khoản".  
5. Hệ thống kiểm tra dữ liệu:  
   - Họ tên không để trống.  
   - Số điện thoại đúng 10 số và chưa từng tồn tại trên hệ thống.  
   - Email đúng định dạng và chưa tồn tại trên hệ thống.  
   - Vai trò được chọn là một vai trò hợp lệ đang hoạt động.  
6. Hệ thống tự động sinh một mật khẩu ngẫu nhiên tạm thời (8 ký tự phức tạp).  
7. Hệ thống tạo tài khoản mới trong cơ sở dữ liệu với trạng thái `Active`, gán cờ `MustChangePasswordOnFirstLogin = True`.  
8. Hệ thống gọi Secondary Actor `Email Service` gửi thông tin tài khoản đăng nhập (Email và Mật khẩu tạm thời) kèm hướng dẫn truy cập tới email của nhân viên mới.  
9. Hệ thống ghi nhận hành động tạo nhân viên vào Audit Log (ghi rõ ID người tạo, ID nhân viên mới, vai trò được gán).  
10. Hệ thống hiển thị thông báo thành công (MSG32: "Tạo tài khoản nhân viên thành công! Thông tin đăng nhập đã được gửi tới email của nhân viên") và cập nhật danh sách nhân viên.  

**Alternative Flows**  
- **Step 5_Email hoặc Số điện thoại của nhân viên đã tồn tại:**  
  1. Hệ thống phát hiện email hoặc số điện thoại bị trùng với tài khoản đã có.  
  2. Hệ thống dừng quy trình và thông báo lỗi (MSG33: "Email hoặc Số điện thoại đã được đăng ký cho một tài khoản khác trong hệ thống").  
  3. Chủ cửa hàng kiểm tra lại thông tin và nhập lại.  
- **Step 5.1_Thiếu thông tin bắt buộc:**  
  1. Chưa chọn vai trò hoặc bỏ trống họ tên: Hệ thống hiển thị lỗi tại trường tương ứng (MSG02: "Vui lòng nhập đầy đủ các trường bắt buộc").  
- **Step 8.1_Gửi email thông tin mật khẩu tạm thất bại:**  
  1. Dịch vụ email không gửi được thư do lỗi kết nối.  
  2. Hệ thống vẫn tạo tài khoản thành công nhưng hiển thị popup mật khẩu tạm trên màn hình của Store Owner kèm thông báo (MSG34: "Tài khoản đã tạo nhưng gửi email thất bại. Mật khẩu tạm thời là: [GeneratedPassword]. Vui lòng sao chép và gửi trực tiếp cho nhân viên").  

**Postconditions**  
1. Tài khoản nhân viên mới được lưu vào hệ thống ở trạng thái hoạt động với vai trò tương ứng.  
2. Nhân viên nhận được thông tin để đăng nhập.  
3. Hành động được lưu vào Audit Log hệ thống.  

---

### UC-009 Gán/thay đổi vai trò nhân viên (Assign Staff Role)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to change or assign roles to a staff member so that their system permissions match their updated job responsibilities.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập vào hệ thống.  
2. Tài khoản nhân viên mục tiêu đang tồn tại trên hệ thống.  

**Normal Flow**  
*Thay đổi vai trò của nhân viên*  
1. Chủ cửa hàng truy cập danh sách nhân viên, tìm đến nhân viên cần phân quyền và nhấp vào nút "Phân quyền/Đổi vai trò".  
2. Hệ thống hiển thị cửa sổ "Phân vai trò nhân viên", thể hiện thông tin hiện tại: Họ tên, Mã nhân viên, Vai trò hiện tại, cùng danh sách các vai trò khả dụng trong hệ thống.  
3. Chủ cửa hàng chọn vai trò mới từ danh sách (ví dụ: Chuyển từ `Sales Staff` sang `Accountant`) và nhập lý do thay đổi (bắt buộc).  
4. Chủ cửa hàng nhấp nút "Lưu thay đổi quyền".  
5. Hệ thống kiểm tra: Lý do thay đổi không để trống (tối thiểu 10 ký tự), vai trò mới khác vai trò hiện tại.  
6. Hệ thống cập nhật vai trò mới cho tài khoản nhân viên trong cơ sở dữ liệu.  
7. Hệ thống làm mới ngay lập tức quyền hạn (Permissions) của nhân viên; nếu nhân viên đang có phiên đăng nhập trực tuyến, hệ thống cập nhật Token phiên làm việc hoặc yêu cầu đăng nhập lại ở yêu cầu API kế tiếp.  
8. Hệ thống ghi vết chi tiết hành động vào Audit Log: Người thực hiện (Store Owner), Đối tượng (Mã nhân viên), Vai trò cũ, Vai trò mới, Lý do thay đổi.  
9. Hệ thống hiển thị thông báo thành công (MSG35: "Cập nhật vai trò nhân viên thành công!") và làm mới giao diện.  

**Alternative Flows**  
- **Step 5_Không nhập lý do thay đổi:**  
  1. Chủ cửa hàng bỏ trống ô lý do thay đổi.  
  2. Hệ thống báo lỗi (MSG36: "Vui lòng nhập lý do điều chỉnh vai trò/quyền hạn để phục vụ kiểm toán hệ thống").  
  3. Chủ cửa hàng nhập lý do và bấm Lưu lại.  
- **Step 5.1_Thao tác thay đổi chính vai trò của Store Owner:**  
  1. Chủ cửa hàng cố tình hạ quyền hoặc gỡ vai trò của tài khoản Store Owner gốc.  
  2. Hệ thống chặn thao tác và thông báo lỗi (MSG37: "Không thể thay đổi hoặc thu hồi vai trò Quản trị viên cấp cao nhất của tài khoản này").  

**Postconditions**  
1. Vai trò của nhân viên được cập nhật trong cơ sở dữ liệu.  
2. Tập quyền hạn mới có hiệu lực ngay lập tức khi nhân viên thao tác trên hệ thống.  
3. Bản ghi thay đổi được lưu đầy đủ vào Audit Log.  

---

### UC-010 Khóa/Mở khóa tài khoản nhân viên (Lock/Unlock Staff Account)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to lock or unlock a staff account so that I can immediately revoke system access when an employee takes leave or resigns, or restore access when they return to work.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập thành công với vai trò `Store Owner`.  
2. Tài khoản nhân viên cần thao tác đang tồn tại trên hệ thống.  

**Normal Flow**  
*Khóa tài khoản nhân viên đang hoạt động*  
1. Chủ cửa hàng truy cập màn hình danh sách nhân viên.  
2. Chủ cửa hàng tìm kiếm nhân viên cần khóa và nhấp vào nút "Khóa tài khoản" (Lock Account).  
3. Hệ thống hiển thị hộp thoại xác nhận: "Bạn có chắc chắn muốn khóa tài khoản của nhân viên [Tên nhân viên] - [Mã nhân viên]?". Đồng thời yêu cầu nhập trường "Lý do khóa tài khoản" (bắt buộc).  
4. Chủ cửa hàng nhập lý do (ví dụ: "Nhân viên tạm nghỉ thai sản" hoặc "Đã chấm dứt hợp đồng lao động") và nhấp nút "Xác nhận khóa".  
5. Hệ thống kiểm tra lý do không để trống.  
6. Hệ thống chuyển trạng thái tài khoản nhân viên từ `Active` sang `Locked`.  
7. Hệ thống ngay lập tức thu hồi và vô hiệu hóa tất cả các phiên đăng nhập (Refresh Token / Active Sessions) hiện có của nhân viên này trên mọi thiết bị.  
8. Hệ thống ghi nhận hành động vào Audit Log (Thời gian, Store Owner ID, Staff ID, Trạng thái cũ: Active, Trạng thái mới: Locked, Lý do).  
9. Hệ thống hiển thị thông báo thành công (MSG38: "Tài khoản nhân viên đã được khóa thành công!") và cập nhật nhãn trạng thái trên danh sách thành `Đã khóa`.  

**Alternative Flows**  
- **Mở khóa tài khoản nhân viên (Unlock Account Flow):**  
  1. Tại bước 2, đối với nhân viên đang có trạng thái `Locked`, Chủ cửa hàng nhấp vào nút "Mở khóa tài khoản" (Unlock Account).  
  2. Hệ thống hiển thị hộp thoại xác nhận mở khóa kèm trường nhập "Lý do mở khóa".  
  3. Chủ cửa hàng nhập lý do và nhấp "Xác nhận mở khóa".  
  4. Hệ thống cập nhật trạng thái tài khoản từ `Locked` sang `Active`.  
  5. Hệ thống ghi nhật ký mở khóa vào Audit Log.  
  6. Hệ thống hiển thị thông báo (MSG39: "Tài khoản nhân viên đã được mở khóa và có thể đăng nhập bình thường").  
- **Step 5_Không nhập lý do thực hiện:**  
  1. Chủ cửa hàng để trống ô lý do khóa/mở khóa.  
  2. Hệ thống hiển thị cảnh báo (MSG40: "Vui lòng nhập lý do để tiếp tục").  
- **Khóa tài khoản chính mình:**  
  1. Chủ cửa hàng vô tình thực hiện thao tác khóa tài khoản của chính mình.  
  2. Hệ thống kiểm tra ID tài khoản và từ chối xử lý (MSG41: "Bạn không thể tự khóa tài khoản quản trị của chính mình").  

**Postconditions**  
1. Trạng thái tài khoản của nhân viên được chuyển sang `Locked` (hoặc `Active`).  
2. Mọi phiên truy cập bị chặn hoàn toàn khi bị khóa.  
3. Audit Log được ghi vết đầy đủ.  

---

### UC-011 Cập nhật thông tin nhân viên (Update Staff Account)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to edit a staff member's profile and contact details so that employee records remain accurate and up-to-date.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập vào hệ thống.  
2. Tài khoản nhân viên cần sửa thông tin tồn tại trong hệ thống.  

**Normal Flow**  
*Cập nhật hồ sơ tài khoản nhân viên*  
1. Chủ cửa hàng truy cập màn hình danh sách nhân viên và nhấp nút "Chỉnh sửa" tại bản ghi của nhân viên tương ứng.  
2. Hệ thống hiển thị màn hình "Cập nhật hồ sơ nhân viên" chứa các thông tin hiện tại: Ảnh chân dung, Mã nhân viên (Read-only), Họ và tên, Số điện thoại, Email, Ngày sinh, Địa chỉ cư trú, Ngày bắt đầu làm việc.  
3. Chủ cửa hàng chỉnh sửa các trường thông tin cần cập nhật (ví dụ: đổi số điện thoại mới, cập nhật địa chỉ cư trú, tải ảnh mới).  
4. Chủ cửa hàng nhấp nút "Lưu cập nhật".  
5. Hệ thống kiểm tra dữ liệu:  
   - Họ tên không rỗng.  
   - Số điện thoại đúng định dạng và nếu đổi số mới thì không được trùng với tài khoản khác trong hệ thống.  
   - Email hợp lệ và không trùng lặp.  
6. (Nếu có ảnh mới): Hệ thống gọi `Cloudinary` để tải ảnh lên và lưu URL mới.  
7. Hệ thống cập nhật bản ghi nhân viên trong cơ sở dữ liệu.  
8. Hệ thống ghi nhận sự kiện cập nhật vào Audit Log (ghi chi tiết các trường dữ liệu có thay đổi).  
9. Hệ thống thông báo thành công (MSG42: "Cập nhật thông tin nhân viên thành công!") và quay lại màn hình chi tiết nhân viên.  

**Alternative Flows**  
- **Step 5_Số điện thoại hoặc Email bị trùng lặp:**  
  1. Số điện thoại mới vừa nhập trùng với một nhân viên hoặc khách hàng khác.  
  2. Hệ thống báo lỗi (MSG33: "Số điện thoại hoặc Email đã tồn tại trên hệ thống").  
  3. Chủ cửa hàng kiểm tra lại và nhập thông tin chính xác.  
- **Step 4.1_Chủ cửa hàng hủy chỉnh sửa:**  
  1. Chủ cửa hàng nhấp nút "Hủy bỏ".  
  2. Hệ thống hủy các thay đổi chưa lưu và quay lại màn hình danh sách nhân viên mà không lưu bất kỳ thay đổi nào.  

**Postconditions**  
1. Thông tin hồ sơ nhân viên được cập nhật trong cơ sở dữ liệu.  
2. Audit Log ghi nhận lịch sử các trường thay đổi.  

---

### UC-012 Xem danh sách nhân viên (View Staff Account List)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to view, search, and filter the list of staff accounts by role and status so that I can easily oversee all personnel in the store.  

**Preconditions**  
Chủ cửa hàng đã đăng nhập và có quyền truy cập vào Phân hệ Quản trị nhân sự.  

**Normal Flow**  
*Xem và tra cứu danh sách tài khoản nhân viên*  
1. Chủ cửa hàng nhấp vào menu "Quản lý nhân viên" trên thanh điều hướng bên trái (Sidebar).  
2. Hệ thống truy vấn cơ sở dữ liệu và hiển thị trang "Danh sách nhân viên" dạng bảng lưới, bao gồm:  
   - Bộ lọc: Ô tìm kiếm từ khóa, Bộ lọc theo Vai trò (`All`, `Sales Staff`, `Technical Staff`, `Shipper`, `Accountant`), Bộ lọc theo Trạng thái (`All`, `Active`, `Locked`).  
   - Bảng dữ liệu gồm các cột: Ảnh đại diện, Mã NV, Họ và tên, Số điện thoại, Email, Vai trò, Ngày tham gia, Trạng thái (Huy hiệu màu: Xanh cho `Active`, Đỏ cho `Locked`), và Cột Thao tác (Xem chi tiết, Sửa, Đổi vai trò, Khóa/Mở khóa, Reset mật khẩu).  
   - Thanh phân trang (mặc định 10 hoặc 20 bản ghi/trang) và tổng số nhân viên.  
3. Mặc định hệ thống sắp xếp danh sách theo ngày tạo mới nhất lên đầu.  
4. Chủ cửa hàng có thể duyệt danh sách qua các trang kế tiếp hoặc bấm vào từng dòng để thao tác các tính năng tương ứng.  

**Alternative Flows**  
- **Danh sách nhân viên trống:**  
  1. Nếu cửa hàng chưa tạo nhân viên nào ngoài tài khoản chủ cửa hàng.  
  2. Hệ thống hiển thị bảng trống kèm thông báo trực quan (MSG43: "Chưa có tài khoản nhân viên nào. Hãy bấm Thêm nhân viên mới để bắt đầu").  
- **Lỗi kết nối cơ sở dữ liệu:**  
  1. Không thể tải dữ liệu do lỗi mạng hoặc máy chủ.  
  2. Hệ thống hiển thị thông báo lỗi (MSG44: "Không thể tải danh sách nhân viên lúc này. Vui lòng tải lại trang").  

**Postconditions**  
Hệ thống hiển thị danh sách nhân viên đầy đủ và chính xác theo quyền hạn của Store Owner.  

---

### UC-013 Xem chi tiết tài khoản nhân viên (View Staff Account Detail)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to view the detailed profile and activity history of a specific staff member so that I can evaluate their operational involvement and account status.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập vào hệ thống.  
2. Chủ cửa hàng đang ở màn hình Danh sách nhân viên.  

**Normal Flow**  
*Xem chi tiết thông tin hồ sơ và nhật ký làm việc của một nhân viên*  
1. Chủ cửa hàng nhấp vào tên hoặc nút "Xem chi tiết" (Icon con mắt) của một nhân viên trong danh sách.  
2. Hệ thống điều hướng sang trang "Chi tiết nhân viên - [Mã NV]".  
3. Hệ thống hiển thị các khối thông tin chi tiết:  
   - Khối 1: Thông tin cá nhân (Ảnh đại diện, Họ tên, Mã nhân viên, Giới tính, Ngày sinh, Số điện thoại, Email cá nhân/công việc, Địa chỉ thường trú).  
   - Khối 2: Thông tin công việc (Vai trò hệ thống, Bộ phận, Ngày tuyển dụng, Trạng thái tài khoản).  
   - Khối 3: Thống kê hiệu suất tóm tắt (Tổng số đơn hàng đã lập đối với Sales Staff, Số lượt giao thành công đối với Shipper, Số phiếu bảo hành đã xử lý đối với Technical Staff).  
   - Khối 4: Lịch sử hoạt động gần đây (Activity Log cá nhân: 10 hành động gần nhất có ghi vết thời gian, loại hành động và IP đăng nhập).  
4. Chủ cửa hàng xem thông tin hoặc nhấp vào các nút tác vụ nhanh (Chỉnh sửa, Đổi mật khẩu, Khóa tài khoản) trên cùng thanh công cụ nếu cần.  

**Alternative Flows**  
- **Tài khoản nhân viên không tồn tại hoặc đã bị xóa:**  
  1. Nhân viên vừa bị xóa hoặc đường dẫn URL chi tiết không hợp lệ.  
  2. Hệ thống hiển thị thông báo lỗi (MSG45: "Không tìm thấy thông tin nhân viên hoặc tài khoản không tồn tại").  
  3. Hệ thống tự động chuyển hướng về danh sách nhân viên sau 3 giây.  

**Postconditions**  
Toàn bộ thông tin chi tiết và lịch sử liên quan của nhân viên được hiển thị trực quan cho Store Owner.  

---

### UC-014 Reset mật khẩu nhân viên (Reset Staff Password)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Email Service  

**Description**  
As a Store Owner, I want to directly reset a staff member's password when they forget it or upon security incidents so that they can regain access securely.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập thành công.  
2. Tài khoản nhân viên mục tiêu đang ở trạng thái `Active`.  

**Normal Flow**  
*Chủ cửa hàng gửi liên kết đặt lại mật khẩu cho nhân viên*  
1. Tại danh sách hoặc màn hình chi tiết nhân viên, Chủ cửa hàng nhấp vào nút "Đặt lại mật khẩu" (Reset Password).  
2. Hệ thống hiển thị hộp thoại với hai phương thức: gửi liên kết đặt lại mật khẩu qua Email công việc (mặc định) hoặc tạo mật khẩu tạm để bàn giao trực tiếp.  
3. Chủ cửa hàng chọn gửi liên kết và nhấp "Tiến hành đặt lại".  
4. Hệ thống thu hồi toàn bộ phiên làm việc hiện tại, sinh token đặt lại dùng một lần có hiệu lực **24 giờ**, rồi gửi liên kết tới Email công việc qua `Email Service`. Mật khẩu hiện tại chỉ bị thay đổi sau khi nhân viên mở liên kết và đặt mật khẩu mới hợp lệ.  
5. Hệ thống ghi Audit Log hành động của Store Owner và thông báo đã gửi liên kết thành công.  

**Alternative Flows**  
- **Step 2.1_Tạo mật khẩu tạm thời:** Hệ thống sinh mật khẩu ngẫu nhiên 10 ký tự, băm và lưu, đặt `IS_FIRST_LOGIN = true`, thu hồi mọi phiên cũ và chỉ hiển thị mật khẩu một lần để Store Owner bàn giao trực tiếp. Nhân viên phải đổi mật khẩu ở lần đăng nhập tiếp theo.  
- **Step 4.1_Gửi email thất bại:** Không báo thành công gửi liên kết; thông báo lỗi và cho phép Chủ cửa hàng thử lại.  
- **Step 1.1_Đặt lại mật khẩu cho tài khoản đang bị khóa:**  
  1. Tài khoản nhân viên đang ở trạng thái `Locked`.  
  2. Hệ thống cảnh báo (MSG47: "Tài khoản nhân viên này đang bị khóa. Bạn cần mở khóa tài khoản trước khi thực hiện đặt lại mật khẩu").  

**Postconditions**  
1. Với phương thức liên kết, chỉ token đặt lại được cấp và mật khẩu đổi khi nhân viên hoàn tất liên kết; với phương thức mật khẩu tạm, mật khẩu được đổi ngay.  
2. Mọi phiên đăng nhập cũ bị chấm dứt.  
3. `IS_FIRST_LOGIN = true` khi dùng mật khẩu tạm; liên kết đặt lại dùng một lần và hết hạn sau 24 giờ.  
4. Sự kiện được ghi lại trong Audit Log.  

---

### UC-015 Xem hồ sơ cá nhân (View My Profile)
**Primary Actors**  
Customer, Store Owner, Sales Staff, Technical Staff, Shipper, Accountant  

**Secondary Actors**  
None  

**Description**  
As an authenticated user, I want to view my profile details, assigned roles, and saved addresses so that I can verify my account information.  

**Preconditions**  
Người dùng đã đăng nhập thành công vào hệ thống.  

**Normal Flow**  
*Xem hồ sơ cá nhân của người dùng hiện tại*  
1. Người dùng nhấp vào avatar của mình trên thanh điều hướng và chọn mục "Thông tin tài khoản" (My Profile).  
2. Hệ thống xác thực danh tính từ phiên làm việc (Session/Token) và truy xuất dữ liệu từ cơ sở dữ liệu.  
3. Hệ thống hiển thị trang hồ sơ cá nhân với các thông tin tương ứng theo nhóm người dùng:  
   - **Đối với Customer:** Ảnh đại diện, Họ tên, Số điện thoại, Email, Ngày sinh, Giới tính, Danh sách địa chỉ nhận hàng đã lưu, Hạng thành viên (nếu có).  
   - **Đối với Staff/Store Owner:** Ảnh đại diện, Mã nhân viên, Họ tên, Số điện thoại, Email công việc, Tên vai trò/chức vụ được gán, Ngày bắt đầu làm việc, Trạng thái tài khoản.  
4. Người dùng xem thông tin chi tiết và có thể nhấp vào các nút điều hướng "Chỉnh sửa hồ sơ" hoặc "Đổi mật khẩu" để cập nhật khi cần.  

**Alternative Flows**  
- **Phiên đăng nhập hết hạn khi đang xem:**  
  1. Phiên làm việc hết hạn trước khi hệ thống kịp lấy dữ liệu.  
  2. Hệ thống hiển thị thông báo (MSG48: "Phiên làm việc đã hết hạn. Vui lòng đăng nhập lại") và điều hướng về trang Login.  

**Postconditions**  
Thông tin hồ sơ cá nhân của người dùng được hiển thị chính xác và bảo mật.  

---

### UC-157 Tạo tài khoản cho khách hàng (Create Customer Account by Staff)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
SMS Service, Email Service  

**Description**  
As a Store Owner or Sales Staff, I want to manually create an online user account for a customer at the sales counter so that the customer can access the website to track their orders, view invoices, and manage warranties.  

**Preconditions**  
1. Nhân viên bán hàng hoặc Chủ cửa hàng đã đăng nhập vào hệ thống quản trị.  
2. Khách hàng đến mua hàng tại quầy chưa có tài khoản đăng nhập website nhưng có nhu cầu tạo tài khoản.  

**Normal Flow**  
*Nhân viên tạo tài khoản khách hàng trực tiếp tại quầy*  
1. Nhân viên truy cập vào mục "Quản lý khách hàng" và nhấp vào nút "Tạo tài khoản đăng nhập" (Create Customer Account).  
2. Hệ thống hiển thị biểu mẫu tạo tài khoản khách hàng gồm: Họ và tên khách hàng, Số điện thoại, Địa chỉ Email (không bắt buộc nếu khách không có email), Địa chỉ nhận hàng, Mật khẩu khởi tạo (hoặc tùy chọn sinh tự động), và Tùy chọn "Gửi thông tin đăng nhập qua SMS/Email".  
3. Nhân viên nhập các thông tin do khách hàng cung cấp.  
4. Nhân viên nhấp nút "Tạo tài khoản".  
5. Hệ thống kiểm tra dữ liệu:  
   - Số điện thoại phải đúng 10 chữ số hợp lệ và chưa từng được đăng ký trong hệ thống.  
   - Email (nếu có nhập) phải đúng định dạng và chưa tồn tại trong hệ thống.  
   - Họ tên khách hàng không để trống.  
6. Hệ thống tạo tài khoản mới với vai trò `Customer`, trạng thái `Active`, và liên kết trực tiếp với Hồ sơ khách hàng (Customer Profile).  
7. Nếu nhân viên chọn gửi thông báo:  
   - Hệ thống gọi Secondary Actor `SMS Service` (nếu chỉ có SĐT) hoặc `Email Service` (nếu có Email) để gửi tin nhắn thông báo tên đăng nhập và mật khẩu tạm thời cho khách hàng.  
8. Hệ thống ghi nhận hành động tạo tài khoản khách hàng của nhân viên vào Activity/Audit Log.  
9. Hệ thống hiển thị thông báo thành công (MSG49: "Tạo tài khoản khách hàng thành công! Thông tin tài khoản đã được kích hoạt") và hiển thị mã khách hàng vừa tạo.  

**Alternative Flows**  
- **Step 5_Số điện thoại khách hàng đã tồn tại trên hệ thống:**  
  1. Hệ thống phát hiện số điện thoại đã thuộc về một tài khoản khác.  
  2. Hệ thống hiển thị thông báo lỗi (MSG50: "Số điện thoại này đã có tài khoản trên hệ thống. Bạn có muốn xem thông tin tài khoản hiện có không?").  
  3. Nhân viên có thể chuyển sang tra cứu tài khoản đó thay vì tạo mới.  
- **Step 7.1_Gửi tin nhắn SMS thất bại:**  
  1. Cổng SMS Gateway gặp trục trặc không gửi được tin.  
  2. Hệ thống hiển thị thông báo: "Tài khoản đã tạo thành công nhưng gửi SMS thất bại. Vui lòng ghi lại mật khẩu tạm: [Password] để gửi trực tiếp cho khách".  

**Postconditions**  
1. Tài khoản khách hàng mới được tạo và liên kết với hồ sơ mua hàng.  
2. Khách hàng có thể sử dụng số điện thoại và mật khẩu vừa cấp để đăng nhập website mua sắm.  

---

### UC-158 Khóa/Mở khóa tài khoản khách hàng (Lock/Unlock Customer Account)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Email Service  

**Description**  
As a Store Owner, I want to lock or unlock a customer account so that I can prevent fraudulent orders or spam while retaining the ability to restore legitimate accounts.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập vào hệ thống quản trị.  
2. Tài khoản khách hàng mục tiêu đang tồn tại trong hệ thống.  

**Normal Flow**  
*Khóa tài khoản khách hàng có hành vi gian lận/bom hàng*  
1. Chủ cửa hàng truy cập màn hình "Quản lý khách hàng", tìm đến khách hàng cần khóa.  
2. Chủ cửa hàng nhấp vào nút "Khóa tài khoản" trên dòng thông tin của khách hàng.  
3. Hệ thống hiển thị hộp thoại xác nhận khóa tài khoản: "Khóa tài khoản của khách hàng [Tên khách hàng] - [SĐT]?" kèm ô nhập "Lý do khóa tài khoản" (bắt buộc).  
4. Chủ cửa hàng chọn lý do (ví dụ: "Đặt đơn ảo nhiều lần", "Gian lận thanh toán", "Yêu cầu từ khách hàng") và nhấp nút "Xác nhận khóa".  
5. Hệ thống kiểm tra lý do không để trống.  
6. Hệ thống cập nhật trạng thái tài khoản khách hàng thành `Locked`.  
7. Hệ thống ngay lập tức thu hồi toàn bộ phiên đăng nhập hiện tại của khách hàng này.  
8. Hệ thống gọi Secondary Actor `Email Service` gửi email thông báo cho khách hàng về việc tài khoản bị khóa và hướng dẫn liên hệ bộ phận chăm sóc khách hàng nếu có nhầm lẫn.  
9. Hệ thống ghi nhận sự kiện vào Audit Log.  
10. Hệ thống hiển thị thông báo thành công (MSG51: "Đã khóa tài khoản khách hàng thành công!").  

**Alternative Flows**  
- **Mở khóa tài khoản khách hàng (Unlock Flow):**  
  1. Tại bước 2, đối với tài khoản đang có trạng thái `Locked`, Chủ cửa hàng nhấp vào nút "Mở khóa tài khoản".  
  2. Hệ thống hiển thị popup xác nhận mở khóa kèm ô nhập lý do.  
  3. Chủ cửa hàng nhập lý do giải quyết khiếu nại và nhấp "Xác nhận".  
  4. Hệ thống cập nhật trạng thái tài khoản thành `Active`.  
  5. Hệ thống ghi nhật ký vào Audit Log và gửi email thông báo mở khóa cho khách hàng.  
  6. Hệ thống hiển thị thông báo thành công (MSG52: "Tài khoản khách hàng đã được mở khóa hoạt động trở lại").  
- **Step 5_Không nhập lý do:**  
  1. Chủ cửa hàng bỏ trống ô lý do khóa tài khoản.  
  2. Hệ thống cảnh báo (MSG40: "Vui lòng nhập lý do để tiếp tục").  

**Postconditions**  
1. Trạng thái tài khoản khách hàng được cập nhật thành `Locked` hoặc `Active`.  
2. Khách hàng bị khóa sẽ không thể đăng nhập hoặc đặt thêm đơn hàng mới trên hệ thống.  

---

### UC-159 Tìm kiếm / lọc nhân viên (theo tên, vai trò, trạng thái) (Search/Filter Staff)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to search and filter the staff list by name, role, or status so that I can quickly locate specific employees for management tasks.  

**Preconditions**  
Chủ cửa hàng đã đăng nhập và đang ở trang "Danh sách nhân viên".  

**Normal Flow**  
*Tìm kiếm và lọc nhân viên linh hoạt theo nhiều tiêu chí*  
1. Chủ cửa hàng tương tác với thanh công cụ tìm kiếm và lọc trên đầu danh sách nhân viên:  
   - Nhập từ khóa tìm kiếm vào ô tìm kiếm: có thể là Tên nhân viên, Số điện thoại, Email hoặc Mã nhân viên.  
   - Chọn bộ lọc "Vai trò": chọn một vai trò cụ thể (`Sales Staff`, `Technical Staff`, `Shipper`, `Accountant`) hoặc `Tất cả vai trò`.  
   - Chọn bộ lọc "Trạng thái": chọn `Đang hoạt động (Active)` hoặc `Đã khóa (Locked)` hoặc `Tất cả`.  
2. Chủ cửa hàng nhấp nút "Tìm kiếm" (hoặc hệ thống tự động tìm kiếm tức thì sau 300ms dừng gõ phím - Debounce search).  
3. Hệ thống gửi truy vấn lọc xuống máy chủ và đối soát các điều kiện:  
   - So khớp chuỗi ký tự không phân biệt hoa thường và không dấu/có dấu trong các cột Tên, Mã NV, SĐT, Email.  
   - Lọc chính xác theo Vai trò và Trạng thái đã chọn.  
4. Hệ thống làm mới bảng dữ liệu với danh sách các nhân viên thỏa mãn tiêu chí lọc.  
5. Hệ thống hiển thị số lượng kết quả tìm thấy (ví dụ: "Tìm thấy 3 nhân viên phù hợp").  

**Alternative Flows**  
- **Không tìm thấy kết quả phù hợp:**  
  1. Không có nhân viên nào thỏa mãn tất cả các tiêu chí tìm kiếm/lọc.  
  2. Hệ thống hiển thị thông báo (MSG53: "Không tìm thấy nhân viên nào phù hợp với điều kiện tìm kiếm") kèm nút "Xóa bộ lọc".  
  3. Chủ cửa hàng nhấp nút "Xóa bộ lọc" -> Hệ thống reset toàn bộ ô tìm kiếm và dropdown về giá trị mặc định, hiển thị lại toàn bộ nhân viên.  

**Postconditions**  
Bảng dữ liệu hiển thị đúng tập hợp các tài khoản nhân viên thỏa mãn các tiêu chí lọc được chọn.  

---

### UC-160 Tạo vai trò mới (Create New Role)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to create a new user role (e.g., Cashier, Warehouse Keeper, Shift Manager) and define its permission set so that I can customize access control to match the store's organizational structure.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập với quyền quản trị cao nhất.  
2. Chủ cửa hàng đang ở màn hình "Quản lý vai trò & Phân quyền" (Roles & Permissions).  

**Normal Flow**  
*Tạo vai trò người dùng mới và phân tập quyền chi tiết*  
1. Chủ cửa hàng nhấp vào nút "Tạo vai trò mới" (Create New Role).  
2. Hệ thống hiển thị biểu mẫu tạo vai trò mới bao gồm:  
   - Tên vai trò (Role Name, ví dụ: "Thủ kho", "Thu ngân").  
   - Mã vai trò (Role Code, viết hoa không dấu, ví dụ: `WAREHOUSE_KEEPER`).  
   - Mô tả vai trò.  
   - Bảng phân quyền chi tiết (Permission Matrix) được gom nhóm theo từng phân hệ chức năng (Quản lý sản phẩm, Quản lý kho, Quản lý đơn hàng, Quản lý tài chính...), mỗi nhóm gồm các quyền: Xem (View), Thêm (Create), Sửa (Update), Xóa (Delete), Phê duyệt (Approve).  
3. Chủ cửa hàng nhập Tên, Mã và Mô tả của vai trò.  
4. Chủ cửa hàng tích chọn các quyền hạn cụ thể cần cấp cho vai trò này trong Bảng phân quyền.  
5. Chủ cửa hàng nhấp nút "Lưu vai trò".  
6. Hệ thống kiểm tra dữ liệu:  
   - Tên vai trò và Mã vai trò không được để trống.  
   - Mã vai trò phải là duy nhất, chưa từng tồn tại trên hệ thống.  
   - Ít nhất 1 quyền hạn phải được tích chọn.  
7. Hệ thống lưu bản ghi vai trò mới cùng danh sách các quyền liên kết (Role-Permission mappings) vào cơ sở dữ liệu.  
8. Hệ thống ghi nhận sự kiện tạo vai trò vào Audit Log.  
9. Hệ thống hiển thị thông báo thành công (MSG54: "Tạo vai trò mới thành công!") và đưa vai trò mới vào danh sách có thể gán cho nhân viên.  

**Alternative Flows**  
- **Step 6_Mã vai trò hoặc Tên vai trò đã tồn tại:**  
  1. Mã hoặc tên vai trò trùng lặp với vai trò đã có.  
  2. Hệ thống báo lỗi (MSG55: "Tên hoặc Mã vai trò này đã tồn tại trong hệ thống. Vui lòng nhập tên khác").  
  3. Chủ cửa hàng chỉnh sửa lại thông tin và lưu lại.  
- **Step 6.1_Chưa chọn bất kỳ quyền nào:**  
  1. Chủ cửa hàng không tích chọn quyền hạn nào trong ma trận quyền.  
  2. Hệ thống hiển thị thông báo cảnh báo (MSG56: "Vui lòng chọn ít nhất một quyền hạn cho vai trò mới").  

**Postconditions**  
1. Vai trò mới được khởi tạo với tập quyền tương ứng trong hệ thống.  
2. Vai trò mới sẵn sàng hiển thị trong danh mục lựa chọn khi phân quyền cho nhân viên.  

---

### UC-161 Xóa/vô hiệu hóa vai trò (Delete/Deactivate Role)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to delete or deactivate a custom role after verifying that no active users are assigned to it so that I can maintain a clean and secure access control structure.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập vào hệ thống quản trị.  
2. Chủ cửa hàng đang ở màn hình danh sách các vai trò (Roles List).  

**Normal Flow**  
*Xóa mềm vai trò tùy biến không còn sử dụng*  
1. Chủ cửa hàng tìm đến vai trò cần xóa trong danh sách và nhấp nút "Xóa vai trò" (Delete Role).  
2. Hệ thống kiểm tra ràng buộc nghiệp vụ:  
   - Kiểm tra xem đây có phải là vai trò hệ thống mặc định cốt lõi không (`Store Owner`, `Customer`, `Sales Staff`).  
   - Kiểm tra số lượng người dùng hiện tại đang được gán vai trò này.  
3. Nếu không có người dùng nào đang gán vai trò này và không phải vai trò hệ thống cốt lõi, hệ thống hiển thị hộp thoại xác nhận: "Bạn có chắc chắn muốn xóa vai trò [Tên vai trò]? Hành động này không thể hoàn tác!".  
4. Chủ cửa hàng nhấp "Xác nhận xóa".  
5. Hệ thống thực hiện xóa mềm (đánh dấu `IsDeleted = True` hoặc `IsActive = False`) vai trò trong cơ sở dữ liệu và hủy các liên kết quyền tương ứng.  
6. Hệ thống ghi nhận hành động xóa vai trò vào Audit Log.  
7. Hệ thống hiển thị thông báo thành công (MSG57: "Đã xóa vai trò thành công!") và loại bỏ vai trò khỏi danh sách hiển thị.  

**Alternative Flows**  
- **Step 2.1_Vai trò vẫn đang có người dùng hoạt động:**  
  1. Hệ thống phát hiện hiện có 1 hoặc nhiều tài khoản nhân viên đang giữ vai trò này.  
  2. Hệ thống từ chối xóa và hiển thị thông báo lỗi (MSG58: "Không thể xóa vai trò này vì đang có [X] nhân viên được gán vai trò. Vui lòng chuyển vai trò của các nhân viên này sang vai trò khác trước khi xóa").  
- **Step 2.2_Cố tình xóa vai trò hệ thống mặc định cốt lõi:**  
  1. Vai trò được chọn là vai trò cố định của hệ thống (`Store Owner`, `Sales Staff`, `Customer`, `Shipper`, `Technical Staff`, `Accountant`).  
  2. Hệ thống khóa nút xóa và hiển thị cảnh báo (MSG59: "Đây là vai trò mặc định của hệ thống và không được phép xóa").  

**Postconditions**  
1. Vai trò bị vô hiệu hóa hoặc xóa mềm khỏi hệ thống.  
2. Vai trò không còn xuất hiện trong danh sách lựa chọn phân quyền.  

---

### UC-162 Xem lịch sử hoạt động của nhân viên (View Staff Audit Log)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to view the detailed activity log of staff members (who did what, when, and on which entity) so that I can audit accountability and trace system actions.  

**Preconditions**  
Chủ cửa hàng đã đăng nhập vào hệ thống và có quyền truy cập vào Phân hệ Báo cáo & Kiểm toán.  

**Normal Flow**  
*Xem và kiểm tra nhật ký hoạt động của nhân viên*  
1. Chủ cửa hàng chọn menu "Nhật ký hoạt động" (Audit Log) trong mục Quản trị người dùng.  
2. Hệ thống hiển thị giao diện Nhật ký hoạt động với bảng dữ liệu chi tiết gồm các cột:  
   - Thời gian thực hiện (Timestamp: `YYYY-MM-DD HH:mm:ss`).  
   - Tên nhân viên và Mã nhân viên (Actor).  
   - Vai trò tại thời điểm thực hiện.  
   - Loại hành động (Action Type: Đăng nhập, Đổi giá, Tạo đơn hàng, Nhập kho, Cập nhật thông tin, Khóa tài khoản...).  
   - Đối tượng bị tác động (Target Entity & Entity ID: ví dụ Đơn hàng #DH1002, Sản phẩm #SP201).  
   - Địa chỉ IP và Thiết bị (User Agent).  
   - Cột Thao tác: "Xem chi tiết thay đổi".  
3. Chủ cửa hàng sử dụng các bộ lọc:  
   - Lọc theo nhân viên cụ thể.  
   - Lọc theo loại hành động.  
   - Lọc theo khoảng thời gian (Từ ngày - Đến ngày).  
4. Hệ thống tải lại bảng nhật ký theo các tiêu chí lọc được chọn.  
5. Chủ cửa hàng nhấp vào nút "Xem chi tiết" tại một dòng để xem chi tiết dữ liệu trước và sau khi thay đổi (Before/After diff data).  

**Alternative Flows**  
- **Không có bản ghi nhật ký trong khoảng thời gian đã lọc:**  
  1. Bộ lọc thời gian không có bất kỳ hành động nào được ghi nhận.  
  2. Hệ thống hiển thị bảng trống kèm thông báo (MSG60: "Không có nhật ký hoạt động nào trong khoảng thời gian này").  
- **Xuất nhật ký kiểm toán ra file:**  
  1. Chủ cửa hàng nhấp vào nút "Xuất file Excel" trên thanh công cụ.  
  2. Hệ thống xuất toàn bộ dữ liệu nhật ký đang lọc ra tệp `.xlsx` và tự động tải xuống máy tính của Store Owner.  

**Postconditions**  
Toàn bộ nhật ký thao tác được hiển thị minh bạch, không thể bị chỉnh sửa hay xóa bởi bất kỳ người dùng nào (kể cả Store Owner) nhằm đảm bảo tính toàn vẹn kiểm toán.  

---
