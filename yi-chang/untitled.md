# Exception

## 

![](../.gitbook/assets/01-yi-chang-de-chan-sheng-guo-cheng-jie-xi-.bmp)

## 1.概述

* 异常机制其实是帮助我们**找到**程序中的问题，异常的根类是`java.lang.Throwable`，其下有两个子类：`java.lang.Error`与`java.lang.Exception`，平常所说的异常指`java.lang.Exception`。

## 2. 异常的处理

Java异常处理的五个关键字：**try、catch、finally、throw、throws**

### 2.1     抛出异常throw

在编写程序时，我们必须要考虑程序出现问题的情况。比如，在定义方法时，方法需要接受参数。那么，当调用方法使用接受到的参数时，首先需要先对参数数据进行合法的判断，数据若不合法，就应该告诉调用者，传递合法的数据进来。这时需要使用抛出异常的方式来告诉调用者。

在java中，提供了一个**throw**关键字，它用来抛出一个**指定的异常**对象。

1. throw关键字必须写在方法的内部
2. throw关键字后边new的对象必须是Exception或者Exception的子类对象
3. throw关键字抛出指定的异常对象,我们就必须处理这个异常对象
   1. throw关键字后边创建的是**RuntimeException**或者是 RuntimeException的子类对象,我们可以不处理,默认交给JVM处理\(打印异常对象,中断程序\)
   2. throw关键字后边创建的是**编译异常**\(写代码的时候报错\),我们就必须处理这个异常,要么throws,要么try...catch

**使用格式：**

```text
throw new 异常类名(参数);
```

例如：

```java
throw new NullPointerException("要访问的arr数组不存在");

throw new ArrayIndexOutOfBoundsException("该索引在数组中不存在，已超出范围");
```

学习完抛出异常的格式后，我们通过下面程序演示下throw的使用。

```java
public class ThrowDemo {
    public static void main(String[] args) {
        //创建一个数组 
        int[] arr = {2,4,52,2};
        //根据索引找对应的元素 
        int index = 4;
        int element = getElement(arr, index);

        System.out.println(element);
        System.out.println("over");
    }
    /*
     * 根据 索引找到数组中对应的元素
     */
    public static int getElement(int[] arr,int index){ 
        //判断数组是否为null
        if(arr == null){
            throw new NullPointerException("传递的数组的值是null");
        }
        
        //判断  索引是否越界
        if(index<0 || index>arr.length-1){
             /*
             判断条件如果满足，当执行完throw抛出异常对象后，方法已经无法继续运算。
             这时就会结束当前方法的执行，并将异常告知给调用者。这时就需要通过异常来解决。 
              */
             throw new ArrayIndexOutOfBoundsException("哥们，角标越界了~~~");
        }
        int element = arr[index];
        return element;
    }
}
```



### 2.2 Objects非空判断

还记得我们学习过一个类Objects吗，曾经提到过它由一些静态的实用方法组成，这些方法是null-save（空指针安全的）或null-tolerant（容忍空指针的），那么在它的源码中，对对象为null的值进行了抛出异常操作。

* `public static <T> T requireNonNull(T obj)`:查看指定引用对象不是null。

查看源码发现这里对为null的进行了抛出异常操作：

```java
public static <T> T requireNonNull(T obj) {
    if (obj == null)
          throw new NullPointerException();
    return obj;
}
```



### 2.3  声明异常throws

**声明异常**：将问题标识出来，报告给调用者。如果方法内通过throw抛出了编译时异常，而没有捕获处理，那么必须通过throws进行声明，让调用者去处理。

关键字**throws**运用于方法声明之上,用于表示当前方法不处理异常,而是提醒该方法的调用者来处理异常\(抛出异常\).

**声明异常格式：**

```java
修饰符 返回值类型 方法名(参数列表) throws AAAExcepiton,BBBExcepiton...{
    throw new AAAExcepiton("产生原因");
    throw new BBBExcepiton("产生原因");
    ...
}
```

* 注意事项：
  1. throws关键字必须写在方法声明处
  2. throws关键字后边声明的异常必须是Exception或者是Exception的子类
  3. 方法内部如果抛出了多个异常对象,那么throws后边必须也声明多个异常 如果抛出的多个异常对象有子父类关系,那么直接声明父类异常即可
  4. 调用了一个声明抛出异常的方法,我们就必须的处理声明的异常 要么继续使用throws声明抛出,交给方法的调用者处理,最终交给JVM 要么try...catch自己处理异常

```java
public class Demo05Throws {
    /*
        FileNotFoundException extends IOException extends Excepiton
        如果抛出的多个异常对象有子父类关系,那么直接声明父类异常即可
     */
    //public static void main(String[] args) throws FileNotFoundException,IOException {
    //public static void main(String[] args) throws IOException {
    public static void main(String[] args) throws Exception {
        readFile("c:\\a.tx");

        System.out.println("后续代码");
    }

    /*
        定义一个方法,对传递的文件路径进行合法性判断
        如果路径不是"c:\\a.txt",那么我们就抛出文件找不到异常对象,告知方法的调用者
        注意:
            FileNotFoundException是编译异常,抛出了编译异常,就必须处理这个异常
            可以使用throws继续声明抛出FileNotFoundException这个异常对象,让方法的调用者处理
     */
    public static void readFile(String fileName) throws FileNotFoundException,IOException{
        if(!fileName.equals("c:\\a.txt")){
            throw new FileNotFoundException("传递的文件路径不是c:\\a.txt");
        }

        /*
            如果传递的路径,不是.txt结尾
            那么我们就抛出IO异常对象,告知方法的调用者,文件的后缀名不对

         */
        if(!fileName.endsWith(".txt")){
            throw new IOException("文件的后缀名不对");
        }

        System.out.println("路径没有问题,读取文件");
    }
}
```



### 2.4  捕获异常try…catch

如果异常出现的话,会立刻终止程序,所以我们得处理异常:

1. 该方法不处理,而是声明抛出,由该方法的调用者来处理\(throws\)。
2. 在方法中使用try-catch的语句块来处理异常。

**try-catch**的方式就是捕获异常。

* **捕获异常**：Java中对异常有针对性的语句进行捕获，可以对出现的异常进行指定方式的处理。

捕获异常语法如下：

```java
try{
     编写可能会出现异常的代码
}
catch(异常类型  e){
     处理异常的代码
     //记录日志/打印异常信息/继续抛出异常
}
```

**try：**该代码块中编写可能产生异常的代码。

**catch：**用来进行某种异常的捕获，实现对捕获到的异常进行处理。

> 注意:try和catch都不能单独使用,必须连用。

* 注意事项：
  1. try中可能会抛出多个异常对象,那么就可以使用多个catch来处理这些异常对象
  2. 如果try中产生了异常,那么就会执行catch中的异常处理逻辑,执行完毕catch中的处理逻辑,继续执行try...catch之后的代码 如果try中没有产生异常,那么就不会执行catch中异常的处理逻辑,执行完try中的代码,继续执行try...catch之后的代码

```java
public static void main(String[] args) {
    try{
        //可能产生异常的代码
        readFile("d:\\a.tx");
        System.out.println("资源释放");
    }catch (IOException e){//try中抛出什么异常对象,catch就定义什么异常变量,用来接收这个异常对象
        //异常的处理逻辑,异常异常对象之后,怎么处理异常对象
        System.out.println("catch - 传递的文件后缀不是.txt");

/*
    Throwable类中定义了3个异常处理的方法
     String getMessage() 返回此 throwable 的简短描述。
     String toString() 返回此 throwable 的详细消息字符串。
     void printStackTrace()  JVM打印异常对象,默认此方法,打印的异常信息是最全面的
 */
        //System.out.println(e.getMessage());//文件的后缀名不对
        //System.out.println(e.toString());//重写Object类的toString java.io.IOException: 文件的后缀名不对
        //System.out.println(e);//java.io.IOException: 文件的后缀名不对
-----------------------------------------------------------------------------
/*
    java.io.IOException: 文件的后缀名不对
        at com.itheima.demo02.Exception.Demo01TryCatch.readFile(Demo01TryCatch.java:55)
        at com.itheima.demo02.Exception.Demo01TryCatch.main(Demo01TryCatch.java:27)
 */
        e.printStackTrace(); //最全面
    }
    System.out.println("后续代码");
}
---------------------------------------------------------------------------
/*
   如果传递的路径,不是.txt结尾
   那么我们就抛出IO异常对象,告知方法的调用者,文件的后缀名不对

*/
public static void readFile(String fileName) throws IOException {

    if(!fileName.endsWith(".txt")){
        throw new IOException("文件的后缀名不对");
    }

    System.out.println("路径没有问题,读取文件");
}
```



### 2.5 finally 代码块

**finally**：有一些特定的代码无论异常是否发生，都需要执行。另外，因为异常会引发程序跳转，导致有些语句执行不到。而finally就是解决这个问题的，在finally代码块中存放的代码都是一定会被执行的。

* 注意事项：
  * finally不能单独使用,必须和try一起使用
  * finally一般用于资源释放\(资源回收\),无论程序是否出现异常,最后都要资源释放\(IO\)

finally的语法:  
try...catch....finally:自身需要处理异常,最终还得关闭资源。

> 注意:finally不能单独使用。

```java
public class TryCatchDemo4 {
    public static void main(String[] args) {
        try {
            read("a.txt");
        } catch (FileNotFoundException e) {
            //抓取到的是编译期异常  抛出去的是运行期 
            throw new RuntimeException(e);
        } finally {
            System.out.println("不管程序怎样，这里都将会被执行。");
        }
        System.out.println("over");
    }
    /*
     *
     * 我们 当前的这个方法中 有异常  有编译期异常
     */
    public static void read(String path) throws FileNotFoundException {
        if (!path.equals("a.txt")) {//如果不是 a.txt这个文件 
            // 我假设  如果不是 a.txt 认为 该文件不存在 是一个错误 也就是异常  throw
            throw new FileNotFoundException("文件不存在");
        }
    }
}
```

## 2.6多个异常分别处理

* 一个try多个catch注意事项
  * catch里边定义的异常变量,如果有子父类关系,那么**子类的异常变量必须写在上边**,否则就会报错 ArrayIndexOutOfBoundsException extends IndexOutOfBoundsException 
* 多个异常使用捕获又该如何处理呢？

  1. 多个异常分别处理。
  2. 多个异常一次捕获，多次处理。
  3. 多个异常一次捕获一次处理。

  一般我们是使用一次捕获多次处理方式，格式如下：

  ```java
  try{
       编写可能会出现异常的代码
  }catch(异常类型A  e){  当try中出现A类型异常,就用该catch来捕获.
       处理异常的代码
       //记录日志/打印异常信息/继续抛出异常
  }catch(异常类型B  e){  当try中出现B类型异常,就用该catch来捕获.
       处理异常的代码
       //记录日志/打印异常信息/继续抛出异常
  }
  ```

```java
try {
    int[] arr = {1,2,3};
    //System.out.println(arr[3]);//ArrayIndexOutOfBoundsException: 3
    List<Integer> list = List.of(1, 2, 3);
    System.out.println(list.get(3));//IndexOutOfBoundsException: Index 3 out-of-bounds for length 3
}catch (ArrayIndexOutOfBoundsException e){
    System.out.println(e);
}catch (IndexOutOfBoundsException e){
    System.out.println(e);
}
```

* 注意事项
  * 运行时异常被抛出可以不处理。即不捕获也不声明抛出。
  * 如果finally有return语句,**永远返回finally**中的结果,避免该情况. 
  * 子父类异常
    * 如果父类抛出了多个异常,子类重写父类方法时,抛出和父类相同的异常或者是父类异常的子类或者不抛出异常。
    * 父类方法没有抛出异常，子类重写父类该方法时也不可抛出异常。此时子类产生该异常，只能捕获处理，不能声明抛出
    * ```java
      public class Fu {
          public void show01() throws NullPointerException,ClassCastException{}
          public void show02() throws IndexOutOfBoundsException{}
          public void show03() throws IndexOutOfBoundsException{}
          public void show04() throws Exception {}
      }

      class Zi extends Fu{
          //子类重写父类方法时,抛出和父类相同的异常
          public void show01() throws NullPointerException,ClassCastException{}
          //子类重写父类方法时,抛出父类异常的子类
          public void show02() throws ArrayIndexOutOfBoundsException{}
          //子类重写父类方法时,不抛出异常
          public void show03() {}

          /*
              父类方法没有抛出异常，子类重写父类该方法时也不可抛出异常。

           */
          //public void show04() throws Exception{}

          //此时子类产生该异常，只能捕获处理，不能声明抛出
          public void show04()  {
              try {
                  throw  new Exception("编译期异常");
              } catch (Exception e) {
                  e.printStackTrace();
              }
          }
      }
      ```

