---
title: "Lập trình đa tuyến: Khi mọi thứ cần chạy cùng lúc"
---

Hồi mới học lập trình mạng, mình từng nghĩ một chương trình chỉ cần chạy tuần tự là đủ. Nhưng khi làm một ứng dụng chat, mình nhận ra: nếu chỉ chạy một luồng (thread), server sẽ “đơ” khi có nhiều client kết nối. Đó là lúc mình khám phá lập trình đa tuyến (multithreading). Hôm nay, mình sẽ chia sẻ về cách multithreading giúp ứng dụng mạng “thở” dễ hơn.

#### Lập trình đa tuyến là gì?
Multithreading là cách chia chương trình thành nhiều luồng chạy song song. Trong lập trình mạng, mỗi luồng có thể xử lý một kết nối, giúp server phục vụ nhiều client cùng lúc. Hãy tưởng tượng bạn là một đầu bếp trong nhà hàng: nếu chỉ nấu một món tại một thời điểm, khách sẽ đợi lâu. Multithreading giống như có nhiều đầu bếp làm việc cùng lúc.

#### Ví dụ thực tế: Server đa tuyến
Mình từng viết một server chat đơn giản, nhưng chỉ một client có thể gửi tin nhắn tại một thời điểm. Mình chuyển sang dùng multithreading:
```python
import socket
import threading

def handle_client(conn, addr):
    while True:
        data = conn.recv(1024).decode()
        if not data:
            break
        print(f"Nhận từ {addr}: {data}")
        conn.send(f"Echo: {data}".encode())
    conn.close()

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(('localhost', 12345))
server.listen()

while True:
    conn, addr = server.accept()
    thread = threading.Thread(target=handle_client, args=(conn, addr))
    thread.start()

```
Mỗi client được xử lý trong một thread riêng, giúp server hoạt động mượt mà hơn. Nhưng mình từng quên khóa (lock) khi ghi log, dẫn đến output lộn xộn. Bài học: luôn cẩn thận với tài nguyên chia sẻ trong multithreading!
Bài học từ đa tuyến
Multithreading giống như làm việc nhóm: mọi người phải phối hợp nhịp nhàng. Mình học được rằng cần hiểu rõ cách thread giao tiếp và tránh các vấn đề như deadlock. Nếu bạn mới bắt đầu, hãy thử viết một server đa tuyến đơn giản – bạn sẽ thấy lập trình mạng thú vị hơn nhiều!
Bạn đã thử multithreading chưa? Hãy chia sẻ trải nghiệm của bạn nhé!