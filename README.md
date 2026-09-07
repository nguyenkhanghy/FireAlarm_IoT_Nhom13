# Hệ Thống Cảm Biến Nguy cơ cháy nổ IoT - Nhóm 13

Hệ thống cảnh báo cháy sớm ứng dụng IoT, có khả năng giám sát môi trường theo thời gian thực thông qua cảm biến nhiệt độ, độ ẩm và khí gas. Thiết bị biên sử dụng ESP32 để thu thập, xử lý dữ liệu, tính toán mức độ rủi ro cháy và phát cảnh báo tại chỗ. Đồng thời, dữ liệu được truyền lên hệ thống trung tâm bằng giao thức MQTT để lưu trữ, giám sát và điều khiển từ xa.

---

# Tính năng

- Giám sát môi trường theo thời gian thực.
- Lọc nhiễu dữ liệu cảm biến.
- Tính tốc độ tăng nhiệt (Rate of Rise).
- Tính toán Risk Score theo nhiều yếu tố.
- Hoạt động theo Finite State Machine.
- Xử lý đa nhiệm không chặn bằng millis().
- Truyền dữ liệu thời gian thực bằng MQTT.
- Hỗ trợ điều khiển thiết bị từ xa.
- Dễ dàng mở rộng thêm cảm biến và chức năng.

---

# Mục tiêu của dự án

- Xây dựng hệ thống cảnh báo cháy sớm có chi phí thấp.
- Nâng cao độ chính xác bằng cách kết hợp nhiều cảm biến.
- Ứng dụng các kỹ thuật xử lý dữ liệu tại thiết bị biên.
- Kết nối hệ thống với nền tảng IoT để giám sát và điều khiển từ xa.
- Tạo nền tảng để phát triển các hệ thống cảnh báo thông minh trong tương lai.

---

# Thành viên và phân công

| Họ và tên | Vai trò | Công việc thực hiện |
|-----------|----------|---------------------|
| **Trần Minh Tiến** | Thuật toán và phần cứng | Nghiên cứu đặc tính cảm biến, xây dựng thuật toán lọc nhiễu (Moving Average), thiết kế mô hình tính điểm rủi ro (Risk Score), thiết kế và lắp ráp phần cứng. |
| **Nguyễn Khang Hy** | Thiết bị biên và tích hợp hệ thống | Lập trình ESP32 bằng C++, xây dựng Finite State Machine (FSM), xử lý các tác vụ không chặn (Non-blocking), xử lý tín hiệu ngoại vi, đóng gói dữ liệu JSON và truyền nhận MQTT. |
| **Vi Phương Nguyên** | Backend và Frontend | Xây dựng Backend bằng Golang, phát triển giao diện giám sát bằng ReactJS, xử lý điều khiển thiết bị từ xa thông qua MQTT. |

---

# Cấu trúc thư mục

```text
FireAlarm_IoT_Nhom13/
│
├── ESP32/
│   ├── sketch.ino
│   ├── diagram.json
│   ├── libraries.txt
│   └── wokwi-project.txt
│
├── Backend/
│
├── Frontend/
│
└── Hardware_Algorithms/
```

### Ý nghĩa các thư mục

### ESP32

Chứa toàn bộ chương trình điều khiển thiết bị biên.

- **sketch.ino:** Chương trình chính chạy trên ESP32.
- **diagram.json:** Sơ đồ kết nối phần cứng trên Wokwi.
- **libraries.txt:** Danh sách thư viện sử dụng.
- **wokwi-project.txt:** Đường dẫn mở dự án mô phỏng.

### Backend

Chứa chương trình xử lý dữ liệu trung tâm.

Các chức năng:

- Nhận dữ liệu từ MQTT Broker.
- Lưu dữ liệu.
- Xử lý yêu cầu từ giao diện.
- Gửi lệnh điều khiển xuống ESP32.

### Frontend

Giao diện giám sát hệ thống.

Các chức năng:

- Hiển thị nhiệt độ.
- Hiển thị độ ẩm.
- Hiển thị nồng độ khí gas.
- Hiển thị Risk Score.
- Hiển thị trạng thái cảnh báo.
- Điều khiển thiết bị từ xa.

### Hardware_Algorithms

Lưu tài liệu thiết kế phần cứng và mô tả các thuật toán xử lý.

---

# Công nghệ sử dụng

## Thiết bị

- ESP32
- DHT22
- MQ-2
- LED
- Buzzer
- Relay

## Ngôn ngữ lập trình

- C++
- Golang
- JavaScript

## Framework và thư viện

- ReactJS
- ArduinoJson
- DHTesp
- PubSubClient

## Giao thức truyền thông

- MQTT
- JSON

## Thuật toán

- Moving Average Filter
- Min-Max Normalization
- Weighted Risk Score
- Finite State Machine (FSM)
- Non-blocking (millis)

---

# Nguyên lý hoạt động

Quá trình xử lý của hệ thống diễn ra theo các bước sau:

1. ESP32 đọc dữ liệu từ cảm biến DHT22 và MQ-2.
2. Dữ liệu được lọc nhiễu bằng thuật toán Moving Average.
3. Tính tốc độ tăng nhiệt (Rate of Rise).
4. Chuẩn hóa các thông số bằng Min-Max Normalization.
5. Tính Risk Score dựa trên trọng số của từng thông số.
6. Xác định trạng thái hoạt động bằng Finite State Machine.
7. Điều khiển LED, Buzzer và Relay theo trạng thái.
8. Đóng gói dữ liệu thành JSON.
9. Gửi dữ liệu lên MQTT Broker định kỳ.

---

# Các trạng thái của hệ thống

| Trạng thái | Điều kiện | Phản hồi |
|------------|-----------|-----------|
| NORMAL | Risk Score thấp | Đèn xanh sáng |
| WARNING | Risk Score tăng | Đèn vàng sáng |
| DANGER | Risk Score cao | Đèn đỏ sáng, Buzzer kêu |
| EMERGENCY | Risk Score rất cao | Đèn đỏ nhấp nháy, Buzzer liên tục, Relay kích hoạt |

---

# Định dạng dữ liệu gửi lên MQTT

Ví dụ dữ liệu:

```json
{
  "temperature": 36.8,
  "humidity": 61.5,
  "gas": 42.3,
  "riskScore": 57.6,
  "state": "WARNING"
}
```

Dữ liệu được gửi định kỳ lên topic:

```text
nhom13/telemetry
```

Lệnh điều khiển từ hệ thống trung tâm được nhận tại topic:

```text
nhom13/command
```

---

# Hướng dẫn chạy mô phỏng

## Bước 1

Mở file

```
ESP32/wokwi-project.txt
```

và truy cập đường dẫn mô phỏng Wokwi.

---

## Bước 2

Nhấn nút **Play** để khởi động hệ thống.

---

## Bước 3

Thay đổi giá trị cảm biến bằng cách nhấn vào:

- DHT22
- MQ-2

để mô phỏng các tình huống cháy.

---

## Bước 4

Quan sát phản hồi trực tiếp trên mô hình:

- LED
- Buzzer
- Relay

Hệ thống sẽ tự động chuyển đổi giữa các trạng thái:

```text
NORMAL
      ↓
WARNING
      ↓
DANGER
      ↓
EMERGENCY
```

---

## Bước 5

Mở **Serial Monitor** để quan sát dữ liệu JSON được gửi lên MQTT Broker theo chu kỳ 3 giây.
