# 程序流程控制

- 顺序结构：自上而下顺序执行
- 分支结构：根据条件，选择对应的代码执行**(if、switch)**
- 循环结构：控制某段代码重复执行**(for、while、do-while)**

## 一、分支结构

### 1、if 分支结构

- 根据条件的真或假，来决定执行某段代码

```java
// 如果if控制的代码只有一行，大括号可省略
// 单条件（分支）
if (条件表达式){
	代码;
}

// 非此即彼
if (条件表达式){
	代码1;
} else{
	代码2;
}

// 多条件（分支）
if (条件表达式){
	代码1;
} else if (条件表达式2){
	代码2;
} else if (条件表达式3){
	代码3;
}
...
else{
	代码n;
}
```

- 需求：自动驾驶，如何让汽车安全通过路口，红黄绿三个灯，每个灯有亮和不亮两种状态

```java
// 假设有红黄绿三个灯，每个灯有亮和不亮两种状态
public static void test(){
	boolean red = true
	boolean yellow = false
	boolean green = false
	if (red) {
		System.out.println("红灯亮，停止！")
	} else if (yellow) {
		System.out.println("黄灯亮，准备！")
	} else if (green) {
		System.out.println("绿灯亮，通过！")
	} else {
		System.out.println("灯光故障，停止！")
	}
}
```

### 2、switch分支结构

- 通过比较值是否相等，来决定执行哪条分支
  - 先执行表达式的值，再拿着这个值与case之后的值进行匹配
  - 与哪个case值匹配为true就执行哪个case代码块，遇到break就跳出switch分支
  - 如果所有的case之后的值都不匹配，则执行default代码块

```java
switch (表达式){
    case 值1:
		执行代码1;
		break;
    case 值2:
		执行代码2;
		break;
	...
	default:
		执行代码n;
}
```

- 需求：根据男女推荐不同的书

```java
public static void test(){
	System.out.println("请输入您的性别：");
	Scanner sc = new Scanner(System.in);
	String sex = sc.next();
	switch (sex){
        case "男":
			System.out.println("《java从入门到精通》");
			break;
        case "女":
			System.out.println("《从你的全世界路过》");
			break;
		default:
			System.out.println("你不是人");
	}
}
```

- if 和 switch的比较：

  - if 在功能上远远强大于switch

  - 当条件是区间的时候，建议使用if

  - 当条件与一个一个值匹配时，建议使用switch（格式好、性能好、代码优雅）

    因为 if 得按照顺序一个一个值去匹配，而switch用**树状结构**，可以直接找到匹配的值

- switch分支的注意事项：
  1. 表达式类型**只能是byte、short、int、char 类型**，JDK5开始支持枚举，==JDK7开始支持String==，**不支持double、float、long 类型**
  
  2. case 给出的值不允许重复，只能是字面量，**不能是变量**
  
  3. 正常使用switch的时候，不要忘记写break，否则会出现**穿透现象**(继续往下执行)
  
     穿透性的作用：相同程序的case块可以**利用穿透性合并一些重复的代码**
  
     ```java
     public static void test(){
     	/*
           周一解决bug
           周二~周四请大牛程序员帮忙
           周五整理代码
           周六、周日打游戏
         */
         Scanner sc = new Scanner(System.in);
         System.out.println("请输入星期：");
         String week = sc.next();
         switch (week) {
         	case "周一":
         		System.out.println("解决bug");
         		break;
         	case "周二":
         	case "周三":
         	case "周四":
         		System.out.println("请大牛程序员帮忙");
         		break;
         	case "周五":
         		System.out.println("整理代码");
         		break;
         	case "周六":
         	case "周日":
         		System.out.println("玩游戏");
         		break;
         	default:
         		System.out.println("输入星期有误");
         }
     }
     ```

## 二、循环结构

- 减少代码的重复编写，灵活控制程序的执行

### 1、for 循环

- 控制一段代码反复执行很多次

```java
for (初始化语句; 循环条件; 迭代语句) {
	循环体语句(重复执行的代码);
}

// 例子：打印三遍HelloWorld
for (int i = 0; i < 3; i++){
	System.out.println("HelloWorld!");
}
```

- 例子：1\~5求和，1\~10奇数求和

```java
public class ForDemo1 {
    public static void main(String[] args) {
        int sum = getSum(5);
        System.out.println(sum);
        getSum2(10);
    }

    // 需求，求1-5之间的数据和，在控制台输出结果
    public static int getSum(int n){
        int sum = 0;
        for (int i = 1; i < n+1; i++){
            sum += i;
        }
        return sum;
    }

    // 需求：求1~10之间的奇数和，在控制台输出求和结果
    public static void getSum2(int n){
        int sum = 0;
        for (int i = 1; i <= n; i+=2) {
            sum += i;
        }
        System.out.println(sum);
    }
}
```

### 3、while 循环

```java
while (循环条件) {
	循环体语句(被重复执行的代码);
    迭代语句;
}

// 例子：打印三遍HelloWorld
int i = 0;
while (i<3){
	System.out.println("HelloWorld!");
	i++;
}
```

- while 循环与 for 循环
  - 功能一样
  - 不知道循环几次用 while，知道循环几次用 for

- **需求：**在银行投资了10万元，银行复利为1.7%，多少年可以实现本金翻倍

  ```java
  package com.itheima.loop;
  
  public class WhileDemo1 {
      public static void main(String[] args) {
          cac(100000);
          cac2(100000);
      }
  
      // 正常思路：循环42次
      public static void cac(int money){
          int n = 0;
          double moneyOut = money;
          while (moneyOut < 2*money){
              moneyOut *= 1+0.017;
              n++;
          }
          System.out.println(n + "年可以实现本金翻倍");
      }
  
      // 二分法解决：循环7次
      public static void cac2(int money){
          int target = money*2;
          double rate = 0.017;
          int low = 1;
          int high =100;
          int mid;
          double moneyOut;
          int n = 0;
          while (low <= high){
              mid = (low+high)/2;
              moneyOut = money*Math.pow(1+rate, mid);
  
              if (moneyOut < target){
                  low = mid+1; // mid年还没超过目标金额，需要更多年数
              }
              else {
                  high = mid-1; // mid年已经超过目标金额，需要更少年数
              }
              n++;
          }
          System.out.println(low + "年可以实现本金翻倍");
          System.out.println("一共要循环" + n + "次");
      }
  }
  ```

- **需求：**珠穆朗玛峰的高度是8848.86米=8848860毫米，假如有一张足够大的纸，厚度为0.1毫米，问：折叠多少次，能折成珠穆朗玛峰的高度

  ```java
  package com.itheima.loop;
  
  public class WhileDemo2 {
      // 需求：珠穆朗玛峰的高度是8848.86米=8848860毫米
      // 假如有一张足够大的纸，厚度为0.1毫米，问：折叠多少次，能折成珠穆朗玛峰的高度
      public static void main(String[] args) {
          double m = 0.1;
          int topHeight = 8848860;
          System.out.println("需要折叠" + cac(m, topHeight) + '次');
      }
  
      public static int cac(double m, int topHeight){
          int n = 0;
          double height = m;
          // double height = 0;
  
          while (height < topHeight){
              n++;
              height *= 2;
              //height = m * Math.pow(2, n);
          }
          return n;
      }
  }
  ```

### 4、do-while循环

- 先执行，后判断，**至少都能执行一次**

```java
初始化语句;
do {
    循环体语句;
    迭代语句;
} while (循环条件);

// 打印三行HelloWorld
int i = 0;
do {
    System.out.println(“Hello World！");
    i++;
} while(i < 3);
```

### 5、for、while、do...while 循环的小结

- for循环 和 while循环（先判断后执行）; do...while （先执行后判断）
- for循环和while循环的执行流程是一模一样的，功能上无区别，for能做的while也能做，反之亦然
- 使用规范：如果**已知循环次数**建议**使用for循环**，如果**不清楚要循环多少次**建议**使用while循环**
- 其他区别：for循环中，控制循环的变量只在循环中使用；while循环中，控制循环的变量在循环后还可以继续使用

### 6、死循环与循环嵌套

- 死循环：可以一直执行下去的一种循环，如果没有干预不会停下来 ==(常用于服务器程序)==

  ```java
  for ( ; ; ) {    
  	System.out.println("Hello World1");
  }
  
  // 经典写法
  while (true) {
  	System.out.println("Hello World2");
  }
  
  do {
  	System.out.println("Hello World3");
  } while (true);
  ```

- 循环嵌套：循环中又包含循环

  特点：外部循环每执行一次，内部循环就会全部执行完一轮

  - **需求：**打印一个n*n的菱形（n为奇数）

  ```java
  package com.itheima.loop;
  
  public class MultiLoop {
      public static void main(String[] args) {
          print(7);
      }
  
      // 打印一个n*n的菱形
      public static void print(int n){
          for (int i = 0; i < n; i++){
              for (int j = 0; j < n; j++){
                  if (i <= n/2){
                      if (j >= n/2 - i && j <= n/2 + i){
                          System.out.print('*'); // 打印不换行
                      }
                      else {
                          System.out.print(' ');
                      }
                      }
                  else {
                      if (j >= n/2 - (n-1-i) && j <= n/2 + (n-1-i)){
                          System.out.print('*');
                      }
                      else {
                          System.out.print(' ');
                      }
                  }
              }
              System.out.println();// 打印换行
          }
      }
  }
  ```

  - **需求：**打印一个乘法口诀表

  ```java
  package com.itheima.loop;
  
  public class MultiLoop2 {
      public static void main(String[] args) {
          print2();
      }
  
      // 打印乘法口诀表
      public static void print2(){
          int n = 9;
          for (int i = 1; i <= 9; i++){
              for (int j = 1; j <= i; j++){
                  System.out.print(j + "*" + i + "=" + i*j + " ");
              }
              System.out.println();
          }
      }
  }
  ```

### 7、break 和 continue

- break：跳出当前所在循环的执行，或者结束所在switch分支的执行（辞职跑路）
- continue：跳出当前循环的当次执行，直接进入循环的下一次执行，只能在循环中使用（只是请假）

## 三、综合案例

### 1、简单计算器

**目标：**

- 设计一个可以执行基本数学运算（加、减、乘、除）的计算器程序

**功能描述：**

- 用户输入两个数字、一个运算符（+、-、*、/）
- 根据所选运算符执行相应的数学运算，显示运算结果

**代码：**

```java
package com.itheima.demo;

import java.util.Scanner;

public class PracticeDemo1 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("请输入数字a：");
        double a = sc.nextDouble();
        System.out.print("请输入数字b：");
        double b = sc.nextDouble();
        System.out.print("请输入运算符：");
        String operator = sc.next();
        System.out.print(a + operator +  b + "的运算结果为：" + calc(a,b,operator));
    }

    // 用户输入两个数字，一个运算符，根据所选运算符执行运算，返回结果
    public static double calc(double a, double b, String operator){
        double result = 0;
        switch (operator){
            case "+":
                result = a + b;
                break;
            case "-":
                result = a - b;
                break;
            case "*":
                result = a * b;
                break;
            case "/":
                if (b==0) {
                    System.out.println("除数不能为零");
                } else {
                    result = a / b;
                }
                break;
            default:
                System.out.println("请输入正确的运算符");
        }
        return result;
    }
}
```

### 2、猜数字小游戏

**目标：**

随机生成一个1-100之间的数据，提示用户猜测，猜大提示过大，猜小提示过小，直到猜中结束游戏

**代码：**

```java
package com.itheima.demo;

import java.util.Scanner;

/**
 * 猜数字游戏的主类
 */
public class PracticeDemo2 {
    /**
     * 程序的入口点
     * @param args 命令行参数
     */
    public static void main(String[] args) {
        // 生成1到100之间的随机数
        int num = (int) (100 * Math.random()) +1;
        // Random random = new Random();
        // int randomNumber = random.nextInt(100) + 1;
        // 欢迎信息
        System.out.println("欢迎来到猜数字游戏，我已经想好一个数字");
        // 调用猜数字的方法
        guess(num);
    }

    /**
     * 猜数字的方法
     * @param num 需要猜的数字
     */
    public static void guess(int num){
        // 创建Scanner对象以读取用户的输入
        Scanner sc = new Scanner(System.in);
        // 初始化猜测次数
        int n = 0;
        // 循环直到用户猜中数字
        while (true) {
            // 增加猜测次数
            n++;
            // 提示用户输入猜测的数字
            System.out.print("请输入您猜的数字：");
            // 读取用户输入的数字
            int numIn = sc.nextInt();
            // 比较用户输入的数字和生成的随机数
            if (numIn < num) {
                // 如果用户输入的数字小于随机数
                System.out.println("过小");
            }
            else if (numIn > num) {
                // 如果用户输入的数字大于随机数
                System.out.println("过大");
            }
            else {
                // 如果用户输入的数字等于随机数
                System.out.println("恭喜您猜中");
                // 输出猜测次数
                System.out.println("您总共猜了" + n + "次");
                // 结束循环
                break;
            }
        }

    }
}
```

- 生成随机数方法

  ```java
  // 方法1
  import java.util.Random
  Random r = new Random();
  int number = r.nextInt(10); //生成[0,10)之间的随机数，即0~9
  
  // 方法2
  int number = (int) Math.random(); // 生成[0,1)之间的随机数
  
  // 例子：生成65-91之间的随机数
  int number2 = 65 + r.nextInt(27)
  ```

### 3、开发验证码

**目标：**

开发一个程序，可以生成指定位数的验证码，每位可以是数字、大小写字母

**代码：**

```java
package com.itheima.demo;

import java.util.Random;
import java.util.Scanner;

/**
 * PracticeDemo3类用于演示生成随机验证码的功能
 */
public class PracticeDemo3 {
    /**
     * 程序的入口点
     * @param args 命令行参数，未使用
     */
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("请输入验证码位数：");
        int num = sc.nextInt();
        System.out.println(getNum(num));
    }

    /**
     * 生成指定长度的随机验证码
     * 验证码由数字、大写字母和小写字母组成
     *
     * @param num 验证码的长度
     * @return 生成的验证码字符串
     */
    public static String getNum(int num){
        Random r = new Random();
        StringBuilder sb = new StringBuilder();
        char single;
        for (int i = 0; i < num; i++){
            int rNum = r.nextInt(3);
            switch (rNum){
                case 0:
                    single = (char) (48 + r.nextInt(10));
                    sb.append(single);
                    break;
                case 1:
                    single = (char) (65 + r.nextInt(26));
                    // 还可以用 (char) ('A' + r.nextInt(26))
                    sb.append(single);
                case 2:
                    single = (char) (97 + r.nextInt(26));
                    // 还可以用 (char) ('a' + r.nextInt(26))
                    sb.append(single);
            }
        }
        return sb.toString();
    }
}
```

### 4、找素数

**目标**：

判断101~200之间有多少个素数，并输出所有素数（除了1和它本身以外，不能被其他正整数整除）

**注意**：

假设一个数 $n$ 不为素数，那么一定存在两个不为1的数(因子) $x, y$，使得该数为这两个因子的乘积 $n=xy$，那么这两个因子就存在两种情况：相等或者不等。

- 若相等，则 $x=y=\sqrt{n}$

- 若不等，则必然有 $x>\sqrt{n}\; and\;y<\sqrt{n}$

  因此，要判断一个数 $n$ 是否是素数，只需要判断它是否能被 $2到\sqrt{n}$之间的数整除即可

**代码：**

```java
package com.itheima.demo;

/**
 * PracticeDemo4 类用于演示和实践循环及方法的使用
 */
public class PracticeDemo4 {
    /**
     * 主程序入口
     * 本方法用于计算给定范围内所有的素数，并输出素数的数量及这些素数
     * @param args 命令行参数，未使用
     */
    public static void main(String[] args) {
        // 定义变量a，作为范围的起始值
        int a = 101;
        // 定义变量b，作为范围的结束值
        int b = 200;
        // 初始化计数器n，用于统计素数数量
        int n = 0;
        // 初始化字符串str，用于存储素数的字符串表示
        String str = "";
        // 遍历从a到b的每个数字，检查是否为素数
        for (int i = a; i <= b; i++){
            // 如果当前数字是素数，则增加计数器，并将其添加到字符串中
            if (isPrime(i)){
                n++;
                str = str + " " + i;
                // System.out.print(i + " ");
            }
        }
        // 输出素数的数量
        System.out.println(n);
        // 输出素数的字符串表示
        System.out.println(str);
    }

    /**
     * 判断一个数是否为素数
     * 本方法通过检查从2到数字的平方根之间的所有数，来判断给定的数是否为素数
     * @param num 待检查的数字
     * @return 如果数字是素数，则返回true；否则返回false
     */
    public static boolean isPrime(int num){
        // 如果数字小于等于1，则不是素数
        if (num<=1){
            return false;
        }
        // 从2到数字的平方根进行遍历，检查是否存在因子
        for (int i = 2; i <= Math.sqrt(num); i++){
            // 如果找到一个因子，则数字不是素数
            if (num % i == 0) {
                return false;
            }
        }
        // 如果没有找到因子，则数字是素数
        return true;
    }
}
```

