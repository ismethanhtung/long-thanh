# 13. Checklist quyết định cho sếp và team

File này dùng trong buổi họp chốt scope. Mỗi mục nên tick một lựa chọn chính.

## 1. Mô hình kinh doanh

- [x] MVP concierge đặt hộ qua provider hợp pháp.
- [ ] Marketplace chỉ giới thiệu/chuyển khách sang provider.
- [ ] Tự vận hành đội xe.
- [ ] Hybrid ngay từ đầu.

Ghi chú quyết định:

- [ ] App có thu phí dịch vụ riêng không?
- [ ] App có thu toàn bộ tiền chuyến rồi trả provider không?
- [ ] App chỉ nhận hoa hồng từ provider?

## 2. Provider vận tải MVP

- [ ] Grab for Business/Concierge.
- [ ] Xanh Business.
- [ ] Be.
- [ ] Taxi hãng.
- [ ] Xe hợp đồng/limousine.
- [ ] Chưa chọn, cần partnership làm việc.

Provider fallback:

- [ ] Có ít nhất 2 provider trước pilot.
- [ ] Có call center/manual fallback.
- [ ] Có SLA bằng văn bản.

## 3. Kênh app

- [x] PWA qua QR.
- [ ] Native iOS/Android từ đầu.
- [ ] Kiosk/agent mode từ đầu.
- [ ] Web admin only pilot.

## 4. Phạm vi địa lý MVP

- [x] Long Thành -> TP.HCM.
- [ ] Long Thành -> Đồng Nai/Bình Dương/Vũng Tàu.
- [ ] Long Thành -> Đà Lạt/Mũi Né.
- [ ] Liên tỉnh toàn quốc.
- [ ] Chỉ tư vấn tuyến xa, chưa đặt.

## 5. Thanh toán

- [ ] Pay at provider.
- [ ] VietQR.
- [ ] Thẻ quốc tế.
- [ ] Corporate billing.
- [ ] Thu phí dịch vụ app riêng.

Khuyến nghị:

- [x] MVP nên hỗ trợ ít nhất pay-at-provider hoặc VietQR để giảm độ phức tạp.
- [ ] Thẻ quốc tế cần kiểm tra onboarding gateway/pháp nhân trước khi cam kết.

## 6. Indoor/pickup

- [x] Static pickup map + ảnh landmark + zone catalogue.
- [ ] Mapbox indoor nếu có dữ liệu.
- [ ] Beacon/Wi-Fi/UWB positioning.
- [ ] Nhân viên cầm bảng tên/VIP pickup.

## 7. AI

- [x] Dịch chat và template hỗ trợ.
- [x] Tóm tắt support case.
- [ ] Tư vấn itinerary có ops duyệt.
- [ ] AI tự động mua vé.
- [ ] AI tự động hoàn tiền.

Khuyến nghị:

- [x] Không cho AI tự quyết định giao dịch tiền/vé trong MVP.

## 8. Vé xe/tuyến tỉnh

- [ ] Chỉ tư vấn lộ trình.
- [ ] Assisted booking bởi ops.
- [ ] API booking qua Vexere/đối tác.
- [ ] Tự ký trực tiếp nhà xe.

## 9. Hóa đơn

- [ ] Chỉ receipt app.
- [ ] Provider xuất hóa đơn vận tải.
- [ ] App xuất hóa đơn phí dịch vụ.
- [ ] Hỗ trợ invoice B2B từ MVP.

## 10. Pháp lý

- [ ] Luật sư đã review mô hình đặt hộ.
- [ ] Luật sư đã review điều khoản marketplace/concierge.
- [ ] Luật sư đã review payment/refund.
- [ ] Luật sư đã review dữ liệu cá nhân.
- [ ] Luật sư đã review xe hợp đồng/liên tỉnh.

## 11. Vận hành

- [ ] Có ops agent trực pilot.
- [ ] Có hotline.
- [ ] Có supervisor refund/escalation.
- [ ] Có quy trình provider fail.
- [ ] Có quy trình khách lạc/tài xế lạc.
- [ ] Có báo cáo cuối ngày.

## 12. Launch readiness

- [ ] Có provider contract.
- [ ] Có pickup instructions chính thức.
- [ ] Có privacy policy/terms/cancellation policy.
- [ ] Có staging/prod environment.
- [ ] Có monitoring/alerting.
- [ ] Có backup provider.
- [ ] Có training ops.
- [ ] Có checklist rollback.

