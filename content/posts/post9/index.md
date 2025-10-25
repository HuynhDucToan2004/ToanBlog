
---
title: "Phân tán đối tượng trong Java bằng RMI: Khi Java “nói chuyện” từ xa"
---

Khi học Java, mình từng nghe về RMI (Remote Method Invocation) và nghĩ nó giống như “phép thuật” cho phép gọi hàm từ xa. Nhưng khi thử, mình nhận ra nó không đơn giản như vậy. Hôm nay, mình sẽ chia sẻ về RMI và cách nó giúp phân tán đối tượng trong Java.

#### RMI là gì?
RMI cho phép một chương trình Java gọi phương thức của đối tượng trên máy khác, như thể đối tượng đó ở ngay trong máy mình. Nó giống như bạn gọi điện nhờ bạn bè chạy một hàm giúp, rồi nhận kết quả.

#### Ví dụ thực tế: Gọi hàm từ xa với RMI
Mình thử viết một chương trình RMI đơn giản:
```java
// Interface
import java.rmi.Remote;
import java.rmi.RemoteException;

public interface Hello extends Remote {
    String sayHello() throws RemoteException;
}

// Server
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;
import java.rmi.server.UnicastRemoteObject;

public class HelloImpl extends UnicastRemoteObject implements Hello {
    public HelloImpl() throws RemoteException {}
    public String sayHello() { return "Hello from server!"; }

    public static void main(String[] args) throws Exception {
        Hello obj = new HelloImpl();
        Registry registry = LocateRegistry.createRegistry(1099);
        registry.rebind("Hello", obj);
        System.out.println("Server ready");
    }
}

// Client
import java.rmi.registry.LocateRegistry;
import java.rmi.registry.Registry;

public class Client {
    public static void main(String[] args) throws Exception {
        Registry registry = LocateRegistry.getRegistry("localhost", 1099);
        Hello stub = (Hello) registry.lookup("Hello");
        String response = stub.sayHello();
        System.out.println("Server: " + response);
    }
```
Mình từng quên chạy RMI registry, dẫn đến lỗi “Connection refused”. Bài học: luôn chạy rmiregistry trước khi khởi động server.
Bài học từ RMI
RMI giống như xây cầu nối giữa các máy tính, nhưng cần cấu hình đúng. Mình học được rằng RMI mạnh mẽ cho ứng dụng phân tán, nhưng đòi hỏi hiểu sâu về Java và mạng. Nếu bạn muốn thử, hãy bắt đầu với một chương trình RMI đơn giản như trên!