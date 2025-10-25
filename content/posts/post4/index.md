---
title: "Quản lý địa chỉ kết nối mạng: Hành trình tìm đúng “nhà”"
---

Khi mới học lập trình mạng, mình cứ nghĩ chỉ cần biết địa chỉ IP là đủ để kết nối. Nhưng rồi mình nhận ra: quản lý địa chỉ mạng không chỉ là IP, mà còn là port, giao thức, và cả cách tìm “nhà” đúng trong thế giới mạng. Hôm nay, mình sẽ chia sẻ về cách quản lý địa chỉ kết nối mạng và những lần mình “lạc đường”.

#### Địa chỉ mạng là gì?
Địa chỉ mạng giống như địa chỉ nhà: nó gồm IP (như số nhà) và port (như phòng cụ thể). Ví dụ:
- IP: 192.168.1.1 xác định máy tính.
- Port: 8080 xác định ứng dụng trên máy đó.

Ngoài ra, bạn cần biết:
- **IPv4 vs IPv6:** IPv4 (như 192.168.1.1) phổ biến, nhưng IPv6 đang dần thay thế vì có nhiều địa chỉ hơn.
- **DNS:** Chuyển tên miền (như google.com) thành IP.

#### Ví dụ thực tế: Kết nối với sai port
Mình từng cố kết nối đến một server:
```python
import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(('localhost', 8080))  # Sai port!

```
Hóa ra server chạy trên port 12345, nên mình nhận lỗi “Connection refused”. Mình phải kiểm tra cấu hình server và dùng netstat để xem port đang mở. Bài học: luôn xác nhận IP và port trước khi kết nối!
Bài học từ quản lý địa chỉ
Quản lý địa chỉ mạng giống như tìm đường trong thành phố lớn: bạn cần bản đồ (DNS), số nhà (IP), và phòng đúng (port). Mình học được rằng công cụ như ping hay nslookup là bạn thân khi debug kết nối.