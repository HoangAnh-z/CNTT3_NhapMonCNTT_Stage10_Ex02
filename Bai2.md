# Hướng dẫn cài đặt cảm biến độ ẩm cho trang trại Lan

## 1. Tổng quan

Cảm biến độ ẩm giúp theo dõi độ ẩm không khí trong nhà lưới, đảm bảo môi trường tối ưu cho Lan phát triển. Dữ liệu từ cảm biến cũng được dùng để tự động bật hoặc tắt máy bơm phun sương khi độ ẩm xuống thấp.

### Thiết bị cần chuẩn bị

- Cảm biến độ ẩm (DHT22 / AM2302)
- Raspberry Pi (hoặc Jetson / SoC tương đương)
- Dây nối Dupont
- Module relay 5V để điều khiển máy bơm
- Tua vít, băng keo, ốc vít
- Thẻ nhớ đã cài hệ điều hành

---

## 2. Các bước cài đặt

### Bước 1. Cập nhật hệ thống

Mở Terminal và cập nhật danh sách gói:

`sudo apt update && sudo apt upgrade -y`

### Bước 2. Cài đặt thư viện cho cảm biến DHT22

Cài đặt thư viện đọc dữ liệu từ cảm biến. Dùng môi trường ảo ***venv*** để không xung đột với hệ thống.

```bash
sudo apt install -y git python3-pip python3-venv libgpiod2
python3 -m venv ~/farm-env
source ~/farm-env/bin/activate
pip install adafruit-circuitpython-dht RPi.GPIO
```

### Bước 3. Kích hoạt giao tiếp 1-Wire (chỉ khi dùng cảm biến 1-Wire)

Bật 1-Wire trên hệ thống.

```bash
sudo raspi-config
# Chọn: Interface Options 
# > 1-Wire > Enable
```

### Bước 4. Kiểm tra module được nhận (chỉ với cảm biến 1-Wire)

Sau khi bật 1-Wire, kiểm tra thiết bị.

`ls /sys/bus/w1/devices/        # Kết quả có dạng 28-xxxxxxx`

### Bước 5. Chạy thử chương trình đọc cảm biến

Tạo file `read_dht22.py` với nội dung sau.

```bash
git clone https://github.com/adafruit/Adafruit_Python_DHT.git
cd Adafruit_Python_DHT/examples
```

### Bước 6. Tự động chạy khi khởi động ( Tuỳ chọn )
Thêm vào contab để đọc dữ liệu định kỳ.

`contab -e`


## 3. Sơ đồ kết nối

| DHT22 | Raspberry Pi (BCM) |
| --- | --- |
| `VCC` | 3.3V (Pin 1) |
| `GND` | GND (Pin 6) |
| `DATA` | `GPIO4` (Pin 7) |
| `NC` | (Không kết nối) |

---

## 4. Vị trí lắp đặt

Lắp cảm biến ở nơi thoáng, tránh ánh nắng trực tiếp và nước bắn vào.

---

## 5. Kiểm tra kết quả

Kết quả mẫu khi chạy lệnh ở bước 5:

```text
Temp=28.4*C  Humidity=67.1%
pi@farm:~ $
```



## 7. So sánh hai loại cảm biến: DHT11 (loại cũ) và DHT22 (loại mới)

| Tiêu chí | Loại cũ: `DHT11` | Loại mới: `DHT22` (AM2302) |
| --- | --- | --- |
| Dải đo độ ẩm | 20% đến 80% RH | 0% đến 100% RH |
| Sai số độ ẩm | ±5% RH | ±2% đến ±5% RH |
| Dải đo nhiệt độ | 0 đến 50 °C | -40 đến 80 °C |
| Sai số nhiệt độ | ±2 °C | ±0.5 °C |
| Độ phân giải | 1% RH, 1 °C | 0.1% RH, 0.1 °C |
| Tần suất đọc tối đa | 1 lần / giây | 1 lần / 2 giây |
| Giao tiếp | Một dây (giao thức riêng) | Một dây (giao thức riêng) |
| Giá | Rẻ nhất | Cao hơn một chút |
| Phù hợp nhà lưới Lan | Hạn chế: dải ẩm 20% đến 80% sát giới hạn, sai số lớn | Tốt hơn: đo được cả vùng ẩm cao, chính xác hơn |

---

## 8. Danh sách kiểm tra thiết bị (Task List)

- [ ] Cảm biến độ ẩm (DHT22 / AM2302)
- [ ] Raspberry Pi (hoặc Jetson / SoC tương đương)
- [ ] Dây nối Dupont
- [ ] Module relay 5V để điều khiển máy bơm
- [ ] Tua vít, băng keo, ốc vít
- [ ] Thẻ nhớ đã cài hệ điều hành
---
