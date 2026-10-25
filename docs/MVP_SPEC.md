# Tâm Lý Tuổi Vàng — Đặc tả MVP

## 1. Mục tiêu
Xây dựng nền tảng giáo dục tâm lý và hỗ trợ thích nghi với giai đoạn nghỉ hưu. MVP cung cấp nội dung giáo dục, tự phản tư không chẩn đoán, bài tập cá nhân, tiếp nhận yêu cầu và luồng đặt lịch. Đây không phải hệ thống chẩn đoán hay điều trị.

## 2. Người dùng và vai trò
- Khách truy cập: đọc nội dung, tìm kiếm, làm bài tự phản tư.
- Người dùng có tài khoản (giai đoạn backend): lưu kế hoạch và nhật ký theo quyền riêng tư.
- Chuyên gia: chỉ xem các yêu cầu/lịch được phân công; cập nhật trạng thái theo phạm vi được phép.
- Biên tập viên: quản lý bài viết và nguồn tham khảo.
- Quản trị viên: quản lý người dùng, phân quyền, nội dung, lịch và yêu cầu.
- Chủ sở hữu hệ thống: cấu hình và xem nhật ký kiểm toán hạn chế.

## 3. Phạm vi MVP
1. Bộ tự đánh giá đa chiều: 6 lĩnh vực (bản sắc/vai trò, cảm xúc/thích nghi, kết nối xã hội, mục đích sống, chuẩn bị thực tế, quan hệ gia đình). Kết quả là gợi ý tự phản tư, không chẩn đoán. Trước phát hành công khai, cần chuyên gia rà soát câu hỏi và cách diễn giải.
2. Thư viện bài viết: danh mục, tìm kiếm, lọc chủ đề, nội dung, nguồn tham khảo, ngày cập nhật, chức năng đọc bằng giọng nói của trình duyệt.
3. Kế hoạch 30 ngày: mục tiêu, lý do, bước đầu tiên, nhật ký tự phản tư; lưu theo tài khoản khi backend sẵn sàng.
4. Yêu cầu hỗ trợ: thu thập dữ liệu tối thiểu, thông báo quyền riêng tư/đồng ý, trạng thái tiếp nhận, luồng quản lý nội bộ.
5. Quản trị: đăng nhập bắt buộc, RBAC, CRUD nội dung, phân công và cập nhật yêu cầu/lịch, nhật ký kiểm toán.
6. Đặt lịch: ngày/giờ, hình thức, chuyên gia, trạng thái giữ chỗ/xác nhận/hủy, thông báo; kiểm tra xung đột phía máy chủ.

## 4. Ngoài phạm vi bản demo
- Chẩn đoán, khuyến nghị điều trị hoặc đánh giá nguy cơ tự động.
- Chatbot AI đóng vai nhà trị liệu.
- Thanh toán, video call tích hợp, cộng đồng mở.
- Lịch chuyên gia thật, gửi email/SMS thật.
- Lưu hồ sơ tâm lý nhạy cảm ở localStorage.
- Bản prototype không có backend không được dùng cho người dùng thật.

## 5. Trang và tuyến đường đề xuất
- `/` Trang chủ
- `/assessment` Tự đánh giá
- `/library` Thư viện bài viết
- `/library/[slug]` Chi tiết bài viết
- `/plan` Kế hoạch 30 ngày
- `/support` Gửi yêu cầu hỗ trợ
- `/booking` Đặt lịch
- `/account` Tài khoản và quyền riêng tư (khi có backend)
- `/admin` Bảng điều khiển
- `/admin/articles` Quản lý bài viết
- `/admin/requests` Quản lý yêu cầu
- `/admin/bookings` Quản lý lịch
- `/admin/users` Quản lý quyền

## 6. Luồng nghiệp vụ
### Tự đánh giá
Mở trang → đọc thông báo giới hạn → trả lời → xác thực đủ câu → tính kết quả theo từng lĩnh vực → gợi ý nội dung/bài tập → không lưu kết quả nếu chưa có đồng ý rõ ràng.

### Thư viện
Tìm kiếm/lọc → mở bài → đọc hoặc dùng TTS → xem nguồn và ngày rà soát → báo lỗi nội dung (giai đoạn sau).

### Kế hoạch và nhật ký
Đọc thông báo lưu trữ → nhập mục tiêu tối thiểu → lưu theo tài khoản đã xác thực → người dùng có thể xuất/xóa → chỉ chủ tài khoản và dịch vụ được phân quyền mới truy cập.

### Yêu cầu hỗ trợ
Thông báo quyền riêng tư và giới hạn phản hồi → thu thập tối thiểu → đồng ý rõ ràng → gửi qua API có xác thực/giới hạn tốc độ → tạo mã yêu cầu → thông báo đã tiếp nhận (không hứa phản hồi nếu chưa có SLA) → phân công người có quyền → cập nhật trạng thái → xóa theo chính sách.

### Đặt lịch
Chọn loại hình → xem lịch trống lấy từ server → chọn giờ → kiểm tra trùng lịch ở server trong giao dịch → xác nhận/giữ chỗ → thông báo → hủy theo chính sách → giải phóng slot và ghi log.

### Quản trị
Đăng nhập → kiểm tra vai trò ở server trên mọi endpoint → thao tác được phép → ghi nhật ký kiểm toán không chứa nội dung nhạy cảm nếu không cần thiết.

## 7. Mô hình dữ liệu khuyến nghị
- `users`: id, email, display_name, role, status, created_at, updated_at
- `consents`: id, user_id nullable, purpose, policy_version, granted_at, withdrawn_at
- `articles`: id, slug, title, summary, body, category_id, status, author_id, reviewer_id, reviewed_at, published_at, updated_at
- `categories`: id, name, slug
- `article_sources`: id, article_id, citation_text, url, accessed_at
- `assessment_definitions`: id, version, title, reviewed_by, reviewed_at, published_at
- `assessment_questions`: id, definition_id, domain, prompt, order_index
- `assessment_options`: id, question_id, label, score
- `assessment_results`: id, user_id, definition_id, domain_scores_json, created_at, retention_until (không lưu câu trả lời thô nếu không cần)
- `plans`: id, user_id, goal, rationale, first_step, start_date, status, created_at, updated_at
- `journal_entries`: id, user_id, prompt_id nullable, encrypted_content, created_at, updated_at, retention_until
- `support_requests`: id, user_id nullable, alias, contact_encrypted nullable, topic, message_encrypted, status, assigned_to nullable, consent_id, created_at, updated_at
- `appointments`: id, user_id nullable, provider_id, start_at, end_at, modality, status, contact_encrypted, consent_id, created_at, updated_at
- `audit_events`: id, actor_id, action, entity_type, entity_id, timestamp, metadata_minimized

Ràng buộc: UUID/ID khó đoán; foreign key; chỉ mục theo trạng thái/ngày; unique constraint trên slot của chuyên gia khi trạng thái đang giữ/xác nhận; xóa/ẩn theo retention policy; không log nội dung nhật ký hoặc thông điệp tư vấn.

## 8. API đề xuất
- `GET /api/articles?query=&category=`
- `GET /api/articles/:slug`
- `POST /api/assessment-results` (tùy chọn; chỉ khi có đồng ý và chính sách lưu)
- `GET /api/me/plans`, `POST /api/me/plans`, `PATCH /api/me/plans/:id`, `DELETE /api/me/plans/:id`
- `GET /api/me/journal`, `POST /api/me/journal`, `DELETE /api/me/journal/:id`
- `POST /api/support-requests`
- `GET /api/admin/support-requests` (role-gated)
- `PATCH /api/admin/support-requests/:id` (role-gated)
- `GET /api/availability?providerId=&from=&to=`
- `POST /api/appointments`
- `POST /api/appointments/:id/cancel`
- `GET/POST/PATCH/DELETE /api/admin/articles...` (role-gated)

## 9. Yêu cầu bảo mật và riêng tư
- HTTPS; xác thực mạnh; RBAC phía server; bảo vệ CSRF khi dùng cookie; rate limiting; validation phía server; output encoding; chống XSS; quản lý secrets qua môi trường; backup và kiểm tra khôi phục.
- Không dùng localStorage cho nhật ký hoặc thông tin tư vấn thật.
- Tối thiểu hóa dữ liệu; consent theo mục đích; chính sách lưu/xóa; quy trình xử lý yêu cầu quyền của chủ thể dữ liệu.
- Không đưa dữ liệu sức khỏe/tâm lý vào analytics hoặc logs mặc định.
- Không hiển thị thông báo “đã gửi” cho đến khi server xác nhận.
- Có quy trình chuyển tuyến khi có dấu hiệu cần trợ giúp khẩn cấp; không để công cụ tự động chẩn đoán.
- Rà soát pháp lý tại Việt Nam trước khi thu thập dữ liệu thật; xác minh yêu cầu hiện hành về bảo vệ dữ liệu cá nhân và dịch vụ liên quan.

## 10. Tiêu chí nghiệm thu
- Tất cả trang có thể điều hướng bằng bàn phím; focus rõ; label liên kết với input; thông báo lỗi dùng aria-live.
- Tăng/giảm chữ, chế độ tối và TTS hoạt động hoặc có fallback rõ ràng.
- Tự đánh giá không trả kết quả khi thiếu câu; diễn giải không gắn nhãn bệnh lý.
- Tìm kiếm/lọc bài viết hoạt động; nguồn tham khảo hiển thị; liên kết ngoài mở an toàn.
- Kế hoạch có thể tạo/sửa/xóa; lưu thật chỉ sau khi backend và quyền riêng tư sẵn sàng.
- Yêu cầu hỗ trợ chỉ báo tiếp nhận khi server trả thành công; lỗi không làm mất dữ liệu đang nhập.
- Không thể truy cập endpoint quản trị bằng cách chỉ ẩn giao diện; mọi quyền phải được kiểm tra phía server.
- Đặt lịch có kiểm tra trùng slot ở server; hủy cập nhật trạng thái và giải phóng slot đúng cách.
- Kiểm thử responsive, trình đọc màn hình, lỗi mạng, input không hợp lệ, session hết hạn và quyền truy cập.
- Có privacy notice, retention policy, quy trình xóa/xuất dữ liệu, backup và kế hoạch xử lý sự cố trước launch.

## 11. Các quyết định cần chốt
1. Tổ chức/cá nhân chịu trách nhiệm vận hành và chuyên môn.
2. Dịch vụ miễn phí hay trả phí; kênh hỗ trợ và SLA thực tế.
3. Ai là chuyên gia, phạm vi dịch vụ, cách xác minh năng lực.
4. Dữ liệu nào thật sự cần lưu; thời hạn lưu; nơi lưu và người được truy cập.
5. Ngân sách hosting, email/SMS và lịch đặt.
6. Có yêu cầu đa ngôn ngữ hay chỉ tiếng Việt ở MVP.
