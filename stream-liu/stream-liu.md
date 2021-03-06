# Stream流

## 1.概述

* 对**集合**和**数组**
  1. 转换为stream流
  2. stream流中的方法进行**简化**操作

### 练习

```java
1.对list集合中的元素进行过滤,只要以张开头的元素,存储到一个新的集合中
2.对listA集合进行过滤,只要姓名长度为3的人,存储到一个新集合中
3.遍历listB集合

public static void main(String[] args) {
    //创建一个List集合,存储姓名
    List<String> list = new ArrayList<>();
    list.add("张无忌");
    list.add("周芷若");
    list.add("赵敏");
    list.add("张强");
    list.add("张三丰");

    //对list集合中的元素进行过滤,只要以张开头的元素,存储到一个新的集合中
    //对listA集合进行过滤,只要姓名长度为3的人,存储到一个新集合中
    //遍历listB集合
    list.stream()
            .filter(name->name.startsWith("张"))
        .filter(name->name.length()==3)
        .forEach(name-> System.out.println(name));
}
```

## 2.获取流

* 所有`Collection` 集合都可以通过`stream`的默认方法获取流
  * `default Stream stream​()`  
* `stream`接口的静态方法`of`可以获取**数组**对应的流
  * `static  Stream of​(T... values)` 
  * 参数是一个可变参数,那么我们就可以传递一个数组

### 代码实例

```java
public static void main(String[] args) {
    //把集合转换为Stream流
    List<String> list = new ArrayList<>();
    Stream<String> stream1 = list.stream();

    Set<String> set = new HashSet<>();
    Stream<String> stream2 = set.stream();

    Map<String,String> map = new HashMap<>();
    //获取键,存储到一个Set集合中
    Set<String> keySet = map.keySet();
    Stream<String> stream3 = keySet.stream();

    //获取值,存储到一个Collection集合中
    Collection<String> values = map.values();
    Stream<String> stream4 = values.stream();

    //获取键值对(键与值的映射关系 entrySet)
    Set<Map.Entry<String, String>> entries = map.entrySet();
    Stream<Map.Entry<String, String>> stream5 = entries.stream();

    //把数组转换为Stream流
    Stream<Integer> stream6 = Stream.of(1, 2, 3, 4, 5);
    //可变参数可以传递数组
    Integer[] arr = {1,2,3,4,5};
    Stream<Integer> stream7 = Stream.of(arr);
    String[] arr2 = {"a","bb","ccc"};
    Stream<String> stream8 = Stream.of(arr2);
}
```

## 3. 常用方法

### 3.1 forEach\(终结方法\)

* 该方法接收一个Consumer接口函数，会将每一个流元素交给该函数进行处理。
* Consumer接口是一个消费型的函数式接口,可以传递Lambda表达式,消费数据
* `void forEach(Consumer<? super T> action)`;
  * 抽象方法 `accept(T t)` 方法中是**有参数**的，所以**lambda**前面有值

```java
public static void main(String[] args) {
    //获取一个Stream流
    Stream<String> stream = Stream.of("张三", "李四", "王五", "赵六", "田七");
    //1.使用Stream流中的方法forEach对Stream流中的数据进行遍历
    stream.forEach((String name)->{
        System.out.println(name);
    });
    
    //or 2.Lambda
    stream.forEach(name->System.out.println(name));
}
```

### 3.2 filter\(延迟方法\)

* 将一个流转换成另一个子集流
* filter方法的参数Predicate是一个函数式接口,所以可以传递Lambda表达式,对数据进行过滤
* `Stream filter(Predicate<? super T> predicate)`;
* 抽象方法:
  * `boolean test(T t)`; 

```java
public static void main(String[] args) {
    //创建一个Stream流
    Stream<String> stream = Stream.of("张三丰", "张翠山", "赵敏", "周芷若", "张无忌");
    //对Stream流中的元素进行过滤,只要姓张的人
    Stream<String> stream2 = stream.filter((String name)->{return name.startsWith("张");});
    //遍历stream2流
    stream2.forEach(name-> System.out.println(name));

    /*
        Stream流属于管道流,只能被消费(使用)一次
        第一个Stream流调用完毕方法,数据就会流转到下一个Stream上
        而这时第一个Stream流已经使用完毕,就会关闭了
        所以第一个Stream流就不能再调用方法了
        IllegalStateException: stream has already been operated upon or closed
     */
    //遍历stream流
    stream.forEach(name-> System.out.println(name));
}
```

### 3.3 Map\(延迟方法\)

* map:用于类型转换
* 如果需要将流中的元素映射到另一个流中，可以使用map方法.
* `Stream map(Function<? super T, ? extends R> mapper)`;
* 该接口需要一个**Function**函数式接口参数，可以将当前流中的**T类型**数据转换为另一种**R类型**的流。
  * Function中抽象方法:
    * `R apply(T t);`  

```java
public static void main(String[] args) {
    //获取一个String类型的Stream流
    Stream<String> stream = Stream.of("1", "2", "3", "4");
    //使用map方法,把字符串类型的整数,转换(映射)为Integer类型的整数
    Stream<Integer> stream2 = stream.map((String s)->{
        return Integer.parseInt(s);
    });
    //遍历Stream2流
    stream2.forEach(i-> System.out.println(i));
}
```

### 3.3 count\(终结方法\)

* 用于统计Stream流中元素的个数
* `long count()`; 
  * count方法是一个终结方法,返回值是一个long类型的整数，所以**不能**再继续调用Stream流中的其他方法了

```java
public static void main(String[] args) {
    //获取一个Stream流
    ArrayList<Integer> list = new ArrayList<>();
    list.add(1);
    list.add(2);
    list.add(3);
    list.add(4);
    list.add(5);
    list.add(6);
    list.add(7);
    Stream<Integer> stream = list.stream();
    long count = stream.count();
    System.out.println(count);//7
}
```

### 3.4 limit \(延迟方法\)

* 用于截取流中的元素
* limit方法可以对流进行截取，**只取用前n个**
* `Stream limit(long maxSize)`; 
  * 参数是一个**long**型，如果集合当前长度**大于参数**则进行截取；否则不进行操作

```java
public static void main(String[] args) {
    //获取一个Stream流
    String[] arr = {"美羊羊","喜洋洋","懒洋洋","灰太狼","红太狼"};
    Stream<String> stream = Stream.of(arr);
    //使用limit对Stream流中的元素进行截取,只要前3个元素
    Stream<String> stream2 = stream.limit(3);
    //遍历stream2流
    stream2.forEach(name-> System.out.println(name));
}
```

### 3.5 skip\(延迟方法\)

* skip:用于**跳过元素**
* 如果希望跳过前几个元素，可以使用skip方法获取一个截取之后的新流
* `Stream skip(long n)`; 
  * 如果流的当前长度大于n，则跳过前n个；否则将会得到一个长度为0的空流。

```java
public static void main(String[] args) {
    //获取一个Stream流
    String[] arr = {"美羊羊","喜洋洋","懒洋洋","灰太狼","红太狼"};
    Stream<String> stream = Stream.of(arr);
    //使用skip方法跳过前3个元素
    Stream<String> stream2 = stream.skip(3);
    //遍历stream2流
    stream2.forEach(name-> System.out.println(name));
}
```

### 3.6 concat\(延迟方法\)

* concat:用于把流组合到一起
* 如果有两个流，希望合并成为一个流，那么可以使用Stream接口的静态方法concat
* `static  Stream concat(Stream<? extends T> a, Stream<? extends T> b)`  

```java
public static void main(String[] args) {
    //创建一个Stream流
    Stream<String> stream1 = Stream.of("张三丰", "张翠山", "赵敏", "周芷若", "张无忌");
    //获取一个Stream流
    String[] arr = {"美羊羊","喜洋洋","懒洋洋","灰太狼","红太狼"};
    Stream<String> stream2 = Stream.of(arr);
    //把以上两个流组合为一个流
    Stream<String> concat = Stream.concat(stream1, stream2);
    //遍历concat流
    concat.forEach(name-> System.out.println(name));
}
```



