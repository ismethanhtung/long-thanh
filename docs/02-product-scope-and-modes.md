# 02. Phạm vi sản phẩm và mô hình vận hành

## Vấn đề cốt lõi

Brief đặt ra câu hỏi rất quan trọng: “Mình thuê tài xế giúp họ hay sao? Grab, xe khách, xe dịch vụ, hay chỉ vạch lộ trình?”

Đây là quyết định lớn nhất vì nó quyết định pháp lý, chi phí, tốc độ triển khai, rủi ro vận hành và kiến trúc hệ thống.

## Mô hình A: Concierge đặt hộ qua đối tác

App nhận nhu cầu khách, lấy thông tin điểm đến, hành lý, số người, giờ hạ cánh, sau đó đặt xe qua Grab for Business/Concierge, Xanh Business, Be, taxi hãng hoặc điều phối viên.

Phù hợp MVP nhất.

Ưu điểm:

- Đi nhanh, giảm rủi ro giấy phép vận tải.
- Không cần tự quản lý tài xế, xe, tuyến, bảo hiểm vận tải.
- Có thể dùng provider đã có năng lực supply.
- Dễ xử lý đỉnh tải bằng queue + đội vận hành.

Nhược điểm:

- Phụ thuộc API/portal/chính sách provider.
- Biên lợi nhuận có thể thấp hơn.
- SLA phụ thuộc đối tác.
- Nếu không có API thật, đặt 50 xe cùng lúc qua portal có thể vẫn tắc.

Checklist:

- [x] Khuyến nghị làm MVP.
- [ ] Cần hợp đồng doanh nghiệp với Grab/Xanh SM/Be/taxi hãng.
- [ ] Cần xác nhận có được đặt xe cho khách không có tài khoản provider.
- [ ] Cần cơ chế hoàn tiền nếu provider hủy.

## Mô hình B: Marketplace giới thiệu nhà xe/đối tác

App hiển thị nhiều phương án, khách tự chọn và được chuyển sang đối tác để đặt/thanh toán.

Ưu điểm:

- Rủi ro vận tải thấp hơn vì app không trực tiếp bán/chốt dịch vụ vận tải.
- Tích hợp ban đầu đơn giản: deeplink, affiliate, lead form.
- Dễ mở rộng nhà xe, SIM, tour, bảo hiểm.

Nhược điểm:

- Trải nghiệm đứt đoạn.
- App khó kiểm soát trạng thái booking.
- Khó hỗ trợ khách nếu provider xử lý kém.
- Khó đảm bảo pickup trong sân bay.

Checklist:

- [ ] Chọn nếu chưa ký được API vận tải.
- [ ] Cần tracking referral/deeplink để biết khách có đặt thành công không.
- [ ] Cần cảnh báo rõ app chỉ giới thiệu, không chịu trách nhiệm vận tải.

## Mô hình C: Tự vận hành đội xe (có vẻ không hợp lý và phức tạp)

Công ty tự ký tài xế, quản lý xe, định giá, điều phối, thanh toán, hóa đơn.

Ưu điểm:

- Kiểm soát trải nghiệm end-to-end.
- Có thể tối ưu tuyến, giá, dịch vụ VIP.
- Biên lợi nhuận cao hơn nếu đạt quy mô.

Nhược điểm:

- Cần giấy phép/điều kiện kinh doanh vận tải.
- Cần app tài xế, quản lý xe, định tuyến, định giá, an toàn, bảo hiểm.
- Cần đội điều phối 24/7.
- Rủi ro pháp lý và vận hành cao.

Checklist:

- [ ] Không khuyến nghị cho MVP.
- [ ] Chỉ làm khi có pháp nhân vận tải hoặc liên doanh với đơn vị đã có giấy phép.
- [ ] Cần tư vấn pháp lý trước khi thiết kế luồng nhận/chia chuyến/thu tiền.

## Mô hình D: Hybrid theo từng loại nhu cầu

Kết hợp:

- Xe nội thành: đặt hộ qua Grab/Xanh/Be/taxi.
- Xe liên tỉnh: đối tác xe hợp đồng/limousine/nhà xe.
- Vé xe khách/tàu/flight: Vexere/đối tác OTA/n8n assisted booking.
- Dịch vụ phụ: SIM/eSIM, đổi tiền, tour, insurance.
- Tư vấn lộ trình: AI + rule engine + operator xác nhận.

Đây là hướng dài hạn hợp lý.

Checklist:

- [x] Khuyến nghị sau MVP.
- [ ] Cần contract rõ với từng nhóm provider.
- [ ] Cần chuẩn hóa trạng thái booking giữa nhiều provider.
- [ ] Cần một admin ops mạnh để điều phối ngoại lệ.

## Phạm vi MVP đề xuất

MVP nên đủ để hoạt động thật, không chỉ demo:

- PWA web mobile-first, có thể mở qua QR tại sân bay.
- Nhập/chọn điểm đến, số khách, hành lý, ngôn ngữ.
- Gợi ý pickup zone và hướng dẫn gặp xe.
- Hiển thị 2-4 phương án: economy, taxi/EV, fixed airport transfer, assisted intercity.
- Booking request tạo trong hệ thống.
- Thanh toán giữ chỗ hoặc thanh toán sau tùy provider.
- Queue xử lý booking và retry provider.
- Admin dashboard để nhân viên xác nhận/đặt hộ/cập nhật trạng thái.
- CSKH chat/call đa ngôn ngữ.
- Email/WhatsApp/SMS gửi xác nhận.

Không nên đưa vào MVP:

- [ ] Tự động mua vé xe/flight không cần người duyệt.
- [ ] Tự định giá dynamic như Grab.
- [ ] Tự ghép chuyến khách lẻ.
- [ ] App tài xế đầy đủ.
- [ ] Indoor positioning bằng beacon nếu chưa có dữ liệu/hạ tầng sân bay.

## Phase 2

- Native iOS/Android bằng Expo.
- Tích hợp API provider nếu ký được.
- Realtime driver tracking nếu provider cho phép.
- Combo intercity: xe sân bay + vé xe khách.
- RAG chatbot về sân bay/tuyến đường/dịch vụ.
- Hóa đơn điện tử và B2B/corporate accounts.

## Phase 3

- Hệ sinh thái travel concierge.
- Dynamic package: xe + tour + SIM + hotel.
- Fleet riêng hoặc partner white-label nếu pháp lý sẵn sàng.
- Dự báo nhu cầu theo lịch bay.
- Kiosk/agent mode tại terminal.

## Ma trận chọn mô hình

| Tiêu chí                | Concierge đặt hộ | Marketplace    | Tự vận hành    | Hybrid                 |
| ----------------------- | ---------------- | -------------- | -------------- | ---------------------- |
| Tốc độ ra MVP           | Cao              | Cao            | Thấp           | Trung bình             |
| Rủi ro pháp lý          | Trung bình thấp  | Thấp           | Cao            | Trung bình             |
| Kiểm soát trải nghiệm   | Trung bình       | Thấp           | Cao            | Cao nếu vận hành tốt   |
| Cần API đối tác         | Nên có           | Không bắt buộc | Không          | Có                     |
| Cần app tài xế          | Không            | Không          | Có             | Chưa cần ban đầu       |
| Xử lý 50 khách cùng lúc | Queue + ops      | Khó theo dõi   | Cần supply lớn | Queue + multi-provider |
| Khuyến nghị             | MVP              | Backup         | Không làm ngay | Tầm nhìn dài hạn       |
