# 多线程

## 1.并发与并行

* 并发：两个或多个事件在同一个时间段发生（交替执行）
* 并行：两个或多个时间在同一时刻发生（同时发生）

## 2.线程与进程

* 进程
  * 进程是指一个内存中运行的应用程序，每个进程都有一个独立的内存空间，一个应用程序可以同时运行多个进程；进程也是程序的一次执行过程，是系统运行程序的基本单位；系统运行一个程序即是一个进程从创建、运行到消亡的过程。
* 线程
  * 线程是进程中的一个执行单元，负责当前进程中程序的执行，一个进程中至少有一个线程。一个进程中是可以有多个线程的，这个应用程序也可以称之为多线程程序。

简而言之：一个程序运行后至少有一个进程，一个进程中可以包含多个线程

![](../.gitbook/assets/04-xian-cheng-gai-nian-.bmp)



**线程调度:**

* 分时调度

  所有线程轮流使用 CPU 的使用权，平均分配每个线程占用 CPU 的时间。

* 抢占式调度

  优先让优先级高的线程使用 CPU，如果线程的优先级相同，那么会随机选择一个\(线程随机性\)，Java使用的为抢占式调度。

## 3 创建线程类

### 3.1 实现步骤

1. 定义Thread类的子类，并重写该类的run\(\)方法，该run\(\)方法的方法体就代表了线程需要完成的任务,因此把run\(\)方法称为线程执行体。
2. 创建Thread子类的实例，即创建了线程对象
3. 调用线程对象的start\(\)方法来启动该线程
   * void start\(\) 使该线程开始执行；Java 虚拟机调用该线程的 run 方法。

* 注意事项：
  * 结果是两个线程并发地运行；当前线程（main线程）和另一个线程（创建的新线程,执行其 run 方法）。
  * 多次启动一个线程是非法的。特别是当线程已经结束执行后，不能再重新启动。
  * java程序属于抢占式调度,那个线程的优先级高,那个线程优先执行;同一个优先级,随机选择一个执行

```java
//main
public class Demo01Thread {
    public static void main(String[] args) {
        //3.创建Thread类的子类对象
        MyThread mt = new MyThread();
        //4.调用Thread类中的方法start方法,开启新的线程,执行run方法
        mt.start();

        for (int i = 0; i <20 ; i++) {
            System.out.println("main:"+i);
        }
    }
}
----------------------------------------------------------------
//Thread子类
public class MyThread extends Thread{
    //2.在Thread类的子类中重写Thread类中的run方法,设置线程任务(开启线程要做什么?)
    @Override
    public void run() {
        for (int i = 0; i <20 ; i++) {
            System.out.println("run:"+i);
        }
    }
}

```

## 4.Thread类-创建多线程

### 4.1 构造方法：

* `public Thread()` :分配一个新的线程对象
* `public Thread(String name)` :分配一个指定名字的新的线程对象。 
* `public Thread(Runnable target)` :分配一个带有指定目标新的线程对象。 
* `public Thread(Runnable target,String name)` :分配一个带有指定目标新的线程对象并指定名字。

### 4.2 常用方法：

* `public String getName()` :获取当前线程名称。
* `public void start()` :导致此线程开始执行; Java虚拟机调用此线程的run方法。 
* `public void run()` :此线程要执行的任务在此处定义代码。 
* `public static void sleep(long millis)` :使当前正在执行的线程以指定的毫秒数暂停（暂时停止执行）。 
* `public static Thread currentThread()` :返回对当前正在执行的线程对象的引用。

### 4.3 开启多线程

```java
public class Demo01GetThreadName {
    public static void main(String[] args) {
        //创建Thread类的子类对象
        MyThread mt = new MyThread();
        //调用start方法,开启新线程,执行run方法
        mt.start();

        new MyThread().start();
        new MyThread().start();

        //链式编程
        System.out.println(Thread.currentThread().getName());
    }
}
```

```java
public class MyThread extends Thread{
    //重写Thread类中的run方法,设置线程任务
    @Override
    public void run() {
        //获取线程名称
        String name = getName();
        System.out.println(name);

        Thread t = Thread.currentThread();
        System.out.println(t);//Thread[Thread-0,5,main]
        String name = t.getName();
        System.out.println(name);

        //链式编程
        System.out.println(Thread.currentThread().getName());
    }
}
```

### 4.4 给Thread起名字

```java
//main
public class Demo01SetThreadName {
    public static void main(String[] args) {
        //开启多线程
        MyThread mt = new MyThread();
        mt.setName("小强");
        mt.start();

        //开启多线程
        new MyThread("旺财").start();
    }
}
```

```java
/*
设置线程的名称:(了解)
    1.使用Thread类中的方法setName(名字)
        void setName(String name) 改变线程名称，使之与参数 name 相同。
    2.创建一个带参数的构造方法,参数传递线程的名称;调用父类的带参构造方法,把线程名称传递给父类,让父类(Thread)给子线程起一个名字
        Thread(String name) 分配新的 Thread 对象。
 */
public class MyThread extends Thread{

    public MyThread(){}

    public MyThread(String name){
        super(name);//把线程名称传递给父类,让父类(Thread)给子线程起一个名字
    }

    @Override
    public void run() {
        //获取线程的名称
        System.out.println(Thread.currentThread().getName());
    }
}
```

### 4.5 sleep

```java
/*
    public static void sleep(long millis):使当前正在执行的线程以指定的毫秒数暂停（暂时停止执行）。
    毫秒数结束之后,线程继续执行
 */
public class Demo01Sleep {
    public static void main(String[] args) {
        //模拟秒表
        for (int i = 1; i <=60 ; i++) {
            System.out.println(i);

            //使用Thread类的sleep方法让程序睡眠1秒钟
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## 5.Runnable-创建多线程

### 使用步骤

1. 创建一个Runnable接口的实现类
2. 在实现类中重写Runnable接口的run方法,设置线程任务
3. 创建一个Runnable接口的实现类对象
4. 创建Thread类对象,构造方法中传递Runnable接口的实现类对象
5. 调用Thread类中的start方法,开启新的线程执行run方法

```java
//RunnableImpl 
public class RunnableImpl implements Runnable{
    //2.在实现类中重写Runnable接口的run方法,设置线程任务
    @Override
    public void run() {
        for (int i = 0; i <20 ; i++) {
            System.out.println(Thread.currentThread().getName()+"-->"+i);
        }
    }
}
```

```java
//RunnableImpl2 
public class RunnableImpl2 implements Runnable{
    //2.在实现类中重写Runnable接口的run方法,设置线程任务
    @Override
    public void run() {
        for (int i = 0; i <20 ; i++) {
            System.out.println("HelloWorld"+i);
        }
    }
}

```

```java
//main
public class Demo01Runnable {
    public static void main(String[] args) {
        //3.创建一个Runnable接口的实现类对象
        RunnableImpl run = new RunnableImpl();
        //4.创建Thread类对象,构造方法中传递Runnable接口的实现类对象
        Thread t = new Thread(run);//打印线程名称
        
        //RunnableImpl2
        Thread t = new Thread(new RunnableImpl2());//打印HelloWorld
        //5.调用Thread类中的start方法,开启新的线程执行run方法
        t.start();

        for (int i = 0; i <20 ; i++) {
            System.out.println(Thread.currentThread().getName()+"-->"+i);
        }
    }
}
```

## 6.匿名内部类

* Thread

```java
//Thread
public static void main(String[] args) {
    //线程的父类是Thread
    // new MyThread().start();
    new Thread(){
    //重写run方法,设置线程任务
        @Override
        public void run() {
            for (int i = 0; i <20 ; i++) {
                System.out.println(Thread.currentThread().getName()+"-->"+"黑马");
            }
        }
    }.start();
}
```

* Runnable

```java
//Runable
public class Demo01InnerClassThread {
    public static void main(String[] args) {
        //线程的接口Runnable
        //Runnable r = new RunnableImpl();//多态
        Runnable r = new Runnable(){
            //重写run方法,设置线程任务
            @Override
            public void run() {
                for (int i = 0; i <20 ; i++) {
                    System.out.println(Thread.currentThread().getName()+"-->"+"程序员");
                }
            }
        };
        new Thread(r).start();

        //简化接口的方式
        new Thread(new Runnable(){
            //重写run方法,设置线程任务
            @Override
            public void run() {
                for (int i = 0; i <20 ; i++) {
                    System.out.println(Thread.currentThread().getName()+"-->"+"传智播客");
                }
            }
        }).start();
    }
}
new Thread(r).start();
--------------------------------------------------------------
//简化接口的方式
new Thread(new Runnable(){
    //重写run方法,设置线程任务
    @Override
    public void run() {
        for (int i = 0; i <20 ; i++) {
            System.out.println(Thread.currentThread().getName()+"-->"+"传智播客");
        }
    }
}).start();
```

## 7.Thread和Runnable的区别

* Thread
  * 不适合资源共享
* Runable接口
  * 容易实现资源共享

