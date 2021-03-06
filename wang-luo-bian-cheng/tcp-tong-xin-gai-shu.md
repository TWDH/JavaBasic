# TCP通信概述

![](../.gitbook/assets/02tcp-tong-xin-de-gai-shu-.bmp)

## 1.概述

**在Java中，提供了两个类用于实现TCP通信程序：**

1. 客户端：`java.net.Socket` 类表示。创建`Socket`对象，向服务端发出连接请求，服务端响应请求，两者建立连接开始通信。
2. 服务端：`java.net.ServerSocket` 类表示。创建`ServerSocket`对象，相当于开启一个服务，并等待客户端的连接。

### 1.1 构造方法

* `Socket(String host, int port)`  
  * 创建一个流套接字并将其连接到指定主机上的指定端口号。
* 参数: 
  * `String host`:服务器主机的名称/服务器的IP地址 
  * `int port`:服务器的端口号

### 1.2 成员方法

* `OutputStream getOutputStream()` 
  * 返回此套接字的输出流。
* `InputStream getInputStream()` 
  * 返回此套接字的输入流。
* `void close()` 
  * 关闭此套接字。

### 1.4 实现步骤\(TCP client\)

1. 创建一个客户端对象Socket,构造方法绑定服务器的IP地址和端口号
2. 使用Socket对象中的方法getOutputStream\(\)获取网络字节输出流OutputStream对象
3. 使用网络字节输出流OutputStream对象中的方法write,给服务器发送数据
4. 使用Socket对象中的方法getInputStream\(\)获取网络字节输入流InputStream对象
5. 使用网络字节输入流InputStream对象中的方法read,读取服务器回写的数据
6. 释放资源\(Socket\)

### 1.5 注意

1. 客户端和服务器端进行交互,必须使用Socket中提供的网络流,不能使用自己创建的流对象
2. 当我们创建客户端对象Socket的时候,就会去请求服务器和服务器经过3次握手建立连接通路 这时如果服务器没有启动,那么就会抛出异常ConnectException: Connection refused: connect 如果服务器已经启动,那么就可以进行交互了

### 代码实例

**客户端**

```java
实现步骤:
    1.创建一个客户端对象Socket,构造方法绑定服务器的IP地址和端口号
    2.使用Socket对象中的方法getOutputStream()获取网络字节输出流OutputStream对象
    3.使用网络字节输出流OutputStream对象中的方法write,给服务器发送数据
    4.使用Socket对象中的方法getInputStream()获取网络字节输入流InputStream对象
    5.使用网络字节输入流InputStream对象中的方法read,读取服务器回写的数据
    6.释放资源(Socket)
    
public static void main(String[] args) throws IOException {
    //1.创建一个客户端对象Socket,构造方法绑定服务器的IP地址和端口号
    Socket socket = new Socket("127.0.0.1",8888);
    //2.使用Socket对象中的方法getOutputStream()获取网络字节输出流OutputStream对象
    OutputStream os = socket.getOutputStream();
    //3.使用网络字节输出流OutputStream对象中的方法write,给服务器发送数据
    os.write("你好服务器".getBytes());

    //4.使用Socket对象中的方法getInputStream()获取网络字节输入流InputStream对象
    InputStream is = socket.getInputStream();

    //5.使用网络字节输入流InputStream对象中的方法read,读取服务器回写的数据
    byte[] bytes = new byte[1024];
    int len = is.read(bytes);
    System.out.println(new String(bytes,0,len));

    //6.释放资源(Socket)
    socket.close();

}
```

**服务器端**

```java
服务器的实现步骤:
    1.创建服务器ServerSocket对象和系统要指定的端口号
    2.使用ServerSocket对象中的方法accept,获取到请求的客户端对象Socket
    3.使用Socket对象中的方法getInputStream()获取网络字节输入流InputStream对象
    4.使用网络字节输入流InputStream对象中的方法read,读取客户端发送的数据
    5.使用Socket对象中的方法getOutputStream()获取网络字节输出流OutputStream对象
    6.使用网络字节输出流OutputStream对象中的方法write,给客户端回写数据
    7.释放资源(Socket,ServerSocket)
    
public static void main(String[] args) throws IOException {
    //1.创建服务器ServerSocket对象和系统要指定的端口号
    ServerSocket server = new ServerSocket(8888);
    //2.使用ServerSocket对象中的方法accept,获取到请求的客户端对象Socket
    Socket socket = server.accept();
    //3.使用Socket对象中的方法getInputStream()获取网络字节输入流InputStream对象
    InputStream is = socket.getInputStream();
    //4.使用网络字节输入流InputStream对象中的方法read,读取客户端发送的数据
    byte[] bytes = new byte[1024];
    int len = is.read(bytes);
    System.out.println(new String(bytes,0,len));
    //5.使用Socket对象中的方法getOutputStream()获取网络字节输出流OutputStream对象
    OutputStream os = socket.getOutputStream();
    //6.使用网络字节输出流OutputStream对象中的方法write,给客户端回写数据
    os.write("收到谢谢".getBytes());
    //7.释放资源(Socket,ServerSocket)
    socket.close();
    server.close();
}
```

