
---
title: "Lập trình multicast: Gửi một, nhận nhiều"
---

Khi làm việc với mạng, mình từng thắc mắc: làm sao gửi dữ liệu đến nhiều người nhận cùng lúc mà không tốn tài nguyên? Đó là lúc mình khám phá multicast. Hôm nay, mình sẽ chia sẻ về lập trình multicast và cách nó giúp tiết kiệm “nhiên liệu” mạng.

#### Multicast là gì?
Multicast giống như phát thanh: một máy gửi dữ liệu, nhiều máy nhận cùng lúc. Nó khác với unicast (gửi cho một người) và broadcast (gửi cho tất cả). Multicast dùng nhóm địa chỉ đặc biệt (như 224.0.0.0 đến 239.255.255.255).

#### Ví dụ thực tế: Multicast với Python
Mình thử viết một chương trình multicast:
```python
# Sender
import socket

group = '224.1.1.1'
port = 12345
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.setsockopt(socket.IP_MULTICAST_TTL, 2)
sock.sendto("Hello multicast!".encode(), (group, port))

# Receiver
import socket
import struct

group = '224.1.1.1'
port = 12345
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
sock.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)
sock.bind(('', port))
mreq = struct.pack('4sl', socket.inet_aton(group), socket.INADDR_ANY)
sock.setsockopt(socket.IPPROTO_IP, socket.IP_ADD_MEMBERSHIP, mreq)

data, addr = sock.recvfrom(1024)
print("Nhận:", data.decode())
```
Mình từng quên cấu hình nhóm multicast, dẫn đến không nhận được dữ liệu. Bài học: luôn kiểm tra cấu hình mạng và tài liệu socket.
Bài học từ multicast
Multicast giống như tổ chức một buổi họp trực tuyến: bạn chỉ cần nói một lần, mọi người đều nghe. Mình học được rằng multicast tiết kiệm băng thông, nhưng cần cấu hình cẩn thận. Hãy thử viết một chương trình multicast để thấy nó thú vị thế nào!