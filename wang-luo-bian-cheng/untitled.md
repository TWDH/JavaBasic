# 练习 - FileUpload

## TCP client

```java
实现步骤:
    1.创建一个本地字节输入流FileInputStream对象,构造方法中绑定要读取的数据源
    2.创建一个客户端Socket对象,构造方法中绑定服务器的IP地址和端口号
    3.使用Socket中的方法getOutputStream,获取网络字节输出流OutputStream对象
    4.使用本地字节输入流FileInputStream对象中的方法read,读取本地文件
    5.使用网络字节输出流OutputStream对象中的方法write,把读取到的文件上传到服务器
    6.使用Socket中的方法getInputStream,获取网络字节输入流InputStream对象
    7.使用网络字节输入流InputStream对象中的方法read读取服务回写的数据
    8.释放资源(FileInputStream,Socket)
public static void main(String[] args) throws IOException {
    //1.创建一个本地字节输入流FileInputStream对象,构造方法中绑定要读取的数据源
    FileInputStream fis = new FileInputStream("c:\\1.jpg");
    //2.创建一个客户端Socket对象,构造方法中绑定服务器的IP地址和端口号
    Socket socket = new Socket("127.0.0.1",8888);
    //3.使用Socket中的方法getOutputStream,获取网络字节输出流OutputStream对象
    OutputStream os = socket.getOutputStream();
    //4.使用本地字节输入流FileInputStream对象中的方法read,读取本地文件
    int len = 0;
    byte[] bytes = new byte[1024];
    while((len = fis.read(bytes))!=-1){
        //5.使用网络字节输出流OutputStream对象中的方法write,把读取到的文件上传到服务器
        os.write(bytes,0,len);
    }

    /*
        解决:上传完文件,给服务器写一个结束标记
        void shutdownOutput() 禁用此套接字的输出流。
        对于 TCP 套接字，任何以前写入的数据都将被发送，并且后跟 TCP 的正常连接终止序列。
     */
    socket.shutdownOutput();

    //6.使用Socket中的方法getInputStream,获取网络字节输入流InputStream对象
    InputStream is = socket.getInputStream();



    //7.使用网络字节输入流InputStream对象中的方法read读取服务回写的数据
    while((len = is.read(bytes))!=-1){
        System.out.println(new String(bytes,0,len));
    }


    //8.释放资源(FileInputStream,Socket)
    fis.close();
    socket.close();
}
```

## TCP server

```java
实现步骤:
    1.创建一个服务器ServerSocket对象,和系统要指定的端口号
    2.使用ServerSocket对象中的方法accept,获取到请求的客户端Socket对象
    3.使用Socket对象中的方法getInputStream,获取到网络字节输入流InputStream对象
    4.判断d:\\upload文件夹是否存在,不存在则创建
    5.创建一个本地字节输出流FileOutputStream对象,构造方法中绑定要输出的目的地
    6.使用网络字节输入流InputStream对象中的方法read,读取客户端上传的文件
    7.使用本地字节输出流FileOutputStream对象中的方法write,把读取到的文件保存到服务器的硬盘上
    8.使用Socket对象中的方法getOutputStream,获取到网络字节输出流OutputStream对象
    9.使用网络字节输出流OutputStream对象中的方法write,给客户端回写"上传成功"
    10.释放资源(FileOutputStream,Socket,ServerSocket)
public static void main(String[] args) throws IOException {
    //1.创建一个服务器ServerSocket对象,和系统要指定的端口号
    ServerSocket server = new ServerSocket(8888);
    //2.使用ServerSocket对象中的方法accept,获取到请求的客户端Socket对象

    /*
        让服务器一直处于监听状态(死循环accept方法)
        有一个客户端上传文件,就保存一个文件
     */
    while(true){
        Socket socket = server.accept();

        /*
            使用多线程技术,提高程序的效率
            有一个客户端上传文件,就开启一个线程,完成文件的上传
         */
        new Thread(new Runnable() {
            //完成文件的上传
            @Override
            public void run() {
               try {
                   //3.使用Socket对象中的方法getInputStream,获取到网络字节输入流InputStream对象
                   InputStream is = socket.getInputStream();
                   //4.判断d:\\upload文件夹是否存在,不存在则创建
                   File file =  new File("d:\\upload");
                   if(!file.exists()){
                       file.mkdirs();
                   }

                /*
                    自定义一个文件的命名规则:防止同名的文件被覆盖
                    规则:域名+毫秒值+随机数
                 */
                   String fileName = "itcast"+System.currentTimeMillis()+new Random().nextInt(999999)+".jpg";

                   //5.创建一个本地字节输出流FileOutputStream对象,构造方法中绑定要输出的目的地
                   //FileOutputStream fos = new FileOutputStream(file+"\\1.jpg");
                   FileOutputStream fos = new FileOutputStream(file+"\\"+fileName);
                   //6.使用网络字节输入流InputStream对象中的方法read,读取客户端上传的文件


                   int len =0;
                   byte[] bytes = new byte[1024];
                   while((len = is.read(bytes))!=-1){
                       //7.使用本地字节输出流FileOutputStream对象中的方法write,把读取到的文件保存到服务器的硬盘上
                       fos.write(bytes,0,len);
                   }


                   //8.使用Socket对象中的方法getOutputStream,获取到网络字节输出流OutputStream对象
                   //9.使用网络字节输出流OutputStream对象中的方法write,给客户端回写"上传成功"
                   socket.getOutputStream().write("上传成功".getBytes());
                   //10.释放资源(FileOutputStream,Socket,ServerSocket)
                   fos.close();
                   socket.close();
               }catch (IOException e){
                   System.out.println(e);
               }
            }
        }).start();


    }

    //服务器就不用关闭
    //server.close();
}
```

