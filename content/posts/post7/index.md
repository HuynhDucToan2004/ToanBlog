
---
title: "Lập trình mạng cho giao thức UDP: Nhanh nhưng không chắc"
---

Sau khi làm việc với TCP, mình muốn thử một giao thức khác: UDP. Nghe nói UDP nhanh hơn, nhưng mình nhanh chóng nhận ra “nhanh” đi kèm với rủi ro. Hôm nay, mình sẽ chia sẻ về lập trình mạng với UDP và những lần mình “mất gói tin” trên hành trình.

#### UDP là gì?
UDP (User Datagram Protocol) giống như gửi thư không bảo đảm: nhanh, nhưng không đảm bảo thư đến đúng hay đầy đủ. Nó phù hợp cho ứng dụng cần tốc độ, như streaming hoặc game online.

#### Ví dụ thực tế: Gửi tin nhắn với UDP
Mình viết một chương trình UDP đơn giản:
```python
# Server
import socket

server = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server.bind(('localhost', 12345))

while True:
    data, addr = server.recvfrom(1024)
    print(f"Nhận từ {addr}: {data.decode()}")
    server.sendto("Hello from server!".encode(), addr)

# Client
import socket

client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
client.sendto("Hello from client!".encode(), ('localhost', 12345))
data, addr = client.recvfrom(1024)
print("Server:", data.decode())

```
Mình từng gửi dữ liệu quá nhanh, dẫn đến mất gói tin. Bài học: UDP cần cơ chế kiểm tra bổ sung nếu dữ liệu quan trọng.
Bài học từ UDP
UDP giống như chạy xe máy: nhanh nhưng cần cẩn thận. Mình học được rằng UDP phù hợp cho ứng dụng ưu tiên tốc độ, nhưng phải tự xử lý lỗi. Hãy thử viết một chương trình UDP để cảm nhận sự khác biệt với TCP!