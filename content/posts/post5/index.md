

---
title: "Lập trình socket cho giao thức TCP: Xây cầu nối bền vững"
---

Lần đầu viết chương trình mạng, mình chọn TCP vì nghe nói nó “đáng tin cậy”. Nhưng khi code, mình nhận ra TCP không chỉ là gửi dữ liệu – nó giống như xây một cây cầu chắc chắn giữa hai máy tính. Hôm nay, mình sẽ chia sẻ về lập trình socket với TCP và những bài học từ những lần “cầu sập”.

#### TCP và socket là gì?
TCP (Transmission Control Protocol) đảm bảo dữ liệu được gửi đến đúng nơi, đúng thứ tự, không bị mất. Socket là “đầu nối” để gửi/nhận dữ liệu qua TCP.

#### Quy trình cơ bản:
- Server tạo socket, bind với IP/port, và lắng nghe.
- Client kết nối đến server.
- Hai bên gửi/nhận dữ liệu.

#### Ví dụ thực tế: Chat với TCP
Mình từng viết một ứng dụng chat TCP:
```python
# Server
import socket

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(('localhost', 12345))
server.listen()
conn, addr = server.accept()
data = conn.recv(1024).decode()
print("Nhận:", data)
conn.send("Hello from server!".encode())
conn.close()

# Client
import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(('localhost', 12345))
client.send("Hello from client!".encode())
response = client.recv(1024).decode()
print("Server:", response)
client.close()

```
Mình quên đóng socket, dẫn đến lỗi tài nguyên. Bài học: luôn đóng socket sau khi dùng!
Bài học từ TCP
TCP giống như gửi thư bảo đảm: chậm nhưng chắc. Mình học được rằng cần kiểm tra trạng thái kết nối và xử lý lỗi cẩn thận. Nếu bạn muốn thử, hãy viết một chat app TCP – nó đơn giản nhưng dạy bạn rất nhiều!