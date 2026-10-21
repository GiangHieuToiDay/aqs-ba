# AquaSuite – Từ điển dữ liệu đề xuất (một kho)

**Trạng thái:** Bản thiết kế đề xuất để nhóm review, chưa phải SQL DDL. **Nguồn:** schema 47 bảng do nhóm cung cấp và các luồng nghiệp vụ đơn hàng, thu tiền, giao nhiều đợt đã trao đổi. Các trường bổ sung là **đề xuất thiết kế**, không phải yêu cầu đã được chủ cửa hàng xác nhận.

## 1. Quyết định thiết kế

- Cửa hàng có đúng **một kho** trong phạm vi phiên bản này. Không tạo bảng `warehouses`. `inventories` có đúng một dòng cho mỗi `variant_id`; tên/địa chỉ kho nếu cần hiển thị được cấu hình trong thông tin cửa hàng, không lặp theo SKU. Nếu sau này mở kho thứ hai phải nâng cấp khóa, các phiếu và lịch sử kho.
- Phiếu nhập, xuất, kiểm kê chứa nhiều SKU: **bỏ `inventory_id` ở đầu phiếu**. Chi tiết phiếu dùng `variant_id`; `stock_movements` tham chiếu `variant_id` và chứng từ phát sinh.
- Số lượng kho có hai khái niệm: `on_hand_qty` = đang hiện hữu trong kho; `reserved_qty` = phần trong đó đã giữ cho đơn; **khả dụng = on_hand_qty − reserved_qty**. `stock_reservations` giải thích phần giữ của từng dòng đơn.
- `payments` là nguồn khoản tiền khách trả. `transactions` phản ánh thu/chi quản trị, khoản thu từ đơn phải liên kết khoản `payment` và không được tạo trùng. `cod_handovers` là việc nội bộ bàn giao tiền đã thu.
- `order_status`, `payment_status` và `delivery_status` là các chiều độc lập. Giá trị tổng hợp có thể lưu để tra cứu, nhưng cập nhật từ các chi tiết đã xác nhận, không sửa thủ công tách rời.
- Mọi FK, unique, kiểu dữ liệu, công thức và quyền nêu dưới đây là **thiết kế mục tiêu**; phạm vi về combo, serial, hợp đồng, điều kiện trả cuối có điểm **[CẦN XÁC NHẬN]** ở cuối tài liệu.

**Quy ước:** `PK` khóa chính; `FK` khóa ngoại; `UQ` duy nhất; `?` có thể để trống; `snapshot` là dữ liệu đóng băng để chứng từ cũ không thay đổi theo danh mục. Mã (`*_code`) cần UQ; tiền dùng DECIMAL/NUMERIC; thời điểm dùng kiểu có quy ước múi giờ thống nhất. Mọi bảng liên quan người thao tác dùng FK `users.id`.

## 2. Danh mục thay đổi từ bản gốc

| Thay đổi | Lý do |
|---|---|
| Bỏ `users.Role_id`, `users.Permission_id`; đổi `permission` thành `permissions`, `roles_permission` thành `role_permissions` | Một nguồn phân quyền qua các bảng nối; sửa lỗi tên `permision_id`. |
| Bỏ `products.supplier_id`, `suppliers.product_id`; đổi `suppliers_product` thành `supplier_products` | Một quan hệ sản phẩm–nhà cung cấp qua bảng nối. |
| Bỏ `warehouse_name`, `address`, `status` khỏi `inventories`; bỏ `inventory_id` khỏi đầu phiếu và `deliveries` | Một kho, phiếu nhiều SKU; mỗi SKU một tồn. |
| Thêm `address_id` cho `customer_addresses` | Có khóa chính và nhiều địa chỉ/khách. |
| Thêm `stock_reservations` | Truy vết lượng giữ theo dòng đơn. |
| Thêm `order_payment_schedules`, `payment_allocations`; thêm điều khoản thanh toán/hợp đồng ở `orders` | Một đơn có thể trả nhiều đợt, một khoản thu có thể phân bổ nhiều mốc. |
| Thêm `product_serials`, `dispatch_item_serials`; loại mảng serial khỏi dòng xuất/giao | Định danh từng thiết bị serial; tránh trùng xuất hoặc bảo hành. |
| Bổ sung số lượng dự kiến/thực nhận ở `delivery_items` | Tính đúng đã giao/còn giao. |
| Sửa tên cột về `snake_case` | Tránh `Role_id`, `Create_at`, `Updated_at`, `Address` cùng tồn tại. |

## 3. Tài khoản, khách hàng và lịch sử

### `permissions` — Danh mục quyền thao tác

| Trường | Ý nghĩa |
|---|---|
| `permission_id` (PK) | Định danh một quyền; thay cho `permission.id`. |
| `code` (UQ) | Mã quyền dùng để kiểm tra truy cập, ví dụ `ORDER_APPROVE`. |
| `description` | Mô tả hành động mà quyền cho phép. |

### `users` — Tài khoản đăng nhập của nhân viên/chủ cửa hàng hoặc khách có tài khoản

| Trường | Ý nghĩa |
|---|---|
| `id` (PK) | Định danh tài khoản, giữ để tương thích bản Java hiện có. |
| `username` | Tên hiển thị. |
| `email` (UQ, ?) | Email đăng nhập/liên hệ; tùy cách đăng nhập. |
| `password` (?) | Mật khẩu dạng hash, có thể trống nếu đăng nhập bằng provider bên ngoài. |
| `avatar` (?) | Đường dẫn ảnh đại diện. |
| `created_at` | Thời điểm tạo tài khoản. |
| `provider` | Nguồn xác thực theo `AuthProvider`. |
| `provider_id` (?) | Mã tài khoản do nguồn xác thực ngoài cung cấp. |
| `status` | Trạng thái cho phép sử dụng tài khoản. |
| `verification_code` (?) | Mã xác thực tạm thời; ưu tiên lưu hash nếu sử dụng. |
| `verification_expired_at` (?) | Thời điểm mã xác thực hết hạn. |
| `is_verified` | Đánh dấu thông tin đăng nhập đã được xác thực theo chính sách. |
| `email_sent_count` | Số lần gửi mã trong kỳ giới hạn. |
| `email_sent_date` (?) | Ngày dùng để đặt lại số lần gửi mã. |
| `phone` (?) | Số điện thoại của tài khoản. |
| `fcm_token` (?) | Token nhận thông báo; một trường chỉ phù hợp nếu mỗi tài khoản quản lý một thiết bị tại một thời điểm. |

**Bỏ:** `Role_id`, `Permission_id`. Email và `provider_id` cần quy tắc unique phù hợp hình thức đăng nhập thực tế.

### `roles` — Vai trò được gán cho tài khoản

| Trường | Ý nghĩa |
|---|---|
| `role_id` (PK) | Định danh vai trò. |
| `role_name` (UQ) | Tên/mã vai trò, ví dụ ACCOUNTANT hoặc TECHNICAL_STAFF. |
| `description` | Phạm vi trách nhiệm của vai trò. |
| `is_active` | Có cho phép tiếp tục sử dụng vai trò. |
| `created_at` | Thời điểm tạo vai trò. |

### `user_roles` — Phân vai trò cho tài khoản

| Trường | Ý nghĩa |
|---|---|
| `user_id` (PK thành phần, FK → `users.id`) | Tài khoản nhận vai trò. |
| `role_id` (PK thành phần, FK → `roles.role_id`) | Vai trò được gán. |
| `assigned_at` | Thời điểm phân vai trò. |
| `assigned_by` (?, FK → `users.id`) | Người phân quyền. |

### `role_permissions` — Các quyền thuộc mỗi vai trò

| Trường | Ý nghĩa |
|---|---|
| `role_id` (PK thành phần, FK) | Vai trò nhận quyền. |
| `permission_id` (PK thành phần, FK) | Quyền được cấp; sửa lỗi `permision_id`. |

### `customers` — Hồ sơ người mua, kể cả khách không đăng nhập

| Trường | Ý nghĩa |
|---|---|
| `customer_id` (PK) | Định danh khách hàng. |
| `user_id` (UQ, FK, ?) | Tài khoản đăng nhập liên kết; để trống với khách không tạo tài khoản. |
| `customer_code` (UQ) | Mã khách hàng cho tra cứu. |
| `customer_name` | Tên người mua hoặc đầu mối liên hệ. |
| `phone` (?) | Số liên hệ phục vụ giao hàng/bảo hành; chính sách bắt buộc cần xác nhận. |
| `email` (?) | Email liên hệ, không mặc nhiên là email đăng nhập. |
| `customer_type` | Phân loại RETAIL, CONTRACTOR, INSTALLER, PROJECT_CUSTOMER; **[CẦN XÁC NHẬN]** phân loại này có loại trừ nhau không. |
| `company_name` (?) | Tên đơn vị/công ty nếu khách là tổ chức. |
| `status` | Trạng thái hồ sơ khách. |
| `created_at` | Thời điểm lập hồ sơ. |
| `updated_at` | Thời điểm cập nhật cuối. |

**Chuyển:** `Address` thành bản ghi trong `customer_addresses` để không có hai nguồn địa chỉ mặc định.

### `customer_addresses` — Các địa chỉ lưu cho khách

| Trường | Ý nghĩa |
|---|---|
| `address_id` (PK) | Định danh địa chỉ, bổ sung so với bản gốc. |
| `customer_id` (FK) | Khách sở hữu địa chỉ. |
| `recipient_name` | Tên người nhận tại địa chỉ. |
| `phone` | Số điện thoại người nhận. |
| `address_text` | Địa chỉ giao/lắp đặt đầy đủ. |
| `is_default` | Địa chỉ được chọn sẵn; tối đa một mặc định mỗi khách. |

### `customer_projects` — Công trình do khách mua thiết bị

| Trường | Ý nghĩa |
|---|---|
| `project_id` (PK) | Định danh công trình. |
| `customer_id` (FK) | Khách sở hữu công trình. |
| `project_name` | Tên công trình để nhận diện. |
| `project_address` | Địa điểm công trình, độc lập địa chỉ nhận hàng từng đợt. |
| `description` (?) | Mô tả nhu cầu thiết bị. |
| `start_date` (?) | Ngày bắt đầu dự kiến/thực tế theo quy ước đã chốt. |
| `expected_completion_date` (?) | Ngày hoàn thành dự kiến, không quản lý tiến độ thi công. |
| `status` | Trạng thái hồ sơ công trình. |
| `created_at` | Thời điểm lập hồ sơ. |

### `customer_interactions` — Lịch sử trao đổi và chăm sóc

| Trường | Ý nghĩa |
|---|---|
| `interaction_id` (PK) | Định danh lượt trao đổi. |
| `customer_id` (FK) | Khách tham gia. |
| `staff_id` (FK → `users.id`) | Nhân viên ghi nhận/trao đổi. |
| `interaction_type` | Kênh PHONE, ZALO, STORE…; giá trị cụ thể cần chốt. |
| `content` | Tóm tắt trao đổi hoặc yêu cầu. |
| `follow_up_at` (?) | Thời điểm cần liên hệ lại. |
| `created_at` | Thời điểm ghi nhận. |

### `audit_logs` — Nhật ký thay đổi dữ liệu nghiệp vụ

| Trường | Ý nghĩa |
|---|---|
| `audit_id` (PK) | Định danh sự kiện. |
| `user_id` (FK, ?) | Tài khoản thao tác; trống nếu tác vụ hệ thống tự thực hiện. |
| `entity_type` | Loại bản ghi bị tác động, ví dụ ORDER. |
| `entity_id` | ID bản ghi tại thời điểm tác động. |
| `action` | Hành động CREATE, UPDATE, CANCEL, CONFIRM… theo danh mục. |
| `old_data` (JSONB, ?) | Dữ liệu trước thay đổi, đã loại dữ liệu nhạy cảm. |
| `new_data` (JSONB, ?) | Dữ liệu sau thay đổi, đã loại dữ liệu nhạy cảm. |
| `reason` (?) | Lý do sửa/hủy/điều chỉnh khi nghiệp vụ yêu cầu. |
| `created_at` | Thời điểm xảy ra. |

## 4. Sản phẩm và nhà cung cấp

### `categories` — Nhóm sản phẩm có cấu trúc cha/con

| Trường | Ý nghĩa |
|---|---|
| `category_id` (PK) | Định danh nhóm. |
| `category_name` | Tên nhóm. |
| `description` (?) | Diễn giải phạm vi nhóm. |
| `status` | Có cho phép dùng nhóm mới. |
| `created_at` | Thời điểm tạo. |
| `updated_at` | Thời điểm cập nhật. |
| `parent_id` (FK tự tham chiếu, ?) | Nhóm cha; tránh chu trình cha–con. |

### `brands` — Hãng/nhãn hiệu sản phẩm

| Trường | Ý nghĩa |
|---|---|
| `brand_id` (PK) | Định danh hãng. |
| `brand_name` | Tên hãng. |
| `description` (?) | Mô tả hãng. |
| `status` | Có cho phép chọn hãng cho sản phẩm mới. |

### `products` — Mẫu sản phẩm dùng chung cho các biến thể

| Trường | Ý nghĩa |
|---|---|
| `product_id` (PK) | Định danh mẫu. |
| `category_id` (FK) | Nhóm sản phẩm. |
| `brand_id` (FK, ?) | Hãng sản xuất. |
| `product_name` | Tên mẫu dùng để tra cứu. |
| `description` (?) | Nội dung giới thiệu. |
| `specifications` (JSONB, ?) | Thuộc tính chung của mẫu. |
| `product_type` | NORMAL hoặc BUNDLE; bán bundle cần quy tắc riêng. |
| `status` | Có cho phép niêm yết/chọn bán. |
| `created_at` | Thời điểm tạo. |
| `updated_at` | Thời điểm sửa. |

**Bỏ:** `supplier_id`; nhà cung cấp nằm ở `supplier_products`.

### `product_variants` — Một SKU bán được của sản phẩm

| Trường | Ý nghĩa |
|---|---|
| `variant_id` (PK) | Định danh biến thể. |
| `product_id` (FK) | Mẫu chứa biến thể. |
| `sku` (UQ) | Mã SKU dùng chọn hàng/xuất kho. |
| `variant_name` | Tên phân biệt biến thể. |
| `color` (?) | Màu sắc nếu có. |
| `size` (?) | Kích thước nếu có. |
| `technical_spec` (JSONB, ?) | Thông số lắp đặt riêng của SKU. |
| `selling_price` | Giá bán hiện hành; giá đã chốt lấy từ dòng báo giá/đơn. |
| `warranty_policy_id` (FK, ?) | Chính sách bảo hành mặc định. |
| `serial_tracking` | SKU có cần quản lý từng serial. |
| `status` | Có cho phép chọn SKU cho chứng từ mới. |
| `created_at` | Thời điểm tạo. |
| `updated_at` | Thời điểm sửa. |

### `product_images` — Hình ảnh mẫu hoặc hình riêng SKU

| Trường | Ý nghĩa |
|---|---|
| `image_id` (PK) | Định danh ảnh. |
| `product_id` (FK) | Mẫu sở hữu ảnh. |
| `variant_id` (FK, ?) | SKU cụ thể nếu ảnh chỉ thuộc một biến thể; phải cùng `product_id`. |
| `image_url` | Đường dẫn ảnh. |
| `is_primary` | Ảnh đại diện trong đúng phạm vi mẫu/SKU. |
| `sort_order` | Thứ tự hiển thị. |

### `bundle_items` — Thành phần của combo

| Trường | Ý nghĩa |
|---|---|
| `bundle_item_id` (PK) | Định danh thành phần. |
| `bundle_product_id` (FK → `products`) | Mẫu combo chứa thành phần. |
| `variant_id` (FK) | SKU thành phần. |
| `quantity` | Số lượng SKU trong một combo. |

**[CẦN XÁC NHẬN]**: Nếu triển khai bán combo, cần SKU đại diện để đặt hàng và một chính sách trừ tồn thành phần; không trừ cả SKU combo lẫn thành phần cho cùng một lần bán.

### `suppliers` — Đơn vị bán hàng cho cửa hàng

| Trường | Ý nghĩa |
|---|---|
| `supplier_id` (PK) | Định danh nhà cung cấp. |
| `supplier_code` (UQ) | Mã nhà cung cấp. |
| `supplier_name` | Tên nhà cung cấp. |
| `contact_name` (?) | Người liên hệ. |
| `phone` (?) | Số liên hệ. |
| `email` (?) | Email liên hệ. |
| `address` (?) | Địa chỉ liên hệ. |
| `tax_code` (?) | Mã số thuế nếu có. |
| `status` | Có tiếp tục giao dịch. |
| `created_at` | Thời điểm tạo hồ sơ. |

**Bỏ:** `product_id`.

### `supplier_products` — Sản phẩm mà nhà cung cấp có thể cung ứng

| Trường | Ý nghĩa |
|---|---|
| `supplier_id` (PK thành phần, FK) | Nhà cung cấp. |
| `product_id` (PK thành phần, FK) | Mẫu sản phẩm cung ứng. |

## 5. Tồn kho một kho và chứng từ kho

### `inventories` — Số dư tồn hiện tại của từng SKU tại kho duy nhất

| Trường | Ý nghĩa |
|---|---|
| `inventory_id` (PK) | Định danh dòng số dư; không dùng làm FK đầu phiếu nhiều SKU. |
| `variant_id` (UQ, FK) | SKU; đúng một dòng tồn/SKU. |
| `on_hand_qty` | Số lượng vật lý hiện có trong kho. |
| `reserved_qty` | Tổng lượng trong kho đang được giữ cho đơn; đối chiếu với các reservation còn hiệu lực. |
| `minimum_qty` | Mức cảnh báo tồn thấp của SKU. |
| `updated_at` | Thời điểm số dư thay đổi gần nhất. |

**Bỏ:** `warehouse_name`, `address`, `status`; không có `warehouse_id`. `available_qty` tính bằng `on_hand_qty − reserved_qty`, tránh lưu bản sao nếu không cần tối ưu.

### `stock_reservations` — Ghi rõ SKU đã giữ cho dòng đơn nào **[mới]**

| Trường | Ý nghĩa |
|---|---|
| `reservation_id` (PK) | Định danh lần giữ. |
| `order_item_id` (FK) | Dòng đơn nhận hàng giữ. |
| `variant_id` (FK) | SKU được giữ, phải bằng SKU của dòng đơn. |
| `quantity` | Lượng ban đầu được giữ. |
| `consumed_qty` | Phần đã dùng để xuất kho. |
| `released_qty` | Phần đã giải phóng vì sửa/hủy/không dùng. |
| `status` | ACTIVE, CLOSED, CANCELLED theo chuyển trạng thái đã chốt. |
| `created_at` | Thời điểm giữ hàng. |
| `updated_at` | Thời điểm dùng/giải phóng gần nhất. |

`reserved_qty` = tổng `quantity − consumed_qty − released_qty` còn hiệu lực. Ba số không âm và `consumed_qty + released_qty ≤ quantity`. **[CẦN XÁC NHẬN]** thời điểm giữ hàng.

### `goods_receipts` — Đầu phiếu nhập từ một nhà cung cấp

| Trường | Ý nghĩa |
|---|---|
| `goods_receipt_id` (PK) | Định danh phiếu. |
| `receipt_code` (UQ) | Mã phiếu nhập. |
| `supplier_id` (FK) | Nhà cung cấp giao hàng. |
| `status` | DRAFT, CONFIRMED, CANCELLED. |
| `receipt_date` | Ngày nghiệp vụ nhận hàng. |
| `total_amount` | Tổng giá trị nhập, đối chiếu các dòng. |
| `created_by` (FK) | Người lập phiếu. |
| `confirmed_by` (FK, ?) | Người xác nhận số hàng nhập. |
| `confirmed_at` (?) | Thời điểm xác nhận. |
| `note` (?) | Ghi chú chung. |

**Bỏ:** `inventory_id`. Xác nhận phiếu tạo biến động tăng kho đúng một lần.

### `goods_receipt_items` — Các SKU thực nhập trên phiếu

| Trường | Ý nghĩa |
|---|---|
| `goods_receipt_item_id` (PK) | Định danh dòng nhập. |
| `goods_receipt_id` (FK) | Phiếu nhập chứa dòng. |
| `variant_id` (FK) | SKU được nhập. |
| `quantity` | Số lượng thực nhập của dòng theo quy ước. |
| `unit_cost` | Đơn giá nhập tại thời điểm chứng từ. |
| `line_total` | Thành tiền nhập của dòng. |

**[CẦN XÁC NHẬN]**: Nếu một phiếu ghi lượng dự kiến và nhận nhiều lần, cần tách lượng dự kiến với lượng thực nhận hoặc lập mỗi đợt nhận một phiếu; không cập nhật cùng dòng khiến trừ/cộng kho lặp.

### `stock_dispatch_notes` — Đầu phiếu xuất kho cho một đơn/đợt

| Trường | Ý nghĩa |
|---|---|
| `dispatch_id` (PK) | Định danh phiếu xuất. |
| `dispatch_code` (UQ) | Mã phiếu xuất. |
| `order_id` (FK) | Đơn có hàng được xuất. |
| `status` | DRAFT, CONFIRMED, CANCELLED. |
| `dispatch_date` | Ngày nghiệp vụ xuất hàng. |
| `created_by` (FK) | Người lập. |
| `confirmed_by` (FK, ?) | Người xác nhận xuất. |
| `confirmed_at` (?) | Thời điểm xác nhận xuất. |
| `note` (?) | Ghi chú xuất. |
| `created_at` | Thời điểm tạo. |
| `updated_at` | Thời điểm sửa. |

**Bỏ:** `inventory_id`. Mỗi phiếu xuất có thể có nhiều `stock_dispatch_items`; xác nhận giảm kho **một lần**.

### `stock_dispatch_items` — SKU, dòng đơn và số lượng xuất

| Trường | Ý nghĩa |
|---|---|
| `dispatch_item_id` (PK) | Định danh dòng xuất. |
| `dispatch_id` (FK) | Phiếu xuất chứa dòng. |
| `order_item_id` (FK) | Dòng đơn được thực hiện; thuộc `order_id` của đầu phiếu. |
| `variant_id` (FK) | SKU xuất; phải bằng SKU của `order_item_id`. |
| `quantity` | Số lượng thực xuất. |

**Bỏ:** `serial_numbers` JSONB; serial được gắn từng chiếc qua `dispatch_item_serials`.

### `stock_movements` — Sổ biến động số lượng theo SKU

| Trường | Ý nghĩa |
|---|---|
| `movement_id` (PK) | Định danh biến động. |
| `variant_id` (FK) | SKU tăng hoặc giảm số dư ở kho duy nhất. |
| `movement_type` | GOODS_RECEIPT, STOCK_DISPATCH, RETURN_IN, SUPPLIER_RETURN, ADJUSTMENT_IN, ADJUSTMENT_OUT. |
| `quantity` | Độ lớn biến động, dương; chiều tăng/giảm do loại biến động xác định. |
| `reference_type` | Loại chứng từ/sự kiện tạo biến động. |
| `reference_id` | ID chứng từ/sự kiện nguồn; cần duy nhất theo hành động gây biến động. |
| `note` (?) | Diễn giải. |
| `created_by` (FK) | Người xác nhận thao tác tạo biến động. |
| `created_at` | Thời điểm ghi sổ. |

**Bỏ:** `inventory_id` để không lặp `variant_id`. Trường hợp có hơn một dòng cùng SKU trên một chứng từ, `reference_id` phải trỏ **dòng chứng từ** hoặc thêm mã sự kiện để chống ghi sổ trùng.

### `stock_counts` — Đầu phiếu kiểm kê toàn kho hoặc một tập SKU

| Trường | Ý nghĩa |
|---|---|
| `stock_count_id` (PK) | Định danh đợt kiểm. |
| `status` | Trạng thái lập/hoàn tất/huỷ, cần định nghĩa chính xác. |
| `counted_by` (FK) | Người chịu trách nhiệm kiểm kê. |
| `count_date` | Ngày kiểm thực tế. |
| `note` (?) | Ghi chú chung. |
| `created_at` | Thời điểm lập. |

**Bỏ:** `inventory_id` khỏi đầu phiếu.

### `stock_count_items` — Kết quả kiểm kê từng SKU

| Trường | Ý nghĩa |
|---|---|
| `stock_count_item_id` (PK) | Định danh dòng kiểm. |
| `stock_count_id` (FK) | Phiếu kiểm chứa dòng. |
| `variant_id` (FK) | SKU được kiểm. |
| `system_qty` | Tồn hệ thống tại thời điểm chốt số đối chiếu, lưu snapshot. |
| `actual_qty` | Số lượng đếm thực tế. |
| `difference_qty` | Chênh lệch `actual_qty − system_qty`; số dẫn xuất. |
| `reason` (?) | Lý do chênh lệch. |

Một SKU xuất hiện một lần trên một phiếu; khi duyệt điều chỉnh chênh lệch tạo `stock_movements` đúng một lần.

## 6. Báo giá, đơn hàng và hóa đơn

### `quotations` — Báo giá gửi khách

| Trường | Ý nghĩa |
|---|---|
| `quotation_id` (PK) | Định danh báo giá. |
| `quotation_code` (UQ) | Mã báo giá. |
| `customer_id` (FK) | Khách nhận báo giá. |
| `project_id` (FK, ?) | Công trình, phải thuộc cùng khách. |
| `status` | DRAFT, SENT, ACCEPTED, REJECTED, CANCELLED, CONVERTED. |
| `quotation_date` | Ngày báo giá. |
| `valid_until` (?) | Hạn hiệu lực của giá chào. |
| `subtotal` | Tổng trước chiết khấu. |
| `discount_amount` | Giảm giá toàn báo giá. |
| `total_amount` | Số tiền chào khách. |
| `created_by` (FK) | Nhân viên lập. |
| `approved_at` (?) | Thời điểm chấp thuận; **[CẦN XÁC NHẬN]** ai là người chấp thuận. |
| `created_at` | Thời điểm tạo. |

Đã ACCEPTED thì không sửa đè lịch sử; thay đổi sau đợt giao đầu phải tạo phiên bản/chứng từ điều chỉnh liên kết báo giá gốc. Cách đánh số phiên bản **[CẦN XÁC NHẬN]**.

### `quotation_items` — SKU và giá chào trong báo giá

| Trường | Ý nghĩa |
|---|---|
| `quotation_item_id` (PK) | Định danh dòng báo giá. |
| `quotation_id` (FK) | Báo giá chứa dòng. |
| `variant_id` (FK) | SKU được chào. |
| `quantity` | Số lượng chào. |
| `unit_price` | Đơn giá chào đã lưu, không tự đổi theo giá danh mục. |
| `discount_amount` | Giảm giá của dòng. |
| `line_total` | Thành tiền dòng sau giảm giá. |

### `carts` — Giỏ tạm của khách mua online

| Trường | Ý nghĩa |
|---|---|
| `cart_id` (PK) | Định danh giỏ. |
| `customer_id` (FK) | Khách sở hữu giỏ. |
| `status` | Trạng thái đang dùng/đã chuyển đơn/bỏ, cần chốt mã. |
| `created_at` | Thời điểm tạo giỏ. |
| `updated_at` | Thời điểm cập nhật. |

### `cart_items` — SKU khách dự định mua

| Trường | Ý nghĩa |
|---|---|
| `cart_item_id` (PK) | Định danh dòng giỏ. |
| `cart_id` (FK) | Giỏ chứa dòng. |
| `variant_id` (FK) | SKU khách chọn. |
| `quantity` | Số lượng dự định mua. |
| `unit_price` | Giá hiển thị khi đưa vào giỏ; **không phải cam kết giá bán**. |

Chuyển giỏ thành đơn phải kiểm lại giá, tình trạng SKU và khả dụng tồn theo chính sách đã thống nhất.

### `promotions` — Chương trình giảm giá

| Trường | Ý nghĩa |
|---|---|
| `promotion_id` (PK) | Định danh chương trình. |
| `promotion_code` (UQ) | Mã áp dụng. |
| `promotion_name` | Tên chương trình. |
| `discount_type` | PERCENTAGE hoặc FIXED_AMOUNT. |
| `discount_value` | Giá trị giảm theo loại. |
| `start_at` | Thời điểm bắt đầu hiệu lực. |
| `end_at` | Thời điểm kết thúc. |
| `status` | Trạng thái cho phép áp dụng. |

### `orders` — Hồ sơ đơn hàng tại quầy hoặc online

| Trường | Ý nghĩa |
|---|---|
| `order_id` (PK) | Định danh đơn. |
| `order_code` (UQ) | Mã đơn hiển thị. |
| `customer_id` (FK) | Khách mua. |
| `project_id` (FK, ?) | Công trình, phải thuộc khách mua. |
| `quotation_id` (FK, ?) | Báo giá chấp thuận làm căn cứ lập đơn, cùng khách/công trình. |
| `sales_staff_id` (FK, ?) | Nhân viên phụ trách. |
| `promotion_id` (FK, ?) | Chương trình giảm giá nếu áp dụng. |
| `order_channel` | IN_STORE hoặc ONLINE. |
| `order_status` | Trạng thái xử lý đơn; định nghĩa bảng chuyển trạng thái trước khi code. |
| `payment_status` | UNPAID, PARTIALLY_PAID, PAID; dẫn xuất từ khoản thu đã xác nhận và hoàn tiền. |
| `order_date` | Thời điểm khách đặt/chốt theo quy ước. |
| `has_contract` | Có hợp đồng/thỏa thuận hợp đồng gắn đơn. **[CẦN XÁC NHẬN]** tiêu chí xác định. |
| `contract_reference` (?, bổ sung) | Số/mã hợp đồng nếu có. |
| `contract_document_url` (?, bổ sung) | Tệp hợp đồng nếu nhóm lưu bản số. |
| `payment_terms` (bổ sung) | Quy tắc PAY_UPFRONT, PAY_ON_DELIVERY, INSTALLMENTS, PAY_AFTER_COMPLETION…; giá trị chốt theo nghiệp vụ. |
| `payment_terms_approved_by` (FK, ?) | Kế toán duyệt điều khoản trả cuối/trả chậm khi cần. |
| `payment_terms_approved_at` (?) | Thời điểm duyệt. |
| `subtotal` | Tổng dòng trước giảm giá toàn đơn. |
| `discount_amount` | Giảm giá toàn đơn. |
| `total_amount` | Tổng khách phải trả sau giảm giá. |
| `delivery_address` (?) | Địa chỉ trên đơn để tham chiếu; địa chỉ thực tế từng đợt ở `deliveries`. |
| `note` (?) | Ghi chú đơn. |
| `created_at` | Thời điểm tạo. |
| `updated_at` | Thời điểm sửa gần nhất. |

**Bỏ:** `due_date` đơn lẻ, chuyển sang `order_payment_schedules`. `has_contract` và `contract_reference` phải nhất quán; nếu sau này một hợp đồng có nhiều đơn, cần nâng cấp thành bảng hợp đồng riêng.

### `order_items` — Dòng hàng và đơn giá đã chốt

| Trường | Ý nghĩa |
|---|---|
| `order_item_id` (PK) | Định danh dòng đơn. |
| `order_id` (FK) | Đơn chứa dòng. |
| `variant_id` (FK) | SKU đã đặt. |
| `sku_snapshot` (bổ sung) | Mã SKU tại lúc chốt đơn để in/đối chiếu lịch sử. |
| `item_name_snapshot` (bổ sung) | Tên sản phẩm/biến thể lúc chốt. |
| `quantity` | Tổng lượng khách đặt đang có hiệu lực. |
| `unit_price` | Giá bán đã chốt, không lấy lại từ giá hiện hành. |
| `discount_amount` | Giảm giá trên dòng. |
| `line_total` | Thành tiền dòng. |

Khi thay đổi lượng sau một đợt giao, phải ghi audit/lịch sử và giữ được tổng đã giao; yêu cầu phiên bản thay đổi **[CẦN XÁC NHẬN]**.

### `sales_invoices` — Chứng từ bán hàng phát hành cho khách

| Trường | Ý nghĩa |
|---|---|
| `invoice_id` (PK) | Định danh chứng từ. |
| `invoice_code` (UQ) | Số chứng từ. |
| `order_id` (FK) | Đơn làm căn cứ lập. |
| `customer_id` (FK) | Khách trên chứng từ, nhất quán với đơn. |
| `invoice_type` | RETAIL_RECEIPT hoặc SALES_INVOICE theo định nghĩa của nhóm. |
| `issue_date` | Ngày phát hành. |
| `subtotal` | Giá trị trước giảm/thuế theo công thức đã chốt. |
| `discount_amount` | Giảm giá thể hiện trên chứng từ. |
| `tax_amount` | Số thuế thể hiện nếu áp dụng. |
| `total_amount` | Tổng tiền chứng từ. |
| `status` | DRAFT, ISSUED, CANCELLED. |
| `created_by` (FK) | Người lập. |
| `created_at` | Thời điểm tạo. |

Phạm vi chứng từ bán hàng của AQS và tích hợp hóa đơn điện tử **[CẦN XÁC NHẬN]**; bản thiết kế không mặc định AQS tự phát hành hóa đơn thuế hợp lệ.

### `sales_invoice_items` — Các dòng trên chứng từ bán hàng

| Trường | Ý nghĩa |
|---|---|
| `invoice_item_id` (PK) | Định danh dòng chứng từ. |
| `invoice_id` (FK) | Chứng từ chứa dòng. |
| `order_item_id` (FK) | Dòng đơn làm căn cứ. |
| `variant_id` (FK) | SKU trên chứng từ, phải khớp dòng đơn. |
| `description` | Mô tả hàng tại thời điểm phát hành. |
| `quantity` | Số lượng ghi trên chứng từ. |
| `unit_price` | Đơn giá chứng từ đã lưu. |
| `discount_amount` | Giảm giá dòng. |
| `tax_rate` | Thuế suất áp dụng nếu chứng từ yêu cầu. |
| `line_total` | Thành tiền dòng. |

## 7. Thanh toán, COD và thu chi

### `order_payment_schedules` — Các khoản phải thu theo thỏa thuận **[mới]**

| Trường | Ý nghĩa |
|---|---|
| `schedule_id` (PK) | Định danh mốc thu. |
| `order_id` (FK) | Đơn phát sinh mốc. |
| `sequence_no` | Thứ tự mốc trong đơn. |
| `amount_due` | Số tiền phải thu ở mốc. |
| `due_date` (?) | Ngày đến hạn nếu thỏa thuận theo ngày. |
| `trigger_type` | Điều kiện làm mốc đến hạn: ngày cố định, xác nhận giao đợt, giao đủ/khách nhận đủ… theo danh mục được chốt. |
| `status` | PLANNED, DUE, SETTLED, CANCELLED; dẫn xuất hoặc cập nhật có kiểm soát. |
| `note` (?) | Diễn giải mốc/thỏa thuận. |

Tổng các mốc còn hiệu lực phải bằng số tiền cần thu theo đơn. Mốc trả cuối và trường hợp phải nghiệm thu **[CẦN XÁC NHẬN]**.

### `payments` — Một lần khách trả tiền cho một đơn

| Trường | Ý nghĩa |
|---|---|
| `payment_id` (PK) | Định danh khoản thu. |
| `order_id` (FK) | Đơn được thanh toán. |
| `customer_id` (FK) | Khách trả, phải phù hợp đơn; nếu không cần tìm trực tiếp có thể suy ra qua đơn. |
| `amount` | Số tiền khách thực trả, dương. |
| `payment_method` | CASH, CARD, BANK_TRANSFER; thể hiện **phương tiện** trả tiền. |
| `collection_channel` (bổ sung) | IN_STORE, ONLINE, AT_DELIVERY…; AT_DELIVERY là trường hợp COD về thời điểm/địa điểm thu. |
| `payment_type` | FULL, PARTIAL, DEBT_PAYMENT nếu nhóm cần phân loại; là nhãn nghiệp vụ, không quyết định số dư. |
| `verification_status` | PENDING, UNMATCHED, CONFIRMED, REJECTED; chỉ CONFIRMED được tính đã trả. |
| `paid_at` (?) | Thời điểm khách thực giao/chuyển tiền, không mặc nhiên là lúc kế toán xác nhận. |
| `received_by` (FK, ?, bổ sung) | Người nhận tiền mặt/COD từ khách nếu có. |
| `recorded_by` (FK) | Người nhập khoản thu vào AQS. |
| `verified_by` (FK, ?) | Kế toán xác nhận khoản thực nhận. |
| `verified_at` (?) | Thời điểm kế toán xác nhận. |
| `reference_no` (?) | Mã giao dịch do ngân hàng/đơn vị thanh toán cấp. |
| `proof_url` (?) | Chứng từ/ảnh chứng minh. |
| `note` (?) | Ghi chú đối soát. |
| `created_at` | Thời điểm tạo bản ghi. |

**Bỏ:** COD khỏi `payment_method` để không trộn phương thức với kênh thu. Giao dịch ngân hàng phải chống ghi trùng theo khóa tham chiếu phù hợp nguồn/bank; tiền mặt không bắt buộc có `reference_no`.

### `payment_allocations` — Phân bổ khoản thu vào các mốc **[mới]**

| Trường | Ý nghĩa |
|---|---|
| `allocation_id` (PK) | Định danh phân bổ. |
| `payment_id` (FK) | Khoản tiền khách đã trả. |
| `schedule_id` (FK) | Mốc phải thu được thanh toán. |
| `amount` | Số tiền từ payment phân bổ cho mốc. |

Khoản thu và mốc phải thuộc **cùng một đơn**. Chỉ tính phân bổ từ payment CONFIRMED; tổng phân bổ không vượt khoản thu hoặc mốc. Nếu nhóm quy định mỗi lần trả chỉ thuộc một mốc, có thể bỏ bảng này và dùng `payments.schedule_id`.

### `deliveries` — Một đợt giao hàng của một đơn

| Trường | Ý nghĩa |
|---|---|
| `delivery_id` (PK) | Định danh đợt giao. |
| `delivery_code` (UQ) | Mã phiếu giao. |
| `dispatch_id` (FK) | Phiếu xuất chuẩn bị cho đợt giao; **[CẦN XÁC NHẬN]** một đợt gắn đúng một phiếu xuất. |
| `order_id` (FK) | Đơn được giao; phải bằng đơn của phiếu xuất. |
| `shipper_id` (FK → `users.id`, ?) | Nhân viên giao hàng nội bộ. |
| `batch_no` | Thứ tự đợt giao trong một đơn. |
| `delivery_status` | Trạng thái giao; cần danh mục và quy tắc chuyển trạng thái. |
| `scheduled_date` (?) | Ngày dự kiến giao. |
| `delivered_at` (?) | Thời điểm khách xác nhận nhận hàng. |
| `recipient_name` | Tên người nhận thực tế. |
| `recipient_phone` | Số liên lạc tại điểm giao. |
| `delivery_address` | Địa chỉ riêng của đợt giao. |
| `cod_required` | Đợt này dự kiến thu tiền khi giao. |
| `cod_amount` | Số tiền dự kiến thu; **không** là số tiền khách đã thanh toán. |
| `note` (?) | Ghi chú giao hàng. |
| `created_at` | Thời điểm lập đợt giao. |

**Bỏ:** `inventory_id`. `UNIQUE(order_id, batch_no)` nếu batch_no được dùng làm số thứ tự đơn.

### `delivery_items` — Lượng khách thực nhận trong mỗi đợt

| Trường | Ý nghĩa |
|---|---|
| `delivery_item_id` (PK) | Định danh dòng giao. |
| `delivery_id` (FK) | Đợt giao chứa dòng. |
| `dispatch_item_id` (FK) | Dòng hàng đã xuất để đi giao. |
| `order_item_id` (FK) | Dòng đơn làm căn cứ; phải khớp với dòng xuất. |
| `planned_qty` (đổi tên từ `quantity`) | Số lượng chuẩn bị giao. |
| `accepted_qty` (bổ sung) | Số lượng người nhận chấp thuận, tính vào đã giao. |
| `rejected_qty` (bổ sung) | Số lượng bị từ chối/không nhận. |

`accepted_qty + rejected_qty ≤ planned_qty`. Hàng chưa được khách nhận không tự nhập lại kho; phải xác minh vị trí và có nghiệp vụ kho phù hợp. **Bỏ:** `serial_numbers`, serial giao thực nhận tra qua `product_serials.delivered_delivery_item_id`.

### `cod_handovers` — Một lần nhân viên giao hàng nộp tiền cho kế toán

| Trường | Ý nghĩa |
|---|---|
| `handover_id` (PK) | Định danh phiếu bàn giao nội bộ. |
| `shipper_id` (FK) | Người nộp tiền. |
| `handover_date` | Ngày nộp tiền. |
| `total_amount` | Tổng số tiền bàn giao, bằng tổng chi tiết được xác nhận. |
| `status` | PENDING, CONFIRMED, REJECTED. |
| `received_by` (FK, ?) | Kế toán/người nhận tiền nội bộ. |
| `verified_at` (?) | Thời điểm xác nhận bàn giao. |
| `note` (?) | Diễn giải chênh lệch hoặc bàn giao. |

### `cod_handover_items` — Khoản thu COD nằm trong phiếu bàn giao

| Trường | Ý nghĩa |
|---|---|
| `handover_item_id` (PK) | Định danh dòng bàn giao. |
| `handover_id` (FK) | Phiếu bàn giao chứa dòng. |
| `delivery_id` (FK) | Đợt giao đã thu tiền. |
| `payment_id` (FK) | Khoản tiền khách trả tại lần giao; đơn phải khớp `delivery_id`. |
| `collected_amount` | Số tiền thực tế giao lại trong dòng này. |

**[CẦN XÁC NHẬN]** có cho nộp tiền một khoản COD thành nhiều lần không. Nếu không, đặt UQ `payment_id` và yêu cầu bàn giao đủ; nếu có, cho nhiều dòng nhưng tổng nộp không vượt số tiền khách đã trả.

### `transactions` — Thu/chi phục vụ báo cáo quản trị, không phải sổ cái kế toán

| Trường | Ý nghĩa |
|---|---|
| `transaction_id` (PK) | Định danh khoản thu/chi quản trị. |
| `transaction_code` (UQ) | Mã giao dịch nội bộ. |
| `transaction_type` | RECEIPT, PAYMENT, REFUND, ADJUSTMENT. |
| `category` | Nhóm nguồn thu/loại chi do nhóm định nghĩa. |
| `customer_id` (FK, ?) | Khách liên quan nếu có. |
| `supplier_id` (FK, ?) | Nhà cung cấp liên quan nếu có. |
| `order_id` (FK, ?) | Đơn liên quan nếu có. |
| `payment_id` (FK, ?) | Khoản khách trả gốc khi là thu từ đơn; có quy tắc tránh tạo trùng. |
| `goods_receipt_id` (FK, ?) | Phiếu nhập liên quan chi hàng nhập nếu có. |
| `invoice_id` (FK, ?) | Chứng từ bán hàng liên quan nếu có. |
| `amount` | Số tiền giao dịch, dương; hướng thu/chi theo `transaction_type`. |
| `payment_method` | Phương thức tiền thực thu/chi. |
| `transaction_date` | Ngày tiền thực phát sinh theo quy ước. |
| `status` | Trạng thái ghi nhận/duyệt, cần chốt. |
| `description` (?) | Diễn giải thu/chi. |
| `created_by` (FK) | Người lập. |
| `approved_by` (FK, ?) | Người duyệt nếu có. |
| `created_at` | Thời điểm tạo. |

Không sinh `RECEIPT` hai lần cho cùng một khoản `payments.CONFIRMED`. Bàn giao COD chỉ là dịch chuyển tiền nội bộ, không cộng thêm doanh thu/tiền khách trả. Chi phí và giá vốn phải có định nghĩa riêng khi tính lợi nhuận.

## 8. Serial, bảo hành và công việc kỹ thuật

### `product_serials` — Danh tính của từng sản phẩm theo serial **[mới]**

| Trường | Ý nghĩa |
|---|---|
| `serial_id` (PK) | Định danh nội bộ cho một thiết bị vật lý. |
| `variant_id` (FK) | SKU của thiết bị. |
| `serial_number` | Số serial trên thiết bị; phạm vi UQ **[CẦN XÁC NHẬN]** theo hãng/SKU/toàn hệ thống. |
| `goods_receipt_item_id` (FK, ?) | Dòng nhập đưa thiết bị vào hệ thống. |
| `delivered_delivery_item_id` (FK, ?) | Dòng giao mà khách đã thực nhận serial. |
| `status` | Tình trạng tồn/xuất/đã giao theo vòng đời đã chốt. |

### `dispatch_item_serials` — Các thiết bị serial được chọn cho dòng xuất **[mới]**

| Trường | Ý nghĩa |
|---|---|
| `dispatch_item_id` (PK thành phần, FK) | Dòng phiếu xuất. |
| `serial_id` (PK thành phần, FK) | Thiết bị đã chọn, `variant_id` phải khớp dòng xuất. |

Một serial không thể xuất đồng thời trên hai phiếu xuất còn hiệu lực. Số serial chọn bằng lượng xuất của SKU theo serial; cách xử lý phiếu bị huỷ/đổi thiết bị cần quy tắc chuyển trạng thái.

### `warranty_policies` — Thời hạn và điều kiện mặc định

| Trường | Ý nghĩa |
|---|---|
| `warranty_policy_id` (PK) | Định danh chính sách. |
| `policy_name` | Tên chính sách/hãng. |
| `duration_months` | Số tháng bảo hành nếu tính theo tháng. |
| `conditions` | Điều kiện áp dụng/loại trừ theo nguồn chính sách. |
| `status` | Chính sách còn áp dụng cho sản phẩm mới. |
| `created_at` | Thời điểm tạo. |

### `warranty_records` — Quyền bảo hành phát sinh từ hàng đã mua

| Trường | Ý nghĩa |
|---|---|
| `warranty_record_id` (PK) | Định danh hồ sơ bảo hành. |
| `customer_id` (FK) | Khách sở hữu hồ sơ. |
| `order_item_id` (FK) | Dòng mua hàng làm bằng chứng; phải thuộc cùng khách. |
| `warranty_policy_id` (FK) | Chính sách áp dụng lúc mua. |
| `serial_id` (FK, ?) | Thiết bị có serial; thay cho `serial_number` dạng text. |
| `start_date` | Ngày bắt đầu hiệu lực theo chính sách sản phẩm. |
| `end_date` | Ngày hết hiệu lực. |
| `status` | Trạng thái hiệu lực của quyền bảo hành. |

Với hàng không serial, phải chốt hồ sơ đại diện mỗi đơn vị hay cả dòng có số lượng >1; tra cứu bằng số điện thoại và ngày mua như nguồn khảo sát.

### `warranty_claims` — Một yêu cầu bảo hành thực tế

| Trường | Ý nghĩa |
|---|---|
| `claim_id` (PK) | Định danh yêu cầu. |
| `warranty_record_id` (FK) | Hồ sơ bảo hành làm căn cứ. |
| `customer_id` (FK) | Khách yêu cầu, phải phù hợp hồ sơ. |
| `issue_description` | Mô tả lỗi/triệu chứng. |
| `status` | RECEIVED, UNDER_INSPECTION, IN_PROGRESS, COMPLETED, REJECTED. |
| `result` (?) | Kết quả xử lý. |
| `rejection_reason` (?) | Lý do từ chối. |
| `received_at` | Thời điểm tiếp nhận. |
| `completed_at` (?) | Thời điểm hoàn tất. |
| `created_by` (FK) | Người lập yêu cầu. |
| `service_provider` (?, bổ sung) | Hãng/nhà phân phối thực hiện bảo hành nếu cần theo dõi; hình thức dữ liệu **[CẦN XÁC NHẬN]**. |

### `technical_jobs` — Lịch lắp đặt, sửa chữa, hỗ trợ bảo hành

| Trường | Ý nghĩa |
|---|---|
| `technical_job_id` (PK) | Định danh công việc. |
| `customer_id` (FK) | Khách được phục vụ. |
| `order_id` (FK, ?) | Đơn liên quan nếu là lắp đặt/hỗ trợ sau bán. |
| `claim_id` (FK, ?) | Yêu cầu bảo hành liên quan nếu là công việc bảo hành. |
| `assigned_to` (FK → `users.id`, ?, đổi tên từ `user_id`) | Kỹ thuật viên được giao việc. |
| `job_type` | INSTALLATION, REPAIR, WARRANTY_SERVICE. |
| `service_address` | Địa điểm thực hiện. |
| `description` | Nội dung được giao. |
| `scheduled_at` (?) | Thời gian dự kiến. |
| `status` | Trạng thái công việc, cần danh mục và điều kiện chuyển. |
| `result` (?) | Kết quả thực hiện. |
| `technical_note` (?) | Ghi chú kỹ thuật. |
| `completed_at` (?) | Thời điểm hoàn tất. |
| `created_at` | Thời điểm tạo việc. |

Một việc giao một kỹ thuật viên là giả định tối giản; nếu cần nhiều người cùng việc sẽ thêm bảng phân công.

## 9. AI hỗ trợ tư vấn và chủ cửa hàng

### `ai_chat_sessions` — Một phiên trao đổi với AI

| Trường | Ý nghĩa |
|---|---|
| `session_id` (PK) | Định danh phiên. |
| `customer_id` (FK, ?) | Khách hỏi tư vấn nếu xác định được. |
| `user_id` (FK, ?) | Tài khoản tham gia/truy vấn; phân biệt nguồn qua `session_type`. |
| `session_type` | PRODUCT_ADVISOR hoặc TELEGRAM_ASSISTANT. |
| `started_at` | Thời điểm bắt đầu. |
| `ended_at` (?) | Thời điểm kết thúc. |

Nếu website cho khách chưa đăng nhập hỏi AI, cả `customer_id` và `user_id` có thể trống; cần cách theo dõi phiên tạm và chính sách giữ dữ liệu **[CẦN XÁC NHẬN]**.

### `ai_chat_messages` — Nội dung từng lượt hội thoại

| Trường | Ý nghĩa |
|---|---|
| `message_id` (PK) | Định danh tin nhắn. |
| `session_id` (FK) | Phiên chứa tin. |
| `sender_type` | USER, ASSISTANT, SYSTEM. |
| `message_text` | Nội dung lời nhắn. |
| `metadata` (JSONB, ?) | Ngữ cảnh truy vấn, SKU tham chiếu hoặc dữ liệu kỹ thuật không nhạy cảm. |
| `created_at` | Thời điểm tin được ghi. |

AI chỉ hỗ trợ tư vấn/tra cứu theo phạm vi dự án; không tự tạo/sửa/hủy đơn, đổi giá hay điều chỉnh tồn kho.

## 10. Quan hệ và điều kiện bắt buộc khi hiện thực

| Kiểm tra | Điều kiện |
|---|---|
| Một kho | `inventories.variant_id` UQ; phiếu đầu không có `inventory_id`; tồn được cập nhật bằng giao dịch nhất quán với biến động. |
| Tồn khả dụng | `on_hand_qty ≥ 0`, `reserved_qty ≥ 0`, `reserved_qty ≤ on_hand_qty` nếu không cho giữ vượt tồn. |
| Phiếu nhiều dòng | Một phiếu có nhiều SKU; `quantity > 0`, không lấy SKU của đầu phiếu. |
| Cùng khách/công trình | `orders.project_id`, `quotations.project_id` phải thuộc khách ở đầu chứng từ. |
| Cùng đơn/SKU | Dòng xuất và dòng giao phải khớp `order_id`, `order_item_id`, `variant_id`. |
| Giao nhiều đợt | Tổng `accepted_qty` của dòng đơn không vượt `order_items.quantity`; số còn giao = lượng đặt hiện tại − tổng lượng thực nhận hợp lệ. |
| Hạch toán kho | Xác nhận phiếu xuất trừ tồn một lần; xác nhận giao không trừ lại. Nhập lại chỉ khi hàng thực về kho. |
| Thanh toán | Số đã trả = tổng khoản thu CONFIRMED − tổng hoàn tiền hợp lệ, không bao gồm bàn giao COD; không tự phát sinh khoản thu khi tạo giao dịch nội bộ. |
| Phân kỳ | Tổng `payment_allocations` trên một payment không vượt `payment.amount`; mốc và payment cùng đơn; trạng thái mốc phải dựa vào số đã xác nhận. |
| Dữ liệu lịch sử | Phiếu và khoản thu đã xác nhận không bị sửa đè trực tiếp; ghi audit và nghiệp vụ điều chỉnh hợp lệ. |
| Serial | Chỉ SKU có `serial_tracking = true` mới gắn serial; serial xuất/giao phải khớp SKU và số lượng thiết bị thực tế. |
| Bảo mật | `password` chỉ lưu hash; `audit_logs` không lưu password, OTP, provider token hoặc dữ liệu nhạy cảm khác. |

Các điều kiện xuyên nhiều dòng/bảng cần khóa hoặc giao dịch trong application/database. Unique/PK/FK/CHECK chỉ kiểm soát một phần; không dùng `CHECK` dòng đơn để tính tổng các đợt giao hay tổng phân bổ của các dòng khác.

## 11. Các quyết định BA cần chốt với cửa hàng trước khi code

1. **Giữ hàng:** bắt đầu lúc khách đặt, nhân viên xác nhận đơn hay sau khi khoản đầu đã được kế toán xác nhận? Có thời hạn giữ không?
2. **Điều khoản thu:** đơn có hợp đồng được nhận diện bằng gì; khoản cuối đến hạn khi giao đủ, khi khách xác nhận nhận đủ hay cần nghiệm thu? Kế toán duyệt loại thỏa thuận nào?
3. **COD:** người giao có thể nhận tiền chuyển khoản tại điểm giao không; có được nộp một khoản COD nhiều lần không; kế toán xác nhận khoản thu vào thời điểm nào?
4. **Giao thiếu/giao lỗi:** hàng khách không nhận đang nằm ở đâu, ai xác nhận nhập lại và khi nào được cộng tồn?
5. **Phiếu nhập nhiều đợt:** mỗi lần nhận lập phiếu riêng hay một phiếu theo dõi nhiều lần nhận?
6. **Combo:** có bán combo trong phiên bản này; tồn quản lý theo thành phần hay SKU combo?
7. **Serial:** số serial có duy nhất toàn hệ thống hay chỉ trong phạm vi hãng/SKU; xử lý serial đổi trả thế nào?
8. **Bảo hành:** hàng không serial một dòng có nhiều sản phẩm được theo dõi từng chiếc hay theo toàn dòng; bên thực hiện hãng/NPP được quản lý tới mức nào?
9. **Báo giá/đơn:** khi thay đổi sau giao một phần, lập phiên bản báo giá mới hay lập phụ lục điều chỉnh; ai duyệt?
10. **Giá vốn/lợi nhuận:** dùng giá vốn nào để báo cáo lãi lỗ theo đơn; giới hạn báo cáo quản trị và chứng từ bán hàng thực tế là gì?

## 12. Kết quả và phạm vi

Mô hình đích này có **52 bảng**: 47 bảng gốc đã chuẩn hóa tên/trách nhiệm, cộng 5 bảng (`stock_reservations`, `order_payment_schedules`, `payment_allocations`, `product_serials`, `dispatch_item_serials`), **không có bảng kho riêng**. `customer_address` đổi tên và thêm khóa chính. Đây là thiết kế đầy đủ theo phạm vi nêu trong cuộc trao đổi, cần chuyển thành migration/DDL và sơ đồ ERD sau khi chốt 10 quyết định nghiệp vụ ở trên.
