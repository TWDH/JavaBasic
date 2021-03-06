---
description: Lambda优化
---

# 方法引用

## 1.方法引用符

* 双冒号`::`为引用运算符，而它所在的表达式被称为**方法引用**
* 如果**Lambda**要表达的函数方案已经存在于某个方 法的实现中，那么则可以通过双冒号来引用该方法作为Lambda的**替代者**。
* 方法**必须存在**，才可以使用方法引用

### 1.1 语义分析

* Lambda表达式写法： 
  * `s -> System.out.println(s);`  
* **方法引用**写法： 
  * `System.out::println` 

## 方法引用

```java
// interface
@FunctionalInterface
public interface Printable {
    //定义字符串的抽象方法
    void print(String s);
}
--------------------------------------------------

//定义一个方法,参数传递Printable接口,对字符串进行打印
public static void printString(Printable p) {
    p.print("HelloWorld");
}

public static void main(String[] args) {
    //调用printString方法,方法的参数Printable是一个函数式接口,所以可以传递Lambda
    printString((s) -> {
        System.out.println(s);
    });

    /*
        分析:
            Lambda表达式的目的,打印参数传递的字符串
            把参数s,传递给了System.out对象,调用out对象中的方法println对字符串进行了输出
            注意:
                1.System.out对象是已经存在的
                2.println方法也是已经存在的
            所以我们可以使用方法引用来优化Lambda表达式
            可以使用System.out方法直接引用(调用)println方法
     */
    printString(System.out::println);
}
```

## 2.对象名引用成员方法

* 使用前提是**对象名**是已经**存在**的,**成员方法**也是已经**存在**, 就可以使用对象名来引用成员方法

### 代码实例

* Printable 接口

```java
@FunctionalInterface
public interface Printable {
    //定义字符串的抽象方法
    void print(String s);
}
```

* MethodRefObject.java

```java
public class MethodRerObject {
    //定义一个成员方法,传递字符串,把字符串按照大写输出
    public void printUpperCaseString(String str){
        System.out.println(str.toUpperCase());
    }
}
```

* main.java
  1. 重写`printString()` 中`Printable`的**抽象方法** `print()` 
     * 重写的print\(\)方法中，有参数s。
  2. 在其中创建`MethodRerObject`对象，并调用其方法`printUpperCaseString()` 
  3. **方法引用**替换`printUpperCaseString`的使用

```java
//定义一个方法,方法的参数传递Printable接口
public static void printString(Printable p){
    p.print("Hello");
}

public static void main(String[] args) {
    //调用printString方法,方法的参数Printable是一个函数式接口,所以可以传递Lambda表达式
    printString((s)->{
        //创建MethodRerObject对象
        MethodRerObject obj = new MethodRerObject();
        //调用MethodRerObject对象中的成员方法printUpperCaseString,把字符串按照大写输出
        obj.printUpperCaseString(s);
    });

    /*
        使用方法引用优化Lambda
        对象是已经存在的MethodRerObject
        成员方法也是已经存在的printUpperCaseString
        所以我们可以使用对象名引用成员方法
     */
    //创建MethodRerObject对象
    MethodRerObject obj = new MethodRerObject();
    printString(obj::printUpperCaseString);
}
```

## 3.通过类名引用静态成员方法

* **类**已经存在,**静态成员方法**也已经**存在** 就可以通过类名直接引用静态成员方法

### 代码实例

* Calcable.java

```java
@FunctionalInterface
public interface Calcable {
    //定义一个抽象方法,传递一个整数,对整数进行绝对值计算并返回
    int calsAbs(int number);
}
```

* main.java

```java
//定义一个方法,方法的参数传递要计算绝对值的整数,和函数式接口Calcable
public static int method(int number,Calcable c){
   return c.calsAbs(number);
}

public static void main(String[] args) {
    //调用method方法,传递计算绝对值得整数,和Lambda表达式
    int number = method(-10,(n)->{
        //对参数进行绝对值得计算并返回结果
        return Math.abs(n);
    });
    System.out.println(number);

    /*
        使用方法引用优化Lambda表达式
        Math类是存在的
        abs计算绝对值的静态方法也是已经存在的
        所以我们可以直接通过类名引用静态方法
     */
    int number2 = method(-10,Math::abs);
    System.out.println(number2);
}
```

## 4. 通过super引用父类成员方法

* 父类

```java
public class Human {
    //定义一个sayHello的方法
    public void sayHello(){
        System.out.println("Hello 我是Human!");
    }
}
```

* 子类

```java
public class Man extends Human{
    //子类重写父类sayHello的方法
    @Override
    public void sayHello() {
        System.out.println("Hello 我是Man!");
    }

    //定义一个方法参数传递Greetable接口
    public void method(Greetable g){
        g.greet();
    }

    public void show(){
        //1.调用method方法,方法的参数Greetable是一个函数式接口,所以可以传递Lambda
        method(()->{
            //创建父类Human对象
            Human h = new Human();
            //调用父类的sayHello方法
            h.sayHello();
        });

        //2.因为有子父类关系,所以存在的一个关键字super,代表父类,所以我们可以直接使用super调用父类的成员方法
        method(()->{
            super.sayHello();
        });

      /*
         3.使用super引用类的成员方法
           super是已经存在的
           父类的成员方法sayHello也是已经存在的
           所以我们可以直接使用super引用父类的成员方法
       */
      method(super::sayHello);
    }

    public static void main(String[] args) {
        new Man().show();
    }
}
```

## 5.通过this引用本类成员方法

* Richable.java \(interface\)

```java
@FunctionalInterface
public interface Richable {
    //定义一个想买什么就买什么的方法
    void buy();
}
```

* Husband.java
  * 调用关系：soHappy\(\) ---&gt; marry\(\) ---&gt; buy\(\) ---&gt; buyHouse\(\) 

```java
public class Husband {
    //定义一个买房子的方法
    public void buyHouse(){
        System.out.println("北京二环内买一套四合院!");
    }

    //定义一个结婚的方法,参数传递Richable接口
    public void marry(Richable r){
        r.buy();
    }

    //定义一个非常高兴的方法
    public void soHappy(){
        //调用结婚的方法,方法的参数Richable是一个函数式接口,传递Lambda表达式
        marry(()->{
            //使用this.成员方法,调用本类买房子的方法
            this.buyHouse();
        });

        /*
            使用方法引用优化Lambda表达式
            this是已经存在的
            本类的成员方法buyHouse也是已经存在的
            所以我们可以直接使用this引用本类的成员方法buyHouse
         */
        marry(this::buyHouse);
    }

    public static void main(String[] args) {
        new Husband().soHappy();
    }
}
```

## 6.类的构造器引用

* 由于构造器的名称与类名完全一样，并不固定。所以构造器引用使用`类名称::new` 的格式表示。

### 代码实例

* PersonBuilder.java \(interface\)

```java
@FunctionalInterface
public interface PersonBuilder {
    //定义一个方法,根据传递的姓名,创建Person对象返回
    Person builderPerson(String name);
}
```

* Person.java

```java
public class Person {
    private String name;

    public Person() {
    }

    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

* main.java

```java
//定义一个方法,参数传递姓名和PersonBuilder接口,方法中通过姓名创建Person对象
public static void printName(String name,PersonBuilder pb){
    Person person = pb.builderPerson(name);
    System.out.println(person.getName());
}

public static void main(String[] args) {
    //调用printName方法,方法的参数PersonBuilder接口是一个函数式接口,可以传递Lambda
    printName("迪丽热巴",(String name)->{
        return new Person(name);
    });

    /*
        使用方法引用优化Lambda表达式
        构造方法new Person(String name) 已知
        创建对象已知 new
        就可以使用Person引用new创建对象
     */
    printName("古力娜扎",Person::new);//使用Person类的带参构造方法,通过传递的姓名创建对象
}
```

## 7. 数组的构造器引用

* 数组也是`Object`的子类对象，所以同样具有构造器

### 代码实例

* ArrayBuilder.java \(interface\)

```java
@FunctionalInterface
public interface ArrayBuilder {
    //定义一个创建int类型数组的方法,参数传递数组的长度,返回创建好的int类型数组
    int[] builderArray(int length);
}
```

* main.java

```java
public class Demo {
    /*
        定义一个方法
        方法的参数传递创建数组的长度和ArrayBuilder接口
        方法内部根据传递的长度使用ArrayBuilder中的方法创建数组并返回
     */
    public static int[] createArray(int length, ArrayBuilder ab){
        return  ab.builderArray(length);
    }

    public static void main(String[] args) {
        //调用createArray方法,传递数组的长度和Lambda表达式
        int[] arr1 = createArray(10,(len)->{
            //根据数组的长度,创建数组并返回
            return new int[len];
        });
        System.out.println(arr1.length);//10

        /*
            使用方法引用优化Lambda表达式
            已知创建的就是int[]数组
            数组的长度也是已知的
            就可以使用方法引用
            int[]引用new,根据参数传递的长度来创建数组
         */
        int[] arr2 =createArray(10,int[]::new);//10
        System.out.println(Arrays.toString(arr2));//[0,0,0, ..., 0, 0]
        System.out.println(arr2.length);//10
    }
}

```

