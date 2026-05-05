# 11. Pháp lý, rủi ro và compliance

Tài liệu này không thay thế tư vấn luật sư. Đây là danh sách rủi ro để hỏi pháp chế trước khi chốt mô hình.

## Kết luận pháp lý sơ bộ

Nếu công ty tự điều hành xe, tài xế, giá cước, hoặc trực tiếp thực hiện các công đoạn chính của vận tải vì lợi nhuận, rủi ro bị xem là kinh doanh vận tải/đơn vị kết nối vận tải sẽ tăng mạnh. MVP nên đi theo hướng concierge/đặt hộ/marketplace với provider đã có giấy phép, đồng thời soạn hợp đồng và điều khoản rõ.

## Các mô hình và rủi ro

| Mô hình | Rủi ro pháp lý | Ghi chú |
| --- | --- | --- |
| Chỉ tư vấn lộ trình | Thấp | Không thu tiền vận tải, không điều phối xe. |
| Marketplace chuyển sang provider | Thấp-Trung bình | Cần disclaimer và hợp đồng affiliate/partner. |
| Concierge đặt hộ qua provider | Trung bình | Cần xác định app là đại lý/dịch vụ hỗ trợ hay tham gia vận tải. |
| Điều phối xe đối tác, định giá, thu tiền | Trung bình-Cao | Cần luật sư rà theo quy định vận tải và thương mại điện tử. |
| Tự quản lý tài xế/xe | Cao | Cần giấy phép/điều kiện kinh doanh vận tải. |

## Điểm từ quy định vận tải cần chú ý

Theo các nguồn về Nghị định 158/2024/NĐ-CP, phần vận tải taxi/phần mềm tính tiền yêu cầu hiển thị trước chuyến các thông tin như đơn vị vận tải, tài xế, biển số, hành trình, khoảng cách, tổng tiền và kênh phản hồi; sau chuyến có yêu cầu hóa đơn điện tử theo quy định. Nghị định cũng có quy định với đơn vị kết nối vận tải nếu thực hiện công đoạn chính như điều hành xe/tài xế hoặc quyết định giá.

Hệ quả thiết kế:

- Nếu app chỉ đặt qua provider, app vẫn nên hiển thị provider/tài xế/biển số/giá nếu provider cung cấp.
- Nếu app tự quyết định giá và điều phối xe, cần pháp chế xác nhận nghĩa vụ.
- Với xe hợp đồng/đoàn/liên tỉnh, cần hợp đồng/điểm đón/trả/danh sách khách đúng quy định.

## Checklist hỏi luật sư

- [ ] App có được thu tiền từ khách rồi đặt xe qua provider không?
- [ ] Nếu app cộng phí dịch vụ, có cần giấy phép gì thêm không?
- [ ] Nếu app chọn provider và điều phối xe, có bị xem là đơn vị kết nối vận tải không?
- [ ] Nếu app tự đưa giá fixed-price, có bị xem là quyết định giá vận tải không?
- [ ] Luồng xe hợp đồng/xe đoàn cần hợp đồng điện tử và danh sách khách như thế nào?
- [ ] Nếu bán vé xe khách/tàu/flight, cần giấy phép đại lý/OTA gì?
- [ ] Điều kiện đặt quầy/kiosk/QR trong sân bay là gì?
- [ ] Điều khoản trách nhiệm khi provider hủy/no-show/chậm là gì?

## Dữ liệu cá nhân

Dữ liệu có thể thu:

- Tên.
- Số điện thoại/email/WhatsApp.
- Quốc tịch/ngôn ngữ.
- Vị trí pickup/dropoff.
- Flight number.
- Nội dung chat.
- Thông tin hóa đơn.

Nguyên tắc:

- Chỉ thu dữ liệu cần cho booking.
- Nói rõ mục đích dùng dữ liệu.
- Không thu passport nếu không cần.
- Không lưu thông tin thẻ.
- Mask số điện thoại trong admin nếu không cần full.
- Xóa/ẩn dữ liệu sau thời hạn retention.

Checklist:

- [ ] Có privacy policy tiếng Việt/Anh.
- [ ] Có consent cho location.
- [ ] Có consent cho gửi WhatsApp/SMS/email.
- [ ] Có quy trình xóa dữ liệu theo yêu cầu nếu áp dụng.
- [ ] Có phân quyền ops/finance/admin.

## Thanh toán và hoàn tiền

Rủi ro:

- Thanh toán thành công nhưng không có xe.
- Provider thu tiền trực tiếp nhưng app cũng thu phí.
- Chargeback thẻ quốc tế.
- QR transfer sai nội dung/sai amount.
- Refund liên provider phức tạp.

Rule cần công bố:

- Khi nào app thu tiền.
- Khi nào provider thu tiền.
- Khi nào được hủy miễn phí.
- Khi nào bị phí no-show.
- Hoàn tiền qua kênh nào và trong bao lâu.
- Ai xuất hóa đơn cho phần nào.

## Hóa đơn

Cần phân biệt:

- Hóa đơn vận tải của provider.
- Hóa đơn phí dịch vụ/concierge của app.
- Hóa đơn combo nếu app là bên bán.

Checklist:

- [ ] Provider có xuất hóa đơn điện tử không?
- [ ] App có xuất hóa đơn cho phí dịch vụ không?
- [ ] Khách quốc tế cần invoice dạng nào?
- [ ] Có trường MST/công ty/địa chỉ/email nhận hóa đơn không?

## Nội dung hiển thị để giảm rủi ro

Trước khi khách xác nhận:

- Provider hoặc loại provider.
- Giá/fare estimate/fixed.
- Chính sách hủy.
- Trách nhiệm của app.
- Trạng thái “đang chờ xác nhận” nếu chưa có xe.

Không nên viết:

- “Guaranteed car in 5 minutes” nếu không có SLA chắc.
- “We operate all cars” nếu không tự vận hành.
- “Refund immediately” nếu gateway/provider không đảm bảo.

## Rủi ro AI

- AI tư vấn sai itinerary.
- AI dịch sai nội dung quan trọng.
- AI tự tạo cam kết hoàn tiền/chính sách.
- AI xử lý dữ liệu cá nhân quá mức.

Guardrails:

- AI chỉ đề xuất, ops duyệt các case phức tạp.
- Structured output cho itinerary.
- Template cố định cho chính sách/hủy/hoàn tiền.
- Không cho AI gọi payment/refund/provider booking trực tiếp nếu chưa có approval workflow.

