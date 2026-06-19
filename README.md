# 📸 ESP32-CAM Object Detection with Edge Impulse & OLED

Dự án này sử dụng module **ESP32-CAM** kết hợp với mô hình Machine Learning được huấn luyện trên nền tảng **Edge Impulse** để thực hiện nhận diện vật thể (Object Detection). Kết quả nhận diện (Tên vật thể và độ chính xác) sẽ được hiển thị trực tiếp lên màn hình **OLED SSD1306**.

## ✨ Tính năng chính
* **Nhận diện thời gian thực:** Chụp ảnh liên tục và chạy suy luận (inference) trực tiếp trên vi điều khiển ESP32.
* **Tích hợp Edge Impulse:** Dễ dàng thay thế bằng bất kỳ mô hình nhận diện hình ảnh nào được export từ Edge Impulse.
* **Hiển thị trực quan:** Kết quả (Label & Độ chính xác % của bounding box lớn nhất) được in ra màn hình OLED.
* **Hỗ trợ Debug:** Ghi nhận log chi tiết (thời gian xử lý DSP, Classification, Anomaly) qua Serial Monitor.

## 🛠️ Yêu cầu phần cứng
1. **ESP32-CAM** (Mặc định trong code sử dụng module `AI_THINKER`).
2. **Màn hình OLED 0.96"** (IC điều khiển SSD1306, giao tiếp I2C).
3. **Mạch nạp code** (Ví dụ: FT232RL, CH340...) để nạp code cho ESP32-CAM.
4. Dây cắm testboard.

## 🔌 Sơ đồ đấu nối (Wiring)

Do ESP32-CAM không có sẵn chân I2C mặc định, dự án này đã định nghĩa lại chân I2C để giao tiếp với màn hình OLED như sau:

| ESP32-CAM | OLED SSD1306 | Ghi chú |
| :--- | :--- | :--- |
| **GPIO 15** | **SDA** | Chân dữ liệu I2C |
| **GPIO 14** | **SCL** | Chân xung nhịp I2C |
| **3.3V / 5V** | **VCC** | Nguồn cấp cho màn hình |
| **GND** | **GND** | Chân nối đất |

## 📚 Yêu cầu phần mềm & Thư viện
Đảm bảo bạn đã cài đặt ESP32 Board Manager trên Arduino IDE. Bạn cần cài đặt các thư viện sau thông qua Library Manager (`Sketch` -> `Include Library` -> `Manage Libraries`):

* `Adafruit GFX Library`
* `Adafruit SSD1306`

**Thư viện Edge Impulse:**
* Bạn cần thêm thư viện dự án của riêng bạn dưới dạng file `.zip` (Tải từ tab Deployment trên Edge Impulse). 
* *Lưu ý:* Trong code hiện tại đang include `<quyphu123-project-1_inferencing.h>`. Nếu bạn sử dụng mô hình khác, hãy nhớ đổi tên file header này tương ứng.

## 🚀 Hướng dẫn cài đặt và sử dụng

1. **Kết nối phần cứng:** Cấp nguồn và nối dây theo bảng sơ đồ phía trên.
2. **Mở Arduino IDE:** Mở file code `.ino` của dự án.
3. **Cấu hình Board:** * Chọn Board: `AI Thinker ESP32-CAM`
   * Chọn Partition Scheme: **`Huge APP (3MB No OTA/1MB SPIFFS)`** *(Bắt buộc để đủ dung lượng bộ nhớ cho mô hình AI).*
4. **Nạp Code (Upload):** * Nối chân `IO0` xuống `GND`.
   * Nhấn nút `RST` trên ESP32-CAM.
   * Bấm nút Upload trên Arduino IDE.
   * Sau khi nạp xong, rút dây `IO0` ra khỏi `GND` và nhấn lại `RST`.
5. **Theo dõi kết quả:** Mở Serial Monitor ở baudrate `115200` để xem quá trình khởi động camera. Sau 2 giây, hệ thống sẽ bắt đầu nhận diện và hiển thị kết quả lên màn hình OLED.

## 📝 Troubleshooting (Khắc phục sự cố)
* **Lỗi `Camera init failed`:** Kiểm tra lại nguồn cấp (ESP32-CAM rất nhạy cảm với nguồn yếu, nên dùng nguồn 5V ổn định từ module nạp hoặc nguồn ngoài) và đảm bảo bạn đã chọn đúng `CAMERA_MODEL_AI_THINKER`.
* **Màn hình OLED không sáng:** Kiểm tra lại chân SDA (15) và SCL (14) xem có bị lỏng không, hoặc địa chỉ I2C của màn hình có đúng là `0x3C` hay không.
* **Lỗi thiếu RAM/Failed to allocate snapshot buffer:** Việc cấp phát mảng hình ảnh đòi hỏi PSRAM. Hãy chắc chắn board của bạn có PSRAM và đã được kích hoạt trong menu `Tools` của Arduino IDE.

---
*Mã nguồn gốc được tham khảo từ CircuitDigest, sau đó được cấu trúc lại và bổ sung tính năng hiển thị OLED.*
