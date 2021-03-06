# 转换流

## 1.字符编码和字符集

### 1.1 字符编码

* 按照某种规则，将字符存储到计算机中，称为**编码** 。
* 将存储在计算机中的二进制数按照 某种规则解析显示出来，称为**解码** 。
* 字符编码`Character Encoding` : 就是一套自然语言的字符与二进制数之间的对应规则。

### 1.2 字符集

* 字符集 `Charset` ：也叫编码表。是一个系统支持的所有字符的集合，包括各国家文字、标点符号、图形符 号、数字等。

![](../.gitbook/assets/image%20%282%29.png)

* ASCII字符集 ：美国
* ISO-8859-1字符集：欧洲
* GBxxx字符集：中国
* Unicode字符集 ：万国
  * UTF-8编码
    * 128个US-ASCII字符，只需一个字节编码。
    * 拉丁文等字符，需要二个字节编码。
    * 大部分常用字（含中文），使用三个字节编码。
    * 其他极少使用的Unicode辅助字符，使用四字节编码。



![&#x8F6C;&#x6362;&#x6D41;&#x539F;&#x7406;](../.gitbook/assets/02-zhuan-huan-liu-de-yuan-li-.bmp)

## 2.写转换流 OutputStreamWriter

* java.io.OutputStreamWriter extends Writer
* **字符流** ---&gt; **字节流**：
  * 可使用指定的 charset 将要写入流中的字符编码成字节。\(编码:把能看懂的变成看不懂\)

### 2.1 共性成员方法

* `void write(int c)` 写入单个字符。
* `void write(char[] cbuf)`写入字符数组。
* `abstract  void write(char[] cbuf, int off, int len)`写入字符数组的某一部分,off数组的开始索引,len写的字符个数。
* `void write(String str)`写入字符串。
* `void write(String str, int off, int len)` 写入字符串的某一部分,off字符串的开始索引,len写的字符个数。
* `void flush()`刷新该流的缓冲。
* `void close()` 关闭此流，但要先刷新它。

### 2.2 构造方法

* `OutputStreamWriter(OutputStream out)` 
  * 创建使用默认字符编码的 OutputStreamWriter。
* `OutputStreamWriter(OutputStream out, String charsetName)` 
  * 创建使用指定字符集的 OutputStreamWriter。
* 参数:
  * `OutputStream out`:
    * 字节输出流,可以用来写转换之后的字节到文件中
  * `String charsetName`:
    * 指定的编码表名称,不区分大小写,可以是utf-8/UTF-8,gbk/GBK,...不指定默认使用UTF-8

### 2.3 使用步骤

1. 创建OutputStreamWriter对象,构造方法中传递字节输出流和指定的编码表名称
2. 使用OutputStreamWriter对象中的方法write,把字符转换为字节存储缓冲区中\(编码\)
3. 使用OutputStreamWriter对象中的方法flush,把内存缓冲区中的字节刷新到文件中\(使用字节流写字节的过程\)
4. 释放资源

### 代码实例

```java
public static void main(String[] args) throws IOException {
    //write_utf_8();
    write_gbk();
}

/*
   使用转换流OutputStreamWriter写GBK格式的文件
*/
private static void write_gbk() throws IOException {
    //1.创建OutputStreamWriter对象,构造方法中传递字节输出流和指定的编码表名称
    OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("10_IO\\gbk.txt"),"GBK");
    //2.使用OutputStreamWriter对象中的方法write,把字符转换为字节存储缓冲区中(编码)
    osw.write("你好");
    //3.使用OutputStreamWriter对象中的方法flush,把内存缓冲区中的字节刷新到文件中(使用字节流写字节的过程)
    osw.flush();
    //4.释放资源
    osw.close();
}

/*
    使用转换流OutputStreamWriter写UTF-8格式的文件
 */
private static void write_utf_8() throws IOException {
    //1.创建OutputStreamWriter对象,构造方法中传递字节输出流和指定的编码表名称
    //OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("10_IO\\utf_8.txt"),"utf-8");
    OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("10_IO\\utf_8.txt"));//不指定默认使用UTF-8
    //2.使用OutputStreamWriter对象中的方法write,把字符转换为字节存储缓冲区中(编码)
    osw.write("你好");
    //3.使用OutputStreamWriter对象中的方法flush,把内存缓冲区中的字节刷新到文件中(使用字节流写字节的过程)
    osw.flush();
    //4.释放资源
    osw.close();
}
```

## 3. 读转换流 - InputStreamReader

* java.io.InputStreamReader extends Reader
* **字节流** ---&gt; **字符流**：
  * 它使用指定的 charset 读取字节并将其解码为字符。\(解码:把看不懂的变成能看懂的\)

### 3.1 共性成员方法

* `int read()` 读取单个字符并返回。
* `int read(char[] cbuf)`一次读取多个字符,将字符读入数组。
* `void close()` 关闭该流并释放与之关联的所有资源。

### 3.2 构造方法

* `InputStreamReader(InputStream in)` 创建一个使用默认字符集的 InputStreamReader。
* `InputStreamReader(InputStream in, String charsetName)` 创建使用指定字符集的 InputStreamReader。
* 参数:
  * `InputStream in:`
    * 字节输入流,用来读取文件中保存的字节
  * `String charsetName:`
    * 指定的编码表名称,不区分大小写,可以是utf-8/UTF-8,gbk/GBK,...不指定默认使用UTF-8

### 3.3 使用步骤

1. 创建InputStreamReader对象,构造方法中传递字节输入流和指定的编码表名称
2. 使用InputStreamReader对象中的方法read读取文件
3. 释放资源

### 3.4 注意事项

* 构造方法中指定的编码表名称要和文件的编码相同,否则会发生乱码

### 代码实例

```java
public static void main(String[] args) throws IOException {
    //read_utf_8();
    read_gbk();
}

/*
    使用InputStreamReader读取GBK格式的文件
 */
private static void read_gbk() throws IOException {
    //1.创建InputStreamReader对象,构造方法中传递字节输入流和指定的编码表名称
    //InputStreamReader isr = new InputStreamReader(new FileInputStream("10_IO\\gbk.txt"),"UTF-8");//???
    InputStreamReader isr = new InputStreamReader(new FileInputStream("10_IO\\gbk.txt"),"GBK");//你好

    //2.使用InputStreamReader对象中的方法read读取文件
    int len = 0;
    while((len = isr.read())!=-1){
        System.out.println((char)len);
    }
    //3.释放资源
    isr.close();
}

/*
    使用InputStreamReader读取UTF-8格式的文件
 */
private static void read_utf_8() throws IOException {
    //1.创建InputStreamReader对象,构造方法中传递字节输入流和指定的编码表名称
    //InputStreamReader isr = new InputStreamReader(new FileInputStream("10_IO\\utf_8.txt"),"UTF-8");
    InputStreamReader isr = new InputStreamReader(new FileInputStream("10_IO\\utf_8.txt"));//不指定默认使用UTF-8
    //2.使用InputStreamReader对象中的方法read读取文件
    int len = 0;
    while((len = isr.read())!=-1){
        System.out.println((char)len);
    }
    //3.释放资源
    isr.close();
}
```

