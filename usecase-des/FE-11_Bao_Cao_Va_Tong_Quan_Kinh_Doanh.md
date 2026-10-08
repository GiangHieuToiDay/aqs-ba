# PHÂN HỆ FE-11: BÁO CÁO & TỔNG QUAN KINH DOANH (BUSINESS ANALYTICS & DASHBOARD)

---

### UC-114 Xem Business Dashboard (View Business Dashboard)
**Primary Actors**  
Store Owner  

**Secondary Actors**  
OpenAI  

**Description**  
As a Store Owner, I want to view an executive Business Dashboard displaying real-time revenue, operating expenses, estimated net profit, pending orders, and inventory valuation so that I can monitor store performance at a glance.  

**Preconditions**  
Chủ cửa hàng đã đăng nhập thành công vào hệ thống quản trị với vai trò `Store Owner`.  

**Normal Flow**  
*Xem bảng điều khiển tổng quan tình hình kinh doanh thời gian thực*  
1. Chủ cửa hàng truy cập vào trang chủ Quản trị (màn hình Dashboard mặc định).  
2. Hệ thống tổng hợp số liệu kinh doanh thời gian thực và hiển thị các khối KPI thẻ số (Metric Cards):  
   - **Doanh thu thuần hôm nay**: Tổng tiền bán hàng từ các đơn hoàn tất và thu tiền cọc.  
   - **Tổng chi phí vận hành**: Tổng chi phí ghi nhận trong ngày (mặt bằng, điện nước, vận chuyển...).  
   - **Lợi nhuận gộp ước tính**: Doanh thu trừ Giá vốn hàng bán (COGS) và Chi phí.  
   - **Đơn hàng chờ xử lý**: Số lượng đơn online chưa duyệt và đơn đang chờ xuất kho.  
   - **Tổng giá trị tồn kho**: Tổng vốn đang lưu chuyển trong kho tính theo giá nhập.  
   - **Công nợ phải thu**: Tổng số tiền khách hàng còn nợ cửa hàng.  
3. Phía dưới hiển thị biểu đồ trực quan (Charts):  
   - Biểu đồ đường (Line chart): Diễn biến doanh thu 7 ngày hoặc 30 ngày gần nhất.  
   - Biểu đồ tròn (Donut chart): Tỷ trọng doanh thu theo Kênh bán (Tại quầy vs Online vs Dự án công trình).  
   - Khối widget tóm tắt nhanh: Top 5 sản phẩm bán chạy nhất tuần và Khối Cảnh báo vận hành khẩn cấp (UC-123).  
4. Hệ thống gọi Secondary Actor `OpenAI` sinh 1 đoạn tóm tắt kinh doanh thông minh (AI Insight Summary): "Doanh thu tuần này tăng 15% so với tuần trước nhờ sức bật của nhóm Sen vòi Inax. Tồn kho bồn cầu TOTO đang chạm ngưỡng tối thiểu, cần nhập hàng sớm".  
5. Chủ cửa hàng theo dõi và tương tác với các biểu đồ để nắm bắt tình hình.  

**Alternative Flows**  
- **Dữ liệu chưa có phát sinh giao dịch mới:**  
  1. Ngày đầu kỳ hoặc ngày lễ chưa phát sinh đơn.  
  2. Các chỉ số hiển thị giá trị 0 hoặc so sánh % giữ nguyên.  
- **Lỗi kết nối OpenAI sinh nhận định:**  
  1. OpenAI không phản hồi sau 5 giây.  
  2. Khối AI Insight ẩn đi, bảng số liệu biểu đồ kinh doanh vẫn hiển thị bình thường.  

**Postconditions**  
Các chỉ số kinh doanh cốt lõi được tính toán và trình bày trực quan, chính xác.  

---

### UC-115 Lọc báo cáo theo thời gian (Filter Report by Period)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to filter report metrics across predefined timeframes (Today, Yesterday, This Week, This Month, This Quarter, This Year) or custom date ranges so that I can analyze performance over specific periods.  

**Preconditions**  
Người dùng đang ở trên màn hình Business Dashboard (UC-114) hoặc bất kỳ trang báo cáo chi tiết nào trong phân hệ.  

**Normal Flow**  
*Thay đổi khung thời gian để lọc lại toàn bộ số liệu báo cáo*  
1. Người dùng nhấp vào bộ chọn thời gian (Date Range Picker) ở góc trên bên phải màn hình báo cáo.  
2. Hệ thống hiển thị menu lựa chọn:  
   - Lựa chọn nhanh: `Hôm nay`, `Hôm qua`, `7 ngày qua`, `Tuần này`, `Tháng này`, `Tháng trước`, `Quý này`, `Năm nay`.  
   - Tùy chọn khoảng ngày tùy chỉnh: Lịch chọn `Từ ngày` và `Đến ngày`.  
3. Người dùng chọn một khoảng thời gian mong muốn (ví dụ: "Tháng trước" hoặc từ `01/09/2026` đến `30/09/2026`).  
4. Người dùng nhấp nút "Áp dụng".  
5. Hệ thống kiểm tra: Ngày bắt đầu <= Ngày kết thúc.  
6. Hệ thống thực thi truy vấn tính toán lại toàn bộ các thẻ KPI, biểu đồ doanh thu, cơ cấu chi phí và danh sách sản phẩm bán chạy theo đúng khoảng thời gian đã chọn.  
7. Giao diện báo cáo tự động làm mới toàn bộ số liệu trong vòng dưới 1 giây.  

**Alternative Flows**  
- **Khoảng ngày tùy chỉnh không hợp lệ:**  
  1. Người dùng chọn ngày bắt đầu lớn hơn ngày kết thúc.  
  2. Hệ thống cảnh báo (MSG61: "Khoảng thời gian không hợp lệ. Ngày bắt đầu phải nhỏ hơn ngày kết thúc").  
  3. Hệ thống giữ nguyên khoảng thời gian cũ.  

**Postconditions**  
Toàn bộ dữ liệu của báo cáo phản ánh chính xác kết quả kinh doanh trong khung thời gian được lọc.  

---

### UC-116 Xem báo cáo Doanh thu, Chi phí & Lãi/Lỗ (View Profit Report)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to review the comprehensive Profit and Loss (P&L) report detailing gross revenue, cost of goods sold, operating expenses, and net profit so that I can evaluate business viability.  

**Preconditions**  
Người dùng đã đăng nhập với vai trò `Store Owner` hoặc `Accountant`.  

**Normal Flow**  
*Xem báo cáo tài chính lãi lỗ chi tiết*  
1. Người dùng chọn menu "Báo cáo" -> "Báo cáo Doanh thu & Lãi/Lỗ" (P&L Report).  
2. Hệ thống hiển thị bảng báo cáo tài chính chuẩn gồm các mục kết cấu:  
   - **(1) Doanh thu bán hàng gộp**: Tổng tiền hàng bán ra trước chiết khấu.  
   - **(2) Giảm trừ doanh thu**: Tiền chiết khấu khuyến mãi + Tiền hoàn trả khi đổi/trả hàng.  
   - **(3) Doanh thu thuần**: (1) - (2).  
   - **(4) Giá vốn hàng bán (COGS)**: Tổng giá trị nhập kho của số hàng đã xuất bán.  
   - **(5) Lợi nhuận gộp**: (3) - (4) (Kèm tỷ suất lợi nhuận gộp %).  
   - **(6) Chi phí hoạt động**: Bảng phân bổ các khoản chi vận hành (Chi phí mặt bằng, Điện nước, Lương thưởng, Phí ship, Tiếp khách, Marketing...).  
   - **(7) Lợi nhuận thuần trước thuế (Lãi/Lỗ ròng)**: (5) - (6).  
3. Người dùng xem bảng phân tích chi tiết và đối chiếu các khoản mục.  
4. Người dùng có thể nhấp vào từng khoản mục chi phí để xem danh sách chứng từ cấu thành (liên kết UC-125).  

**Alternative Flows**  
- **Doanh nghiệp bị lỗ trong kỳ:**  
  1. Chi phí vận hành vượt quá lợi nhuận gộp.  
  2. Chỉ số Lợi nhuận thuần hiển thị số âm với màu đỏ nổi bật kèm cảnh báo để chủ cửa hàng rà soát cắt giảm chi phí.  

**Postconditions**  
Báo cáo lãi/lỗ được tính toán chính xác, chuẩn mực và minh bạch.  

---

### UC-117 So sánh doanh thu theo kênh (View Revenue by Channel)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to compare revenue distribution across sales channels (In-Store Counter POS, Online Website, Construction Project Contracts) so that I can optimize channel investments.  

**Preconditions**  
Người dùng đang ở phân hệ Báo cáo kinh doanh.  

**Normal Flow**  
*Xem phân tích so sánh tỷ trọng doanh thu giữa các kênh bán hàng*  
1. Người dùng chọn mục "Báo cáo theo kênh bán hàng" (Revenue by Channel).  
2. Hệ thống hiển thị bảng và biểu đồ so sánh đa chiều giữa 3 kênh:  
   - Kênh Tại quầy (Counter Sales / POS).  
   - Kênh Trực tuyến (Online Website).  
   - Kênh Bán công trình / Dự án (Project Contractors).  
3. Dữ liệu so sánh bao gồm:  
   - Số lượng đơn hàng của từng kênh.  
   - Giá trị đơn hàng trung bình (AOV - Average Order Value).  
   - Tổng doanh thu và Tỷ lệ phần trăm đóng góp vào doanh số tổng (%).  
   - Tỷ lệ đổi trả hàng của từng kênh.  
4. Biểu đồ cột chồng và biểu đồ tròn thể hiện trực quan sự dịch chuyển doanh thu giữa các kênh qua các tháng.  
5. Người dùng xem phân tích để điều chỉnh chính sách ưu đãi cho kênh tiềm năng.  

**Alternative Flows**  
- **Một kênh chưa phát sinh đơn:**  
  1. Kênh dự án chưa có hợp đồng nào trong tháng.  
  2. Kênh đó hiển thị doanh thu 0 VNĐ và chiếm 0% tỷ trọng.  

**Postconditions**  
Bức tranh doanh thu đa kênh được hiển thị rõ nét, hỗ trợ chiến lược kinh doanh.  

---

### UC-118 Xem sản phẩm bán chạy (View Best-selling Products)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to view top-selling products and variants by sales volume and revenue so that we can prioritize inventory purchasing and display layouts.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống.  

**Normal Flow**  
*Xem bảng xếp hạng Top sản phẩm bán chạy nhất*  
1. Người dùng chọn mục "Báo cáo sản phẩm bán chạy" (Top Best-Sellers).  
2. Hệ thống truy vấn và hiển thị bảng xếp hạng sản phẩm:  
   - Bộ lọc: Xếp hạng theo "Số lượng bán ra" hoặc "Tổng doanh thu mang lại".  
   - Bộ lọc theo Danh mục: `Tất cả`, `Bồn cầu`, `Chậu lavabo`, `Sen tắm`, `Vòi chậu`, `Phụ kiện`.  
   - Bảng danh sách Top 10 / Top 20 sản phẩm: Ảnh sản phẩm, Tên thiết bị, Mã SKU, Danh mục, Số lượng đã bán, Tổng doanh thu, Lợi nhuận đóng góp, Tồn kho hiện tại.  
3. Hệ thống hiển thị huy hiệu vinh danh Top 1, Top 2, Top 3 kèm biểu đồ thanh ngang so sánh trực quan.  
4. Người dùng xem phân tích và có thể nhấp trực tiếp vào mã SKU để xem biến động kho (UC-082) hoặc tạo đề xuất nhập thêm hàng (UC-138) nếu tồn kho đang thấp.  

**Alternative Flows**  
- **Sản phẩm bán chạy sắp hết hàng (Stockout Risk):**  
  1. Sản phẩm nằm trong Top 3 bán chạy nhưng tồn kho khả dụng chỉ còn dưới 5 cái.  
  2. Cột Tồn kho hiển thị cảnh báo đỏ nhấp nháy: "Nguy cơ đứt hàng - Cần nhập ngay!".  

**Postconditions**  
Danh sách mặt hàng chủ lực được nhận diện chính xác, giúp tối ưu hóa vốn lưu động.  

---

### UC-119 Thống kê trạng thái đơn hàng (View Order Status Stats)
**Primary Actors**  
Store Owner, Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Sales Staff, I want to view statistical distributions of orders by status (Pending, Shipping, Completed, Cancelled, Returned) and identify bottleneck stages with slow processing.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý.  

**Normal Flow**  
*Xem thống kê luồng đơn hàng và phát hiện điểm nghẽn vận hành*  
1. Người dùng chọn mục "Thống kê đơn hàng" (Order Pipeline Analytics).  
2. Hệ thống hiển thị phễu xử lý đơn hàng (Order Funnel) và biểu đồ phân bổ trạng thái:  
   - Tỷ lệ đơn hoàn tất thành công (Fulfillment Rate, ví dụ: 94%).  
   - Tỷ lệ đơn bị hủy (Cancellation Rate, ví dụ: 4%).  
   - Tỷ lệ đổi trả (Return Rate, ví dụ: 2%).  
   - Thời gian xử lý trung bình từ lúc khách đặt đến khi đơn được xác nhận (Average Lead Time).  
   - Thời gian trung bình từ xuất kho đến khi giao thành công.  
3. Bảng cảnh báo đơn hàng bị ngâm lâu (Overdue Pending Orders): Liệt kê các đơn hàng đã ở trạng thái Chờ xác nhận quá 2 giờ hoặc Đang chuẩn bị hàng quá 24 giờ mà chưa đi giao.  
4. Người dùng kiểm tra danh sách đơn chậm trễ và đôn đốc nhân viên liên quan giải quyết.  

**Alternative Flows**  
- **Không có đơn hàng nào bị chậm tiến độ:**  
  1. Mọi đơn hàng đều được xử lý đúng hạn cam kết SLA.  
  2. Bảng cảnh báo hiển thị nhãn xanh: "Vận hành hoàn hảo! Không có đơn hàng nào bị quá hạn".  

**Postconditions**  
Các rủi ro tắc nghẽn trong khâu xử lý đơn hàng được phát hiện và xử lý kịp thời.  

---

### UC-120 Xem báo cáo tồn kho (View Inventory Report)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to view comprehensive inventory valuation reports, stock turnover rates, and dead stock items so that inventory carrying costs can be minimized.  

**Preconditions**  
Người dùng đã đăng nhập và có quyền xem báo cáo kho.  

**Normal Flow**  
*Xem báo cáo tổng hợp giá trị kho và hàng tồn lâu ngày*  
1. Người dùng chọn mục "Báo cáo tồn kho & Giá trị vốn" (Inventory Report).  
2. Hệ thống tải dữ liệu kho và hiển thị các số liệu tổng quan:  
   - **Tổng giá trị vốn kho**: Tổng tiền vốn lưu kho theo giá nhập trung bình.  
   - **Tổng số lượng SKU**: Số biến thể đang quản lý tồn kho.  
   - **Số mặt hàng dưới định mức**: Các sản phẩm đang chạm ngưỡng tồn tối thiểu.  
   - **Hàng tồn kho chậm luân chuyển (Dead Stock)**: Danh sách các sản phẩm không phát sinh đơn hàng bán ra trong hơn 90 ngày.  
3. Bảng dữ liệu chi tiết gồm: Tên thiết bị, SKU, Danh mục, Tồn thực tế, Tồn giữ chỗ, Tồn khả dụng, Đơn giá vốn, Tổng giá trị tồn, Số ngày lưu kho trung bình.  
4. Người dùng lọc theo nhóm hàng tồn lâu để lên kế hoạch xả hàng giảm giá hoặc thương lượng trả hàng lại cho nhà cung cấp (UC-194).  

**Alternative Flows**  
- **Xuất bảng báo cáo ra file Excel:**  
  1. Người dùng nhấp nút "Xuất file kiểm kê". Hệ thống xuất toàn bộ bảng dữ liệu ra file `.xlsx`.  

**Postconditions**  
Giá trị tài sản hàng hóa trong kho được kiểm soát chuẩn xác đến từng đồng vốn.  

---

### UC-121 Xem báo cáo công nợ (View Debt Report)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to view aging debt reports for both customers and suppliers so that accounts receivable can be recovered and accounts payable are scheduled properly.  

**Preconditions**  
Người dùng đã đăng nhập với vai trò `Store Owner` hoặc `Accountant`.  

**Normal Flow**  
*Xem báo cáo phân tích tuổi nợ công nợ phải thu và phải trả*  
1. Người dùng chọn mục "Báo cáo công nợ tổng hợp" (Debt Aging Report).  
2. Hệ thống hiển thị 2 tab báo cáo:  
   - Tab 1: **Công nợ phải thu của Khách hàng**:  
     + Tổng dư nợ hiện tại.  
     + Phân loại theo tuổi nợ (Aging Buckets): Trong hạn (0-30 ngày), Quá hạn 1-15 ngày, Quá hạn 16-30 ngày, Quá hạn trên 30 ngày (Nợ xấu).  
     + Bảng chi tiết: Tên khách hàng/Chủ thầu, SĐT, Số tiền nợ, Hạn nợ gần nhất, Nút nhắc nợ (UC-147).  
   - Tab 2: **Công nợ phải trả Nhà cung cấp**:  
     + Tổng số tiền cửa hàng còn nợ các hãng/nhà phân phối thiết bị vệ sinh.  
     + Danh sách các khoản sắp đến hạn thanh toán trong 7 ngày tới để chuẩn bị dòng tiền.  
3. Người dùng rà soát các khoản nợ quá hạn của khách hàng và yêu cầu nhân viên bán hàng đôn đốc thu hồi nợ (UC-057).  

**Alternative Flows**  
- **Không có khoản nợ quá hạn:**  
  1. Toàn bộ khách hàng thanh toán đúng hạn.  
  2. Cột nợ quá hạn hiển thị 0 VNĐ với biểu tượng màu xanh an toàn.  

**Postconditions**  
Dòng tiền và rủi ro công nợ của cửa hàng được theo dõi chặt chẽ.  

---

### UC-122 Xuất báo cáo ra file (Export Report)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to export report tables to Excel (.xlsx) or PDF formats with applied filters so that data can be archived, presented to partners, or analyzed offline.  

**Preconditions**  
Người dùng đang ở trên bất kỳ màn hình báo cáo số liệu nào trong phân hệ FE-11.  

**Normal Flow**  
*Xuất dữ liệu báo cáo ra file định dạng Excel hoặc PDF*  
1. Sau khi áp dụng bộ lọc thời gian và tiêu chí mong muốn, người dùng nhấp vào nút "Xuất báo cáo" (Export) trên thanh công cụ.  
2. Hệ thống hiển thị menu lựa chọn định dạng xuất:  
   - `Xuất bảng tính Excel (.xlsx)`: Chứa dữ liệu bảng thô đầy đủ công thức tính tổng và tiêu đề cột.  
   - `Xuất tài liệu PDF (.pdf)`: Chứa bản in trình bày trang nhã có logo cửa hàng, biểu đồ và chữ ký phê duyệt.  
3. Người dùng nhấp chọn định dạng (ví dụ: `Excel (.xlsx)`).  
4. Hệ thống tạo tài liệu trên máy chủ, đóng gói toàn bộ các bản ghi đang lọc và gửi luồng dữ liệu (Data Stream) về trình duyệt.  
5. Trình duyệt tự động tải tệp tin xuống thư mục Downloads của người dùng với tên chuẩn: `Bao_Cao_[TenBaoCao]_[TuNgay]_[DenNgay].xlsx`.  
6. Hệ thống hiển thị thông báo thành công (MSG129: "Xuất dữ liệu báo cáo thành công!").  

**Alternative Flows**  
- **Dữ liệu báo cáo quá lớn (> 50.000 dòng):**  
  1. Báo cáo kéo dài nhiều năm có lượng bản ghi khổng lồ.  
  2. Hệ thống xử lý xuất nền (Background Export) và gửi thông báo kèm đường dẫn tải tệp khi hoàn thành sau ít phút.  

**Postconditions**  
Tệp báo cáo hoàn chỉnh được lưu về máy tính của người dùng để lưu trữ hoặc xử lý ngoại tuyến.  

---

### UC-123 Xem cảnh báo vận hành (View Operational Alerts)
**Primary Actors**  
Store Owner, Sales Staff, Accountant  

**Secondary Actors**  
None  

**Description**  
As an internal user, I want to view operational alerts on the dashboard (abnormal stock drains, overdue orders, past-due customer debts, suspicious price changes) so that anomalies are resolved immediately.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống.  

**Normal Flow**  
*Xem danh sách các cảnh báo rủi ro vận hành thời gian thực*  
1. Người dùng nhìn thấy biểu tượng Chuông cảnh báo (Alert Bell) trên thanh tiêu đề có huy hiệu số lượng cảnh báo đỏ nhấp nháy, hoặc xem khối "Cảnh báo vận hành" trực tiếp trên Dashboard.  
2. Người dùng nhấp vào biểu tượng Chuông cảnh báo.  
3. Hệ thống mở danh sách các cảnh báo được phân loại theo mức độ nghiêm trọng:  
   - **Mức Khẩn cấp (Đỏ)**: Có 3 mặt hàng bồn cầu bán chạy bị hết sạch tồn kho; Có 2 đơn hàng online bị treo quá 4 tiếng chưa ai nhận.  
   - **Mức Cảnh báo (Vàng)**: Khách hàng nhà thầu [Tên KH] có khoản nợ 45 triệu đã quá hạn thanh toán 5 ngày; Nhân viên vừa điều chỉnh giảm giá biến thể > 20%.  
   - **Mức Thông tin (Xanh)**: Ca bán hàng sáng vừa kết thúc và đã đối soát khớp tiền két.  
4. Người dùng nhấp vào một dòng cảnh báo cụ thể.  
5. Hệ thống điều hướng thẳng người dùng tới màn hình xử lý của sự cố tương ứng (Deep-linking).  

**Alternative Flows**  
- **Đánh dấu đã xử lý cảnh báo:**  
  1. Sau khi xem xét sự cố, người dùng bấm nút "Đã kiểm tra & Đóng cảnh báo".  
  2. Hệ thống ẩn cảnh báo đó khỏi danh sách chưa đọc.  

**Postconditions**  
Các rủi ro kinh doanh và tắc nghẽn quy trình được thông báo tức thì đến người quản lý.  

---

### UC-124 Ghi nhận khoản chi phí (Record Expense)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
Cloudinary  

**Description**  
As a Store Owner or Accountant, I want to record an operational expense (rent, electricity, water, warehouse transport, petty office supplies, marketing) with attached receipt photos so that operating expenses are deducted from gross profit.  

**Preconditions**  
Người dùng đã đăng nhập với vai trò `Store Owner` hoặc `Accountant`.  

**Normal Flow**  
*Nhập phiếu ghi nhận chi phí vận hành cửa hàng*  
1. Người dùng chọn mục "Quản lý chi phí vận hành" -> Nhấp nút "Tạo khoản chi mới" (New Expense).  
2. Hệ thống hiển thị biểu mẫu "Ghi nhận chi phí":  
   - Mã phiếu chi: Tự động sinh `EXP-YYYYMMDD-XXXXX`.  
   - Nhóm chi phí (Dropdown): `Tiền thuê mặt bằng & Kho bãi`, `Điện nước & Internet`, `Vận chuyển & Bốc vác thuê ngoài`, `Văn phòng phẩm & Tiêu hao`, `Chi phí tiếp khách & Bán hàng`, `Lương & Phụ cấp`, `Chi phí khác`.  
   - Số tiền chi (VNĐ): Nhập số tiền hợp lệ > 0.  
   - Ngày phát sinh chi phí: Mặc định là ngày hôm nay.  
   - Phương thức chi: `Tiền mặt từ két quầy` hoặc `Chuyển khoản ngân hàng`.  
   - Người nhận tiền / Đơn vị cung cấp dịch vụ.  
   - Nội dung chi tiết khoản chi.  
   - Đính kèm ảnh hóa đơn/chứng từ biên nhận (Tùy chọn).  
3. Người dùng nhập đầy đủ thông tin và tải lên ảnh hóa đơn (nếu có).  
4. Người dùng nhấp nút "Lưu khoản chi".  
5. Hệ thống kiểm tra dữ liệu: Số tiền > 0, Nhóm chi phí và Nội dung không để trống.  
6. (Nếu có ảnh): Hệ thống gọi `Cloudinary` lưu trữ ảnh hóa đơn.  
7. Hệ thống lưu bản ghi khoản chi vào bảng `OperationalExpenses`.  
8. Hệ thống tự động tính trừ khoản tiền này vào Sổ quỹ tiền mặt/Tài khoản ngân hàng tương ứng và cập nhật Báo cáo Lãi/Lỗ (UC-116).  
9. Hệ thống ghi nhận sự kiện vào Audit Log.  
10. Hệ thống hiển thị thông báo thành công (MSG130: "Ghi nhận chi phí vận hành thành công!").  

**Alternative Flows**  
- **Số tiền chi không hợp lệ (<= 0 hoặc để trống):**  
  1. Người dùng nhập số tiền bằng 0 hoặc âm.  
  2. Hệ thống báo lỗi (MSG131: "Số tiền chi phải là số dương lớn hơn 0").  

**Postconditions**  
Khoản chi được ghi nhận chính thức, phản ánh vào dòng tiền chi và báo cáo lãi lỗ của cửa hàng.  

---

### UC-125 Xem danh sách khoản chi (View Expense List)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to view, search, and filter operational expenses by category, payment method, date range, and creator so that expenditure is fully transparent.  

**Preconditions**  
Người dùng đã đăng nhập vào hệ thống quản lý tài chính.  

**Normal Flow**  
*Xem danh sách các khoản chi phí vận hành đã ghi nhận*  
1. Người dùng chọn mục "Danh sách khoản chi" (Expense List).  
2. Hệ thống tải và hiển thị bảng dữ liệu các khoản chi:  
   - Bộ lọc: Lọc theo Khoảng thời gian, Lọc theo Nhóm chi phí, Lọc theo Phương thức thanh toán (Tiền mặt / Chuyển khoản), Lọc theo Người tạo phiếu.  
   - Bảng danh sách: Mã phiếu chi, Ngày chi, Nhóm chi phí, Nội dung chi tiết, Người nhận tiền, Phương thức, Số tiền (VNĐ - in đỏ), Ảnh chứng từ (Icon xem nhanh), Người lập phiếu, Thao tác ("Xem chi tiết", "Sửa", "Xóa").  
   - Thống kê tóm tắt trên đầu: Tổng số tiền đã chi trong khoảng thời gian đang lọc.  
3. Người dùng lọc theo tháng để rà soát chi phí.  

**Alternative Flows**  
- **Không có khoản chi nào trong kỳ:**  
  1. Không phát sinh chi phí trong khoảng ngày chọn.  
  2. Hệ thống hiển thị bảng trống kèm tổng chi 0 VNĐ.  

**Postconditions**  
Danh sách chi phí được hiển thị đầy đủ, minh bạch.  

---

### UC-126 Cập nhật/Xóa khoản chi (Update/Delete Expense)
**Primary Actors**  
Store Owner, Accountant  

**Secondary Actors**  
None  

**Description**  
As a Store Owner or Accountant, I want to edit or soft-delete a mistakenly recorded expense with mandatory reason logging so that accounting records remain reconcilable and auditable.  

**Preconditions**  
1. Người dùng đã đăng nhập với vai trò `Store Owner` hoặc `Accountant`.  
2. Phiếu chi mục tiêu đang tồn tại trong hệ thống và chưa bị khóa sổ kế toán kỳ cũ.  

**Normal Flow**  
*Chỉnh sửa hoặc xóa khoản chi bị nhập sai thông tin*  
1. Tại danh sách khoản chi (UC-125), người dùng nhấp nút "Chỉnh sửa" hoặc "Xóa" tại dòng phiếu chi cần thao tác.  
2. **Nếu chọn Chỉnh sửa:**  
   - Hệ thống mở biểu mẫu cập nhật số tiền, nội dung hoặc phân loại chi phí kèm ô bắt buộc nhập: "Lý do điều chỉnh chi phí".  
   - Người dùng nhập lý do (ví dụ: "Nhập thừa 1 số 0 ở tiền điện nước") và nhấp "Lưu thay đổi".  
   - Hệ thống cập nhật số tiền mới, điều chỉnh lại số dư quỹ và cập nhật báo cáo tài chính.  
3. **Nếu chọn Xóa:**  
   - Hệ thống hiển thị hộp thoại xác nhận xóa kèm ô bắt buộc nhập "Lý do hủy/xóa khoản chi".  
   - Người dùng nhập lý do và nhấp "Xác nhận xóa".  
   - Hệ thống thực hiện xóa mềm (`IsDeleted = True`), hoàn lại số tiền vào số dư quỹ.  
4. Hệ thống ghi vết toàn bộ giá trị trước và sau khi sửa/xóa vào Audit Log (FE-02).  
5. Hệ thống hiển thị thông báo thành công (MSG132: "Cập nhật/Xóa khoản chi thành công!").  

**Alternative Flows**  
- **Không nhập lý do sửa/xóa:**  
  1. Người dùng để trống ô lý do.  
  2. Hệ thống chặn thao tác và cảnh báo (MSG40: "Bắt buộc nhập lý do để phục vụ kiểm toán tài chính").  
- **Kỳ kế toán đã bị khóa sổ:**  
  1. Khoản chi thuộc về tháng đã được chủ cửa hàng chốt sổ tài chính chính thức.  
  2. Hệ thống khóa chức năng sửa/xóa và thông báo (MSG133: "Khoản chi này thuộc kỳ kế toán đã khóa sổ, không được phép điều chỉnh trực tiếp").  

**Postconditions**  
1. Khoản chi được cập nhật hoặc xóa mềm khỏi hệ thống.  
2. Số liệu dòng tiền và báo cáo tài chính được tính toán lại chính xác.  

---

### UC-153 Xem báo cáo doanh số cá nhân (View Personal Sales Report)
**Primary Actors**  
Sales Staff  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff, I want to view my individual sales performance metrics (total sales volume, number of orders created, conversion rate, personal commission/KPI target attainment) so that I can track my productivity.  

**Preconditions**  
Nhân viên bán hàng đã đăng nhập vào hệ thống bằng tài khoản của chính mình.  

**Normal Flow**  
*Nhân viên bán hàng theo dõi doanh số và thành tích KPI cá nhân*  
1. Nhân viên bán hàng nhấp chọn mục "Doanh số của tôi" (My Sales Report) trên menu cá nhân.  
2. Hệ thống tự động lọc các đơn hàng do chính tài khoản của nhân viên này tạo lập hoặc trực tiếp chăm sóc chốt đơn thành công trong kỳ (ca trực / tuần / tháng).  
3. Hệ thống hiển thị giao diện báo cáo thành tích cá nhân:  
   - **Tổng doanh số đạt được**: Tổng giá trị các đơn hàng bán thành công (VNĐ).  
   - **Chỉ tiêu KPI được giao**: Mức doanh số mục tiêu của tháng (ví dụ: 200.000.000 VNĐ).  
   - **Tỷ lệ hoàn thành KPI**: Thanh tiến trình % trực quan (ví dụ: Đạt 78% KPI tháng).  
   - **Hoa hồng ước tính**: Tiền thưởng hoa hồng tạm tính theo chính sách thưởng của cửa hàng.  
   - **Số lượng đơn hàng**: Tổng số đơn đã chốt và số lượng đơn bị hủy.  
   - Danh sách các đơn hàng đóng góp vào doanh số trong kỳ.  
4. Nhân viên xem số liệu đối soát để nâng cao hiệu suất bán hàng.  

**Alternative Flows**  
- **Lọc theo các tháng trước:**  
  1. Nhân viên chọn bộ lọc tháng trước để xem lại kết quả đã đạt được.  
  2. Hệ thống tải lại dữ liệu lịch sử tương ứng.  

**Postconditions**  
Dữ liệu doanh số cá nhân được cung cấp minh bạch, chính xác và bảo mật riêng tư cho từng nhân viên.  

---
