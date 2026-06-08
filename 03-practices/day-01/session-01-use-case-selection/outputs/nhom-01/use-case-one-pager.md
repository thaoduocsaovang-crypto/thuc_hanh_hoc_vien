---
mo-ta: phieu mo ta case nhom-01 chon o muc ban nhap session 01
trang-thai: active
phien-ban: v2.1
created-at: 2026-06-08 11:20 +07:00
updated-at: 2026-06-08 11:20 +07:00
---

# Phiếu mô tả trường hợp sử dụng 01 trang

## Thông tin chung

| Mục | Nội dung |
| --- | --- |
| Tên nhóm | Nhóm 01 |
| Tên bài toán | Định tuyến ticket hỗ trợ giả lập và tự động tạo nháp phản hồi |
| Người dùng chính | Nhân viên điều phối ticket hỗ trợ (Ticket Dispatcher) |
| Người chịu trách nhiệm trình bày | Học viên nhóm 01 |
| Phiên bản | v0.1 |

## Mô tả bài toán

Hiện tại, việc tiếp nhận, phân loại và định tuyến các ticket yêu cầu hỗ trợ từ khách hàng đến đúng các phòng ban chuyên trách (như nhóm Kỹ thuật, nhóm Cước phí, nhóm Chăm sóc khách hàng) được thực hiện hoàn toàn thủ công. Quy trình này lặp đi lặp lại hàng ngày với số lượng lớn, gây mất nhiều thời gian chờ đợi của khách hàng và dễ dẫn đến sai sót định tuyến do sự chủ quan của con người. Trợ lý AI sẽ hỗ trợ phân tích tiêu đề và nội dung mô tả ticket giả lập để tự động phân loại, đề xuất nhóm xử lý thích hợp và chuẩn bị sẵn một bản nháp phản hồi ban đầu. Điều này giúp đẩy nhanh tốc độ phản hồi và giảm tải công việc lặp lại cho nhân viên điều phối.

## Đầu vào

| Loại đầu vào | Mô tả | Nguồn dữ liệu mô phỏng |
| --- | --- | --- |
| Tài liệu | Danh sách các nhóm xử lý giả lập và quy tắc định tuyến mô phỏng | Tài liệu quy trình giả lập tự xây dựng |
| Bảng dữ liệu | Không có | N/A |
| Log/ticket/email | Dữ liệu ticket giả lập gồm Tiêu đề ticket, Mô tả ticket và Nhóm xử lý ban đầu (nếu có) | File dữ liệu ticket mô phỏng sạch PII |

## Đầu ra mong muốn

| Đầu ra | Định dạng | Người sử dụng |
| --- | --- | --- |
| Nhãn phân loại | Chuỗi văn bản (Text) | Nhân viên điều phối ticket |
| Nhóm xử lý đề xuất | Chuỗi văn bản (Tên nhóm xử lý đề xuất) | Nhân viên điều phối ticket |
| Bản nháp phản hồi cho người gửi | Văn bản (Mẫu email phản hồi nhanh) | Nhân viên điều phối ticket (kiểm duyệt và gửi) |

## Giá trị kỳ vọng

- Thời gian tiết kiệm dự kiến: Giảm trung bình từ 5 phút xuống 1 phút cho mỗi ticket cần phân loại và soạn nháp phản hồi (tiết kiệm khoảng 80% thời gian xử lý bước đầu).
- Lỗi hoặc thiếu sót muốn giảm: Giảm tỷ lệ định tuyến sai ticket giữa các nhóm kỹ thuật/nghiệp vụ xuống dưới 5%.
- Chỉ số đo hiệu quả: Độ chính xác định tuyến (Routing Accuracy) của AI so với lựa chọn của con người, thời gian xử lý trung bình (AHT) mỗi ticket, tỷ lệ bản nháp phản hồi được chấp nhận/sử dụng ngay.

## Phạm vi bản thử nghiệm tối thiểu (MVP)

- Chỉ xử lý: Các ticket giả lập thuộc 3 chủ đề phổ biến: Lỗi kết nối mạng internet, Hỏi đáp hóa đơn/cước phí, và Yêu cầu đăng ký dịch vụ mới.
- Chưa xử lý: Ticket có chứa file đính kèm (hình ảnh, tài liệu PDF), các ticket có nội dung bằng tiếng nước ngoài hoặc ngôn ngữ quá phức tạp, và chưa tự động gửi trực tiếp phản hồi cho khách hàng mà không qua kiểm duyệt.
- Điều kiện để xem là hoàn thành: AI phân loại đúng nhóm xử lý đạt độ chính xác trên 80% trên tập dữ liệu thử nghiệm giả lập và sinh ra bản nháp phản hồi đầy đủ cấu trúc lịch sự.

## Kiểm soát rủi ro

| Rủi ro | Cách kiểm soát |
| --- | --- |
| Dữ liệu thật hoặc nhạy cảm | Chỉ dùng dữ liệu mô phỏng |
| AI trả lời sai | Có người kiểm tra trước khi dùng |
| Đầu ra vượt phạm vi | Có lan can an toàn (guardrail) trong hướng dẫn |
| Thiếu truy vết | Lưu đầu vào, đầu ra và phiên bản prompt |

## Điểm con người duyệt

Nhân viên điều phối (Agent/Dispatcher) đóng vai trò kiểm duyệt trực tiếp (HITL). Trước khi ticket được chuyển đi hoặc phản hồi nháp được gửi đi, nhân viên phải click xác nhận hoặc điều chỉnh lại nhãn phân loại/nhóm xử lý và chỉnh sửa bản nháp phản hồi cho phù hợp. Tuyệt đối không có hành động tự động hóa gửi email hay định tuyến tự động mà không có sự phê duyệt của con người.

## Dữ liệu mô phỏng và bàn giao sang buổi 2

- Dữ liệu mô phỏng cần chuẩn bị: Danh sách 20-30 ticket giả lập chứa tiêu đề và mô tả đa dạng chủ đề nhưng hoàn toàn phi nhạy cảm (không chứa thông tin thật của bất kỳ khách hàng nào).
- Quy tắc định tuyến hoặc phân loại: Bảng quy tắc ánh xạ từ từ khóa/chủ đề sang nhóm xử lý giả lập (ví dụ: "chậm", "rớt mạng" -> Nhóm Hỗ trợ kỹ thuật; "hóa đơn", "tiền cước" -> Nhóm Kế toán/Cước).
- Điểm con người duyệt: Giao diện duyệt trên hệ thống (hoặc bước chờ Approve trong n8n workflow).
- Trường nhật ký vận hành cần ghi: ticket_id_giap_lap, timestamp, router_group_ai, router_group_human_approved, hitl_flag (True/False), processing_time_ms.
