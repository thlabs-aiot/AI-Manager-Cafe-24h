# Smart Cafe AI Monitor (Hệ thống Giám sát & Chống thất thoát F&B)

![Status](https://img.shields.io/badge/Status-In%20Development-yellow)
![Platform](https://img.shields.io/badge/Platform-Raspberry%20Pi%20%7C%20Web-blue)
![Tech](https://img.shields.io/badge/Tech-Python%20%7C%20OpenCV%20%7C%20FastAPI%20%7C%20React-green)

> **Mô tả:** Hệ thống AI biên (Edge AI) chạy trên Raspberry Pi giúp tự động đếm số lượng ly nước bán ra thông qua Camera, đo kích thước miệng ly để xác định giá tiền, và đối chiếu thời gian thực với dữ liệu từ máy POS/Bill để phát hiện gian lận hoặc sai sót.

---

## 1. Đặt Vấn Đề (Problem)
Việc quản lý chuỗi cửa hàng Cafe 24h gặp khó khăn lớn trong việc kiểm soát doanh thu thực tế:
- Chủ quán không thể có mặt 24/7.
- Camera an ninh thông thường chỉ để "xem lại" khi sự việc đã rồi.
- Nhân viên có thể cố tình không in bill để thu tiền riêng (thất thoát doanh thu).

## 2. Giải Pháp (Solution)
Xây dựng hệ thống giám sát tự động gồm:
1.  **Camera Top-Down:** Soi vuông góc xuống quầy ra món.
2.  **Core Algorithm:** Sử dụng OpenCV đo đường kính miệng ly (độ chính xác <2mm) để phân loại Size (S/M/L,..) tương ứng với giá tiền dự tính sẽ có 15 loại ly khác nhau.
3.  **POS Listener:** Bắt tín hiệu in bill từ máy thu ngân.
4.  **Logic Engine:** Đối chiếu `[Ly thực tế]` vs `[Bill đã in]`. Nếu lệch -> **CẢNH BÁO**.

---

## 📅 3. Lộ Trình Phát Triển (Development Roadmap)
**Thời gian dự kiến:** 3 Tháng (14/12/2025 - 14/03/2026)

### Giai đoạn 1: Core Engine & Hardware Setup (Tháng 1)
*Thời gian: 14/12/2025 - 14/01/2026*
**Mục tiêu:** Camera nhận diện và đo được kích thước ly chính xác trên RPi.

- [ ] **Tuần 1: Setup Môi trường & Phần cứng**
    - [ ] Lắp ráp khung giá đỡ Camera (Rig) vuông góc 90 độ.
    - [ ] Thi công thảm nền màu Đen Nhám (Matte Black) để khử nhiễu phản xạ.
    - [ ] Cài đặt OS Raspberry Pi 64-bit, OpenCV, Python env.
- [ ] **Tuần 2: Thuật toán Đo lường (Measurement)**
    - [ ] Code module `Auto-Calibration`: Tự tính tỷ lệ Pixel/CM qua vật tham chiếu.
    - [ ] Code module `Cup-Detector`: Dùng Hough Circle Transform bắt miệng ly.
    - [ ] Xử lý nhiễu: Lọc bỏ các vòng tròn giả, ổn định kết quả đo (Moving Average).
- [ ] **Tuần 3: Phân loại & Dữ liệu mẫu**
    - [ ] Đo đạc thực tế mẫu ly để set ngưỡng (Threshold).
    - [ ] Test độ chính xác với các loại nước khác nhau (đen, sữa, nước ép).
- [ ] **Tuần 4: Đóng gói Core Service**
    - [ ] Viết API local trên RPi (FastAPI) trả về kết quả JSON realtime.

### Giai đoạn 2: POS Integration & Logic (Tháng 2)
*Thời gian: 15/01/2026 - 14/02/2026*
**Mục tiêu:** Hệ thống biết "so sánh" và phát hiện lỗi.

- [ ] **Tuần 5: Kết nối máy POS**
    - [ ] Nghiên cứu giao thức máy in (ESC/POS) hoặc API phần mềm bán hàng.
    - [ ] Viết module `POS-Listener` để bắt dữ liệu Bill (Món, Size, Time).
- [ ] **Tuần 6: Xây dựng Logic "Matching"**
    - [ ] Code State Machine: `Idle` -> `Detected` -> `Waiting_Bill` -> `Valid/Violation`.
    - [ ] Xử lý độ trễ: Cho phép in bill trễ X giây so với lúc ra nước.
- [ ] **Tuần 7: Local Database & Storage**
    - [ ] Thiết kế SQLite Schema: `Events`, `Violations`, `DailyStats`.
    - [ ] Cơ chế lưu ảnh bằng chứng (Snapshot) khi phát hiện lỗi.
- [ ] **Tuần 8: Cảnh báo tại chỗ (On-site Alert)**
    - [ ] Tích hợp Loa/Đèn báo động vào GPIO của RPi.
    - [ ] Test quy trình: Không bill -> Hú còi.

### Giai đoạn 3: Cloud Dashboard & Deployment (Tháng 3)
*Thời gian: 15/02/2026 - 14/03/2026*
**Mục tiêu:** Chủ quán quản lý từ xa & Triển khai thực tế.

- [ ] **Tuần 9: Cloud Backend (Sync)**
    - [ ] Xây dựng Cloud DB (Firebase/Postgres).
    - [ ] Viết Worker đồng bộ dữ liệu từ RPi lên Cloud (tối ưu băng thông).
- [ ] **Tuần 10: Web Dashboard (Frontend)**
    - [ ] Code giao diện Dashboard (ReactJS): Biểu đồ doanh thu, List vi phạm.
    - [ ] Tính năng "Playback": Xem lại ảnh/clip lúc xảy ra vi phạm.
- [ ] **Tuần 11: Cấu hình từ xa**
    - [ ] Tính năng cập nhật kích thước ly (Config) từ Web xuống RPi.
- [ ] **Tuần 12: Đóng gói & UAT**
    - [ ] Thiết kế vỏ hộp in 3D bảo vệ RPi.
    - [ ] Chạy thử nghiệm 24h liên tục tại quán.
    - [ ] Fix bug & Viết tài liệu hướng dẫn sử dụng.

---

## 🛠 Tech Stack

### Hardware (Edge)
* **MCU:** Raspberry Pi 5 + AI Kit.
* **Camera:** Module Camera Raspberry và C906 AI Vision .
* **Sensor:** nếu cần sẽ phát sinh tìm sensor.
* **Mount:** Khung nhôm định hình & Tấm lót cao su đen,....

### Software
* **Core AI:** Python 3.9+, OpenCV (Computer Vision).
* **Local Backend:** FastAPI, SQLite.
* **Cloud/Web:** ReactJS (Frontend), Firebase/Supabase (DB & Auth).
* **Deployment:** Docker (cho RPi), GitHub Actions.

---

## Yêu cầu Triển khai (Installation Requirements)

Để thuật toán hoạt động chính xác >95%, điểm bán cần tuân thủ:
1.  **Vị trí:** Camera lắp cố định, vuông góc 90 độ so với mặt bàn.
2.  **Ánh sáng:** Đủ sáng, không bị lóa trực tiếp vào miệng ly (dùng đèn tản sáng).
3.  **Vật tư:**
    * Sử dụng thảm lót màu tối.
    * Các size ly phải có đường kính chênh lệch tối thiểu **0.5cm - 1cm**.
    * Có dán vật tham chiếu (Calibration mark) trên bàn.

---

## Contribution
Dự án được phát triển bởi Võ Ngọc Tân. Mọi đóng góp vui lòng tạo Pull Request hoặc Issue.

## 📄 License
TH Labs.
