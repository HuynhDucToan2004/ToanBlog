---
title: "Tổng quan về lập trình mạng: Hành trình bắt đầu với những kết nối vô hình"
---

Hồi mới học lập trình, mình cứ nghĩ mạng máy tính chỉ là việc cắm dây Ethernet hoặc kết nối Wi-Fi. Nhưng khi bước vào thế giới lập trình mạng, mình nhận ra nó giống như một vũ trụ vô hình, nơi các máy tính “trò chuyện” với nhau qua những giao thức bí ẩn. Hôm nay, mình sẽ chia sẻ tổng quan về lập trình mạng – cánh cửa đầu tiên để bạn hiểu cách xây dựng ứng dụng kết nối toàn cầu.

#### Lập trình mạng là gì?
Lập trình mạng là nghệ thuật viết code để các thiết bị giao tiếp với nhau qua mạng (Internet, LAN, WAN). Nó bao gồm việc gửi/nhận dữ liệu, quản lý kết nối, và đảm bảo mọi thứ hoạt động mượt mà. Hãy tưởng tượng bạn đang điều khiển một “bưu điện số”: mỗi gói tin (packet) là một lá thư, và nhiệm vụ của bạn là đảm bảo thư được gửi đúng người, đúng thời điểm.

#### Các khái niệm cơ bản bạn cần nắm:
- **Giao thức (Protocol):** Quy tắc để máy tính hiểu nhau, như TCP, UDP, HTTP.
- **Socket:** Điểm kết nối để gửi/nhận dữ liệu.
- **Client-Server:** Mô hình phổ biến nơi client gửi yêu cầu và server trả lời.

#### Ví dụ thực tế: Một chat app đơn giản
Mình từng thử viết một ứng dụng chat đơn giản bằng Python. Code phía client gửi tin nhắn đến server:
```python
import socket

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(('localhost', 12345))
client.send("Xin chào, server!".encode())
response = client.recv(1024).decode()
print("Server trả lời:", response)
import socket

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(('localhost', 12345))
server.listen()
conn, addr = server.accept()
data = conn.recv(1024).decode()
print("Client gửi:", data)
conn.send("Chào client, mình nhận được rồi!".encode())

Chạy code này, mình thấy hai máy tính “nói chuyện” được với nhau, nhưng mình quên đóng socket, dẫn đến lỗi treo kết nối. Bài học: luôn kiểm tra tài liệu và đóng tài nguyên đúng cách!
 
 