# 面向对象高级

面向对象的三大特征：封装、**继承、多态**

## 一、继承(Extends)

### 1、认识继承

- **为什么要继承**

  - **提高代码的复用性，减少重复代码的书写**

  - 需求：在员工管理系统中，需要处理讲师(姓名、技能)、咨询师(姓名、解答问题的总人数)的数据

    ![image-20250310111649948](images\day06-面向对象高级\image-20250310111649948.png)

- **什么是继承**

  java中提供了一个关键字extends，可以让一个类与另一个类之间建立起父子关系

  ![image-20250310111924686](images\day06-面向对象高级\image-20250310111924686.png)

- **子类能继承什么**

  子类能继承父类的**非私有**成员（成员变量、成员方法）

- **继承后对象的创建**

  子类的对象是由子类、父类多张设计图共同完成的，子类对象是完整的

### 2、权限修饰符

- 什么是权限修饰符

  用来限制类中的成员 (成员变量、成员方法、构造器) 能够被访问的范围

  ![image-20250310113749168](images\day06-面向对象高级\image-20250310113749168.png)



### 3、继承的特点

- **单继承：**一个类只能继承一个直接父类

  ![image-20250310115845627](images\day06-面向对象高级\image-20250310115845627.png)

- **多层继承：**java**不支持多继承**，但**支持多层继承**

  ![image-20250310115925125](images\day06-面向对象高级\image-20250310115925125.png)

- **祖宗类：**java中所有的类都是Object类的子类

  - java中所有类，要么直接继承了Object，要么默认继承了Object，要么间接继承了Object
  - Object类是所有类的祖宗类

- **就近原则：**优先访问自己类中，自己类中没有的才会访问父类

  1. 在子类方法中访问其他成员（成员变量、成员方法），是依照**就近原则的**

     先子类局部范围找，然后子类成员范围找，然后父类成员范围找，如果**父类范围还没有找到则报错**。

  2. 如果子父类中，出现了**重名的成员**，会**优先使用子类的**

  3. 如果子父类中出现了重名的成员，此时一定要在子类中使用父类的怎么办？

     可以通过super关键字，指定访问父类的成员：

     **super.父类成员变量/父类成员方法**

  ```java
  public class Test {
  	Zi zi = new Zi();
  	zi.show();
  }
  
  class Fu {
  	String name = "fu的name";
  	public void run() {
          System.out.println("fu类的方法");
      }
  }
  
  class Zi extends Fu {
  	String name = "zi的name";
  	public void show() {
  		String name = "show的name";
  		System.out.println(name); // show的name
  		System.out.println(this.name);// zi的name
      	System.out.println(super.name);// fu的name
  		 run();// 子类的
       	super.run();// 父类的
       	go();// 报错
  	}
      public void run() {
          System.out.println("zi类的方法");
      }
  }
  ```

### 4、方法重写(Override)

#### 4.1 如何进行方法重写

- 当子类觉得父类中的某个方法不好用，或者无法满足自己的需求时，子类可以重写一个**方法名称、参数列表一样**的方法，去**覆盖父类的这个方法**，这就是方法重写

- ==重写小技巧：==使用**Override**注解，他可以指定java编译器，检查我们方法重写的格式是否正确，代码可读性也会更好

  ```java
  package com.itheima.extends3override;
  
  public class Test {
      public static void main(String[] args) {
          Cat cat = new Cat();
          cat.cry();
      }
  }
  
  class Cat extends Animal {
      // 方法重写：方法名称、形参列表必须一样
      // 重写的一般规范：声明不变，重新实现
      @Override //方法重写校验注解（标志）：要求方法名称和形参列表必须和被重写方法一直，否则报错！更安全、优雅，可读性好
      public void cry() {
          System.out.println("喵喵喵");
      }
  }
  
  class Animal {
      public void cry(){
          System.out.println("动物会叫");
      }
  }
  ```

#### 4.2 方法重写的注意事项

- 子类重写父类方法时，访问权限必须**大于或者等于**父类该方法的权限**（ public > protected > 缺省 ）**

  ```java
  class Cat extends Animal {
      @Override
      public void cry() {
          System.out.println("喵喵喵");
      }
  }
  
  class Animal {
      void cry(){
          System.out.println("动物会叫");
      }
  }
  ```

- 重写的方法返回值类型，必须与被重写方法的**返回值类型**一样，或者范围更小

  ```java
  class Cat extends Animal {
      @Override 
      public Integer cry() {
          System.out.println("喵喵喵");
          return null;
      }
  }
  
  class Animal {
      public Object cry(){
          System.out.println("动物会叫");
          return null;
      }
  }
  ```

- **私有方法、静态方法不能被重写**，如果重写会报错的

#### 4.3 方法重写的应用场景

- 开发中常见的应用场景：子类重写Object类的**toString()方法**，以便返回对象的内容

  ```java
  package com.itheima.extends3override;
  
  public class Test2 {
      public static void main(String[] args) {
          Student s = new Student("游朝政",'男',25);
          System.out.println(s);//com.itheima.extends3override.Student@b4c966a
          // 直接输出对象，默认会自动调用Object下的toString()方法（可以省略不写toString()），返回地址
          /* 输出对象的地址实际上是没有意义的，开发中更希望输出对象时能看对象的内容信息，
           * 所以子类需要重写Object的toString方法，
           * 以便以后输出对象时默认就近调用子类重写的toString()方法返回对象内容 */
  
      }
  }
  
  class Student {
      private String name;
      private char sex;
      private int age;
  
      public Student() {}
  
      public Student(String name, char sex, int age){
          this.name = name;
          this.sex = sex;
          this.age = age;
      }
  
      @Override
      public String toString() {
          return "Student{" +
                  "name='" + name + '\'' +
                  ", sex=" + sex +
                  ", age=" + age +
                  '}';
      }
      
  	//getter/setter方法
  	// ...
  }
  ```

#### 4.4 子类构造器

- **子类构造器的特点**

  子类的全部构造器，都会**先调用父类的构造器**，**再执行自己**

  ```java
  package com.itheima.extends4constructor;
  
  public class Test {
      public static void main(String[] args) {
          // 目标：认识子类构造器的特点，再看应用场景
          // 父类构造器必须先调用父类的构造器，再执行自己的构造器
          Zi z = new Zi();
      }
  }
  
  class Zi extends Fu {
      public Zi() {
          super(666);// 指定调用父类有参构造器
          System.out.println("子类无参数构造器执行了");
      }
  }
  
  class Fu {
      public Fu() {
          System.out.println("父类无参数构造器执行了");
      }
  
      public Fu(int a) {
          System.out.println("父类有参数构造器执行了");
      }
  }
  ```

- **子类构造器如何实现调用父类构造器**

  - **默认**情况下，**子类全部构造器**的**第一行代码都是 super()** （写不写都有） ，它会调用父类的无参数构造器
  - 如果**父类没有无参数构造器**，则我们**必须**在**子类构造器的第一行手写super(….)**，指定去调用父类的有参数构造器

- **子类构造器调用父类构造器的应用场景**

  - 子类构造器可以通过**调用父类构造器**，把对象中**包含父类这部分的数据先初始化赋值** 

  - 再回来把对象里包含子类这部分的数据也进行初始化赋值

    ![image-20250310154440722](images\day06-面向对象高级\image-20250310154440722.png)

#### 4.5 this(...) 调用兄弟构造器

- 任意类的构造器中，是可以通过this(…) 去调用**该类**的**其他构造器**的

  ![image-20250310155830810](images\day06-面向对象高级\image-20250310155830810.png)

- **this(...)和super(…)使用时的注意事项：**

  **this(…) 、super(…)** 都只能放在构造器的**第一行**，因此，**有了this(…)就不能写super(…)了**，反之亦然

  ```java
  public class Student {        
  	private String schoolName;
  	private String name;
     
      public Student(String name){
            this(name , “黑马程序员”);	
  	}	
       
      public Student(String name , String schoolName ){
            super();
            this.name = name;        
            this.schoolName = schoolName;
      }	
  }
  ```

## 二、多态(Polymorphsm)

### 1、认识多态

- 什么是多态

  多态是继承/实现情况下的一种现象，表现为：**对象多态**、**行为多态**

  ![image-20250310162026922](images\day06-面向对象高级\image-20250310162026922.png)

- 多态的前提

  有**继承/实现**关系；存在父类引用子类对象；**存在方法重写**

- 多态的一个**注意事项**：

  多态是**对象、行为的多态**，Java中的**属性(成员变量)不谈多态**

**代码：**

![image-20250310163825033](images\day06-面向对象高级\image-20250310163825033.png)

**结果：**

![image-20250310164007346](images\day06-面向对象高级\image-20250310164007346.png)

### 2、多态的好处

- 使用多态的好处

  在多态形式下，右边的对象是解耦合的，更便于扩展和维护

  ![image-20250310164223054](images\day06-面向对象高级\image-20250310164223054.png)

- **定义方法**时，使**用父类类型的形参**，可以接收一切子类对象，扩展性更强、更便利

```java
package com.itheima.polymorphsm2;

public class Test {
    public static void main(String[] args) {
        // 1.右边对象是解耦合的，可以替换,例如换成Tortois()
        Animal a1 = new Wolf();
        a1.run();
        
        // 2.go()方法可以接收一切子类对象
        Wolf w = new Wolf();
        go(w);
        
        Tortoise t = new Tortoise();
        go(t);

    }

    /* 2.定义go()方法时，使用父类类型的形参，可以接收一切子类对象，
     * 扩展性更强、更便利 */
    public static void go(Animal a){
        System.out.println("开始");
        a.run();
    }
}
```

- 多态下产生的问题：

  多态下**不能使用子类的独有功能**

  ![image-20250310170248669](images\day06-面向对象高级\image-20250310170248669.png)

### 3、多态下的类型转换

- 自动类型转换：父类 变量名 = new 子类()

  例如：People p = new Teacher();

- 强制类型转换：**子类 变量名 = (子类) 父类变量**

  例如 Teacher t = (Teacher) p;

- 强制类型转换的注意事项：

  - 存在继承/实现关系就可以在编译阶段进行强制类型转换，编译阶段不会报错
  - 运行时，如果发现对象的真实类型与强转后的类型不同，就会报类型转换异常（ClassCastException）的错误出来

- **强转前，Java建议**：

  使用**instanceof关键字**，判断当前对象的真实类型，再进行强转

  例如：p instanceof Student （返回true/false）

- 强转能解决的问题：

  可以把对象转换成其真正的类型，从而解决了多态下不能调用子类独有方法的问题

代码：

```java
package com.itheima.polymorphsm3;

public class Test {
    public static void main(String[] args) {
        Animal a1 = new Wolf();
        a1.run();
        // a1.eatSheep;// 报错：多态下不能调用子类独有功能
        // 解决：强制类型转换
        Wolf w1 = (Wolf) a1;
        w1.eatSheep();

        System.out.println("============");
        // 2.go()方法可以接收一切子类对象
        Wolf w = new Wolf();
        go(w);

        Tortoise t = new Tortoise();
        go(t);

    }
    
    public static void go(Animal a){
        System.out.println("开始");
        a.run();
        if (a instanceof Wolf) {
            Wolf w = (Wolf) a;
            w.eatSheep();
        } else if (a instanceof Tortoise) {
            Tortoise t = (Tortoise) a;
            t.shrinkHead();
        }
    }
}
```

结果：

![image-20250310175842043](images\day06-面向对象高级\image-20250310175842043.png)

## 三、综合小项目——加油站支付小模块

- 某加油站为了吸引更多的车主，推出了如下活动，车主可以办理金卡和银卡。

- 卡片信息包括：车牌号码、车主姓名、电话号码、卡片余额。

- 金卡办理时入存金额必须>=5000元，银卡办理时预存金额必须>=2000元，金卡支付时享受8折优惠，银卡支付时享受9折优惠，金卡消费满200元可以提供打印免费洗车票的服务。

- **需求：请使用面向对象编程，完成该加油站支付机的存款和消费程序**

- 分析：

  ```java
  /**
   * 目标：加油站支付小程序
   * 1、定义卡片类，以便创建金卡和银卡，封装用户数据
   * 2、定义卡片父亲: Card，定义金卡和银卡的共同属性和方法
   * 3、定义金卡类，继承卡片类Card，重写消费方式（8折），封装特有方法打印洗车券
   * 4、定义银卡类，继承卡片类Card，重写消费方式（9折）
   * 5、办一张金卡：创建金卡对象，交给一个独立的业务（支付机器），完成存款和消费
   */
  ```

- 代码：

  - 创建Card父类

    ```java
    package com.itheima.demo;
    
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.NoArgsConstructor;
    
    // lombok可以实现自动为类添加getter/setter方法、构造器、toString方法等
    @Data //@Data注解可以自动生成getter/setter方法
    @NoArgsConstructor //@NoArgsConstructor注解可以自动生成无参构造器
    @AllArgsConstructor //@AllArgsConstructor注解可以自动生成有参构造器
    public class Card {
        private String cardId;// 车牌号
        private String name;// 车主姓名
        private String phoneNumber;// 车主电话
        private double money;// 余额
    
        public void desposit(double money) {
            this.money += money;
        }
    
        public void consume(double money) {
            this.money -= money;
        }
    }
    ```

  - 创建金卡类

    ```java
    package com.itheima.demo;
    
    public class GoldCard extends Card {
        public GoldCard(String cardId, String name, String phoneNumber, double money) {
            super(cardId, name, phoneNumber, money);
        }
        @Override
        public void consume(double money) {
            double payMoney = money * 0.8;
            if (getMoney() < payMoney) {
                System.out.println("对不起，需要消费的金额为：" + payMoney);
                System.out.println("您的余额不足，只有：" + getMoney());
                return;
            }
            System.out.println("消费成功，您的消费金额为：" + payMoney);
            setMoney(getMoney() - payMoney);
            System.out.println("消费后您的余额为：" + getMoney());
            if (payMoney >=200){
                System.out.println("恭喜您，消费满两百，获得一张免费洗车券");
            } else {
                System.out.println("很遗憾，消费不满两百，您不能免费洗车券");
            }
    
        }
    }
    ```

  - 创建银卡类

    ```java
    package com.itheima.demo;
    
    public class SilverCard extends Card {
        public SilverCard(String cardId, String name, String phoneNumber, double money) {
            super(cardId, name, phoneNumber, money);
        }
        @Override
        public void consume(double money) {
            double payMoney = money * 0.9;
            if (getMoney() < payMoney) {
                System.out.println("对不起，需要消费的金额为：" + payMoney);
                System.out.println("您的余额不足，只有：" + getMoney());
                return;
            }
            System.out.println("消费成功，您的消费金额为：" + payMoney);
            setMoney(getMoney() - payMoney);
            System.out.println("消费后您的余额为：" + getMoney());
        }
    }
    ```

  - 运行代码文件

    ```java
    package com.itheima.demo;
    
    import java.util.Scanner;
    
    /**
     * 目标：加油站支付小程序
     * 1、定义卡片类，以便创建金卡和银卡，封装用户数据
     * 2、定义卡片父亲: Card，定义金卡和银卡的共同属性和方法
     * 3、定义金卡类，继承卡片类Card，重写消费方式（8折），封装特有方法打印洗车券
     * 4、定义银卡类，继承卡片类Card，重写消费方式（9折）
     * 5、办一张金卡：创建金卡对象，交给一个独立的业务（支付机器），完成存款和消费
     */
    public class Test {
        public static void main(String[] args) {
            // 创建金卡对象并初始化
            GoldCard gc = new GoldCard("川A2211AG", "张三", "13800000000", 5000);
            // 创建银卡对象并初始化
            SilverCard sc = new SilverCard("川A8848KF", "李四", "13800000000", 2000);
            // 金卡用户进行消费或存款操作
            judge(gc);
            System.out.println("============");
            // 银卡用户进行消费或存款操作
            judge(sc);
        }
    
        /**
         * 根据用户选择进行消费或存款操作
         * @param c 卡片对象，可以是金卡或银卡
         */
        public static void judge(Card c) {
            Scanner sc = new Scanner(System.in);
            System.out.println("消费请按1");
            System.out.println("存款请按2");
            int num = sc.nextInt();
            switch (num) {
                case 1:
                    System.out.println("请输入消费金额：");
                    double money = sc.nextDouble();
                    // 调用卡片的消费方法
                    c.consume(money);
                    break;
                case 2:
                    System.out.println("请输入存款金额：");
                    double money2 = sc.nextDouble();
                    // 调用卡片的存款方法
                    c.desposit(money2);
                    break;
                default:
                    System.out.println("输入错误");
            }
        }
    }
    ```

    
