# 12. Roadmap triển khai

## Phase 0: Discovery và chốt mô hình

Thời gian: 2-4 tuần.

Deliverables:

- Chốt mô hình MVP: concierge/marketplace/hybrid.
- Danh sách provider có thể ký.
- Quy trình pháp lý sơ bộ.
- Pickup zone map từ sân bay hoặc phương án tạm.
- Prototype UX clickable.
- Tech spike PWA + map + booking queue.

Exit criteria:

- [ ] Có ít nhất 1 provider xe nội thành khả thi.
- [ ] Có ít nhất 1 provider xe/taxi fallback.
- [ ] Có quyết định payment MVP.
- [ ] Có quy trình ops thủ công nếu API chưa có.
- [ ] Có pháp chế duyệt mô hình thu tiền/đặt hộ.

## Phase 1: MVP production pilot

Thời gian: 8-12 tuần.

Scope:

- PWA khách.
- Admin ops dashboard.
- Booking core.
- Provider manual/portal-assisted.
- Payment basic: pay at provider hoặc VietQR/card tùy quyết định.
- Notification email/WhatsApp/SMS tối thiểu.
- Pickup guidance static.
- Queue worker.
- Audit log.
- Support case.

Không làm:

- App tài xế.
- Dynamic pricing tự xây.
- Indoor positioning nâng cao.
- AI tự mua vé.
- Multi-provider API phức tạp nếu chưa có hợp đồng.

Exit criteria:

- [ ] Khách tạo booking thật từ QR/PWA.
- [ ] Ops xử lý booking end-to-end.
- [ ] Có xe/provider xác nhận và khách nhận hướng dẫn pickup.
- [ ] Có trạng thái realtime/polling.
- [ ] Có hủy/hoàn tiền theo policy.
- [ ] Có report cuối ngày.

## Phase 2: Automation và provider API

Thời gian: 8-16 tuần sau MVP.

Scope:

- Tích hợp API provider đầu tiên nếu ký được.
- Provider fallback tự động.
- Webhook provider/payment.
- Flight/load board.
- RAG FAQ.
- AI translation support.
- Intercity assisted itinerary.
- Native app build nếu cần.

Exit criteria:

- [ ] Tỷ lệ booking tự động xác nhận > 60% cho tuyến nội thành.
- [ ] Manual intervention giảm rõ.
- [ ] Provider health dashboard hoạt động.
- [ ] CSKH đa ngôn ngữ có template/AI assist.

## Phase 3: Hybrid travel concierge

Thời gian: 3-6 tháng sau phase 2.

Scope:

- Multi-leg booking.
- Vé xe khách/tàu/flight qua partner.
- Tour/SIM/eSIM/currency exchange.
- Corporate accounts.
- Kiosk/agent mode.
- Advanced pickup/indoor map nếu có dữ liệu.

Exit criteria:

- [ ] App xử lý được Long Thành -> tỉnh gần bằng nhiều phương án.
- [ ] Có revenue ngoài taxi.
- [ ] Có partner SLAs.
- [ ] Ops playbook ổn định.

## Team đề xuất

MVP:

- 1 Product Manager/BA.
- 1 UX/UI designer.
- 2 frontend engineers.
- 2 backend engineers.
- 1 QA.
- 1 DevOps/infra part-time.
- 2-4 ops agents cho pilot.
- 1 partnership/legal owner.

Phase 2+:

- Thêm backend integration engineer.
- Thêm data/analytics.
- Thêm CS lead.
- Thêm security/compliance review.

## Rủi ro timeline

| Rủi ro | Ảnh hưởng | Giảm thiểu |
| --- | --- | --- |
| Không có API provider | Automation chậm | Làm portal-assisted MVP, adapter manual |
| Chưa có quyền đặt QR/kiosk tại sân bay | Acquisition yếu | Chạy qua hotel/tour/corporate/ads trước |
| Payment quốc tế onboarding lâu | Khách quốc tế khó trả tiền | Pay at provider/VietQR nội địa/alternate gateway |
| Pháp lý chưa rõ | Không dám launch | Chốt mô hình thấp rủi ro, luật sư review |
| Pickup zone thay đổi | Khách lạc | Content map cập nhật qua CMS/admin |
| Supply thiếu giờ cao điểm | SLA fail | Multi-provider + forecast + ops |

## KPI pilot

- Conversion QR open -> booking request.
- Booking request -> confirmed ride.
- Time to provider assignment.
- Time from arrival to pickup.
- Failed no supply rate.
- Customer support cases per 100 bookings.
- Refund rate.
- Rating.
- Provider cancellation rate.
- Manual minutes per booking.
- Gross margin per completed booking.

## Nghiệm thu MVP chi tiết

- [ ] PWA chạy tốt trên Safari iOS, Chrome Android, desktop.
- [ ] Booking không duplicate khi refresh/bấm lại.
- [ ] Admin board cập nhật trạng thái.
- [ ] Queue retry/fallback hoạt động.
- [ ] Payment webhook hoặc manual payment reconciliation hoạt động.
- [ ] Hủy booking có audit.
- [ ] Có log correlation id.
- [ ] Có privacy/terms/cancellation policy.
- [ ] Có backup ops khi provider fail.

