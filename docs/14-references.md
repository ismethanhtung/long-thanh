# 14. Nguồn tham khảo

Các nguồn dưới đây được dùng để xác minh phần có thể thay đổi theo thời gian: sân bay Long Thành, API/nền tảng, payment, map, queue và quy định vận tải.

## Long Thành

- [ACV - Groundbreaking ceremony / Long Thanh Phase 1](https://vietnamairport.vn/en/tin-tuc/acv-s-activities/the-groundbreaking-ceremony-of-three-bidding-packages-under-the-construction-projects-of-long-thanh-international-airport-phase-1-and-passenger-terminal-t3-tan-son-nhat-international-airport): xác nhận công suất toàn dự án 100 triệu khách/năm, giai đoạn 1 25 triệu khách/năm.
- [ACV - Groundbreaking Phase 1 Sub-project 3](https://vietnamairport.vn/en/tin-tuc/acv-s-activities/groundbreaking-ceremony-for-the-long-thanh-international-airport-phase-1-sub-project-3-2): xác nhận các hạng mục hạ tầng và công suất thiết kế.
- [Cổng thông tin Chính phủ - Quy hoạch Long Thành](https://en.baochinhphu.vn/pm-approves-long-thanh-intl-airport-scheme-11110061.htm): xác nhận quy hoạch 5.000 ha và mục tiêu trung chuyển khu vực.

## Frontend/PWA

- [Expo Documentation](https://docs.expo.dev/): Expo là nền tảng build app chạy trên Android, iOS và Web.
- [Expo Progressive Web Apps](https://docs.expo.dev/guides/progressive-web-apps/): hướng dẫn PWA, manifest, service worker.
- [Expo Router](https://docs.expo.dev/router/introduction/): file-based routing cho React Native và Web.

## Backend/queue/realtime

- [NestJS Queues](https://docs.nestjs.com/techniques/queues): NestJS hỗ trợ BullMQ, Redis-backed queue, producers/consumers.
- [NestJS WebSocket Gateways](https://docs.nestjs.com/websockets/gateways): WebSocket gateway trong NestJS.
- [BullMQ Docs](https://docs.bullmq.io/): queue Redis cho Node.js, retry, concurrency, distributed workers.

## Maps

- [Google Places API](https://developers.google.com/maps/documentation/places/web-service): autocomplete/place search/place details.
- [Google Routes API](https://developers.google.com/maps/documentation/routes): compute routes và route matrix.
- [Mapbox Android Indoor Maps](https://docs.mapbox.com/android/maps/guides/indoor/): indoor floor selector, experimental API.
- [Mapbox iOS Indoor Maps](https://docs.mapbox.com/ios/maps/guides/indoor/): indoor map và floor selection trên iOS.
- [Mapbox Indoor v3 Tileset](https://docs.mapbox.com/data/tilesets/reference/mapbox-indoor-v3/): dữ liệu indoor cho venue như airport, coverage cần xác minh thực tế.

## Provider vận tải

- [Grab for Business - Business Transport](https://www.grab.com/mm/en/business/transport/): mô tả portal, transport, concierge cho khách/visitor. Cần làm việc trực tiếp để xác nhận API tại Việt Nam.
- [Xanh Business](https://www.xanhsm.com/business-transport): giải pháp vận tải doanh nghiệp.
- [Be - About be](https://be.com.vn/en/about-be/): thông tin Be Group, dịch vụ transport và giấy phép vận tải được công bố trên site.
- [Vexere](https://vexere.com/en-US): nền tảng vé xe, train/flight, mạng lưới tuyến/nhà xe; API B2B cần xác minh qua partnership.

## Payment

- [VietQR API](https://www.vietqr.io/): payment kit, QR/deeplink/payment confirmation.
- [VietQR API Overview](https://api.vietqr.vn/en): mô tả payment reconciliation, sandbox, token, callback.
- [NAPAS FastFund 247 with VietQR](https://en.napas.com.vn/napas-fastfund-247-with-vietqr-code-service): VietQR/NAPAS QR transfer.
- [payOS API](https://payos.vn/docs/api/): API tạo link thanh toán và môi trường production.
- [Stripe Supported Payment Methods](https://docs.stripe.com/payments/payment-methods/overview): thẻ, ví, payment method families; onboarding cần xác minh theo pháp nhân.

## AI/automation

- [OpenAI Structured Outputs](https://platform.openai.com/docs/guides/structured-outputs): output theo JSON schema, phù hợp itinerary/ops tools.
- [OpenAI File Search](https://platform.openai.com/docs/guides/tools-file-search): RAG/knowledge base cho FAQ sân bay/dịch vụ.
- [n8n Integrations](https://docs.n8n.io/integrations/): node/workflow automation cho tích hợp và quy trình assisted.

## Pháp lý vận tải

- [Chinhphu.vn - Quy định mới về taxi theo Nghị định 158/2024/NĐ-CP](https://xaydungchinhsach.chinhphu.vn/quy-dinh-moi-ve-dieu-kien-voi-xe-o-to-kinh-doanh-van-tai-hanh-khach-bang-taxi-119241223183421802.htm): tóm tắt quy định taxi/phần mềm tính tiền.
- [LuatVietnam - Decree 158/2024/ND-CP Road Transport](https://english.luatvietnam.vn/decree-no-158-2024-nd-cp-dated-december-29-2024-of-the-government-prescribing-road-transport-operations-381392-doc1.html): bản tiếng Anh/tóm lược chi tiết về vận tải, taxi, đơn vị kết nối vận tải.
- [Thuviennhadat - Decree 158/2024/ND-CP Road Transport](https://thuviennhadat.vn/vbpl/decree-158-2024-nd-cp-road-transport-644980.html): nội dung liên quan hợp đồng vận tải, xe hợp đồng.

## Ghi chú tin cậy

- Các trang provider như Grab/Xanh/Be/Vexere xác nhận sản phẩm/doanh nghiệp công khai, nhưng không đủ để kết luận có API đặt xe công khai cho dự án. Phải làm việc trực tiếp với business/partner team.
- Phần pháp lý cần luật sư Việt Nam rà soát trước launch, đặc biệt nếu app thu tiền, điều phối xe, quyết định giá, hoặc bán vé.
