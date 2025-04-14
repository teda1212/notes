## 一、线程和多线程

单线程

- 线程(Thread)是一个程序内部的一条执行流程
- 程序中如果只有一条执行线程，那这个程序就是单线程程序

多线程

- 多线程是指从软硬件上实现的多条执行流程的技术（多条线程由CPU负责调度执行）
- 消息通信、买票、百度网盘、淘宝、京东系统都离不开多线程技术

## 二、创建线程

### 1、方式一：继承Tread类

**创建方法**

①定义一个子类MyThread继承线程类java.lang.Thread，重写run()方法

②创建MyThread类的对象

③调用线程对象的start()方法启动线程（启动后还是执行run方法的）

**优缺点**

- 优点：编码简单
- 缺点：线程类已经继承Thread，无法继承其他类，不利于功能的扩展

```java
package com.itheima.demo1create;

public class Test {
    // main方法本是很是由一条主线程负责推荐执行的
    public static void main(String[] args) {
        // 目标：认识多线程，掌握创建多线程的方式一：继承Thread类
        // 3、创建一个线程对象
        Thread t1 = new MyThread();
        // 4、调用线程对象的start方法，开启线程，并执行run方法
        t1.start();
        
        // 主线程，与子线程交替执行
        for(int i = 0; i < 5; i++){
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }
    }
}

// 1、定义一个子类继承Thread类，成为一个线程类
class MyThread extends Thread {
    // 2、重写run方法，编写线程执行的代码
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }
    }
}
```

**注意事项：**

1、启动线程必须是调用start方法，不是调用run方法。

- **直接调用run方法会当成普通方法执行，此时相当于还是单线程执行。**

- **只有调用start方法才是启动一个新的线程执行。**

2、**不要**把主线程任务放在启动子线程之前

- **这样主线程一直是先跑完的，相当于是一个单线程的效果了。**

### 2、方式二：实现Runnable接口

**创建方法**

①定义一个线程任务类MyRunnable实现Runnable接口，重写run()方法

②创建MyRunnable任务对象

③把MyRunnable任务对象交给Thread处理。

| Thread类提供的构造器           | 说明                         |
| ------------------------------ | ---------------------------- |
| public Thread(Runnable target) | 封装Runnable对象成为线程对象 |

④调用线程对象的start()方法启动线程

**优缺点**

- 优点：任务类只是**实现接口**，可以**继续继承其他类**、**实现其他接口**，扩展性强。

- 缺点：需要多一个Runnable对象。

```java
package com.itheima.demo2create;

public class Test {
    // main方法本是很是由一条主线程负责推荐执行的
    public static void main(String[] args) {
        // 目标：认识多线程，掌握创建多线程的方式一：继承Thread类
        // 3、创建一个Runnable对象，表示一个线程任务
        Runnable r = new MyThread();
        // 4、把线程任务对象交给线程对象
        Thread t1 = new Thread(r);
        // 5、调用线程对象的start方法，开启线程，并执行run方法
        t1.start();

        // 主线程，与子线程交替执行
        for(int i = 0; i < 5; i++){
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }
    }
}

// 1、定义一个子类实现Runnable接口
class MyThread implements Runnable {
    // 2、重写run方法，编写线程执行的代码
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }
    }
}
```

**匿名内部类写法**

①可以创建Runnable的匿名内部类对象。

②再交给Thread线程对象。

③再调用线程对象的start()启动线程。

```java
package com.itheima.demo2create;

public class Test2 {
    // 四条线程交替执行
    public static void main(String[] args) {
        // 目标：认识多线程，掌握创建多线程的方式一：继承Thread类
        // Thread-0
        // 创建一个匿名内部类对象，表示一个线程任务
        Runnable r = new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 5; i++) {
                    System.out.println(Thread.currentThread().getName() + "-->" + i);
                }
            }
        };
        // 把线程任务对象交给线程对象
        Thread t1 = new Thread(r);
        // 调用线程对象的start方法，开启线程，并执行run方法
        t1.start();
			
        // Thread-1
        //简化版本
        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 5; i++) {
                    System.out.println(Thread.currentThread().getName() + "-->" + i);
                }
            }
        }).start();

        // Thread-2
        // Lambda表达式
        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println(Thread.currentThread().getName() + "-->" + i);
            }
        }).start();

        // main主线程，与子线程交替执行
        for(int i = 0; i < 5; i++){
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }
    }
}
```

### 3、方式三：实现Callable接口

**前两种**创建方法的**共同缺点**

- 假如线程执行完毕后有一些**数据需要返回**，他们重写的**run方法均不能直接返回结果**

**解决方法及优点**

- JDK 5.0提供了Callable接口和FutureTask类来实现

- 这种方式最大的优点：可以返回线程执行完毕后的结果

**创建方法**

① 创建任务对象

- 定义一个类实现Callable接口，重写call方法，封装要做的事情，和要返回的数据。

- 把Callable类型的对象封装成FutureTask（线程任务对象）。

  <img src="./images/day04-多线程/image-20250414212013181.png" alt="image-20250414212013181" style="zoom:50%;" align = "left"/>

② 把线程任务对象交给Thread对象。

③ 调用Thread对象的start方法启动线程。

④ 线程执行完毕后、通过FutureTask对象的的get方法去获取线程任务执行的结果。

**优缺点**

- 优点：线程任务类只是实现接口，可以继续继承类和实现接口，扩展性强；可**以在线程执行完毕后去获取线程执行的结果**。
- 缺点：编码复杂一点。

```java
package com.itheima.demo3create;

import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

public class Test {
    public static void main(String[] args) {
        // 第一条线程
        // 3、创建Callable接口实现类的对象
        Callable<String> c1 = new MyCallable(100);
        // 4、把Callable对象封装成一个真正的线程任务对象，FutureTask对象
        /**
         * 未来任务对象的作用
         * a、本质上是一个Runnable线程任务对象
         * b、可以获取到线程任务的执行结果
         */
        FutureTask<String> f1 = new FutureTask<>(c1);
        // 5、把线程任务对象交给线程对象
        Thread t1 = new Thread(f1);
        // 6、调用线程对象的start方法，开启线程
        t1.start();

        // 第二条线程
        Callable<String> c2 = new MyCallable(50);
        FutureTask<String> f2 = new FutureTask<>(c2);
        Thread t2 = new Thread(f2);
        t2.start();

        // 获取线程执行完毕后的结果
        // 两个try-catch，防止一个线程异常导致后面线程也挂了
        try {
            // 如果第一个线程还没有执行完毕，会让出CPU，等待第一个线程执行完毕后，才会往下执行
            System.out.println(f1.get());
        } catch (Exception e) {
            e.printStackTrace();
        }
        try {
            // 如果第二个线程还没有执行完毕，会让出CPU，等待第二个线程执行完毕后，才会往下执行
            System.out.println(f2.get());
        } catch (Exception e) {
            e.printStackTrace();
        }

    }
}

// 1、定义一个实现类实现Callable接口
class MyCallable implements Callable<String> {
    private int n;
    public MyCallable(int n) {
        this.n = n;
    }
    // 2、重写call方法，定义线程执行体
    @Override
    public String call() throws Exception {
        int sum = 0;
        for (int i = 0; i < this.n; i++) {
            sum += i;
        }
        return Thread.currentThread().getName() + "-->" + sum;
    }
}
```

### 4、方法对比

| 方式             | 优点                                                         | 缺点                                                   |
| ---------------- | ------------------------------------------------------------ | ------------------------------------------------------ |
| 继承Thread类     | 编程比较简单，可以直接使用Thread类中的方法                   | 扩展性较差，不能再继承其他的类，不能返回线程执行的结果 |
| 实现Runnable接口 | 扩展性强，实现该接口的同时还可以继承其他的类。               | 编程相对复杂，不能返回线程执行的结果                   |
| 实现Callable接口 | 扩展性强，实现该接口的同时还可以继承其他的类。可以得到线程执行的结果 | 编程相对复杂                                           |

## 三、线程的常用方法

Thread的常用方法

![image-20250414212538315](./images/day04-多线程/image-20250414212538315.png)

- 查名/改名，拿到当前线程对象

```java
package com.itheima.demo4threadapi;

public class ThreadApiDemo1 {
    public static void main(String[] args) {
        Thread t1 = new MyThread("线程1");
        // t1.setName("线程1");
        t1.start();
        System.out.println(t1.getName());

        Thread t2 = new MyThread();
        t2.setName("线程2");
        t2.start();
        System.out.println(t2.getName());

        // 哪个线程调用这段代码，这个代码就拿到哪个线程
        Thread m = Thread.currentThread(); // 拿到当前线程，主线程
        m.setName("主线程");
        System.out.println(m.getName()); // main

    }
}

// 1、定义一个子类继承Thread类，成为一个线程类
class MyThread extends Thread {
    // 2、重写run方法，编写线程执行的代码
    public MyThread(String name) {
        super(name);
    }
    public MyThread() {
        super();
    }
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }
    }
}
```

- Thread.sleep()

```java
package com.itheima.demo4threadapi;

public class ThreadApiDemo2 {
    public static void main(String[] args) {
        // 目标：搞清楚Thread类的sleep方法的作用(线程休眠)
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + "-->" + i);
            try {
                // 让当前线程进入休眠状态，直到时间到了，才会继续执行
                // 用户没交钱，就加上这段代码，交钱就注释掉
                Thread.sleep(1000);// 1000ms = 1s
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

- thread.join()

```java
package com.itheima.demo4threadapi;

public class ThreadApiDemo3 {
    public static void main(String[] args) {
        // 目标：搞清楚线程的join方法，线程插队：让调用这个方法的线程先执行完毕
        Thread t1 = new MyThread2();
        t1.start();

        for (int i = 0; i < 5; i++){
            System.out.println(Thread.currentThread().getName() + "-->" + i);
            // 主线程输出到1时，让子线程插队等待其执行完毕
            if (i == 1){
                try {
                    t1.join();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

// 1、定义一个子类继承Thread类，成为一个线程类
class MyThread2 extends Thread {
    // 2、重写run方法，编写线程执行的代码
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + "-->" + i);
        }
    }
}
```

