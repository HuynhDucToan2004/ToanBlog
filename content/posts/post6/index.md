
---
title: "Kỹ thuật đa tiến trình và tuần tự hóa đối tượng: Khi mạng cần chia sẻ"
---

Khi làm việc với ứng dụng mạng, mình từng nghĩ multithreading là đủ để xử lý nhiều kết nối. Nhưng rồi mình phát hiện đa tiến trình (multiprocessing) và tuần tự hóa đối tượng (serialization) lại là “vũ khí” mạnh mẽ hơn trong một số trường hợp. Hôm nay, mình sẽ chia sẻ về cách hai kỹ thuật này giúp ứng dụng mạng mạnh mẽ hơn.

#### Đa tiến trình và tuần tự hóa là gì?
- **Đa tiến trình:** Mỗi tiến trình chạy độc lập, có bộ nhớ riêng, khác với thread chia sẻ bộ nhớ.
- **Tuần tự hóa:** Chuyển đối tượng thành dạng dữ liệu (như JSON, pickle) để gửi qua mạng.

#### Ví dụ thực tế: Gửi đối tượng qua mạng
Mình từng thử gửi một dictionary phức tạp qua mạng:
```python
import socket
import pickle
import multiprocessing

def handle_client(conn):
    data = conn.recv(1024)
    obj = pickle.loads(data)  # Tuần tự hóa ngược
    print("Nhận:", obj)
    conn.send(pickle.dumps({"response": "OK"}))
    conn.close()

server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(('localhost', 12345))
server.listen()

while True:
    conn, addr = server.accept()
    process = multiprocessing.Process(target=handle_client, args=(conn,))
    process.start()

```
Client gửi dictionary:
```
import socket
import pickle

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(('localhost', 12345))
data = {"name": "Hiếu", "age": 25}
client.send(pickle.dumps(data))  # Tuần tự hóa
response = pickle.loads(client.recv(1024))
print("Server:", response)
client.close()
```
Mình từng quên kiểm tra kích thước dữ liệu, dẫn đến lỗi khi gửi đối tượng lớn. Bài học: luôn giới hạn và kiểm tra dữ liệu trước khi tuần tự hóa.
Bài học rút ra
Đa tiến trình và tuần tự hóa giống như chia đội và đóng gói hàng hóa: mỗi đội làm việc độc lập, nhưng hàng phải được đóng gói đúng cách. Mình học được rằng cần chọn đúng công cụ (thread hay process) tùy tình huống.
