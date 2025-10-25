---
title: "Quản lý các luồng nhập xuất: Khi dữ liệu không chịu đứng yên"
---

Lần đầu làm việc với lập trình mạng, mình bị choáng ngợp bởi cách dữ liệu “chạy” qua lại giữa client và server. Mình từng nghĩ cứ gửi dữ liệu là xong, nhưng hóa ra, quản lý luồng nhập/xuất (I/O) lại là một câu chuyện hoàn toàn khác. Hôm nay, mình sẽ chia sẻ về luồng I/O trong lập trình mạng và cách mình học được từ những lần “đuối” với dữ liệu.

#### Luồng nhập xuất là gì?
Luồng I/O (Input/Output) là cách dữ liệu di chuyển vào và ra khỏi chương trình của bạn. Trong lập trình mạng, I/O thường liên quan đến việc đọc dữ liệu từ socket (input) và gửi dữ liệu đi (output). Nhưng vấn đề là dữ liệu không đến một cách đều đặn – nó có thể đến từng mẩu, hoặc bị tắc nghẽn.

#### Hai mô hình I/O phổ biến:
- **Đồng bộ (Blocking I/O):** Chương trình chờ cho đến khi dữ liệu được xử lý xong.
- **Bất đồng bộ (Non-blocking I/O):** Chương trình tiếp tục chạy mà không cần chờ.

#### Ví dụ thực tế: Đọc dữ liệu từ socket
Mình từng viết một server nhận tin nhắn từ nhiều client. Ban đầu, mình dùng blocking I/O:
```python
import socket

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(('localhost', 12345))
server.listen()

while True:
    conn, addr = server.accept()
    data = conn.recv(1024).decode()  # Chờ dữ liệu, có thể gây treo
    print(f"Nhận từ {addr}: {data}")

``` 

Vấn đề Server chỉ xử lý một client tại một thời điểm. Nếu client gửi chậm, server bị “kẹt”. Mình chuyển sang dùng select để quản lý nhiều kết nối:
python

```import select

sockets_list = [server]
while True:
    read_sockets, _, _ = select.select(sockets_list, [], [])
    for sock in read_sockets:
        if sock == server:
            conn, addr = server.accept()
            sockets_list.append(conn)
        else:
            data = sock.recv(1024).decode()
            print(f"Nhận từ {sock.getpeername()}: {data}")

```

Cách này giúp server xử lý nhiều client cùng lúc mà không bị treo.

Bài học rút ra
Quản lý I/O giống như điều khiển giao thông: bạn phải biết khi nào để xe chạy, khi nào dừng. Mình học được rằng cần chọn đúng mô hình I/O (đồng bộ hay bất đồng bộ) tùy vào ứng dụng. Nếu bạn đang làm việc với nhiều kết nối, hãy thử select hoặc asyncio để tránh tắc nghẽn.
Bạn đã từng gặp vấn đề với luồng I/O chưa? Chia sẻ với mình nhé!