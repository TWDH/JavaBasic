# 包装类

![](../.gitbook/assets/02-bao-zhuang-lei-de-gai-nian-.bmp)



## 概述

Java提供了两个类型系统，基本类型与引用类型，使用基本类型在于效率，然而很多情况，会创建对象使用，因为对象可以做更多的功能，如果想要我们的基本类型像对象一样操作，就可以使用基本类型对应的包装类，如下：

| 基本类型 | 对应的包装类（位于java.lang包中） |
| :--- | :--- |
| byte | Byte |
| short | Short |
| int | **Integer** |
| long | Long |
| float | Float |
| double | Double |
| char | **Character** |
| boolean | Boolean |

## 装箱与拆箱

基本类型与对应的包装类对象之间，来回转换的过程称为”装箱“与”拆箱“：

* **装箱**：从【基本类型】转换为对应的【包装类】对象。
  * 构造方法：
    * `Integer(int value)` 构造一个新分配的 Integer 对象，它表示指定的 int 值。
    * `Integer(String s)` 构造一个新分配的 Integer 对象，它表示 String 参数所指示的 int 值。 传递的字符串,必须是基本类型的字符串,否则会抛出异常 "100" 正确 "a" 抛异常
  * 静态方法：
    * `static Integer valueOf(int i)` 返回一个表示指定的 int 值的 Integer 实例。
    * `static Integer valueOf(String s)` 返回保存指定的 String 的值的 Integer 对象。 
* **拆箱**：从【包装类】对象转换为对应的【基本类型】。
  * 成员方法：
    * int intValue\(\) 以 int 类型返回该 Integer 的值。

```java
//基本数值---->包装对象
Integer i = new Integer(4);//使用构造函数函数
Integer iii = Integer.valueOf(4);//使用包装类中的valueOf方法
===========================================================
//包装对象---->基本数值
int num = i.intValue();
```

## 自动装箱与自动拆箱

### 1. 自动装箱

* 直接把int类型的整数赋值包装类

```java
public static void main(String[] args) {
/*
    自动装箱:直接把int类型的整数赋值包装类
    Integer in = 1; 就相当于 Integer in = new Integer(1);
 */
    Integer in = 1;
```

### 2.自动拆箱

* in是包装类,无法直接参与运算,可以自动转换为基本数据类型,在进行计算

```java
/*
    自动拆箱:in是包装类,无法直接参与运算,可以自动转换为基本数据类型,在进行计算
    in+2;就相当于 in.intVale() + 2 = 3
    in = in.intVale() + 2 = 3 又是一个自动装箱
 */
    in = in+2;
```

### ArrayList的应用

```java
ArrayList<Integer> list = new ArrayList<>();
/*
ArrayList集合无法直接存储整数,可以存储Integer包装类
*/
list.add(1); //-->自动装箱 list.add(new Integer(1));

int a = list.get(0); //-->自动拆箱  list.get(0).intValue();
```



### 基本类型与字符串之间的转换

**基本类型 ---&gt;String：**

1. 基本类型的值+"" 最简单的方法\(工作中常用\) 
2. 包装类的静态方法`toString(参数)`, 不是Object类的toString\(\) 重载 static String toString\(int i\) 返回一个表示指定整数的 String 对象。 
3. String类的静态方法`valueOf(参数)` static String valueOf\(int i\) 返回 int 参数的字符串表示形式。

**字符串 ---&gt; 基本类型：**

除了Character类之外，其他所有包装类都具有parseXxx静态方法可以将字符串参数转换为对应的基本类型：

* `public static byte parseByte(String s)`：将字符串参数转换为对应的byte基本类型。
* `public static short parseShort(String s)`：将字符串参数转换为对应的short基本类型。
* `public static int parseInt(String s)`：将字符串参数转换为对应的int基本类型。
* `public static long parseLong(String s)`：将字符串参数转换为对应的long基本类型。
* `public static float parseFloat(String s)`：将字符串参数转换为对应的float基本类型。
* `public static double parseDouble(String s)`：将字符串参数转换为对应的double基本类型。
* `public static boolean parseBoolean(String s)`：将字符串参数转换为对应的boolean基本类型。

代码使用（仅以Integer类的静态方法parseXxx为例）如：

```java
public class Demo18WrapperParse {
    public static void main(String[] args) {
        int num = Integer.parseInt("100");
    }
}
```

> 注意:如果字符串参数的内容无法正确转换为对应的基本类型，则会抛出`java.lang.NumberFormatException`异常。

