# TÀI LIỆU ĐẶC TẢ USE CASE HỆ THỐNG QUẢN LÝ CỬA HÀNG THIẾT BỊ VỆ SINH (AQS)
> **Đồ án Tốt nghiệp / Capstone Project**  
> **Chuẩn đặc tả**: RUP / Agile Use Case Specification Standard  
> **Mẫu đặc tả**: Primary & Secondary Actors, Description (User Story), Preconditions, Normal Flow, Alternative Flows (Branching & Exceptions với mã lỗi MSG), Postconditions.  
> **Tổng số Use Cases**: **256 / 256 Use Cases** đã được đặc tả hoàn chỉnh 100%, không mơ hồ.

---

## 1. DANH SÁCH TÁC NHÂN HỆ THỐNG (ACTORS)

| STT | Tác nhân (Actor) | Phân loại | Mô tả vai trò & Trách nhiệm trong hệ thống |
|:---:|:---|:---:|:---|
| 1 | **Store Owner** | Con người (Nội bộ) | Quản lý tài khoản và phân quyền, quản lý bảng giá, nhập hàng, quản lý nhà cung cấp, kiểm toán và theo dõi dashboard kinh doanh. |
| 2 | **Sales Staff** | Con người (Nội bộ) | Xử lý bán hàng tại quầy POS, kiểm tra tồn kho tức thì, tiếp nhận đơn online, lập báo giá công trình, quản lý ca bán và khách hàng. |
| 3 | **Technical Staff** | Con người (Nội bộ) | Khảo sát đường nước, thi công lắp đặt thiết bị, sửa chữa và tiếp nhận/thực hiện các dịch vụ bảo hành tại nhà hoặc cửa hàng. |
| 4 | **Shipper** | Con người (Nội bộ) | Vận chuyển hàng hóa từ kho đến chân công trình, đối soát hàng hóa, cập nhật kết quả giao, thu tiền COD và nộp tiền về cửa hàng. |
| 5 | **Accountant** | Con người (Nội bộ) | Quản lý thu chi sổ quỹ, theo dõi khoản phải thu/phải trả, quản lý công nợ khách hàng và NCC, lập đối chiếu công nợ và báo cáo tài chính. |
| 6 | **Guest** | Con người (Bên ngoài) | Khách vãng lai xem danh mục sản phẩm, thông số kỹ thuật, trò chuyện với AI tư vấn, tra cứu đơn hàng và đăng ký tài khoản. |
| 7 | **Customer** | Con người (Bên ngoài) | Khách hàng đã có tài khoản, đặt hàng trực tuyến, thanh toán VietQR, theo dõi tiến độ đơn, gửi yêu cầu bảo hành online và quản lý sổ địa chỉ. |
| 8 | **OpenAI** | Hệ thống bên ngoài | Trí tuệ nhân tạo cung cấp Chatbot tư vấn giải pháp thiết bị vệ sinh tự động (FE-04), OCR quét hóa đơn chứng từ (ACC-F19) và trợ lý Telegram (FE-12). |
| 9 | **Cloudinary** | Hệ thống bên ngoài | Dịch vụ lưu trữ đám mây cho toàn bộ hình ảnh sản phẩm, ảnh chụp hiện trạng công trình, chứng từ thanh toán và biên bản bàn giao. |
| 10 | **Email Service** | Hệ thống bên ngoài | Gửi thư xác thực OTP, thư kích hoạt tài khoản, thông báo trạng thái đơn hàng, phiếu bảo hành điện tử và báo giá PDF. |
| 11 | **SMS Service** | Hệ thống bên ngoài | Gửi tin nhắn SMS/ZNS chứa OTP, thông báo mật khẩu tạm thời, nhắc nợ khách hàng và thông báo giao hàng. |

---

## 2. DANH MỤC CÁC TẬP TIN ĐẶC TẢ CHI TIẾT (14 PHÂN HỆ)

Dưới đây là 14 tài liệu đặc tả độc lập tương ứng với 14 phân hệ chức năng của toàn bộ hệ thống AQS:

| STT | Phân hệ chức năng | Tập tin đặc tả Markdown | Phạm vi Use Case | Số lượng | Trạng thái |
|:---:|:---|:---|:---:|:---:|:---:|
| 1 | **FE-01** - Quản lý tài khoản và phân quyền | [`FE-01_Quan_Ly_Tai_Khoan_Va_Phan_Quyen.md`](./FE-01_Quan_Ly_Tai_Khoan_Va_Phan_Quyen.md) | UC-001 -> UC-015, UC-157 -> UC-162 | 21 | ✅ Hoàn thành |
| 2 | **FE-02** - Truy vết lịch sử thao tác (Audit Log) | [`FE-02_Truy_Vet_Lich_Su_Thao_Tac.md`](./FE-02_Truy_Vet_Lich_Su_Thao_Tac.md) | UC-016 -> UC-018 | 3 | ✅ Hoàn thành |
| 3 | **FE-03** - Quản lý sản phẩm & biến thể | [`FE-03_Quan_Ly_San_Pham.md`](./FE-03_Quan_Ly_San_Pham.md) | UC-019 -> UC-030, UC-132, UC-143, UC-163 -> UC-177 | 30 | ✅ Hoàn thành |
| 4 | **FE-04** - AI tư vấn lựa chọn sản phẩm | [`FE-04_AI_Tu_Van_Lua_Chon_San_Pham.md`](./FE-04_AI_Tu_Van_Lua_Chon_San_Pham.md) | UC-031 -> UC-037, UC-134 | 8 | ✅ Hoàn thành |
| 5 | **FE-05** - Mua hàng & theo dõi đơn trực tuyến | [`FE-05_Mua_Hang_Va_Theo_Doi_Don_Truc_Tuyen.md`](./FE-05_Mua_Hang_Va_Theo_Doi_Don_Truc_Tuyen.md) | UC-038 -> UC-045 | 8 | ✅ Hoàn thành |
| 6 | **FE-06** - Quản lý đơn hàng, báo giá & ca POS | [`FE-06_Quan_Ly_Don_Hang_Bao_Gia_Va_Thanh_Toan.md`](./FE-06_Quan_Ly_Don_Hang_Bao_Gia_Va_Thanh_Toan.md) | UC-046 -> UC-073, UC-144 -> UC-152, UC-253 -> UC-256 | 41 | ✅ Hoàn thành |
| 7 | **FE-07** - Quản lý tồn kho, nhập hàng & NCC | [`FE-07_Quan_Ly_Ton_Kho_Nhap_Hang_Va_NCC.md`](./FE-07_Quan_Ly_Ton_Kho_Nhap_Hang_Va_NCC.md) | UC-074 -> UC-088, UC-138 -> UC-140, UC-178 -> UC-194 | 35 | ✅ Hoàn thành |
| 8 | **FE-08** - Xuất kho & giao hàng theo đợt | [`FE-08_Xuat_Kho_Va_Giao_Hang_Theo_Dot.md`](./FE-08_Xuat_Kho_Va_Giao_Hang_Theo_Dot.md) | UC-089 -> UC-098, UC-135, UC-201, UC-249 -> UC-252 | 16 | ✅ Hoàn thành |
| 9 | **FE-09** - Quản lý bảo hành thiết bị | [`FE-09_Quan_Ly_Bao_Hanh.md`](./FE-09_Quan_Ly_Bao_Hanh.md) | UC-099 -> UC-106, UC-136 -> UC-137, UC-154 -> UC-156, UC-195 -> UC-200 | 19 | ✅ Hoàn thành |
| 10 | **FE-10** - Quản lý khách hàng & lịch sử mua | [`FE-10_Quan_Ly_Khach_Hang_Va_Lich_Su_Mua_Hang.md`](./FE-10_Quan_Ly_Khach_Hang_Va_Lich_Su_Mua_Hang.md) | UC-107 -> UC-113, UC-133, UC-141 -> UC-142 | 10 | ✅ Hoàn thành |
| 11 | **FE-11** - Báo cáo & tổng quan kinh doanh | [`FE-11_Bao_Cao_Va_Tong_Quan_Kinh_Doanh.md`](./FE-11_Bao_Cao_Va_Tong_Quan_Kinh_Doanh.md) | UC-114 -> UC-126, UC-153 | 14 | ✅ Hoàn thành |
| 12 | **FE-12** - Trợ lý AI qua Telegram | [`FE-12_Tro_Ly_AI_Qua_Telegram.md`](./FE-12_Tro_Ly_AI_Qua_Telegram.md) | UC-127 -> UC-131 | 5 | ✅ Hoàn thành |
| 13 | **FE-13** - Quản lý công việc lắp đặt & sửa chữa | [`FE-13_Quan_Ly_Cong_Viec_Lap_Dat_Va_Sua_Chua.md`](./FE-13_Quan_Ly_Cong_Viec_Lap_Dat_Va_Sua_Chua.md) | UC-241 -> UC-248 | 8 | ✅ Hoàn thành |
| 14 | **ACC** - Kế toán Thu - Chi, Công nợ & Sổ quỹ | [`ACC_Quan_Ly_Thu_Chi_Va_Cong_No.md`](./ACC_Quan_Ly_Thu_Chi_Va_Cong_No.md) | UC-202 -> UC-240 | 39 | ✅ Hoàn thành |
| **TỔNG** | **Toàn bộ hệ thống Capstone AQS** | **14 Phân hệ chi tiết** | **UC-001 -> UC-256** | **256** | **ĐẠT 100%** |
