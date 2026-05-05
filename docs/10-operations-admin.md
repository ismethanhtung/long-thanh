# 10. Vận hành và admin

## Nguyên tắc vận hành

App chỉ tốt nếu vận hành thật chạy được tại sân bay. Với Long Thành, điểm khó không chỉ là code, mà là:

- Giờ hạ cánh tạo burst.
- Khách bị lạc trong terminal.
- Provider thiếu xe hoặc hủy.
- Khách quốc tế không gọi điện được.
- Payment/booking/ticket có ngoại lệ.
- Cần phối hợp sân bay, pickup zone, bảo vệ, taxi lane.

## Đội vận hành MVP

Vai trò:

- `Ops Agent`: xử lý booking, đặt hộ, cập nhật trạng thái.
- `Support Agent`: chat/call khách.
- `Supervisor`: xử lý refund, VIP, provider escalation.
- `Finance`: đối soát cuối ngày.
- `Partnership Manager`: liên hệ provider.

Ca trực:

- [ ] 24/7 nếu sân bay vận hành quốc tế nhiều chuyến đêm.
- [ ] Tối thiểu cover peak arrival windows trong pilot.

## Dashboard cần có

### Operations board

Thông tin mỗi booking:

- Booking code.
- Flight.
- ETA/created time.
- Passenger language.
- Destination.
- Passenger count/luggage.
- Payment status.
- Provider status.
- SLA timer.
- Last event.
- Assigned ops.

Actions:

- Assign provider.
- Open provider portal.
- Manual confirm.
- Send pickup instructions.
- Call/chat customer.
- Mark issue.
- Cancel/refund.

### Flight demand board

Mục tiêu: chuẩn bị trước supply.

- Chuyến bay sắp hạ cánh.
- Số booking gắn với flight.
- Quốc tế/nội địa nếu có.
- Số khách đoàn.
- Provider supply status.
- Cảnh báo cần thêm xe.

### Pickup zone board

- Zone A/B/C/D.
- Số khách đang chờ.
- Số xe đang đến.
- Sự cố tại zone.
- Nhân viên sân bay phụ trách nếu có.

### Provider board

- Provider online/offline/degraded.
- Time to assign.
- Cancel rate.
- No supply count.
- Manual order backlog.

## SLA đề xuất

| Sự kiện | SLA mục tiêu | Nếu quá SLA |
| --- | --- | --- |
| Booking được nhận | < 2 giây | Alert API |
| Có phương án giá | < 5 giây nếu cached | Fallback static/fixed |
| Tìm provider | 1-3 phút | Escalate ops |
| Ops phản hồi khách | < 2 phút | Supervisor alert |
| Driver assigned | 5-15 phút tùy supply | Offer alternative |
| Refund initiated | < 24 giờ | Finance alert |

## Quy trình xử lý “không có xe”

1. Worker đánh dấu provider attempts fail.
2. Booking chuyển `WAITING_OPS`.
3. Ops thử provider dự phòng/taxi hãng.
4. Nếu vẫn không có:
   - Đề xuất chờ thêm.
   - Đề xuất xe cao cấp/giá khác.
   - Đề xuất khách tự đặt qua app provider.
   - Hoàn tiền nếu đã thu.
5. Gửi tin nhắn rõ ràng, không để khách nhìn spinner vô hạn.

## Quy trình khách/tài xế không tìm thấy nhau

1. Khách bấm `Driver cannot find me`.
2. App gửi vị trí/zone/ảnh nếu có.
3. Ops nhận case high priority.
4. Ops gửi template đa ngôn ngữ:
   - Hướng dẫn khách đứng tại landmark.
   - Hướng dẫn tài xế đến đúng pickup point.
5. Nếu quá 10 phút:
   - Gọi khách.
   - Gọi provider/tài xế nếu có số.
   - Reassign nếu cần.

## Quy trình flight delay

1. Khách nhập flight number.
2. Nếu chưa tích hợp flight data, ops có thể chỉnh ETA thủ công.
3. Booking có `scheduled_pickup_at` cập nhật.
4. Provider booking chỉ tạo gần thời điểm pickup để tránh phí chờ.
5. Nếu khách đã có xe và delay lớn, ops xử lý hủy/đổi theo policy.

## Quy trình đoàn

1. Tạo group booking.
2. Gán một ops owner.
3. Xác nhận danh sách số khách/hành lý.
4. Chọn phương án nhiều xe hoặc van/bus.
5. Tạo sub-booking.
6. Mỗi xe có pickup code.
7. Trưởng đoàn có dashboard link.
8. Ops tick từng xe:
   - [ ] Xe đã đến.
   - [ ] Tài xế đã liên hệ.
   - [ ] Khách đã lên.
   - [ ] Xe đã rời sân bay.

## Đối soát cuối ngày

Finance cần:

- Tổng tiền khách trả.
- Tổng tiền provider.
- Phí dịch vụ app.
- Refund pending/completed.
- Booking cancelled/no-show.
- Hóa đơn cần xuất.
- Booking provider thu tiền trực tiếp.

Daily report:

- GMV.
- Completed trips.
- Failed no supply.
- Provider performance.
- Manual workload.
- Customer complaints.

## Playbook sự cố

### Provider API down

- Circuit breaker provider.
- Tắt provider trong option planner.
- Chuyển booking sang provider khác/manual.
- Thông báo ops.

### Payment webhook down

- Không rely duy nhất vào webhook.
- Poll/reconcile payment provider.
- Mark payment `RECONCILIATION_REQUIRED`.
- Không xác nhận provider nếu payment bắt buộc chưa chắc.

### Queue backlog cao

- Scale worker.
- Tăng ops staffing.
- Giảm provider timeout.
- Ưu tiên booking đã paid/VIP.

### Khách khiếu nại bị tính sai

- Mở booking timeline.
- Kiểm tra price snapshot.
- Kiểm tra provider receipt.
- Escalate finance/provider.
- Ghi audit.

