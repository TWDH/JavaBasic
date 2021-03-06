# StringBuilder类

![](../.gitbook/assets/01stringbuilder-de-yuan-li-%20%281%29%20%281%29.bmp)

## 概述

* StringBuilder又称为可变字符序列，它是一个类似于 String 的字符串缓冲区，通过某些方法调用可以改变该序列的长度和内容。

## 构造方法

1. `public StringBuilder()`：构造一个空的StringBuilder容器。 
2. `public StringBuilder(String str)`：构造一个StringBuilder容器，并将字符串添加进去。

```java
public class StringBuilderDemo {
    public static void main(String[] args) {
        StringBuilder sb1 = new StringBuilder();
        System.out.println(sb1); // (空白)
        // 使用带参构造
        StringBuilder sb2 = new StringBuilder("itcast");
        System.out.println(sb2); // itcast
    }
}
```

## 成员方法

### 1. append

* `public StringBuilder append(...)`：添加任意类型数据的字符串形式，并返回当前对象自身。

```java
public class Demo02StringBuilder {
	public static void main(String[] args) {
		//创建对象
		StringBuilder builder = new StringBuilder();
		//public StringBuilder append(任意类型)
		StringBuilder builder2 = builder.append("hello");
		//append方法返回的是this，this = builder(谁调用谁是this)
		System.out.println("builder:"+builder);
		System.out.println("builder2:"+builder2);
		System.out.println(builder == builder2); //true
	  
		// 可以添加 任何类型
		//使用append方法无需接收返回值
		builder.append("hello");
		builder.append("world");
		builder.append(true);
		builder.append(100);
		
		// 在我们开发中，会遇到调用一个方法后，返回一个对象的情况。然后使用返回的对象继续调用方法。
    // 这种时候，我们就可以把代码现在一起，如append方法一样，代码如下
		
		//链式编程
		builder.append("hello").append("world").append(true).append(100);
		System.out.println("builder:"+builder);
	}
}
```

### 2. toString

* `public String toString()`：将当前StringBuilder对象转换为String对象。
  * String ----&gt; StringBuilder
    * 可以使用StringBuilder的构造方法
  * StringBuilder ----&gt; String
    * toString方法

```java
public static void main(String[] args) {
    //String->StringBuilder
    String str = "hello";
    StringBuilder bu = new StringBuilder(str);
    
    //往StringBuilder中添加数据
    bu.append("world");
    System.out.println("bu:"+bu);

    //StringBuilder->String
    String s = bu.toString();
    System.out.println("s:"+s);
}
```



