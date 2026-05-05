# 03. Luồng người dùng và tình huống ngoại lệ

## Actor chính

- `Passenger`: khách Việt Nam hoặc quốc tế.
- `Travel group leader`: người đặt cho nhóm.
- `Ops agent`: nhân viên vận hành/concierge.
- `Customer support`: CSKH đa ngôn ngữ.
- `Transport provider`: Grab/Xanh/Be/taxi/xe hợp đồng/nhà xe.
- `Driver`: tài xế thuộc provider hoặc đối tác.
- `Finance admin`: đối soát, hoàn tiền, hóa đơn.
- `System worker`: queue worker xử lý booking/ticket/payment/webhook.

## Luồng 1: Khách quốc tế đi khách sạn tại TP.HCM

1. Khách quét QR tại arrival hoặc mở link PWA.
2. App tự chọn ngôn ngữ theo browser, cho đổi ngôn ngữ thủ công.
3. Khách nhập tên khách sạn.
4. App dùng Places API để tìm khách sạn và yêu cầu khách xác nhận địa chỉ.
5. App hỏi số người, số hành lý, nhu cầu xe 4/7 chỗ, trẻ em, người già.
6. App đề xuất phương án:
    - Fastest available ride.
    - Fixed-price airport transfer.
    - Premium/VIP.
    - EV/taxi nếu có.
7. Khách chọn phương án và phương thức thanh toán.
8. Booking được tạo với trạng thái `REQUESTED`.
9. Queue gửi job `provider.search_and_book`.
10. Provider trả kết quả xe/tài xế hoặc ops agent đặt hộ.
11. App hiển thị biển số, tên tài xế, pickup zone, hướng dẫn đi bộ.
12. Khách nhấn `I am at pickup point`.
13. Tài xế/ops xác nhận gặp khách.
14. Trip bắt đầu, app hiển thị tracking nếu có.
15. Trip hoàn tất, app gửi receipt/invoice request, đánh giá.

Ngoại lệ:

- Nếu khách sạn trùng tên: app bắt khách chọn địa chỉ trên bản đồ.
- Nếu provider không có xe: chuyển sang provider khác hoặc ops gọi hãng taxi.
- Nếu khách không có mạng: PWA cache trang hướng dẫn pickup + hotline.
- Nếu tài xế không nói tiếng Anh: chat dịch tự động qua template/AI.
- Nếu khách đổi điểm đến: tạo `CHANGE_DESTINATION_REQUEST`, tính lại giá nếu provider hỗ trợ.

## Luồng 2: 50 khách cùng xuống một chuyến bay

Vấn đề: nhiều request cùng lúc, provider API có rate limit, tài xế thiếu, ops quá tải.

Thiết kế:

1. Mỗi booking request vào Redis/BullMQ queue.
2. Queue chia job theo provider, priority, pickup time.
3. Rate limiter bảo vệ provider API/portal.
4. Worker xử lý song song nhưng có giới hạn concurrency.
5. Nếu provider A hết xe, job fallback sang provider B/C.
6. Admin thấy board theo chuyến bay/pickup zone.
7. Hệ thống gom thông tin nhưng không tự ghép khách trừ khi pháp lý cho phép.
8. Với đoàn, tạo group booking để điều phối nhiều xe hoặc xe lớn.

Trạng thái cần có:

- `QUEUED`: đã nhận request.
- `SEARCHING_PROVIDER`: đang tìm provider.
- `WAITING_OPS`: cần người can thiệp.
- `CONFIRMED`: đã có xe.
- `FAILED_NO_SUPPLY`: hết xe/tất cả provider fail.
- `CUSTOMER_CANCELLED`: khách hủy.

## Luồng 3: Khách muốn đi tỉnh gần

Ví dụ Long Thành -> Vũng Tàu/Đà Lạt/Mũi Né.

1. Khách nhập thành phố/tỉnh.
2. App nhận diện đây là liên tỉnh.
3. App hiển thị phương án:
    - Xe riêng door-to-door.
    - Limousine/xe khách nếu có tuyến phù hợp.
    - Xe ra bến/điểm đón + vé xe.
4. App hỏi số người, hành lý, giờ muốn đi, có thể chờ bao lâu.
5. Nếu chọn xe riêng: điều phối provider xe hợp đồng/taxi liên tỉnh.
6. Nếu chọn xe khách: tìm vé qua Vexere/đối tác/ops.
7. Nếu cần trung chuyển: tạo itinerary nhiều chặng.
8. Khách thanh toán một đơn hoặc từng chặng tùy provider.
9. App nhắc giờ, pickup, điểm đón/trả.

Ngoại lệ:

- Không có chuyến xe khách phù hợp: đề xuất xe riêng hoặc nghỉ tại TP.HCM.
- Hành lý quá nhiều: lọc xe/nhà xe hỗ trợ hành lý lớn.
- Khách cần invoice: chỉ hiển thị provider hỗ trợ chứng từ hoặc đưa vào assisted flow.

## Luồng 4: Khách muốn đi Đà Nẵng ngay

Đây là luồng “tư vấn hành trình”, không nên chỉ đặt xe.

1. App nhận diện khoảng cách xa.
2. App hỏi: “Bạn muốn đi nhanh nhất, rẻ nhất, hay ít chuyển nhất?”
3. Hệ thống so sánh:
    - Flight nội địa gần nhất.
    - Xe/taxi đến sân bay khác hoặc điểm nối chuyến.
    - Tàu/xe khách nếu hợp lý.
    - Nghỉ qua đêm + di chuyển hôm sau.
4. AI/rule engine tạo itinerary nháp.
5. Ops agent duyệt itinerary trước khi thu tiền.
6. App cho khách xác nhận từng chặng.

Checklist cần chốt:

- [ ] Có bán vé máy bay/tàu/xe trong app không?
- [ ] Có cho AI tự đặt vé không hay luôn cần ops duyệt?
- [ ] Ai chịu trách nhiệm nếu khách lỡ chuyến nối tiếp?
- [ ] Có chính sách hoàn/hủy theo từng provider không?

## Luồng 5: Khách đoàn

1. Trưởng đoàn nhập số khách, số hành lý, flight number.
2. Chọn loại dịch vụ: nhiều xe 7 chỗ, bus/van, VIP pickup.
3. App tạo group booking.
4. Ops agent xác nhận thủ công.
5. Hệ thống tạo nhiều sub-booking hoặc một contract booking.
6. Mỗi sub-booking có xe/tài xế riêng.
7. Trưởng đoàn nhận một link theo dõi toàn bộ xe.
8. Ops có checklist từng khách/xe đã lên.

Ngoại lệ:

- Flight delay: hệ thống cập nhật ETA nếu tích hợp flight API hoặc ops nhập tay.
- Thiếu xe: gọi thêm provider dự phòng.
- Khách bị lạc: share location/pickup zone/hotline.

## Luồng 6: Khách chỉ muốn hướng dẫn đường

Không phải ai cũng muốn đặt xe qua app.

1. Khách nhập đích đến.
2. App hiển thị:
    - Đi bằng taxi/xe công nghệ.
    - Đi bus/shuttle nếu có.
    - Ra bến xe/ga nào.
    - Ước tính chi phí/thời gian.
3. App cho chọn “Tự đi” hoặc “Đặt giúp tôi”.

Giá trị:

- Tăng trust.
- Giảm áp lực phải đặt qua app.
- Có thể chuyển đổi sau khi khách thấy lộ trình phức tạp.

## Luồng 7: Thanh toán, hoàn tiền, hóa đơn

1. Khách chọn phương thức thanh toán.
2. App tạo `payment_intent`.
3. Nếu thanh toán thẻ quốc tế: dùng Stripe/payment gateway nếu pháp nhân và quốc gia hỗ trợ.
4. Nếu VietQR: tạo QR động và chờ webhook xác nhận.
5. Nếu provider thu tiền trực tiếp: app chỉ ghi `PAY_AT_PROVIDER`.
6. Nếu booking fail sau khi thu tiền: tạo refund request.
7. Nếu khách cần hóa đơn: thu thông tin invoice trước hoặc sau chuyến.

Ngoại lệ:

- Thanh toán thành công nhưng provider booking fail: auto refund hoặc ops chuyển provider khác.
- Webhook thanh toán chậm: trạng thái `PAYMENT_PENDING_RECONCILIATION`.
- Khách nhập sai MST/email: cho sửa trước khi xuất hóa đơn.

## Luồng 8: Hủy/đổi chuyến

Các rule cần có:

- Khách hủy trước khi provider nhận: miễn phí.
- Khách hủy sau khi provider nhận: theo chính sách provider.
- Tài xế hủy: fallback provider, giữ booking id cũ nhưng tạo provider order mới.
- Flight delay: nếu khách nhập flight number, hệ thống tự nhắc ops giữ/đổi xe.
- No-show: ops đánh dấu và áp phí theo chính sách đã công bố.

## Luồng 9: CSKH đa ngôn ngữ

Kênh:

- In-app chat.
- WhatsApp/Zalo/Email.
- Hotline.

Mẫu sự cố:

- “I cannot find the driver.”
- “The driver cannot find me.”
- “I want to change destination.”
- “My flight is delayed.”
- “I paid but no car was assigned.”
- “I need invoice.”
- “I left luggage in the car.”

AI nên làm:

- Dịch tin nhắn.
- Gợi ý template trả lời.
- Tóm tắt case cho ops.
- Không tự hứa hoàn tiền hoặc thay đổi giá nếu chưa có rule.
