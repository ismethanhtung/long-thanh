# Bộ tài liệu thiết kế app đặt xe Long Thành

Tài liệu này biến brief trong [description.md](/Users/thanhtung/Downloads/app-xe/description.md) thành một bản thiết kế sản phẩm và kỹ thuật có thể đem đi thảo luận với sếp, đối tác vận tải, pháp chế, thiết kế, backend, frontend và vận hành.

Mục tiêu không phải chỉ làm một app gọi xe. Mục tiêu là một nền tảng hỗ trợ hành khách tại sân bay Long Thành chọn phương án rời sân bay phù hợp nhất: xe công nghệ, taxi/xe hợp đồng đối tác, xe khách/liên tỉnh, tour ngắn ngày, SIM, đổi tiền, hỗ trợ đa ngôn ngữ và điều phối trong khu vực sân bay.

## Cách đọc nhanh

1. Đọc [01-context-and-market.md](/Users/thanhtung/Downloads/app-xe/docs/01-context-and-market.md) để hiểu bối cảnh Long Thành và nhu cầu khách.
2. Đọc [02-product-scope-and-modes.md](/Users/thanhtung/Downloads/app-xe/docs/02-product-scope-and-modes.md) để chọn mô hình kinh doanh: đặt hộ, marketplace, tự vận hành hay hybrid.
3. Đọc [03-user-journeys.md](/Users/thanhtung/Downloads/app-xe/docs/03-user-journeys.md) để thấy mọi luồng người dùng chính.
4. Đọc [04-app-ux-blueprint.md](/Users/thanhtung/Downloads/app-xe/docs/04-app-ux-blueprint.md) để hình dung ứng dụng hoàn chỉnh hoạt động ra sao.
5. Đọc [06-system-architecture.md](/Users/thanhtung/Downloads/app-xe/docs/06-system-architecture.md) và [07-tech-stack.md](/Users/thanhtung/Downloads/app-xe/docs/07-tech-stack.md) để chốt kiến trúc/stack.
6. Dùng [13-decision-checklist.md](/Users/thanhtung/Downloads/app-xe/docs/13-decision-checklist.md) làm checklist hỏi sếp và đối tác.

## Danh sách file và ý nghĩa

| File | Ý nghĩa |
| --- | --- |
| [01-context-and-market.md](/Users/thanhtung/Downloads/app-xe/docs/01-context-and-market.md) | Phân tích Long Thành, phân khúc khách, nhu cầu thật, kịch bản từ sân bay về HCM/tỉnh/du lịch. |
| [02-product-scope-and-modes.md](/Users/thanhtung/Downloads/app-xe/docs/02-product-scope-and-modes.md) | Các mô hình sản phẩm và kinh doanh có thể chọn, ưu/nhược/điều kiện pháp lý. |
| [03-user-journeys.md](/Users/thanhtung/Downloads/app-xe/docs/03-user-journeys.md) | Toàn bộ luồng khách, tài xế/đối tác, admin, CSKH, tình huống lỗi. |
| [04-app-ux-blueprint.md](/Users/thanhtung/Downloads/app-xe/docs/04-app-ux-blueprint.md) | Blueprint màn hình app khách, web/PWA, admin, kiosk và nguyên tắc UX. |
| [05-feature-spec.md](/Users/thanhtung/Downloads/app-xe/docs/05-feature-spec.md) | Đặc tả tính năng theo module, MVP, phase 2, phase 3, acceptance criteria. |
| [06-system-architecture.md](/Users/thanhtung/Downloads/app-xe/docs/06-system-architecture.md) | Kiến trúc tổng thể, realtime, queue, chống quá tải 50+ khách cùng lúc, sơ đồ Mermaid. |
| [07-tech-stack.md](/Users/thanhtung/Downloads/app-xe/docs/07-tech-stack.md) | So sánh và đề xuất stack frontend, backend, DB, map, payment, AI, hosting. |
| [08-integrations.md](/Users/thanhtung/Downloads/app-xe/docs/08-integrations.md) | Chiến lược tích hợp Grab/Xanh SM/Be/Vexere/VietQR/Stripe/n8n/OpenAI. |
| [09-data-model-and-api.md](/Users/thanhtung/Downloads/app-xe/docs/09-data-model-and-api.md) | Mô hình dữ liệu, trạng thái booking/payment/job, API contract sơ bộ. |
| [10-operations-admin.md](/Users/thanhtung/Downloads/app-xe/docs/10-operations-admin.md) | Vận hành thật tại sân bay: pickup zone, điều phối, CSKH, dashboard, SLA. |
| [11-legal-risk-compliance.md](/Users/thanhtung/Downloads/app-xe/docs/11-legal-risk-compliance.md) | Rủi ro pháp lý, dữ liệu cá nhân, vận tải, hóa đơn, đối soát, các điểm cần luật sư xác nhận. |
| [12-roadmap-and-estimation.md](/Users/thanhtung/Downloads/app-xe/docs/12-roadmap-and-estimation.md) | Roadmap triển khai từ prototype đến production, mốc nghiệm thu và rủi ro. |
| [13-decision-checklist.md](/Users/thanhtung/Downloads/app-xe/docs/13-decision-checklist.md) | Checklist checkbox để sếp chọn phương án và phạm vi. |
| [14-references.md](/Users/thanhtung/Downloads/app-xe/docs/14-references.md) | Nguồn tham khảo đã dùng, kèm ghi chú áp dụng. |

## Khuyến nghị mặc định

Phương án nên chọn để đi nhanh và giảm rủi ro:

- [x] MVP là PWA trước, sau đó đóng gói iOS/Android bằng Expo khi có traction.
- [x] Không tự quản lý tài xế/xe trong giai đoạn đầu.
- [x] Tích hợp/điều phối qua nhà cung cấp hợp pháp: Grab for Business/Concierge, Xanh Business, Be hoặc hãng taxi/xe hợp đồng đã có giấy phép.
- [x] Hệ thống đóng vai trò concierge/marketplace/điều phối, có đội vận hành xác nhận các đơn phức tạp.
- [x] Dùng queue để xử lý đồng thời nhiều yêu cầu sau khi chuyến bay hạ cánh.
- [x] Tách rõ `booking` của app và `provider_order` của đối tác để tránh lệ thuộc một nhà cung cấp.

## Những quyết định cần chốt

- [ ] Công ty muốn chỉ đặt xe hộ, làm marketplace hay tự vận hành xe?
- [ ] Có ký được API/portal doanh nghiệp với Grab, Xanh SM, Be, taxi hãng, Vexere không?
- [ ] Có được phép đặt kiosk/QR/standee/hỗ trợ viên trong sân bay không?
- [ ] Có cần xuất hóa đơn cho khách quốc tế ngay từ MVP không?
- [ ] Có xử lý thanh toán quốc tế trong app hay chuyển khách sang provider thanh toán?
- [ ] Có cần indoor map thật trong terminal ngay từ đầu hay chỉ cần pickup zone + chỉ dẫn ảnh/QR?

