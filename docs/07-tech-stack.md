# 07. Tech stack đề xuất

## Kết luận nhanh

Stack đề xuất:

- Frontend: Expo + React Native + Expo Router + PWA.
- Backend: Node.js + NestJS.
- DB: PostgreSQL.
- Cache/queue: Redis + BullMQ.
- Realtime: WebSocket qua NestJS gateway.
- Map: Google Maps Platform cho Places/Routes; Mapbox cho indoor/airport visualization nếu có dữ liệu.
- Payment: VietQR/payOS cho VND nội địa; Stripe hoặc gateway tương đương cho thẻ quốc tế nếu pháp nhân/onboarding phù hợp.
- AI/automation: OpenAI cho structured extraction/chat/translation/RAG; n8n cho workflow có người giám sát.
- Observability: OpenTelemetry + Sentry + Prometheus/Grafana hoặc managed equivalent.

## Frontend

### Phương án 1: Expo React Native + Web/PWA

Khuyến nghị.

Lý do:

- Một codebase cho iOS, Android, Web/PWA.
- PWA quan trọng vì khách quốc tế không muốn cài app chỉ để rời sân bay.
- Expo Router hỗ trợ routing thống nhất web/native.
- Sau MVP có thể build app native khi cần push notification/deep native features.

Rủi ro:

- PWA trên iOS có giới hạn về background/push/offline so với native.
- Map native và web có thể cần wrapper khác nhau.
- Indoor positioning nâng cao có thể cần native module.

Checklist:

- [x] MVP dùng PWA.
- [x] Codebase chuẩn Expo để không bỏ đường native sau này.
- [ ] Nếu cần beacon/Bluetooth/UWB, đánh giá native app sớm.

### Phương án 2: Next.js PWA riêng + React Native sau

Ưu điểm:

- Web/PWA tối ưu hơn.
- SEO/landing tốt hơn.
- Dễ dùng component web/admin chung.

Nhược điểm:

- Sau này phải build mobile riêng.
- Tốn công đồng bộ UI/logic.

Chọn nếu:

- [ ] MVP chỉ web, không có kế hoạch native trong 6-12 tháng.

## Backend

### NestJS

Lý do:

- TypeScript end-to-end với frontend.
- Module rõ ràng cho booking/payment/provider/ops.
- Hỗ trợ WebSocket gateway.
- Có package tích hợp BullMQ.
- Phù hợp team cần kiến trúc nghiêm túc hơn Express thuần.

Module gợi ý:

- `AuthModule`
- `PassengerModule`
- `BookingModule`
- `ProviderModule`
- `PaymentModule`
- `MapModule`
- `NotificationModule`
- `OpsModule`
- `SupportModule`
- `InvoiceModule`
- `AiAssistModule`
- `AuditModule`

## Database

### PostgreSQL

Khuyến nghị vì:

- Transaction chắc cho payment/booking.
- JSONB lưu provider payload.
- Index tốt cho reporting.
- Có thể mở rộng PostGIS nếu sau này cần geospatial nâng cao.

ORM:

- [x] Prisma nếu team thích schema/type-safe và tốc độ dev.
- [ ] TypeORM nếu team đã quen NestJS pattern truyền thống.
- [ ] Drizzle nếu muốn SQL-like type safety nhẹ.

## Redis + BullMQ

Dùng để giải quyết đúng vấn đề brief nêu: 50 khách cùng lúc.

BullMQ phù hợp vì:

- Queue Redis-backed.
- Retry, delay, priority, concurrency.
- Worker scale ngang.
- NestJS có tích hợp `@nestjs/bullmq`.

Queue critical:

- Provider booking không chạy sync trong HTTP request.
- Payment/refund không phụ thuộc UI.
- Notification retry được.

## Map và định vị

### Google Maps Platform

Dùng cho:

- Places Autocomplete.
- Place Details.
- Routes API/ETA/distance.
- Geocoding.

Điểm mạnh:

- Dữ liệu địa điểm khách sạn/POI tốt.
- Khách quốc tế quen Google.

Rủi ro:

- Chi phí theo usage.
- Cần cache hợp lý theo policy.

### Mapbox

Dùng cho:

- Custom map UI.
- Indoor map nếu dữ liệu terminal có trong Mapbox Indoor hoặc được sân bay cung cấp.
- Floor selector/indoor visualization trên native.

Lưu ý:

- Indoor API của Mapbox hiện có phần experimental theo docs, nên không nên đặt là dependency bắt buộc của MVP.
- Với sân bay mới, phải xác minh dữ liệu indoor thực tế.

### Pickup map MVP

Nên làm:

- Static terminal map chính thức.
- Ảnh landmark.
- Pickup zone catalogue.
- QR zone.
- GPS/geofence ngoài trời chỉ là phụ trợ.

## Payment

### VietQR/payOS

Phù hợp:

- Khách Việt Nam.
- VND.
- QR scan từ app ngân hàng.
- Đối soát tự động qua webhook.

Rủi ro:

- Khách quốc tế có thể không dùng được.
- Refund cần flow rõ.

### Stripe hoặc gateway thẻ quốc tế

Phù hợp:

- Visa/Mastercard/Amex/JCB.
- Apple Pay/Google Pay nếu hỗ trợ.
- Khách quốc tế.

Rủi ro:

- Onboarding theo pháp nhân/quốc gia.
- FX, chargeback, dispute.
- Không lưu card data trong hệ thống.

### Pay at provider

Phù hợp:

- MVP nếu provider đã thu tiền.
- Giảm rủi ro payment/refund.

Nhược điểm:

- App khó capture revenue.
- Khó đảm bảo khách không hủy/no-show.

## AI và automation

### OpenAI/LLM

Use cases nên làm:

- Dịch chat khách - ops/tài xế.
- Tóm tắt support case.
- Trích xuất destination từ text tự do.
- Tư vấn itinerary có structured output.
- RAG hỏi đáp sân bay/dịch vụ.

Không nên để AI:

- Tự đặt vé/thu tiền/hoàn tiền không có rule.
- Tự cam kết pháp lý/chính sách.
- Tự quyết định provider trong case rủi ro cao mà không logging.

### n8n

Use cases:

- Workflow có nhiều API và cần ops duyệt.
- Ticket booking assisted.
- Gửi thông báo đa kênh.
- Sync dữ liệu provider/report.

Rủi ro:

- Phải khóa quyền workflow editor.
- Không public webhook không bảo vệ.
- Không chạy code node tùy tiện trong production.

## Hosting/deployment

MVP options:

- [x] Managed cloud: AWS/GCP/Fly/Render/Railway tùy team.
- [x] PostgreSQL managed.
- [x] Redis managed.
- [x] CDN cho PWA.
- [ ] Kubernetes chỉ khi có đội vận hành đủ mạnh.

Production baseline:

- API autoscaling.
- Worker autoscaling theo queue depth.
- DB backup.
- Redis persistence phù hợp queue.
- Separate staging/production.
- Secret manager.
- CI/CD.

## Stack thay thế

| Thành phần | Đề xuất | Thay thế | Khi nào dùng thay thế |
| --- | --- | --- | --- |
| Frontend | Expo RN Web | Next.js | Chỉ cần web/PWA trước |
| Backend | NestJS | Fastify/Express | Team nhỏ, logic ít |
| DB | PostgreSQL | MySQL | Team/infra đã chuẩn MySQL |
| Queue | BullMQ | Temporal | Workflow dài/phức tạp, cần durable orchestration mạnh |
| Map | Google + Mapbox | HERE/TomTom/AWS Location | Hợp đồng/giá tốt hơn hoặc cần SEA data khác |
| Payment | VietQR + Stripe/gateway | OnePay/NganLuong/Paymentwall | Onboarding quốc tế/nội địa phù hợp hơn |
| AI | OpenAI | Claude/Gemini/local | Chính sách dữ liệu/giá/latency |

