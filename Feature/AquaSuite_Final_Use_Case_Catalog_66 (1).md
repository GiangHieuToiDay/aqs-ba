# AQUASUITE — FINAL CONSOLIDATED USE CASE CATALOG

**Phiên bản:** UC Catalog v1.0 — 12/10/2026
**Trạng thái:** Danh sách 66 UC theo phạm vi người dùng đã chọn; các vấn đề chưa thống nhất được đánh dấu review.
**Nguồn đối chiếu:** Report 1 Project Introduction; Report 2 Project Management Plan; Report 3 SRS (08/10/2026); danh sách UC-001…UC-157 do người dùng cung cấp.

## 1. Thống kê

| Feature | Module | Số UC |
|---|---|---:|
| FE-01 | Tài khoản và phân quyền | 6 |
| FE-02 | Nhật ký hệ thống | 1 |
| FE-03 | Sản phẩm | 6 |
| FE-04 | Tư vấn AI | 3 |
| FE-05 | Mua sắm trực tuyến | 4 |
| FE-06 | Bán hàng và báo giá | 9 |
| FE-07 | Kho và mua hàng | 7 |
| FE-08 | Xuất kho và giao hàng | 5 |
| FE-09 | Bảo hành | 5 |
| FE-10 | Khách hàng | 3 |
| FE-11 | Báo cáo | 4 |
| FE-12 | AI Telegram | 1 |
| FE-13 | Kỹ thuật | 3 |
| FE-14 | Kế toán | 9 |
| **Tổng** | **14 module** | **66** |

**Lưu ý:** Các ID UC-25, UC-35, UC-52, UC-57 được chủ động để trống.

## 2. Danh sách Use Case cuối cùng

Cột “UC gốc” là ID trong danh sách 157 UC do người dùng gửi, không phải ID của Report 3.

### FE-01 — Tài khoản và phân quyền (6 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-01 | Đăng ký tài khoản khách hàng | UC-001 |
| UC-02 | Đăng nhập và xác thực người dùng | UC-002 |
| UC-03 | Quản lý hồ sơ cá nhân và địa chỉ | UC-003, UC-004 |
| UC-04 | Quản lý mật khẩu và khôi phục tài khoản | UC-005, UC-006, UC-010 |
| UC-05 | Quản lý tài khoản và trạng thái người dùng | UC-007, UC-008 |
| UC-06 | Quản lý vai trò và phân quyền | UC-009 |

### FE-02 — Nhật ký hệ thống (1 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-07 | Tra cứu và xem chi tiết nhật ký hoạt động | UC-012, UC-013 |

### FE-03 — Sản phẩm (6 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-08 | Quản lý danh mục và thương hiệu | UC-014, UC-021 |
| UC-09 | Quản lý sản phẩm | UC-015, UC-017 |
| UC-10 | Quản lý SKU, biến thể và giá bán | UC-016, UC-018, UC-019, UC-020 |
| UC-11 | Nhập và xuất dữ liệu sản phẩm | UC-023 |
| UC-12 | Tìm kiếm và xem thông tin sản phẩm | UC-024, UC-025 |
| UC-13 | Xem và quản lý gói sản phẩm | UC-027 |

### FE-04 — Tư vấn AI (3 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-14 | Tư vấn và gợi ý sản phẩm bằng AI | UC-028, UC-029 |
| UC-15 | Quản lý yêu cầu và lịch sử tư vấn | UC-030, UC-032 |
| UC-16 | Chuyển tư vấn thành báo giá hoặc đơn hàng | UC-033 |

### FE-05 — Mua sắm trực tuyến (4 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-17 | Quản lý giỏ hàng | UC-034 |
| UC-18 | Đặt hàng và thanh toán trực tuyến | UC-035, UC-036 |
| UC-19 | Theo dõi đơn hàng của khách hàng | UC-037 |
| UC-20 | Yêu cầu thay đổi hoặc hủy đơn hàng | UC-038, UC-039 |

### FE-06 — Bán hàng và báo giá (9 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-21 | Tạo và quản lý đơn hàng bán | UC-040, UC-041, UC-043, UC-044, UC-045, UC-052 |
| UC-22 | Xác nhận và xử lý đơn hàng online | UC-042 |
| UC-23 | Quản lý thanh toán và xác minh thanh toán | UC-046, UC-047, UC-059 |
| UC-24 | Quản lý công nợ khách hàng | UC-048 |
| UC-26 | Xử lý yêu cầu thay đổi hoặc hủy đơn | UC-051 |
| UC-27 | Quản lý báo giá dự án | UC-054, UC-055, UC-060 |
| UC-28 | Chuyển báo giá thành đơn hàng | UC-056 |
| UC-29 | Quản lý chiết khấu, đặt cọc và giữ hàng | UC-057, UC-058 |
| UC-30 | Quản lý thu hộ COD và bàn giao tiền | UC-061 |

### FE-07 — Kho và mua hàng (7 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-31 | Tra cứu và quản lý thông tin tồn kho | UC-062, UC-071, UC-072 |
| UC-32 | Quản lý nhập kho | UC-063, UC-064, UC-065 |
| UC-33 | Quản lý nhà cung cấp | UC-066 |
| UC-34 | Quản lý đơn nhập hàng | UC-067, UC-075 |
| UC-36 | Quản lý giữ hàng | UC-073, UC-076 |
| UC-37 | Xử lý hàng trả về và trả nhà cung cấp | UC-077, UC-078 |
| UC-38 | Nhập/xuất dữ liệu tồn kho | UC-079 |

### FE-08 — Xuất kho và giao hàng (5 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-39 | Quản lý phiếu xuất kho | UC-080, UC-081, UC-082 |
| UC-40 | Lập và quản lý đợt giao hàng | UC-083, UC-084, UC-087 |
| UC-41 | Thực hiện và cập nhật kết quả giao hàng | UC-085, UC-086, UC-091 |
| UC-42 | Theo dõi lịch sử và tiến độ giao hàng | UC-088 |
| UC-43 | Tra cứu chuyến giao được phân công | UC-089, UC-090, UC-092 |

### FE-09 — Bảo hành (5 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-44 | Quản lý chính sách bảo hành | UC-093, UC-094 |
| UC-45 | Quản lý hồ sơ bảo hành | UC-095, UC-096, UC-103 |
| UC-46 | Tiếp nhận và xử lý yêu cầu bảo hành | UC-097, UC-098, UC-101, UC-102 |
| UC-47 | Gửi yêu cầu bảo hành trực tuyến | UC-099 |
| UC-48 | Tra cứu bảo hành của khách hàng | UC-100 |

### FE-10 — Khách hàng (3 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-49 | Quản lý hồ sơ khách hàng và liên kết tài khoản | UC-104, UC-105 |
| UC-50 | Tra cứu lịch sử khách hàng và mua hàng | UC-106, UC-108 |
| UC-51 | Quản lý công trình/dự án khách hàng | UC-107 |

### FE-11 — Báo cáo (4 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-53 | Xem dashboard và cảnh báo vận hành | UC-111, UC-120 |
| UC-54 | Xem báo cáo kinh doanh và doanh số | UC-112, UC-113, UC-114, UC-115, UC-116, UC-122 |
| UC-55 | Xem báo cáo tồn kho và công nợ | UC-117, UC-118 |
| UC-56 | Quản lý chi phí vận hành và xuất báo cáo | UC-119, UC-121 |

### FE-12 — AI Telegram (1 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-58 | Tra cứu dữ liệu kinh doanh qua Telegram | UC-124, UC-125, UC-126 |

### FE-13 — Kỹ thuật (3 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-59 | Tra cứu công việc và lịch kỹ thuật | UC-127, UC-128, UC-129 |
| UC-60 | Tiếp nhận và thực hiện công việc kỹ thuật | UC-130, UC-133, UC-134 |
| UC-61 | Ghi nhận kết quả và hoàn thành công việc | UC-131, UC-132 |

### FE-14 — Kế toán (9 UC)

| UC mới | Tên Use Case | UC gốc |
|---|---|---|
| UC-62 | Tra cứu và quản lý giao dịch tài chính | UC-135, UC-142, UC-143 |
| UC-63 | Quản lý phiếu thu và phân bổ thanh toán | UC-136, UC-137, UC-138, UC-149 |
| UC-64 | Quản lý phiếu chi và chứng từ | UC-139, UC-140, UC-141 |
| UC-65 | Quản lý và đối soát công nợ phải thu | UC-144, UC-145, UC-150 |
| UC-66 | Quản lý và đối soát công nợ phải trả | UC-146, UC-147, UC-148 |
| UC-67 | Xem báo cáo thu chi và dòng tiền | UC-151, UC-152 |
| UC-68 | Xem báo cáo doanh thu và công nợ | UC-153, UC-154, UC-155 |
| UC-69 | Xem dashboard kế toán | UC-156 |
| UC-70 | Xử lý các khoản thanh toán chờ xác minh | UC-157 |

## 3. Kết quả review và điểm cần xác nhận

| Chủ đề | Đánh giá / hành động |
|---|---|
| Mã Use Case | Giữ nguyên mã UC-01…UC-70 với các khoảng trống UC-25, UC-35, UC-52, UC-57. Không tự đánh số lại vì có thể ảnh hưởng tài liệu và test case. |
| Đối chiếu SRS | Report 3 bản 08/10 có hệ đánh số UC khác danh sách đã tinh gọn. Ví dụ UC-003 trong SRS là Log Out, không phải Quản lý thông tin cá nhân. Cột UC gốc trong file này tham chiếu danh sách UC-001…UC-157 người dùng đã gửi, KHÔNG phải mã trực tiếp của Report 3. |
| UC-25 bị loại | Xử lý trả/đổi hàng và hoàn tiền không còn trong FE-06 nhưng FE-07 vẫn có xử lý hàng trả về tại kho. Cần xác nhận trả hàng của khách có nằm trong scope hay chỉ trả nhà cung cấp. |
| UC-35 bị loại | Kiểm kê và điều chỉnh tồn kho không còn UC độc lập. Report 1 nêu nhu cầu truy vết điều chỉnh kho; cần làm rõ có thực hiện kiểm kê trong phiên bản này không. |
| UC-52 bị loại | Quản lý lịch sử tương tác và nhóm khách hàng bị bỏ. Không đồng nghĩa bỏ dữ liệu khách hàng, công trình và lịch sử đơn. |
| UC-57 bị loại | Liên kết/hủy liên kết Telegram bị bỏ nhưng UC-58 vẫn tra cứu dữ liệu riêng qua Telegram. Cần mô tả phương thức xác thực và cấp quyền trước khi triển khai. |
| FE-06/FE-14 | UC-23/24 và UC-63/65 có giao thoa thanh toán, công nợ. Phân định: bán hàng ghi nhận trạng thái đơn, kế toán xác minh/phân bổ và đối soát, tránh hai nguồn sự thật. |
| FE-07/FE-08 | UC-36 giữ hàng và UC-39 xuất kho phải định nghĩa rõ thời điểm reserve/release/deduct, không trừ kho hai lần. |
| FE-09/FE-13 | UC-46 tiếp nhận/xử lý bảo hành và UC-60/61 thực hiện công việc kỹ thuật: liên kết yêu cầu bảo hành với phiếu công việc; tránh ghi trùng kết quả. |
| Giới hạn scope | Theo Report 2: không kế toán tổng hợp nâng cao, không quản lý tiến độ thi công, không sửa dữ liệu bằng AI Telegram. Các UC kế toán và dự án chỉ nên triển khai mức vận hành cửa hàng. |
| FE-03 combo | UC-13 còn trong danh sách; nếu triển khai combo như SKU bán được, cần xác nhận mô hình bundle và quan hệ với inventory. Không mặc nhiên coi là chỉ gợi ý trên giao diện. |
| Bảo mật và audit | Ghi audit log, kiểm tra quyền Telegram, từ chối lệnh sửa dữ liệu và trừ tồn kho là system behavior/business rule, không nhất thiết có UC độc lập. |
| Đăng xuất | Report 3 có Log Out; danh sách tinh gọn không có UC riêng. Nên đưa thành luồng trong UC-02 hoặc yêu cầu bảo mật phiên đăng nhập. |
| Cấu hình cửa hàng | UC gốc 053 đã bỏ; cần xác định cấu hình cửa hàng/thanh toán ở mức cố định hay thuộc phần quản trị kỹ thuật. |

## 4. Nguyên tắc viết Use Case Specification

- Mỗi UC mô tả mục tiêu Actor, precondition, main flow, alternative/exception flow, postcondition và business rules.
- Không tách CRUD, nút bấm, API hoặc bước hệ thống thành UC nếu không tạo mục tiêu độc lập.
- Không xóa business rule quan trọng chỉ vì UC thao tác đã được gộp hoặc loại.
- Trước khi khóa SRS, BA phải thống nhất ID mapping và quyết định các điểm review mục 3.

## 5. Truy vết nguồn

- Report 1: phạm vi quản lý sản phẩm/SKU, bán hàng, kho, bảo hành, báo giá và giao nhiều đợt.
- Report 2: scope/constraints; không kế toán nâng cao, không quản lý tiến độ thi công, AI Telegram chỉ đọc dữ liệu.
- Report 3: tham chiếu các luồng Payment Processing, Import Products, Goods Issue, Sales Order, Warranty; lưu ý bảng UC trong Report 3 đánh số khác catalog đã tinh gọn.
- Danh sách 157 UC: là nguồn trực tiếp cho tên và cột mapping “UC gốc” trong catalog này.
