# Static

## static关键字-成员变量

* 使用static关键字的内容不再属于对象自己，而属于类
* 凡是本类的对象，都共享同一份static内容（属性）
* 静态变量
  * 类名称.静态变量
  *  `Student.room = "101教室";`

```java
//教室（room）只赋值一次，两个对象都能用
public class Demo01StaticField {
    public static void main(String[] args) {
        Student two = new Student("黄蓉", 16);
        two.room = "101教室";
        System.out.println("姓名：" + two.getName()
                + "，年龄：" + two.getAge() + "，教室：" + two.room
                + "，学号：" + two.getId());

        Student one = new Student("郭靖", 19);
        System.out.println("姓名：" + one.getName()
                + "，年龄：" + one.getAge() + "，教室：" + one.room
                + "，学号：" + one.getId());
    }
}
================================================================
//Student类
public class Student {
    private int id; // 学号
    private String name; // 姓名
    private int age; // 年龄
    static String room; // 所在教室
    private static int idCounter = 0; // 学号计数器，每当new了一个新对象的时候，计数器++

    public Student() {
        this.id = ++idCounter;
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
        this.id = ++idCounter;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

## static关键字-成员方法

* 静态方法
  * 使用static修饰成员方法。
  * 静态方法不属于对象，属于类
    1. 通过类名称调用:`obj.methodStatic();`
    2. 通过对象名调用:`MyClass.methodStatic();`
  * 对于本来类当中的静态方法，可以省略类名称 
* 注意事项：
  * 一旦使用static修饰成员方法，那么这就成为了静态方法。静态方法不属于对象，而是属于类的。
    * 如果方法【没有】static关键字，那么必须首先创建对象，然后通过对象才能使用它。
    * 如果方法【有了】static关键字，那么不需要创建对象，直接就能通过类名称来使用它。
  * 成员方法 ---&gt; 对象
  * 静态方法 ---&gt; 类

```java
public class MyClass {
    int num; // 成员变量
    static int numStatic; // 静态变量
    // 成员方法
    public void method() {
        System.out.println("这是一个成员方法。");
        // 成员方法可以访问成员变量
        System.out.println(num);
        // 成员方法可以访问静态变量
        System.out.println(numStatic);
    }
    // 静态方法
    public static void methodStatic() {
        System.out.println("这是一个静态方法。");
        // 静态方法可以访问静态变量
        System.out.println(numStatic);
        // 静态不能直接访问非静态【重点】
        //System.out.println(num); // 错误写法！

        // 静态方法中不能使用this关键字。
        //System.out.println(this); // 错误写法！
    }

}
==================================================================
public class Demo02StaticMethod {
    public static void main(String[] args) {
        MyClass obj = new MyClass(); // 首先创建对象
        // 然后才能使用没有static关键字的内容
        obj.method();

        // 对于静态方法来说，可以通过对象名进行调用，也可以直接通过类名称来调用。
        obj.methodStatic(); // 正确，不推荐，这种写法在编译之后也会被javac翻译成为“类名称.静态方法名”
        MyClass.methodStatic(); // 正确，推荐

        // 对于本来当中的静态方法，可以省略类名称
        myMethod();
        Demo02StaticMethod.myMethod(); // 完全等效
    }

    public static void myMethod() {
        System.out.println("自己的方法！");
    }

}
```

## 注意事项：

1. 静态不能直接访问非静态
   * 原因：内存中【先】有静态内容，【后】有非静态内容。“先人不知道后人，后人知道先人”
2. 静态方法中不能使用this
   * 原因：this代表当前对象，通过谁调用的方法，谁就是当前对象

## 静态代码块

* 特点：
  * 第一次用到本类时，静态代码执行唯一一次
  * 静态内容总是优先于非静态内容，所以静态代码快比构造方法先执行 
* 格式：
  * ```java
    public class 类名称 {
        static {
            // 静态代码块的内容
        }
    }
    ============================================================
    public class Person {
        static {
            System.out.println("静态代码块执行！");
        }

        public Person() {
            System.out.println("构造方法执行！");
        }
    }
    ```
* 


