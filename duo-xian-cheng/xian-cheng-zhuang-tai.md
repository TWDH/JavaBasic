# 线程池

![&#x7EBF;&#x7A0B;&#x6C60;](../.gitbook/assets/02-xian-cheng-chi-.bmp)

## 1.概述

* 线程池：容纳多个线程的容器，其中的线程可以反复使用，省去频繁创建线程的操作。

## 2.Executors：

* 线程池工厂类，用来生成线程池

### 2.1 静态方法:

* 方法：
  * 创建一个可重用固定线程数的线程池
    * `static ExecutorService newFixedThreadPool(int nThreads)`
* 参数：
  * `int nThreads`:创建线程池中包含的线程数量
* 返回值：
  * `ExecutorService接口`,返回的是ExecutorService接口的实现类对象,我们可以使用ExecutorService接口接收\(面向接口编程\)

### 2.2 线程池接口：

* 作用：用来从线程池中获取线程,调用start方法,执行线程任务
* 方法：
  * 提交一个 Runnable 任务用于执行
    * `submit(Runnable task)`
  * 关闭/销毁线程池的方法
    * `void shutdown()` 

### 2.3 使用步骤

1. 使用线程池的工厂类Executors里边提供的静态方法newFixedThreadPool生产一个指定线程数量的线程池
2. 创建一个类,实现Runnable接口,重写run方法,设置线程任务
3. 调用ExecutorService中的方法submit,传递线程任务\(实现类\),开启线程,执行run方法
4. 调用ExecutorService中的方法shutdown销毁线程池\(不建议执行\)

* RunnableImpl
  * ```java
    public class RunnableImpl implements Runnable{
        @Override
        public void run() {
            System.out.println(Thread.currentThread().getName()+"创建了一个新的线程执行");
        }
    }
    ```
* ThreadPool
  * ```java
    public class Demo01ThreadPool {
        public static void main(String[] args) {
            //1.使用线程池的工厂类Executors里边提供的静态方法newFixedThreadPool生产一个指定线程数量的线程池
            ExecutorService es = Executors.newFixedThreadPool(2);
            //3.调用ExecutorService中的方法submit,传递线程任务(实现类),开启线程,执行run方法
            es.submit(new RunnableImpl());//pool-1-thread-1创建了一个新的线程执行
            //线程池会一直开启,使用完了线程,会自动把线程归还给线程池,线程可以继续使用
            es.submit(new RunnableImpl());//pool-1-thread-1创建了一个新的线程执行
            es.submit(new RunnableImpl());//pool-1-thread-2创建了一个新的线程执行

            //4.调用ExecutorService中的方法shutdown销毁线程池(不建议执行)
            es.shutdown();

            es.submit(new RunnableImpl());//抛异常,线程池都没有了,就不能获取线程了
        }
    }
    ```

