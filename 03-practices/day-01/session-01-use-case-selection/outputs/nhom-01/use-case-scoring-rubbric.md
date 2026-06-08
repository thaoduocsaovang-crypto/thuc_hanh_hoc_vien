---
mo-ta: thang cham diem rubbric lua chon bai toan buoi 1 - nhom 01
trang-thai: active
phien-ban: v2.1
created-at: 2026-06-08 11:20 +07:00
updated-at: 2026-06-08 11:20 +07:00
---

# Thang chấm điểm (rubbric) lựa chọn bài toán

## Cách dùng

Chấm từng bài toán ứng viên theo thang 100 điểm. Ưu tiên bài toán đạt từ 70 điểm trở lên, không dùng dữ liệu thật và có điểm con người duyệt rõ ràng.

## Bảng điểm

| Tiêu chí | Điểm tối đa | Câu hỏi kiểm tra | Điểm nhóm tự chấm | Nhận xét ngắn gọn |
| --- | ---: | --- | ---: | --- |
| Giá trị nghiệp vụ | 20 | Bài toán có giảm thời gian, giảm lỗi hoặc tăng chất lượng đầu ra không? | 18 | Giảm 80% thời gian phân phối và phản hồi bước đầu cho ticket, nâng cao hiệu suất. |
| Tính khả thi trong lớp | 15 | Có thể tạo bản thử nghiệm tối thiểu (MVP) trong 6 buổi không? | 14 | Cực kỳ khả thi, quy trình xử lý văn bản đơn giản qua n8n và Gemini API có thể dựng xong trong 1-2 buổi. |
| Dữ liệu mô phỏng | 15 | Có thể tạo dữ liệu mô phỏng đủ đại diện mà không dùng dữ liệu thật không? | 15 | Dễ dàng sinh dữ liệu ticket giả lập đa dạng và sạch PII bằng LLM hoặc chuẩn bị thủ công. |
| Mức độ đo lường | 10 | Có chỉ số đo hiệu quả tối thiểu không? | 9 | Có các chỉ số rõ ràng: Tỷ lệ phân loại chính xác, thời gian xử lý trung bình (AHT), tỷ lệ dùng bản nháp. |
| Khả năng phát triển | 10 | Bài toán có thể mở rộng sau lớp mà không đổi toàn bộ thiết kế không? | 9 | Dễ dàng mở rộng quy mô bằng cách cập nhật quy tắc định tuyến, tích hợp RAG vào sinh bản nháp. |
| Kiểm soát rủi ro | 20 | Có điểm con người duyệt, logging, trace và guardrail không? | 18 | Cơ chế HITL kiểm duyệt 100% trước khi bấm gửi/chuyển tiếp giúp hạn chế tối đa rủi ro. |
| Năng lực AI phù hợp | 10 | AI có phù hợp với tác vụ đọc, tóm tắt, phân loại, trích xuất hoặc tạo nháp không? | 10 | Việc phân loại chủ đề và sinh email phản hồi mẫu là thế mạnh cốt lõi của LLM thế hệ mới. |
| Tổng | 100 | | 93 | Thiết kế bài toán đạt mức độ khả thi và kiểm soát an sau an toàn cao. |

## Quy tắc loại nhanh

Loại hoặc thu hẹp bài toán nếu có một trong các điều kiện sau:

- [x] Cần dữ liệu thật của VTN -> Không (Đạt)
- [x] Cần token thật, API key thật hoặc certificate thật -> Không (Đạt)
- [x] Cần quyền ghi hoặc thực thi trên hệ thống thật -> Không (Đạt)
- [x] Không có người duyệt trước khi sử dụng đầu ra -> Đạt (Có con người duyệt 100%)
- [x] Không mô tả được đầu vào hoặc đầu ra -> Đạt (Mô tả chi tiết và rõ ràng)
- [x] Không thể đo hiệu quả tối thiểu -> Đạt (Đo lường rõ ràng qua tỷ lệ chính xác và AHT)

## Kết luận nhóm

- **Bài toán được chọn**: Case 3: định tuyến ticket hỗ trợ giả lập và soạn nháp phản hồi.
- **Tổng điểm**: 93/100
- **Lý do chọn**: Điểm số đánh giá cao nhờ sự cân bằng giữa tính khả thi kỹ thuật (dễ dàng mô phỏng dữ liệu phi nhạy cảm) và tính ứng dụng thực tế (giảm thời gian điều phối ticket). Bên cạnh đó, bài toán không đụng chạm đến bất kỳ dữ liệu thật hay hệ thống nghiệp vụ sản xuất nào của VTN, đồng thời duy trì quy trình kiểm duyệt (HITL) an toàn tuyệt đối.
- **Danh sách quy trình ưu tiên của nhóm**:
  1. Xây dựng bộ dữ liệu ticket giả lập sạch PII (tiêu đề và nội dung mô tả lỗi mạng, cước phí, đăng ký dịch vụ).
  2. Thiết kế prompt phân loại ticket và đề xuất phòng ban xử lý phù hợp.
  3. Thiết kế prompt sinh bản nháp phản hồi lịch sự, đúng chuẩn nghiệp vụ dựa trên ngữ cảnh ticket.
  4. Tạo giao diện duyệt thủ công (HITL Approve node) trên n8n để người điều phối duyệt/sửa trước khi kết thúc quy trình.
- **Điều kiện cần chuẩn bị trước buổi 2 để dựng AI workflow/Case 10**:
  - File dữ liệu JSON/CSV mô phỏng các ticket hỗ trợ.
  - Các prompt mẫu phục vụ tác vụ phân loại và sinh văn bản.
  - Môi trường chạy n8n và kết nối API Gemini đã được thiết lập sẵn sàng.
