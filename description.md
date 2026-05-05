bên mình đang sắp có dự án làm app đặt xe cho khách đi từ sân bay long thành đặt xe về nhà. nghiên cứu mấy cái tech stack công nghệ ng ta đang dùng hiện tại cho mấy app như grab rồi làm 1 bản báo cáo gửi anh nha.

1. mô tả:

- là dự án trọng điểm quốc gia tại Đồng Nai, cách TP.HCM khoảng 40km, hướng tới trở thành trung tâm trung chuyển hàng không lớn nhất Việt Nam và khu vực. Dự án có diện tích 5.000ha, công suất thiết kế 100 triệu hành khách/năm, Giảm tải cho sân bay Tân Sơn Nhất, là cửa ngõ hàng không lớn nhất Việt Nam.
- về phục vụ cho chuyến bay thế nào: chủ yếu phục vụ các chuyến bay quốc tế (khoảng 80%), bao gồm các tuyến bay quốc tế đến/đi từ TP.HCM, và khoảng 10-20% chuyến bay nội địa.
- về khách hàng: chủ yếu phục vụ hành khách quốc tế, quá cảnh và các chuyến bay nội địa đường dài. Sân bay giúp giảm tải cho Tân Sơn Nhất, phục vụ hành khách đi/đến vùng kinh tế trọng điểm phía Nam.

phân tích cẩn thận, khai thác hoàn toàn dự án này, ví dụ như phân tích xem nhu cầu khách hàng thế nào (ví dụ như khách nước ngoài muốn đi xung quanh Tp Hồ Chí Minh, hoặc đến khách sạn, hoặc họ muốn đi tỉnh khác như Đà Nẵng)

2.  phân tích về tech stack, về bản đồ

- phân tích về tính năng hệ thống: như có xe hay không, có cần quản lý xe không? hay chỉ quản lý tuyến đường. hay cần liên kết với các nhà cung cấp xe (ví dụ khách nước ngoài đi Đà Nẵng, thì hệ thống cung cấp xe, hay liên kết với nhà xe, hay là chỉ giới thiệu xe đó cho khách, hay là chỉ cần vạch lộ trình cho khách là đi đến bến xe rồi... -> hoặc là kết hợp? -> hỏi sếp)
- Progressive Web Apps (tìm hiểu thêm): truy cập website như một ứng dụng, quan trọng với khách quốc tế và giảm rất nhiều phức tạp: ví dụ như muốn đăng app lên appstore nước ngoài cần tuân thủ theo từng nước,...

3. phân tích thêm nhu cầu khách hàng:

- họ muốn biết trước tài xế thì sao? tiếng Anh thì sao? thanh toán qua thẻ quốc tế? họ muốn xuất hoá đơn?
- Sân bay long thành rất rộng - 5000ha, nếu được thì có thêm tính năng định vị trong diện tích nhỏ, để khách và tài xế dễ gặp nhau.
- Ví dụ khách muốn đi Đà Nẵng luôn (có thể gặp vấn đề là không có chuyến đúng giờ đó - có thể khách phải chờ - thêm các vấn đề về di chuyển đến bến xe -> đặt grab hộ) - thì có thể cho khách chọn vé luôn (khách sẽ thanh toán trước để mua vé - mình sẽ dùng agents hoặc n8n để chọn vé phù hợp và mua cho khách rồi trả vé cho khách)

4. về vấn đề của mình:

- mình thuê tài xế giúp họ hay sao? (grab, xe khách, xe dịch vụ,...)
- quá nhiều khách dùng cùng 1 lúc thì sao? ví dụ 50 khách cùng xuống máy bay và cùng muốn mình đặt grab hộ họ (nếu mình muốn tự chủ làm giống grab thì sẽ gặp khó khăn pháp lý bên dưới) -> vấn đề là làm sao mình có thể đặt giúp họ 50 xe grab trong cùng lúc?
- đồng thời nếu như mình dùng các dịch vụ có sẵn như grab thì sẽ không phải quản lý và tính toán rất nhiều thứ khác như: map, đường đi ngắn nhất, tính toán chi phí, quản lý xe, app cho tài xế,... -> chỉ cần đặt xe giúp họ là được.
- vấn đề về chính xác: ví dụ khách muốn đi từ long thành -> khách sạn A -> mình đặt hộ grab lỡ nó đến khách sạn A nhưng nhầm thì sao?
- sau khi tìm hiểu thì Hiện Grab có Partner API (Grab for Business) - nhưng cần đăng kí. cái này có thể rất phù hợp với dự án này, vì là tính năng bắt buộc (khách muốn đến khách sạn, cần phải ra bến xe trước,...), dùng grab có thể giảm rất nhiều vấn đề phức tạp + chi phí.

5. khó khăn pháp lý:

- nếu muốn tự chủ trong việc quản lý xe cần có giấy phép kinh doanh vận tải, nên hướng tiếp cận đặt xe hộ qua grab, vin,... là tốt hơn
- Cần giấy phép kinh doanh vận tải nếu muốn Tự quản lý tài xế, xe, định tuyến.

6. dịch vụ thêm:

- có thêm các dịch vụ đi kèm như: SIM 4G, đổi tiền,...

7. Luồng:

- khách xuống xe, mở app, nhập nơi muốn đi.
- muốn đến khách sạn -> đạt grab hộ
- muốn đến tỉnh khác? nghiên cứu thêm, tuỳ tỉnh, có thể là cho khách chọn vé xe, mua vé xe cho khách, đặt grab cho khách ra bến xe.

8. tech stack tham khảo
   Frontend (App khách)
   React Native (Expo)
   ├── Vừa iOS, Android, Web (PWA)
   ├── Không cần cài app nếu dùng PWA → quan trọng với khách quốc tế
   └── Mapbox SDK (bản đồ trong nhà sân bay - hỗ trợ indoor map)
   Backend
   Node.js + NestJS
   ├── DB nghiên cứu thêm
   ├── Redis (cache, queue, session)
   ├── BullMQ (xử lý job queue - giải bài 50 người cùng lúc)
   └── WebSocket (tracking realtime)
   Bản đồ & Định vị
   ├── Google Maps Platform (routing, places API)
   ├── Mapbox (indoor positioning - trong sân bay)
   Tích hợp bên thứ ba
   ├── Grab Partner API → đặt xe hộ
   ├── Be API → phương án dự phòng
   ├── VietQR / Stripe → thanh toán
   ├── OpenAI / GPT → chatbot hỗ trợ đa ngôn ngữ
   ├── n8n / automation → tự động mua vé xe, máy bay cho khách
   └── ViettelPost / Vexere API / Phuong Trang → đặt vé xe khách
   AI & Automation
   ├── n8n: orchestrate workflow mua vé tự động
   ├── Claude API / GPT-4: dịch thuật realtime trong chat tài xế-khách
   └── RAG chatbot: hỏi đáp về sân bay, tuyến đường, dịch vụ
