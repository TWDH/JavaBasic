# 序列化

![](../.gitbook/assets/03-xu-lie-hua-he-fan-xu-lie-hua-de-gai-shu-.bmp)

## 1. 对象序列化流 ObjectOutputStream

* java.io.ObjectOutputStream extends OutputStream
* 对象的序列化流
* 作用:
  * 把对象以流的方式写入到文件中保存

### 1.1 构造方法

* `ObjectOutputStream(OutputStream out)`  
  * 创建写入指定 OutputStream 的 ObjectOutputStream。
* 参数：
  * OutputStream out:字节输出流

### 1.2 特有的成员方法:

* `void writeObject(Object obj)` 
  * 将指定的对象写入 ObjectOutputStream。

### 1.3 使用步骤

1. 创建ObjectOutputStream对象,构造方法中传递字节输出流
2. 使用ObjectOutputStream对象中的方法writeObject,把对象写入到文件中
3. 释放资源

### 1.4 反序列化的前提: 

1. 类必须实现Serializable
2. 必须存在类对应的class文件

### 代码实例

```java
public static void main(String[] args) throws IOException {
    //1.创建ObjectOutputStream对象,构造方法中传递字节输出流
    ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("10_IO\\person.txt"));
    //2.使用ObjectOutputStream对象中的方法writeObject,把对象写入到文件中
    oos.writeObject(new Person("小美女",18));
    //3.释放资源
    oos.close();
}
```

## 2. 对象反序列化流 ObjectInputStream

* java.io.ObjectInputStream extends InputStream
* 对象的反序列化流
* **作用:**
  * 把文件中保存的对象,以流的方式读取出来使用

### 2.1 构造方法

* `ObjectInputStream(InputStream in)`  
  * 创建从指定 InputStream 读取的 ObjectInputStream。
* 参数:
  * InputStream in:字节输入流

### 2.2 特有成员方法

* `Object readObject()`  
  * 从 ObjectInputStream 读取对象。 

### 2.3 使用步骤

1. 创建ObjectInputStream对象,构造方法中传递字节输入流
2. 使用ObjectInputStream对象中的方法readObject读取保存对象的文件
3. 释放资源
4. 使用读取出来的对象\(打印\)

### 2.4 反序列化的前提: 

1. 类必须实现Serializable
2. 必须存在类对应的class文件

### 代码实例

```java
public static void main(String[] args) throws IOException, ClassNotFoundException {
    //1.创建ObjectInputStream对象,构造方法中传递字节输入流
    ObjectInputStream ois = new ObjectInputStream(new FileInputStream("10_IO\\person.txt"));
    //2.使用ObjectInputStream对象中的方法readObject读取保存对象的文件
    Object o = ois.readObject();
    //3.释放资源
    ois.close();
    //4.使用读取出来的对象(打印)
    System.out.println(o);
    Person p = (Person)o;
    System.out.println(p.getName()+p.getAge());
}
```

### transient瞬态关键字

* 被**transient**修饰成员变量,**不能**被序列化
* 被**static**修饰的成员变量**不能**被序列化的,序列化的都是对象
  * 静态优先于非静态加载到内存中\(静态优先于对象进入到内存中\)

### 指定序列号serialVersionUID

`private static final long serialVersionUID = 1L;` 

## 3. 练习 - 序列化集合

```java
/*
练习：序列化集合
    当我们想在文件中保存多个对象的时候
    可以把多个对象存储到一个集合中
    对集合进序列化和反序列化
分析:
    1.定义一个存储Person对象的ArrayList集合
    2.往ArrayList集合中存储Person对象
    3.创建一个序列化流ObjectOutputStream对象
    4.使用ObjectOutputStream对象中的方法writeObject,对集合进行序列化
    5.创建一个反序列化ObjectInputStream对象
    6.使用ObjectInputStream对象中的方法readObject读取文件中保存的集合
    7.把Object类型的集合转换为ArrayList类型
    8.遍历ArrayList集合
    9.释放资源
 */
public static void main(String[] args) throws IOException, ClassNotFoundException {
    //1.定义一个存储Person对象的ArrayList集合
    ArrayList<Person> list = new ArrayList<>();
    //2.往ArrayList集合中存储Person对象
    list.add(new Person("张三",18));
    list.add(new Person("李四",19));
    list.add(new Person("王五",20));
    //3.创建一个序列化流ObjectOutputStream对象
    ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("10_IO\\list.txt"));
    //4.使用ObjectOutputStream对象中的方法writeObject,对集合进行序列化
    oos.writeObject(list);
    
    //5.创建一个反序列化ObjectInputStream对象
    ObjectInputStream ois = new ObjectInputStream(new FileInputStream("10_IO\\list.txt"));
    //6.使用ObjectInputStream对象中的方法readObject读取文件中保存的集合
    Object o = ois.readObject();
    //7.把Object类型的集合转换为ArrayList类型
    ArrayList<Person> list2 = (ArrayList<Person>)o;
    //8.遍历ArrayList集合
    for (Person p : list2) {
        System.out.println(p);
    }
    //9.释放资源
    ois.close();
    oos.close();
}
```

