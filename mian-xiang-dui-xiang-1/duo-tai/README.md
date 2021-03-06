# 多态

## 概念：

* 对象的多态性
* 左父右子
* 父类引用指向子类对象（一只猫当做动物看待）
* 编译看左，运行看右
  * ```java
    父类名称 对象名 = new 子类名称();
    或者：
    接口名称 对象名 = new 实现类名称();
    =======================================
    public static void main(String[] args) {
        // 使用多态的写法
        // 左侧父类的引用，指向了右侧子类的对象
        Fu obj = new Zi();

        obj.method(); //new的是什么，就调用什么的方法
        obj.methodFu();//父类特有方法
    }
    ```

## 访问成员变量：

1. 直接通过对象名称访问成员变量：看等号左边是谁，优先用谁，没有则向上找。
2. 间接通过成员方法访问成员变量：看该方法属于谁，优先用谁，没有则向上找。 
   * new的是什么，就调用什么的方法

```java
public class Demo01MultiField {
    public static void main(String[] args) {
        // 使用多态的写法，父类引用指向子类对象
        Fu obj = new Zi();
        System.out.println(obj.num); // 父：10

        // 子类没有覆盖重写，就是父：10
        // 子类如果覆盖重写，就是子：20
        obj.showNum();
    }
}
```

## 访问成员方法：

* 看new的是谁，就优先用谁，没有则向上找。
* 口诀：编译看左边，运行看右边。

成员变量：编译看左边，运行还看左边。   
成员方法：编译看左边，运行看右边。

```java
public class Demo02MultiMethod {
    public static void main(String[] args) {
        Fu obj = new Zi(); // 多态

        obj.method(); // 父子都有，优先用子
        obj.methodFu(); // 子类没有，父类有，向上找到父类
    }
}
```

## 作用：

![](../../.gitbook/assets/04-shi-yong-duo-tai-de-hao-chu-.png)

## 向上转型：

* 对象的向上转型，就是多态写法
* 格式：`父类名称 对象名 = new 子类名称();`
* 含义：右侧创建一个子类对象，把它当做父类来看待使用
  * ```java
    Animal animal = new Cat();
    创建一只猫，当做动物看待
    ```
* 注意事项：
  * 向上转型一定是安全的。小范围 ---&gt; 大范围
  * 对象一旦向上转型为父类，那么就**无法调用子类**原本特有的内容。

### 向下转型：

* 对象的向下转型，是一个【还原】动作
* 格式：`子类名称 对象名 = (子类名称) 父类对象;`
* 含义：将父类对象，【还原】成为本来的子类对象。
  * ```java
    Animal animal = new Cat(); //本来是cat，向上转型成为动物
    Cat cat = (Cat) animal; //本来是猫，已经被当做动物了，还原回来成为本来的猫
    ```
* 注意事项：
  * 必须保证对象本来创建的时候就是猫；才能向下转型成为猫
  * 如果创建时就不是猫，强行向下转型；则报错

## instanceof

* 判断父类引用对象，本来是什么子类
* 格式：对象 instanceof 类名称
* 返回boolean值，判断前面【对象】能不能作为后面类型的【实例】

```java
public class Demo02Instanceof {
    public static void main(String[] args) {
        Animal animal = new Dog(); // 本来是一只狗
        animal.eat(); // 狗吃SHIT

        // 如果希望掉用子类特有方法，需要向下转型
        // 判断一下父类引用animal本来是不是Dog
        if (animal instanceof Dog) {
            Dog dog = (Dog) animal;
            dog.watchHouse();
        }
        // 判断一下animal本来是不是Cat
        if (animal instanceof Cat) {
            Cat cat = (Cat) animal;
            cat.catchMouse();
        }

        giveMeAPet(new Dog());
    }

    public static void giveMeAPet(Animal animal) {
        if (animal instanceof Dog) {
            Dog dog = (Dog) animal;
            dog.watchHouse();
        }
        if (animal instanceof Cat) {
            Cat cat = (Cat) animal;
            cat.catchMouse();
        }
    }

}
```



