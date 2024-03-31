# javaSE

## 初级

**JRE（Java Runtime Environment）**：Java的运行环境，安装了运行环境之后，Java程序才可以运行，一般不做开发，只是需要运行Java程序直接按照JRE即可。

**JDK（Java Development Kit）**：包含JRE，并且还附带了大量开发者工具，我们学习Java程序开发就使用JDK即可。

### Java八种基本数据类型

整形：byte,int,short,long

浮点型：float,double

字符型：char

布尔型：boolean

### 面向对象

#### 类与对象

```java
public static void main(String[] args) {
  	//这里的a存放的是具体的某个值
  	int a = 10;
  	//创建一个变量指代我们刚刚创建好的对象，变量的类型就是对应的类名
  	//这里的p存放的是对象的引用，而不是本体，我们可以通过对象的引用来间接操作对象
    Person p = new Person();
    Person p2 = p1;
    //这里，我们将变量p2赋值为p1的值，那么实际上只是传递了对象的引用，而不是对象本身的复制，这跟我们前面的基本数据类型有些不同，p2和p1都指向的是同一个对象
    System.out.println(p1 == p2);    //使用 == 可以判断两个变量引用的是不是同一个对象
}
```

##### 方法

仅仅返回值不同，不可以重载

##### 构造方法

在我们自己定义一个构造方法之后，会覆盖掉默认的那一个无参构造方法，除非我们手动重载一个无参构造，否则要创建这个类的对象，必须调用我们自己定义的构造方法：

##### 静态属性

静态的内容，我们可以理解为是属于这个类的，也可以理解为是所有对象共享的内容。我们通过使用`static`关键字来声明一个变量或一个方法为静态的，一旦被声明为静态，那么通过这个类创建的所有对象，操作的都是同一个目标，也就是说，对象再多，也只有这一个静态的变量或方法。一个对象改变了静态变量的值，那么其他的对象读取的就是被改变的值。

一般情况下，我们并不会通过一个具体的对象去修改和使用静态属性，而是通过这个类去使用

```java
public static void main(String[] args) {
    Person.info = "让我看看";
    System.out.println(Person.info);
}
static void test(){
    System.out.println("我是静态方法");
}
```

那么，静态变量，是在什么时候进行初始化的呢？

我们实际上是将.class文件丢给JVM去执行的，而每一个.class文件其实就是我们编写的一个类，我们在Java中使用一个类之前，JVM并不会在一开始就去加载它，而是在需要时才会去加载（优化）一般遇到以下情况时才会会加载类：

- 访问类的静态变量，或者为静态变量赋值
- new 创建类的实例（隐式加载）
- 调用类的静态方法
- 子类初始化时
- 其他的情况会在讲到反射时介绍

所有被标记为静态的内容，会在类刚加载的时候就分配，而不是在对象创建的时候分配，所以说静态内容一定会在第一个对象初始化之前完成加载。

#### 包和访问控制

##### 包

包的命名规则同样是英文和数字的组合，最好是一个域名的格式，比如我们经常访问的`www.baidu.com`，后面的baidu.com就是域名，我们的包就可以命名为`com.baidu`

当我们使用同一个包中的类时，直接使用即可（之前就是直接使用的，因为都直接在一个缺省的包中）而当我们需要使用其他包中的类时，需要先进行导入才可以：

##### 访问权限控制

所以说Java中引入了访问权限控制（可见性），我们可以为成员变量、成员方法、静态变量、静态方法甚至是类指定访问权限，不同的访问权限，有着不同程度的访问限制：

- private - 私有，标记为私有的内容无法被除当前类以外的任何位置访问。
- 什么都不写 - 默认，默认情况下，只能被类本身和同包中的其他类访问。
- protected - 受保护，标记为受保护的内容可以能被类本身和同包中的其他类访问，也可以被子类访问
- public - 公共，标记为公共的内容，允许在任何地方被访问。

#### 封装继承多态

封装、继承和多态是面向对象编程的三大特性。

> 封装，把对象的属性和方法结合成一个独立的整体，隐藏实现细节，并提供对外访问的接口。
>
> 继承，从已知的一个类中派生出一个新的类，叫子类。子类实现了父类所有非私有化的属性和方法，并根据实际需求扩展出新的行为。
>
> 多态，多个不同的对象对同一消息作出响应，同一消息根据不同的对象而采用各种不同的方法。

##### 封装

封装的目的是为了保证变量的安全性，使用者不必在意具体实现细节，而只是通过外部接口即可访问类的成员，如果不进行封装，类中的实例变量可以直接查看和修改，可能给整个代码带来不好的影响，因此在编写类时一般将成员变量私有化，外部类需要使用Getter和Setter方法来查看和设置变量

```java
//我们甚至还可以将构造方法改成私有的，需要通过我们的内部的方式来构造对象：
public class Person {
    private String name;
    private int age;
    private String sex;

    private Person(){}   //不允许外部使用new关键字创建对象
    
    public static Person getInstance() {   //而是需要使用我们的独特方法来生成对象并返回
        return new Person();
    }
}
//通过这种方式，我们可以实现单例模式：
public class Test {
    private static Test instance;
    
    private Test(){}
    
    public static Test getInstance() {
        if(instance == null) 
            instance = new Test();
        return instance;
    }
}
```

##### 继承

在定义不同类的时候存在一些相同属性，为了方便使用可以将这些共同属性抽象成一个父类，在定义其他子类时可以继承自该父类，减少代码的重复定义，子类可以使用父类中**非私有**的成员。想要继承一个类，我们只需要使用`extends`关键字即可。

类的继承可以不断向下，但是同时只能继承一个类，同时，标记为`final`的类不允许被继承

当一个类继承另一个类时，属性会被继承，可以直接访问父类中定义的属性，除非父类中将属性的访问权限修改为`private`，那么子类将无法访问（但是依然是继承了这个属性的）

同样的，在父类中定义的方法同样会被子类继承，子类直接获得了此方法，当我们创建一个子类对象时就可以直接使用这个方法：

如果父类存在一个有参构造方法，子类必须在构造方法中调用，因为子类在构造时，不仅要初始化子类的属性，还需要初始化父类的属性，所以说在默认情况下，子类其实是调用了父类的构造方法的，只是在无参的情况下可以省略，但是现在父类构造方法需要参数，那么我们就需要手动指定了：

```java
public class Person {
    protected String name;   //因为子类需要用这些属性，所以说我们就将这些变成protected，外部不允许访问
    protected int age;
    protected String sex;
    protected String profession;

  	//构造方法也改成protected，只能子类用
    protected Person(String name, int age, String sex, String profession) {
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.profession = profession;
    }

    public void hello(){
        System.out.println("["+profession+"] 我叫 "+name+"，今年 "+age+" 岁了!");
    }
}

public class Student extends Person{
    public Student(String name, int age, String sex) {    //因为学生职业已经确定，所以说学生直接填写就可以了
        super(name, age, sex, "学生");   //使用super代表父类，父类的构造方法就是super()
    }

    public void study(){
        System.out.println("我的名字是 "+name+"，我在学习！");
    }
}

public class Worker extends Person{
    public Worker(String name, int age, String sex) {
        super(name, age, sex, "工人");    //父类构造调用必须在最前面
        System.out.println("工人构造成功！");    //注意，在调用父类构造方法之前，不允许执行任何代码，只能在之后执行
    }
}

//我们在使用子类时，可以将其当做父类来使用：
public static void main(String[] args) {
    Person person = new Student("小明", 18, "男");    //这里使用父类类型的变量，去引用一个子类对象（向上转型）
    person.hello();    //父类对象的引用相当于当做父类来使用，只能访问父类对象的内容
}

//虽然我们这里使用的是父类类型引用的对象，但是这并不代表子类就彻底变成父类了，这里仅仅只是当做父类使用而已。我们也可以使用强制类型转换，将一个被当做父类使用的子类对象，转换回子类：
public static void main(String[] args) {
    Person person = new Student("小明", 18, "男");
    Student student = (Student) person;   //使用强制类型转换（向下转型）
    student.study();
}
//但是注意，这种方式只适用于这个对象本身就是对应的子类才可以，如果本身都不是这个子类，或者说就是父类，那么会出现问题：
public static void main(String[] args) {
    Person person = new Worker("小明", 18, "男");   //实际创建的是Work类型的对象
    Student student = (Student) person;
    student.study();
}//报错，因为本身不是这个类型，强转也没用。



public static void main(String[] args) {
    Person person = new Student("小明", 18, "男");
    if(person instanceof Student) {   //我们可以使用instanceof关键字来对类型进行判断
        System.out.println("对象是 Student 类型的");
    }
    if(person instanceof Person) {
        System.out.println("对象是 Person 类型的");
    }
}
```

在子类存在同名变量的情况下，我们同样可以使用`super`关键字来表示父类：但是注意，没有`super.super`这种用法，也就是说如果存在多级继承的话，那么最多只能通过这种方法访问到父类的属性（包括继承下来的属性）

##### 顶层Object类

实际上所有类都默认继承自Object类，除非手动指定继承的类型，但是依然改变不了最顶层的父类是Object类。所有类都包含Object类中的方法

##### 方法的重写

方法的重写不同于之前的方法重载，方法的重载是为某个方法提供更多种类，而方法的重写是覆盖原有的方法实现，比如我们现在不希望使用Object类中提供的`equals`方法，那么我们就可以将其重写了，静态方法不支持重写，因为它是属于类本身的，但是它可以被继承。

注意，我们如果不希望子类重写某个方法，我们可以在方法前添加`final`关键字，表示这个方法已经是最终形态

或者，如果父类中方法的可见性为`private`，那么子类同样无法访问，也就不能重写，但是可以定义同名方法，虽然这里可以编译通过，但是并不是对父类方法的重写，仅仅是子类自己创建的一个新方法。还有，我们在重写父类方法时，如果希望调用父类原本的方法实现，那么同样可以使用`super`关键字，子类在重写父类方法时，不能降低父类方法中的可见性，总之，子类重写的方法权限不能比父类还低。

##### 抽象类

```java
public abstract class Person {   //通过添加abstract关键字，表示这个类是一个抽象类
    protected String name;   //大体内容其实普通类差不多
    protected int age;
    protected String sex;
    protected String profession;

    protected Person(String name, int age, String sex, String profession) {
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.profession = profession;
    }

    public abstract void exam();   //抽象类中可以具有抽象方法，也就是说这个方法只有定义，没有方法体
}
```

而具体的实现，需要由子类来完成，而且如果是子类，必须要实现抽象类中所有抽象方法,抽象类由于不是具体的类定义（它是类的抽象）可能会存在某些方法没有实现，因此无法直接通过new关键字来直接创建对象.

要使用抽象类，我们只能去创建它的子类对象。抽象类一般只用作继承使用，当然，抽象类的子类也可以是一个抽象类

注意，抽象方法的访问权限不能为`private`

##### 接口

接口中只能出现3种成员
1.公共的静态常量(public final static )
2.公共的抽象方法(public abstract )
3.静态内部类(static class)

接口甚至比抽象类还抽象，他只代表某个确切的功能！也就是只包含方法的定义，甚至都不是一个类！接口一般只代表某些功能的抽象，接口包含了一些列方法的定义，类可以实现这个接口，表示类支持接口代表的功能（类似于一个插件，只能作为一个附属功能加在主体上，同时具体实现还需要由主体来实现）

```java
public interface Study {    //使用interface表示这是一个接口
    void study();    //接口中只能定义访问权限为public抽象方法，其中public和abstract关键字可以省略
}

//我们可以让类实现这个接口：
public class Student extends Person implements Study {   //使用implements关键字来实现接口
    public Student(String name, int age, String sex) {
        super(name, age, sex, "学生");
    }

    @Override
    public void study() {    //实现接口时，同样需要将接口中所有的抽象方法全部实现
        System.out.println("我会学习！");
    }
}

//接口不同于继承，接口可以同时实现多个：
public class Student extends Person implements Study, A, B, C {  //多个接口的实现使用逗号隔开
  
}
```

接口跟抽象类一样，不能直接创建对象，但是我们也可以将接口实现类的对象以接口的形式去使用：

![image-20220921234735828](https://image.itbaima.cn/markdown/2022/09/21/VJfhzYKuF38tRq4.png)

当做接口使用时，只有接口中定义的方法和Object类的方法，无法使用类本身的方法和父类的方法。

```java
//从Java8开始，接口中可以存在方法的默认实现：
public interface Study {
    void study();

    default void test() {   //使用default关键字为接口中的方法添加默认实现
        System.out.println("我是默认实现");
    }
}
//如果方法在接口中存在默认实现，那么实现类中不强制要求进行实现。

//接口不同于类，接口中不允许存在成员变量和成员方法，但是可以存在静态变量和静态方法，在接口中定义的变量只能是：
public interface Study {
    public static final int a = 10;   //接口中定义的静态变量只能是public static final的
  
  	public static void test(){    //接口中定义的静态方法也只能是public的
        System.out.println("我是静态方法");
    }
    
    void study();
}
```

跟普通的类一样，我们可以直接通过接口名.的方式使用静态内容,接口是可以继承自其他接口的,并且接口没有继承数量限制，接口支持多继承：



```java
//最后我们来介绍一下Object类中提供的克隆方法，为啥要留到这里才来讲呢？因为它需要实现接口才可以使用：

package java.lang;

public interface Cloneable {    //这个接口中什么都没定义
}



//实现接口后，我们还需要将克隆方法的可见性提升一下，不然还用不了：
public class Student extends Person implements Study, Cloneable {   //首先实现Cloneable接口，表示这个类具有克隆的功能
    public Student(String name, int age, String sex) {
        super(name, age, sex, "学生");
    }

    @Override
    public Object clone() throws CloneNotSupportedException {   //提升clone方法的访问权限
        return super.clone();   //因为底层是C++实现，我们直接调用父类的实现就可以了
    }
    
    @Override
    public void study() {
        System.out.println("我会学习！");
    }

}




//接着我们来尝试一下，看看是不是会得到一个一模一样的对象：

public static void main(String[] args) throws CloneNotSupportedException {  //这里向上抛出一下异常，还没学异常，所以说照着写就行了
    Student student = new Student("小明", 18, "男");
    Student clone = (Student) student.clone();   //调用clone方法，得到一个克隆的对象
    System.out.println(student);
    System.out.println(clone);
    System.out.println(student == clone);
}
```



克隆操作可以完全复制一个对象的所有属性，但是像这样的拷贝操作其实也分为浅拷贝和深拷贝。

浅拷贝： 对于类中基本数据类型，会直接复制值给拷贝对象；对于引用类型，只会复制对象的地址，而实际上指向的还是原来的那个对象，拷贝个基莫。
深拷贝： 无论是基本类型还是引用类型，深拷贝会将引用类型的所有内容，全部拷贝为一个新的对象，包括对象内部的所有成员变量，也会进行拷贝。

那么clone方法出来的克隆对象，是深拷贝的结果还是浅拷贝的结果呢？

```java
public static void main(String[] args) throws CloneNotSupportedException {
    Student student = new Student("小明", 18, "男");
    Student clone = (Student) student.clone();
    System.out.println(student.name == clone.name);
}

true
```

可以看到，虽然Student对象成功拷贝，但是其内层对象并没有进行拷贝，依然只是对象引用的复制，所以Java为我们提供的`clone`方法只会进行浅拷贝。

#### 枚举类

```java
public enum Status {   //enum表示这是一个枚举类，枚举类的语法稍微有一些不一样
    RUNNING, STUDY, SLEEP;    //直接写每个状态的名字即可，最后面分号可以不打，但是推荐打上
}

//使用枚举类也非常方便，就像使用普通类型那样：
private Status status;   //类型变成刚刚定义的枚举类

public Status getStatus() {
    return status;
}

public void setStatus(Status status) {
    this.status = status;
}
```

枚举类型使用起来就非常方便了，其实枚举类型的本质就是一个普通的类，但是它继承自`Enum`类，我们定义的每一个状态其实就是一个`public static final`的Status类型成员变量：

```java
//既然枚举类型是普通的类，那么我们也可以给枚举类型添加独有的成员方法：
public enum Status {
    RUNNING("睡觉"), STUDY("学习"), SLEEP("睡觉");   //无参构造方法被覆盖，创建枚举需要添加参数（本质就是调用的构造方法）

    private final String name;    //枚举的成员变量
    Status(String name){    //覆盖原有构造方法（默认private，只能内部使用！）
        this.name = name;
    }

    public String getName() {   //获取封装的成员变量
        return name;
    }
}
```

枚举类还自带一些继承下来的实用方法，比如获取枚举类中的所有枚举

#### 基本类型包装类

其中能够表示数字的基本类型包装类，继承自Number类，对应关系如下表：

byte -> Byte
boolean -> Boolean
short -> Short
char -> Character
int -> Integer
long -> Long
float -> Float
double -> Double

字符和布尔继承自Object类

char -> Character

boolean -> Boolean

```java
//自动装箱：
public static void main(String[] args) {   
    Integer i = 10;    //将int类型值作为包装类型使用
    Integer i = Integer.valueOf(10);    //上面的写法跟这里是等价的
}


//本质
public static void main(String[] args) {
    Integer i = 10;
    int a = i.intValue();   //通过此方法变成基本类型int值
}

//自动拆箱
public static void main(String[] args) {
    Integer a = 10, b = 20;
    int c = a * b;    //直接自动拆箱成基本类型参与到计算中
    System.out.println(c);
}

public static void main(String[] args) {
    Integer a = new Integer(10);
    Integer b = new Integer(10);

    System.out.println(a == b);    //虽然a和b的值相同，但是并不是同一个对象，所以说==判断为假
}

//我们发现，通过自动装箱转换的Integer对象，如果值相同，得到的会是同一个对象，这是因为：
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)   //这里会有一个IntegerCache，如果在范围内，那么会直接返回已经提前创建好的对象
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
//IntegerCache会默认缓存-128~127之间的所有值，将这些值提前做成包装类放在数组中存放，虽然我们目前还没有学习数组，但是各位小伙伴只需要知道，我们如果直接让 -128~127之间的值自动装箱为Integer类型的对象，那么始终都会得到同一个对象，这是为了提升效率，因为小的数使用频率非常高，有些时候并不需要创建那么多对象，创建对象越多，内存也会消耗更多。但是如果超出这个缓存范围的话，就会得到不同的对象了

```

除了我们上面认识的这几种基本类型包装类之外，还有两个比较特殊的包装类型。

其中第一个是用于计算超大数字的BigInteger，我们知道，即使是最大的long类型，也只能表示64bit的数据，无法表示一个非常大的数，但是BigInteger没有这些限制，我们可以让他等于一个非常大的数字：

```java
public static void main(String[] args) {
    BigInteger i = BigInteger.valueOf(Long.MAX_VALUE);
    i = i.multiply(BigInteger.valueOf(Long.MAX_VALUE));   //即使是long的最大值乘以long的最大值，也能给你算出来
    System.out.println(i);
}
```

我们接着来看第二种，前面我们说了，浮点类型精度有限，对于需要精确计算的场景，就没办法了，而BigDecimal可以实现小数的精确计算。

```java
public static void main(String[] args) {
    BigDecimal i = BigDecimal.valueOf(10);
    i = i.divide(BigDecimal.valueOf(3), 100, RoundingMode.CEILING);
  	//计算10/3的结果，精确到小数点后100位
  	//RoundingMode是舍入模式，就是精确到最后一位时，该怎么处理，这里CEILING表示向上取整
    System.out.println(i);
}
```



#### 数组，字符串

```java
public static void main(String[] args) {
    int[] array = new int[10];   //在创建数组时，需要指定数组长度，也就是可以容纳多个int变量的值
  	Object obj = array;   //因为同样是类，肯定是继承自Object的，所以说可以直接向上转型
}

类型[] 变量名称 = new 类型[数组大小];
类型 变量名称[] = new 类型[数组大小];  //支持C语言样式，但不推荐！

类型[] 变量名称 = new 类型[]{...};  //静态初始化（直接指定值和大小）
类型[] 变量名称 = {...};   //同上，但是只能在定义时赋值

//有时候为了方便，我们可以使用简化版的for语句foreach语法来遍历数组中的每一个元素：
public static void main(String[] args) {
    int[] array = new int[10];
    for (int i : array) {    //int i就是每一个数组中的元素，array就是我们要遍历的数组
        System.out.print(i+" ");   //每一轮循环，i都会更新成数组中下一个元素
    }
}

public static void main(String[] args) {   //反编译的结果
    int[] array = new int[10];
    int[] var2 = array;
    int var3 = array.length;
    for(int var4 = 0; var4 < var3; ++var4) {
        int i = var2[var4];
        System.out.print(i + " ");
    }

}
```

对于基本类型的数组来说，是不支持自动装箱和拆箱的：还有，由于基本数据类型和引用类型不同，所以说int类型的数组时不能被Object类型的数组变量接收的,  但是如果是引用类型的话，是可以的

##### 可变长参数

```java
public class Person {
    String name;
    int age;
    String sex;

    public void test(String... strings){

    }
}

//我们在使用时，可以传入0 - N个对应类型的实参：
public static void main(String[] args) {
    Person person = new Person();
    person.test("1！", "5！", "哥们在这跟你说唱"); //这里我们可以自由传入任意数量的字符串
}

//那么我们在方法中怎么才能得到这些传入的参数呢，实际上可变长参数本质就是一个数组：
public void test(String... strings){   //strings这个变量就是一个String[]类型的
    for (String string : strings) {
        System.out.println(string);   //遍历打印数组中每一个元素
    }
}

//注意，如果同时存在其他参数，那么可变长参数只能放在最后：
public void test(int a, int b, String... strings){
    
}



//这个是我们在执行Java程序时，输入的命令行参数，我们可以来打印一下：

public static void main(String[] args) {
    for (String arg : args) {
        System.out.println(arg);
    }
}
```

可以看到，默认情况下直接运行什么都没有，但是如果我们在运行时，添加点内容的话：

java com/test/Main lbwnb aaaa xxxxx   #放在包中需要携带主类完整路径才能运行

##### 字符串

字符串类是一个比较特殊的类，它用于保存字符串。我们知道，基本类型`char`可以保存一个2字节的Unicode字符，而字符串则是一系列字符的序列（在C中就是一个字符数组）Java中没有字符串这种基本类型，因此只能使用类来进行定义。注意，字符串中的字符一旦确定，无法进行修改，只能重新创建。

String本身也是一个类，只不过它比较特殊，每个用双引号括起来的字符串，都是String类型的一个实例对象：

注意，如果是直接使用双引号创建的字符串，如果内容相同，为了优化效率，那么始终都是同一个对象：

```java
public static void main(String[] args) {
    String str1 = "Hello World";
    String str2 = "Hello World";
    System.out.println(str1 == str2);
}
```

但是如果我们使用构造方法主动创建两个新的对象，那么就是不同的对象了

如果我们仅仅是想要判断两个字符串的内容是否相同，不要使用`==`，String类重载了`equals`方法用于判断和比较内容是否相同

```java
//字符数组和字符串之间是可以快速进行相互转换的：
public static void main(String[] args) {
    String str = "Hello World";
    char[] chars = str.toCharArray();
    System.out.println(chars);
}
public static void main(String[] args) {
    char[] chars = new char[]{'奥', '利', '给'};
    String str = new String(chars);
    System.out.println(str);
}
```

##### StringBuilder类

拼接字符串实际上底层需要进行很多操作，如果程序中大量进行字符串的拼接似乎不太好，编译器是很聪明的，String的拼接会在编译时进行各种优化：

在使用 StringBuffer 类时，每次都会对 StringBuffer 对象本身进行操作，而不是生成新的对象，所以如果需要对字符串进行修改推荐使用 StringBuffer。

StringBuilder 类在 Java 5 中被提出，它和 StringBuffer 之间的最大不同在于 StringBuilder 的方法不是线程安全的（不能同步访问）。

由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类。

如果直接使用加的话，每次运算都会生成一个新的对象，这里进行4次加法运算，那么中间就需要产生4个字符串对象出来，是不是有点太浪费了？这种情况实际上会被优化为下面的写法：

```java
public static void main(String[] args) {
    String str1 = "你看";
    String str2 = "这";
    String str3 = "汉堡";
    String str4 = "做滴";
    String str5 = "行不行";
    StringBuilder builder = new StringBuilder();
    builder.append(str1).append(str2).append(str3).append(str4).append(str5);
    System.out.println(builder.toString());
}

```

##### 正则表达式

我们现在想要实现这样一个功能，对于给定的字符串进行判断，如果字符串符合我们的规则，那么就返回真，否则返回假，比如现在我们想要判断字符串是不是邮箱的格式：

```java
public static void main(String[] args) {
    String str = "aaaa731341@163.com";
  	//假设邮箱格式为 数字/字母@数字/字母.com
}
```

正则表达式(regular expression)描述了一种字符串匹配的模式（pattern），可以用来检查一个串是否含有某种子串、将匹配的子串替换或者从某个串中取出符合某个条件的子串等。

用于规定给定组件必须要出现多少次才能满足匹配的，我们一般称为限定符，限定符表如下：
字符 	描述

*****     匹配前面的子表达式零次或多次。例如，zo* 能匹配 "z" 以及 "zoo"。***** 等价于 {0,}。

**+**      匹配前面的子表达式一次或多次。例如，zo+ 能匹配 "zo" 以及 "zoo"，但不能匹配 "z"。+ 等价于 {1,}。

? 	匹配前面的子表达式零次或一次。例如，do(es)? 可以匹配 "do" 、 "does"、 "doxy" 中的 "do" 。? 等价于 {0,1}。

{n} 	n 是一个非负整数。匹配确定的 n 次。例如，o{2} 不能匹配 "Bob" 中的 o，但是能匹配 "food" 中的两个 o。

{n,} 	n 是一个非负整数。至少匹配n 次。例如，o{2,} 不能匹配 "Bob" 中的 o，但能匹配 "foooood" 中的所有 o。o{1,} 等价于 o+。o{0,} 则等价于 o*。

{n,m}    m 和 n 均为非负整数，其中 n <= m。最少匹配 n 次且最多匹配 m 次。例如，o{1,3} 将匹配 "fooooood" 中的前三个 o。o{0,1} 等价于 o?。请注意在逗号和两个数之间不能有空格。

```java
//如果我们想要表示一个范围内的字符，可以使用方括号：
public static void main(String[] args) {
    String str = "abcabccaa";
    System.out.println(str.matches("[abc]*"));   //表示abc这几个字符可以出现 0 - N 次
}
```

字符 	描述
[ABC]    	匹配 [...] 中的所有字符，例如 [aeiou] 匹配字符串 "google runoob taobao" 中所有的 e o u a 字母。
[ ^ABC] 	匹配除了 [...] 中字符的所有字符，例如 [ ^aeiou] 匹配字符串 "google runoob taobao" 中除了 e o u a 字母的所有字母。
[A-Z] 	     [A-Z] 表示一个区间，匹配所有大写字母，[a-z] 表示所有小写字母。
. 	            匹配除换行符（\n、\r）之外的任何单个字符，相等于 [ ^\n\r]
[\s\S] 	    匹配所有。\s 是匹配所有空白符，包括换行，\S 非空白符，不包括换行。
\w 	匹配字母、数字、下划线。等价于 [A-Za-z0-9_]



#### 内部类

##### 成员内部类

```java
//我们可以直接在类的内部定义成员内部类：
public class Test {
    public class Inner {   //内部类也是类，所以说里面也可以有成员变量、方法等，甚至还可以继续套娃一个成员内部类
        public void test(){
            System.out.println("我是成员内部类！");
        }
    }
}
//成员内部类和成员方法、成员变量一样，是对象所有的，而不是类所有的，如果我们要使用成员内部类，那么就需要：
public static void main(String[] args) {
    Test test = new Test();   //我们首先需要创建对象
    Test.Inner inner = test.new Inner();   //成员内部类的类型名称就是 外层.内部类名称
    inner.test();//我们同样可以使用成员内部类中的方法
}
```

成员内部类可以访问到外部的成员变量,   因为成员内部类本身就是某个对象所有的，每个对象都有这样的一个类定义，这里的name是其所依附对象的

```java
//外部能访问内部类里面的成员变量吗？那么如果内部类中也定义了同名的变量，此时我们怎么去明确要使用的是哪一个呢？
public class Test {
    private final String name;

    public Test(String name){
        this.name = name;
    }
    public class Inner {

        String name;
        public void test(String name){
            System.out.println("方法参数的name = "+name);    //依然是就近原则，最近的是参数，那就是参数了
            System.out.println("成员内部类的name = "+this.name);   //在内部类中使用this关键字，只能表示内部类对象
            System.out.println("成员内部类的name = "+Test.this.name);
          	//如果需要指定为外部的对象，那么需要在前面添加外部类型名称
        }
    }
}


public class Inner {
    String name;
    public void test(String name){
        this.toString();		//内部类自己的toString方法
        super.toString();    //内部类父类的toString方法
        Test.this.toString();   //外部类的toSrting方法
        Test.super.toString();  //外部类父类的toString方法
    }
}
```

##### 静态内部类

前面我们介绍了成员内部类，它就像成员变量和成员方法一样，是属于对象的，同样的，静态内部类就像静态方法和静态变量一样，是属于类的，我们可以直接创建使用。

```java
public class Test {
    private final String name;

    public Test(String name){
        this.name = name;
    }

    public static class Inner {
        public void test(){
            System.out.println("我是静态内部类！");
        }
    }
}

//不需要依附任何对象，我们可以直接创建静态内部类的对象：
public static void main(String[] args) {
    Test.Inner inner = new Test.Inner();   //静态内部类的类名同样是之前的格式，但是可以直接new了
  	inner.test();
}
```

静态内部类由于是静态的，所以相对外部来说，整个内部类中都处于静态上下文（注意只是相当于外部来说）是无法访问到外部类的非静态内容的.     只不过受影响的只是外部内容的使用，内部倒是不受影响，还是跟普通的类一样

##### 局部内部类

局部内部类就像局部变量一样，可以在方法中定义

```java
public class Test {
    public void hello(){
        class Inner{   //局部内部类跟局部变量一样，先声明后使用
            public void test(){
                System.out.println("我是局部内部类");
            }
        }
        //既然是在方法中声明的类，那作用范围也就只能在方法中了
        Inner inner = new Inner();   //局部内部类直接使用类名就行
        inner.test();
    }
}
```

##### 匿名内部类

匿名内部类是我们使用频率非常高的一种内部类，它是局部内部类的简化版。

在抽象类和接口中都会含有某些抽象方法需要子类去实现，我们当时已经很明确地说了不能直接通过new的方式去创建一个抽象类或是接口对象，但是我们可以使用匿名内部类。

正常情况下，要创建一个抽象类的实例对象，只能对其进行继承，先实现未实现的方法，然后创建子类对象。

而我们可以在方法中使用匿名内部类，将其中的抽象方法实现，并直接创建实例对象：

```java
public abstract class Student {
    public abstract void test();
}

public static void main(String[] args) {
    Student student = new Student() {   //在new的时候，后面加上花括号，把未实现的方法实现了
        @Override
        public void test() {
            System.out.println("我是匿名内部类的实现!");
        }
    };
    student.test();
}
```

此时这里创建出来的Student对象，就是一个已经实现了抽象方法的对象，这个抽象类直接就定义好了，甚至连名字都没有，就可以直接就创出对象。

匿名内部类中同样可以使用类中的属性（因为它本质上就相当于是对应类型的子类）所以说：

此时这里创建出来的Student对象，就是一个已经实现了抽象方法的对象，这个抽象类直接就定义好了，甚至连名字都没有，就可以直接就创出对象。

匿名内部类中同样可以使用类中的属性（因为它本质上就相当于是对应类型的子类）所以说：


```java
Student student = new Student() {
    int a;   //因为本质上就相当于是子类，所以说子类定义一些子类的属性完全没问题
    
	@Override
	public void test() {
    System.out.println(name + "我是匿名内部类的实现!");   //直接使用父类中的name变量
	}
};    
```
同样的，接口也可以通过这种匿名内部类的形式

当然，并不是说只有抽象类和接口才可以像这样创建匿名内部类，普通的类也可以，只不过意义不大，一般情况下只是为了进行一些额外的初始化工作而已。

##### Lambda表达式

前面我们介绍了匿名内部类，我们可以通过这种方式创建一个临时的实现子类。

特别的，**如果一个接口中有且只有一个待实现的抽象方法**，那么我们可以将匿名内部类简写为Lambda表达式：

```java
public static void main(String[] args) {
    Study study = () -> System.out.println("我是学习方法！");   //是不是感觉非常简洁！
  	study.study();
}
```

我们来看一下Lambda表达式的具体规范：

- 标准格式为：([参数类型 参数名称,]...) ‐> { 代码语句，包括返回值 }
- 和匿名内部类不同，Lambda仅支持接口，不支持抽象类
- 接口内部必须有且仅有一个抽象方法（可以有多个方法，但是必须保证其他方法有默认实现，必须留一个抽象方法出来）

比如我们之前写的Study接口，只要求实现一个无参无返回值的方法，所以说直接就是最简单的形式：

当然，如果有一个参数和返回值的话：

```java
public static void main(String[] args) {
    Study study = (a) -> {
        System.out.println("我是学习方法");
        return "今天学会了"+a;    //实际上这里面就是方法体，该咋写咋写
    };
    System.out.println(study.study(10));
}

//注意，如果方法体中只有一个返回语句，可以直接省去花括号和return关键字：
Study study = (a) -> "今天学会了"+a;

//如果参数只有一个，那么可以省去小括号：
Study study = a -> "今天学会了"+a;
```

##### 方法引用

方法引用就是将一个已实现的方法，直接作为接口中抽象方法的实现（当然前提是方法定义得一样才行）

```java
public interface Study {
    int sum(int a, int b);   //待实现的求和方法
}

//此时，我们可以直接将已有方法的实现作为接口的实现：
public static void main(String[] args) {
    Study study = (a, b) -> Integer.sum(a, b);   //直接使用Integer为我们通过好的求和方法
    System.out.println(study.sum(10, 20));
}

//我们发现，Integer.sum的参数和返回值，跟我们在Study中定义的完全一样，所以说我们可以直接使用方法引用
public static void main(String[] args) {
    Study study = Integer::sum;    //使用双冒号来进行方法引用，静态方法使用 类名::方法名 的形式
    System.out.println(study.sum(10, 20));
}




//如果是普通从成员方法，我们同样需要使用对象来进行方法引用：
public static void main(String[] args) {
    Main main = new Main();
    Study study = main::lbwnb;   //成员方法因为需要具体对象使用，所以说只能使用 对象::方法名 的形式
}

public String lbwnb(){
    return "卡布奇诺今犹在，不见当年倒茶人。";
}

//因为现在只需要一个String类型的返回值，由于String的构造方法在创建对象时也会得到一个String类型的结果，所以说：
public static void main(String[] args) {
    Study study = String::new;    //没错，构造方法也可以被引用，使用new表示
}
//反正只要是符合接口中方法的定义的，都可以直接进行方法引用，对于Lambda表达式和方法引用，在Java新特性介绍篇视频教程中还有详细的讲解，这里就不多说了。
```



方法引用其实本质上就相当于将其他方法的实现，直接作为接口中抽象方法的实现。任何方法都可以通过方法引用作为实现

#### 异常

我们在之前其实已经接触过一些异常了，比如数组越界异常，空指针异常，算术异常等，他们其实都是异常类型，我们的每一个异常也是一个类，他们都继承自Exception类！异常类型本质依然类的对象，但是异常类型支持在程序运行出现问题时抛出（也就是上面出现的红色报错）也可以提前声明，告知使用者需要处理可能会出现的异常！

异常的第一种类型是运行时异常，如上述的列子，在编译阶段无法感知代码是否会出现问题，只有在运行的时候才知道会不会出错（正常情况下是不会出错的），这样的异常称为运行时异常，异常也是由类定义的，所有的运行时异常都继承自RuntimeException。

异常的另一种类型是编译时异常，编译时异常明确指出可能会出现的异常，在编译阶段就需要进行处理（捕获异常）必须要考虑到出现异常的情况，如果不进行处理，将无法通过编译！默认继承自`Exception`类的异常都是编译时异常。



异常其实就两大类，一个是编译时异常，一个是运行时异常，我们先来看编译时异常。

译时异常只需要继承Exception就行了，编译时异常的子类有很多很多，仅仅是SE中就有700多个。

运行时异常只需要继承RuntimeException就行了：

##### 抛出异常

```java
//注意，如果不同的分支条件会出现不同的异常，那么所有在方法中可能会抛出的异常都需要注明：
private static void test(int a) throws FileNotFoundException, ClassNotFoundException {  //多个异常使用逗号隔开
    if(a == 1)
        throw new FileNotFoundException();
    else 
        throw new ClassNotFoundException();
}

//我们在重写方法时，如果父类中的方法表明了会抛出某个异常，只要重写的内容中不会抛出对应的异常我们可以直接省去：
@Override
protected Object clone() {
    return new Object();
}
```

##### 异常处理

```java
public static void main(String[] args) {
    try {
        //我们可以将代码编写到try语句块中，只要是在这个范围内发生的异常，都可以被捕获，使用catch关键字对指定的异常进行捕获，这里我们捕获的是NullPointerException空指针异常：
        Object object = null;
        object.toString();
    } catch (NullPointerException e){
        //注意，catch中捕获的类型只能是Throwable的子类，也就是说要么是抛出的异常，要么是错误，不能是其他的任何类型。我们可以在catch语句块中对捕获到的异常进行处理：
        
        e.printStackTrace();   //打印栈追踪信息
        System.out.println("异常错误信息："+e.getMessage());   //获取异常的错误信息
    }finally {
        //无论是否出现异常，都会在最后执行
  	System.out.println("lbwnb");   //无论是否出现异常，都会在最后执行
}
    System.out.println("程序继续正常运行！");
}
```

##### 断言表达式

我们可以使用断言表达式来对某些东西进行判断，如果判断失败会抛出错误，只不过默认情况下没有开启断言，我们需要在虚拟机参数中手动开启一下：

![image-20220924220327591](https://image.itbaima.cn/markdown/2022/09/24/cAG8kY395fOuTLg.png)

开启断言之后，我们就可以开始使用了。

断言表达式需要使用到`assert`关键字，如果assert后面的表达式判断结果为false，将抛出AssertionError错误。

```java
public static void main(String[] args) {
    assert false;
}

//比如我们可以判断变量的值，如果大于10就抛出错误
public static void main(String[] args) {
    int a = 10;
    assert a > 10 : "我是自定义的错误信息";
    //我们可以在表达式的后面添加错误信息：
}
```

## 中级

### 泛型

JDK 5新增了泛型，它能够在编译阶段就检查类型安全，大大提升开发效率。

#### 泛型类

泛型其实就一个待定类型，我们可以使用一个特殊的名字表示泛型，泛型在定义时并不明确是什么类型，而是需要到使用时才会确定对应的泛型类型。

```java
public class Score<T> {   //泛型类需要使用<>，我们需要在里面添加1 - N个类型变量
    String name;
    String id;
    T value;   //T会根据使用时提供的类型自动变成对应类型

    public Score(String name, String id, T value) {   //这里T可以是任何类型，但是一旦确定，那么就不能修改了
        this.name = name;
        this.id = id;
        this.value = value;
    }
}
```

只不过这里需要注意一下，我们在方法中使用待确定类型的变量时，因为此时并不明确具体是什么类型，那么默认会认为这个变量是一个Object类型的变量，因为无论具体类型是什么，一定是Object类的子类：

因为泛型本身就是对某些待定类型的简单处理，如果都明确要使用什么类型了，那大可不必使用泛型。还有，不能通过这个不确定的类型变量就去直接创建对象和对应的数组：

```java
public class Test<A, B, C> {   //多个类型变量使用逗号隔开
    public A a;
    public B b;
    public C c;
}
```

那么在使用时，就需要将这三种类型都进行明确指定：

```java
public static void main(String[] args) {
    Test<String, Integer, Character> test = new Test<>();  //使用钻石运算符可以省略其中的类型
    test.a = "lbwnb";
    test.b = 10;
    test.c = '淦';
}
```

泛型只能确定为一个引用类型，基本类型是不支持的：如果要存放基本数据类型的值，我们只能使用对应的包装类：当然，如果是基本类型的数组，因为数组本身是引用类型，所以说是可以的，静态内容不能使用类型参数

#### 泛型与多态

不只是类，包括接口、抽象类，都是可以支持泛型的：

```java
public interface Study<T> {
    T test();
}
```

当子类实现此接口时，我们可以选择在实现类明确泛型类型，或是继续使用此泛型让具体创建的对象来确定类型，   或者是继续摆烂，依然使用泛型：继承也是同样的：

#### 泛型方法

当某个方法（无论是是静态方法还是成员方法）需要接受的参数类型并不确定时，我们也可以使用泛型来表示：

```java
public class Main {
    public static void main(String[] args) {
        String str = test("Hello World!");
    }

    private static <T> T test(T t){   //在返回值类型前添加<>并填写泛型变量表示这个是一个泛型方法
        return t;
    }
}
```

泛型方法会在使用时自动确定泛型类型，比如上我们定义的是类型T作为参数，同样的类型T作为返回值，实际传入的参数是一个字符串类型的值，那么T就会自动变成String类型，因此返回值也是String类型。

#### 泛型界限

只需要在泛型变量的后面添加`extends`关键字即可指定上界，使用时，具体类型只能是我们指定的上界类型或是上界类型的子类，不得是其他类型。否则一律报错：

通配符  ? 相当于数学中的无穷区间

`? extends A`  相当于右区间为A，	G<? extends A> 可以作为G< A > 和G< B >的父类，其中B是A的子类，可以接收A及其子类

`? super A`  相当于左区间为A，	G<? super A>可以作为G< A >和G< B >的父类，其中B是A的父类，可以接收A及其父类

类里面的方法只能定义上界`<T extends Object>`

#### 类型擦除

在Java中并不是真的有泛型类型（为了兼容之前的Java版本）因为所有的对象都是属于一个普通的类型，一个泛型类型编译之后，实际上会直接使用默认的类型：

```java
public abstract class A {
    abstract Object test(Object t);  //默认就是Object
}

//当然，如果我们给类型变量设定了上界，那么会从默认类型变成上界定义的类型：
public abstract class A <T extends Number>{   //设定上界为Number
    abstract T test(T t);
}

//那么编译之后：
public abstract class A {
    abstract Number test(Number t);  //上界Number，因为现在只可能出现Number的子类
}

//因此，泛型其实仅仅是在编译阶段进行类型检查，当程序在运行时，并不会真的去检查对应类型，所以说哪怕是我们不去指定类型也可以直接使用：
public static void main(String[] args) {
    Test test = new Test();    //对于泛型类Test，不指定具体类型也是可以的，默认就是原始类型
}
```

由于类型擦除，实际上我们在使用时，编译后的代码是进行了强制类型转换的：

我们思考一个问题，既然继承泛型类之后可以明确具体类型，那么为什么`@Override`不会出现错误呢？我们前面说了，重写的条件是需要和父类的返回值类型和形参一致，而泛型默认的原始类型是Object类型，子类明确后变为其他类型，这显然不满足重写的条件，但是为什么依然能编译通过呢？

我们来看看编译之后长啥样：

```java
// Compiled from "B.java"
public class com.test.entity.B extends com.test.entity.A<java.lang.String> {
  public com.test.entity.B();
  java.lang.String test(java.lang.String);
  java.lang.Object test(java.lang.Object);   //桥接方法，这才是真正重写的方法，但是使用时会调用上面的方法。真正重写的方法，在调用时才会调用上面我们自己重写的方法
}

public class B extends A {    
    public Object test(Object obj) {   //这才是重写的桥接方法
        return this.test((Integer) obj);   //桥接方法调用我们自己写的方法
    }    
    public String test(String str) {   //我们自己写的方法
        return null;
    }
}
```

类型擦除机制其实就是为了方便使用后面集合类（不然每次都要强制类型转换）同时为了向下兼容采取的方案。因此，泛型的使用会有一些限制：

首先，在进行类型判断时，不允许使用泛型，只能使用原始类型：只能判断是不是原始类型，里面的具体类型是不支持的，泛型类型是不支持创建参数化类型数组的：

要用只能用原始类型：

```java
public static void main(String[] args) {
    Test[] test = new Test[10];   //同样是因为类型擦除导致的，运行时可不会去检查具体类型是什么
    
    //只不过只是把它当做泛型类型的数组还是可以用的：
    Test<String>[] test = new Test[10];
    test[1] = new Test<String>();
}
```



#### 函数式接口

学习了泛型，我们来介绍一下再JDK 1.8中新增的函数式接口。

函数式接口就是JDK1.8专门为我们提供好的用于Lambda表达式的接口，这些接口都可以直接使用Lambda表达式，非常方便，这里我们主要介绍一下四个主要的函数式接口：

**Supplier供给型函数式接口：** 这个接口是专门用于供给使用的，其中只有一个get方法用于获取需要的对象。

``` java
@FunctionalInterface   //函数式接口都会打上这样一个注解
public interface Supplier<T> {
    T get();   //实现此方法，实现供给功能
}

//比如我们要实现一个专门供给Student对象Supplier，就可以使用：
public class Student {
    public void hello(){
        System.out.println("我是学生！");
    }
}

//专门供给Student对象的Supplier
private static final Supplier<Student> STUDENT_SUPPLIER = Student::new;//lambda表达式，方法引用
public static void main(String[] args) {
    Student student = STUDENT_SUPPLIER.get();
    student.hello();
}
```

Consumer消费型函数式接口： 这个接口专门用于消费某个对象的。

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);    //这个方法就是用于消费的，没有返回值

    default Consumer<T> andThen(Consumer<? super T> after) {   //这个方法便于我们连续使用此消费接口
        Objects.requireNonNull(after);//判空
        return (T t) -> { accept(t); after.accept(t); };
    }
}

//专门消费Student对象的Consumer
private static final Consumer<Student> STUDENT_CONSUMER = student -> System.out.println(student+" 真好吃！");
public static void main(String[] args) {
    Student student = new Student();
    STUDENT_CONSUMER.accept(student);
}

//当然，我们也可以使用andThen方法继续调用：
public static void main(String[] args) {
    Student student = new Student();
    STUDENT_CONSUMER   //我们可以提前将消费之后的操作以同样的方式预定好
            .andThen(stu -> System.out.println("我是吃完之后的操作！")) 
            .andThen(stu -> System.out.println("好了好了，吃饱了！"))
            .accept(student);   //预定好之后，再执行
}
```
Function函数型函数式接口： 这个接口消费一个对象，然后会向外供给一个对象（前两个的融合体）

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);   //这里一共有两个类型参数，其中一个是接受的参数类型，还有一个是返回的结果类型

    default <V> Function<V, R> compose(Function<? super V, ? extends T> before) {
        Objects.requireNonNull(before);
        return (V v) -> apply(before.apply(v));
    }

    default <V> Function<T, V> andThen(Function<? super R, ? extends V> after) {
        Objects.requireNonNull(after);
        return (T t) -> after.apply(apply(t));
    }

    static <T> Function<T, T> identity() {
        return t -> t;
    }
}

//这里实现了一个简单的功能，将传入的int参数转换为字符串的形式
private static final Function<Integer, String> INTEGER_STRING_FUNCTION = Object::toString;
public static void main(String[] args) {
    String str = INTEGER_STRING_FUNCTION.apply(10);
    System.out.println(str);
}

public static void main(String[] args) {
    String str = INTEGER_STRING_FUNCTION
            .compose((String s) -> s.length())   //将此函数式的返回值作为当前实现的实参
            .apply("lbwnb");   //传入上面函数式需要的参数
    System.out.println(str);
}

public static void main(String[] args) {
    Function<String, String> function = Function.identity();   //原样返回
    System.out.println(function.apply("不会吧不会吧，不会有人听到现在还是懵逼的吧"));
}
```



Predicate断言型函数式接口： 接收一个参数，然后进行自定义判断并返回一个boolean结果。

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);    //这个方法就是我们要实现的

    default Predicate<T> and(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) && other.test(t);
    }

    default Predicate<T> negate() {
        return (t) -> !test(t);
    }

    default Predicate<T> or(Predicate<? super T> other) {
        Objects.requireNonNull(other);
        return (t) -> test(t) || other.test(t);
    }

    static <T> Predicate<T> isEqual(Object targetRef) {
        return (null == targetRef)
                ? Objects::isNull
                : object -> targetRef.equals(object);
    }
}
```

#### 判空包装

Java8还新增了一个非常重要的判空包装类Optional，这个类可以很有效的处理空指针问题。

在Java8之后，有了Optional类，它可以更加优雅地处理这种问题，我们来看看如何使用：

```java
private static void test(String str){
    Optional
            .ofNullable(str)   //将传入的对象包装进Optional中
            .ifPresent(s -> System.out.println("字符串长度为："+s.length()));  
  					//如果不为空，则执行这里的Consumer实现
}

//我们可以对于这种有可能为空的情况进行处理，如果为空，那么就返回另一个备选方案：
private static void test(String str){
    String s = Optional.ofNullable(str).orElse("我是为null的情况备选方案");
    System.out.println(s);
}

//是不是感觉很方便？我们还可以将包装的类型直接转换为另一种类型：
private static void test(String str){
    Integer i = Optional
            .ofNullable(str)
            .map(String::length)   //使用map来进行映射，将当前类型转换为其他类型，或者是进行处理
            .orElse(-1);
    System.out.println(i);
}
```



### 集合

集合跟数组一样，可以表示同样的一组元素，但是他们的相同和不同之处在于：

1. 它们都是容器，都能够容纳一组元素。

不同之处：

1. 数组的大小是固定的，集合的大小是可变的。
2. 数组可以存放基本数据类型，但集合只能存放对象。
3. 数组存放的类型只能是一种，但集合可以有不同种类的元素

equals返回true和hashcode相等认为同一个元素



1. Java集合可分为Collection和Map两种体系

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





#### 集合根接口

```java
public interface Collection<E> extends Iterable<E> {
    //-------这些是查询相关的操作----------
   	//获取当前集合中的元素数量
    int size();

    //查看当前集合是否为空
    boolean isEmpty();

    //查询当前集合中是否包含某个元素
    boolean contains(Object o);

    //返回当前集合的迭代器，我们会在后面介绍
    Iterator<E> iterator();

    //将集合转换为数组的形式
    Object[] toArray();

    //支持泛型的数组转换，同上
    <T> T[] toArray(T[] a);

    //-------这些是修改相关的操作----------

    //向集合中添加元素，不同的集合类具体实现可能会对插入的元素有要求，
  	//这个操作并不是一定会添加成功，所以添加成功返回true，否则返回false
    boolean add(E e);

    //从集合中移除某个元素，同样的，移除成功返回true，否则false
    boolean remove(Object o);


    //-------这些是批量执行的操作----------

    //查询当前集合是否包含给定集合中所有的元素
  	//从数学角度来说，就是看给定集合是不是当前集合的子集
    boolean containsAll(Collection<?> c);

    //添加给定集合中所有的元素
  	//从数学角度来说，就是将当前集合变成当前集合与给定集合的并集
  	//添加成功返回true，否则返回false
    boolean addAll(Collection<? extends E> c);

    //移除给定集合中出现的所有元素，如果某个元素在当前集合中不存在，那么忽略这个元素
  	//从数学角度来说，就是求当前集合与给定集合的差集
  	//移除成功返回true，否则false
    boolean removeAll(Collection<?> c);

    //Java8新增方法，根据给定的Predicate条件进行元素移除操作
    default boolean removeIf(Predicate<? super E> filter) {
        Objects.requireNonNull(filter);
        boolean removed = false;
        final Iterator<E> each = iterator();   //这里用到了迭代器，我们会在后面进行介绍
        while (each.hasNext()) {
            if (filter.test(each.next())) {
                each.remove();
                removed = true;
            }
        }
        return removed;
    }

    //只保留当前集合中在给定集合中出现的元素，其他元素一律移除
  	//从数学角度来说，就是求当前集合与给定集合的交集
  	//移除成功返回true，否则false
    boolean retainAll(Collection<?> c);

    //清空整个集合，删除所有元素
    void clear();


    //-------这些是比较以及哈希计算相关的操作----------

    //判断两个集合是否相等
    boolean equals(Object o);

    //计算当前整个集合对象的哈希值
    int hashCode();

    //与迭代器作用相同，但是是并行执行的，我们会在下一章多线程部分中进行介绍
    @Override
    default Spliterator<E> spliterator() {
        return Spliterators.spliterator(this, 0);
    }

    //生成当前集合的流，我们会在后面进行讲解
    default Stream<E> stream() {
        return StreamSupport.stream(spliterator(), false);
    }

    //生成当前集合的并行流，我们会在下一章多线程部分中进行介绍
    default Stream<E> parallelStream() {
        return StreamSupport.stream(spliterator(), true);
    }
}
```

#### LIst列表

首先我们需要介绍的是List列表（线性表），线性表支持随机访问，相比之前的Collection接口定义，功能还会更多一些。首先介绍ArrayList，我们已经知道，它的底层是用数组实现的，内部维护的是一个可动态进行扩容的数组，也就是我们之前所说的顺序表，跟我们之前自己写的ArrayList相比，它更加的规范，并且功能更加强大，同时实现自List接口。

List是集合类型的一个分支，它的主要特性有：

是一个有序的集合，插入元素默认是插入到尾部，按顺序从前往后存放，每个元素都有一个自己的下标位置
列表中允许存在重复元素

在List接口中，定义了列表类型需要支持的全部操作，List直接继承自前面介绍的Collection接口，其中很多地方重新定义了一次Collection接口中定义的方法，这样做是为了更加明确方法的具体功能，当然，为了直观，我们这里就省略掉：

一般的，如果我们要使用一个集合类，我们会使用接口的引用：

```java
public static void main(String[] args) {
    List<String> list = new ArrayList<>();   //使用接口的引用来操作具体的集合类实现，是为了方便日后如果我们想要更换不同的集合类实现，而且接口中本身就已经定义了主要的方法，所以说没必要直接用实现类
    list.add("科技与狠活");   //使用add添加元素
  	list.add("上头啊");
    System.out.println(list);   //打印集合类，可以得到一个非常规范的结果
}

//集合的各种功能我们都可以来测试一下，特别注意一下，我们在使用Integer时，要注意传参问题：
public static void main(String[] args) {
    List<Integer> list = new ArrayList<>();
    list.add(10);   //添加Integer的值10
    list.remove((Integer) 10);   //注意，不能直接用10，默认情况下会认为传入的是int类型值，删除的是下标为10的元素，我们这里要删除的是刚刚传入的值为10的Integer对象
    System.out.println(list);   //可以看到，此时元素成功被移除
}

public static void main(String[] args) {
    List<Integer> list = new ArrayList<>();
    list.add(new Integer(10));   //添加的是一个对象
    list.remove(new Integer(10));   //删除的是另一个对象
    System.out.println(list);
}
//可以看到，结果依然是删除成功，这是因为集合类在删除元素时，只会调用equals方法进行判断是否为指定元素，而不是进行等号判断，所以说一定要注意，如果两个对象使用equals方法相等，那么集合中就是相同的两个对象：


//在Arrays工具类中，我们可以快速生成一个只读的List：
public static void main(String[] args) {
    List<String> list = Arrays.asList("A", "B", "C");   //非常方便
    System.out.println(list);
}

//注意，这个生成的List是只读的，不能进行修改操作，只能使用获取内容相关的方法，否则抛出 UnsupportedOperationException 异常。要生成正常使用的，我们可以将这个只读的列表作为参数传入：
public static void main(String[] args) {
    List<String> list = new ArrayList<>(Arrays.asList("A", "B", "C"));
    System.out.println(list);
}

//当然，也可以利用静态代码块：
public static void main(String[] args) {
    List<String> list = new ArrayList<String>() {{   //使用匿名内部类（匿名内部类在Java8无法使用钻石运算符，但是之后的版本可以）
            add("A");
            add("B");
            add("C");
    }};
    System.out.println(list);
}
```

LinkedList同样是List的实现类，只不过它是采用的链式实现，也就是我们之前讲解的链表，只不过它是一个双向链表，也就是同时保存两个方向：

LinkedList的使用和ArrayList的使用几乎相同，各项操作的结果也是一样的，在什么使用使用ArrayList和LinkedList，我们需要结合具体的场景来决定，尽可能的扬长避短。

只不过LinkedList不仅可以当做List来使用，也可以当做双端队列使用，我们会在后面进行详细介绍

#### 迭代器

我们接着来介绍迭代器，实际上我们的集合类都是支持使用`foreach`语法的

```java
public static void main(String[] args) {
    List<String> list = Arrays.asList("A", "B", "C");
    for (String s : list) {   //集合类同样支持这种语法
        System.out.println(s);
    }
}

public static void main(String[] args) {
    List<String> list = Arrays.asList("A", "B", "C");
  	//通过调用iterator方法快速获取当前集合的迭代器
  	//Iterator迭代器本身也是一个接口，由具体的集合实现类来根据情况实现
    Iterator<String> iterator = list.iterator();
}

//我们想要遍历一个集合中所有的元素，那么就可以直接使用迭代器来完成，而不需要关心集合类是如何实现，我们该怎么去遍历：
public static void main(String[] args) {
    List<String> list = Arrays.asList("A", "B", "C");
    Iterator<String> iterator = list.iterator();
    while (iterator.hasNext()) {    //每次循环一定要判断是否还有元素剩余
        System.out.println(iterator.next());  //如果有就可以继续获取到下一个元素

        
//在Java8提供了一个支持Lambda表达式的forEach方法，这个方法接受一个Consumer，也就是对遍历的每一个元素进行的操作：
public static void main(String[] args) {
    List<String> list = Arrays.asList("A", "B", "C");
    list.forEach(System.out::println);
}
```





#### Queue和Dueue

```java
public interface Queue<E> extends Collection<E> {
    //队列的添加操作，是在队尾进行插入（只不过List也是一样的，默认都是尾插）
  	//如果插入失败，会直接抛出异常
    boolean add(E e);

    //同样是添加操作，但是插入失败不会抛出异常
    boolean offer(E e);

    //移除队首元素，但是如果队列已经为空，那么会抛出异常
    E remove();

   	//同样是移除队首元素，但是如果队列为空，会返回null
    E poll();

    //仅获取队首元素，不进行出队操作，但是如果队列已经为空，那么会抛出异常
    E element();

    //同样是仅获取队首元素，但是如果队列为空，会返回null
    E peek();
}

//我们可以直接将一个LinkedList当做一个队列来使用：
public static void main(String[] args) {
    Queue<String> queue = new LinkedList<>();   //当做队列使用，还是很方便的
    queue.offer("AAA");
    queue.offer("BBB");
    System.out.println(queue.poll());
    System.out.println(queue.poll());
}

```

![image-20220725103600318](https://image.itbaima.cn/markdown/2022/07/25/xBuZckTNtR54AEq.png)

普通队列中从队尾入队，队首出队，而双端队列允许在队列的两端进行入队和出队操作：

```java
//在双端队列中，所有的操作都有分别对应队首和队尾的
public interface Deque<E> extends Queue<E> {
    //在队首进行插入操作
    void addFirst(E e);

    //在队尾进行插入操作
    void addLast(E e);
		
  	//不用多说了吧？
    boolean offerFirst(E e);
    boolean offerLast(E e);

    //在队首进行移除操作
    E removeFirst();

    //在队尾进行移除操作
    E removeLast();

    //不用多说了吧？
    E pollFirst();
    E pollLast();

    //获取队首元素
    E getFirst();

    //获取队尾元素
    E getLast();

		//不用多说了吧？
    E peekFirst();
    E peekLast();

    //从队列中删除第一个出现的指定元素
    boolean removeFirstOccurrence(Object o);

    //从队列中删除最后一个出现的指定元素
    boolean removeLastOccurrence(Object o);

    // *** 队列中继承下来的方法操作是一样的，这里就不列出了 ***

    ...

    // *** 栈相关操作已经帮助我们定义好了 ***

    //将元素推向栈顶
    void push(E e);

    //将元素从栈顶出栈
    E pop();


    // *** 集合类中继承的方法这里也不多种介绍了 ***

    ...

    //生成反向迭代器，这个迭代器也是单向的，但是是next方法是从后往前进行遍历的
    Iterator<E> descendingIterator();
}

//我们来测试一下反向迭代器和正向迭代器：
public static void main(String[] args) {
    Deque<String> deque = new LinkedList<>();
    deque.addLast("AAA");
    deque.addLast("BBB");
    
    Iterator<String> descendingIterator = deque.descendingIterator();
    System.out.println(descendingIterator.next());

    Iterator<String> iterator = deque.iterator();
    System.out.println(iterator.next());
}
//可以看到，正向迭代器和反向迭代器的方向是完全相反的。

//这里需要介绍一下优先级队列，优先级队列可以根据每一个元素的优先级，对出队顺序进行调整，默认情况按照自然顺序，并没有对元素排序，只是对出队顺序调整
public static void main(String[] args) {
    Queue<Integer> queue = new PriorityQueue<>();
    queue.offer(10);
    queue.offer(4);
    queue.offer(5);
    System.out.println(queue.poll());
    System.out.println(queue.poll());
    System.out.println(queue.poll());
}
```



#### Set集合

我们发现接口中定义的方法都是Collection中直接继承的，因此，Set支持的功能其实也就和Collection中定义的差不多，只不过：

- 不允许出现重复元素
- 不支持随机访问，不允许通过下标访问

首先认识一下HashSet，它的底层就是采用哈希表实现的，底层实质上是借用的一个HashMap在实现，我们可以非常高效的从HashSet中存取元素，在Set接口中并没有定义支持指定下标位置访问的添加和删除操作，我们只能简单的删除Set中的某个对象，由于底层采用哈希表实现，所以说无法维持插入元素的顺序：

那要是我们就是想要使用维持顺序的Set集合呢？我们可以使用LinkedHashSet，LinkedHashSet底层维护的不再是一个HashMap，而是LinkedHashMap，它能够在插入数据时利用链表自动维护顺序，因此这样就能够保证我们插入顺序和最后的迭代顺序一致了。

还有一种Set叫做TreeSet，它会在元素插入时进行排序

```java
//当然，我们也可以自定义排序规则：
public static void main(String[] args) {
    TreeSet<Integer> set = new TreeSet<>((a, b) -> b - a);  //同样是一个Comparator
    set.add(1);
    set.add(3);
    set.add(2);
    System.out.println(set);
}
```

#### Map映射

在Map中，这些映射关系被存储为键值对

```java
//这里我们使用最常见的HashMap，它的底层采用哈希表实现：
public static void main(String[] args) {
    Map<Integer, String> map = new HashMap<>();
    map.put(1, "小明");   //使用put方法添加键值对，返回值我们会在后面讨论
    map.put(2, "小红");
    System.out.println(map.get(2)); //使用get方法根据键获取对应的值
}
```

注意，Map中无法添加相同的键，同样的键只能存在一个，即使值不同。如果出现键相同的情况，那么会覆盖掉之前的：

```java
//为了防止意外将之前的键值对覆盖掉，我们可以使用：
public static void main(String[] args) {
    Map<Integer, String> map = new HashMap<>();
    map.put(1, "小明");
    map.putIfAbsent(1, "小红");   //Java8新增操作，只有在不存在相同键的键值对时才会存放
    System.out.println(map.get(1));
    
    System.out.println(map.getOrDefault(3, "备胎"));   //Java8新增操作，当不存在对应的键值对时，返回备选方案
}

```

因为HashMap底层采用哈希表实现，所以不维护顺序，我们在获取所有键和所有值时，可能会是乱序的：如果需要维护顺序，我们同样可以使用LinkedHashMap，它的内部对插入顺序进行了维护：

我们接着来看看Map的底层是如何实现的，首先是最简单的HashMap，我们前面已经说过了，它的底层采用的是哈希表，首先回顾我们之前学习的哈希表，我们当时说了，哈希表可能会出现哈希冲突，这样保存的元素数量就会存在限制，而我们可以通过连地址法解决这种问题，最后哈希表就长这样了：
![image-20240306224737691](C:\Users\nxbhx\AppData\Roaming\Typora\typora-user-images\image-20240306224737691.png)

实际上这个表就是一个存放头结点的数组+若干结点，而HashMap也是这样的

可以看到，实际上底层大致结构跟我们之前学习的差不多，只不过多了一些特殊的东西：

- HashMap支持自动扩容，哈希表的大小并不是一直不变的，否则太过死板
- HashMap并不是只使用简单的链地址法，当链表长度到达一定限制时，会转变为效率更高的红黑树结构

而LinkedHashMap是直接继承自HashMap，具有HashMap的全部性质，同时得益于每一个节点都是一个双向链表，在插入键值对时，同时保存了插入顺序

```java
//当然还有一种比较特殊的Map叫做TreeMap，就像它的名字一样，就是一个Tree，它的内部直接维护了一个红黑树（没有使用哈希表）因为它会将我们插入的结点按照规则进行排序，所以说直接采用红黑树会更好，我们在创建时，直接给予一个比较规则即可，跟之前的TreeSet是一样的：
public static void main(String[] args) {
    Map<Integer , String> map = new TreeMap<>((a, b) -> b - a);
    map.put(0, "单走");
    map.put(1, "一个六");
    map.put(3, "**");
    System.out.println(map);
}
```



#### Stream流

Java 8 API添加了一个新的抽象称为流Stream，可以让你以一种声明的方式处理数据。Stream 使用一种类似用 SQL 语句从数据库查询数据的直观方式来提供一种对 Java 集合运算和表达的高阶抽象。Stream API可以极大提高Java程序员的生产力，让程序员写出高效率、干净、简洁的代码。这种风格将要处理的元素集合看作一种流， 流在管道中传输， 并且可以在管道的节点上进行处理， 比如筛选， 排序，聚合等。元素流在管道中经过中间操作（intermediate operation）的处理，最后由最终操作(terminal operation)得到前面处理的结果。



```java
//它看起来就像一个工厂的流水线一样！我们就可以把一个Stream当做流水线处理
public static void main(String[] args) {
    List<String> list = new ArrayList<>();
    list.add("A");
    list.add("B");
    list.add("C");
  
  	//移除为B的元素
  	Iterator<String> iterator = list.iterator();
    while (iterator.hasNext()){
        if(iterator.next().equals("B")) iterator.remove();
    }
  
  	//Stream操作
    list = list     //链式调用
            .stream()    //获取流
            .filter(e -> !e.equals("B"))   //只允许所有不是B的元素通过流水线
            .collect(Collectors.toList());   //将流水线中的元素重新收集起来，变回List
    System.out.println(list);
}
```

**注意**：不能认为每一步是直接依次执行的！实际上，stream会先记录每一步操作，而不是直接开始执行内容，当整个链式调用完成后，才会依次进行，也就是说需要的时候，工厂的机器才会按照预定的流程启动。

```java
//我们还可以通过flat来对整个流进行进一步细分：
public static void main(String[] args) {
    List<String> list = new ArrayList<>();
    list.add("A,B");
    list.add("C,D");
    list.add("E,F");   //我们想让每一个元素通过,进行分割，变成独立的6个元素
    list = list
            .stream()    //生成流
            .flatMap(e -> Arrays.stream(e.split(",")))    //分割字符串并生成新的流
            .collect(Collectors.toList());   //汇成新的List
    System.out.println(list);   //得到结果
}
```

#### 集合类对象相等的判定

 "=="比较基本数据类型时比较的是表面值内容，而比较两个对象时比较的是两个对象的内存地址值

对于equals方法，注意：equals方法不能作用于基本数据类型的变量

如果没有对equals方法进行重写，则比较的是引用类型的变量所指向的对象的地址；

诸如String、Date等类对equals方法进行了重写的话，比较的是所指向的对象的内容

在集合类中，对equals重写了判断值内容，在map中还要比较哈希值，所以map中重写equals时也要重写hashcode

## 高级

### I/O流

JDK提供了一套用于IO操作的框架，为了方便我们开发者使用，就定义了一个像水流一样，根据流的传输方向和读取单位，分为字节流InputStream和OutputStream以及字符流Reader和Writer的IO框架，当然，这里的Stream并不是前面集合框架认识的Stream，这里的流指的是数据流，通过流，我们就可以一直从流中读取数据，直到读取到尽头，或是不断向其中写入数据，直到我们写入完成，而这类IO就是我们所说的BIO，

字节流一次读取一个字节，也就是一个byte的大小，而字符流顾名思义，就是一次读取一个字符，也就是一个char的大小（在读取纯文本文件的时候更加适合），有关这两种流，会在后面详细介绍，这个章节我们需要学习16个关键的流。

#### 文件字节流

首先介绍一下FileInputStream，我们可以通过它来获取文件的输入流：

```java
public static void main(String[] args) {
    try {   //注意，IO相关操作会有很多影响因素，有可能出现异常，所以需要明确进行处理
        FileInputStream inputStream = new FileInputStream("路径");
        //路径支持相对路径和绝对路径
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    }
}

相对路径是在当前运行目录（就是你在哪个目录运行java命令启动Java程序的）的路径下寻找文件，而绝对路径，是从根目录开始寻找。路径分割符支持使用/或是\\，但是不能写为\因为它是转义字符！比如在Windows下：
C://User/lbw/nb    这个就是一个绝对路径，因为是从盘符开始的
test/test          这个就是一个相对路径，因为并不是从盘符开始的，而是一个直接的路径

在Linux和MacOS下：
/root/tmp       这个就是一个绝对路径，绝对路径以/开头
test/test       这个就是一个相对路径，不是以/开头的
    
    
    
//在使用完成一个流之后，必须关闭这个流来完成对资源的释放，否则资源会被一直占用：
public static void main(String[] args) {
    FileInputStream inputStream = null;    //定义可以先放在try外部
    try {
        inputStream = new FileInputStream("路径");
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } finally {
        try {    //建议在finally中进行，因为关闭流是任何情况都必须要执行的！
            if(inputStream != null) inputStream.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

//虽然这样的写法才是最保险的，但是显得过于繁琐了，尤其是finally中再次嵌套了一个try-catch块，因此在JDK1.7新增了try-with-resource语法，用于简化这样的写法（本质上还是和这样的操作一致，只是换了个写法）
public static void main(String[] args) {

    //注意，这种语法只支持实现了AutoCloseable接口的类！
    try(FileInputStream inputStream = new FileInputStream("路径")) {   //直接在try()中定义要在完成之后释放的资源

    } catch (IOException e) {   //这里变成IOException是因为调用close()可能会出现，而FileNotFoundException是继承自IOException的
        e.printStackTrace();
    }
    //无需再编写finally语句块，因为在最后自动帮我们调用了close()
}



public static void main(String[] args) {
    //test.txt：a
    try(FileInputStream inputStream = new FileInputStream("test.txt")) {
        //使用read()方法进行字符读取
        System.out.println((char) inputStream.read());  //读取一个字节的数据（英文字母只占1字节，中文占2字节）
        System.out.println(inputStream.read());   //唯一一个字节的内容已经读完了，再次读取返回-1表示没有内容了
    }catch (IOException e){
        e.printStackTrace();
    }
}

//使用read可以直接读取一个字节的数据，注意，流的内容是有限的，读取一个少一个。我们如果想一次性全部读取的话，可以直接使用一个while循环来完成：
public static void main(String[] args) {
    //test.txt：abcd
    try(FileInputStream inputStream = new FileInputStream("test.txt")) {
        int tmp;
        while ((tmp = inputStream.read()) != -1){   //通过while循环来一次性读完内容
            System.out.println((char)tmp);
        }
    }catch (IOException e){
        e.printStackTrace();
    }
}
```

使用`available`方法能查看当前可读的剩余字节数量（注意：并不一定真实的数据量就是这么多，尤其是在网络I/O操作时，这个方法只能进行一个预估也可以说是暂时能一次性可以读取的数量，当然在磁盘IO下，一般情况都是真实的数据量）

```java
//当然，一个一个读取效率太低了，那能否一次性全部读取呢？我们可以预置一个合适容量的byte[]数组来存放：
public static void main(String[] args) {
    //test.txt：abcd
    try(FileInputStream inputStream = new FileInputStream("test.txt")) {
        byte[] bytes = new byte[inputStream.available()];   //我们可以提前准备好合适容量的byte数组来存放
        System.out.println(inputStream.read(bytes));   //一次性读取全部内容（返回值是读取的字节数）
        System.out.println(new String(bytes));   //通过String(byte[])构造方法得到字符串
    }catch (IOException e){
        e.printStackTrace();
    }
}

//也可以控制要读取数量：
System.out.println(inputStream.read(bytes, 1, 2));   //第二个参数是从给定数组的哪个位置开始放入内容，第三个参数是读取流中的字节数

//通过skip()方法可以跳过指定数量的字节：
public static void main(String[] args) {
    //test.txt：abcd
    try(FileInputStream inputStream = new FileInputStream("test.txt")) {
        System.out.println(inputStream.skip(1));
        System.out.println((char) inputStream.read());   //跳过了一个字节
    }catch (IOException e){
        e.printStackTrace();
    }
}

//注意：FileInputStream是不支持reset()的，虽然有这个方法，但是这里先不提及。



public static void main(String[] args) {
    try(FileOutputStream outputStream = new FileOutputStream("output.txt")) {
        //注意：若此文件不存在，会直接创建这个文件！
        outputStream.write('c');   //同read一样，可以直接写入内容
      	outputStream.write("lbwnb".getBytes());   //也可以直接写入byte[]
      	outputStream.write("lbwnb".getBytes(), 0, 1);  //同上输入流
      	outputStream.flush();  //建议在最后执行一次刷新操作（强制写入）来保证数据正确写入到硬盘文件中
    }catch (IOException e){
        e.printStackTrace();
    }
}

//那么如果是我只想在文件尾部进行追加写入数据呢？我们可以调用另一个构造方法来实现：
public static void main(String[] args) {
    try(FileOutputStream outputStream = new FileOutputStream("output.txt", true)) {  //true表示开启追加模式
        outputStream.write("lb".getBytes());   //现在只会进行追加写入，而不是直接替换原文件内容
        outputStream.flush();
    }catch (IOException e){
        e.printStackTrace();
    }
}

//利用输入流和输出流，就可以轻松实现文件的拷贝了：
public static void main(String[] args) {
    try(FileOutputStream outputStream = new FileOutputStream("output.txt");
        FileInputStream inputStream = new FileInputStream("test.txt")) {   //可以写入多个
        byte[] bytes = new byte[10];    //使用长度为10的byte[]做传输媒介
        int tmp;   //存储本地读取字节数
        while ((tmp = inputStream.read(bytes)) != -1){   //直到读取完成为止
            outputStream.write(bytes, 0, tmp);    //写入对应长度的数据到输出流,0为bytes里的开始，tmp为bytes中读取的长度
        }
    }catch (IOException e){
        e.printStackTrace();
    }
}
```



#### 文件字符流

字符流不同于字节，字符流是以一个具体的字符进行读取，因此它只适合读纯文本的文件，如果是其他类型的文件不适用：

```java
public static void main(String[] args) {
    try(FileReader reader = new FileReader("test.txt")){
      	reader.skip(1);   //现在跳过的是一个字符
        System.out.println((char) reader.read());   //现在是按字符进行读取，而不是字节，因此可以直接读取到中文字符
    }catch (IOException e){
        e.printStackTrace();
    }
}

//同理，字符流只支持char[]类型作为存储：
public static void main(String[] args) {
    try(FileReader reader = new FileReader("test.txt")){
        char[] str = new char[10];
        reader.read(str);
        System.out.println(str);   //直接读取到char[]中
    }catch (IOException e){
        e.printStackTrace();
    }
}

//既然有了Reader肯定也有Writer：
public static void main(String[] args) {
    try(FileWriter writer = new FileWriter("output.txt")){
      	writer.getEncoding();   //支持获取编码（不同的文本文件可能会有不同的编码类型）
       writer.write('牛');
       writer.append('牛');   //其实功能和write一样
      	writer.flush();   //刷新
    }catch (IOException e){
        e.printStackTrace();
    }
}

//我们发现不仅有write()方法，还有一个append()方法，但是实际上他们效果是一样的，看源码：
public Writer append(char c) throws IOException {
    write(c);
    return this;
}
//append支持像StringBuilder那样的链式调用，返回的是Writer对象本身。

//这里需要额外介绍一下File类，它是专门用于表示一个文件或文件夹，只不过它只是代表这个文件，但并不是这个文件本身。通过File对象，可以更好地管理和操作硬盘上的文件。
public static void main(String[] args) {
    File file = new File("test.txt");   //直接创建文件对象，可以是相对路径，也可以是绝对路径
    System.out.println(file.exists());   //此文件是否存在
    System.out.println(file.length());   //获取文件的大小
    System.out.println(file.isDirectory());   //是否为一个文件夹
    System.out.println(file.canRead());   //是否可读
    System.out.println(file.canWrite());   //是否可写
    System.out.println(file.canExecute());   //是否可执行
}

//通过File对象，我们就能快速得到文件的所有信息，如果是文件夹，还可以获取文件夹内部的文件列表等内容：
File file = new File("/");
System.out.println(Arrays.toString(file.list()));   //快速获取文件夹下的文件名称列表
for (File f : file.listFiles()){   //所有子文件的File对象
    System.out.println(f.getAbsolutePath());   //获取文件的绝对路径
}

//如果我们希望读取某个文件的内容，可以直接将File作为参数传入字节流或是字符流：

```



#### 缓冲流

虽然普通的文件流读取文件数据非常便捷，但是每次都需要从外部I/O设备去获取数据，由于外部I/O设备的速度一般都达不到内存的读取速度，很有可能造成程序反应迟钝，因此性能还不够高，而缓冲流正如其名称一样，它能够提供一个缓冲，提前将部分内容存入内存（缓冲区）在下次读取时，如果缓冲区中存在此数据，则无需再去请求外部设备。同理，当向外部设备写入数据时，也是由缓冲区处理，而不是直接向外部设备写入。

要创建一个缓冲字节流，只需要将原本的流作为构造参数传入BufferedInputStream即可：

```java
public static void main(String[] args) {
    try (BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream("test.txt"))){   //传入FileInputStream
        System.out.println((char) bufferedInputStream.read());   //操作和原来的流是一样的
    }catch (IOException e){
        e.printStackTrace();
    }
}

//实际上进行I/O操作的并不是BufferedInputStream，而是我们传入的FileInputStream，而BufferedInputStream虽然有着同样的方法，但是进行了一些额外的处理然后再调用FileInputStream的同名方法，这样的写法称为装饰者模式，我们会在设计模式篇中详细介绍。我们可以来观察一下它的close方法源码：
public void close() throws IOException {
    byte[] buffer;
    while ( (buffer = buf) != null) {
        if (bufUpdater.compareAndSet(this, buffer, null)) {  //CAS无锁算法，并发会用到，暂时不需要了解
            InputStream input = in;
            in = null;
            if (input != null)
                input.close();
            return;
        }
        // Else retry in case a new buf was CASed in fill()
    }
}
```

I/O操作一般不能重复读取内容（比如键盘发送的信号，主机接收了就没了），而缓冲流提供了缓冲机制，一部分内容可以被暂时保存，BufferedInputStream支持`reset()`和`mark()`操作，首先我们来看看`mark()`方法的介绍：当调用`mark()`之后，输入流会以某种方式保留之后读取的`readlimit`数量的内容，当读取的内容数量超过`readlimit`则之后的内容不会被保留，当调用`reset()`之后，会使得当前的读取位置回到`mark()`调用时的位置。我们发现虽然后面的部分没有保存，但是依然能够正常读取，其实`mark()`后保存的读取内容是取`readlimit`和BufferedInputStream类的缓冲区大小两者中的最大值，而并非完全由`readlimit`确定。因此我们限制一下缓冲区大小，再来观察一下结果：

了解完了BufferedInputStream之后，我们再来看看BufferedOutputStream，其实和BufferedInputStream原理差不多，只是反向操作：

既然有缓冲字节流，那么肯定也有缓冲字符流，缓冲字符流和缓冲字节流一样，也有一个专门的缓冲区，BufferedReader构造时需要传入一个Reader对象：

```java
public static void main(String[] args) {
    try (BufferedOutputStream outputStream = new BufferedOutputStream(new FileOutputStream("output.txt"))){
        outputStream.write("lbwnb".getBytes());
        outputStream.flush();
    }catch (IOException e) {
        e.printStackTrace();
    }
}

//相比Reader更方便的是，它支持按行读取：
public static void main(String[] args) {
    try (BufferedReader reader = new BufferedReader(new FileReader("test.txt"))){
        System.out.println(reader.readLine());   //按行读取
    }catch (IOException e) {
        e.printStackTrace();
    }
}

//读取后直接得到一个字符串，当然，它还能把每一行内容依次转换为集合类提到的Stream流：
public static void main(String[] args) {
    try (BufferedReader reader = new BufferedReader(new FileReader("test.txt"))){
        reader
                .lines()
                .limit(2)
                .distinct()
                .sorted()
                .forEach(System.out::println);
    }catch (IOException e) {
        e.printStackTrace();
    }
}
//它同样也支持mark()和reset()操作：
//BufferedReader处理纯文本文件时就更加方便了，BufferedWriter在处理时也同样方便：
public static void main(String[] args) {
    try (BufferedWriter reader = new BufferedWriter(new FileWriter("output.txt"))){
        reader.newLine();   //使用newLine进行换行
        reader.write("汉堡做滴彳亍不彳亍");   //可以直接写入一个字符串
      	reader.flush();   //清空缓冲区
    }catch (IOException e) {
        e.printStackTrace();
    }
}

```



#### 转换流

1. 转换流：属于字符流

   InputStreamReader：将一个字节的输入睿转换为字符的输入流

   OutputStreamWriter：将一个字符的输出流转换为字节的输出流

2. 作用：提供字节流与字符流之间的转换

3. 解码：字节，字节数组 ---> 字符数组，字符串

   编码：字符数组，字符串 ---> 字节，字节数组

有时会遇到这样一个很麻烦的问题：我这里读取的是一个字符串或是一个个字符，但是我只能往一个OutputStream里输出，但是OutputStream又只支持byte类型，如果要往里面写入内容，进行数据转换就会很麻烦，那么能否有更加简便的方式来做这样的事情呢？

```java
public static void main(String[] args) {
    try(OutputStreamWriter writer = new OutputStreamWriter(new FileOutputStream("test.txt"))){  //虽然给定的是FileOutputStream，但是现在支持以Writer的方式进行写入
        writer.write("lbwnb");   //以操作Writer的样子写入OutputStream
    }catch (IOException e){
        e.printStackTrace();
    }
}

public static void main(String[] args) {
    try(InputStreamReader reader = new InputStreamReader(new FileInputStream("test.txt"))){  //虽然给定的是FileInputStream，但是现在支持以Reader的方式进行读取
        System.out.println((char) reader.read());
    }catch (IOException e){
        e.printStackTrace();
    }
}
```

InputStreamReader和OutputStreamWriter本质也是Reader和Writer，因此可以直接放入BufferedReader来实现更加方便的操作。

#### 打印流

打印流其实我们从一开始就在使用了，比如System.out就是一个PrintStream，PrintStream也继承自FilterOutputStream类因此依然是装饰我们传入的输出流，但是它存在自动刷新机制，例如当向PrintStream流中写入一个字节数组后自动调用flush()方法。PrintStream也永远不会抛出异常，而是使用内部检查机制checkError()方法进行错误检查。最方便的是，它能够格式化任意的类型，将它们以字符串的形式写入到输出流。

```java
public static void main(String[] args) {
    try(PrintStream stream = new PrintStream(new FileOutputStream("test.txt"))){
        stream.println("lbwnb");   //其实System.out就是一个PrintStream
    }catch (IOException e){
        e.printStackTrace();
    }
}
```

与此相同的还有一个PrintWriter，不过他们的功能基本一致，PrintWriter的构造方法可以接受一个Writer作为参数

#### 数据流

数据流DataInputStream也是FilterInputStream的子类，同样采用装饰者模式，最大的不同是它支持基本数据类型的直接读取：

```java
public static void main(String[] args) {
    try (DataInputStream dataInputStream = new DataInputStream(new FileInputStream("test.txt"))){
        System.out.println(dataInputStream.readBoolean());   //直接将数据读取为任意基本数据类型
    }catch (IOException e) {
        e.printStackTrace();
    }
}

//用于写入基本数据类型：
public static void main(String[] args) {
    try (DataOutputStream dataOutputStream = new DataOutputStream(new FileOutputStream("output.txt"))){
        dataOutputStream.writeBoolean(false);
    }catch (IOException e) {
        e.printStackTrace();
    }
}
//注意，写入的是二进制数据，并不是写入的字符串，使用DataInputStream可以读取，一般他们是配合一起使用的。
```





#### 对象流

既然基本数据类型能够读取和写入基本数据类型，那么能否将对象也支持呢？ObjectOutputStream不仅支持基本数据类型，通过对对象的序列化操作，以某种格式保存对象，来支持对象类型的IO，注意：它不是继承自FilterInputStream的。

1. 对象的序列化
   - 对象序列机制允许把内存中的java对象转换成平台无关的二进制流，从而允许把着中国二进制流持久的保存在磁盘上，或通过网络将着这种二进制流传输到另一个网络节点，当其他程序获取了这种二进制流，就可以恢复成原来的Java对象
2. 如果需要让某个对象支持序列化机制，则必须让对象所属的类及其属性是可序列化的，为了让某个类是可序列化的，该类必须实现如下两个接口之一，否则会抛出NotSeralizableException异常
   - Seriallzable
   - Externallzable

```java
public static void main(String[] args) {
    try (ObjectOutputStream outputStream = new ObjectOutputStream(new FileOutputStream("output.txt"));
         ObjectInputStream inputStream = new ObjectInputStream(new FileInputStream("output.txt"))){
        People people = new People("lbw");
        outputStream.writeObject(people);
      	outputStream.flush();
        people = (People) inputStream.readObject();
        System.out.println(people.name);
    }catch (IOException | ClassNotFoundException e) {
        e.printStackTrace();
    }
}

//在我们后续的操作中，有可能会使得这个类的一些结构发生变化，而原来保存的数据只适用于之前版本的这个类，因此我们需要一种方法来区分类的不同版本：
static class People implements Serializable{
    private static final long serialVersionUID = 123456;   //在序列化时，会被自动添加这个属性，它代表当前类的版本，我们也可以手动指定版本。

    String name;

    public People(String name){
        this.name = name;
    }
}
//当发生版本不匹配时，会无法反序列化为对象：


//如果我们不希望某些属性参与到序列化中进行保存，我们可以添加transient关键字：
public static void main(String[] args) {
    try (ObjectOutputStream outputStream = new ObjectOutputStream(new FileOutputStream("output.txt"));
         ObjectInputStream inputStream = new ObjectInputStream(new FileInputStream("output.txt"))){
        People people = new People("lbw");
        outputStream.writeObject(people);
        outputStream.flush();
        people = (People) inputStream.readObject();
        System.out.println(people.name);  //虽然能得到对象，但是name属性并没有保存，因此为null
    }catch (IOException | ClassNotFoundException e) {
        e.printStackTrace();
    }
}

static class People implements Serializable{
    private static final long serialVersionUID = 1234567;

    transient String name;

    public People(String name){
        this.name = name;
    }
}
```

其实我们可以看到，在一些JDK内部的源码中，也存在大量的transient关键字，使得某些属性不参与序列化，取消这些不必要保存的属性，可以节省数据空间占用以及减少序列化时间。

### 多线程

在Java中，我们从开始，一直以来编写的都是单线程应用程序（运行`main()`方法的内容），也就是说只能同时执行一个任务（无论你是调用方法、还是进行计算，始终都是依次进行的，也就是同步的），而如果我们希望同时执行多个任务（两个方法**同时**在运行或者是两个计算同时在进行，也就是异步的），就需要用到Java多线程框架。实际上一个Java程序启动后，会创建很多线程，不仅仅只运行一个主线程：



并行：逻辑上同时发生，指的是在某一个时间段内同时运行多个程序

并发：物理上同时发生，指的是在某一个时间点同时运行多个程序

线程：在同一个进程内可以执行多个任务，每一个任务可以看作一个线程，是程序的执行单元，执行路径，是程序使用CPU的最基本单位

单线程：程序只有一条执行路径

多线程：程序有多条执行路径

多线程的意义：为了提高应用程序的使用率

线程的执行是具有随机性的

#### 线程的创建和启动

通过创建Thread对象来创建一个新的线程，Thread构造方法中需要传入一个Runnable接口的实现（其实就是编写要在另一个线程执行的内容逻辑）同时Runnable只有一个未实现方法，因此可以直接使用lambda表达式：

```java
public static void main(String[] args) {
    Thread t = new Thread(() -> {    //直接编写逻辑
        System.out.println("我是另一个线程！");
    });
    t.start();   //调用此方法来开始执行此线程
}

public static void main(String[] args) {
    Thread t1 = new Thread(() -> {
        for (int i = 0; i < 50; i++) {
            System.out.println("我是一号线程："+i);
        }
    });
    Thread t2 = new Thread(() -> {
        for (int i = 0; i < 50; i++) {
            System.out.println("我是二号线程："+i);
        }
    });
    t1.start();
    t2.start();
}

我们可以看到打印实际上是在交替进行的，也证明了他们是在同时运行！
```

注意：我们发现还有一个run方法，也能执行线程里面定义的内容，但是run是直接在当前线程执行，并不是创建一个线程执行！
![image-20240307220325321](C:\Users\nxbhx\AppData\Roaming\Typora\typora-user-images\image-20240307220325321.png)

实际上，线程和进程差不多，也会等待获取CPU资源，一旦获取到，就开始按顺序执行我们给定的程序，当需要等待外部IO操作（比如Scanner获取输入的文本），就会暂时处于休眠状态，等待通知，或是调用sleep()方法来让当前线程休眠一段时间：





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
- boolean isAlive( )：返回boolean，判断线程是否还活着z



#### 线程的休眠和中断

我们前面提到，一个线程处于运行状态下，线程的下一个状态会出现以下情况：

当CPU给予的运行时间结束时，会从运行状态回到就绪（可运行）状态，等待下一次获得CPU资源。
当线程进入休眠 / 阻塞(如等待IO请求) / 手动调用wait()方法时，会使得线程处于等待状态，当等待状态结束后会回到就绪状态。
当线程出现异常或错误 / 被stop() 方法强行停止 / 所有代码执行结束时，会使得线程的运行终止。

```java
public static void main(String[] args) {
    Thread t = new Thread(() -> {
        try {
            System.out.println("l");
            Thread.sleep(1000);   //sleep方法是Thread的静态方法，它只作用于当前线程（它知道当前线程是哪个）
            System.out.println("b");    //调用sleep后，线程会直接进入到等待状态，直到时间结束
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    });
    t.start();
}
```

通过调用`sleep()`方法来将当前线程进入休眠，使得线程处于等待状态一段时间。我们发现，此方法显示声明了会抛出一个InterruptedException异常，那么这个异常在什么时候会发生呢？

我们发现，每一个Thread对象中，都有一个interrupt()方法，调用此方法后，会给指定线程添加一个中断标记以告知线程需要立即停止运行或是进行其他操作，由线程来响应此中断并进行相应的处理，我们前面提到的stop()方法是强制终止线程，这样的做法虽然简单粗暴，但是很有可能导致资源不能完全释放，而类似这样的发送通知来告知线程需要中断，让线程自行处理后续，会更加合理一些，也是更加推荐的做法。

通过`isInterrupted()`可以判断线程是否存在中断标志，如果存在，说明外部希望当前线程立即停止，也有可能是给当前线程发送一个其他的信号，如果我们并不是希望收到中断信号就是结束程序，而是通知程序做其他事情，我们可以在收到中断信号后，复位中断标记，然后继续做我们的事情：

```java
public static void main(String[] args) {
    Thread t = new Thread(() -> {
        System.out.println("线程开始运行！");
        while (true){
            if(Thread.currentThread().isInterrupted()){   //判断是否存在中断标志
                System.out.println("发现中断信号，复位，继续运行...");
                Thread.interrupted();  //复位中断标记（返回值是当前是否有中断标记，这里不用管）
            }
        }
    });
    t.start();
    try {
        Thread.sleep(3000);   //休眠3秒，一定比线程t先醒来
        t.interrupt();   //调用t的interrupt方法
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}

//复位中断标记后，会立即清除中断标记。那么，如果现在我们想暂停线程呢？我们希望线程暂时停下，比如等待其他线程执行完成后，再继续运行，那这样的操作怎么实现呢？
public static void main(String[] args) {
    Thread t = new Thread(() -> {
        System.out.println("线程开始运行！");
        Thread.currentThread().suspend();   //暂停此线程
        System.out.println("线程继续运行！");
    });
    t.start();
    try {
        Thread.sleep(3000);   //休眠3秒，一定比线程t先醒来
        t.resume();   //恢复此线程
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

```

虽然这样很方便地控制了线程的暂停状态，但是这两个方法我们发现实际上也是不推荐的做法，它很容易导致死锁！

#### 线程的优先级

实际上，Java程序中的每个线程并不是平均分配CPU时间的，为了使得线程资源分配更加合理，Java采用的是抢占式调度方式，优先级越高的线程，优先使用CPU资源！我们希望CPU花费更多的时间去处理更重要的任务，而不太重要的任务，则可以先让出一部分资源。线程的优先级一般分为以下三种：

MIN_PRIORITY 最低优先级
MAX_PRIORITY 最高优先级
NOM_PRIORITY 常规优先级

```java
public static void main(String[] args) {
    Thread t = new Thread(() -> {
        System.out.println("线程开始运行！");
    });
    t.start();
    t.setPriority(Thread.MIN_PRIORITY);  //通过使用setPriority方法来设定优先级
}
```

优先级越高的线程，获得CPU资源的概率会越大，并不是说一定优先级越高的线程越先执行！



#### 线程的礼让和加入

我们还可以在当前线程的工作不重要时，将CPU资源让位给其他线程，通过使用`yield()`方法来将当前资源让位给其他同优先级线程

```java
public static void main(String[] args) {
    Thread t1 = new Thread(() -> {
        System.out.println("线程1开始运行！");
        for (int i = 0; i < 50; i++) {
            if(i % 5 == 0) {
                System.out.println("让位！");
                Thread.yield();
            }
            System.out.println("1打印："+i);
        }
        System.out.println("线程1结束！");
    });
    Thread t2 = new Thread(() -> {
        System.out.println("线程2开始运行！");
        for (int i = 0; i < 50; i++) {
            System.out.println("2打印："+i);
        }
    });
    t1.start();
    t2.start();
}
//观察结果，我们发现，在让位之后，尽可能多的在执行线程2的内容。
```

当我们希望一个线程等待另一个线程执行完成后再继续进行，我们可以使用`join()`方法来实现线程的加入

线程1加入后，线程2等待线程1待执行的内容全部执行完成之后，再继续执行的线程2内容。注意，线程的加入只是等待另一个线程的完成，并不是将另一个线程和当前线程合并！我们来看看：

```java
public static void main(String[] args) {
    Thread t1 = new Thread(() -> {
        System.out.println(Thread.currentThread().getName()+"开始运行！");
        for (int i = 0; i < 50; i++) {
            System.out.println(Thread.currentThread().getName()+"打印："+i);
        }
        System.out.println("线程1结束！");
    });
    Thread t2 = new Thread(() -> {
        System.out.println("线程2开始运行！");
        for (int i = 0; i < 50; i++) {
            System.out.println("2打印："+i);
            if(i == 10){
                try {
                    System.out.println("线程1加入到此线程！");
                    t1.join();    //在i==10时，让线程1加入，先完成线程1的内容，在继续当前内容
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    });
    t1.start();
    t2.start();
}
```

实际上，t2线程只是暂时处于等待状态，当t1执行结束时，t2才开始继续执行，只是在效果上看起来好像是两个线程合并为一个线程在执行而已。

#### 线程锁和线程同步

线程之间的共享变量（比如之前悬念中的value变量）存储在主内存（main memory）中，每个线程都有一个私有的工作内存（本地内存），工作内存中存储了该线程以读/写共享变量的副本。它类似于我们在计算机组成原理中学习的多核心处理器高速缓存机制：
![image-20221004204209038](https://image.itbaima.cn/markdown/2022/10/04/SKlbIZyvxMnauLJ.png)

高速缓存通过保存内存中数据的副本来提供更加快速的数据访问，但是如果多个处理器的运算任务都涉及同一块内存区域，就可能导致各自的高速缓存数据不一致，在写回主内存时就会发生冲突，这就是引入高速缓存引发的新问题，称之为：缓存一致性。

通过synchronized关键字来创造一个线程锁，首先我们来认识一下synchronized代码块，它需要在括号中填入一个内容，必须是一个对象或是一个类，我们在value自增操作外套上同步代码块：

```java
private static int value = 0;

public static void main(String[] args) throws InterruptedException {
    Thread t1 = new Thread(() -> {
        for (int i = 0; i < 10000; i++) {
            synchronized (Main.class){  //使用synchronized关键字创建同步代码块
                value++;
            }
        }
        System.out.println("线程1完成");
    });
    Thread t2 = new Thread(() -> {
        for (int i = 0; i < 10000; i++) {
            synchronized (Main.class){
                value++;
            }
        }
        System.out.println("线程2完成");
    });
    t1.start();
    t2.start();
    Thread.sleep(1000);  //主线程停止1秒，保证两个线程执行完成
    System.out.println(value);
}
```

我们发现，现在得到的结果就是我们想要的内容了，因为在同步代码块执行过程中，拿到了我们传入对象或类的锁（传入的如果是对象，就是对象锁，不同的对象代表不同的对象锁，如果是类，就是类锁，类锁只有一个，实际上类锁也是对象锁，是Class类实例，但是Class类实例同样的类无论怎么获取都是同一个），但是注意两个线程必须使用同一把锁！

当一个线程进入到同步代码块时，会获取到当前的锁，而这时如果其他使用同样的锁的同步代码块也想执行内容，就必须等待当前同步代码块的内容执行完毕，在执行完毕后会自动释放这把锁，而其他的线程才能拿到这把锁并开始执行同步代码块里面的内容（实际上synchronized是一种悲观锁，随时都认为有其他线程在对数据进行修改，后面在JUC篇视频教程中我们还会讲到乐观锁，如CAS算法）

synchronized关键字也可以作用于方法上，调用此方法时也会获取锁：

```java
private static int value = 0;

private static synchronized void add(){
    value++;
}

public static void main(String[] args) throws InterruptedException {
    Thread t1 = new Thread(() -> {
        for (int i = 0; i < 10000; i++) add();
        System.out.println("线程1完成");
    });
    Thread t2 = new Thread(() -> {
        for (int i = 0; i < 10000; i++) add();
        System.out.println("线程2完成");
    });
    t1.start();
    t2.start();
    Thread.sleep(1000);  //主线程停止1秒，保证两个线程执行完成
    System.out.println(value);
}
```

我们发现实际上效果是相同的，只不过这个锁不用你去给，如果是静态方法，就是使用的类锁，而如果是普通成员方法，就是使用的对象锁。通过灵活的使用synchronized就能很好地解决我们之前提到的问题了。



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

#### 死锁

其实死锁的概念在`操作系统`中也有提及，它是指两个线程相互持有对方需要的锁，但是又迟迟不释放，导致程序卡住：

![image-20221004205058223](https://image.itbaima.cn/markdown/2022/10/04/Ja6TPO23wCI8pvn.png)

我们发现，线程A和线程B都需要对方的锁，但是又被对方牢牢把握，由于线程被无限期地阻塞，因此程序不可能正常终止。我们来看看以下这段代码会得到什么结果：

那么我们如何去检测死锁呢？我们可以利用jstack命令来检测死锁，首先利用jps找到我们的java进程：

jstack自动帮助我们找到了一个死锁，并打印出了相关线程的栈追踪信息，同样的，使用jconsole也可以进行监测。

因此，前面说不推荐使用 suspend()去挂起线程的原因，是因为suspend()在使线程暂停的同时，并不会去释放任何锁资源。其他线程都无法访问被它占用的锁。直到对应的线程执行resume()方法后，被挂起的线程才能继续，从而其它被阻塞在这个锁的线程才可以继续执行。但是，如果resume()操作出现在suspend()之前执行，那么线程将一直处于挂起状态，同时一直占用锁，这就产生了死锁。



**线程的死锁问题**

- 死锁：
  - 不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃自己需要的同步资源，就形成了线程的死锁
  - 出现死锁后，不会出现异常，不会出现提示，只是所有的线程都处于阻塞状态，无法继续
- 解决方法：
  - 专门的算法，原则
  - 尽量减少同步资源的定义
  - 尽量避免嵌套同步

#### wait和notify方法

```java
public static void main(String[] args) throws InterruptedException {
    Object o1 = new Object();
    Thread t1 = new Thread(() -> {
        synchronized (o1){
            try {
                System.out.println("开始等待");
                o1.wait();     //进入等待状态并释放锁
                System.out.println("等待结束！");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    });
    Thread t2 = new Thread(() -> {
        synchronized (o1){
            System.out.println("开始唤醒！");
            o1.notify();     //唤醒处于等待状态的线程
          	for (int i = 0; i < 50; i++) {
               	System.out.println(i);   
            }
          	//唤醒后依然需要等待这里的锁释放之前等待的线程才能继续
        }
    });
    t1.start();
    Thread.sleep(1000);
    t2.start();
}
```

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



#### ThreadLocal

们可以使用ThreadLocal类，来创建工作内存中的变量，它将我们的变量值存储在内部（只能存储一个变量），不同的线程访问到ThreadLocal对象时，都只能获取到当前线程所属的变量。

```java
public static void main(String[] args) throws InterruptedException {
    ThreadLocal<String> local = new ThreadLocal<>();  //注意这是一个泛型类，存储类型为我们要存放的变量类型
    Thread t1 = new Thread(() -> {
        local.set("lbwnb");   //将变量的值给予ThreadLocal
        System.out.println("变量值已设定！");
        System.out.println(local.get());   //尝试获取ThreadLocal中存放的变量
    });
    Thread t2 = new Thread(() -> {
        System.out.println(local.get());   //尝试获取ThreadLocal中存放的变量
    });
    t1.start();
    Thread.sleep(3000);    //间隔三秒
    t2.start();
}
```

上面的例子中，我们开启两个线程分别去访问ThreadLocal对象，我们发现，第一个线程存放的内容，第一个线程可以获取，但是第二个线程无法获取，我们再来看看第一个线程存入后，第二个线程也存放，是否会覆盖第一个线程存放的内容：

```java
public static void main(String[] args) throws InterruptedException {
    ThreadLocal<String> local = new ThreadLocal<>();  //注意这是一个泛型类，存储类型为我们要存放的变量类型
    Thread t1 = new Thread(() -> {
        local.set("lbwnb");   //将变量的值给予ThreadLocal
        System.out.println("线程1变量值已设定！");
        try {
            Thread.sleep(2000);    //间隔2秒
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("线程1读取变量值：");
        System.out.println(local.get());   //尝试获取ThreadLocal中存放的变量
    });
    Thread t2 = new Thread(() -> {
        local.set("yyds");   //将变量的值给予ThreadLocal
        System.out.println("线程2变量值已设定！");
    });
    t1.start();
    Thread.sleep(1000);    //间隔1秒
    t2.start();
}
```

我们发现，即使线程2重新设定了值，也没有影响到线程1存放的值，所以说，不同线程向ThreadLocal存放数据，只会存放在线程自己的工作空间中，而不会直接存放到主内存中，因此各个线程直接存放的内容互不干扰。

在线程中创建的子线程，无法获得父线程工作内存中的变量：我们可以使用InheritableThreadLocal来解决：

```java
public static void main(String[] args) {
    ThreadLocal<String> local = new InheritableThreadLocal<>();
    Thread t = new Thread(() -> {
       local.set("lbwnb");
        new Thread(() -> {
            System.out.println(local.get());
        }).start();
    });
    t.start();
}
```

在InheritableThreadLocal存放的内容，会自动向子线程传递。

#### 定时器

我们有时候会有这样的需求，我希望定时执行任务，比如3秒后执行，其实我们可以通过使用`Thread.sleep()`来实现：



我们通过自行封装一个TimerTask类，并在启动时，先休眠3秒钟，再执行我们传入的内容。那么现在我们希望，能否循环执行一个任务呢？比如我希望每隔1秒钟执行一次代码，这样该怎么做呢？

```java
public static void main(String[] args) {
    new TimerLoopTask(() -> System.out.println("我是定时任务！"), 3000).start();   //创建并启动此定时任务
}

static class TimerLoopTask{
    Runnable task;
    long loopTime;

    public TimerLoopTask(Runnable runnable, long loopTime){
        this.task = runnable;
        this.loopTime = loopTime;
    }

    public void start(){
        new Thread(() -> {
            try {
                while (true){   //无限循环执行
                    Thread.sleep(loopTime);
                    task.run();   //休眠后再运行
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();
    }
}
```

Java也为我们提供了一套自己的框架用于处理定时任务：

```java
public static void main(String[] args) {
    Timer timer = new Timer();    //创建定时器对象
    timer.schedule(new TimerTask() {   //注意这个是一个抽象类，不是接口，无法使用lambda表达式简化，只能使用匿名内部类
        @Override
        public void run() {
            System.out.println(Thread.currentThread().getName());    //打印当前线程名称
        }
    }, 1000);    //执行一个延时任务
}
```

我们可以通过创建一个Timer类来让它进行定时任务调度，我们可以通过此对象来创建任意类型的定时任务，包延时任务、循环定时任务等。我们发现，虽然任务执行完成了，但是我们的程序并没有停止，这是因为Timer内存维护了一个任务队列和一个工作线程：

TimerThread继承自Thread，是一个新创建的线程，在构造时自动启动：而它的run方法会循环地读取队列中是否还有任务，如果有任务依次执行，没有的话就暂时处于休眠状态：

我们可以在使用完成后，调用Timer的`cancel()`方法以正常退出我们的程序：

```java
public static void main(String[] args) {
    Timer timer = new Timer();
    timer.schedule(new TimerTask() {
        @Override
        public void run() {
            System.out.println(Thread.currentThread().getName());
            timer.cancel();  //结束
        }
    }, 1000);
}
```

#### 守护线程

不要把操作系统中的守护进程和守护线程相提并论！

守护进程在后台运行运行，不需要和用户交互，本质和普通进程类似。而守护线程就不一样了，当其他所有的非守护线程结束之后，守护线程自动结束，也就是说，Java中所有的线程都执行完毕后，守护线程自动结束，因此守护线程不适合进行IO操作，只适合打打杂：

```java
public static void main(String[] args) throws InterruptedException{
    Thread t = new Thread(() -> {
        while (true){
            try {
                System.out.println("程序正常运行中...");
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    });
    t.setDaemon(true);   //设置为守护线程（必须在开始之前，中途是不允许转换的）
    t.start();
    for (int i = 0; i < 5; i++) {
        Thread.sleep(1000);
    }
}
```

在守护线程中产生的新线程也是守护的：

#### 再谈集合类

集合类中有一个东西是Java8新增的Spliterator接口，翻译过来就是：可拆分迭代器（Splitable Iterator）和Iterator一样，Spliterator也用于遍历数据源中的元素，但它是为了并行执行而设计的。Java 8已经为集合框架中包含的所有数据结构提供了一个默认的Spliterator实现。在集合跟接口Collection中提供了一个spliterator()方法用于获取可拆分迭代器。

```java
default Stream<E> parallelStream() {
    return StreamSupport.stream(spliterator(), true); //parallelStream就是利用了可拆分迭代器进行多线程操作
}
```

并行流，其实就是一个多线程执行的流，它通过默认的ForkJoinPool实现（这里不讲解原理），它可以提高你的多线程任务的速度

```java
public static void main(String[] args) {
    List<Integer> list = new ArrayList<>(Arrays.asList(1, 4, 5, 2, 9, 3, 6, 0));
    list
            .parallelStream()    //获得并行流
            .forEach(i -> System.out.println(Thread.currentThread().getName()+" -> "+i));
}
```

我们发现，forEach操作的顺序，并不是我们实际List中的顺序，同时每次打印也是不同的线程在执行！我们可以通过调用`forEachOrdered()`方法来使用单线程维持原本的顺序：

我们之前还发现，在Arrays数组工具类中，也包含大量的并行方法：更多地使用并行方法，可以更加充分地发挥现代计算机多核心的优势，但是同时需要注意多线程产生的异步问题！

因为多线程的加入，我们之前认识的集合类都废掉了：为之前的集合类，并没有考虑到多线程运行的情况，如果两个线程同时执行，那么有可能两个线程同一时间都执行同一个方法，这种情况下就很容易出问题：当然，在Java早期的时候，还有一些老的集合类，这些集合类都是线程安全的：因为这些集合类中的每一个方法都加了锁，所以说不会出现多线程问题，但是这些老的集合类现在已经不再使用了

我们可以使用Vector代替List使用Hashtable<Integer, String>   也可以使用Hashtable来代替Map



### 反射

反射就是把Java类中的各个成分映射成一个个的Java对象。即在运行状态中，对于任意一个类，都能够知道这个类所有的属性和方法，对于任意一个对象，都能调用它的任意一个方法和属性。这种动态获取信息及动态调用对象方法的功能叫Java的反射机制。

简而言之，我们可以通过反射机制，获取到类的一些属性，包括类里面有哪些字段，有哪些方法，继承自哪个类，甚至还能获取到泛型！它的权限非常高，慎重使用！

- 反射机制允许程序在执行期间借助于Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及其方法

- 加载完类之后，在堆内存的方法区中就产生了一个Class类型的对象（一个类只有一个Class对象），这个对象就包含了完整的类的结构信息，我们可以通过这个对象看到类的结构，这个对象就像一面镜子，透过这个镜子看到类的节后，所以，我们形象的称之为：反射

- Java反射机制提供的功能
  - 在允许时判断任意一个对象所属的类
  - 在运行时构造任意一个类的对象
  - 在运行时判断任意一个类所具有的成员变量和方法
  - 在运行时后去泛型信息
  - 在运行时调用任意一个对象的成员变量和方法
  - 在运行时处理注解
  - 生成动态代理


#### Java类加载机制

在学习Java的反射机制之前，我们需要先了解一下类的加载机制，一个类是如何被加载和使用的：

![image-20240309233537213](C:\Users\nxbhx\AppData\Roaming\Typora\typora-user-images\image-20240309233537213.png)

在Java程序启动时，JVM会将一部分类（class文件）先加载（并不是所有的类都会在一开始加载），通过ClassLoader将类加载，在加载过程中，会将类的信息提取出来（存放在元空间中，JDK1.8之前存放在永久代），同时也会生成一个Class对象存放在内存（堆内存），注意此Class对象只会存在一个，与加载的类唯一对应！



#### Class类

通过前面，我们了解了类的加载，同时会提取一个类的信息生成Class对象存放在内存中，而反射机制其实就是利用这些存放的类信息，来获取类的信息和操作类。那么如何获取到每个类对应的Class对象呢，我们可以通过以下方式：

```java
public static void main(String[] args) throws ClassNotFoundException {
    Class<String> clazz = String.class;   //使用class关键字，通过类名获取
    Class<?> clazz2 = Class.forName("java.lang.String");   //使用Class类静态方法forName()，通过包名.类名获取，注意返回值是Class<?>
    Class<?> clazz3 = new String("cpdd").getClass();  //通过实例对象获取
}
```

注意Class类也是一个泛型类，只有第一种方法，能够直接获取到对应类型的Class对象，而以下两种方法使用了`?`通配符作为返回值，但是实际上都和第一个返回的是同一个对象：

```java
System.out.println(clazz == clazz2);
System.out.println(clazz == clazz3);
```

通过比较，验证了我们一开始的结论，在JVM中每个类始终只存在一个Class对象，无论通过什么方法获取，都是一样的。

基本数据类型也有对应的Class对象（反射操作可能需要用到），而且我们不仅可以通过class关键字获取，其实本质上是定义在对应的包装类中的：

每个包装类中（包括Void），都有一个获取原始类型Class方法，注意，getPrimitiveClass获取的是原始类型，并不是包装类型，只是可以使用包装类来表示。

```java
public static void main(String[] args) {
    Class<?> clazz = int.class;
    System.out.println(Integer.TYPE == int.class);
}
```

通过对比，我们发现实际上包装类型都有一个TYPE，其实也就是基本类型的Class。包装类型的Class对象并不是基本类型Class对象。

数组类型也是一种类型，只是编程不可见，因此我们可以直接获取数组的Class对象：

```java
public static void main(String[] args) {
    Class<String[]> clazz = String[].class;
    System.out.println(clazz.getName());  //获取类名称（得到的是包名+类名的完整名称）
    System.out.println(clazz.getSimpleName());
    System.out.println(clazz.getTypeName());
    System.out.println(clazz.getClassLoader());   //获取它的类加载器
    System.out.println(clazz.cast(new Integer("10")));   //强制类型转换
}
```

#### Class对象与多态

正常情况下，我们使用instanceof进行类型比较：

```java
public static void main(String[] args) {
    String str = "";
    System.out.println(str instanceof String);
}
//它可以判断一个对象是否为此接口或是类的实现或是子类，而现在我们有了更多的方式去判断类型：

public static void main(String[] args) {
    String str = "";
    System.out.println(str.getClass() == String.class);   //直接判断是否为这个类型
}

//如果需要判断是否为子类或是接口/抽象类的实现，我们可以使用asSubClass()方法：
public static void main(String[] args) {
    Integer i = 10;
    i.getClass().asSubclass(Number.class);   //当Integer不是Number的子类时，会产生异常
}

//通过getSuperclass()方法，我们可以获取到父类的Class对象：
public static void main(String[] args) {
    Integer i = 10;
    System.out.println(i.getClass().getSuperclass());
}

//也可以通过getGenericSuperclass()获取父类的原始类型的Type：
public static void main(String[] args) {
    Integer i = 10;
    Type type = i.getClass().getGenericSuperclass();
    System.out.println(type);
    System.out.println(type instanceof Class);
} 
//我们发现Type实际上是Class类的父接口，但是获取到的Type的实现并不一定是Class。

//同理，我们也可以像上面这样获取父接口：
public static void main(String[] args) {
    Integer i = 10;
    for (Class<?> anInterface : i.getClass().getInterfaces()) {
        System.out.println(anInterface.getName());
    }
  
  	for (Type genericInterface : i.getClass().getGenericInterfaces()) {
        System.out.println(genericInterface.getTypeName());
    }
}
```

反射只能获得类内写了的属性，不能获得继承的父类的属性

#### 创建类对象

既然我们拿到了类的定义，那么是否可以通过Class对象来创建对象、调用方法、修改变量呢？当然是可以的，那我们首先来探讨一下如何创建一个类的对象：

```java
public static void main(String[] args) throws InstantiationException, IllegalAccessException {
    Class<Student> clazz = Student.class;
    Student student = clazz.newInstance();
    student.test();
}

static class Student{
    public void test(){
        System.out.println("萨日朗");
    }
}
```

通过使用`newInstance()`方法来创建对应类型的实例，返回泛型T，注意它会抛出InstantiationException和IllegalAccessException异常，那么什么情况下会出现异常呢？

当类默认的构造方法被带参构造覆盖时，会出现InstantiationException异常，因为`newInstance()`只适用于默认无参构造。

```java
public static void main(String[] args) throws InstantiationException, IllegalAccessException {
    Class<Student> clazz = Student.class;
    Student student = clazz.newInstance();
    student.test();
}

static class Student{

    private Student(){}

    public void test(){
        System.out.println("萨日朗");
    }
}
```

当默认无参构造的权限不是`public`时，会出现IllegalAccessException异常，表示我们无权去调用默认构造方法。在JDK9之后，不再推荐使用`newInstance()`方法了，而是使用我们接下来要介绍到的，通过获取构造器，来实例化对象：

```java
public static void main(String[] args) throws NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {
    Class<Student> clazz = Student.class;
    Student student = clazz.getConstructor(String.class).newInstance("what's up");
    student.test();
}

static class Student{

    public Student(String str){}

    public void test(){
        System.out.println("萨日朗");
    }
}
```

通过获取类的构造方法（构造器）来创建对象实例，会更加合理，我们可以使用`getConstructor()`方法来获取类的构造方法，同时我们需要向其中填入参数，也就是构造方法需要的类型，当然我们这里只演示了。那么，当访问权限不是public的时候呢？

我们发现，当访问权限不足时，会无法找到此构造方法，那么如何找到非public的构造方法呢？

```java
Class<Student> clazz = Student.class;
Constructor<Student> constructor = clazz.getDeclaredConstructor(String.class);
constructor.setAccessible(true);   //修改访问权限
Student student = constructor.newInstance("what's up");
student.test();
```

使用`getDeclaredConstructor()`方法可以找到类中的非public构造方法，但是在使用之前，我们需要先修改访问权限，在修改访问权限之后，就可以使用非public方法了（这意味着，反射可以无视权限修饰符访问类的内容）

#### 调用类方法

我们可以通过反射来调用类的方法（本质上还是类的实例进行调用）只是利用反射机制实现了方法的调用，我们在包下创建一个新的类：

```java
package com.test;
public class Student {
    public void test(String str){
        System.out.println("萨日朗"+str);
    }
}

//这次我们通过forName(String)来找到这个类并创建一个新的对象：
public static void main(String[] args) throws ReflectiveOperationException {
    Class<?> clazz = Class.forName("com.test.Student");
    Object instance = clazz.newInstance();   //创建出学生对象
    Method method = clazz.getMethod("test", String.class);   //通过方法名和形参类型获取类中的方法
    
    method.invoke(instance, "what's up");   //通过Method对象的invoke方法来调用方法
}
```

通过调用getMethod()方法，我们可以获取到类中所有声明为public的方法，得到一个Method对象，我们可以通过Method对象的invoke()方法（返回值就是方法的返回值，因为这里是void，返回值为null）来调用已经获取到的方法，注意传参。

我们发现，利用反射之后，在一个对象从构造到方法调用，没有任何一处需要引用到对象的实际类型，我们也没有导入Student类，整个过程都是反射在代替进行操作，使得整个过程被模糊了，过多的使用反射，会极大地降低后期维护性。

同构造方法一样，当出现非public方法时，我们可以通过反射来无视权限修饰符，获取非public方法并调用，现在我们将test()方法的权限修饰符改为private：

```java
public static void main(String[] args) throws ReflectiveOperationException {
    Class<?> clazz = Class.forName("com.test.Student");
    Object instance = clazz.newInstance();   //创建出学生对象
    Method method = clazz.getDeclaredMethod("test", String.class);   //通过方法名和形参类型获取类中的方法
    method.setAccessible(true);

    method.invoke(instance, "what's up");   //通过Method对象的invoke方法来调用方法
}
```

Method和Constructor都和Class一样，他们存储了方法的信息，包括方法的形式参数列表，返回值，方法的名称等内容，我们可以直接通过Method对象来获取这些信息：

当方法的参数为可变参数时，我们该如何获取方法呢？实际上，我们在之前就已经提到过，可变参数实际上就是一个数组，因此我们可以直接使用数组的class对象表示：

```java
Method method = clazz.getDeclaredMethod("test", String[].class);
```

#### 修改类的属性

我们还可以通过反射访问一个类中定义的成员字段也可以修改一个类的对象中的成员字段值，通过`getField()`方法来获取一个类定义的指定字段：

```java
public static void main(String[] args) throws ReflectiveOperationException {
    Class<?> clazz = Class.forName("com.test.Student");
    Object instance = clazz.newInstance();

    Field field = clazz.getField("i");   //获取类的成员字段i
    field.set(instance, 100);   //将类实例instance的成员字段i设置为100

    Method method = clazz.getMethod("test");
    method.invoke(instance);
}
```

在得到Field之后，我们就可以直接通过`set()`方法为某个对象，设定此属性的值，比如上面，我们就为instance对象设定值为100，当访问private字段时，同样可以按照上面的操作进行越权访问：

```java
public static void main(String[] args) throws ReflectiveOperationException {
    Class<?> clazz = Class.forName("com.test.Student");
    Object instance = clazz.newInstance();

    Field field = clazz.getDeclaredField("i");   //获取类的成员字段i
    field.setAccessible(true);
    field.set(instance, 100);   //将类实例instance的成员字段i设置为100

    Method method = clazz.getMethod("test");
    method.invoke(instance);
}
```

当字段为final时，就修改失败了！当然，通过反射可以直接将final修饰符直接去除，去除后，就可以随意修改内容了

```java
public static void main(String[] args) throws ReflectiveOperationException {
    Integer i = 10;

    Field field = Integer.class.getDeclaredField("value");

    Field modifiersField = Field.class.getDeclaredField("modifiers");  //这里要获取Field类的modifiers字段进行修改
    modifiersField.setAccessible(true);
    modifiersField.setInt(field,field.getModifiers()&~Modifier.FINAL);  //去除final标记

    field.setAccessible(true);
    field.set(i, 100);   //强行设置值

    System.out.println(i);
}
```

```java
public static void main(String[] args) throws ReflectiveOperationException {
    List<String> i = new ArrayList<>();

    Field field = ArrayList.class.getDeclaredField("size");
    field.setAccessible(true);
    field.set(i, 10);

    i.add("测试");   //只添加一个元素
    System.out.println(i.size());  //大小直接变成11
    i.remove(10);   //瞎移除都不带报错的，淦
}
```

实际上，整个ArrayList体系由于我们的反射操作，导致被破坏，因此它已经无法正常工作了！

再次强调，在进行反射操作时，必须注意是否安全，虽然拥有了创世主的能力，但是我们不能滥用，我们只能把它当做一个不得已才去使用的工具！



#### 类加载器

我们接着来介绍一下类加载器，实际上类加载器就是用于加载一个类的，但是类加载器并不是只有一个。

**思考：** 既然说Class对象和加载的类唯一对应，那如果我们手动创建一个与JDK包名一样，同时类名也保持一致，JVM会加载这个类吗？

```java
package java.lang;

public class String {    //JDK提供的String类也是
    public static void main(String[] args) {
        System.out.println("我姓🐴，我叫🐴nb");
    }
}
```

但是我们明明在自己写的String类中定义了main方法啊，为什么会找不到此方法呢？实际上这是ClassLoader的`双亲委派机制`在保护Java程序的正常运行：

![image-20240310011031999](C:\Users\nxbhx\AppData\Roaming\Typora\typora-user-images\image-20240310011031999.png)

实际上类最开始是由BootstarpClassLoader进行加载，BootstarpClassLoader用于加载JDK提供的类，而我们自己编写的类实际上是AppClassLoader加载的，只有BootstarpClassLoader都没有加载的类，才会让AppClassLoader来加载，因此我们自己编写的同名包同名类不会被加载，而实际要去启动的是真正的String类，也就自然找不到main方法了。

```java
public class Main {
    public static void main(String[] args) {
        System.out.println(Main.class.getClassLoader());   //查看当前类的类加载器
        System.out.println(Main.class.getClassLoader().getParent());  //父加载器
        System.out.println(Main.class.getClassLoader().getParent().getParent());  //爷爷加载器
        System.out.println(String.class.getClassLoader());   //String类的加载器
    }
}
```

由于BootstarpClassLoader是C++编写的，我们在Java中是获取不到的。

既然通过ClassLoader就可以加载类，那么我们可以自己手动将class文件加载到JVM中吗？先写好我们定义的类：

```java
package com.test;

public class Test {
    public String text;

    public void test(String str){
        System.out.println(text+" > 我是测试方法！"+str);
    }
}
//通过javac命令，手动编译一个.class文件：
nagocoler@NagodeMacBook-Pro HelloWorld % javac src/main/java/com/test/Test.java
    
//编译后，得到一个class文件，我们把它放到根目录下，然后编写一个我们自己的ClassLoader，因为普通的ClassLoader无法加载二进制文件，因此我们编写一个自定义的来让它支持：
//定义一个自己的ClassLoader
static class MyClassLoader extends ClassLoader{
    public Class<?> defineClass(String name, byte[] b){
        return defineClass(name, b, 0, b.length);   //调用protected方法，支持载入外部class文件
    }
}

public static void main(String[] args) throws IOException {
    MyClassLoader classLoader = new MyClassLoader();
    FileInputStream stream = new FileInputStream("Test.class");
    byte[] bytes = new byte[stream.available()];
    stream.read(bytes);
    Class<?> clazz = classLoader.defineClass("com.test.Test", bytes);   //类名必须和我们定义的保持一致
    System.out.println(clazz.getName());   //成功加载外部class文件
}    
//现在，我们就将此class文件读取并解析为Class了，现在我们就可以对此类进行操作了（注意，我们无法在代码中直接使用此类型，因为它是我们直接加载的），我们来试试看创建一个此类的对象并调用其方法：    
try {
    Object obj = clazz.newInstance();
    Field field = clazz.getField("text");   //获取成员变量 String text;
    field.set(obj, "华强");
    Method method = clazz.getMethod("test", String.class);   //获取我们定义的test(String str)方法
    method.invoke(obj, "哥们这瓜多少钱一斤？");
}catch (Exception e){
    e.printStackTrace();
}
```

通过这种方式，我们就可以实现外部加载甚至是网络加载一个类，只需要把类文件传递即可，这样就无需再将代码写在本地，而是动态进行传递，不仅可以一定程度上防止源代码被反编译（只是一定程度上，想破解你代码有的是方法），而且在更多情况下，我们还可以对byte[]进行加密，保证在传输过程中的安全性。

### 注解

注意： 注解跟我们之前讲解的注释完全不是一个概念，不要搞混了。

其实我们在之前就接触到注解了，比如@Override表示重写父类方法（当然不加效果也是一样的，此注解在编译时会被自动丢弃）注解本质上也是一个类，只不过它的用法比较特殊。

注解可以被标注在任意地方，包括方法上、类名上、参数上、成员属性上、注解定义上等，就像注释一样，它相当于我们对某样东西的一个标记。而与注释不同的是，注解可以通过反射在运行时获取，注解也可以选择是否保留到运行时。









#### 预设注解

JDK预设了以下注解，作用于代码：

@Override - 检查（仅仅是检查，不保留到运行时）该方法是否是重写方法。如果发现其父类，或者是引用的接口中并没有该方法时，会报编译错误。
@Deprecated - 标记过时方法。如果使用该方法，会报编译警告。
@SuppressWarnings - 指示编译器去忽略注解中声明的警告（仅仅编译器阶段，不保留到运行时）
@FunctionalInterface - Java 8 开始支持，标识一个匿名函数或函数式接口。
@SafeVarargs - Java 7 开始支持，忽略任何使用参数为泛型变量的方法或构造函数调用产生的警告。



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

#### 元注解

元注解是作用于注解上的注解，用于我们编写自定义的注解：

@Retention - 标识这个注解怎么保存，是只在代码中，还是编入class文件中，或者是在运行时可以通过反射访问。
@Documented - 标记这些注解是否包含在用户文档中。
@Target - 标记这个注解应该是哪种 Java 成员。
@Inherited - 标记这个注解是继承于哪个注解类(默认 注解并没有继承于任何子类)
@Repeatable - Java 8 开始支持，标识某注解可以在同一个声明上使用多次。

看了这么多预设的注解，你们肯定眼花缭乱了，那我们来看看`@Override`是如何定义的：

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {
}
```

该注解由`@Target`限定为只能作用于方法上，ElementType是一个枚举类型，用于表示此枚举的作用域，一个注解可以有很多个作用域。`@Retention`表示此注解的保留策略，包括三种策略，在上述中有写到，而这里定义为只在代码中。一般情况下，自定义的注解需要定义1个`@Retention`和1-n个`@Target`。



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

**如何自定义注解**

参照@SuppressWarnings定义

1. 注解声明为：@interface
2. 内部定义成员，通常使用value表示
3. 可以指定成员的默认值，使用default定义
4. 如果自定义注解没有成员，表明是一个标识作用

如果注解有成员，在使用注解时，需要指明成员的值

自定义注解必须配上注解的信息处理流程（使用反射）才有意义

自定义注解都会通过指明两个元注解：Retention，Target

#### 注解的使用

我们还可以在注解中定义一些属性，注解的属性也叫做成员变量，注解只有成员变量，没有方法。注解的成员变量在注解的定义中以“无形参的方法”形式来声明，其方法名定义了该成员变量的名字，其返回值定义了该成员变量的类型：

```java
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Test {
    String value();
}
//默认只有一个属性时，我们可以将其名字设定为value，否则，我们需要在使用时手动指定注解的属性名称，使用value则无需填入：
public class Main {
    @Test(test = "")
    public static void main(String[] args) {
    }
} 

//我们也可以使用default关键字来为这些属性指定默认值：
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Test {
    String value() default "都看到这里了，给个三连吧！";
}


//当属性存在默认值时，使用注解的时候可以不用传入属性值。当属性为数组时呢？
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
public @interface Test {
    String[] value();
}

//当属性为数组，我们在使用注解传参时，如果数组里面只有一个内容，我们可以直接传入一个值，而不是创建一个数组：
@Test("关注点了吗")
public static void main(String[] args) {
}

public class Main {
    @Test({"value1", "value2"})   //多个值时就使用花括号括起来
    public static void main(String[] args) {

    }
}
```







#### 反射获取注解

既然我们的注解可以保留到运行时，那么我们来看看，如何获取我们编写的注解，我们需要用到反射机制：

```java
public static void main(String[] args) {
    Class<Student> clazz = Student.class;
    for (Annotation annotation : clazz.getAnnotations()) {
        System.out.println(annotation.annotationType());   //获取类型
        System.out.println(annotation instanceof Test);   //直接判断是否为Test
        Test test = (Test) annotation;
        System.out.println(test.value());   //获取我们在注解中写入的内容
    }
}
```

通过反射机制，我们可以快速获取到我们标记的注解，同时还能获取到注解中填入的值，那么我们来看看，方法上的标记是不是也可以通过这种方式获取注解：

```java
public static void main(String[] args) throws NoSuchMethodException {
    Class<Student> clazz = Student.class;
    for (Annotation annotation : clazz.getMethod("test").getAnnotations()) {
        System.out.println(annotation.annotationType());   //获取类型
        System.out.println(annotation instanceof Test);   //直接判断是否为Test
        Test test = (Test) annotation;
        System.out.println(test.value());   //获取我们在注解中写入的内容
    }
}
```

无论是方法、类、还是字段，都可以使用`getAnnotations()`方法（还有几个同名的）来快速获取我们标记的注解。