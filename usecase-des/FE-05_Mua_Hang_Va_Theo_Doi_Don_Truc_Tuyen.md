# PHÂN HỆ FE-05: MUA HÀNG & THEO DÕI ĐƠN TRỰC TUYẾN

---

### UC-038 Thêm sản phẩm vào giỏ hàng (Add Product to Cart)
**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
None  

**Description**  
As a Customer or Guest, I want to select a product variant (color, dimensions, specifications) and quantity, then add it to my shopping cart so that I can accumulate items before placing an online order.  

**Preconditions**  
Người dùng đang ở trang chi tiết sản phẩm (UC-029) trên website của cửa hàng.  

**Normal Flow**  
*Chọn biến thể và thêm sản phẩm vào giỏ hàng*  
1. Người dùng xem thông tin sản phẩm và lựa chọn biến thể cụ thể (ví dụ: Bồn cầu 1 khối TOTO - Màu Trắng bóng, Nắp đóng êm, Tâm xả 300mm).  
2. Hệ thống cập nhật hiển thị hình ảnh, mã SKU, giá bán niêm yết tương ứng của biến thể và số lượng tồn kho khả dụng hiện tại.  
3. Người dùng chọn số lượng muốn mua (mặc định là 1, có nút tăng giảm `+` / `-`).  
4. Người dùng nhấp vào nút "Thêm vào giỏ hàng" (Add to Cart).  
5. Hệ thống kiểm tra điều kiện nghiệp vụ:  
   - Biến thể sản phẩm đang ở trạng thái kích hoạt (`Active`).  
   - Số lượng chọn mua > 0 và tổng số lượng (số lượng mới + số lượng đã có sẵn trong giỏ của mặt hàng đó) <= Số lượng tồn kho khả dụng (Available Stock).  
6. Hệ thống thêm mặt hàng vào giỏ hàng:  
   - Đối với Customer đã đăng nhập: Cập nhật cơ sở dữ liệu giỏ hàng trên máy chủ liên kết với User ID.  
   - Đối với Guest chưa đăng nhập: Cập nhật giỏ hàng lưu tại Session / Local Storage trên trình duyệt của máy khách.  
7. Hệ thống cập nhật số lượng hiển thị trên biểu tượng Giỏ hàng ở Header (Badge Counter).  
8. Hệ thống hiển thị thông báo nhẹ (Toast notification) góc phải màn hình (MSG71: "Đã thêm sản phẩm vào giỏ hàng thành công!") kèm nút "Xem giỏ hàng" và nút "Tiến hành thanh toán".  

**Alternative Flows**  
- **Step 5_Số lượng chọn mua vượt quá tồn kho khả dụng:**  
  1. Người dùng chọn số lượng lớn hơn số lượng hàng hiện có thể bán trong kho.  
  2. Hệ thống ngăn chặn hành động thêm và hiển thị cảnh báo (MSG72: "Số lượng bạn chọn vượt quá số lượng còn lại trong kho. Hiện chỉ còn [AvailableQuantity] sản phẩm").  
  3. Hệ thống tự động điều chỉnh số lượng chọn về bằng với tồn kho tối đa khả dụng.  
- **Step 1.1_Người dùng chưa chọn biến thể:**  
  1. Sản phẩm có nhiều biến thể (màu sắc/kích thước) nhưng người dùng bấm nút Thêm vào giỏ mà chưa chọn đủ thuộc tính.  
  2. Hệ thống làm nổi bật các trường biến thể cần chọn và hiển thị thông báo (MSG73: "Vui lòng chọn đầy đủ màu sắc và kích thước sản phẩm").  
- **Step 4.1_Người dùng nhấp "Mua ngay" (Buy Now):**  
  1. Người dùng nhấp nút "Mua ngay" thay vì "Thêm vào giỏ hàng".  
  2. Hệ thống thêm sản phẩm vào giỏ hàng và ngay lập tức điều hướng người dùng thẳng tới màn hình Đặt hàng online (UC-040).  

**Postconditions**  
1. Mặt hàng và biến thể được lưu chính xác vào giỏ hàng của người dùng.  
2. Biểu tượng giỏ hàng trên thanh tiêu đề được cập nhật số đếm.  

---

### UC-039 Quản lý giỏ hàng (Manage Cart)
**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
None  

**Description**  
As a Customer or Guest, I want to review my shopping cart, modify item quantities, remove unwanted items, and see the subtotal and total order amount so that I can adjust my purchases before checkout.  

**Preconditions**  
Người dùng nhấp vào biểu tượng Giỏ hàng trên thanh điều hướng hoặc truy cập trang `/gio-hang`.  

**Normal Flow**  
*Xem và điều chỉnh các mặt hàng trong giỏ hàng*  
1. Người dùng nhấp vào biểu tượng Giỏ hàng (Cart icon) trên Header.  
2. Hệ thống hiển thị trang "Giỏ hàng của bạn", bao gồm:  
   - Danh sách các sản phẩm trong giỏ: Ảnh thu nhỏ, Tên sản phẩm, Biến thể (màu, kích thước), Đơn giá, Ô điều chỉnh số lượng (`-`, Số lượng, `+`), Thành tiền từng dòng, Nút "Xóa" (biểu tượng thùng rác).  
   - Bảng tổng kết giỏ hàng ở bên phải: Tạm tính, Dự kiến phí vận chuyển, Giảm giá (nếu có), và Tổng tiền cần thanh toán.  
   - Nút "Tiến hành đặt hàng" (Proceed to Checkout) và liên kết "Tiếp tục mua hàng".  
3. Người dùng thực hiện thay đổi số lượng của một mặt hàng bằng cách nhấp nút `+` hoặc `-` (hoặc nhập số lượng mới vào ô).  
4. Hệ thống kiểm tra số lượng mới <= Tồn kho khả dụng của biến thể đó.  
5. Hệ thống tính toán lại Thành tiền của mặt hàng, Tạm tính và Tổng tiền đơn hàng ngay lập tức mà không cần tải lại toàn bộ trang (Real-time recalculation).  
6. Người dùng kiểm tra tổng tiền và nhấp nút "Tiến hành đặt hàng" để chuyển sang bước thanh toán.  

**Alternative Flows**  
- **Step 3.1_Người dùng xóa một sản phẩm khỏi giỏ hàng:**  
  1. Người dùng nhấp vào nút "Xóa" tại dòng sản phẩm không còn nhu cầu mua.  
  2. Hệ thống hiển thị popup xác nhận: "Bạn có chắc chắn muốn bỏ sản phẩm này khỏi giỏ hàng?".  
  3. Người dùng chọn "Đồng ý".  
  4. Hệ thống loại bỏ sản phẩm đó khỏi giỏ hàng, cập nhật lại số lượng sản phẩm trên Header, tính toán lại tổng tiền và hiển thị thông báo (MSG74: "Đã xóa sản phẩm khỏi giỏ hàng").  
- **Step 3.2_Giỏ hàng trở nên trống rỗng (Empty Cart):**  
  1. Sau khi xóa hết các sản phẩm, giỏ hàng không còn mặt hàng nào.  
  2. Hệ thống hiển thị màn hình giỏ hàng trống với hình minh họa trực quan và thông báo (MSG75: "Giỏ hàng của bạn đang trống! Hãy khám phá hàng ngàn thiết bị vệ sinh chất lượng cao của chúng tôi") kèm nút "Tiếp tục mua sắm".  
- **Step 4.1_Tăng số lượng vượt quá tồn kho:**  
  1. Người dùng nhấp nút `+` khi số lượng đã bằng với số lượng tồn kho khả dụng hiện tại.  
  2. Nút `+` bị vô hiệu hóa (disabled) và hệ thống hiển thị thông báo cảnh báo (MSG76: "Số lượng sản phẩm trong kho chỉ còn tối đa [MaxQuantity]").  

**Postconditions**  
1. Danh sách sản phẩm, số lượng và tổng giá trị trong giỏ hàng được cập nhật chính xác theo mong muốn của người dùng.  

---

### UC-040 Đặt hàng online (Place Online Order)
**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
Email Service, SMS Service  

**Description**  
As a Customer or Guest, I want to enter my delivery address, select a payment method (COD or VietQR Bank Transfer), and submit an online order so that the store can process and deliver the goods to me.  

**Preconditions**  
1. Giỏ hàng của người dùng có ít nhất một sản phẩm hợp lệ.  
2. Người dùng nhấp nút "Tiến hành đặt hàng" từ trang Giỏ hàng (UC-039).  

**Normal Flow**  
*Khách hàng hoàn tất đặt đơn hàng trực tuyến*  
1. Hệ thống hiển thị trang "Thanh toán & Đặt hàng" (Checkout Page) gồm 3 phần chính:  
   - Thông tin người nhận hàng: Họ tên, Số điện thoại, Email, Địa chỉ chi tiết (nếu là Customer đã đăng nhập thì tự động điền Địa chỉ mặc định từ UC-005, có nút chọn địa chỉ khác; nếu là Guest thì yêu cầu nhập mới).  
   - Phương thức vận chuyển & Ghi chú đơn hàng: Giao hàng tiêu chuẩn cửa hàng, Ô nhập ghi chú hẹn giờ giao hàng hoặc hướng dẫn đường đi.  
   - Phương thức thanh toán: Chọn 1 trong 2 phương thức: (1) Thanh toán khi nhận hàng - COD; (2) Chuyển khoản ngân hàng qua mã VietQR.  
   - Bảng tóm tắt đơn hàng: Danh sách mặt hàng, Tạm tính, Phí ship, Tổng cộng thanh toán.  
2. Người dùng kiểm tra thông tin người nhận, chọn phương thức thanh toán "Thanh toán khi nhận hàng (COD)" và nhập ghi chú nếu có.  
3. Người dùng nhấp nút "Xác nhận đặt hàng" (Place Order).  
4. Hệ thống thực hiện kiểm tra an toàn giao dịch cuối cùng:  
   - Kiểm tra tồn kho khả dụng cho toàn bộ các sản phẩm trong giỏ hàng.  
   - Kiểm tra tính hợp lệ của Số điện thoại, Họ tên và Địa chỉ người nhận.  
5. Hệ thống khởi tạo đơn hàng mới trong cơ sở dữ liệu:  
   - Mã đơn hàng được sinh tự động theo quy tắc `ORD-YYYYMMDD-XXXXX`.  
   - Kênh bán hàng: `Online`.  
   - Trạng thái đơn hàng ban đầu: `Chờ xác nhận (Unconfirmed)`.  
   - Trạng thái thanh toán: `Chưa thanh toán (Unpaid)`.  
6. Hệ thống kích hoạt quy trình giữ chỗ tồn kho (Reserve Stock - liên kết UC-086) cho toàn bộ các sản phẩm trong đơn.  
7. Hệ thống làm rỗng giỏ hàng của người dùng.  
8. Hệ thống gọi Secondary Actor `Email Service` và `SMS Service` gửi thông báo xác nhận đơn hàng kèm mã đơn đến khách hàng.  
9. Hệ thống chuyển hướng người dùng đến trang "Đặt hàng thành công" (Order Success Page) hiển thị mã đơn hàng, tổng tiền, tóm tắt sản phẩm và hướng dẫn tiếp theo.  

**Alternative Flows**  
- **Step 2.1_Người dùng chọn phương thức "Chuyển khoản VietQR":**  
  1. Người dùng chọn phương thức thanh toán VietQR.  
  2. Người dùng bấm "Xác nhận đặt hàng".  
  3. Hệ thống tạo đơn hàng và chuyển tiếp sang màn hình Thanh toán chuyển khoản VietQR (Kích hoạt UC-041).  
- **Step 4.1_Sản phẩm trong giỏ bị hết hàng trong lúc chờ đặt:**  
  1. Trong khoảng thời gian khách suy nghĩ, khách hàng khác đã mua hết tồn kho của một sản phẩm trong giỏ.  
  2. Hệ thống chặn đặt hàng, thông báo lỗi cụ thể (MSG77: "Sản phẩm [Tên sản phẩm] vừa hết hàng hoặc không đủ số lượng để cung cấp. Vui lòng cập nhật lại giỏ hàng").  
  3. Hệ thống điều hướng khách về trang giỏ hàng để cập nhật số lượng.  
- **Step 4.2_Thiếu thông tin người nhận hàng:**  
  1. Số điện thoại hoặc địa chỉ nhận hàng bị bỏ trống hoặc sai định dạng.  
  2. Hệ thống hiển thị cảnh báo đỏ tại các trường thông tin bị thiếu (MSG02, MSG22) và yêu cầu người dùng điền đầy đủ trước khi gửi.  

**Postconditions**  
1. Đơn hàng mới được tạo lập thành công với trạng thái `Unconfirmed`.  
2. Tồn kho của các sản phẩm được đưa vào trạng thái giữ chỗ (`Reserved Quantity`).  
3. Giỏ hàng được làm sạch.  
4. Khách hàng nhận được thông báo xác nhận qua Email/SMS.  

---

### UC-041 Thanh toán chuyển khoản VietQR (Pay by Bank Transfer)
**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
None  

**Description**  
As a Customer or Guest, I want to scan a dynamically generated VietQR code with pre-filled payment amount and order reference so that I can quickly and accurately transfer money via mobile banking.  

**Preconditions**  
1. Người dùng đã hoàn tất bước tạo đơn hàng và chọn phương thức "Chuyển khoản VietQR" (từ UC-040).  
2. Đơn hàng đang ở trạng thái thanh toán `Unpaid`.  

**Normal Flow**  
*Quét mã VietQR động để thanh toán đơn hàng online*  
1. Hệ thống chuyển hướng người dùng đến màn hình "Thanh toán chuyển khoản qua VietQR".  
2. Hệ thống gọi thuật toán sinh mã chuẩn VietQR (VietQR NAPAS 247 Specification) dựa trên thông tin cấu hình tài khoản ngân hàng của cửa hàng (UC-064) và thông tin đơn hàng:  
   - Tên ngân hàng thụ hưởng & Số tài khoản cửa hàng.  
   - Tên chủ tài khoản cửa hàng.  
   - Số tiền chính xác cần thanh toán (chính xác đến từng đồng).  
   - Nội dung chuyển khoản chuẩn hóa duy nhất: `DH [Mã đơn hàng] [Số điện thoại khách]` (ví dụ: `DH ORD2026100801 0987654321`).  
3. Hệ thống hiển thị mã QR động nổi bật trên màn hình kèm:  
   - Hướng dẫn 3 bước: Mở app ngân hàng -> Quét mã QR -> Xác nhận chuyển tiền (không sửa nội dung chuyển khoản).  
   - Bảng thông tin thanh toán dạng văn bản có nút "Sao chép số tài khoản", "Sao chép số tiền", "Sao chép nội dung chuyển khoản".  
   - Đồng hồ đếm ngược thời gian thanh toán (15 phút).  
4. Người dùng mở ứng dụng Ngân hàng trên điện thoại (Mobile Banking), quét mã VietQR. Ứng dụng ngân hàng tự động điền đầy đủ Số tài khoản, Tên người nhận, Số tiền và Nội dung chuyển khoản.  
5. Người dùng xác nhận chuyển khoản thành công trên ứng dụng ngân hàng của mình.  
6. Người dùng quay lại màn hình website và nhấp nút "Tôi đã chuyển khoản xong".  
7. Hệ thống chuyển đơn hàng sang trạng thái thanh toán `Chờ xác nhận tiền (Pending Verification)` và thông báo cho khách hàng: "Cửa hàng đã ghi nhận thông báo chuyển tiền của bạn và sẽ kiểm tra tài khoản để xác nhận ngay!".  
8. Hệ thống chuyển hướng người dùng về màn hình Theo dõi đơn hàng (UC-043).  

**Alternative Flows**  
- **Hết hạn thời gian quét mã VietQR (Payment Timeout):**  
  1. Hết 15 phút người dùng không thực hiện chuyển khoản hoặc không nhấp xác nhận.  
  2. Màn hình VietQR hiển thị thông báo hết hiệu lực: "Mã QR đã hết hạn hiệu lực giao dịch. Đơn hàng của bạn vẫn được lưu lại".  
  3. Khách hàng có thể bấm "Tạo lại mã QR mới" để thanh toán tiếp, hoặc bấm "Đổi phương thức sang COD".  
- **Khách hàng nhấp "Hủy thanh toán / Quay lại":**  
  1. Khách hàng nhấp nút "Quay lại đơn hàng".  
  2. Hệ thống giữ đơn hàng ở trạng thái `Unconfirmed` và `Unpaid`, khách hàng có thể thanh toán lại sau trong mục "Đơn hàng của tôi".  

**Postconditions**  
1. Mã VietQR chuẩn xác được tạo ra cho đơn hàng.  
2. Trạng thái thanh toán của đơn hàng được cập nhật theo tiến trình.  

---

### UC-042 Xem danh sách đơn hàng cá nhân (View My Order List)
**Primary Actors**  
Customer  

**Secondary Actors**  
None  

**Description**  
As a Customer, I want to view the list of my placed orders categorized by processing status so that I can keep track of all my purchases.  

**Preconditions**  
Khách hàng đã đăng nhập vào tài khoản trên website.  

**Normal Flow**  
*Xem danh sách đơn hàng đã đặt trong tài khoản cá nhân*  
1. Khách hàng nhấp vào mục "Đơn hàng của tôi" (My Orders) từ menu tài khoản.  
2. Hệ thống tải và hiển thị trang danh sách đơn hàng với các tab phân loại theo trạng thái:  
   - `Tất cả` (All).  
   - `Chờ xác nhận` (Pending/Unconfirmed).  
   - `Đang xử lý / Chuẩn bị hàng` (Processing).  
   - `Đang giao hàng` (Shipping).  
   - `Đã giao thành công / Hoàn tất` (Delivered/Completed).  
   - `Đã hủy` (Cancelled).  
3. Mỗi đơn hàng trong danh sách hiển thị thẻ thông tin tóm tắt:  
   - Mã đơn hàng (ORD-XXXXX) và Ngày đặt.  
   - Trạng thái đơn hàng (Huy hiệu màu) và Trạng thái thanh toán (Chưa thanh toán / Đã thanh toán).  
   - Danh sách sản phẩm thu gọn (Ảnh đại diện sản phẩm đầu tiên, Tên sản phẩm, Biến thể, Số lượng và nhãn "+X sản phẩm khác" nếu có nhiều món).  
   - Tổng số tiền đơn hàng.  
   - Các nút hành động tương ứng: "Xem chi tiết", "Theo dõi đơn", "Hủy đơn" (nếu đơn chưa xác nhận), "Mua lại".  
4. Khách hàng nhấp chuyển giữa các tab trạng thái để lọc đơn hàng cần tra cứu.  
5. Khách hàng nhấp vào nút "Xem chi tiết" tại một đơn hàng để chuyển sang xem đầy đủ thông tin (UC-043).  

**Alternative Flows**  
- **Chưa có đơn hàng nào:**  
  1. Khách hàng chưa từng đặt mua sản phẩm nào.  
  2. Hệ thống hiển thị thông báo (MSG78: "Bạn chưa có đơn hàng nào. Hãy khám phá và mua sắm ngay hôm nay!") kèm nút "Khám phá sản phẩm".  

**Postconditions**  
Danh sách toàn bộ các đơn hàng của khách hàng được hiển thị chính xác, bảo mật và phân loại rõ ràng.  

---

### UC-043 Theo dõi trạng thái đơn hàng (Track Order Status)
**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
None  

**Description**  
As a Customer or Guest, I want to track the detailed progress, payment status, and delivery milestones of an order so that I know exactly when my products will arrive.  

**Preconditions**  
Khách hàng mở chi tiết đơn hàng từ danh sách đơn hàng cá nhân, hoặc Khách vãng lai truy cập vào đường dẫn tra cứu đơn hàng công khai.  

**Normal Flow**  
*Theo dõi tiến độ xử lý và hành trình giao hàng của đơn hàng*  
1. Người dùng truy cập trang "Chi tiết đơn hàng - [Mã đơn]".  
2. Hệ thống hiển thị đầy đủ thông tin chi tiết của đơn hàng:  
   - Thanh tiến trình trạng thái trực quan (Status Timeline Steps):  
     + Bước 1: Đặt hàng thành công (`Đã đặt`).  
     + Bước 2: Cửa hàng xác nhận (`Đã xác nhận`).  
     + Bước 3: Đang đóng gói / Xuất kho (`Chuẩn bị hàng`).  
     + Bước 4: Đang trên đường giao (`Đang giao hàng`).  
     + Bước 5: Giao hàng thành công (`Hoàn tất`).  
   - Mỗi bước trên thanh tiến trình hiển thị dấu thời gian thực hiện cụ thể (Ngày, giờ, phút).  
   - Thông tin người nhận & Địa chỉ giao hàng.  
   - Phương thức thanh toán & Trạng thái thanh toán (Đã thanh toán đủ / Chưa thanh toán / Thu hộ COD).  
   - Danh sách chi tiết các mặt hàng: Tên, biến thể, hình ảnh, số lượng, đơn giá, tổng tiền từng món.  
   - Tổng kết tài chính: Tạm tính sản phẩm, Phí vận chuyển, Giảm giá, Tổng tiền thanh toán.  
   - Lịch sử các đợt giao hàng (nếu đơn hàng được giao theo nhiều đợt - liên kết FE-08).  
3. Người dùng theo dõi tiến trình và cập nhật thông tin cần thiết.  

**Alternative Flows**  
- **Tra cứu dành cho Guest (Khách vãng lai chưa tạo tài khoản):**  
  1. Người dùng truy cập trang `https://store.com/tra-cuu-don-hang`.  
  2. Người dùng nhập Mã đơn hàng và Số điện thoại đặt hàng.  
  3. Hệ thống đối soát cơ sở dữ liệu: Nếu khớp chính xác, chuyển sang hiển thị chi tiết như bước 2 của Normal Flow; nếu không khớp, báo lỗi (MSG79: "Mã đơn hàng hoặc số điện thoại không chính xác").  
- **Đơn hàng đã bị hủy:**  
  1. Đơn hàng đang ở trạng thái `Cancelled`.  
  2. Thanh tiến trình hiển thị trạng thái hủy màu đỏ kèm lý do hủy đơn và người thực hiện hủy (Khách hủy hoặc Cửa hàng hủy).  

**Postconditions**  
Người dùng nắm bắt được thông tin trạng thái thực tế và lịch sử vận chuyển của đơn hàng.  

---

### UC-044 Hủy đơn hàng chưa xác nhận (Cancel Unconfirmed Order)
**Primary Actors**  
Customer  

**Secondary Actors**  
None  

**Description**  
As a Customer, I want to directly cancel an online order while it is still in the 'Unconfirmed' state so that I can stop the purchase without needing store staff intervention.  

**Preconditions**  
1. Khách hàng đã đăng nhập vào hệ thống.  
2. Đơn hàng mục tiêu đang ở trạng thái `Chờ xác nhận (Unconfirmed)` (Cửa hàng chưa tiếp nhận xử lý).  

**Normal Flow**  
*Khách hàng tự hủy đơn hàng online chưa được xác nhận*  
1. Khách hàng mở trang Chi tiết đơn hàng hoặc tìm thấy đơn hàng trong danh sách "Đơn hàng của tôi".  
2. Hệ thống kiểm tra trạng thái đơn: Do trạng thái là `Unconfirmed`, hệ thống hiển thị nút "Hủy đơn hàng" (Cancel Order).  
3. Khách hàng nhấp vào nút "Hủy đơn hàng".  
4. Hệ thống hiển thị hộp thoại xác nhận hủy đơn, bao gồm:  
   - Cảnh báo: "Bạn có chắc chắn muốn hủy đơn hàng [Mã đơn hàng]? Hành động này không thể hoàn tác".  
   - Danh sách lý do hủy (Dropdown): "Thay đổi ý định mua sắm", "Muốn đổi sản phẩm/biến thể khác", "Tìm thấy giá tốt hơn ở nơi khác", "Nhập sai thông tin giao hàng", "Lý do khác".  
   - Ô nhập chi tiết lý do (nếu chọn lý do khác).  
5. Khách hàng chọn lý do và nhấp nút "Xác nhận hủy đơn".  
6. Hệ thống thực hiện hủy đơn hàng trong cơ sở dữ liệu:  
   - Chuyển trạng thái đơn hàng sang `Đã hủy (Cancelled)`.  
   - Cập nhật người hủy là `Khách hàng (Customer)` kèm lý do hủy và thời gian hủy.  
7. Hệ thống tự động kích hoạt chức năng giải phóng tồn kho giữ chỗ (Release Stock - liên kết UC-087) cho toàn bộ các sản phẩm của đơn hàng.  
8. Hệ thống ghi nhật ký sự kiện vào Activity Log.  
9. Hệ thống hiển thị thông báo thành công (MSG80: "Đơn hàng của bạn đã được hủy thành công!") và cập nhật giao diện đơn hàng sang trạng thái `Đã hủy`.  

**Alternative Flows**  
- **Step 2.1_Đơn hàng vừa được nhân viên xác nhận ngay trước lúc bấm hủy:**  
  1. Trong lúc khách hàng đang mở trang, nhân viên cửa hàng vừa duyệt đơn sang trạng thái `Confirmed`.  
  2. Khách hàng nhấp "Hủy đơn hàng".  
  3. Hệ thống kiểm tra trạng thái thời gian thực và phát hiện đơn không còn ở `Unconfirmed`.  
  4. Hệ thống chặn thao tác tự hủy và hiển thị thông báo (MSG81: "Đơn hàng đã được nhân viên cửa hàng tiếp nhận và xác nhận. Bạn không thể tự hủy trực tiếp. Vui lòng gửi yêu cầu hủy đơn để được hỗ trợ").  
  5. Hệ thống hiển thị nút chuyển sang chức năng Yêu cầu hủy đơn đã xác nhận (UC-045).  
- **Khách hàng đổi ý không muốn hủy:**  
  1. Tại hộp thoại xác nhận hủy, khách hàng nhấp "Không, giữ lại đơn hàng".  
  2. Hệ thống đóng hộp thoại và giữ nguyên trạng thái đơn hàng.  

**Postconditions**  
1. Đơn hàng chuyển sang trạng thái `Cancelled`.  
2. Toàn bộ số lượng tồn kho giữ chỗ được giải phóng về trạng thái khả dụng cho khách hàng khác mua.  

---

### UC-045 Yêu cầu hủy/thay đổi đơn đã xác nhận (Request Cancellation/Change)
**Primary Actors**  
Customer  

**Secondary Actors**  
Email Service  

**Description**  
As a Customer, I want to submit a request to cancel or modify an order that has already been confirmed by the store so that staff can review the logistics progress and safely process my request.  

**Preconditions**  
1. Khách hàng đã đăng nhập.  
2. Đơn hàng mục tiêu đang ở trạng thái `Đã xác nhận (Confirmed)` hoặc `Đang chuẩn bị hàng (Processing)`, nhưng chưa chuyển sang trạng thái `Đang giao hàng (Shipping)`.  

**Normal Flow**  
*Gửi yêu cầu can thiệp đối với đơn hàng đã được cửa hàng xác nhận*  
1. Khách hàng mở trang Chi tiết đơn hàng.  
2. Nhận thấy đơn hàng đã ở trạng thái `Confirmed` hoặc `Processing`, hệ thống hiển thị nút "Yêu cầu thay đổi / Hủy đơn".  
3. Khách hàng nhấp vào nút "Yêu cầu thay đổi / Hủy đơn".  
4. Hệ thống hiển thị biểu mẫu "Gửi yêu cầu thay đổi đơn hàng":  
   - Chọn loại yêu cầu: (1) "Yêu cầu hủy đơn hoàn toàn"; (2) "Yêu cầu thay đổi địa chỉ nhận hàng/SĐT"; (3) "Yêu cầu đổi món trong đơn".  
   - Ô nhập nội dung chi tiết yêu cầu và lý do (bắt buộc).  
5. Khách hàng chọn loại yêu cầu, nhập lý do chi tiết và nhấp nút "Gửi yêu cầu".  
6. Hệ thống kiểm tra nội dung lý do không được để trống (tối thiểu 10 ký tự).  
7. Hệ thống tạo một bản ghi yêu cầu hỗ trợ đơn hàng liên kết với đơn hàng mục tiêu, gán cờ `HasPendingChangeRequest = True` trên đơn hàng để cảnh báo nhân viên kho/bán hàng tạm hoãn xuất kho.  
8. Hệ thống gọi Secondary Actor `Email Service` gửi thông báo khẩn tới bộ phận quản lý đơn hàng của cửa hàng.  
9. Hệ thống hiển thị thông báo thành công (MSG82: "Yêu cầu của bạn đã được gửi tới cửa hàng. Nhân viên phụ trách sẽ kiểm tra tiến độ kho và liên hệ lại với bạn sớm nhất").  
10. Giao diện đơn hàng hiển thị biểu ngữ (Banner): "Đơn hàng đang có một yêu cầu hủy/thay đổi đang chờ nhân viên xử lý".  

**Alternative Flows**  
- **Đơn hàng đã xuất kho và đang trên đường giao (In Transit):**  
  1. Đơn hàng đã ở trạng thái `Đang giao hàng (Shipping)`.  
  2. Nút gửi yêu cầu bị ẩn hoặc vô hiệu hóa. Hệ thống hiển thị hướng dẫn (MSG83: "Đơn hàng đã được xuất kho và đang trên đường giao hàng. Vui lòng liên hệ trực tiếp hotline cửa hàng hoặc làm việc với Shipper khi nhận hàng").  
- **Đã có một yêu cầu đang chờ xử lý trước đó:**  
  1. Khách hàng đã gửi một yêu cầu trước đó và nhân viên chưa xử lý xong.  
  2. Hệ thống thông báo (MSG84: "Đơn hàng này đã có một yêu cầu đang chờ xử lý. Bạn không thể gửi thêm yêu cầu mới lúc này").  

**Postconditions**  
1. Yêu cầu hủy/thay đổi đơn hàng được ghi nhận vào hệ thống.  
2. Đơn hàng được gắn cờ cảnh báo để nhân viên bán hàng tiếp nhận xử lý (kích hoạt UC-062).  

---
