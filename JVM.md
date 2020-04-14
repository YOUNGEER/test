# JVM

## 加载，链接，初始化

- java类的**<u>加载，连接和初始化</u>**都是在程序**运行**期间完成的
- 加载：查找并加载类的二进制（.class）数据
- ：将类的class文件中的二进制数据读入到内存中，将其放在运行时数据区的方法区中，然后在内存中创建一个java.lang.Class对象（规范并未说明Class对象位于哪里，HotSpot虚拟机将其放在了方法区中），用来封装类在方法区中的数据结构。
  - 验证：确保被加载类的正确性
  - 准备：为类的**静态变量**分配内存，并将其初始化为**默认值**
  - 解析：把类中的符号引用转换为直接引用
- 初始化：为类的静态变量赋于**正确的初始值**
  - 假如这个类还没有被加载和链接，那么就先加载和链接
  - 假如这个类存在直接父类，并且直接父类没有初始化，那么就先初始化该父类
  - 假如类中存在初始化语句，那就按照顺序进行初始化
- 使用
- 卸载



### java程序对类的使用方式可分为

#### 主动使用（7种）

- 创建类的实例，new
- 访问某个类或接口的静态变量，或者是对该静态变量赋值
- 调用类的静态方法
- 反射（比如Class.forName("com.cl.MyClass"))
- 初始化一个类的子类
- 被标记为虚拟机的启动类(比如Test)
- JDK1.7提供的动态语言机制
- **所有的java虚拟机必须在每个类或接口被java程序首次主动使用时才进行初始化**



#### 被动使用

- 除了主动使用的七种方式都是被动使用，不会导致类初始化

### 加载.class文件的方式

- 从本地系统中直接加载
- 通过网络下载.class文件
- 从jar，zip归档文件中加载
- 从专有数据库中提取
- 将Java源文件动态编译为.class文件

```java
public class Test1 {

    public static void main(String[] args) {
        /**
         * 1.对于静态字段来说，只有定义了该字段的类才会初始化
         * output:
         * parent static code
         * parent
         */
        System.out.println(Child1.str1);

        /**
         * 1.当一个类在进行初始化时，要求其所有父类都先初始化完毕
         * output:
         * parent static code
         * parent
         * child static code
         * parent
         */
        System.out.println(Child1.str2);
    }
}

class MyParent1 {
    public static String str1 = "parent";
    static {
        System.out.println("parent static code");
    }
}

class Child1 extends MyParent1 {
    public static String str2 = "parent";
    static {
        System.out.println("child static code");
    }
}
```

## .class文件

### 字节码

#### 什么是字节码？

- Java之所以可以“一次编译，到处运行”，一是因为JVM针对各种操作系统、平台都进行了定制，二是因为无论在什么平台，都可以编译生成固定格式的字节码（.class文件）供JVM使用。因此，也可以看出字节码对于Java生态的重要性。之所以被称之为字节码，是因为字节码文件由十六进制值组成，而JVM以两个十六进制值为一组，即以字节为单位进行读取

#### 字节码结构

![image-20200122232130786](/Users/wangyang/Library/Application Support/typora-user-images/image-20200122232130786.png)

![image-20200122232146109](/Users/wangyang/Library/Application Support/typora-user-images/image-20200122232146109.png)



#### idea如何看

- 由于idea打开.class文件都是反编译过的，方便阅读，和源代码基本类似，如果想看16进制的文件，可以
- 通过Java HelloWorld.java 生成class文件 HelloWorld.class
- 查看class 文件 打开文件 `vim HelloWorld.class` ，然后输入`:%!xxd` 就是以16进制显示class文件了

## jvm参数

### 形式

- -XX:+<option>开启option参数
- -XX:-<option>关闭option参数
- -XX:<option>=<value> 将option选项的值设置为value

### 种类

- -XX:+TraceClassLoading：输出所有加载的类

## 常量的本质 final

```java
public class Test2 {

    public static void main(String[] args) {
        /**
         * 常量在编译阶段会存入到调用这个常量的方法的类的常量池中
         * 本质上，调用类并没有直接引用定义该常量的的类，所以不会初始化
         * output:
         * parent
         */
        System.out.println(Parent.str);
    }
}

class Parent {

    public static final String str = "parent";

    static {
        System.out.println("parent static code");
    }

}
```

```java
/**
 * 当一个接口完成初始化的时候，并不要求其父类接口也完成初始化
 * 只有当使用都父类变量时，才会进行父类初始化
 */
public class Test3 {
    public static void main(String[] args) {
        System.out.println(Child3.a);
    }
}

/**
 * 接口的变量，默认是public static final 类型
 */
interface Parent3{
    int a=5;
}
interface Child3 extends Parent3{
    int b=6;
}
```



### 反编译理解

- GETSTATIC
- LDC
- ICONST_2
- BIPUSH

```java
public class com/wy/lucky/suanfa/Test2 {

  // compiled from: Test2.java

  // access flags 0x1
  public <init>()V
   L0
    LINENUMBER 18 L0
    ALOAD 0
    INVOKESPECIAL java/lang/Object.<init> ()V
    RETURN
   L1
    LOCALVARIABLE this Lcom/wy/lucky/suanfa/Test2; L0 L1 0
    MAXSTACK = 1
    MAXLOCALS = 1

  // access flags 0x9
  public static main([Ljava/lang/String;)V
    // parameter  args
   L0
    LINENUMBER 21 L0
    GETSTATIC java/lang/System.out : Ljava/io/PrintStream;
    LDC "parent"
    INVOKEVIRTUAL java/io/PrintStream.println (Ljava/lang/String;)V
   L1
    LINENUMBER 22 L1
    GETSTATIC java/lang/System.out : Ljava/io/PrintStream;
    ICONST_2
    INVOKEVIRTUAL java/io/PrintStream.println (I)V
   L2
    LINENUMBER 23 L2
    GETSTATIC java/lang/System.out : Ljava/io/PrintStream;
    BIPUSH 20
    INVOKEVIRTUAL java/io/PrintStream.println (I)V
   L3
    LINENUMBER 24 L3
    RETURN
   L4
    LOCALVARIABLE args [Ljava/lang/String; L0 L4 0
    MAXSTACK = 2
    MAXLOCALS = 1
}
```

### 编译期常量

- 不会对进行初始化

### 运行期常量

- 会对类进行初始化

### 数组类型

- 运行期jvm动态生成的，父类是Object

# 类的加载

- 类的加载最终产品就是位于内存中的Class对象，该对象封装了类在方法区中的类的数据接口和开放出来的接口

## 类加载器

### 系统自带的

- 根加载器bootstrap：加载最核心的类库，比如java.lang下面的类，rt.jar包下面的，没有父加载器，是C实现的
- 外部扩展加载器extension：主要是加载jdk目录的jre/lib/ext中的类，或者是java.ext.dirs中加载，父加载器是bootstrap，是java.lang.ClassLoader的子类，是纯Java代码实现的
- app/system加载器：应用加载器，主要加载环境变量或者是java.class.path指定的目录中加载类，程序员自定义代码的默认加载器，父加载器是extension，是java.lang.ClassLoader的子类，是纯java代码实现的

![image-20200206174247748](/Users/wangyang/Library/Application Support/typora-user-images/image-20200206174247748.png)

### 双亲委托模型

- 自底向上检查是否已经加载
- 自顶向下加载
- 优势
  - 防止重复加载：采用双亲委派模式的是好处是Java类随着它的类加载器一起具备了一种**带有优先级的层次关系**，通过这种层级关可以**避免类的重复加载**，当父亲已经加载了该类时，就没有必要子ClassLoader再加载一次。
  - 保证安全：其次是考虑到安全因素，java核心api中定义类型不会被随意替换，假设通过网络传递一个名为java.lang.Integer的类，通过双亲委托模式传递到启动类加载器，而启动类加载器在核心Java API发现这个名字的类，发现该类已被加载，并不会重新加载网络传递的过来的java.lang.Integer，而直接返回已加载过的Integer.class，这样便可以**防止核心API库被随意篡改**。
  - 

## ClassLoader源码分析

```java
public class MyTest6 extends ClassLoader {

    public String classLoaderName;
    public String extensionFileName=".class";

    public MyTest6(ClassLoader parent, String classLoaderName) {
        super(parent);
        this.classLoaderName = classLoaderName;
    }

    public MyTest6(String classLoaderName) {
        super();
        this.classLoaderName = classLoaderName;
    }

    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        byte[] data=loadClassData(name);
        return defineClass(name,data,0,data.length);
    }

    private byte[] loadClassData(String name){
        InputStream is=null;
        byte[] data=null;
        ByteArrayOutputStream baos=null;
        try {
            this.classLoaderName=this.classLoaderName.replace(".","/");
            is=new FileInputStream(new File(name+extensionFileName));
            baos=new ByteArrayOutputStream();

            int lenght=0;
            while (-1!=(lenght=is.read())){
                baos.write(lenght);
            }
            data=baos.toByteArray();

        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            try {
                baos.close();
                is.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        return data;
    }

    public static void testLoad(ClassLoader classLoader) throws Exception {
        Class clazz=classLoader.loadClass("Loader1Demo");
        Object object=clazz.newInstance();
        System.out.println(object.toString());
    }

}
```

- 构造方法，指定加载器名字，父类加载器
- 重写findClass方法，将byte数组转成Class实例，在loadClass方法中被调用，
- loadClass的过程可以简单理解为：
  - findLoadedClass
  - findParentClassLoader
  - findClass
- 自定义loadClassData方法，将.class文件转成二进制文件



# 内存模型

![image-20200219000459259](/Users/wangyang/Library/Application Support/typora-user-images/image-20200219000459259.png)

## 分类

- 虚拟机栈：线程私有，描述的是方法执行的内存模型，通过stack frame的push和pop操作为一个完整的过程。栈帧主要分为： 局部变量表、操作数栈、动态链接、方法出口等信息

  - 局部变量表：存放this对象，方法参数，局部变量
  - 操作数栈：布局变量表中的数据无法直接使用，需要转换到操作数（汇编）
  - 方法返回值地址：记录方法返回的地址

- 程序计数器：线程私有，是当前线程所执行的字节码的行号指示器

- 本地方法栈：native方法栈，hotpot vm将本地方法栈和虚拟机栈放在了一起

- 堆：jvm最大的一块空间，主要是用来存放对象的，线程共享

  - 对象持有内容
    - 对象实例属性和方法
    - class相关的元数据，只会持有一份（matedata，一般称为方法区）

  		- 句柄引用
  		- 直接引用
    - 垃圾回收机制
      - 新生代
      - 老年代
      - Eden

- 方法区：属于堆区域，存储类的元数据，主要是class对象信息，属性和方法的签名信息，永久代permanent generation，在jdk1.8后就移除了，用mate space代替

- 运行时常量区

- 直接内存区

## 对象在内存中的布局

- 对象头
- 实例数据
- 对其填充

## 垃圾回收

### 引用计数算法（Reference Counting）

- 原理：当一个对象被引用后，引用计算器就加1，当引用失效后，计数器减1，为0就可以进行垃圾回收
- 缺陷：无法解决对象间循环引用的问题

### 根搜索算法（gc roots  tracing）

- 一般java，c#都是该算法
- 原理：gc roots作为起始点，如果一个对象没有和根相连，那么该对象表示未有效引用，可以回收
- 方法区回收满足条件
  - 该类的所有实例对象都GC了，也即内存中不存在该Class实例
  - 加载该Class的classLoader已经为GC了
  - 该类对象Class没有在其他地方被引用，比如反射

### 常见的GC算法

#### 标记-清除算法（mark-sweep）

- 先标记可能回收的对象，再清楚可能回收的对象

- 效率不高，需要扫描所有对象，堆越大，效率越低
- 存在内存碎片问题，GC越多，碎片越多

#### 复制搜索算法（copying）

- 原理：将可用内存划分成两块，一块用完后，将存活的对象copy到另一块中，然后将用完的那块一次性清楚掉
- 缺点：需要大的内存空间
- 优点：简单，高效，不存在内存碎片的问题，在对象生命周期很短的地方可以使用，比如新生代
- 应用：现代商业虚拟机都是利用这一算法来回收新生代空间的，一般会将堆分三个部分，一个大的eden和两个小的survior空间，eden和一个survior的存活的对象会一次性copy到另一个survior上，然后再清楚eden和survior，比如hotspot默认将eden和survior分配的比例为：8:1:1，这样只会有10%的空间浪费



#### 标记-压缩算法（marked-compact）

- 原理：标记后，让存活的对象往一端进行移动，然后清楚另一端的对象，这样不存在内存碎片的问题
- 缺点：需要更多的时间来进行内存压缩，移动



#### 分代收集（generate collection），主要是heap

- 将堆分为新生代和老年代

- 新生代用了copy算法，可分为：eden+surviorA+surviorB，一般是对象先在eden区，然后将eden和surviorA存活的对象复制到surviorB中，如果超过surviorB的内存，那么超过的会进去到old区，一次gc后，会调换surviorA和surviorB的作用，以此反复，同时，对于survior多次未回收的对象，也会进入到old区中，表明该对象生命周期长

- old区的用了压缩方法进行gc

### GC的时机

- Scavenge GC
  - 触发时机：new出新对象，eden区满的时候，时间短效率极高
- Full GC
  - 对整个jvm进行扫描，效率低，应尽量避免，System.gc



















