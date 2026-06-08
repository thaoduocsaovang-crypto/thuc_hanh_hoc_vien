---
mo-ta: bang kiem rui ro va kiem soat so bo cho bai toan buoi 1 - nhom 01
trang-thai: active
phien-ban: v2.1
created-at: 2026-06-08 11:20 +07:00
updated-at: 2026-06-08 11:20 +07:00
---

# Bảng kiểm rủi ro và kiểm soát

## Dữ liệu

- [x] Không dùng dữ liệu thật của VTN
- [x] Không dùng thông tin khách hàng thật
- [x] Không dùng IP thật, mã trạm thật hoặc cấu hình thật
- [x] Có dữ liệu mô phỏng thay thế
- [x] Có mô tả nguồn dữ liệu mô phỏng

## Phân quyền và điểm kết nối

- [x] Không dùng token thật
- [x] Không dùng API key thật
- [x] Không hard-code thông tin bí mật
- [x] Không đưa API key hoặc token vào prompt, ảnh chụp màn hình, log, n8n export hoặc bài nộp
- [x] Nếu mô tả điểm kết nối (endpoint), chỉ dùng endpoint giả lập
- [x] Quyền truy cập trong bài thực hành là quyền đọc hoặc quyền mô phỏng

## Con người trong vòng lặp

- [x] Có bước con người trong vòng lặp (human in the loop)
- [x] Có người chịu trách nhiệm duyệt đầu ra
- [x] Có tiêu chí duyệt hoặc từ chối
- [x] Không để AI tự quyết định việc có rủi ro kỹ thuật, pháp lý, nhân sự hoặc vận hành

## Nhật ký và truy vết

- [x] Có lưu đầu vào mẫu
- [x] Có lưu đầu ra mẫu
- [x] Có ghi phiên bản prompt hoặc hướng dẫn
- [x] Có ghi trạng thái thành công hoặc lỗi
- [x] Có cách truy lại lý do AI đưa ra kết quả
- [x] Nhật ký vận hành chỉ chứa dữ liệu phi nhạy cảm

## Lan can an toàn

- [x] Có quy tắc từ chối nếu câu hỏi vượt phạm vi
- [x] Có quy tắc không suy đoán khi thiếu dữ liệu
- [x] Có quy tắc yêu cầu trích dẫn hoặc nêu căn cứ khi dùng tài liệu
- [x] Có quy tắc chuyển sang con người khi rủi ro cao

## Kết luận

| Mục | Kết luận |
| --- | --- |
| Bài toán đủ an toàn để làm trong lớp? | Có |
| Điều kiện cần sửa trước khi chốt | Không có điều kiện đặc biệt, duy trì việc sử dụng dữ liệu mô phỏng 100%. |
| Người xác nhận | Học viên nhóm 01 |

## Phân tích Rủi ro chi tiết và Rào cản an toàn (Guardrails)

### Rủi ro 1: Rò rỉ thông tin cá nhân nhạy cảm (PII) của khách hàng/kỹ sư trong ticket thô
*   **Mô tả**: Trong thực tế, khách hàng hoặc kỹ sư vận hành khi tạo ticket hỗ trợ có thể điền thông tin liên hệ cá nhân (Họ tên, SĐT, Email, IP thiết bị cá nhân) vào phần mô tả ticket. Nếu gửi trực tiếp dữ liệu thô này lên các mô hình AI trực tuyến công cộng mà không xử lý trước, có nguy cơ lộ lọt thông tin cá nhân nhạy cảm và vi phạm luật an toàn thông tin.
*   **Rào cản an toàn (Guardrails)**:
    *   *Môi trường thực hành*: Chỉ sử dụng bộ dữ liệu mô phỏng hoàn toàn (đã được làm sạch, thay thế tên thật bằng các tên giả lập như "Nguyen Van A", số điện thoại dummy).
    *   *Tiền xử lý dữ liệu*: Cài đặt bộ lọc Regex hoặc mô hình Nhận diện thực thể tên (NER) để tự động phát hiện và che giấu các thông tin có định dạng Số điện thoại, Email, Số CCCD trước khi chuyển nội dung sang LLM API.

### Rủi ro 2: AI tạo phản hồi nháp sai lệch chính sách hoặc hứa hẹn vượt thẩm quyền (Hallucination)
*   **Mô tả**: Mô hình ngôn ngữ lớn (LLM) có thể tự ý "bịa" ra các cam kết kỹ thuật (như "sự cố sẽ được sửa trong 5 phút") hoặc hứa hẹn bồi thường tài chính/khuyến mại cước phí cho khách hàng, gây ra rủi ro pháp lý và tổn hại uy tín doanh nghiệp.
*   **Rào cản an toàn (Guardrails)**:
    *   *HITL bắt buộc*: Áp dụng quy trình kiểm duyệt 100%. Nhân viên điều phối ticket phải đọc và phê duyệt/chỉnh sửa bản nháp thư trước khi gửi đi. Không cấp quyền cho hệ thống tự động trả lời tự động khách hàng.
    *   *System Instruction chặt chẽ*: Định nghĩa rõ trong prompt hệ thống của AI: "Không đưa ra bất kỳ mốc thời gian cam kết sửa chữa cụ thể nào; không hứa hẹn bồi thường tiền cước; chỉ dùng cấu trúc thư nháp để tiếp nhận yêu cầu lịch sự và thông báo sẽ có kỹ sư liên hệ lại".

### Rủi ro 3: AI định tuyến sai ticket khẩn cấp hoặc ticket an ninh bảo mật
*   **Mô tả**: Các ticket khẩn cấp (ví dụ: tấn công mạng, mất kết nối diện rộng) có thể bị AI nhận diện sai hoặc phân loại nhầm vào nhóm xử lý có độ ưu tiên thấp hoặc sai phòng ban (như nhóm bán hàng), dẫn đến kéo dài thời gian gián đoạn dịch vụ.
*   **Rào cản an toàn (Guardrails)**:
    *   *Định tuyến cứng (Hard-coded bypass)*: Thiết lập bộ lọc từ khóa quan trọng (ví dụ: "hack", "downtime", "sập nguồn", "tấn công") để đẩy thẳng các ticket này vào nhóm xử lý sự cố khẩn cấp của NOC mà không qua xử lý phân loại của AI.
    *   *Ngưỡng tin cậy (Confidence threshold)*: Yêu cầu AI trả về mức độ tự tin (confidence score) khi phân loại. Nếu điểm tin cậy dưới 80%, chuyển ticket sang nhóm "Unknown" để con người phân loại thủ công.

### Rủi ro 4: Ghi nhật ký (Logging) chứa thông tin bảo mật của mạng lưới
*   **Mô tả**: Quá trình lưu vết hoạt động của hệ thống để quản trị và cải tiến (logging) có thể vô tình ghi lại toàn bộ nội dung ticket chứa thông tin kỹ thuật nhạy cảm hoặc mật khẩu/tài khoản.
*   **Rào cản an toàn (Guardrails)**:
    *   Chỉ thiết lập ghi log các thông tin meta phi nhạy cảm bao gồm: `ticket_id_giap_lap` (mã định danh ngẫu nhiên), `timestamp`, `ai_suggested_group`, `human_approved_group`, `hitl_flag` và `execution_time_ms`. Tuyệt đối không lưu nội dung chi tiết tiêu đề và mô tả ticket thô vào file log hệ thống.

---

## Ghi chú

Bảng này là rà rủi ro sơ bộ ở session 01, không thay thế bảng kiểm tuân thủ trước khi thí điểm trong session 06.
