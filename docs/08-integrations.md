# 08. Tích hợp bên thứ ba

## Nguyên tắc

Không hard-code vào một provider. Hệ thống phải coi mọi đối tác là adapter có capability khác nhau.

Capability cần chuẩn hóa:

- Estimate price/time.
- Create booking/order.
- Cancel order.
- Get status.
- Driver info.
- Vehicle info.
- Tracking.
- Receipt/invoice.
- Refund.
- Webhook/polling.
- Manual portal fallback.

## Ma trận provider vận tải

| Provider | Vai trò | Mức tích hợp khả thi | Ghi chú |
| --- | --- | --- | --- |
| Grab for Business | Đặt xe doanh nghiệp/khách | Portal/Concierge/API nếu được cấp | Cần làm việc trực tiếp với Grab; tài liệu API công khai về ride booking không chắc có sẵn. |
| Xanh Business | Xe điện/taxi doanh nghiệp | Portal/contract/API nếu có | Phù hợp hình ảnh xanh, VIP, doanh nghiệp. |
| Be | Ride-hailing nội địa | Đối tác/contract/API nếu có | Be có giấy phép và hệ sinh thái vận tải tại Việt Nam. |
| Taxi hãng | Taxi sân bay/fixed-price | API nếu có, portal, call center | Có thể là fallback quan trọng. |
| Xe hợp đồng/limousine | Liên tỉnh, đoàn | Manual/API riêng | Cần kiểm tra giấy phép, hợp đồng, chính sách pickup/dropoff. |
| Vexere/OTA | Vé xe/tàu/flight | Affiliate/API/assisted | Cần hợp đồng B2B nếu muốn đặt hộ tự động. |

## Grab for Business

Use cases:

- Đặt xe hộ khách không có app Grab.
- Quản lý chi phí, báo cáo, corporate billing.
- Concierge/front desk booking nếu có quyền.

Cần hỏi Grab:

- [ ] Có API tạo chuyến cho khách/visitor tại Việt Nam không?
- [ ] Có sandbox không?
- [ ] Có hỗ trợ pickup Long Thành khi sân bay vận hành không?
- [ ] Có webhook driver assigned/trip completed không?
- [ ] Có trả driver name/license plate/ETA không?
- [ ] Có cho app mình thu tiền rồi trả Grab sau không, hay Grab thu trực tiếp?
- [ ] Có giới hạn số booking đồng thời/rate limit không?
- [ ] Có điều khoản đặt xe cho khách quốc tế không có số điện thoại Việt Nam không?

Fallback nếu không có API:

- Admin dashboard mở task cho ops.
- Ops dùng portal Concierge/Grab for Business.
- Ops nhập mã chuyến/tài xế/biển số vào hệ thống.
- Hệ thống tiếp tục gửi update cho khách.

## Xanh Business

Use cases:

- Xe điện, dịch vụ doanh nghiệp, khách VIP.
- Hình ảnh xanh cho sân bay mới.
- Đặt xe tháng, event, đoàn.

Cần hỏi:

- [ ] Có portal/API cho đối tác đặt chuyến không?
- [ ] Có hỗ trợ visitor booking không?
- [ ] Có gói airport transfer/fixed-price không?
- [ ] Có hỗ trợ hóa đơn doanh nghiệp không?
- [ ] Có SLA supply tại Long Thành không?

## Be

Use cases:

- Provider nội địa dự phòng.
- Tăng supply khi Grab/Xanh thiếu xe.
- beTaxi/beCar.

Cần hỏi:

- [ ] Có B2B/API/portal cho doanh nghiệp không?
- [ ] Có capability đặt hộ khách không?
- [ ] Có webhook/tracking không?
- [ ] Có chính sách sân bay Long Thành không?

## Taxi hãng và xe hợp đồng

Vai trò:

- Fixed-price airport transfer.
- Backup khi ride-hailing thiếu xe.
- Xe 7 chỗ, van, đoàn, VIP.
- Liên tỉnh.

Cần chuẩn hóa contract:

- Giá.
- Phí chờ.
- Chính sách hủy/no-show.
- Hành lý.
- Pickup zone.
- Ngôn ngữ tài xế.
- Hóa đơn.
- SLA xác nhận.
- Hotline điều phối.

## Vexere/nhà xe/OTA

Use cases:

- Vé xe khách/limousine.
- So sánh phương án đi tỉnh.
- Combo xe sân bay -> bến/điểm đón -> vé xe.

Cần hỏi:

- [ ] Có API B2B không?
- [ ] Có hold seat trước thanh toán không?
- [ ] Có webhook ticket confirmation/cancel/refund không?
- [ ] Có hỗ trợ khách quốc tế không có số Việt Nam không?
- [ ] Ai chịu trách nhiệm nếu nhà xe đổi điểm đón/giờ?

Nếu chưa có API:

- Phase 1 chỉ hiển thị tư vấn + ops assisted.
- Không tự động thu tiền vé nếu chưa chắc quy trình hoàn/hủy.

## Payment integrations

### VietQR/payOS

Luồng:

1. Backend tạo payment intent.
2. Backend gọi payOS/VietQR tạo payment link/QR.
3. Khách scan QR bằng banking app.
4. Payment provider gửi webhook.
5. Backend verify signature/checksum.
6. Booking chuyển sang paid/queued.

Cần có:

- [ ] Dynamic QR theo từng order.
- [ ] Webhook xác nhận thanh toán.
- [ ] Reconciliation job nếu webhook fail.
- [ ] Refund hoặc manual refund process.

### Stripe/gateway thẻ quốc tế

Luồng:

1. Backend tạo payment intent.
2. Frontend dùng hosted checkout/payment element.
3. Provider xử lý card/3DS.
4. Webhook update backend.
5. Capture/refund theo booking status.

Cần có:

- [ ] Onboarding pháp nhân.
- [ ] Chính sách chargeback.
- [ ] Không lưu thông tin thẻ.
- [ ] Support Apple Pay/Google Pay nếu gateway hỗ trợ.

## Notification

Kênh:

- Email: receipt/booking detail.
- SMS: fallback nội địa.
- WhatsApp: khách quốc tế.
- Zalo: khách Việt.
- Push notification: native phase 2.

Nội dung event:

- Booking requested.
- Payment pending/success.
- Driver assigned.
- Pickup instruction.
- Provider cancelled.
- Refund initiated.
- Trip completed.

## AI/OpenAI

Use cases:

- `translate_message`: dịch chat.
- `extract_destination`: lấy địa chỉ từ text tự do.
- `plan_itinerary`: tạo itinerary có schema.
- `summarize_case`: tóm tắt case cho ops.
- `faq_rag`: hỏi đáp sân bay/dịch vụ.

Guardrails:

- Structured output cho itinerary.
- Human approval trước khi mua vé/thu tiền phức tạp.
- Không gửi dữ liệu passport/thẻ nếu không cần.
- Log prompt/output theo policy bảo mật.

## n8n

Workflow phù hợp:

- Assisted ticket purchase.
- Provider report sync.
- Notification routing.
- Daily reconciliation.
- Lead handoff cho tour/SIM.

Production rules:

- [ ] Không để public editor.
- [ ] Giới hạn credential theo workflow.
- [ ] Webhook có secret/signature.
- [ ] Không cho ops tạo workflow production tùy tiện.
- [ ] Các workflow quan trọng phải versioned/reviewed.

