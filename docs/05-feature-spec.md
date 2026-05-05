# 05. Đặc tả tính năng

## Module 1: Destination search

Mục tiêu: khách nhập nơi muốn đến mà không sai địa chỉ.

Tính năng:

- Autocomplete địa điểm.
- Tìm theo hotel name, address, district, city, bus station, airport.
- Xác nhận địa chỉ bằng map pin.
- Lưu destination phổ biến.
- Cảnh báo địa chỉ mơ hồ.

Acceptance criteria:

- [ ] Khách nhập “Rex Hotel Saigon” thấy đúng khách sạn và địa chỉ.
- [ ] Nếu có nhiều kết quả giống nhau, app không tự chọn bừa.
- [ ] Nếu điểm đến nằm ngoài vùng phục vụ MVP, app chuyển sang assisted flow.

## Module 2: Route and option planner

Mục tiêu: biến destination thành các phương án di chuyển.

Tính năng:

- Tính ETA/distance.
- Phân loại chuyến: nội thành, liên tỉnh gần, liên tỉnh xa, nối chuyến.
- Rule engine đề xuất phương án.
- Multi-leg itinerary.
- Giá ước tính/fixed price tùy provider.

Acceptance criteria:

- [ ] Long Thành -> Quận 1: hiển thị xe công nghệ/taxi/private transfer.
- [ ] Long Thành -> Vũng Tàu: hiển thị xe riêng + xe khách nếu cấu hình có tuyến.
- [ ] Long Thành -> Đà Nẵng: chuyển sang tư vấn itinerary, không giả vờ có xe thông thường.

## Module 3: Booking

Tính năng:

- Tạo booking request.
- Idempotency key để tránh duplicate khi khách bấm nhiều lần.
- Trạng thái booking chuẩn hóa.
- Hủy/đổi thông tin trước khi provider confirm.
- Liên kết một booking với nhiều provider order attempts.

Trạng thái:

```text
DRAFT
REQUESTED
PAYMENT_REQUIRED
PAYMENT_PENDING
QUEUED
SEARCHING_PROVIDER
WAITING_OPS
PROVIDER_ASSIGNED
DRIVER_ASSIGNED
PICKUP_PENDING
IN_PROGRESS
COMPLETED
CANCELLED_BY_CUSTOMER
CANCELLED_BY_PROVIDER
FAILED_NO_SUPPLY
REFUND_PENDING
REFUNDED
```

Acceptance criteria:

- [ ] Refresh trang không tạo booking mới.
- [ ] Provider A fail thì có thể tạo provider attempt B.
- [ ] Khách và ops thấy cùng một trạng thái nhất quán.

## Module 4: Provider orchestration

Tính năng:

- Adapter cho từng provider.
- Provider priority.
- Rate limit.
- Retry/backoff.
- Circuit breaker nếu provider lỗi.
- Manual handoff nếu không có API.

Provider modes:

- `API`: tạo order qua API.
- `PORTAL_ASSISTED`: ops dùng portal đối tác, nhập kết quả lại.
- `PHONE_ASSISTED`: ops gọi tổng đài/đối tác.
- `DEEPLINK`: chuyển khách sang app/provider.

Acceptance criteria:

- [ ] Khi provider API timeout, job retry có giới hạn.
- [ ] Khi provider fail quá nhiều, circuit mở và provider tạm bị bỏ qua.
- [ ] Ops luôn có thể override provider assignment.

## Module 5: Payment

Tính năng:

- Payment intent.
- Card/Stripe hoặc payment gateway.
- VietQR dynamic QR.
- Pay at provider.
- Refund request.
- Reconciliation.
- Payment webhook.

Trạng thái:

```text
NOT_REQUIRED
REQUIRES_PAYMENT
PENDING
AUTHORIZED
CAPTURED
FAILED
CANCELLED
REFUND_PENDING
PARTIALLY_REFUNDED
REFUNDED
RECONCILIATION_REQUIRED
```

Acceptance criteria:

- [ ] Thanh toán thành công nhưng không có xe thì booking vào refund flow.
- [ ] Webhook đến trễ vẫn update đúng booking.
- [ ] Không capture tiền nếu chỉ cần pre-authorization, tùy provider/policy.

## Module 6: Pickup and wayfinding

Tính năng:

- Pickup zone catalogue.
- Static map/ảnh hướng dẫn.
- QR theo zone.
- “I am here”.
- Share location nếu browser cho phép.
- Indoor map phase 2 nếu có dữ liệu.

Acceptance criteria:

- [ ] Khách không cần đọc văn bản dài vẫn biết đi đến zone.
- [ ] Nếu GPS sai trong nhà ga, app không phụ thuộc hoàn toàn vào GPS.
- [ ] Tài xế/ops thấy khách đã đến pickup zone.

## Module 7: Chat/support

Tính năng:

- In-app support.
- Template đa ngôn ngữ.
- AI translation.
- Case timeline.
- Escalation.
- Attach photo/location.

Acceptance criteria:

- [ ] Khách bấm “Driver cannot find me” tạo case đúng booking.
- [ ] Ops thấy ngôn ngữ khách và template phù hợp.
- [ ] AI không tự xử lý refund nếu chưa có rule.

## Module 8: Intercity/ticketing

Tính năng:

- Search tuyến/nhà xe nếu có API/đối tác.
- Assisted ticket booking.
- Multi-leg itinerary.
- Thời gian chờ/chuyển chặng.
- Ticket confirmation.

Acceptance criteria:

- [ ] App nói rõ chặng nào do ai cung cấp.
- [ ] Nếu vé chưa confirm, không hiển thị như đã chắc chắn.
- [ ] Khách nhận nhắc giờ ra điểm đón.

## Module 9: Invoice/receipt

Tính năng:

- Receipt app.
- Provider receipt.
- E-invoice request.
- Business invoice fields.
- Export finance CSV.

Acceptance criteria:

- [ ] Khách có thể yêu cầu hóa đơn trong hoặc sau chuyến.
- [ ] Hệ thống phân biệt hóa đơn provider và phí dịch vụ app.
- [ ] Finance đối soát được provider cost và customer charge.

## Module 10: Admin and ops

Tính năng:

- Board booking realtime.
- Manual provider assignment.
- Flight grouping.
- SLA alerts.
- Refund workflow.
- Provider health.
- Audit log.

Acceptance criteria:

- [ ] Ops xử lý được booking không có API provider.
- [ ] Supervisor thấy booking quá SLA.
- [ ] Mọi thay đổi trạng thái có audit trail.

## Module 11: Security and compliance

Tính năng:

- Role-based access control.
- Audit log.
- PII masking.
- Data retention.
- Consent.
- Secrets management.
- Webhook signature verification.

Acceptance criteria:

- [ ] Nhân viên chỉ thấy dữ liệu cần thiết.
- [ ] Không lưu card number trong hệ thống.
- [ ] Webhook provider/payment được xác thực.

