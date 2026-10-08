# PHÂN HỆ FE-04: AI TƯ VẤN LỰA CHỌN SẢN PHẨM (AI ADVISOR)

---

### UC-031 Trò chuyện với AI tư vấn (Chat with AI Advisor)
**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
OpenAI  

**Description**  
As a Customer or Guest, I want to chat with an AI Advisor about my bathroom/sanitary ware needs (space dimensions, budget, design style, technical specs) so that I can receive instant, tailored product recommendations and technical guidance.  

**Preconditions**  
Người dùng truy cập vào website cửa hàng thiết bị vệ sinh và nhấp vào biểu tượng Widget Trò chuyện AI ở góc dưới bên phải màn hình.  

**Normal Flow**  
*Tương tác và trò chuyện tự nhiên với Chatbot AI tư vấn thiết bị vệ sinh*  
1. Người dùng nhấp vào biểu tượng "AI Tư vấn thiết bị vệ sinh" (Chatbot Widget).  
2. Hệ thống hiển thị khung chat với lời chào thân thiện kèm các gợi ý nhanh (ví dụ: "Tư vấn thiết bị cho phòng tắm nhỏ 3m²", "Chọn combo bồn cầu và sen tắm tầm giá 15 triệu", "Tư vấn sen tắm cây nhiệt độ cho gia đình có trẻ nhỏ").  
3. Người dùng nhập câu hỏi hoặc nhu cầu vào ô chat (ví dụ: "Nhà tôi có phòng tắm 4m2, đường nước cách sàn 15cm, ngân sách khoảng 20 triệu, muốn lắp bồn cầu thông minh và sen tắm đứng, shop tư vấn giúp").  
4. Người dùng nhấp nút "Gửi" (hoặc nhấn phím Enter).  
5. Hệ thống hiển thị câu hỏi của người dùng trên giao diện chat và kích hoạt trạng thái "AI đang suy nghĩ và phân tích...".  
6. Hệ thống xây dựng ngữ cảnh truy vấn (Context Enrichment):  
   - Kết hợp câu hỏi người dùng với danh mục sản phẩm, thông số kỹ thuật, giá bán và tình trạng tồn kho từ cơ sở dữ liệu của cửa hàng.  
   - Gửi yêu cầu phân tích và sinh phản hồi tới Secondary Actor `OpenAI` qua API an toàn.  
7. Secondary Actor `OpenAI` xử lý ngữ cảnh, đưa ra lời tư vấn chuyên môn phù hợp và trả về phản hồi kèm danh sách các mã sản phẩm tương ứng.  
8. Hệ thống hiển thị câu trả lời hoàn chỉnh của AI lên khung chat kèm các thẻ tóm tắt sản phẩm (Product Cards) có hình ảnh, giá và nút "Xem chi tiết".  
9. Hệ thống tự động lưu lịch sử tin nhắn của phiên chat vào cơ sở dữ liệu.  
10. Người dùng tiếp tục trò chuyện hoặc chọn xem chi tiết sản phẩm.  

**Alternative Flows**  
- **Step 6.1_Dịch vụ OpenAI gặp sự cố hoặc vượt thời gian chờ (Timeout):**  
  1. API của OpenAI không phản hồi sau 10 giây hoặc trả về lỗi dịch vụ.  
  2. Hệ thống bắt lỗi ngoại lệ và hiển thị phản hồi dự phòng thông minh (MSG64: "Hệ thống AI đang phản hồi chậm do quá tải. Bạn có thể để lại thông tin để nhân viên cửa hàng tư vấn trực tiếp qua Zalo/Điện thoại ngay nhé!").  
  3. Hệ thống hiển thị nút gợi ý "Gửi yêu cầu nhân viên tư vấn" (UC-033).  
- **Step 3.1_Người dùng nhập câu hỏi ngoài phạm vi nghiệp vụ cửa hàng:**  
  1. Người dùng hỏi các chủ đề không liên quan (chính trị, giải trí, thời sự, v.v.).  
  2. AI nhận diện và phản hồi lịch sự: "Xin lỗi quý khách, em là trợ lý chuyên tư vấn thiết bị vệ sinh và phòng tắm của cửa hàng. Quý khách cần hỗ trợ thêm thông tin gì về kích thước, mẫu mã thiết bị vệ sinh không ạ?".  
- **Step 3.2_Người dùng chưa đăng nhập (Guest) tham gia trò chuyện:**  
  1. Người dùng trò chuyện với tư cách Khách vãng lai.  
  2. Hệ thống tạo phiên chat tạm thời gắn với Cookie / Local Storage của trình duyệt để lưu lại tin nhắn trong vòng 24 giờ.  

**Postconditions**  
1. Câu trả lời của AI và danh sách sản phẩm gợi ý được hiển thị cho người dùng.  
2. Toàn bộ đoạn hội thoại được lưu vào bảng lịch sử chat.  

---

### UC-032 Xem sản phẩm gợi ý từ AI (View AI-recommended Products)
**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
None  

**Description**  
As a Customer or Guest, I want to view detailed product cards recommended by the AI Advisor within the chat window so that I can inspect their specifications, check stock availability, and directly add them to my shopping cart.  

**Preconditions**  
Người dùng đang trong phiên trò chuyện với AI và AI vừa phản hồi gợi ý một danh sách sản phẩm phù hợp.  

**Normal Flow**  
*Xem và tương tác với các sản phẩm được AI đề xuất*  
1. Trên giao diện khung chat AI, người dùng xem danh sách các thẻ sản phẩm (Product Carousel / Cards) do AI trả về. Mỗi thẻ hiển thị: Ảnh sản phẩm, Tên sản phẩm, Mã SKU, Giá niêm yết, Trạng thái còn hàng, và Đánh giá sao.  
2. Người dùng nhấp vào một thẻ sản phẩm cụ thể để xem tóm tắt thông số kỹ thuật (ví dụ: kích thước xả, chất liệu men, lưu lượng nước).  
3. Người dùng có thể thực hiện một trong các thao tác:  
   - Nhấp vào nút "Xem chi tiết trên website": Hệ thống mở trang chi tiết sản phẩm đầy đủ trong tab mới (UC-029).  
   - Nhấp vào nút "Thêm vào giỏ hàng": Hệ thống kích hoạt chức năng thêm sản phẩm vào giỏ hàng ngay lập tức (UC-038).  
4. Hệ thống ghi nhận tương tác nhấp chọn sản phẩm gợi ý của người dùng vào dữ liệu đo lường hiệu quả AI (AI Analytics Log).  

**Alternative Flows**  
- **Sản phẩm gợi ý vừa hết hàng trong kho:**  
  1. Sản phẩm khi AI gợi ý có tồn kho khả dụng bằng 0.  
  2. Trên thẻ sản phẩm hiển thị nhãn màu cam "Hết hàng tạm thời".  
  3. Nút "Thêm vào giỏ hàng" chuyển thành nút "Liên hệ đặt hàng" hoặc "Gửi yêu cầu tư vấn sản phẩm tương đương".  

**Postconditions**  
Người dùng tiếp cận được thông tin sản phẩm mục tiêu hoặc đưa sản phẩm vào quy trình mua hàng.  

---

### UC-033 Gửi yêu cầu tư vấn chuyên sâu (Send Consultation Request)
**Primary Actors**  
Customer, Guest  

**Secondary Actors**  
Email Service  

**Description**  
As a Customer or Guest, I want to submit a detailed consultation request form (contact details, building site address, design blueprint/photos, specific requirements) so that a human sales staff can directly contact and assist me.  

**Preconditions**  
1. Người dùng đang ở trên website hoặc đang trò chuyện với AI và có nhu cầu được nhân viên hỗ trợ chuyên sâu (cho công trình hoặc phòng tắm phức tạp).  

**Normal Flow**  
*Khách hàng gửi biểu mẫu yêu cầu nhân viên tư vấn trực tiếp*  
1. Người dùng nhấp vào nút "Yêu cầu tư vấn chuyên sâu" (trong khung chat AI hoặc trên thanh menu chính).  
2. Hệ thống hiển thị biểu mẫu "Đăng ký tư vấn thiết bị phòng tắm & công trình":  
   - Họ và tên khách hàng (nếu đã đăng nhập thì tự động điền).  
   - Số điện thoại liên hệ (tự động điền nếu đã đăng nhập).  
   - Địa chỉ công trình / Tỉnh/Thành phố.  
   - Loại công trình (Nhà phố, Biệt thự, Chung cư, Khách sạn, Cải tạo nhà cũ).  
   - Ngân sách dự kiến (Dưới 20 triệu, 20-50 triệu, Trên 50 triệu).  
   - Nội dung yêu cầu chi tiết.  
   - Tệp đính kèm (ảnh hiện trạng phòng tắm, bản vẽ thiết kế nếu có).  
3. Người dùng điền đầy đủ các thông tin và tải lên tệp ảnh (nếu có).  
4. Người dùng nhấp nút "Gửi yêu cầu tư vấn".  
5. Hệ thống kiểm tra dữ liệu: Họ tên không để trống, Số điện thoại đúng 10 chữ số hợp lệ.  
6. Hệ thống tạo một bản ghi yêu cầu tư vấn mới với trạng thái `Chờ tiếp nhận (Pending)` và mã yêu cầu định dạng `TV-XXXXX`.  
7. Nếu có tệp đính kèm, hệ thống lưu tệp lên kho lưu trữ đám mây.  
8. Hệ thống gọi Secondary Actor `Email Service` gửi thông báo cho Ban quản lý/Bộ phận bán hàng của cửa hàng về yêu cầu tư vấn mới.  
9. Hệ thống hiển thị thông báo thành công (MSG65: "Gửi yêu cầu tư vấn thành công! Mã yêu cầu của bạn là TV-XXXXX. Chuyên viên của chúng tôi sẽ liên hệ lại trong vòng 30 phút") và cung cấp liên kết theo dõi tiến độ.  

**Alternative Flows**  
- **Step 5_Số điện thoại không hợp lệ:**  
  1. Người dùng nhập số điện thoại sai định dạng.  
  2. Hệ thống hiển thị cảnh báo (MSG22: "Số điện thoại không hợp lệ").  
  3. Người dùng nhập lại số điện thoại chính xác.  
- **Step 3.1_Người dùng gửi yêu cầu kế thừa từ đoạn chat AI:**  
  1. Người dùng nhấp nút gửi yêu cầu ngay trong luồng chat AI sau khi AI tư vấn dở dang.  
  2. Hệ thống tự động trích xuất tóm tắt nhu cầu và danh sách sản phẩm AI đã đề xuất đưa vào phần nội dung ghi chú yêu cầu tư vấn.  
  3. Người dùng chỉ cần bổ sung Số điện thoại và gửi đi.  

**Postconditions**  
1. Bản ghi yêu cầu tư vấn mới được lưu vào hệ thống ở trạng thái `Pending`.  
2. Đội ngũ bán hàng nhận được thông báo để xử lý tiếp nhận (UC-034).  

---

### UC-034 Tiếp nhận và xử lý yêu cầu tư vấn (Handle Consultation Request)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
SMS Service, Email Service  

**Description**  
As a Sales Staff or Store Owner, I want to receive, assign, and process customer consultation requests so that I can provide phone/Zalo support, update request progress, and convert leads into sales quotes or orders.  

**Preconditions**  
1. Nhân viên bán hàng hoặc Chủ cửa hàng đã đăng nhập vào hệ thống quản trị.  
2. Có ít nhất một yêu cầu tư vấn mới đang ở trạng thái `Pending` trong danh sách.  

**Normal Flow**  
*Nhân viên bán hàng tiếp nhận và xử lý yêu cầu tư vấn của khách hàng*  
1. Nhân viên truy cập vào mục "Quản lý yêu cầu tư vấn" trên thanh điều hướng.  
2. Hệ thống hiển thị danh sách các yêu cầu tư vấn chưa phân công hoặc đang xử lý.  
3. Nhân viên nhấp chọn một yêu cầu tư vấn cụ thể và nhấp nút "Tiếp nhận xử lý".  
4. Hệ thống cập nhật trạng thái yêu cầu sang `Đang xử lý (In Progress)` và gán nhân viên phụ trách là nhân viên hiện tại.  
5. Hệ thống hiển thị toàn bộ hồ sơ yêu cầu: Thông tin khách hàng, Nhu cầu, Loại công trình, Bản vẽ đính kèm, và Tóm tắt lịch sử chat AI (nếu có).  
6. Nhân viên liên hệ với khách hàng qua Điện thoại hoặc Zalo để tư vấn giải pháp thiết bị vệ sinh.  
7. Sau khi trao đổi, nhân viên quay lại hệ thống nhập kết quả tư vấn vào ô "Ghi chú xử lý" (ví dụ: "Đã gọi điện tư vấn combo Inax 25 triệu, khách hẹn gửi báo giá chi tiết qua Zalo vào sáng mai").  
8. Nhân viên chọn cập nhật trạng thái mới cho yêu cầu:  
   - `Đã tư vấn - Chờ phản hồi` (Follow-up required).  
   - `Đã chốt - Chuyển sang báo giá/đơn hàng` (Kích hoạt UC-134).  
   - `Hủy/Không liên lạc được` (kèm nhập lý do).  
9. Nhân viên nhấp nút "Lưu kết quả tư vấn".  
10. Hệ thống lưu toàn bộ tiến trình xử lý, thời gian và ghi chú vào lịch sử yêu cầu.  
11. Hệ thống gửi thông báo cập nhật tiến độ cho khách hàng qua Email/SMS (nếu có cấu hình).  
12. Hệ thống hiển thị thông báo thành công (MSG66: "Cập nhật tiến trình tư vấn thành công!").  

**Alternative Flows**  
- **Khách hàng không nghe máy (Call Unanswered):**  
  1. Nhân viên gọi cho khách hàng nhưng không liên lạc được.  
  2. Nhân viên ghi nhận trạng thái: "Không nghe máy lần 1" và lên lịch hệ thống nhắc gọi lại sau 2 giờ.  
  3. Hệ thống lưu lịch hẹn nhắc nhở vào bảng công việc của nhân viên.  
- **Khách hàng từ chối nhu cầu (Cancelled Lead):**  
  1. Khách hàng thông báo đã mua nơi khác hoặc không còn nhu cầu.  
  2. Nhân viên chuyển trạng thái sang `Đóng - Khách hủy nhu cầu` và ghi rõ lý do.  
  3. Hệ thống lưu trữ yêu cầu để phục vụ thống kê tỷ lệ chuyển đổi.  

**Postconditions**  
1. Trạng thái yêu cầu tư vấn và thông tin nhân viên xử lý được cập nhật trong hệ thống.  
2. Khách hàng có thể theo dõi được tiến trình phản hồi từ cửa hàng.  

---

### UC-035 Xem lịch sử trò chuyện AI (View AI Chat History)
**Primary Actors**  
Customer  

**Secondary Actors**  
None  

**Description**  
As a Customer, I want to review my previous chat sessions with the AI Advisor within my personal account so that I can look back at recommended products and advice without re-asking questions.  

**Preconditions**  
Khách hàng đã đăng nhập vào tài khoản cá nhân trên website.  

**Normal Flow**  
*Khách hàng xem lại các cuộc hội thoại tư vấn đã lưu với AI*  
1. Khách hàng truy cập trang tài khoản cá nhân và chọn mục "Lịch sử tư vấn AI" (AI Chat History).  
2. Hệ thống truy vấn và hiển thị danh sách các phiên trò chuyện trước đây của khách hàng, mỗi phiên gồm: Thời gian bắt đầu, Tiêu đề tóm tắt do AI sinh ra (ví dụ: "Tư vấn bồn cầu 1 khối TOTO", "Combo phòng tắm 10m2"), và Số lượng tin nhắn.  
3. Khách hàng nhấp chọn một phiên trò chuyện cụ thể.  
4. Hệ thống tải toàn bộ nội dung trao đổi chi tiết của phiên chat đó: các câu hỏi của khách hàng, câu trả lời của AI và các sản phẩm đã được gợi ý trong phiên đó.  
5. Khách hàng xem lại thông tin hoặc nhấp vào nút "Tiếp tục trò chuyện" để gửi thêm câu hỏi mới dựa trên ngữ cảnh phiên chat cũ.  

**Alternative Flows**  
- **Chưa có lịch sử trò chuyện nào:**  
  1. Khách hàng chưa từng sử dụng tính năng trò chuyện AI.  
  2. Hệ thống hiển thị thông báo (MSG67: "Bạn chưa có đoạn trò chuyện nào với AI. Bấm Trò chuyện ngay để nhận tư vấn thiết bị vệ sinh miễn phí!").  
- **Xóa một phiên trò chuyện:**  
  1. Khách hàng nhấp vào biểu tượng thùng rác tại một phiên trò chuyện.  
  2. Hệ thống hiển thị xác nhận xóa: "Bạn có chắc chắn muốn xóa lịch sử đoạn trò chuyện này?".  
  3. Khách hàng nhấp "Xác nhận". Hệ thống đánh dấu xóa mềm phiên chat đó và làm mới danh sách.  

**Postconditions**  
Lịch sử trao đổi với AI được hiển thị đầy đủ, an toàn và riêng tư cho khách hàng.  

---

### UC-036 Xem danh sách yêu cầu tư vấn (View Consultation Request List)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to view, search, and filter all customer consultation requests by status, assigned staff, date, and project budget so that requests can be prioritized and allocated efficiently.  

**Preconditions**  
Người dùng đã đăng nhập với vai trò `Sales Staff` hoặc `Store Owner`.  

**Normal Flow**  
*Xem danh sách các yêu cầu tư vấn của khách hàng*  
1. Người dùng chọn menu "Yêu cầu tư vấn" trong hệ thống quản trị.  
2. Hệ thống tải và hiển thị trang danh sách yêu cầu tư vấn với cấu trúc dạng bảng:  
   - Bộ lọc: Ô tìm kiếm (Tên KH, SĐT, Mã yêu cầu), Lọc theo Trạng thái (`Chờ tiếp nhận`, `Đang xử lý`, `Đã tư vấn`, `Đã chốt đơn`, `Đã hủy`), Lọc theo Nhân viên phụ trách, Lọc theo Khoảng thời gian.  
   - Bảng danh sách: Mã yêu cầu, Họ tên khách, Số điện thoại, Địa điểm công trình, Ngân sách dự kiến, Thời gian gửi, Người phụ trách, Trạng thái (Huy hiệu màu sắc), Thao tác (Xem chi tiết, Nhận xử lý).  
   - Thống kê tóm tắt trên đầu trang: Số lượng yêu cầu mới chờ xử lý trong ngày, Số lượng yêu cầu đang quá hạn tiếp nhận (trên 30 phút).  
3. Người dùng chọn các tiêu chí lọc cần xem và duyệt danh sách.  
4. Người dùng nhấp vào một yêu cầu để xem chi tiết hoặc thực hiện tiếp nhận (UC-034).  

**Alternative Flows**  
- **Lọc không có kết quả:**  
  1. Tiêu chí lọc không trả về yêu cầu nào.  
  2. Hệ thống hiển thị thông báo "Không có yêu cầu tư vấn nào phù hợp" kèm nút làm mới bộ lọc.  

**Postconditions**  
Danh sách yêu cầu tư vấn hiển thị đầy đủ, giúp nhân viên không bỏ sót khách hàng tiềm năng.  

---

### UC-037 Theo dõi trạng thái yêu cầu tư vấn (Track My Consultation Request)
**Primary Actors**  
Customer  

**Secondary Actors**  
None  

**Description**  
As a Customer, I want to track the status and feedback of my consultation requests in my personal account so that I know who is handling my inquiry and when they will contact me.  

**Preconditions**  
Khách hàng đã đăng nhập và đã từng gửi ít nhất một yêu cầu tư vấn trên hệ thống.  

**Normal Flow**  
*Khách hàng theo dõi tiến độ xử lý yêu cầu tư vấn của mình*  
1. Khách hàng truy cập trang tài khoản cá nhân và chọn mục "Yêu cầu tư vấn của tôi".  
2. Hệ thống hiển thị danh sách các yêu cầu tư vấn của khách hàng cùng tiến độ trực quan:  
   - Mã yêu cầu, Ngày gửi, Ngân sách và Nhu cầu đã đăng ký.  
   - Thanh tiến trình trạng thái trực quan: `Đã tiếp nhận` -> `Đang phân tích & liên hệ` -> `Đã hoàn tất tư vấn`.  
   - Thông tin nhân viên chuyên trách hỗ trợ (Họ tên nhân viên, Số hotline hỗ trợ).  
   - Các ghi chú phản hồi công khai từ phía nhân viên cửa hàng.  
3. Khách hàng nhấp vào mã yêu cầu để xem lại đầy đủ thông tin chi tiết đã gửi.  

**Alternative Flows**  
- **Khách hàng chưa đăng nhập tra cứu qua mã yêu cầu và SĐT:**  
  1. Khách hàng vãng lai truy cập trang tra cứu công khai `https://store.com/tra-cuu-tu-van`.  
  2. Người dùng nhập Mã yêu cầu (TV-XXXXX) và Số điện thoại liên hệ đã đăng ký.  
  3. Hệ thống đối soát thông tin: Nếu khớp, hiển thị tiến độ và trạng thái xử lý; nếu không khớp, báo lỗi (MSG68: "Thông tin mã yêu cầu hoặc số điện thoại không chính xác").  

**Postconditions**  
Khách hàng nắm rõ tiến độ xử lý yêu cầu tư vấn của mình từ cửa hàng.  

---

### UC-134 Chuyển yêu cầu tư vấn thành báo giá/đơn hàng (Convert Consultation to Quote/Order)
**Primary Actors**  
Sales Staff, Store Owner  

**Secondary Actors**  
None  

**Description**  
As a Sales Staff or Store Owner, I want to inherit customer info and recommended product lists directly from a completed consultation request to instantly create a sales quote or sales order without manual re-typing.  

**Preconditions**  
1. Nhân viên đã đăng nhập và đang mở chi tiết một yêu cầu tư vấn đã được tiếp nhận.  
2. Khách hàng đã thống nhất danh mục sản phẩm và phương án thiết bị vệ sinh.  

**Normal Flow**  
*Kế thừa dữ liệu từ yêu cầu tư vấn sang báo giá hoặc đơn hàng*  
1. Tại màn hình chi tiết yêu cầu tư vấn, nhân viên nhấp vào nút "Chuyển thành Báo giá/Đơn hàng" (Convert to Quote/Order).  
2. Hệ thống hiển thị hộp thoại lựa chọn hình thức chuyển đổi:  
   - Lựa chọn 1: "Tạo Báo giá công trình" (Create Project Quote - liên kết UC-065).  
   - Lựa chọn 2: "Tạo Đơn hàng bán" (Create Sales Order - liên kết UC-048).  
3. Nhân viên chọn hình thức mong muốn (ví dụ: "Tạo Báo giá công trình") và nhấp "Tiếp tục".  
4. Hệ thống tự động khởi tạo màn hình tạo Báo giá/Đơn hàng mới với các trường dữ liệu được tự động điền sẵn (Auto-fill):  
   - Tên khách hàng, Số điện thoại, Địa chỉ giao hàng/công trình.  
   - Danh sách các sản phẩm và biến thể đã được khách hàng lựa chọn trong quá trình tư vấn.  
   - Ghi chú kỹ thuật từ yêu cầu tư vấn được chuyển thành Ghi chú đơn hàng.  
5. Nhân viên kiểm tra, điều chỉnh số lượng, áp dụng chính sách chiết khấu (nếu có) và nhấp nút "Lưu báo giá/đơn hàng".  
6. Hệ thống tạo thành công Báo giá/Đơn hàng mới với mã tham chiếu liên kết ngược lại Mã yêu cầu tư vấn gốc.  
7. Hệ thống tự động cập nhật trạng thái của Yêu cầu tư vấn gốc sang `Đã chuyển đổi thành công (Converted)`.  
8. Hệ thống hiển thị thông báo thành công (MSG69: "Đã chuyển yêu cầu tư vấn thành Báo giá [BG-XXXXX] thành công!").  

**Alternative Flows**  
- **Sản phẩm trong yêu cầu tư vấn đã ngừng kinh doanh hoặc hết hàng:**  
  1. Khi hệ thống kế thừa danh sách sản phẩm, phát hiện một mặt hàng đang bị vô hiệu hóa hoặc không còn bán.  
  2. Hệ thống hiển thị cảnh báo (MSG70: "Sản phẩm [Tên sản phẩm] hiện đã ngừng kinh doanh hoặc hết hàng. Vui lòng chọn sản phẩm thay thế trước khi lưu").  
  3. Nhân viên đổi sang sản phẩm tương đương trên màn hình đơn hàng và tiếp tục lưu.  

**Postconditions**  
1. Báo giá hoặc Đơn hàng mới được tạo lập nhanh chóng với độ chính xác cao.  
2. Yêu cầu tư vấn được đóng với trạng thái chuyển đổi thành công.  

---
