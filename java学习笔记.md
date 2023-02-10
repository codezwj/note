# java学习笔记

部分基础知识没记

所有与C/C++语法相同的都没记

**注释**

1. 文档注释

   /**  

   ​	@author 指定Java程序的作者

   ​	@version 指定源文件的版本

      */

   ​	文档注释内容可以被JDK提供的工具javadoc解析，生成一套以网页文件形式体现的该程序的说明文档

2. 单行注释和多行注释规则同c，多行注释不可嵌套使用

**hello world**

```java
public class hello{
    pubilc static void main(String[] args){
        System.out.println("hello world");
    }
}
```



1. 一个Java源文件中可以声明多个class，但是只能有一个类声明为public，且要求声明public的类名必须与源文件名相同

2. 程序的入口是main()方法，格式是固定的

3. System.out.println()：输出后换行

   System.out.print()：输出

   System.out.printf()：输出规则同C中的printf

   \n  在Java里也可以换行

   String连接时用 + 

4. 每一句执行语句后有"  ;  "

5. 编译的过程：编译以后，会生成一个或多个字节码文件，字节码文件的文件名与Java源文件中的类名相同

6. API：语言提供的类库

## Java语法

**关键字**

- 所有关键字都是小写

- Java严格区分大小写

- Java中用final指示常量，作用同C中的const，被赋值后不能再修改

- 可以使用static final定义类常量

- default为默认访问权限

  ```java
  public class text{
      public static final int a = 1;//类常量需在main方法外部，加public修饰后可在其它类中调用
      public static void main(String[] args){
          System.out.println(a);
      }
  }
  ```

  

**保留字**

Java保留字：现有Java版本未使用，但以后版本可能会作为关键字使用，自己命名时要避开  ： goto，const   

**标识符**

- Java对各种变量，方法和类等要素命名时使用的字符序列称为标识符，由字母，数字和 _ $ 组成
- 数字不可开头，不能包含空格，长度无限制
- Java使用unicode字符集，因此标识符可以使用汉字声明，不建议使用

**命名规范**

- 包名：多单词组成时所有字母都小写：xxxyyyzzz
- 类名，接口名：多单词组成时，所有单词首字母大写：XxxYyyZzz
- 变量名，方法名：多单词组成时，第一个单词首字母小写，第二个单词开始每个单词首字母大写：xxxYyyZzz
- 常量名：所有字母都大写，多单词时每个单词之间用 _ 连接：XXX_YYY_ZZZ

**数据类型**

基本数据类型：

- 数值型：整型（byte(1字节，-128~127)，short(2字节，-2^15~2^15-1)，int(4字节，-2^31~2^31-1)，long(8字节，-2^63~2^63-1)），浮点型（float(4字节，-3.403E38~3.403E38)，double(8字节，-1.798E308~1.798E308)）

  - 声明long型变量，赋值时必须以" l "或" L "结尾，不带的话会自动转化为int型

  - Java浮点型默认为double型，声明float型，须在赋值时以" f "或" F "结尾，float精度为7位，double精度位float的两倍

  - 浮点型两种表示形式：小数型，科学记数法（规则同C）

- 字符型（char(2字节)），赋值时要带单引号，Java中字符可以输出中文字符

  - 直接用Unicode值表示字符型常量：'\uXXXX' ，XXXX表示一个十六进制整数，如'\u000a'表示\n

- 布尔型（boolean）：true；false，Java中true不代表1或非0的数，false不代表0，这与C不同

引用数据类型：

- 类（class）：字符属于类
- 接口（interfa）
- 数组（array）：（字符串String，赋值时加双引号）

tips：类型直接的运算不包括布尔型

自动类型转换：自动转换为高精度的

强制类型转换：语法与C相同 

八进制和十六进制与二进制之间的转换：二进制每三位看成一个数转换成八进制，每四位看成一个数转换成十六进制

**运算符**

1. Java支持连续赋值
2. instanceof   ：检查是否是类的对象  eg：“hello” instanceof String   结果为true
3. 位运算规则同C语言，运算符>>>无符号右移或无符号，即右移时无论补码的符号位是1还是0，补位时都补0
4. 移位操作的右值要完成对32取模，如果左值为long型，则对64取模

**输入**

Scanner类

需要import java.util.Scanner;

next():只读取输入直到空格。它不能读两个由空格或符号隔开的单词。此外，next()在读取输入后将光标放在同一行中。(next()只读空格之前的数据,并且光标指向本行)

nextLine():读取输入，包括单词之间的空格和除回车以外的所有符号(即。它读到行尾)。读取输入后，nextLine()将光标定位在下一行。

```java
//代码演示
public class Text {
	public static void main(String[] args) {
		Scanner input = new Scanner(System.in);//实例化
		System.out.println("请输入一个字符串(中间能加空格或符号)");
		String a = input.nextLine();
		System.out.println("请输入一个字符串(中间不能加空格或符号)");
		String b = input.next();
		System.out.println("请输入一个整数");
		int c;
		c = input.nextInt();
		System.out.println("请输入一个double类型的小数");
		double d = input.nextDouble();
		System.out.println("请输入一个float类型的小数");
		float f = input.nextFloat();
		System.out.println("按顺序输出abcdf的值：");
		System.out.println(a);
		System.out.println(b);
		System.out.println(c);
		System.out.println(d);
		System.out.println(f);
	}
}
```

```java
public class Text{
    public static void main(String[] args){
        String text = "hello world";
        int num=0;
        char a = text.charAt(num);//获取字符串num位置的字符
        System.out.println(a);
    }
}
```

**随机数**

Math.radom()；函数返回一个double型的[0.0,1.0]的数

**switch**

switch（）括号中表达式可以为byte,short,char,int ,枚举类型(JDK5.0新增),String类型(JDK7.0新增)

**流程控制**

break label;(结束指定标识(label)的一层循环结构)

continue label;(结束指定标识(label)的一层循环结构的当次循环)

```java
label:for(int i=0;i<3;i++){
    for(int j=0;j<3;j++){
        if(j % 2 == 0){
            break label;//结束外层循环
        }
        System.out.print(j);
    }
    System.out.println();
}


label:for(int i=0;i<3;i++){
    for(int j=0;j<3;j++){
        if(j % 2 == 0){
            continue label;//结束外层循环并执行下一次循环
        }
        System.out.print(j);
    }
    System.out.println();
}
```

**Math**

导入  import static java.lang.Math.*;

sqrt(x)：求平方根

pow(x,y)：同C中pow函数

Math类中提供一些常用的三角函数，指数函数及它的反函数，提供两个与常量接近的值PI，E

**判断字符串是否相等**

s.equals(t);

如果字符串s与字符串t相等返回true，否则返回false，s与t可以为字符串变量也可以为字符串字面量，判断字符串是否相等但不区分大小写s.equalsIgnoreCase(t);

不要使用 == 判断字符串是否相等

- 在String，Date,File,包装类等都重写了equals，用于比较属性，未重写的equals和==相等
- == 可以比较基本数据类型和引用地址

**toString**

String，Date，File，包装类等都重写了Object类中的toString()方法

**抽象类(abstract)**

1. 抽象类不能直接实例化
2. 抽象类里面可以有普通成员和方法
3. 一个类，如果继承了抽象类就必须重写所有抽象类中的抽象方法
4. 抽象类可以发生向上转型
5. 抽象类可以继承抽象类
6. 抽象类不能被**final**(修饰引用，方法(不能被重写)，类(不能被继承))修饰
**接口(Interface)**

1. 接口中的方法不能被实现
2. 接口中的方法默认为public abstract
3. 接口中可以有静态方法(JDK1.8)，通过接口名.方法可以直接调用，不能通过实现类调用
4. 接口中的默认方法可以通过对象调用，也可以被重写
5. 接口不能被直接实例化
6. 可以通过匿名内部类来间接实例化
7. 可以通过实现类的形式来进行实例化
8. 通过lambda表达式间接实例化
9. 如果实现类覆盖了接口中的所有抽象方法，则此实现类可以被实例化，否则反之
10. 接口和接口之间可以继承和多继承，用extends

- 创建了接口的非匿名实现类的非匿名对象
- 创建了接口的非匿名实现类的匿名对象
- 创建了接口的匿名实现类的非匿名对象
  - 直接拿接口实例化对象然后重写方法
- 创建了接口的匿名实现类的匿名对象

**内部类**

- 成员内部类（static成员内部类和非static成员内部类）
- 局部内部类（不谈修饰符），匿名内部类

实例化内部类：外部类.内部类 = new 外部类.内部类( )；

**GC(垃圾回收)**





**单例模式**

只生成一个实例，减少了系统性能开销

- 应用场景
  - 网站的计数器
  - 应用程序的日志应用
  - 数据库的连接池
  - 读取配置文件的类
  - Windows的Task Manager（任务管理器）典型的单例模式
  - Windows的Recycle Bin（回收站）
  - Application也是单例



- 饿汉模式

  - 线程安全

  1. 私有化类的构造器，使之不能通过new创建新的对象
  2. 内部创建类的对象(声明为静态)
  3. 提供公共的静态方法，返回类的对象

- 懒汉模式

  1. 私有化类的构造器 . . .

  2. 先声明当前类的对象，没有初始化

     ```java
     private static 类 instance = null;
     ```

  3. 声明public static 的返回当前类的对象的方法

     ```java
     public static 类 getInstance(){
         if(instance == null){
             instance = new 类();
         }
         return instance;
     }
     ```

  4. 使用同步机制使懒汉式线程安全

     ```java
     class lei{
         private lei(){}
         private static lei instance = null;
         public static synchronized lei geteInstance(){
             if(instance == null){
                 instance = new lei();
             }
             return instance;
         }
     }
     
     //同步代码块
     class lei{
         private lei(){}
         private static lei instance = null;
         public static lei geteInstance(){
             
             
             //效率较低
                 syncronized(lei.class){
                     if(instance == null){
                         instance = new lei();
                     }
                     return instance;
                 }
             
             
             
             //效率较高
              	if(instance == null){
                     synchronized(lei.class){
                         if(instance == new null){
                             instance = new lei();
                         }
                     }
                 }        
             return instance;
             
             
             
         }
     }
     ```

     

     

- 枚举类

- 静态内部类

1. 只能有一个实例
2. 必须自己创建自己的唯一的实例
3. 必须给所有其他对象提供这个实例

**模板方法设计模式**

抽象类体现的就是一种模板模式的设计，抽象类作为多个子类的通用模板，子类在抽象类的基础上进行扩展，改造，但子类总体上会保留抽象类的行为方式

**代理模式(Proxy)**

为其它对象提供一种代理以控制对这个对象的访问

- 应用场景：
  - 安全代理
  - 远程代理
  - 延迟加载
  - 静态代理
  - 动态代理

**工厂设计模式**

实现了创建者与调用者的分离，即将创建对象的具体过程屏蔽隔离起来，达到提高灵活性的目的

- 简单工厂模式
- 工厂方法模式
- 抽象工厂模式
- 

**代码块**

- 静态代码块

  static{  }

  - 内部可以有输出语句
  - 随着类的加载而执行，且只执行一次
  - 作用：初始化类的信息
  - 执行优先于非静态代码块

- 非静态代码块

  {  }

  - 内部可以有输出语句
  - 随着对象的创建而执行
  - 每创建一个对象，就执行一次非静态代码块
  - 作用：可以在创建对象时，对对象的属性等进行初始化

**异常处理**

1. try...catch...finally
2. throws + 异常类型

​	catch中一般调用

- getMessage( )
- printStackTrace( )

```java
try{
    可能出现的问题
    出现第一句异常后不再执行剩下的待监听语句
}catch(异常名 变量名){
    针对问题的处理
}finally{
    不管有没有异常，finally中一定会执行
    一般用于释放资源
}

/*
1.catch语句中必须有内容
2.try里面的代码越少越好
3.try可以跟多个catch
4.同一个catch中可以用 | 表示或来抛出多个异常
5.try-catch可以嵌套
*/
```



```java
public static void method() throws ArithmeticException{
    int a = 10;
    int b = 1;
    if(b==0){
        throw new ArithmeticException;//手动抛出
    }else{
        System.out.in.println(a/b);
    }
}

/*
throws只能跟在方法名后面
throw后面必须跟一个异常类对象
*/
```



```java
//自定义异常类
public class Test extends Exception{
    
}
/*
1.继承现有的异常结构，RuntimeException，Exception
2.提供全局常量，serialVersionUID
3.提供重载的构造器
```





**多线程**

并行：逻辑上同时发生，指的是在某一个时间段内同时运行多个程序

并发：物理上同时发生，指的是在某一个时间点同时运行多个程序

线程：在同一个进程内可以执行多个任务，每一个任务可以看作一个线程，是程序的执行单元，执行路径，是程序使用CPU的最基本单位

单线程：程序只有一条执行路径

多线程：程序有多条执行路径

多线程的意义：为了提高应用程序的使用率

线程的执行是具有随机性的

**创建一个线程：**

**方式一：**继承Thread类

1. 自定义MyThread类继承Thread类
2. 重写run( )方法（把此线程要执行的操作写在run中）
3. 创建对象
4. 启动线程(在main中调用start( )方法 )

**方式二：**实现Runnable接口

1. 自定义MyRunnable类实现Runnable接口
2. 重写run方法
3. 创建MyRunnable类的对象
4. 创建Thread类对象，并把第三步的对象作为构造参数传递进去
5. 启动线程（通过Thread类的对象调用start( )）

**方式三：**创建Callable接口

与使用Runnable相比，Callable功能更强大

- 相比run( )方法，可以有返回值
- 方法可以抛出异常
- 支持泛型的返回值
- 需要借助Future Task类，比如获取返回结果

1. 重写call( )方法
2. 创建MyCallable类的对象
3. 创建Future Task类对象，并把第二步的对象作为构造参数传递进去
4. 创建Thread类对象，并把第三步的对象作为构造参数传递进去
5. 通过start方法启动线程

**Future接口：**

- 可以对具体的Runnable，Callable任务的执行结果进行取消，查询是否完成，获取结果等
- Future Task是Future接口的唯一实现类
- Future Task同时实现了Runnable，Future接口，它既可以作为Runnable被线程执行，又可以作为Future得到Callable的返回值

**方式四：**使用线程池

提前创建好多个线程，放入线程池中，使用时直接获取，使用完放回池中，可以避免频繁创建销毁，实现重复利用

优点：

- 提高响应速度（减少了创建新线程的时间）
- 降低资源消耗（重复利用线程池中线程，不需要每次都创建）
- 便于线程管理

​	corePoolSize：线程池的大小

​	maximumPoolSize：最大线程数

​	keepAliveTime：线程没有任务时最多保持多长时间后会终止



1. 提供指定线程数量的线程池
2. 指定执行的线程的操作，需要提供实现Runnable接口(excute方法)或Callable接口(submit方法)实现类的对象
3. 关闭线程池(shutdown方法)

**ExecutorService：**真正的线程池接口，常见子类ThreadPoolExecutor

- void execute(Runnable command)：执行任务/命令，没有返回值，一般用来执行Runnable
- <T>Future<T>submit(Callable<T> task)：执行任务，有返回值，一般来执行Callable
- void shutdown( )：关闭线程池

**Executors：**工具类，线程池的工厂类，用于创建并返回不同类型的线程池

- Executors.newCachedThreadPool( )：创建一个可根据选哟创建新线程的线程池

- Executors.newFixedThreadPool(n)：创建一个可重用固定线程数的线程池

- Executors.newSingleThreadExecutor( )：创建一个只有一个线程的线程池

- Executors.newScheduledThreadPool(n)：创建一个线程池，它可安排在给定延迟后运行命令或定期的执行

  

**Thread类有关方法**s

- void start( )：启动线程，并执行对象的run( )方法
- run( )：线程在被调度时执行的操作
- String getName( )：返回线程的名称
- void setName(String name)：设置该线程名称
- static Thread.currentThread( )：返回当前线程，在Thread子类中就是this，常用于主线程和Runnable实现类
- static void yield( )：线程让步，暂停当前正在执行的线程，把执行几乎让给优先级相同或更高的线程，若队列中没有同优先级的线程忽略此方法，就是多个线程执行时，让cpu主动放掉当前的线程换另一个线程执行
- join( )：在线程A中调用线程B的join( )，此时线程A进入阻塞状态，直到线程B完全执行完后，线程A才结束阻塞状态
- static void sleep(long millis)：指定时间：毫秒，令当前活动线程在指定时间段内放弃对CPU控制，使其它线程有机会被执行，时间到后重新排队，抛出InterruptedException异常
- stop( )：强制线程生命期结束，不推荐使用
- boolean isAlive( )：返回boolean，判断线程是否还活着

**线程调度**

- 同优先级线程组成先进先出队列，使用时间片策略
- 对高优先级，使用优先调度的抢占式策略
- 线程的优先级等级
  - MAX_PRIORITY：10
  - MIN_PRIORITY：1
  - NORM_PRIORITY：5
- 涉及的方法：
  - getPriority( )：返回线程优先值
  - setPriority(int newPriority)：改变线程的优先级
- 线程创建时继承父线程的优先级
- 低优先级只是获取调度的概率低，并非一定是在高优先级线程之后才被调用

**同步线程**

1. 同步代码块

```java
//同步代码块
	synchronized(锁){	//同步监视器
        需要同步的代码		//操作共享数据的代码，即为需要被同步的代码
            			//共享数据：多个线程共同操作的变量
    }
//多线程必须是同一把锁，同一个锁对象，可以是任意一个对象
```

2. 同步方法

   如果操作共享数据的代码完整的声明在一个方法中，我们不妨将此方法声明为同步的

   ```java
   public void run(){
       show();
   }
   
   public synchronized void show(){
       需要同步的代码
   }
   //同步方法中将所有需要同步的代码写入一个方法中，同步监视器为this，用实现Runnable方法实现多线程不需要声明为静态方法，使用继承Thread类实现多线程需要将同步方法声明为静态方法，此时同步监视器为当前类本身
   ```

3. Lock锁

   ```java
   private ReentrantLock lock = new ReentrantLock();//默认参数为false，为非公平锁，线程之间靠抢，参数为true为公平锁，线程之间按照先进先出的队列执行
   
   ...
   try{
       lock.lock();
       同步代码
   }finally{
       lock.unlock();
   }
   ```

**synchronized与Lock异同**

相同：二者都可以解决线程安全问题

不同：synchronized机制在执行完相同的同步代码以后，自动的释放同步监视器，Lock需要手动的启动同步lock( )，同时结束同步也需要手动的实现unlock( )

**线程的死锁问题**

- 死锁：
  - 不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃自己需要的同步资源，就形成了线程的死锁
  - 出现死锁后，不会出现异常，不会出现提示，只是所有的线程都处于阻塞状态，无法继续
- 解决方法：
  - 专门的算法，原则
  - 尽量减少同步资源的定义
  - 尽量避免嵌套同步

**线程的通信**

- wait( )：一旦执行此方法，当前线程就进入阻塞状态，并释放同步监视器
- notify( )：一定执行此方法，就会唤醒被wait的一个线程，如果有多个线程被wait，就唤醒优先级高的线程，都一样就随机
- notifyAll( )：一旦执行此方法，就会唤醒所有被wait的线程
- wait( ),notify( ),notifyAll( )三个方法必须使用在同步代码块或同步方法中，调用者必须是同步代码块或同步方法的同步监视器，否则出现IllegalMonitorStateException异常，这三个方法被定义在object中

**sleep( )和wait( )的异同**

1. 相同点：一旦执行方法，都可以使得当前的线程进入阻塞状态
2. 不同点：
   - 两个方法声明的位置不同，Thread类中声明sleep( ),Object类中声明wait( )
   - 调用的要求不同：sleep( )可以在任何需要的场景下调用，wait( )必须在同步代码块或同步方法中
   - 关于是否释放同步监视器：如果两个方法都使用在同步代码块或同步方法中，sleep( )不会释放锁，wait( )释放锁



**String类**

1. String声明为final的，不可被继承

2. String实现了Serializable接口，表示字符串是支持序列化的

   实现了Comparable接口，表示String可以比较大小

3. String内部定义了final char[ ] value用于存储字符串数据

4. String代表不可变的字符序列，简称不可变性

   eg：当对字符串重新赋值时，需要重写指定内存区域赋值，不能使用原有的value进行赋值；

   当对现有的字符串进行连接操作时，也需要重新指定内存区域赋值，不能使用原有的value进行赋值

   当调用String的replace( )方法修改指定的字符或字符串时，也需要重新指定内存区域赋值

5. 通过字面量的方式（区别于new）给一个字符串赋值，此时的字符串值声明在字符串常量池中

6. 字符串常量池中是不会存储相同内容的字符串的

**枚举类**

1. 类的对象只有有限个，确定的，我们称此类为枚举类
2. 当需要定义一组常量时，强烈建议使用枚举类
3. 如果枚举类中只有一个对象，则可以作为单例模式的实现方式

自定义枚举类：

```java
class Season{
    //声明Season对象的属性：private final修饰
    private final String seasonName;
    private final String seasonDesc;
    
    //私有化类的构造器，并给对象属性赋值
    private Season(String seasonName,String seasonDesc){
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }
    
    //提供当前枚举类的多个对象：public static final
    public static final Season SPRING = new Season("春天"，"春暖花开")；
        
    //其他诉求：获取枚举类对象属性
        public String getSeasonName( ){
        
    }
}
```

JDK5.0之后用enum关键字定义枚举类

```java
enum Season(){
    //对象间用逗号隔开
    SPRING("春天"，"春")，SUMMER("夏天"，"夏")；
        
    //其它与自定义相同
    //声明Season对象的属性：private final修饰
    private final String seasonName;
    private final String seasonDesc;
    
    //私有化类的构造器，并给对象属性赋值
    private Season(String seasonName,String seasonDesc){
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }
        
    //其他诉求：获取枚举类对象属性
        public String getSeasonName( ){
        
    }
}
```

enum类主要方法

- values( )方法：返回枚举类型的数组。该方法可以很方便的遍历所有的枚举值
- valueOf(String str)：可以把一个字符串转为对应的枚举类对象，要求字符串必须是枚举类对象的“名字”，如不是，会有运行时异常：IllegalArgumentExcepton
- toString( )：返回当前枚举类对象常量的名称

使用enum关键字定义的枚举类实现接口的情况

1. 实现接口，在enum类中实现抽象方法
2. 让枚举类的对象分别实现接口中的抽象方法
   
**注解(Annotation)**

- 生成文档相关的注解

@author 表明开发该类模块的作者，多个作者之间使用，分隔

@version 表明该类模块的版本

@see 参考转向，也就是相关主题

@since 从哪个版本开始增加的

@param 对方法中某参数的说明，如果没有参数就不能写

@return 对方法返回值的说明，如果方法的返回值类型是void就不能写

@exception 对方法可能抛出的异常进行说明，如果方法没有用throws显式抛出的异常就不能写其中

- 在编译时进行格式检查（JDK内置的三个基本注解）

@Override 限定重写父类方法，改注解只能用于方法

@Deprecated 用于表示所修饰的元素（类，方法等）已过时，通常是因为所修饰的接口危险或存在更好的选择

@SuppressWarnings 抑制编译器警告

- 跟踪代码依赖性，实现替代配置文件功能

Servlet3.0提供了注解，使得不再需要在web.xml文件中进行Servlet的部署

**如何自定义注解**

参照@SuppressWarnings定义

1. 注解声明为：@interface
2. 内部定义成员，通常使用value表示
3. 可以指定成员的默认值，使用default定义
4. 如果自定义注解没有成员，表明是一个标识作用

如果注解有成员，在使用注解时，需要指明成员的值

自定义注解必须配上注解的信息处理流程（使用反射）才有意义

自定义注解都会通过指明两个元注解：Retention，Target

```java
public @interface MyAnnotation{
    
}
```

**元注解**

元注解用来修饰注解

JDK5.0提供4个标准的meta-annotation类型

- Retention

  只能用于修饰一个Annotation定义，用于指定该Annotation的生命周期，@Retention包含一个RetentionPolicy类型的成员变量，使用@Retention时必须为该value成员变量指定值

  - RetentionPolicy.SOURCE:在源文件中有效（即源文件保留），编译器直接丢弃这种策略的注释
  - RetentionPolicy.CLASS:在class文件中有效（即class保留），当运行Java程序时，JVM不会保留注解，这是默认值
  - RetentionPolicy.RUNTIME:在运行时有效（即运行时保留），当运行Java程序时，JVM会保留注释，程序可以通过反射获取该注释

- Target

  用于修饰Annotation的定义，用于指定被修饰的Annotation能用于修饰那些程序元素，@Target也包含一个名为value的成员变量

- Documented

  用于指定被该元注解修饰的注解类将被javadoc工具提取成文档，默认情况下，javadoc是不包括注解的

  - 定义为Documented的注解必须设置Retention值为RUNTIME

- Inherited

  被它修饰的注解将具有继承性，如果某个类使用了被@Inherited修饰的注解，则其子类将自动具有该注解

**jdk8注解新特性**

1. 可重复注解

   1）在MyAnnotation上声明@Repeatable，成员值为MyAnnotations.class

   2）MyAnnotation的Target和Retention和MyAnnotation相同

2. 类型注解

 **集合**

面向对象语言对事物的体现都是以对象的形式，为了方便对多个对象的操作，就要对对象进行存储，使用Array存储对象方面具有一些弊端，而Java集合就像一种容器，可以动态的把多个对象引用放入容器中

- 数组的内存存储方面的特点
  - 数组初始化后，长度就确定了
  - 数组声明的类型，就决定了进行元素初始化时的类型
- 数组在存储数据方面的弊端
  - 数组初始化后，长度就不可变了，不便于扩展
  - 数组中提供的属性和方法少，不便于进行添加，删除，插入等操作，且效率不高，同时无法直接获取存储元素的个数
  - 数组存储的数据是有序的，可以是重复的

Java集合类可以用于存储数量不等的多个对象，还可以用于保存具有映射关系的关联数组

**集合框架概述**

1. 集合，数组都是对多个数据进行存储操作的结果，简称Java容器

   说明：此时的存储，主要指的是内存层面的存储，不涉及到持久化的存储（txt，jpg，avi，数据库中）

2. Java集合可分为Collection和Map两种体系

   - Collection接口：单列集合，定义了存取一组对象的方法的集合，一个一个的对象

     - List：元素有序，可重复的数据  习惯称为"动态数组"
       - ArratList
       - LinkedList
       - Vector
     - Set：元素无序，不可重复的数据  类似数学中的"集合"
       - HashSet
       - LinkedHashSet
       - TreeSet

   - Map接口：双列集合，保存具有映射关系"key-value对"的集合，一对一对的数据   类似数学中的"函数"

     - HashMap：作为Map的主要实现类，线程不安全，效率高，可以存储null的key和value

       底层：数组+链表 （jdk7及之前）

       数组+链表+红黑树 （jdk8）

     - LinkedHashMap：保证在遍历map元素时，可以按照添加的顺序实现遍历，对于频繁的遍历操作，此类执行效率高于HashMap，原因：在原有的HashMap底层结构继承上，添加了一对指针，指向前一个和后一个元素

     - TreeMap：保证按照添加的key-value对进行排序，实现排序遍历，此时考虑key的自然排序或定制排序

       底层使用红黑树

     - Hashtable：作为古老的实现类，线程安全，效率低，不能存储null的key和value

     - Properties：常用来处理配置文件，key和value都是String类型

**使用Iterator迭代器接口遍历集合元素**

- 集合对象每次调用iterator( )方法都得到一个全新的迭代器对象，默认游标都在集合的第一个元素之前

- 内部的方法：hasNext( )和next( )
  - next( )：1）指针下移2）将下移以后集合位置上的元素返回
- 内部定义了remove( )，可以在遍历的时候，删除集合中的元素，此方法不同于集合直接调用remove( )

**List接口**

- ArrayList：作为List接口的主要实现类，线程不安全的，效率高，底层使用Object[ ] elementData存储
- sLinkedList：对应频繁的插入，删除操作，LinkedList效率较高，底层使用双向链表存储
- Vector：作为List接口的古老实现类，线程安全，效率低，底层使用Object[ ]数组 elementData存储

ArrayList，LinkedList，Vector三者的相同点：三者都是实现了List接口，存储数据的特点相同，存储有序的，可重复的数据

**Set接口**

- Set接口是Collection的子接口，set接口没有提供额外的方法
- Set集合不允许包含相同的元素，如果试把两个相同的元素加入同一个Set集合中，则添加操作失败
- Set判断两个对象是否相同不是使用 == 运算符，而是根据equals( )方法

**HashSet**

- HashSet是接口的典型实现，大多数时候使用Set集合时都使用这个实现类

- HashSet按Hash算法来存储集合中的元素，因此具有很好的存取，查找，删除性能

- 特点：

  - 不能保证元素的排列顺序
  - HashSet不是线程安全的
  - 集合元素可以时null

- HashSet集合判断两个元素相等的标准：两个对象通过hashCode( )方法比较相等，并且两个对象的equals( )方法返回值也相等

- 对于存放在Set容器中的对象，对应的类一定要重写equals( )和hashCode(Object obj)方法，以实现对象相等规则，即：“相等的对象必须具有相等的散列码”

- 添加元素的过程：

  - 向HashSet中添加元素a，手心啊调用元素a所在类的hashCode( )方法。计算元素a的哈希值此哈希值接着通过某种伏安法，，算出HashSet底层数组中的存放位置（即位：索引位置），判断数组此位置上是否已经有元素：

    - 如果此位置上没有元素，则元素a添加成功

    - 如果此位置上有其它元素，或以链表形式存在的多个元素，则比较元素a与元素b的hash值

      - 如果hash值不相同，则元素a添加成功

      - 如果相同需要调用元素a所在类的equals方法

        返回true添加失败

        返回false，添加成功

  - 对于添加成功的情况而言，元素a与已经查找指定索引位置上数据以链表的方式存储

    jdk7：元素啊放到数组中，指向原来的元素

    jdk8：原来的元素在数组中，指向元素a

  - 向Set中添加的数据，其所在的类一定要重写hashCode( )和equals( )

    重写的hashCode和equals尽可能保持一致性

**LinkedHashSet**

- LinkedHashSet是HashSet的子类
- LinkedHashSet根据元素的hashCode值来决定元素的存储位置，但它同时使用双向链表维护与那苏的次序，这使得元素看起来是以插入顺序保存的
- LinkedHashSet不允许集合元素重复

**TreeSet**

- TreeSet是SortSet接口的实现类，TreeSet可以确保集合元素处于排序状态
- TreeSet底层使用红黑树结构存储数据
- TreeSet两种排序方法：自然排序和定制排序，默认情况下采用自然排序
- 向TreeSet中添加的数据，要求是相同类的对象
- 自然排序中，比较两个对象是否相同的标准为：compareTo( )返回0，不再是eauals( )
- 定制排序中，比较两个对象是否相同的标准为：compare( )返回0，不再是equals( )

**Map结构的理解**

- Map中的key：无序的，不可重复的，使用Set存储所有的key  ；  key所在的类要重写equals( )和hashCode( )
- Map中的value：无序的，可重复的，使用Collection存储的所有的value
- 一个键值对：key-value构成了一个Entry对象
- Map中的entry：无序的，不可重复的，使用Set存储所有的entry

**HashMap的底层实现原理**

- jdk7：

  在实例化以后，底层创建了长度是16的一维数组Entry[ ] table

  map.put(key1.value1);

  首先，调用key1所在类的hashCode( )计算key1哈希值，此哈希值经过某种算法计算以后，得到在Entry数组中的存放位置，如果此位置是数据为空，此时的key1-value1添加成功，如果不为空（意味着此位置上存在一个或多个数据（以链表形式存在）），比较key1和已经存在的一个或多个哈希值，如果都不同，则添加成功，都相同调用equals方法，返回false添加成功，返回true，使用value1替换相同的key的value值

- jdk8相较于7的不同点：

  1. new HashMap( )：底层没有创建一个长度为16的数组

  2. jdk8底层的数组是Node[ ]，而非Entry[ ]

  3. 首次调用put( )方法时，底层创建长度为16的数组

  4. jdk7底层结构只有：数组+链表，jdk8中底层结构：数组+链表+红黑树

     当数组的某一个索引位置上的元素以链表形式存在的数据个数 > 8 且当前数组的长度 > 64时，此时此索引位置上的所有数据改为使用红黑树存储

**泛型**

允许在定义类，接口时通过一个标识标识类中某个属性的类型或者是某个方法的返回值 及参数类型，这个类型参数将在使用时（例如，继承或实现这个接口，用这个类型声明变量，创建对象时）确定（即传入实际的类型参数，也称为类型实参）

**在集合中使用泛型**

1. 集合接口或集合类在jdk5.0时都修改为带泛型的结构
2. 在实例化集合类时，可以指明具体的泛型类型
3. 指明完以后，在集合类或接口中凡是定义类或接口时，内部结构使用到类的泛型的位置，都指定为实例化的泛型类型
4. 泛型的类型必须是类，不能是基本数据类型，需要用到基本数据类型的，拿包装类代替
5. 如果实例化时，没有指明泛型的类型，默认类型为java.lang.Object

**自定义泛型类**

1. 泛型类可能有多个参数，此时应将多个参数一起放在尖括号内，比如<E1,E2,E3>

2. 泛型类的构造器如下：`public GenericClass( ){ }`

3. 实例化操作原来泛型位置的结构必须与指定的泛型类型一致

4. 泛型不同的引用不能相互赋值

   尽管在编译时ArrayList<String>和ArrayLIst<Integer>是两种类型，但是在运行时只有一个ArrayList被加载到JVM中

5. 泛型如果不指定，将被擦除，泛型毒药的类型均按照Object处理，但不等价于Object，泛型要使用一路都用，要不用，一路都不用

6. 如果泛型类是一个接口或抽象类，则不可创建泛型类的对象

7. jdk1.7，泛型的简化操作：ArrayList<Fruit>flist = new ArrayList<>( )

8. 泛型的指定中不能使用基本数据类型，可以使用包装类替换

9. 在类/接口是声明的泛型，在本类或本接口中即代表某种类型，可以作为非静态属性的裂隙，非静态方法的参数类型，非静态方法的返回值类型，但在静态方法中不能使用类的泛型

10. 异常类不能是泛型的

11. 不能使用new  E[ ]，但是可以: E[ ] elements = (E[ ])new Object[capacity];

12. 父类有泛型，子类可以选择保留泛型也可以选择指定泛型类型：

    - 子类不保留父类的泛型：按需实现
      - 没有类型 擦除
      - 具体类型
    - 子类保留父类的泛型：泛型子类
      - 全部保留
      - 部分保留

    子类除了指定或保留父亲的泛型，还可以增加自己的泛型

**泛型方法**

在方法中出现了泛型的结构，泛型参数与类的泛型参数没有任何关系，即泛型方法所属的类是不是泛型都没有关系

`public <E> List<E> f (E[ ] arr){ }`

泛型方法，可以声明为静态的，原因：泛型参数是在调用时确定的，并非在实例化类时确定

**泛型在继承方面的体现**

1. 虽然类A是类B的父类，但是G<A>和G<B>二者并不具备子父类关系，二者是并列关系
2. 类A是类B的父类，A<G> 是 B<G> 的父类 

**通配符的使用**

通配符：?

类A是类B的父类，G<A> 和G<B>是没有关系的，二者共同的父类是 G<?>

eg：

```java
public void test(){
    List<Object> list1 = null;
    List<String> list2 = null;
    
    List<?> list = null;
    
    list = list1;
    list = list2;
    
    //对于List<?>就不能向其内部添加数据，除了添加null
    //允许读取数据，读取的数据类型为Object
}
```

**有限制条件的通配符的使用**

? 相当于数学中的无穷区间

`? extends A`  相当于右区间为A

​	G<? extends A> 可以作为G<A> 和G<B>的父类，其中B是A的子类

`? super A`  相当于左区间为A

​	G<? super A>可以作为G<A>和G<B>的父类，其中B是A的父类

**File类**

- java.io.File类：文件和文件目录路径的抽象表示形式，与平台无关
- File能新建，删除，重命名文件和目录，但File不能访问文件内容本身，如果需要访问文件内容本身，则需要使用输入输出流
- 想要在Java程序中标识一个真实存在的文件或目录，那么必须有一个File对象，但是Java程序中的一个File对象，可能没有一个真实存在的文件或目录
- File对象可以作为参数传递给流的构造器

```java
public File(String Pathname)	//以pathname为路径创建File对象，如果为相对路径，则默认当前路径在系统属性user.dir中存储
 
public File(String parent,String child)	//以parent为父路径。child为子路径创建File对象
    
public File(File parent,String child)	//根据一个父File对象和子文件路径创建File对象
    
//Windows和DOS系统默认使用"\"来表示
//UNIX和URL使用"/"来表示
```

