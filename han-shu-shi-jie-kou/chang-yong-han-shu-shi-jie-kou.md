# 常用函数式接口

## 1. Supplier接口

* java.util.function.Supplier
* 接口仅包含一个无参的方法：
  * `T get()`
    * 用来获取一个**泛型参数**指定类型的**对象数据**。
* Supplier接口被称之为生产型接口,
  * 指定接口的泛型是什么类型，那么接口中的get方法就会生产什么类型的数据

```java
public class Demo01Supplier {
    //定义一个方法,方法的参数传递Supplier<T>接口,泛型执行String,get方法就会返回一个String
    public static String getString(Supplier<String> sup){
        return sup.get();
    }

    public static void main(String[] args) {
        //调用getString方法,方法的参数Supplier是一个函数式接口,所以可以传递Lambda表达式
        String s = getString(()->{
            //生产一个字符串,并返回
            return "胡歌";
        });
        System.out.println(s);

        //优化Lambda表达式
        String s2 = getString(()->"胡歌");
        System.out.println(s2);
    }
}
```

## 2.Consumer接口

* 它不是生产一个数据，而是消费一个数据，其数据类型由泛型决定。
* 抽象方法`void accept(T t)` 
  * 意为消费一个指定泛型的数据。
* Consumer接口是一个消费型接口,泛型执行什么类型,就可以使用**accept**方法消费什么类型的数据 至于具体怎么消费\(使用\),需要自定义\(输出,计算....\)

```java
public class Demo01Consumer {
    /*
        定义一个方法
        方法的参数传递一个字符串的姓名
        方法的参数传递Consumer接口,泛型使用String
        可以使用Consumer接口消费字符串的姓名
     */
    public static void method(String name, Consumer<String> con){
        con.accept(name);
    }

    public static void main(String[] args) {
        //调用method方法,传递字符串姓名,方法的另一个参数是Consumer接口,是一个函数式接口,所以可以传递Lambda表达式
        method("赵丽颖",(String name)->{
            //对传递的字符串进行消费
            //消费方式:直接输出字符串
            //System.out.println(name);

            //消费方式:把字符串进行反转输出
            String reName = new StringBuffer(name).reverse().toString();
            System.out.println(reName);
        });
    }
}
```

### 2.1 默认方法 andThen

* 作用:
  * 需要两个Consumer接口,可以把两个Consumer接口组合到一起,在对数据进行消费

```java
例如:
 Consumer<String> con1
 Consumer<String> con2
 String s = "hello";
 con1.accept(s);
 con2.accept(s);
 连接两个Consumer接口  再进行消费
 con1.andThen(con2).accept(s); 谁写前边谁先消费
 
 public class Demo02AndThen {
    //定义一个方法,方法的参数传递一个字符串和两个Consumer接口,Consumer接口的泛型使用字符串
    public static void method(String s, Consumer<String> con1 ,Consumer<String> con2){
        //con1.accept(s);
        //con2.accept(s);
        //使用andThen方法,把两个Consumer接口连接到一起,在消费数据
        con1.andThen(con2).accept(s);//con1连接con2,先执行con1消费数据,在执行con2消费数据
    }

    public static void main(String[] args) {
        //调用method方法,传递一个字符串,两个Lambda表达式
        method("Hello",
                (t)->{
                    //消费方式:把字符串转换为大写输出
                    System.out.println(t.toUpperCase());
                },
                (t)->{
                    //消费方式:把字符串转换为小写输出
                    System.out.println(t.toLowerCase());
                });
    }
}
```

### 练习

```java
/*
    练习:
        字符串数组当中存有多条信息，请按照格式“姓名：XX。性别：XX。”的格式将信息打印出来。
        要求将打印姓名的动作作为第一个Consumer接口的Lambda实例，
        将打印性别的动作作为第二个Consumer接口的Lambda实例，
        将两个Consumer接口按照顺序“拼接”到一起。
 */
public class Demo03Test {
    //定义一个方法,参数传递String类型的数组和两个Consumer接口,泛型使用String
    public static void printInfo(String[] arr, Consumer<String> con1,Consumer<String> con2){
        //遍历字符串数组
        for (String message : arr) {
            //使用andThen方法连接两个Consumer接口,消费字符串
            con1.andThen(con2).accept(message);
        }
    }

    public static void main(String[] args) {
        //定义一个字符串类型的数组
        String[] arr = { "迪丽热巴,女", "古力娜扎,女", "马尔扎哈,男" };

        //调用printInfo方法,传递一个字符串数组,和两个Lambda表达式
        printInfo(arr,(message)->{
            //消费方式:对message进行切割,获取姓名,按照指定的格式输出
            String name = message.split(",")[0];
            System.out.print("姓名: "+name);
        },(message)->{
            //消费方式:对message进行切割,获取年龄,按照指定的格式输出
            String age = message.split(",")[1];
            System.out.println("。年龄: "+age+"。");
        });
    }
}
```

## 3.Predicate接口

* 作用:
  * 对某种数据类型的数据进行判断,结果返回一个boolean值
* 抽象方法：
  * `boolean test(T t)`:用来对指定数据类型数据进行**判断**的方法

```java
/*
    定义一个方法
    参数传递一个String类型的字符串
    传递一个Predicate接口,泛型使用String
    使用Predicate中的方法test对字符串进行判断,并把判断的结果返回
 */
public static boolean checkString(String s, Predicate<String> pre){
    return  pre.test(s);
}

public static void main(String[] args) {
    //定义一个字符串
    String s = "abcdef";

    //1.调用checkString方法对字符串进行校验,参数传递字符串和Lambda表达式
    boolean b = checkString(s,(String str)->{
        //对参数传递的字符串进行判断,判断字符串的长度是否大于5,并把判断的结果返回
        return str.length()>5;
    });

    //or 2.优化Lambda表达式
    boolean b = checkString(s,str->str.length()>5);
    System.out.println(b);
}
```

### 3.1 predicate - and / or / negate

```java
逻辑表达式:可以连接多个判断的条件
&&:与运算符,有false则false
||:或运算符,有true则true
!:非(取反)运算符,非真则假,非假则真

需求:判断一个字符串,有两个判断的条件
    1.判断字符串的长度是否大于5
    2.判断字符串中是否包含a
两个条件必须同时满足,我们就可以使用&&运算符连接两个条件

Predicate接口中有一个方法and,表示并且关系,也可以用于连接两个判断条件
default Predicate<T> and(Predicate<? super T> other) {
    Objects.requireNonNull(other);
    return (t) -> this.test(t) && other.test(t);
}
方法内部的两个判断条件,也是使用&&运算符连接起来的
--------------------------------------------------------------------
/*
    定义一个方法,方法的参数,传递一个字符串
    传递两个Predicate接口
        一个用于判断字符串的长度是否大于5
        一个用于判断字符串中是否包含a
        两个条件必须同时满足
 */
public static boolean checkString(String s, Predicate<String> pre1,Predicate<String> pre2){
    //return pre1.test(s) && pre2.test(s);
    return pre1.and(pre2).test(s);//等价于return pre1.test(s) && pre2.test(s);
    return pre1.or(pre2).test(s);//等价于return pre1.test(s) || pre2.test(s);
    return pre.negate().test(s);//等效于return !pre.test(s);
}

public static void main(String[] args) {
    //定义一个字符串
    String s = "abcdef";
    //调用checkString方法,参数传递字符串和两个Lambda表达式
    boolean b = checkString(s,(String str)->{
        //判断字符串的长度是否大于5
        return str.length()>5;
    },(String str)->{
        //判断字符串中是否包含a
        return str.contains("a");
    });
    System.out.println(b);
}
```

## 4. Function接口

* 用来根据一个类型的数据得到另一个类型的数据， 前者称为前置条件，后者称为后置条件。
* **抽象方法为：**
  * `R apply(T t)`，
    * 根据类型**T**的**参数**获取类型**R**的**结果**。
  * 使用的场景例如：将String类型转换为Integer类型。

```java
/*
    定义一个方法
    方法的参数传递一个字符串类型的整数
    方法的参数传递一个Function接口,泛型使用<String,Integer>
    使用Function接口中的方法apply,把字符串类型的整数,转换为Integer类型的整数
 */
public static void change(String s, Function<String,Integer> fun){
    //Integer in = fun.apply(s);
    int in = fun.apply(s);//自动拆箱 Integer->int
    System.out.println(in);
}

public static void main(String[] args) {
    //定义一个字符串类型的整数
    String s = "1234";
    //调用change方法,传递字符串类型的整数,和Lambda表达式
    change(s,(String str)->{
        //把字符串类型的整数,转换为Integer类型的整数返回
        return Integer.parseInt(str);
    });
    //优化Lambda
    change(s,str->Integer.parseInt(str));
}
```

### 4.1 默认方法：andThen

需求+分析

```java
Function接口中的默认方法andThen:用来进行组合操作

需求:
    把String类型的"123",转换为Inteter类型,把转换后的结果加10
    把增加之后的Integer类型的数据,转换为String类型

分析:
    转换了两次
    第一次是把String类型转换为了Integer类型
        所以我们可以使用Function<String,Integer> fun1
            Integer i = fun1.apply("123")+10;
    第二次是把Integer类型转换为String类型
        所以我们可以使用Function<Integer,String> fun2
            String s = fun2.apply(i);
    我们可以使用andThen方法,把两次转换组合在一起使用
        String s = fun1.andThen(fun2).apply("123");
        fun1先调用apply方法,把字符串转换为Integer
        fun2再调用apply方法,把Integer转换为字符串
```

```java
/*
    定义一个方法
    参数串一个字符串类型的整数
    参数再传递两个Function接口
        一个泛型使用Function<String,Integer>
        一个泛型使用Function<Integer,String>
 */
public static void change(String s, Function<String,Integer> fun1,Function<Integer,String> fun2){
    String ss = fun1.andThen(fun2).apply(s);
    System.out.println(ss);
}

public static void main(String[] args) {
    //定义一个字符串类型的整数
    String s = "123";
    //调用change方法,传递字符串和两个Lambda表达式
    change(s,(String str)->{
        //把字符串转换为整数+10
        return Integer.parseInt(str)+10;
    },(Integer i)->{
        //把整数转换为字符串
        return i+"";
    });

    //优化Lambda表达式
    change(s,str->Integer.parseInt(str)+10,i->i+"");
}
```

