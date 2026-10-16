# Phần 1. Phân tích Hệ thống (Input / Output & Sơ đồ phần cứng)

## 1. Xác định Input / Output cho Hệ thống Trang trại Lan
- **Input (Đầu vào):** 
  - Cảm biến độ ẩm đất (`Soil Moisture Sensor`).
  - Cảm biến nhiệt độ và độ ẩm không khí (`DHT22 Sensor`).
  - Cảm biến ánh sáng (`Photoresistor/LDR`).
  - Lệnh điều khiển thủ công từ người nông dân qua ứng dụng hoặc nút bấm vật lý.
- **Output (Đầu ra):**
  - Van nước điện từ tưới phun sương (`Solenoid Valve`).
  - Hệ thống đèn LED kích thích sinh trưởng (`LED Grow Lights`).
  - Màn hình hiển thị thông số LCD/OLED và chuông cảnh báo.

## 2. Sơ đồ phối hợp phần cứng (System Architecture)
![Sơ đồ kiến trúc phần cứng Smart-Farm tích hợp IoT cho trang trại Lan](https://images.unsplash.com/photo-1586771107445-d3ca888129ff?q=80&w=1000&auto=format&fit=crop)

---

# Phần 2. Triển khai (File `README.md` chuyên nghiệp)

# 🚀 Smart-Farm IoT: Hệ Thống Tự Động Hóa Trang Trại Lan

*Hệ thống nông nghiệp thông minh ứng dụng công nghệ IoT và Điện toán biên, giúp tối ưu hóa quy trình chăm sóc hoa lan, tiết kiệm tài nguyên nước và nâng cao năng suất mùa vụ cho nhà nông.*

---

## 📋 1. Mô tả tính năng chính
- [x] Tự động đo đạc độ ẩm đất và kích hoạt tưới nước khi dưới ngưỡng cho phép.
- [x] Theo dõi thời gian thực nhiệt độ và độ ẩm không khí trong vườn lan.
- [ ] Tự động điều chỉnh hệ thống đèn chiếu sáng dựa trên cường độ ánh sáng ngoài trời *(Đang phát triển)*.
- [x] Cảnh báo thông minh qua màn hình LCD khi các thông số vượt ngưỡng an toàn.

---

## ⚙️ 2. Hướng dẫn cài đặt hệ thống
Thực hiện các lệnh sau trên trạm điều khiển trung tâm (Raspberry Pi/ESP32) để khởi chạy hệ thống:

```bash
# Cập nhật hệ thống và các gói phụ thuộc
sudo apt update && sudo apt upgrade -y

# Clone mã nguồn dự án Smart-Farm từ kho lưu trữ
git clone https://github.com/smart-farm/orchid-iot.git
cd orchid-iot

# Cài đặt môi trường và các thư viện cảm biến cần thiết
pip install -r requirements.txt

# Khởi chạy ứng dụng giám sát trang trại
python main.py
```

---

## 📊 3. Bảng thông số kỹ thuật phần cứng

| Linh kiện / Cảm biến | Model chuẩn | Thông số hoạt động | Chức năng chính |
| :--- | :--- | :--- | :--- |
| **Vi điều khiển (SoC)** | ESP32 NodeMCU | 240MHz, Wi-Fi/BLE | Xử lý trung tâm & truyền dữ liệu |
| **Cảm biến độ ẩm đất** | Capacitive v1.2 | 3.3V - 5V DC | Đo lượng nước trong giá thể trồng lan |
| **Cảm biến môi trường** | DHT22 | $-40^\circ\text{C}$ tới $+80^\circ\text{C}$ | Đo nhiệt độ và độ ẩm không khí |
| **Van điện từ** | 12V Solenoid | 12V DC, 300mA | Đóng/mở nguồn nước tưới tự động |

---

## 🔄 4. Chu kỳ xử lý lệnh tưới Lan tại CPU (Fetch – Decode – Execute)
> Quá trình CPU thực thi lệnh bật van tưới nước được chia thành các giai đoạn chặt chẽ sau:
> 1. **Fetch (Đọc lệnh):** CPU truy xuất ô nhớ chứa mã lệnh bật van nước từ bộ nhớ chính (RAM) thông qua thanh ghi bộ đếm chương trình (Program Counter).
> 2. **Decode (Giải mã lệnh):** Bộ giải mã lệnh trong CPU phân tích mã nhị phân vừa nạp, xác định rằng đây là yêu cầu kích hoạt chân GPIO điều khiển van điện từ.
> 3. **Execute (Thực thi lệnh):** CPU gửi tín hiệu điện áp mức cao ($3.3\text{V}$) đến chân chân nguồn của relay để mở van nước, đồng thời cập nhật trạng thái hệ thống vào thanh ghi.

---

## 🏔️ 5. Mô hình tháp tri thức DIKW trong trang trại Lan
Quy trình từ lúc thu thập tín hiệu thô đến khi ra quyết định kinh doanh hoặc vận hành:

- 🧠 **Wisdom (Tri thức):** *"Thời điểm lý tưởng nhất để tưới lan là sáng sớm (trước $7\text{h}$) khi nhiệt độ chưa quá cao và độ ẩm giữ được lâu nhất, giúp cây không bịốc nhiệt."*
- 💡 **Knowledge (Hiểu biết):** *"Nếu nhiệt độ không khí vượt quá $32^\circ\text{C}$ và độ ẩm đất giảm dưới $40\%$, hoa lan sẽ bị mất nước và chậm phát triển."*
- 📈 **Information (Thông tin):** *"Nhiệt độ khu vực nhà kính A đang ở mức $35^\circ\text{C}$ và độ ẩm đất là $30\%$ vào lúc $12\text{h}$ trưa."*
- 📊 **Data (Dữ liệu thô):** Chuỗi giá trị thô từ cảm biến: `[DHT22: 35.2, Soil: 30%, Timestamp: 12:00:05]`

---

## ⚠️ 6. Lưu ý vận hành an toàn hệ thống
- Kiểm tra định kỳ nguồn điện $12\text{V}$ cấp cho van điện từ để tránh hiện tượng sụt áp.
- **Xử lý sự cố tràn RAM do lưu trữ nhật ký (Log Full):** 
  - Khi RAM bị đầy do lưu quá nhiều dữ liệu cảm biến, hệ thống có thể bị treo. 
  - *Cách xử lý an toàn:* Tiến hành cấu hình cơ chế tự động xoay vòng file log (`logrotate`) hoặc định kỳ xuất file log cũ ra thẻ nhớ SD/Cloud, sau đó thực hiện lệnh xóa cache bộ nhớ tạm (`sudo sync && sudo sysctl -w vm.drop_caches=3`) để giải phóng không gian RAM mà không làm mất nguồn nuôi cảm biến.