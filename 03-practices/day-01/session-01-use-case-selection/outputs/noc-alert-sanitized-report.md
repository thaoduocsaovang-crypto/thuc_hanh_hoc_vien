# BÁO CÁO PHÂN TÍCH VÀ LÀM SẠCH DỮ LIỆU CẢNH BÁO NOC

* **Chức danh thực hiện**: Chuyên gia Vận hành NOC (NOC L2/L3 Specialist)
* **Ngày thực hiện**: 08/06/2026
* **Nguồn dữ liệu**: `sample-noc-alerts.csv` (115 cảnh báo mô phỏng)

---

## 1. Phân Loại Cảnh Báo Theo Loại Thiết Bị (Device Type)

Dưới đây là bảng thống kê số lượng cảnh báo được phân loại theo từng loại thiết bị hệ thống trong mạng lưới:

| Loại thiết bị (Device Type) | Số lượng cảnh báo | Tỷ lệ phần trăm (%) | Nhận xét nhanh |
| :--- | :---: | :---: | :--- |
| **Server** (Máy chủ) | 23 | 20.00% | Số lượng cảnh báo cao nhất, tập trung vào dung lượng ổ đĩa, tiến trình docker và truy cập trái phép. |
| **UPS** (Bộ lưu điện) | 22 | 19.13% | Cảnh báo tần suất cao về biến động điện áp, chế độ bypass và kiểm tra lỗi card mạng. |
| **Base-station** (Trạm phát sóng) | 19 | 16.52% | Cảnh báo chủ yếu về lỗi mô-đun vô tuyến (RF), lỗi đồng bộ đồng hồ và nguồn dự phòng. |
| **Switch** (Thiết bị chuyển mạch) | 18 | 15.65% | Cảnh báo xoay quanh thay đổi cấu trúc STP, nhiễu broadcast và chập chờn giao diện (flap). |
| **Firewall** (Tường lửa) | 18 | 15.65% | Liên quan đến dung lượng bảng phiên (session), sử dụng bộ nhớ và các cuộc tấn công dò mật khẩu (brute force). |
| **Router** (Thiết bị định tuyến) | 15 | 13.04% | Cảnh báo về mất liên kết OSPF, chập chờn BGP và mất gói tin đường truyền (uplink). |
| **Tổng cộng** | **115** | **100.00%** | **Hệ thống hoạt động ổn định trong môi trường giả lập thử nghiệm lab.** |

---

## 2. Danh Sách Cảnh Báo Nguy Kịch (Critical) Đang Mở (Open) & Hành Động Khắc Phục (HITL)

Trong tổng số 115 cảnh báo, hệ thống ghi nhận **03 cảnh báo nguy kịch (critical)** đang ở trạng thái **chưa xử lý (open)**. Dưới đây là nội dung chi tiết được dịch sang tiếng Việt cùng quy trình khắc phục đề xuất cho kỹ thuật viên vận hành trực tiếp (Human-in-the-loop - HITL):

### 🚨 Cảnh báo ALERT-040
* **Thời gian ghi nhận**: `2026-05-01 23:56:00`
* **Trạm phát sinh**: `TEST_SITE_034`
* **Loại thiết bị**: `BASE-STATION`
* **Thông tin lỗi gốc (English)**: *"power backup warning in training scenario"*
* **Nội dung lỗi dịch tiếng Việt**: **Cảnh báo nguồn dự phòng (ắc quy) trong kịch bản đào tạo.**
* **Đề xuất hành động khắc phục nhanh (HITL)**:
1. Kiểm tra trạng thái nguồn điện lưới và dung lượng ắc quy dự phòng tại trạm `TEST_SITE_034`.
2. Phối hợp với kỹ thuật viên trực trạm để đo đạc điện áp đầu vào/đầu ra và kiểm tra hệ thống chuyển nguồn tự động (ATS) hoạt động ổn định.
3. Chuẩn bị máy phát điện dự phòng di động nếu lỗi xảy ra thực tế ngoài phòng lab.

### 🚨 Cảnh báo ALERT-058
* **Thời gian ghi nhận**: `2026-05-01 19:41:00`
* **Trạm phát sinh**: `TEST_SITE_013`
* **Loại thiết bị**: `BASE-STATION`
* **Thông tin lỗi gốc (English)**: *"RF module communication failure in lab environment"*
* **Nội dung lỗi dịch tiếng Việt**: **Lỗi truyền thông/mất kết nối khối vô tuyến (RF module) trong môi trường thử nghiệm lab.**
* **Đề xuất hành động khắc phục nhanh (HITL)**:
1. Kiểm tra cáp tín hiệu quang (CPRI/OBSAI) kết nối giữa khối xử lý băng gốc (BBU) và khối vô tuyến (RRU) tại phòng lab `TEST_SITE_013`.
2. Thực hiện khởi động lại mềm (soft reset) mô-đun RF từ hệ thống quản lý mạng (NMS).
3. Nếu lỗi vật lý kéo dài, cử kỹ thuật viên phòng lab kiểm tra đầu nối và thay thế mô-đun RF dự phòng.

### 🚨 Cảnh báo ALERT-081
* **Thời gian ghi nhận**: `2026-05-01 13:48:00`
* **Trạm phát sinh**: `TEST_SITE_020`
* **Loại thiết bị**: `SWITCH`
* **Thông tin lỗi gốc (English)**: *"high broadcast traffic detected in lab subnet"*
* **Nội dung lỗi dịch tiếng Việt**: **Phát hiện lưu lượng tin quảng bá (broadcast traffic) tăng cao bất thường trong phân mạng lab.**
* **Đề xuất hành động khắc phục nhanh (HITL)**:
1. Truy cập Switch tại `TEST_SITE_020`, xác định các cổng mạng có lưu lượng broadcast tăng đột biến.
2. Kiểm tra nguy cơ lặp mạng (network loop) do cấu hình STP hoặc đấu nối sai cáp vật lý.
3. Thực hiện cô lập cổng mạng bị ảnh hưởng (shutdown port) hoặc kích hoạt tính năng kiểm soát bão quảng bá (broadcast storm control) để bảo vệ mạng.

---

## 3. Bảng Đối Chiếu Minh Họa Kết Quả Làm Sạch Dữ Liệu Cá Nhân Nhạy Cảm (PII)

Để tuân thủ nghiêm ngặt quy định bảo mật dữ liệu cá nhân, toàn bộ thông tin nhạy cảm bao gồm **Họ tên**, **Số điện thoại**, và **Email** trong cột `summary` đã được phát hiện và làm sạch triệt để.

Dưới đây là bảng đối chiếu minh họa trên một số dòng log tiêu biểu có chứa PII từ dữ liệu đầu vào.
* *Lưu ý bảo mật*: Thông tin nhạy cảm ở cột **Dữ liệu trước khi lọc** đã được che mờ một phần bằng các ký tự ẩn (`*`) để đảm bảo an toàn tuyệt đối, trong khi cột **Dữ liệu sau khi lọc** hiển thị nhãn chuẩn hóa `[REDACTED_PII]`.

| Mã Cảnh Báo | Loại Thiết Bị | Dữ Liệu Trước Khi Lọc (Đã che mờ một phần) | Dữ Liệu Sau Khi Lọc (Sanitized summary) |
| :--- | :--- | :--- | :--- |
| **ALERT-001** | SERVER | database query latency spike in performance test [Contact: Ngu*** A (098*****21)] | database query latency spike in performance test [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-009** | BASE-STATION | RF module communication failure in lab environment [Assigned to: Ngu*** E (091*****78)] | RF module communication failure in lab environment [Assigned to: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-013** | FIREWALL | high memory usage alert on training cluster [Contact: Ngu*** A (098*****21)] | high memory usage alert on training cluster [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-022** | SERVER | database query latency spike in performance test [Contact: Tra*** B (tr***@vtn.com.vn)] | database query latency spike in performance test [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-025** | SWITCH | interface flap in synthetic log [Contact: Tra*** B (tr***@vtn.com.vn)] | interface flap in synthetic log [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-041** | SERVER | swap space exhausted in sandbox environment [Contact: Ngu*** A (098*****21)] | swap space exhausted in sandbox environment [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-042** | SWITCH | spanning tree topology change detected [Operator notes: contact engineer Le*** C at le***@vtn.vn if alert persists] | spanning tree topology change detected [Operator notes: contact engineer [REDACTED_PII] at [REDACTED_PII] if alert persists] |
| **ALERT-045** | SERVER | disk space low on simulated system drive [Contact: Ngu*** A (098*****21)] | disk space low on simulated system drive [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-061** | SWITCH | spanning tree topology change detected [Contact: Ngu*** A (098*****21)] | spanning tree topology change detected [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-067** | FIREWALL | high memory usage alert on training cluster [Operator notes: contact engineer Le*** C at le***@vtn.vn if alert persists] | high memory usage alert on training cluster [Operator notes: contact engineer [REDACTED_PII] at [REDACTED_PII] if alert persists] |
| **ALERT-068** | SERVER | unauthorized login attempt simulated in audit log [Contact: Tra*** B (tr***@vtn.com.vn)] | unauthorized login attempt simulated in audit log [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-070** | FIREWALL | session table capacity warning (simulated) [Ticket owner: Pha*** D - px***@vtn.com.vn] | session table capacity warning (simulated) [Ticket owner: [REDACTED_PII] - [REDACTED_PII]] |
| **ALERT-077** | FIREWALL | simulated brute force attack blocked on local port [Contact: Tra*** B (tr***@vtn.com.vn)] | simulated brute force attack blocked on local port [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-084** | SERVER | docker daemon crash warning in test node [Contact: Tra*** B (tr***@vtn.com.vn)] | docker daemon crash warning in test node [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-085** | ROUTER | BGP session flapping on simulated interface [Operator notes: contact engineer Le*** C at le***@vtn.vn if alert persists] | BGP session flapping on simulated interface [Operator notes: contact engineer [REDACTED_PII] at [REDACTED_PII] if alert persists] |
| **ALERT-093** | UPS | input voltage fluctuation detected in lab [Operator notes: contact engineer Le*** C at le***@vtn.vn if alert persists] | input voltage fluctuation detected in lab [Operator notes: contact engineer [REDACTED_PII] at [REDACTED_PII] if alert persists] |
| **ALERT-095** | BASE-STATION | RF module communication failure in lab environment [Assigned to: Ngu*** E (091*****78)] | RF module communication failure in lab environment [Assigned to: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-096** | UPS | high load capacity warning on bench unit [Operator notes: contact engineer Le*** C at le***@vtn.vn if alert persists] | high load capacity warning on bench unit [Operator notes: contact engineer [REDACTED_PII] at [REDACTED_PII] if alert persists] |
| **ALERT-102** | SERVER | unauthorized login attempt simulated in audit log [Contact: Ngu*** A (098*****21)] | unauthorized login attempt simulated in audit log [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-109** | UPS | input voltage fluctuation detected in lab [Contact: Ngu*** A (098*****21)] | input voltage fluctuation detected in lab [Contact: [REDACTED_PII] ([REDACTED_PII])] |
| **ALERT-110** | BASE-STATION | RF module communication failure in lab environment [Operator notes: contact engineer Le*** C at le***@vtn.vn if alert persists] | RF module communication failure in lab environment [Operator notes: contact engineer [REDACTED_PII] at [REDACTED_PII] if alert persists] |
| **ALERT-115** | BASE-STATION | power backup warning in training scenario [Ticket owner: Pha*** D - px***@vtn.com.vn] | power backup warning in training scenario [Ticket owner: [REDACTED_PII] - [REDACTED_PII]] |

---

## 4. Phụ Lục: Danh Sách Toàn Bộ 115 Cảnh Báo Đã Được Làm Sạch PII

Dưới đây là toàn bộ dữ liệu cảnh báo từ file log NOC gốc, với tất cả thông tin nhạy cảm đã được làm sạch và thay thế hoàn toàn bằng nhãn `[REDACTED_PII]`:

| Mã Cảnh Báo | Thời Gian | Mã Trạm | Thiết Bị | Mức Độ | Nội Dung Cảnh Báo (Sanitized) | Trạng Thái |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| ALERT-001 | 2026-05-01 08:02:00 | TEST_SITE_018 | SERVER | **HIGH** | database query latency spike in performance test [Contact: [REDACTED_PII] ([REDACTED_PII])] | open |
| ALERT-002 | 2026-05-01 21:16:00 | TEST_SITE_009 | SWITCH | low | interface flap in synthetic log | open |
| ALERT-003 | 2026-05-01 23:59:00 | TEST_SITE_026 | SERVER | **HIGH** | unauthorized login attempt simulated in audit log | resolved |
| ALERT-004 | 2026-05-01 21:35:00 | TEST_SITE_010 | SERVER | low | disk space low on simulated system drive | in-progress |
| ALERT-005 | 2026-05-01 22:56:00 | TEST_SITE_004 | FIREWALL | **HIGH** | session table capacity warning (simulated) | in-progress |
| ALERT-006 | 2026-05-02 01:25:00 | TEST_SITE_010 | BASE-STATION | low | RF module communication failure in lab environment | in-progress |
| ALERT-007 | 2026-05-01 10:31:00 | TEST_SITE_018 | UPS | **HIGH** | communication failure with network card in test | open |
| ALERT-008 | 2026-05-01 21:37:00 | TEST_SITE_027 | BASE-STATION | low | power backup warning in training scenario | resolved |
| ALERT-009 | 2026-05-01 21:52:00 | TEST_SITE_012 | BASE-STATION | **HIGH** | RF module communication failure in lab environment [Assigned to: [REDACTED_PII] ([REDACTED_PII])] | open |
| ALERT-010 | 2026-05-01 15:21:00 | TEST_SITE_022 | UPS | medium | communication failure with network card in test | resolved |
| ALERT-011 | 2026-05-01 11:56:00 | TEST_SITE_008 | UPS | low | bypass mode activated for maintenance demo | resolved |
| ALERT-012 | 2026-05-01 09:50:00 | TEST_SITE_023 | ROUTER | medium | packet loss above simulated threshold on uplink | in-progress |
| ALERT-013 | 2026-05-01 18:03:00 | TEST_SITE_020 | FIREWALL | medium | high memory usage alert on training cluster [Contact: [REDACTED_PII] ([REDACTED_PII])] | in-progress |
| ALERT-014 | 2026-05-01 14:22:00 | TEST_SITE_020 | SERVER | medium | disk space low on simulated system drive | in-progress |
| ALERT-015 | 2026-05-01 11:50:00 | TEST_SITE_026 | FIREWALL | medium | session table capacity warning (simulated) | open |
| ALERT-016 | 2026-05-01 21:54:00 | TEST_SITE_005 | ROUTER | low | packet loss above simulated threshold on uplink | resolved |
| ALERT-017 | 2026-05-01 16:32:00 | TEST_SITE_011 | UPS | **CRITICAL** | communication failure with network card in test | in-progress |
| ALERT-018 | 2026-05-01 17:53:00 | TEST_SITE_019 | BASE-STATION | low | RF module communication failure in lab environment | open |
| ALERT-019 | 2026-05-01 09:24:00 | TEST_SITE_010 | FIREWALL | medium | security policy update failed in sandbox | resolved |
| ALERT-020 | 2026-05-02 02:45:00 | TEST_SITE_006 | SERVER | medium | unauthorized login attempt simulated in audit log | in-progress |
| ALERT-021 | 2026-05-02 01:48:00 | TEST_SITE_005 | ROUTER | low | OSPF adjacency loss in training network | open |
| ALERT-022 | 2026-05-02 00:59:00 | TEST_SITE_022 | SERVER | medium | database query latency spike in performance test [Contact: [REDACTED_PII] ([REDACTED_PII])] | open |
| ALERT-023 | 2026-05-01 09:14:00 | TEST_SITE_027 | BASE-STATION | medium | clock synchronization out of bounds (simulated) | open |
| ALERT-024 | 2026-05-01 17:41:00 | TEST_SITE_026 | SWITCH | low | spanning tree topology change detected | in-progress |
| ALERT-025 | 2026-05-01 16:18:00 | TEST_SITE_004 | SWITCH | **HIGH** | interface flap in synthetic log [Contact: [REDACTED_PII] ([REDACTED_PII])] | open |
| ALERT-026 | 2026-05-01 18:35:00 | TEST_SITE_001 | SWITCH | **HIGH** | high broadcast traffic detected in lab subnet | in-progress |
| ALERT-027 | 2026-05-01 19:37:00 | TEST_SITE_013 | UPS | medium | bypass mode activated for maintenance demo | in-progress |
| ALERT-028 | 2026-05-01 16:17:00 | TEST_SITE_003 | SWITCH | low | interface flap in synthetic log | open |
| ALERT-029 | 2026-05-01 21:54:00 | TEST_SITE_029 | BASE-STATION | low | temperature warning on lab device | open |
| ALERT-030 | 2026-05-01 08:27:00 | TEST_SITE_032 | BASE-STATION | **HIGH** | clock synchronization out of bounds (simulated) | resolved |
| ALERT-031 | 2026-05-01 19:19:00 | TEST_SITE_023 | SWITCH | **CRITICAL** | high broadcast traffic detected in lab subnet | in-progress |
| ALERT-032 | 2026-05-01 09:55:00 | TEST_SITE_020 | SWITCH | **CRITICAL** | high broadcast traffic detected in lab subnet | resolved |
| ALERT-033 | 2026-05-01 23:08:00 | TEST_SITE_028 | UPS | medium | high load capacity warning on bench unit | in-progress |
| ALERT-034 | 2026-05-01 09:26:00 | TEST_SITE_012 | ROUTER | **CRITICAL** | packet loss above simulated threshold on uplink | in-progress |
| ALERT-035 | 2026-05-01 10:45:00 | TEST_SITE_003 | BASE-STATION | low | power backup warning in training scenario | in-progress |
| ALERT-036 | 2026-05-01 18:25:00 | TEST_SITE_032 | SERVER | low | docker daemon crash warning in test node | in-progress |
| ALERT-037 | 2026-05-01 15:31:00 | TEST_SITE_015 | FIREWALL | low | VPN tunnel down in test configuration | resolved |
| ALERT-038 | 2026-05-01 20:59:00 | TEST_SITE_013 | FIREWALL | low | VPN tunnel down in test configuration | in-progress |
| ALERT-039 | 2026-05-02 00:31:00 | TEST_SITE_029 | SERVER | **HIGH** | docker daemon crash warning in test node | in-progress |
| ALERT-040 | 2026-05-01 23:56:00 | TEST_SITE_034 | BASE-STATION | **CRITICAL** | power backup warning in training scenario | open |
| ALERT-041 | 2026-05-01 22:45:00 | TEST_SITE_003 | SERVER | medium | swap space exhausted in sandbox environment [Contact: [REDACTED_PII] ([REDACTED_PII])] | in-progress |
| ALERT-042 | 2026-05-01 08:56:00 | TEST_SITE_033 | SWITCH | **HIGH** | spanning tree topology change detected [Operator notes: contact engineer [REDACTED_PII] at [REDACTED_PII] if alert persists] | in-progress |
| ALERT-043 | 2026-05-01 14:05:00 | TEST_SITE_009 | SERVER | medium | unauthorized login attempt simulated in audit log | in-progress |
| ALERT-044 | 2026-05-01 12:36:00 | TEST_SITE_004 | SERVER | low | disk space low on simulated system drive | open |
| ALERT-045 | 2026-05-01 09:38:00 | TEST_SITE_034 | SERVER | low | disk space low on simulated system drive [Contact: [REDACTED_PII] ([REDACTED_PII])] | open |
| ALERT-046 | 2026-05-01 14:16:00 | TEST_SITE_014 | BASE-STATION | **HIGH** | power backup warning in training scenario | in-progress |
| ALERT-047 | 2026-05-01 08:46:00 | TEST_SITE_020 | UPS | low | input voltage fluctuation detected in lab | open |
| ALERT-048 | 2026-05-01 21:48:00 | TEST_SITE_019 | SERVER | medium | unauthorized login attempt simulated in audit log | resolved |
| ALERT-049 | 2026-05-01 08:05:00 | TEST_SITE_024 | BASE-STATION | low | temperature warning on lab device | open |
| ALERT-050 | 2026-05-01 13:58:00 | TEST_SITE_006 | SERVER | medium | disk space low on simulated system drive | in-progress |
| ALERT-051 | 2026-05-01 10:21:00 | TEST_SITE_009 | ROUTER | **HIGH** | packet loss above simulated threshold on uplink | resolved |
| ALERT-052 | 2026-05-01 12:33:00 | TEST_SITE_030 | UPS | **CRITICAL** | battery replacement warning (simulated) | in-progress |
| ALERT-053 | 2026-05-02 03:49:00 | TEST_SITE_028 | UPS | medium | communication failure with network card in test | open |
| ALERT-054 | 2026-05-01 19:24:00 | TEST_SITE_023 | SERVER | low | database query latency spike in performance test | resolved |
| ALERT-055 | 2026-05-01 11:29:00 | TEST_SITE_021 | ROUTER | low | high CPU usage due to synthetic routing traffic | resolved |
| ALERT-056 | 2026-05-01 23:12:00 | TEST_SITE_023 | ROUTER | medium | OSPF adjacency loss in training network | open |
| ALERT-057 | 2026-05-01 18:14:00 | TEST_SITE_016 | SWITCH | medium | mac address table overflow simulated alert | open |
| ALERT-058 | 2026-05-01 19:41:00 | TEST_SITE_013 | BASE-STATION | **CRITICAL** | RF module communication failure in lab environment | open |
| ALERT-059 | 2026-05-01 20:13:00 | TEST_SITE_030 | SERVER | low | database query latency spike in performance test | in-progress |
| ALERT-060 | 2026-05-02 00:04:00 | TEST_SITE_030 | UPS | medium | high load capacity warning on bench unit | in-progress |
| ALERT-061 | 2026-05-01 12:32:00 | TEST_SITE_018 | SWITCH | medium | spanning tree topology change detected [Contact: [REDACTED_PII] ([REDACTED_PII])] | open |
| ALERT-062 | 2026-05-02 01:38:00 | TEST_SITE_014 | UPS | medium | communication failure with network card in test | in-progress |
| ALERT-063 | 2026-05-01 15:53:00 | TEST_SITE_004 | FIREWALL | **CRITICAL** | session table capacity warning (simulated) | in-progress |
| ALERT-064 | 2026-05-02 00:12:00 | TEST_SITE_013 | UPS | medium | bypass mode activated for maintenance demo | resolved |
| ALERT-065 | 2026-05-01 11:20:00 | TEST_SITE_013 | UPS | medium | input voltage fluctuation detected in lab | in-progress |
| ALERT-066 | 2026-05-01 18:32:00 | TEST_SITE_016 | ROUTER | low | interface gigabitethernet0/1 down in test bed | in-progress |
| ALERT-067 | 2026-05-01 17:56:00 | TEST_SITE_008 | FIREWALL | medium | high memory usage alert on training cluster [Operator notes: contact engineer [REDACTED_PII] at [REDACTED_PII] if alert persists] | resolved |
| ALERT-068 | 2026-05-01 11:42:00 | TEST_SITE_034 | SERVER | medium | unauthorized login attempt simulated in audit log [Contact: [REDACTED_PII] ([REDACTED_PII])] | resolved |
| ALERT-069 | 2026-05-01 22:06:00 | TEST_SITE_018 | SWITCH | medium | spanning tree topology change detected | resolved |
| ALERT-070 | 2026-05-01 20:55:00 | TEST_SITE_004 | FIREWALL | **HIGH** | session table capacity warning (simulated) [Ticket owner: [REDACTED_PII] - [REDACTED_PII]] | open |
| ALERT-071 | 2026-05-01 18:00:00 | TEST_SITE_028 | ROUTER | medium | OSPF adjacency loss in training network | in-progress |
| ALERT-072 | 2026-05-01 11:22:00 | TEST_SITE_005 | UPS | low | bypass mode activated for maintenance demo | in-progress |
| ALERT-073 | 2026-05-01 22:46:00 | TEST_SITE_027 | BASE-STATION | medium | synthetic VSWR alarm on sector antenna A | resolved |
| ALERT-074 | 2026-05-01 21:38:00 | TEST_SITE_007 | SWITCH | low | interface flap in synthetic log | in-progress |
| ALERT-075 | 2026-05-01 13:39:00 | TEST_SITE_024 | BASE-STATION | medium | temperature warning on lab device | resolved |
| ALERT-076 | 2026-05-01 08:01:00 | TEST_SITE_030 | FIREWALL | medium | VPN tunnel down in test configuration | resolved |
| ALERT-077 | 2026-05-02 03:56:00 | TEST_SITE_002 | FIREWALL | medium | simulated brute force attack blocked on local port [Contact: [REDACTED_PII] ([REDACTED_PII])] | resolved |
| ALERT-078 | 2026-05-01 14:02:00 | TEST_SITE_011 | SWITCH | low | link aggregation bundle degraded in test lab | in-progress |
| ALERT-079 | 2026-05-01 12:57:00 | TEST_SITE_016 | SERVER | low | disk space low on simulated system drive | in-progress |
| ALERT-080 | 2026-05-01 08:50:00 | TEST_SITE_023 | UPS | medium | input voltage fluctuation detected in lab | open |
| ALERT-081 | 2026-05-01 13:48:00 | TEST_SITE_020 | SWITCH | **CRITICAL** | high broadcast traffic detected in lab subnet | open |
| ALERT-082 | 2026-05-02 02:11:00 | TEST_SITE_016 | SERVER | medium | disk space low on simulated system drive | open |
| ALERT-083 | 2026-05-01 18:07:00 | TEST_SITE_029 | SWITCH | low | high broadcast traffic detected in lab subnet | in-progress |
| ALERT-084 | 2026-05-01 15:15:00 | TEST_SITE_034 | SERVER | **HIGH** | docker daemon crash warning in test node [Contact: [REDACTED_PII] ([REDACTED_PII])] | open |
| ALERT-085 | 2026-05-01 18:00:00 | TEST_SITE_002 | ROUTER | low | BGP session flapping on simulated interface [Operator notes: contact engineer [REDACTED_PII] at [REDACTED_PII] if alert persists] | resolved |
| ALERT-086 | 2026-05-01 21:05:00 | TEST_SITE_011 | ROUTER | low | packet loss above simulated threshold on uplink | in-progress |
| ALERT-087 | 2026-05-01 22:02:00 | TEST_SITE_018 | FIREWALL | **CRITICAL** | simulated brute force attack blocked on local port | resolved |
| ALERT-088 | 2026-05-01 17:25:00 | TEST_SITE_029 | SWITCH | low | spanning tree topology change detected | open |
| ALERT-089 | 2026-05-01 19:57:00 | TEST_SITE_017 | UPS | medium | high load capacity warning on bench unit | in-progress |
| ALERT-090 | 2026-05-01 14:52:00 | TEST_SITE_026 | ROUTER | **HIGH** | BGP session flapping on simulated interface | in-progress |
| ALERT-091 | 2026-05-01 14:47:00 | TEST_SITE_011 | UPS | medium | bypass mode activated for maintenance demo | open |
| ALERT-092 | 2026-05-01 23:03:00 | TEST_SITE_028 | FIREWALL | medium | high memory usage alert on training cluster | resolved |
| ALERT-093 | 2026-05-01 10:20:00 | TEST_SITE_009 | UPS | medium | input voltage fluctuation detected in lab [Operator notes: contact engineer [REDACTED_PII] at [REDACTED_PII] if alert persists] | in-progress |
| ALERT-094 | 2026-05-02 03:20:00 | TEST_SITE_024 | ROUTER | low | BGP session flapping on simulated interface | open |
| ALERT-095 | 2026-05-02 00:45:00 | TEST_SITE_021 | BASE-STATION | medium | RF module communication failure in lab environment [Assigned to: [REDACTED_PII] ([REDACTED_PII])] | resolved |
| ALERT-096 | 2026-05-01 13:25:00 | TEST_SITE_003 | UPS | medium | high load capacity warning on bench unit [Operator notes: contact engineer [REDACTED_PII] at [REDACTED_PII] if alert persists] | resolved |
| ALERT-097 | 2026-05-01 15:43:00 | TEST_SITE_002 | FIREWALL | low | simulated brute force attack blocked on local port | open |
| ALERT-098 | 2026-05-01 17:10:00 | TEST_SITE_029 | FIREWALL | low | VPN tunnel down in test configuration | in-progress |
| ALERT-099 | 2026-05-01 17:11:00 | TEST_SITE_019 | FIREWALL | low | high memory usage alert on training cluster | resolved |
| ALERT-100 | 2026-05-01 09:09:00 | TEST_SITE_024 | BASE-STATION | low | power backup warning in training scenario | in-progress |
| ALERT-101 | 2026-05-02 03:44:00 | TEST_SITE_007 | UPS | **HIGH** | battery replacement warning (simulated) | resolved |
| ALERT-102 | 2026-05-02 00:46:00 | TEST_SITE_024 | SERVER | medium | unauthorized login attempt simulated in audit log [Contact: [REDACTED_PII] ([REDACTED_PII])] | in-progress |
| ALERT-103 | 2026-05-02 01:02:00 | TEST_SITE_027 | SWITCH | medium | spanning tree topology change detected | in-progress |
| ALERT-104 | 2026-05-01 23:22:00 | TEST_SITE_001 | BASE-STATION | low | synthetic VSWR alarm on sector antenna A | resolved |
| ALERT-105 | 2026-05-01 18:05:00 | TEST_SITE_030 | UPS | medium | bypass mode activated for maintenance demo | resolved |
| ALERT-106 | 2026-05-02 02:11:00 | TEST_SITE_025 | FIREWALL | **CRITICAL** | high memory usage alert on training cluster | resolved |
| ALERT-107 | 2026-05-01 14:13:00 | TEST_SITE_027 | ROUTER | low | BGP session flapping on simulated interface | resolved |
| ALERT-108 | 2026-05-01 11:33:00 | TEST_SITE_021 | FIREWALL | low | session table capacity warning (simulated) | resolved |
| ALERT-109 | 2026-05-01 17:08:00 | TEST_SITE_002 | UPS | low | input voltage fluctuation detected in lab [Contact: [REDACTED_PII] ([REDACTED_PII])] | open |
| ALERT-110 | 2026-05-02 03:47:00 | TEST_SITE_025 | BASE-STATION | medium | RF module communication failure in lab environment [Operator notes: contact engineer [REDACTED_PII] at [REDACTED_PII] if alert persists] | in-progress |
| ALERT-111 | 2026-05-01 10:23:00 | TEST_SITE_009 | ROUTER | medium | high CPU usage due to synthetic routing traffic | resolved |
| ALERT-112 | 2026-05-01 20:38:00 | TEST_SITE_004 | SWITCH | low | high broadcast traffic detected in lab subnet | resolved |
| ALERT-113 | 2026-05-02 02:35:00 | TEST_SITE_027 | SERVER | medium | unauthorized login attempt simulated in audit log | open |
| ALERT-114 | 2026-05-02 01:13:00 | TEST_SITE_006 | SERVER | medium | disk space low on simulated system drive | resolved |
| ALERT-115 | 2026-05-02 00:31:00 | TEST_SITE_005 | BASE-STATION | **CRITICAL** | power backup warning in training scenario [Ticket owner: [REDACTED_PII] - [REDACTED_PII]] | in-progress |
