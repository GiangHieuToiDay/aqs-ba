# Ghi chú chỉnh sửa đặc tả FE-01

**Ngày:** 08/10/2026  
**Nhánh:** `docs/clarify-analysis`  
**Phạm vi:** Đặc tả tổng quan FE-01, 14 tài liệu màn hình trong `docs/Fe-01` và bảng thống kê `usecase-des/README.md`.

Tài liệu này ghi lại quyết định dùng để đồng bộ đặc tả. Khi thay đổi một quyết định nghiệp vụ, nhóm cần cập nhật cả use case tổng quan, màn hình liên quan và ghi chú này.

| Nội dung đã chỉnh | Lý do | Tài liệu chính |
| --- | --- | --- |
| Chọn đăng ký Customer bằng **Số điện thoại + OTP SMS**, Email bổ sung sau; form chỉ nhập mật khẩu một lần. Không đưa Google OAuth vào luồng FE-01 hiện tại. | UC-001 cũ yêu cầu Email, OTP Email, Google OAuth và xác nhận mật khẩu; màn đăng ký chi tiết lại dùng Số điện thoại, OTP SMS. Hai luồng không thể cùng là luồng chuẩn. | [UC-001](../../usecase-des/FE-01_Quan_Ly_Tai_Khoan_Va_Phan_Quyen.md), [Register](Common/Account/Register.md) |
| Chọn đăng nhập Customer bằng **Số điện thoại + mật khẩu + OTP SMS**; nhân viên bằng **Email công việc + mật khẩu + OTP Email**. Chỉ cấp phiên sau khi xác thực OTP. | UC-002 cũ cấp phiên ngay sau mật khẩu và có Google Login, trái với hai màn đăng nhập chi tiết. | [UC-002](../../usecase-des/FE-01_Quan_Ly_Tai_Khoan_Va_Phan_Quyen.md), [Login](Common/Account/Login.md), [StaffLogin](Admin/Account/StaffLogin.md) |
| Đổi UC-007 thành khôi phục mật khẩu **Customer qua Số điện thoại + OTP SMS**. Phản hồi gửi mã dùng cùng một thông báo khi số không tồn tại hoặc tài khoản không đủ điều kiện. Nhân viên liên hệ Chủ cửa hàng để được đặt lại mật khẩu qua UC-014. | UC-007 cũ dùng Email và liên kết; màn Customer dùng SMS. Thông báo riêng cho số không tồn tại/khóa làm lộ tình trạng tài khoản. Màn Staff từng gợi một luồng tự khôi phục chưa có đặc tả. | [UC-007/014](../../usecase-des/FE-01_Quan_Ly_Tai_Khoan_Va_Phan_Quyen.md), [ForgotPassword](Common/Account/ForgotPassword.md), [StaffLogin](Admin/Account/StaffLogin.md) |
| UC-014 dùng hai phương thức đã có trên màn chi tiết nhân viên: **liên kết đặt lại qua Email** (mặc định, token một lần 24 giờ) hoặc **mật khẩu tạm bàn giao trực tiếp** (chỉ hiện một lần, buộc đổi ở lần đăng nhập kế tiếp). | UC-014 cũ cho nhập mật khẩu chỉ định, gửi mật khẩu mới qua Email và luôn hiện mật khẩu trên màn hình; khác với màn chi tiết. | [UC-014](../../usecase-des/FE-01_Quan_Ly_Tai_Khoan_Va_Phan_Quyen.md), [StaffDetail](Admin/Staff/StaffDetail.md) |
| Thay mã `UC-01.xx` trên 14 file bằng mã UC nguồn. Màn chứa nhiều nghiệp vụ ghi nhiều mã; đổi mật khẩu lần đầu được ghi là luồng bổ trợ của UC-002/008, không tạo mã UC mới. | Mã cũ trùng số nhưng khác nghĩa với danh mục 256 UC; ví dụ `UC-01.07` trên Logout trong khi UC-007 là quên mật khẩu. | [Danh mục FE-01](../../usecase-des/FE-01_Quan_Ly_Tai_Khoan_Va_Phan_Quyen.md), các file trong thư mục này |
| Chỉ `Store Owner` được khóa/mở khóa hoặc đổi trạng thái tài khoản khách hàng. `Sales Staff` và `Accountant` được tìm kiếm, xem hồ sơ theo UC-109/110 nhưng không đổi trạng thái; `Shipper` chỉ nhận thông tin giao hàng trong đơn được phân công. Chỉ `Store Owner` quản trị danh sách nhân viên. | UC-158 chỉ cấp quyền khóa/mở khóa cho Store Owner; UC-109/110 cấp quyền tra cứu, xem hồ sơ cho Sales Staff và Accountant. “Quản lý bán hàng” và “Quản lý chi nhánh” chưa được định nghĩa là tác nhân trong danh mục. | [CustomerList](Admin/Customer/CustomerList.md), [CustomerDetail](Admin/Customer/CustomerDetail.md), [StaffList](Admin/Staff/StaffList.md) |
| Chốt thời gian OTP **180 giây**, cooldown **60 giây**, tối đa **3 lần gửi/10 phút tính cả lần đầu** theo định danh và mục đích; sai OTP lần thứ 5 thì hủy mã. Khóa tạm đăng nhập/IP **15 phút**; Access Token **15 phút**; token khôi phục Customer **15 phút**. | Các khoảng “3–5”, “15–30”, “15–60” và cách tính lượt gửi không đủ chính xác để triển khai, hiển thị đồng hồ hoặc kiểm thử. | [Register](Common/Account/Register.md), [Login](Common/Account/Login.md), [ForgotPassword](Common/Account/ForgotPassword.md), [StaffLogin](Admin/Account/StaffLogin.md) |
| Làm rõ tài khoản chưa có địa chỉ thì không có địa chỉ mặc định; khi có ít nhất một địa chỉ thì có đúng một địa chỉ mặc định. | Quy tắc “luôn luôn có đúng một” không đúng với tài khoản mới. | [Address](Common/Profile/Address.md) |
| So mật khẩu mới với hash hiện tại bằng `verify`/`compare` **trước khi băm để lưu**. | So trực tiếp hai chuỗi hash không xác định được mật khẩu có bị dùng lại khi thuật toán dùng salt. | [ChangePassword](Common/Profile/ChangePassword.md) |
| Khi mật khẩu tạm của nhân viên hết hạn sau **24 giờ**, tài khoản giữ `Pending`; `Store Owner` gửi lại email kích hoạt, mật khẩu tạm cũ mất hiệu lực. | Luồng tạo nhân viên trước đây nêu hạn 24 giờ nhưng chưa có đường xử lý sau khi hết hạn. | [StaffCreate](Admin/Staff/StaffCreate.md), [StaffList](Admin/Staff/StaffList.md), [StaffLogin](Admin/Account/StaffLogin.md) |
| Đổi các liên kết `file:///E:/...` sang đường dẫn tương đối; sửa đường dẫn text trỏ sai thư mục. Sửa số UC của FE-03 từ **30** thành **29** và bỏ tuyên bố “không mơ hồ” trong bảng tổng quan. | Đường dẫn máy cá nhân không dùng được trong repo; số thống kê và mức độ hoàn thiện phải phản ánh nội dung thực tế. | [Bảng tổng quan](../../usecase-des/README.md), các file trong thư mục này |

## Quy ước mã UC của các màn FE-01

| Màn | UC nguồn |
| --- | --- |
| Register / Login / Logout / ForgotPassword | UC-001 / UC-002 / UC-003 / UC-007 |
| Profile / Address / ChangePassword | UC-015 (xem) và UC-004 (sửa) / UC-005 / UC-006 |
| StaffLogin / FirstTimeChangePassword / StaffCreate | UC-002 / luồng bổ trợ UC-002, UC-008 / UC-008 |
| StaffList / StaffDetail | UC-012, UC-159 / UC-013, UC-011, UC-009, UC-010, UC-014, UC-162 |
| CustomerList / CustomerDetail | UC-109, UC-158 / UC-110, UC-158 |

## Kiểm tra sau chỉnh sửa

- Danh mục use case vẫn có **256 mã khác nhau từ UC-001 đến UC-256**, không thiếu hoặc trùng mã.
- File FE-03 thực tế có **29** tiêu đề UC, khớp số liệu mới trong bảng tổng quan.
- Cả **23 liên kết Markdown** trong 14 file màn FE-01 đều trỏ đến file tồn tại trong repo.
- Các giá trị thời gian dạng khoảng đã được loại khỏi 14 file màn FE-01; những ngưỡng đã chốt ở bảng trên dùng để kiểm thử.

Đây là chỉnh sửa tài liệu trên nhánh `docs/clarify-analysis`; chưa thay đổi mã nguồn ứng dụng và chưa gộp vào `main`.
