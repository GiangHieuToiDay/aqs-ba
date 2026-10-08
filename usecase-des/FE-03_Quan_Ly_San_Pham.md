# PHÂN HỆ FE-03: QUẢN LÝ SẢN PHẨM (PRODUCT CATALOG MANAGEMENT)

---

### UC-019 Quản lý danh mục sản phẩm (Manage Product Categories)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to manage product categories (e.g., Smart Toilets, Basin Sinks, Thermostatic Showers, Bathroom Accessories) in a hierarchical tree structure so that products are systematically organized for browsing and inventory control.  

**Preconditions**  
Chủ cửa hàng đã đăng nhập thành công với vai trò `Store Owner` và truy cập vào mục "Danh mục sản phẩm".  

**Normal Flow**  
*Quản lý cấu trúc cây phân cấp danh mục thiết bị vệ sinh*  
1. Chủ cửa hàng truy cập màn hình "Quản lý danh mục sản phẩm".  
2. Hệ thống tải và hiển thị danh mục dạng cây đa cấp (Parent-Child Tree View):  
   - Danh mục cha cấp 1 (ví dụ: `Bồn cầu`, `Chậu rửa & Lavabo`, `Sen tắm & Vòi`, `Bồn tắm`, `Gương & Phụ kiện`).  
   - Danh mục con cấp 2 (ví dụ trong `Bồn cầu`: `Bồn cầu thông minh`, `Bồn cầu 1 khối`, `Bồn cầu 2 khối`, `Bồn cầu treo tường`).  
   - Thông tin mỗi nhánh: Mã danh mục, Tên danh mục, Ảnh biểu tượng (Icon), Số lượng sản phẩm liên kết, Trạng thái (Hiển thị / Ẩn).  
   - Thanh công cụ tác vụ: Thêm danh mục cha, Thêm danh mục con, Kéo thả sắp xếp thứ tự hiển thị (Drag & Drop Reorder), Chỉnh sửa, Ẩn/Hiện.  
3. Chủ cửa hàng thực hiện sắp xếp lại thứ tự hiển thị bằng cách kéo thả nhánh danh mục lên vị trí ưu tiên trên thanh điều hướng website.  
4. Hệ thống cập nhật thứ tự `DisplayOrder` và lưu vào cơ sở dữ liệu.  
5. Hệ thống hiển thị thông báo thành công (MSG134: "Cập nhật thứ tự danh mục thành công!").  

**Alternative Flows**  
- **Danh mục chưa có dữ liệu:**  
  1. Hệ thống mới khởi tạo chưa có danh mục.  
  2. Hệ thống hiển thị cây rỗng kèm nút "Tạo danh mục gốc đầu tiên" (UC-164).  

**Postconditions**  
Cấu trúc phân loại sản phẩm của cửa hàng được cập nhật và phản ánh trên giao diện website và các màn hình quản trị.  

---

### UC-020 Tạo mới sản phẩm (Create Product)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to create a new product container (general name, brand, category, description, installation specs, features, origin) so that I can define variants and list it for sale.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập vào hệ thống.  
2. Danh mục sản phẩm (FE-03) và Thương hiệu (UC-177) đã được tạo sẵn trên hệ thống.  

**Normal Flow**  
*Tạo mới hồ sơ thông tin chung cho thiết bị vệ sinh*  
1. Chủ cửa hàng nhấp vào nút "Thêm sản phẩm mới" (Create Product).  
2. Hệ thống hiển thị biểu mẫu tạo sản phẩm:  
   - Tên sản phẩm (ví dụ: "Bồn cầu thông minh TOTO Neorest").  
   - Mã sản phẩm chung (Product Code, ví dụ: `PROD-TOTO-NEOREST`).  
   - Chọn Danh mục sản phẩm (Dropdown phân cấp từ UC-019).  
   - Chọn Thương hiệu / Nhà sản xuất (TOTO, Inax, Caesar, Viglacera...).  
   - Chọn Đơn vị tính cơ bản (Bộ, Cái...).  
   - Mô tả sản phẩm (Trình soạn thảo Rich Text: tính năng men chống bám bẩn CeFiONtect, xả xoáy Tornado, sấy sưởi ấm...).  
   - Thông số kỹ thuật chung (Tâm xả chuẩn, áp lực nước yêu cầu, vật liệu men sứ, xuất xứ).  
   - Ảnh đại diện sản phẩm và Album ảnh chi tiết.  
3. Chủ cửa hàng nhập các thông tin và tải lên các tệp ảnh sản phẩm.  
4. Chủ cửa hàng nhấp nút "Tiếp tục tạo biến thể" hoặc "Lưu sản phẩm".  
5. Hệ thống kiểm tra dữ liệu:  
   - Tên sản phẩm không để trống (từ 5 đến 200 ký tự).  
   - Mã sản phẩm là duy nhất trong hệ thống.  
   - Bắt buộc chọn Danh mục và Thương hiệu hợp lệ.  
6. Hệ thống gọi Secondary Actor `Cloudinary` tải các tệp ảnh lên lưu trữ đám mây.  
7. Hệ thống lưu bản ghi sản phẩm vào cơ sở dữ liệu ở trạng thái `Bản nháp (Draft)` hoặc `Hoạt động (Active)`.  
8. Hệ thống ghi nhận sự kiện vào Audit Log.  
9. Hệ thống hiển thị thông báo thành công (MSG135: "Tạo sản phẩm mới thành công! Hãy tiếp tục khai báo các biến thể sản phẩm") và chuyển hướng sang màn hình Tạo biến thể (UC-022).  

**Alternative Flows**  
- **Mã sản phẩm hoặc Tên sản phẩm bị trùng lặp:**  
  1. Mã sản phẩm đã tồn tại trong cơ sở dữ liệu.  
  2. Hệ thống báo lỗi (MSG136: "Mã sản phẩm đã tồn tại. Vui lòng nhập mã sản phẩm khác").  
- **Bỏ trống các trường bắt buộc:**  
  1. Bỏ trống Tên hoặc chưa chọn Danh mục.  
  2. Hệ thống đánh dấu đỏ tại các ô thiếu dữ liệu (MSG02).  

**Postconditions**  
Sản phẩm cha được tạo lập thành công và sẵn sàng để định nghĩa các biến thể chi tiết (SKU).  

---

### UC-021 Cập nhật sản phẩm (Update Product)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to edit general product details (name, category, description, technical specs, uploaded media) so that product catalog info remains up to date.  

**Preconditions**  
Chủ cửa hàng đã mở trang chi tiết sản phẩm nội bộ (UC-132) và nhấp nút "Chỉnh sửa sản phẩm".  

**Normal Flow**  
*Cập nhật thông tin thông số và mô tả sản phẩm*  
1. Hệ thống hiển thị biểu mẫu chỉnh sửa chứa các dữ liệu hiện có của sản phẩm.  
2. Chủ cửa hàng chỉnh sửa các trường cần thay đổi: Tên sản phẩm, Danh mục, Mô tả tính năng, Thông số kỹ thuật (ví dụ: bổ sung kích thước tâm xả 305mm), thêm hoặc xóa bớt ảnh.  
3. Chủ cửa hàng nhấp nút "Lưu thay đổi".  
4. Hệ thống kiểm tra dữ liệu hợp lệ.  
5. (Nếu có tải thêm ảnh mới): Hệ thống gọi `Cloudinary` tải ảnh lên máy chủ lưu trữ.  
6. Hệ thống cập nhật bản ghi sản phẩm trong cơ sở dữ liệu.  
7. Hệ thống ghi nhận chi tiết các trường thay đổi vào Audit Log (FE-02).  
8. Hệ thống hiển thị thông báo thành công (MSG137: "Cập nhật sản phẩm thành công!") và tải lại giao diện chi tiết sản phẩm.  

**Alternative Flows**  
- **Đổi danh mục sản phẩm:**  
  1. Chủ cửa hàng chuyển sản phẩm sang danh mục mới.  
  2. Hệ thống hiển thị cảnh báo xác nhận việc di chuyển sản phẩm trên cây phân cấp điều hướng website.  

**Postconditions**  
Thông tin chung của sản phẩm được cập nhật chính xác trên toàn hệ thống.  

---

### UC-022 Tạo biến thể sản phẩm (Create Product Variant)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to create product variants (distinguished by color, size, bowl shape, remote type) with dedicated SKU, list price, warranty policy, and serial tracking configuration so that each sellable unit is accurately tracked.  

**Preconditions**  
1. Đã có sản phẩm chung (UC-020).  
2. Chủ cửa hàng đang ở tab "Biến thể sản phẩm".  

**Normal Flow**  
*Tạo biến thể hàng hóa có mã SKU riêng biệt*  
1. Chủ cửa hàng nhấp nút "Thêm biến thể mới" (Add Variant).  
2. Hệ thống hiển thị biểu mẫu tạo biến thể:  
   - Thuộc tính biến thể: Màu sắc (Trắng, Đen mờ, Bạc kim loại), Kích thước/Mẫu mã (Dài x Rộng x Cao), Kiểu xả, v.v.  
   - Tên biến thể (Hệ thống tự động ghép: [Tên SP] - [Màu] - [Kích thước]).  
   - Mã SKU: Nhập thủ công hoặc bấm nút "Tự động sinh mã SKU" (UC-173, ví dụ: `TOTO-CS885-WH-300`).  
   - Mã vạch / Barcode (EAN-13 nếu có).  
   - Giá niêm yết (List Price): Giá bán lẻ (VNĐ).  
   - Giá nhập dự kiến (Cost Price): Phục vụ tính lãi gộp.  
   - Áp dụng Chính sách bảo hành (Dropdown chọn từ UC-198).  
   - Checkbox "Bật quản lý theo dõi mã Serial" (UC-030).  
   - Ảnh riêng của biến thể (nếu màu sắc khác biệt).  
3. Chủ cửa hàng nhập các thông số biến thể và giá bán.  
4. Chủ cửa hàng nhấp nút "Lưu biến thể".  
5. Hệ thống kiểm tra:  
   - Mã SKU là duy nhất trên toàn hệ thống (không trùng với bất kỳ SKU nào khác).  
   - Giá niêm yết > 0 và Giá niêm yết >= Giá nhập.  
6. Hệ thống lưu bản ghi biến thể mới vào bảng `ProductVariants`.  
7. Hệ thống tự động khởi tạo bản ghi tồn kho ban đầu cho biến thể với Số lượng = 0.  
8. Hệ thống ghi vết vào Audit Log.  
9. Hệ thống hiển thị thông báo thành công (MSG138: "Tạo biến thể sản phẩm thành công!") và đưa biến thể vào danh sách quản lý.  

**Alternative Flows**  
- **Mã SKU bị trùng lặp:**  
  1. Mã SKU nhập vào đã thuộc về một biến thể khác.  
  2. Hệ thống báo lỗi (MSG139: "Mã SKU này đã tồn tại trên hệ thống. Vui lòng kiểm tra lại").  
- **Giá bán thấp hơn giá vốn:**  
  1. Chủ cửa hàng nhập giá bán lẻ nhỏ hơn giá nhập dự kiến.  
  2. Hệ thống hiển thị cảnh báo màu vàng (MSG140: "Cảnh báo: Giá bán niêm yết đang thấp hơn giá vốn nhập hàng! Bạn có chắc chắn muốn lưu?").  

**Postconditions**  
Biến thể sản phẩm mới được tạo lập với SKU riêng và sẵn sàng cho các nghiệp vụ nhập kho (FE-07) và bán hàng.  

---

### UC-023 Cập nhật biến thể sản phẩm (Update Product Variant)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to edit variant specifications, assigned image, barcode, and warranty policy so that specific item details remain correct.  

**Preconditions**  
Chủ cửa hàng mở danh sách biến thể của sản phẩm (UC-170) và nhấp nút "Sửa biến thể".  

**Normal Flow**  
*Chỉnh sửa thuộc tính và thông tin của một biến thể sản phẩm*  
1. Hệ thống hiển thị biểu mẫu chỉnh sửa thông tin biến thể.  
2. Chủ cửa hàng chỉnh sửa: Tên biến thể, Thuộc tính màu sắc/kích thước, Thay đổi ảnh biến thể, Cập nhật chính sách bảo hành, hoặc Mã vạch.  
3. Chủ cửa hàng nhấp nút "Lưu thay đổi".  
4. Hệ thống kiểm tra tính hợp lệ của dữ liệu.  
5. Hệ thống cập nhật bản ghi biến thể trong cơ sở dữ liệu.  
6. Hệ thống ghi nhận chi tiết thay đổi vào Audit Log.  
7. Hệ thống hiển thị thông báo thành công (MSG141: "Cập nhật biến thể sản phẩm thành công!").  

**Alternative Flows**  
- **Biến thể đã phát sinh giao dịch xuất nhập kho:**  
  1. Mã SKU đã liên kết với hóa đơn và phiếu kho cũ.  
  2. Hệ thống khóa trường Mã SKU (Read-only) để bảo toàn tính toàn vẹn dữ liệu kế toán và kho, chỉ cho phép sửa các thông tin mô tả, ảnh và chính sách bảo hành.  

**Postconditions**  
Dữ liệu biến thể được lưu cập nhật vào cơ sở dữ liệu.  

---

### UC-024 Điều chỉnh giá biến thể (Update Variant Price)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to adjust the listed price of a product variant with mandatory reason logging so that price changes immediately apply to the catalog and are permanently audited.  

**Preconditions**  
1. Người dùng đã đăng nhập với vai trò `Store Owner`.  
2. Biến thể sản phẩm mục tiêu đang tồn tại trên hệ thống.  

**Normal Flow**  
*Thay đổi giá niêm yết của biến thể và ghi vết kiểm toán*  
1. Tại danh sách biến thể hoặc chi tiết sản phẩm, Chủ cửa hàng nhấp vào nút "Đổi giá" (Update Price) hoặc nhấp đúp vào ô Giá niêm yết.  
2. Hệ thống mở hộp thoại "Điều chỉnh giá bán biến thể - [Mã SKU]":  
   - Tên biến thể và Mã SKU.  
   - Giá niêm yết hiện tại (ví dụ: `15.500.000 VNĐ`).  
   - Giá vốn nhập hàng tham chiếu (ví dụ: `11.000.000 VNĐ`).  
   - Ô nhập "Giá niêm yết mới (VNĐ)" (bắt buộc).  
   - Ô nhập "Lý do điều chỉnh giá" (bắt buộc, tối thiểu 10 ký tự, ví dụ: "Áp dụng bảng giá mới của hãng TOTO quý 4").  
3. Chủ cửa hàng nhập giá mới và nhập lý do điều chỉnh.  
4. Chủ cửa hàng nhấp nút "Xác nhận đổi giá".  
5. Hệ thống kiểm tra: Giá mới > 0, khác giá cũ, và Lý do không để trống.  
6. Hệ thống cập nhật `Price` mới cho biến thể trong cơ sở dữ liệu.  
7. Hệ thống tự động kích hoạt UC-016 (Record Audit Log): Ghi nhận chi tiết `OldPrice`, `NewPrice`, `OperatorID`, `Timestamp` và `Reason` vào bảng Audit Log.  
8. Giá mới có hiệu lực ngay lập tức trên website và phần mềm bán hàng tại quầy.  
9. Hệ thống hiển thị thông báo thành công (MSG142: "Cập nhật giá bán biến thể thành công! Sự kiện đã được lưu vào nhật ký kiểm toán").  

**Alternative Flows**  
- **Không nhập lý do đổi giá:**  
  1. Chủ cửa hàng bỏ trống ô lý do.  
  2. Hệ thống chặn lưu và hiển thị cảnh báo (MSG143: "Bắt buộc nhập lý do thay đổi giá để phục vụ kiểm toán tài chính").  
- **Giá mới thấp hơn giá vốn quá mức (> 10%):**  
  1. Giá bán mới thấp hơn giá vốn nhập vào.  
  2. Hệ thống hiển thị hộp thoại cảnh báo nguy cơ bán lỗ và yêu cầu xác nhận 2 lớp trước khi ghi nhận.  

**Postconditions**  
1. Giá bán mới của biến thể được cập nhật trong toàn hệ thống.  
2. Bản ghi thay đổi giá được lưu vĩnh viễn trong Audit Log.  

---

### UC-025 Bật/Tắt trạng thái sản phẩm (Activate/Deactivate Product)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to toggle the active/inactive status of a product so that discontinued models are hidden from the public website while retaining past transactional data.  

**Preconditions**  
Chủ cửa hàng đang ở màn hình danh sách sản phẩm.  

**Normal Flow**  
*Ẩn hoặc hiển thị lại sản phẩm trên hệ thống*  
1. Tại dòng sản phẩm cần thao tác, Chủ cửa hàng gạt công tắc Trạng thái (`Active` / `Inactive`).  
2. Hệ thống hiển thị hộp thoại xác nhận:  
   - "Bạn có chắc chắn muốn ngừng kinh doanh / ẩn sản phẩm [Tên sản phẩm] khỏi website?".  
   - Cảnh báo: "Khi ẩn sản phẩm, toàn bộ các biến thể thuộc sản phẩm này cũng sẽ không hiển thị trên website công khai, nhưng dữ liệu đơn hàng cũ vẫn được bảo lưu".  
3. Chủ cửa hàng nhấp "Xác nhận ẩn".  
4. Hệ thống cập nhật trạng thái `IsActive = False` cho sản phẩm và tất cả các biến thể trực thuộc.  
5. Hệ thống xóa sản phẩm khỏi chỉ mục tìm kiếm công khai và bộ nhớ đệm (Cache Invalidation).  
6. Hệ thống ghi nhận sự kiện vào Audit Log.  
7. Hệ thống hiển thị thông báo thành công (MSG144: "Đã tắt trạng thái hiển thị của sản phẩm!").  

**Alternative Flows**  
- **Bật lại sản phẩm đã ngừng kinh doanh:**  
  1. Gạt công tắc từ Inactive sang Active.  
  2. Hệ thống kích hoạt lại sản phẩm và đưa sản phẩm hiển thị lại trên website mua hàng.  

**Postconditions**  
Trạng thái hiển thị của sản phẩm được đồng bộ trên toàn bộ các kênh bán hàng.  

---

### UC-026 Xem danh sách sản phẩm nội bộ (View Product List)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As an internal staff member, I want to view a comprehensive internal table of products and variants (including stock on hand, reserved stock, cost price, list price, and status) so that I can manage operations and sales.  

**Preconditions**  
Nhân viên đã đăng nhập vào hệ thống quản lý bán hàng/kho.  

**Normal Flow**  
*Xem danh mục sản phẩm nội bộ đầy đủ số liệu vận hành*  
1. Người dùng chọn mục "Danh sách sản phẩm" trên thanh điều hướng quản trị.  
2. Hệ thống hiển thị bảng danh sách sản phẩm nội bộ:  
   - Thanh công cụ: Tìm kiếm, Lọc danh mục, Lọc thương hiệu, Lọc tồn kho (Còn hàng / Hết hàng / Sắp hết), Lọc trạng thái.  
   - Bảng dữ liệu sản phẩm: Ảnh thu nhỏ, Mã sản phẩm, Tên thiết bị, Danh mục, Thương hiệu, Số lượng biến thể, Khoảng giá bán niêm yết, Tổng tồn thực tế, Tổng tồn khả dụng, Trạng thái hoạt động, Cột Thao tác (Xem chi tiết, Sửa, Đổi giá, Bật/Tắt).  
   - Hỗ trợ mở rộng dòng (Expand row): Nhấp vào dấu `+` để xem nhanh toàn bộ các biến thể và tồn kho của từng SKU trực thuộc sản phẩm đó.  
3. Người dùng duyệt danh sách, tìm kiếm và thao tác các nghiệp vụ liên quan.  

**Alternative Flows**  
- **Danh sách rỗng:**  
  1. Chưa có sản phẩm nào được nhập.  
  2. Hệ thống hiển thị bảng trống kèm nút hướng dẫn "Tạo sản phẩm mới" (UC-020) hoặc "Nhập từ file Excel" (UC-174).  

**Postconditions**  
Toàn bộ danh mục sản phẩm và trạng thái vận hành được hiển thị chính xác.  

---

### UC-027 Duyệt danh sách sản phẩm (Browse Products)
**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
None  

**Description**  
As a Customer or Guest, I want to browse products organized by categories (e.g., smart bidet toilets, shower mixers, bathtubs) on the website catalog so that I can explore the store's sanitary ware offerings.  

**Preconditions**  
Người dùng truy cập vào website của cửa hàng.  

**Normal Flow**  
*Khách hàng duyệt xem danh mục sản phẩm trên giao diện người dùng*  
1. Người dùng di chuột vào menu danh mục sản phẩm trên thanh điều hướng (ví dụ: Danh mục "Sen tắm & Vòi").  
2. Hệ thống hiển thị Menu thả xuống (Mega Menu) thể hiện các nhóm danh mục con (Sen cây nhiệt độ, Sen âm tường, Vòi chậu nóng lạnh...).  
3. Người dùng nhấp chọn nhóm danh mục "Sen cây nhiệt độ".  
4. Hệ thống điều hướng đến trang danh mục sản phẩm tương ứng:  
   - Tiêu đề danh mục, Banner giới thiệu và Đường dẫn breadcrumb (`Trang chủ > Sen tắm & Vòi > Sen cây nhiệt độ`).  
   - Lưới hiển thị sản phẩm (Product Grid): Mỗi sản phẩm hiển thị Ảnh sản phẩm sắc nét, Tên thiết bị, Thương hiệu, Giá niêm yết (kèm giá khuyến mãi nếu có), Nhãn "Mới" / "Bán chạy", Huy hiệu trạng thái còn hàng.  
   - Tùy chọn sắp xếp: `Mới nhất`, `Giá từ thấp đến cao`, `Giá từ cao đến thấp`, `Bán chạy nhất`.  
   - Thanh phân trang dữ liệu (Pagination).  
5. Người dùng cuộn trang xem danh sách sản phẩm và nhấp chọn sản phẩm quan tâm để xem chi tiết (UC-029).  

**Alternative Flows**  
- **Danh mục chưa có sản phẩm nào:**  
  1. Danh mục mới chưa được gán sản phẩm đang kích hoạt.  
  2. Hệ thống hiển thị thông báo: "Chưa có sản phẩm nào trong danh mục này. Quý khách vui lòng tham khảo các danh mục khác!".  

**Postconditions**  
Danh mục sản phẩm được trình bày trực quan, hấp dẫn và mượt mà trên website.  

---

### UC-028 Tìm kiếm và lọc sản phẩm (Search and Filter Products)
**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
None  

**Description**  
As a Customer or Guest, I want to search and filter products by keywords, categories, price range, brand, and technical attributes (rough-in size, bowl type, water pressure) so that I can quickly find the exact fixtures I need.  

**Preconditions**  
Người dùng đang ở trên trang danh mục sản phẩm hoặc trang tìm kiếm của website.  

**Normal Flow**  
*Tìm kiếm bằng từ khóa và áp dụng đa bộ lọc sản phẩm*  
1. Người dùng nhập từ khóa vào ô tìm kiếm chính trên Header (ví dụ: "bồn cầu 1 khối TOTO") hoặc sử dụng bộ lọc bên sườn trái (Sidebar Filters):  
   - Bộ lọc Thương hiệu: TOTO, INAX, Caesar, Viglacera.  
   - Bộ lọc Khoảng giá: Dưới 5 triệu, 5-10 triệu, 10-20 triệu, Trên 20 triệu (hoặc thanh trượt khoảng giá tùy chỉnh).  
   - Bộ lọc Thuộc tính kỹ thuật chuyên dụng:  
     + Tâm xả bồn cầu (Rough-in): 300mm, 305mm, 400mm.  
     + Kiểu xả: Xả xoáy Tornado, Xả thẳng, Xả 2 nút nhấn tiết kiệm nước.  
     + Chất liệu men: Men Nano nung chống bám bẩn.  
2. Người dùng tích chọn các tiêu chí lọc.  
3. Hệ thống gửi truy vấn AJAX/API đối soát tức thì các điều kiện (Multi-faceted Search).  
4. Hệ thống cập nhật lại danh sách sản phẩm trên lưới mà không tải lại toàn bộ trang:  
   - Hiển thị số lượng kết quả tìm thấy (ví dụ: "Tìm thấy 12 sản phẩm phù hợp").  
   - Hiển thị các thẻ chip bộ lọc đang áp dụng kèm nút "Xóa tất cả bộ lọc".  
5. Người dùng xem các sản phẩm phù hợp.  

**Alternative Flows**  
- **Không tìm thấy sản phẩm nào phù hợp:**  
  1. Bộ lọc quá hẹp hoặc từ khóa không khớp.  
  2. Hệ thống hiển thị thông báo (MSG145: "Không tìm thấy sản phẩm nào phù hợp với bộ lọc đã chọn") kèm danh sách gợi ý "Có thể bạn quan tâm" và nút "Đặt lại bộ lọc".  

**Postconditions**  
Danh sách sản phẩm được thu hẹp chính xác theo đúng nhu cầu kỹ thuật và ngân sách của khách.  

---

### UC-029 Xem chi tiết sản phẩm (View Product Detail)
**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
None  

**Description**  
As a Customer or Guest, I want to view comprehensive product information (high-res image gallery, variant selectors, technical blueprint, installation dimensions, warranty terms, real-time stock) so that I can make an informed purchasing decision.  

**Preconditions**  
Người dùng nhấp vào một sản phẩm từ danh sách sản phẩm (UC-027, UC-028) hoặc từ gợi ý AI (UC-032).  

**Normal Flow**  
*Xem đầy đủ chi tiết sản phẩm trên giao diện người dùng*  
1. Hệ thống điều hướng người dùng đến trang chi tiết sản phẩm: `https://store.com/san-pham/[Slug-Ten-San-Pham]`.  
2. Hệ thống hiển thị giao diện chi tiết sản phẩm gồm các khối thông tin chuẩn:  
   - Khối Thư viện ảnh (Image Gallery): Ảnh đại diện lớn có tính năng phóng to (Zoom in), các ảnh thu nhỏ các góc chụp, ảnh bản vẽ kỹ thuật kích thước lắp đặt.  
   - Khối Thông tin mua hàng: Tên sản phẩm, Thương hiệu, Đánh giá sao, Giá niêm yết & Giá bán, Bộ chọn biến thể (Màu sắc, Mẫu mã, Tâm xả), Bộ chọn số lượng, Trạng thái tồn kho ("Còn hàng" hoặc "Hết hàng"), Nút "Thêm vào giỏ hàng" và Nút "Mua ngay".  
   - Khối Cam kết dịch vụ: Huy hiệu bảo hành chính hãng [X] năm, Miễn phí vận chuyển nội thành, Hỗ trợ lắp đặt tại nhà.  
   - Tab 1: "Mô tả chi tiết & Tính năng nổi bật" (Nội dung văn bản, video giới thiệu công nghệ).  
   - Tab 2: "Thông số kỹ thuật" (Bảng thông số chi tiết: Kích thước dài x rộng x cao, Tâm xả, Áp lực nước cấp, Lượng nước xả, Công nghệ men sứ, Xuất xứ).  
   - Tab 3: "Chính sách bảo hành & Đổi trả".  
   - Tab 4: "Đánh giá từ khách hàng đã mua".  
   - Khối "Sản phẩm thường mua cùng" (Gợi ý combo sen vòi, bồn cầu đồng bộ).  
3. Người dùng nhấp chọn giữa các biến thể khác nhau: Hệ thống tự động cập nhật lại Giá bán, Ảnh biến thể và Tồn kho khả dụng theo đúng biến thể được chọn.  

**Alternative Flows**  
- **Sản phẩm đã bị ngừng kinh doanh (Inactive):**  
  1. Người dùng truy cập bằng đường link cũ đã lưu.  
  2. Hệ thống hiển thị thông báo "Sản phẩm này hiện đã ngừng kinh doanh" và gợi ý các sản phẩm thế hệ mới tương đương.  

**Postconditions**  
Người dùng nắm bắt đầy đủ thông tin kỹ thuật và quyết định thêm vào giỏ hàng hoặc gửi yêu cầu tư vấn.  

---

### UC-030 Cấu hình quản lý Serial cho biến thể (Configure Serial Tracking)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to toggle serial number tracking for high-value or complex variants (smart toilets, digital showers, electronic faucets) so that the system strictly enforces serial entry during goods receiving and dispatching.  

**Preconditions**  
Chủ cửa hàng đang ở màn hình tạo mới hoặc chỉnh sửa biến thể sản phẩm (UC-022, UC-023).  

**Normal Flow**  
*Bật cấu hình quản lý theo dõi số Serial cho biến thể*  
1. Tại biểu mẫu cấu hình biến thể, Chủ cửa hàng cuộn đến mục "Quản lý mã định danh Serial".  
2. Chủ cửa hàng tích chọn vào ô "Bắt buộc quản lý theo dõi số Serial (Serial Tracking Required)".  
3. Hệ thống hiển thị ghi chú nghiệp vụ: "Khi bật tính năng này, hệ thống sẽ bắt buộc nhập/quét mã Serial duy nhất của từng sản phẩm khi Nhập kho (FE-07), Xuất kho (FE-08) và Kích hoạt bảo hành (FE-09)".  
4. Chủ cửa hàng có thể chọn thêm: Cấu hình quy tắc tiền tố Serial (Prefix) để kiểm tra hợp lệ khi quét mã (ví dụ: Serial phải bắt đầu bằng `SN-TOTO-`).  
5. Chủ cửa hàng nhấp nút "Lưu cấu hình".  
6. Hệ thống cập nhật cờ `IsSerialTracked = True` cho biến thể trong cơ sở dữ liệu.  
7. Hệ thống ghi nhận sự kiện vào Audit Log.  
8. Hệ thống hiển thị thông báo thành công (MSG146: "Đã bật cấu hình quản lý Serial cho biến thể thành công!").  

**Alternative Flows**  
- **Tắt quản lý Serial khi biến thể đã có hàng tồn trong kho:**  
  1. Biến thể đang có số lượng tồn kho và các mã Serial đang lưu trữ.  
  2. Hệ thống cảnh báo: "Biến thể đang có tồn kho theo Serial. Tắt quản lý Serial sẽ chuyển sang theo dõi theo số lượng thông thường. Bạn có chắc chắn?".  
  3. Chủ cửa hàng xác nhận -> Hệ thống mới thực hiện chuyển đổi.  

**Postconditions**  
Biến thể được gắn cờ quản lý Serial, các quy trình xuất nhập kho kế tiếp bắt buộc quét mã Serial.  

---

### UC-132 Xem chi tiết & thông số kỹ thuật sản phẩm nội bộ (View Internal Product Detail)
**Primary Actors**  
Sales Staff, Store Owner, Technical Staff  

**Secondary Actors**  
None  

**Description**  
As an internal user, I want to view comprehensive internal product details (rough-in dimensions, water pressure requirements, installation diagrams, stock across warehouses, supplier cost) so that I can provide precise technical advice to customers.  

**Preconditions**  
Nhân viên đã đăng nhập vào hệ thống quản lý bán hàng.  

**Normal Flow**  
*Tra cứu thông số kỹ thuật lắp đặt chuyên sâu phục vụ tư vấn công trình*  
1. Nhân viên tìm kiếm và mở sản phẩm từ Danh sách sản phẩm nội bộ (UC-026).  
2. Hệ thống hiển thị trang "Chi tiết kỹ thuật sản phẩm nội bộ":  
   - Bảng thông số kỹ thuật chuẩn công trình:  
     + Tâm xả (mm), Đường kính ống thoát thải yêu cầu (Phi 90 hay Phi 110).  
     + Áp lực nước cấp tối thiểu (MPa) và tối đa; khuyến nghị có cần dùng bơm tăng áp hay không.  
     + Nguồn điện cấp (đối với bồn cầu điện tử: ổ cắm 220V chống giật cách sàn bao nhiêu cm).  
     + Kích thước đóng gói và trọng lượng nguyên đai nguyên kiện (để tính toán vận chuyển và bốc vác).  
   - Bản vẽ kỹ thuật mặt cắt 2D/3D (File CAD/PDF đính kèm để xem sơ đồ đi ống âm tường).  
   - Bảng phân bổ tồn kho thời gian thực: Tồn thực tế tại kho chính, Số lượng đang giữ cho các đơn khác, Số lượng có thể bán ngay.  
   - Giá vốn nhập hàng và Biên lợi nhuận hiện tại (chỉ hiển thị với quyền được phép xem giá vốn).  
3. Nhân viên sử dụng các thông số kỹ thuật để giải đáp chính xác thắc mắc của thợ điện nước hoặc chủ nhà.  

**Alternative Flows**  
- **Tải tệp bản vẽ kỹ thuật PDF:**  
  1. Nhân viên nhấp vào liên kết "Tải bản vẽ hướng dẫn lắp đặt PDF".  
  2. Hệ thống mở file hướng dẫn để nhân viên in hoặc gửi qua Zalo cho khách hàng/thợ công trình.  

**Postconditions**  
Thông số kỹ thuật chính xác được cung cấp nhanh chóng, tránh sai sót trong lắp đặt thực tế.  

---

### UC-143 Xem và gợi ý Combo thiết bị vệ sinh (View & Suggest Product Bundles)
**Primary Actors**  
Sales Staff, Customer, Guest  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Customer, I want to view synchronized bathroom package bundles (e.g., Toilet + Basin + Shower + Mirror package) with discounted bundle pricing so that complete bathrooms can be purchased with matching aesthetic style and cost savings.  

**Preconditions**  
Cửa hàng đã tạo các gói combo thiết bị vệ sinh đồng bộ trong hệ thống.  

**Normal Flow**  
*Khám phá và chọn mua combo thiết bị vệ sinh trọn gói cho phòng tắm*  
1. Người dùng chọn mục "Combo phòng tắm đồng bộ" (Product Bundles) trên menu.  
2. Hệ thống hiển thị danh sách các gói combo thiết bị:  
   - `Combo Tiết Kiệm (Dành cho nhà trọ / căn hộ cho thuê)`: Gồm bồn cầu 2 khối, chậu treo tường, sen tắm lạnh, phụ kiện 6 món. Giá trọn gói 4.990.000 VNĐ.  
   - `Combo Hiện Đại (Dành cho nhà phố)`: Gồm bồn cầu 1 khối nắp êm, chậu bàn đá lavabo, sen cây nóng lạnh, gương led cảm ứng. Giá trọn gói 12.500.000 VNĐ (Tiết kiệm 15% so với mua lẻ).  
   - `Combo Cao Cấp Master (Dành cho biệt thự)`: Gồm bồn cầu thông minh tự động, bồn tắm nằm massage, sen cây âm tường nhiệt độ.  
3. Người dùng nhấp vào một combo để xem chi tiết: Danh sách từng thiết bị thành phần, Mã biến thể, Đơn giá mua lẻ từng món, Tổng giá mua lẻ và Giá bán combo ưu đãi.  
4. Người dùng có thể tùy chọn đổi màu hoặc nâng cấp một món thành phần trong combo (nếu cấu hình cho phép).  
5. Người dùng nhấp nút "Thêm toàn bộ combo vào giỏ hàng" hoặc "Tạo đơn hàng combo tại quầy".  
6. Hệ thống thêm đồng thời tất cả các mặt hàng thành phần với mức giá chiết khấu combo vào giỏ hàng / đơn hàng.  
7. Hệ thống hiển thị thông báo thành công (MSG147: "Đã thêm toàn bộ thiết bị trong combo vào đơn hàng thành công!").  

**Alternative Flows**  
- **Một mặt hàng trong combo bị hết hàng:**  
  1. Một thiết bị thành phần trong combo có tồn kho bằng 0.  
  2. Hệ thống hiển thị cảnh báo: "Thiết bị [Tên món] trong combo hiện đang tạm hết hàng" và gợi ý thiết bị thay thế có giá trị tương đương để giữ nguyên giá combo.  

**Postconditions**  
Gói combo thiết bị phòng tắm được đưa vào giỏ hàng hoặc đơn bán hàng với chính sách giá ưu đãi đồng bộ.  

---

### UC-163 Tìm kiếm và xem danh mục sản phẩm (Search and View Categories)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to search and view existing product categories by name, code, or hierarchy level so that I can quickly understand the catalog structure.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý.  

**Normal Flow**  
*Tìm kiếm và lọc danh mục sản phẩm*  
1. Người dùng mở trang Danh mục sản phẩm và nhập từ khóa vào ô tìm kiếm (ví dụ: "vòi", "sen", "bồn cầu").  
2. Hệ thống lọc tức thì danh sách các danh mục có Tên hoặc Mã danh mục chứa từ khóa.  
3. Hệ thống hiển thị các nhánh kết quả phù hợp kèm đường dẫn đầy đủ từ danh mục gốc đến danh mục con.  
4. Người dùng nhấp vào danh mục để xem thông tin tóm tắt và số lượng sản phẩm liên kết.  

**Alternative Flows**  
- **Không tìm thấy danh mục:**  
  1. Từ khóa không khớp. Hệ thống báo không tìm thấy kết quả.  

**Postconditions**  
Danh mục cần tìm được làm nổi bật trên cây cấu trúc.  

---

### UC-164 Tạo danh mục sản phẩm (Create Product Category)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to create a new product category with code, name, parent category, icon, and SEO description so that new product lines can be classified.  

**Preconditions**  
Chủ cửa hàng đang ở màn hình Quản lý danh mục sản phẩm.  

**Normal Flow**  
*Tạo mới danh mục sản phẩm trong hệ thống*  
1. Chủ cửa hàng nhấp nút "Thêm danh mục mới" (New Category).  
2. Hệ thống hiển thị biểu mẫu:  
   - Tên danh mục (ví dụ: "Bồn tiểu nam cảm ứng").  
   - Mã danh mục (ví dụ: `CAT-TIEU-NAM`).  
   - Chọn Danh mục cha (Dropdown: chọn danh mục cấp trên hoặc để trống nếu là danh mục gốc).  
   - Tải lên ảnh biểu tượng / Icon danh mục.  
   - Thứ tự hiển thị (Số nguyên).  
   - Mô tả danh mục & Tiêu đề SEO Meta.  
3. Chủ cửa hàng nhập thông tin và nhấp nút "Lưu danh mục".  
4. Hệ thống kiểm tra: Tên và Mã danh mục không để trống, Mã danh mục là duy nhất.  
5. (Nếu có ảnh): Hệ thống gọi `Cloudinary` tải ảnh lên lưu trữ.  
6. Hệ thống lưu danh mục mới vào cơ sở dữ liệu.  
7. Hệ thống ghi nhận sự kiện vào Audit Log.  
8. Hệ thống hiển thị thông báo thành công (MSG148: "Tạo danh mục sản phẩm mới thành công!") và làm mới cây danh mục.  

**Alternative Flows**  
- **Mã danh mục bị trùng lặp:**  
  1. Mã danh mục đã có. Hệ thống báo lỗi (MSG149: "Mã danh mục này đã tồn tại").  

**Postconditions**  
Danh mục mới xuất hiện trên hệ thống và sẵn sàng để gán cho sản phẩm.  

---

### UC-165 Xóa mềm danh mục sản phẩm (Soft Delete Product Category)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to soft-delete an unused category after verifying it has no active subcategories or products so that historical records remain intact.  

**Preconditions**  
Chủ cửa hàng chọn một danh mục không còn sử dụng để xóa.  

**Normal Flow**  
*Xóa mềm danh mục sản phẩm an toàn*  
1. Chủ cửa hàng nhấp vào biểu tượng "Xóa" tại danh mục cần gỡ bỏ.  
2. Hệ thống kiểm tra ràng buộc toàn vẹn dữ liệu:  
   - Kiểm tra xem danh mục có danh mục con trực thuộc hay không.  
   - Kiểm tra xem có sản phẩm nào đang thuộc danh mục này hay không.  
3. Nếu không có danh mục con và không có sản phẩm liên kết: Hệ thống hiển thị hộp thoại xác nhận: "Bạn có chắc chắn muốn xóa danh mục [Tên danh mục]?".  
4. Chủ cửa hàng nhấp "Xác nhận xóa".  
5. Hệ thống thực hiện đánh dấu xóa mềm (`IsDeleted = True`) danh mục trong cơ sở dữ liệu.  
6. Hệ thống ghi nhận thao tác vào Audit Log.  
7. Hệ thống hiển thị thông báo thành công (MSG150: "Đã xóa danh mục thành công!") và loại bỏ nhánh khỏi cây danh mục.  

**Alternative Flows**  
- **Danh mục đang chứa sản phẩm hoặc danh mục con:**  
  1. Hệ thống phát hiện có [X] sản phẩm đang thuộc danh mục này.  
  2. Hệ thống chặn xóa và hiển thị thông báo lỗi (MSG151: "Không thể xóa danh mục này vì đang chứa [X] sản phẩm. Vui lòng chuyển các sản phẩm sang danh mục khác trước khi xóa").  

**Postconditions**  
Danh mục bị ẩn hoàn toàn khỏi giao diện người dùng và quản trị nhưng dữ liệu lịch sử được bảo toàn.  

---

### UC-166 Cập nhật danh mục sản phẩm (Update Product Category)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to edit category name, parent association, description, or icon so that category data remains accurate.  

**Preconditions**  
Chủ cửa hàng mở biểu mẫu chỉnh sửa một danh mục hiện có.  

**Normal Flow**  
*Chỉnh sửa thông tin danh mục sản phẩm*  
1. Chủ cửa hàng nhấp nút "Sửa" tại dòng danh mục mong muốn.  
2. Hệ thống hiển thị biểu mẫu chỉnh sửa với các dữ liệu hiện có.  
3. Chủ cửa hàng cập nhật: Tên danh mục, Thay đổi danh mục cha, Cập nhật ảnh icon, Thứ tự hiển thị.  
4. Chủ cửa hàng nhấp nút "Lưu thay đổi".  
5. Hệ thống kiểm tra: Danh mục cha mới không được là chính danh mục này hoặc là danh mục con của chính nó (ngăn chặn vòng lặp Circular Reference).  
6. Hệ thống lưu cập nhật vào cơ sở dữ liệu.  
7. Hệ thống ghi nhật ký vào Audit Log.  
8. Hệ thống hiển thị thông báo thành công (MSG152: "Cập nhật danh mục sản phẩm thành công!").  

**Alternative Flows**  
- **Chọn danh mục cha không hợp lệ (Vòng lặp cây):**  
  1. Người dùng chọn danh mục cha là một nhánh con của chính danh mục đang sửa.  
  2. Hệ thống báo lỗi (MSG153: "Không thể chọn danh mục con làm danh mục cha của chính nó").  

**Postconditions**  
Thông tin danh mục và quan hệ phân cấp cây được cập nhật chính xác.  

---

### UC-167 Xem danh mục sản phẩm (View Product Category)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to view detailed properties of a category (slug, metadata, assigned product counts, active status) so that catalog details are verified.  

**Preconditions**  
Người dùng nhấp vào một danh mục cụ thể từ danh sách.  

**Normal Flow**  
*Xem chi tiết thông tin và danh sách sản phẩm thuộc danh mục*  
1. Người dùng nhấp chọn một danh mục trong cây phân cấp.  
2. Hệ thống hiển thị bảng thông tin chi tiết của danh mục:  
   - Mã danh mục, Tên danh mục, Đường dẫn thân thiện (Slug: `/bon-cau/bon-cau-thong-minh`).  
   - Danh mục cha, Cấp độ phân cấp (Level 1, Level 2).  
   - Tổng số sản phẩm đang hoạt động trực thuộc.  
   - Danh sách tóm tắt các sản phẩm tiêu biểu trong danh mục.  
3. Người dùng xem thông tin và kiểm tra cấu trúc phân bổ hàng hóa.  

**Alternative Flows**  
- **Không tìm thấy danh mục:**  
  1. Danh mục đã bị xóa trước đó. Hệ thống thông báo không tồn tại.  

**Postconditions**  
Thông tin thuộc tính của danh mục được hiển thị rõ ràng.  

---

### UC-168 Tìm kiếm / lọc sản phẩm (Search/Filter Internal Products)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As an internal user, I want to search and filter internal products by name, product code, SKU, category, brand, and stock status so that I can quickly retrieve items during sales or restocking.  

**Preconditions**  
Người dùng đang ở trên màn hình Danh sách sản phẩm nội bộ (UC-026).  

**Normal Flow**  
*Tìm kiếm sản phẩm nhanh chóng trong hệ thống quản trị*  
1. Người dùng nhập từ khóa tìm kiếm (Mã sản phẩm, Mã SKU, hoặc Tên sản phẩm) vào thanh tìm kiếm.  
2. Người dùng kết hợp các bộ lọc:  
   - Danh mục (chọn từ cây danh mục).  
   - Thương hiệu (TOTO, INAX, Caesar...).  
   - Trạng thái tồn kho (`Tất cả`, `Còn hàng trong kho`, `Sắp hết hàng`, `Hết hàng`).  
   - Trạng thái kinh doanh (`Đang kinh doanh`, `Ngừng kinh doanh`).  
3. Người dùng nhấn Enter hoặc nhấp "Tìm kiếm".  
4. Hệ thống tải lại bảng dữ liệu với các sản phẩm thỏa mãn tiêu chí tìm kiếm.  
5. Hệ thống hiển thị số lượng bản ghi tìm thấy.  

**Alternative Flows**  
- **Không tìm thấy kết quả:**  
  1. Không có sản phẩm nào phù hợp.  
  2. Hệ thống hiển thị thông báo "Không tìm thấy sản phẩm nào" kèm nút "Đặt lại bộ lọc".  

**Postconditions**  
Bảng dữ liệu hiển thị tập sản phẩm cần quản lý theo đúng tiêu chí lọc.  

---

### UC-169 Quản lý hình ảnh sản phẩm (Manage Product Images)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to upload, reorder, designate primary thumbnail, and delete product gallery images so that the catalog presents high-quality visual showcases.  

**Preconditions**  
Chủ cửa hàng đang ở tab "Thư viện hình ảnh" trong chi tiết chỉnh sửa sản phẩm.  

**Normal Flow**  
*Tải lên, sắp xếp và đổi ảnh đại diện sản phẩm*  
1. Chủ cửa hàng nhấp nút "Tải ảnh lên" (Upload Images) hoặc kéo thả nhiều tệp ảnh vào khu vực tải lên (Dropzone).  
2. Hệ thống kiểm tra định dạng tệp (chỉ chấp nhận JPG, PNG, WEBP) và dung lượng (< 5MB/ảnh).  
3. Hệ thống gọi Secondary Actor `Cloudinary` để tải các tệp ảnh lên đám mây và nhận về danh sách URL ảnh bảo mật.  
4. Các ảnh mới được hiển thị trong lưới thư viện ảnh của sản phẩm.  
5. Chủ cửa hàng nhấp vào nút "Đặt làm ảnh đại diện" (Set as Primary) tại bức ảnh đẹp nhất để làm ảnh hiển thị chính trên website.  
6. Chủ cửa hàng có thể kéo thả để sắp xếp thứ tự hiển thị của các ảnh phụ.  
7. Chủ cửa hàng nhấp nút "Lưu thư viện ảnh".  
8. Hệ thống lưu cấu trúc ảnh vào cơ sở dữ liệu.  
9. Hệ thống hiển thị thông báo thành công (MSG154: "Cập nhật hình ảnh sản phẩm thành công!").  

**Alternative Flows**  
- **Xóa một ảnh trong thư viện:**  
  1. Chủ cửa hàng nhấp vào biểu tượng thùng rác tại một bức ảnh.  
  2. Hệ thống hỏi xác nhận xóa -> Chủ cửa hàng đồng ý.  
  3. Hệ thống gỡ liên kết ảnh và gọi lệnh xóa trên Cloudinary.  
- **Tải lên tệp không đúng định dạng:**  
  1. Người dùng chọn tệp PDF hoặc Word.  
  2. Hệ thống từ chối tệp và báo lỗi (MSG18: "Chỉ chấp nhận tệp hình ảnh định dạng JPG, PNG, WEBP").  

**Postconditions**  
Album hình ảnh và ảnh đại diện của sản phẩm được cập nhật hoàn chỉnh.  

---

### UC-170 Xem danh sách biến thể của một sản phẩm (View Product Variants List)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As an internal user, I want to view all variants of a product (color, size, SKU, barcodes, pricing, stock levels, serial flags) so that I can manage item variations comprehensively.  

**Preconditions**  
Người dùng đã mở trang chi tiết của một sản phẩm trong hệ thống quản trị.  

**Normal Flow**  
*Xem danh sách toàn bộ các biến thể của một sản phẩm*  
1. Người dùng chọn tab "Danh sách biến thể" (Product Variants).  
2. Hệ thống hiển thị bảng danh sách các biến thể của sản phẩm:  
   - Cột: Ảnh biến thể, Mã SKU, Mã vạch Barcode, Tên biến thể (Kèm thuộc tính màu/kích thước), Giá niêm yết, Giá vốn, Tồn thực tế, Tồn giữ chỗ, Tồn khả dụng, Quản lý Serial (Có/Không), Trạng thái (Đang bán / Tạm ẩn), Cột Thao tác (Sửa, Đổi giá, Bật/Tắt, Xóa).  
3. Người dùng xem thông tin từng biến thể và theo dõi tình trạng hàng hóa cụ thể.  

**Alternative Flows**  
- **Sản phẩm chưa có biến thể nào:**  
  1. Sản phẩm mới tạo chưa khai báo biến thể.  
  2. Hệ thống hiển thị bảng trống kèm nút "Thêm biến thể đầu tiên" (UC-022).  

**Postconditions**  
Danh sách tất cả các biến thể trực thuộc sản phẩm được hiển thị đầy đủ và chi tiết.  

---

### UC-171 Bật/Tắt trạng thái biến thể (Activate/Deactivate Variant)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to activate or deactivate an individual variant so that temporarily unavailable options are hidden from checkout without taking down the entire product.  

**Preconditions**  
Chủ cửa hàng đang ở danh sách biến thể của một sản phẩm (UC-170).  

**Normal Flow**  
*Ẩn hoặc mở bán lại một biến thể sản phẩm cụ thể*  
1. Tại dòng biến thể tương ứng (ví dụ: Biến thể màu Đen nhám hết hàng nhà máy), Chủ cửa hàng gạt công tắc Trạng thái sang `Inactive`.  
2. Hệ thống hiển thị hộp thoại xác nhận: "Bạn có chắc muốn tạm dừng bán biến thể [Mã SKU]? Biến thể này sẽ không hiển thị cho khách chọn trên website".  
3. Chủ cửa hàng nhấp "Xác nhận".  
4. Hệ thống cập nhật `IsActive = False` cho biến thể đó.  
5. Hệ thống ghi nhận thao tác vào Audit Log.  
6. Hệ thống hiển thị thông báo thành công (MSG155: "Đã tắt kích hoạt biến thể thành công!"). Biến thể này chuyển sang trạng thái ẩn trên website mua sắm.  

**Alternative Flows**  
- **Bật lại biến thể:**  
  1. Gạt công tắc từ Inactive sang Active khi có hàng trở lại.  
  2. Hệ thống kích hoạt lại biến thể ngay lập tức.  

**Postconditions**  
Biến thể được cập nhật trạng thái hiển thị, kiểm soát chính xác các tùy chọn cho khách hàng.  

---

### UC-172 Xóa biến thể (xóa mềm) (Soft Delete Variant)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to soft-delete a product variant that has been permanently discontinued so that historical orders remain intact while the SKU is retired.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập vào hệ thống.  
2. Biến thể mục tiêu không còn tồn kho thực tế (Tồn kho = 0) và không nằm trong các đơn hàng đang giao dở dang.  

**Normal Flow**  
*Xóa mềm biến thể vĩnh viễn không còn kinh doanh*  
1. Chủ cửa hàng nhấp nút "Xóa" tại dòng biến thể cần loại bỏ.  
2. Hệ thống kiểm tra điều kiện nghiệp vụ:  
   - Kiểm tra tồn kho thực tế: Tồn kho phải bằng 0.  
   - Kiểm tra không có đơn hàng nào đang ở trạng thái Chờ xử lý hoặc Đang giao chứa biến thể này.  
3. Nếu thỏa mãn, hệ thống hiển thị hộp thoại xác nhận kèm cảnh báo: "Bạn có chắc chắn muốn xóa biến thể [Mã SKU]? Hành động này sẽ đánh dấu xóa mềm, dữ liệu lịch sử các đơn hàng cũ vẫn được lưu giữ".  
4. Chủ cửa hàng nhấp "Xác nhận xóa".  
5. Hệ thống cập nhật `IsDeleted = True` cho biến thể trong cơ sở dữ liệu.  
6. Hệ thống ghi nhận sự kiện vào Audit Log.  
7. Hệ thống hiển thị thông báo thành công (MSG156: "Đã xóa biến thể thành công!") và loại bỏ khỏi bảng danh sách.  

**Alternative Flows**  
- **Biến thể vẫn còn hàng tồn kho (> 0):**  
  1. Biến thể còn hàng trong kho.  
  2. Hệ thống chặn thao tác và thông báo (MSG157: "Không thể xóa biến thể vì vẫn còn tồn kho thực tế ([X] sản phẩm). Vui lòng điều chỉnh xuất hết tồn kho hoặc chọn Tắt kích hoạt thay vì xóa").  

**Postconditions**  
Biến thể bị xóa mềm, không thể chọn mua mới nhưng dữ liệu kế toán và hóa đơn cũ được bảo toàn 100%.  

---

### UC-173 Quản lý SKU (tạo, sinh tự động, kiểm tra trùng) (Manage SKU)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As an internal user, I want the system to generate standardized unique SKU codes automatically (e.g., [BRAND]-[CATEGORY]-[MODEL]-[COLOR]) and check duplicates in real-time so that inventory identification is consistent.  

**Preconditions**  
Người dùng đang ở biểu mẫu tạo hoặc chỉnh sửa biến thể sản phẩm.  

**Normal Flow**  
*Tự động sinh mã SKU chuẩn hóa và kiểm tra tính duy nhất*  
1. Sau khi người dùng chọn Thương hiệu, Danh mục, Tên model và Thuộc tính biến thể, người dùng nhấp vào nút "Tự động sinh mã SKU" (Auto-generate SKU).  
2. Hệ thống áp dụng thuật toán chuẩn hóa mã kho của cửa hàng:  
   - `[Mã Hãng] - [Mã Nhóm] - [Mã Model] - [Mã Màu] - [Kích Thước]`  
   - Ví dụ: `TOTO-BC-MS885-TRANG-300`.  
3. Hệ thống điền chuỗi SKU vừa sinh vào ô nhập liệu "Mã SKU".  
4. Hệ thống tự động thực hiện truy vấn bất đồng bộ (Debounced Asynchronous Validation) kiểm tra tính duy nhất của mã SKU này trong cơ sở dữ liệu.  
5. Kết quả kiểm tra hợp lệ: Hệ thống hiển thị dấu tích xanh cạnh ô nhập liệu: "Mã SKU hợp lệ và chưa từng sử dụng".  

**Alternative Flows**  
- **Mã SKU bị trùng:**  
  1. Mã SKU đã tồn tại trong cơ sở dữ liệu.  
  2. Hệ thống hiển thị dấu chéo đỏ kèm thông báo lỗi (MSG139: "Mã SKU đã tồn tại! Hệ thống tự động thêm hậu tố -V2 để đảm bảo duy nhất").  
  3. Người dùng chấp nhận hoặc chỉnh sửa lại theo ý muốn.  

**Postconditions**  
Mã SKU chuẩn hóa, duy nhất được thiết lập cho biến thể sản phẩm.  

---

### UC-174 Import sản phẩm từ Excel/CSV (Import Products)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to batch-import products and variants from an Excel/CSV file using a template with pre-validation so that new supplier catalogs can be uploaded quickly without manual entry.  

**Preconditions**  
1. Chủ cửa hàng đã đăng nhập vào hệ thống.  
2. Chủ cửa hàng đã chuẩn bị tệp dữ liệu Excel theo mẫu chuẩn của hệ thống.  

**Normal Flow**  
*Nhập dữ liệu hàng loạt sản phẩm từ file Excel*  
1. Chủ cửa hàng truy cập màn hình Danh sách sản phẩm và nhấp nút "Nhập dữ liệu từ Excel" (Import Excel).  
2. Hệ thống mở cửa sổ Import:  
   - Cung cấp nút tải về tệp mẫu: "Tải file mẫu Excel (.xlsx)".  
   - Khu vực tải lên tệp dữ liệu kèm hướng dẫn các cột bắt buộc (Tên SP, Mã SKU, Danh mục, Thương hiệu, Giá niêm yết, ĐVT).  
3. Chủ cửa hàng chọn và tải tệp `.xlsx` từ máy tính lên hệ thống.  
4. Hệ thống đọc tệp và thực thi tiến trình kiểm tra hợp lệ trước (Pre-validation):  
   - Kiểm tra định dạng dữ liệu từng dòng.  
   - Kiểm tra trùng lặp mã SKU trong file và trùng với cơ sở dữ liệu.  
   - Kiểm tra tồn tại của Danh mục và Thương hiệu đã khai báo.  
5. Hệ thống hiển thị kết quả kiểm tra trước:  
   - Tổng số dòng: 150 dòng.  
   - Hợp lệ: 148 dòng (Màu xanh).  
   - Bị lỗi: 2 dòng (Màu đỏ, chỉ rõ lỗi: Dòng 14 thiếu Tên sản phẩm; Dòng 89 Mã SKU bị trùng lặp).  
6. Chủ cửa hàng có thể chọn "Bỏ qua các dòng lỗi và nhập 148 dòng hợp lệ" hoặc "Hủy bỏ để sửa lại file".  
7. Chủ cửa hàng chọn tiếp tục nhập các dòng hợp lệ và nhấp "Bắt đầu Import".  
8. Hệ thống tiến hành ghi dữ liệu hàng loạt vào cơ sở dữ liệu trong một Transaction an toàn.  
9. Hệ thống ghi nhận sự kiện Import vào Audit Log.  
10. Hệ thống hiển thị thông báo thành công (MSG158: "Đã nhập thành công 148 sản phẩm vào hệ thống!") và làm mới danh sách sản phẩm.  

**Alternative Flows**  
- **File Excel sai định dạng mẫu:**  
  1. Tệp tải lên bị thiếu cột tiêu đề bắt buộc hoặc không phải file Excel.  
  2. Hệ thống từ chối tệp và báo lỗi (MSG159: "Cấu trúc file Excel không đúng mẫu quy định. Vui lòng tải file mẫu chuẩn để nhập liệu").  

**Postconditions**  
Hàng trăm sản phẩm mới được đưa vào cơ sở dữ liệu nhanh chóng và an toàn.  

---

### UC-175 Export danh sách sản phẩm (Export Products)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to export the filtered product and variant list to Excel/CSV with images and stock levels so that data can be audited or shared with partners.  

**Preconditions**  
Người dùng đang ở trên màn hình Danh sách sản phẩm.  

**Normal Flow**  
*Xuất danh sách sản phẩm ra tệp bảng tính Excel*  
1. Người dùng áp dụng các bộ lọc mong muốn (hoặc để mặc định toàn bộ danh mục).  
2. Người dùng nhấp vào nút "Xuất file Excel" (Export to Excel) trên thanh công cụ.  
3. Hệ thống hiển thị hộp thoại chọn các trường dữ liệu cần xuất:  
   - [x] Thông tin cơ bản: Mã SP, Tên sản phẩm, Danh mục, Thương hiệu.  
   - [x] Biến thể: Mã SKU, Tên biến thể, Mã vạch.  
   - [x] Bảng giá: Giá niêm yết (kèm Giá vốn nếu có quyền).  
   - [x] Tồn kho: Tồn thực tế, Tồn giữ chỗ, Tồn khả dụng.  
   - [x] Thông số kỹ thuật: Kích thước, Tâm xả, Thời hạn bảo hành.  
4. Người dùng nhấp "Xác nhận xuất file".  
5. Hệ thống đóng gói dữ liệu và tạo tệp `Danh_Sach_San_Pham_[YYYYMMDD].xlsx`.  
6. Trình duyệt tự động tải tệp tin về máy tính của người dùng.  
7. Hệ thống hiển thị thông báo thành công (MSG160: "Xuất danh sách sản phẩm ra Excel thành công!").  

**Alternative Flows**  
- **Không có dữ liệu để xuất:**  
  1. Bộ lọc không tìm thấy sản phẩm nào.  
  2. Nút xuất bị vô hiệu hóa kèm thông báo thích hợp.  

**Postconditions**  
Tệp danh mục sản phẩm được tải về máy tính người dùng phục vụ lưu trữ hoặc in ấn bảng giá.  

---

### UC-176 Quản lý đơn vị tính (cái, hộp, bộ, kg...) (Manage Units of Measurement)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Store Owner, I want to manage units of measurement (UOM - Cái, Bộ, Thùng, Hộp, Mét, Cuộn) so that products are accurately measured and accounted for during sales and inventory counts.  

**Preconditions**  
Chủ cửa hàng đã đăng nhập và truy cập trang "Cấu hình đơn vị tính".  

**Normal Flow**  
*Tạo và quản lý các đơn vị tính hàng hóa*  
1. Chủ cửa hàng truy cập mục "Đơn vị tính" trong cài đặt Sản phẩm.  
2. Hệ thống hiển thị danh sách các ĐVT hiện có: Mã ĐVT, Tên ĐVT (Bộ, Cái, Thùng, Hộp...), Mô tả, Số lượng sản phẩm đang áp dụng.  
3. Chủ cửa hàng nhấp nút "Thêm đơn vị tính mới".  
4. Hệ thống hiển thị biểu mẫu: Tên đơn vị tính (ví dụ: "Cuộn", "Mét"), Ký hiệu viết tắt.  
5. Chủ cửa hàng nhập thông tin và nhấp "Lưu".  
6. Hệ thống kiểm tra tên ĐVT không trùng lặp và lưu vào cơ sở dữ liệu.  
7. Hệ thống hiển thị thông báo thành công (MSG161: "Thêm đơn vị tính thành công!"). Đơn vị tính mới sẵn sàng xuất hiện trong dropdown chọn ĐVT khi tạo sản phẩm.  

**Alternative Flows**  
- **Xóa đơn vị tính đang được sử dụng:**  
  1. Chủ cửa hàng bấm xóa một đơn vị tính đang có sản phẩm gán (ví dụ: "Bộ").  
  2. Hệ thống chặn xóa và cảnh báo (MSG162: "Không thể xóa đơn vị tính này vì đang có sản phẩm áp dụng. Vui lòng chuyển đổi ĐVT của sản phẩm trước khi xóa").  

**Postconditions**  
Danh mục đơn vị tính được chuẩn hóa trên toàn hệ thống.  

---

### UC-177 Quản lý thương hiệu / nhà sản xuất (Manage Brands / Manufacturers)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner, I want to manage brands and manufacturers (TOTO, INAX, Caesar, Viglacera, American Standard) with logos, origins, and warranty contact info so that products are correctly branded.  

**Preconditions**  
Chủ cửa hàng đã đăng nhập vào hệ thống quản lý.  

**Normal Flow**  
*Tạo và quản lý thông tin các thương hiệu thiết bị vệ sinh*  
1. Chủ cửa hàng truy cập mục "Quản lý thương hiệu" trong danh mục Sản phẩm.  
2. Hệ thống hiển thị danh sách các thương hiệu: Logo, Tên thương hiệu, Xuất xứ (Nhật Bản, Đài Loan, Việt Nam, Mỹ...), Số điện thoại hotline bảo hành của hãng, Website hãng, Số lượng sản phẩm đang kinh doanh.  
3. Chủ cửa hàng nhấp nút "Thêm thương hiệu mới".  
4. Hệ thống hiển thị biểu mẫu:  
   - Tên thương hiệu (ví dụ: "Grohe").  
   - Mã thương hiệu (ví dụ: `BRAND-GROHE`).  
   - Quốc gia xuất xứ (ví dụ: "Đức").  
   - Logo thương hiệu (Tải ảnh lên).  
   - Hotline tổng đài kỹ thuật & bảo hành của hãng.  
   - Mô tả giới thiệu thương hiệu.  
5. Chủ cửa hàng nhập thông tin và tải logo lên.  
6. Hệ thống gọi `Cloudinary` lưu trữ ảnh logo.  
7. Hệ thống lưu bản ghi thương hiệu mới vào cơ sở dữ liệu.  
8. Hệ thống hiển thị thông báo thành công (MSG163: "Thêm thương hiệu mới thành công!") và đưa thương hiệu vào danh sách lựa chọn của sản phẩm.  

**Alternative Flows**  
- **Tên thương hiệu bị trùng:**  
  1. Tên thương hiệu đã tồn tại. Hệ thống báo lỗi trùng lặp (MSG164: "Tên thương hiệu đã tồn tại").  

**Postconditions**  
Hồ sơ thương hiệu được thiết lập, hỗ trợ phân loại sản phẩm và tra cứu bảo hành chính hãng.  

---
