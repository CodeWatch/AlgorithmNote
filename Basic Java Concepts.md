##Basic Java Concepts

本章列举了一些较为重要和较难记忆理解的Java知识收录此处共查阅学习，如有谬误，欢迎发送pull request。

<a name="AnchorIndex" id="AnchorIndex"></a>
Index:  
-[与C++的一些不同之处](#Anchor1)  
-[](#Anchor2)  

-------
<a name="Anchor1" id="Anchor1"></a>
-**[与C++的一些不同之处]**([Back to Index](#AnchorIndex))   

1.char类型的宽度为16bit；c++为8bit  
2.没有unsigned修饰符；c++有  
3.下列代码是非法的；c++合法
```cpp
     int x = 12;
     {  
          int x = 13;  
     }
```
4.String s;并没有创建一个对象，只是声明了一个引用；c++在栈上创建了一个对象
5.类成员会获得默认值；c++不会自动初始化类成员
6.类成员可以直接初始化；c++的类成员不能直接初始化，可以在构造函数里初始化
7.对于对象参数，默认为传引用；c++默认为传值
8.=号赋值只是使新的引用指向了旧的对象，也即新旧引用指向同一个对象；c++则会生成一个新的对象
9.>>>号是高位补0右移运算符；c++无此符号
10.拥有finalize()方法；c++只有析构函数
11.可以使用int[] A或int A[]方式声明一个数组，因为是引用，不可以指定数组大小；c++只能使用int A[]的方式，可以指定数组大小
12.默认开启动态绑定；C++使用virtual关键字才能开启动态绑定
13.所有容器都有共同的基类Collection；C++容器之间没有共同的基类，依赖迭代器达成共性
14.泛型和模板对比：
     >*C++模板在编译期检查对模板对象的调用是否在该对象所属类总存在来保证类型安全
     >*Java由于采用了类型擦除，仅使用T会使对象所属类被擦除到Object，而非其真正所属类，因此也就无法验证调用是否合法，因此要使用T extends Type来表示泛型边界（一个好的例子见Thinking in Java P375）
15.Comparable接口的方法compareTo(Type obj)返回的值包括-1，0，1，分别表示当前对象小于，等于，大于参数obj；
   C++的比较函数cmp(Type obj1, Type obj2)则只返回true和false，obj1<=obj2返回true，反之返回false

-------
<a name="Anchor1" id="Anchor1"></a>
-**[与C++的一些不同之处]**([Back to Index](#AnchorIndex))

###*Java重要知识点
1.java.lang是默认导入每个java文件的  
2.equals()默认行为是比较引用，可以重写；==判断的是引用的内存地址是否相同  
3.对byte和short的位移操作要特别小心，它们会被转换为int型然后位移，这会导致不正确的结果  
4.bool类型不允许任何类型转换，也不允许使用类似while(1)的语句，0,1并不等同于false和true，必须使用while(true)  
5.float或double转为整形值时，总是对该数字进行截尾  
6.提升：对基本数据类型进行算术运算或按位运算时，低于int型的数据类型（char、byte和short）总是会先转换为int再运算；数据类型大的类型决定了结果的类型  
7.不能仅仅使用返回值不同的方法作为重载方法，应该使参数列表不同（至少是顺序不同）  
8.如果已经定义了一个构造器，无论是否有参数，编译器不会自动创建默认构造器  
9.this的三个应用场合：
  *用于引用自身对象的成员，特别是因为成员与方法参数重名  
  *用于在一个重载的构造器中调用另一个构造器，此时必须放在第一句，且只有构造器可以调用其他构造器   
  *用于返回自身对象的引用
10.被static修饰的方法内不能调用非static方法和成员，因为非static方法和成员依赖具体的对象（除非这个静态方法获得自身类的一个对象实例并通过该实例调用非晶态方法）  
11.Java没有C++的析构函数，取而代之是finalize()，用于释放非Java代码（如native C）申请的系统资源，由GC调用（不要直接调用finalize()方法），而GC的调用时机是不能被预期的。  
12.Java的GC机制包括两种："停止-复制"和“标记-清扫”，都是从堆栈和静态存储区开始扫描引用。采用自适应技术在两种技术中切换，碎片多时用“停止-复制”，碎片少时用“标记-清扫”。  
13.不同于类成员，局部变量必须被初始化，否则报错。  
14.类成员的初始化顺序:类加载->new操作（或访问静态域，此行为不会导致空间分配）->静态成员初始化（包括静态块）(只发生一次，如果以前new操作/访问静态域操作出现过则不再初始化)->分配空间，非静态成员初始化（包括非静态块）->构造方法。类成员变量的初始化（包括但不限于默认初始化行为）绝对先于构造器的对类成员的初始化操作。  
   一个继承自A类的B类对象的初始化顺序：类B加载->类A加载->类A的静态成员初始化->类B的静态成员初始化->类A非静态成员初始化->类A的构造方法->类B非静态成员初始化->类B的构造方法  
15.构造器是static方法。  
16.一下两种声明和初始化数组的方式都是合法的，最后一个逗号是可选的  
     *Integer[] a = {new Integer(1), new Integer(2), 3, }//Auto Boxing  
     *Integer[] b = new Integer[]{new Integer(1), new Integer(2), 3, }//Auto Boxing  
17.可变参数形如void function(type... args)，三个注意点：  
     *可以给可变参数列表传0个参数  
     *可变参数列表不依赖于Auto Boxing  
     *尽量避免在多个重载方法中使用可变参数，可能会引起歧义  
          void function(int a, int... args){}  
          void function(int... args){}  
     *如果必须使用，添加非可变参数如  
          void function(int a, int... args){}  
          void function(string b, int... args){} 
18.enum的使用跟类相近，可以使用在switch内。可以对一个enum对象赋予枚举类型值，如EnumType et = EnumType.type1  
19.一个Java源文件中最多只能有一个public类，当有一个public类时，源文件名必须与之一致，否则无法编译，如果源文件中没有一个public类，则文件名与类中没有一致性要求。至于main()不是必须要放在public类中才能运行程序。总结出如下4点  
     *Java源文件可以有public类，也可以没有  
     *如果有public类，则该public类必须与文件名同名  
     *如果没有public类，则不要求有与文件名同名的类  
     *main()方法的放在哪个类里没有硬性要求，跟是否在public类中也没有关系，一个文件中多个类可以各自持有一个main()方法，都可以用java命令运行  
20.Java解释器的运行过程是首先找出环境变量CLASSPATH用于查找.class的根目录，然后加载。  
21.访问权限控制的等级：public->protected->包访问权限->private  
22.类的权限控制的等级：public->包访问权限，不可以是protected或private的  
23.一个类的构造器是private的，任何其他类无法直接创建该类，只有该类的static方法或对static对象赋值（即非延迟化实现）才可以创建一个该类的对象  
24.使用成员的访问控制主要有两个原因：  
     *使用户不要碰触他们不该碰触的部分  
     *更重要的是，可以让类的设计者改变类的内部实现而不影响会对客户端程序员产生重大影响  
25.使用super可以访问从基类继承的方法和成员，前提是这些方法和成员是public、protected，或着父类和子类在同一个包内且不是private的。  
26.如果父类的构造器没有无参形式（由于用户自定义了带参构造器），子类的构造器需要手动调用super(para);如果存在，则自动调用super();  
27.如果需要保证一个行为一定被执行（例如一些清除操作，手动编写和调用而不应该依赖finalize()和GC），应该将该行为放在finally块中：  
     try{  
          ...  
     }finally{  
          //mustBeDone();  
     }  
     对于清除操作，如果被清除对象是派生类，则注意对其父类也调用清除操作，方式类似构造方法的构造链的，顺序相反，与C++的析构函数的方式相同。  
28.可以把 @ override放在重写方法之前，如果对父类的方法进行的重载而非重写，编译器会报错  
29.final关键字使用在数据、方法和类的意义不同。  
     *final数据：  
          +对于基本类型，final使数值恒定不变；对于对象引用，final使对象引用不变，而对象本身是可以改变的  
               -如数组对象，final int[] a = {1,2,3}，a只能指向该数组，但1,2,3是可以修改的  
          +static final标识的变量是编译期常量  
               -如 final static int a = 1;  
          +final数据的两次初始化机会：定义处和构造器中。如果在定义处没有初始化，在构造器中必须初始化  
          +final参数：在方法内不允许修改该参数变量的值  
     *final方法：  
          +禁止该方法被重写，但可以使用，也即关闭动态绑定  
          +在子类中声明一个跟父类private（final）方法同样的方法并不会产生重写效果  
          +private方法都隐式的被指定为final方法，private方法并不作为父类接口的一部分，向上转型不能调用子类的同名方法（非覆盖），可能可以调用自身的private方法（如果在父类内部）  
     *final类：该类不能被继承  
30.多态的三种例外情况：  
     *private方法。如上所述，private方法被隐式的指定为final方法，因此也不存在重写的情况  
     *域。同名的静态域被当成两个不同的数据空间，而不能进行“多态”，根据引用决定调用的目标  
     *静态方法。与静态域类似，同名的静态方法会被当成两个不同的方法，没有“多态”，根据引用决定调用的目标  
31.在构造器里调用多态方法可能导致使用了未被正确初始化的变量，如在父类的构造器调用一个在子类中重写的方法，而该方法可能使用了一个子类中的非静态成员变量，该变量此时还未被正确赋值。  
32.Java SE5中添加了协变返回类型，这种机制允许子类重写方法返回父类被重写方法返回类型的派生类型，这样的形式同样被认为是多态的。  
33.包含abstract方法的类必是抽象类，抽象类不一定包含abstract方法（可能出于阻止产生该类对象的目的）  
34.Interface的域默认的是static、final和public，方法默认为public。在实现Interface时，必须将实现接口的方法声明为public。接口也可以通过extends关键字来扩展接口  
35.使用Interface的理由  
     *为实现多继承提供类似的实现，进而能够向上转型为多个基类型（以及由此带来的灵活性）  
     *空接口提供一个标签机制  
     *多个不同的类型实现同一个接口，可以使得这些类能够向上转型为同一个基类型（例如作为方法的参数来实现策略设计模式，或是作为泛型容器的实现方式）  
36.菱形继承问题：在Java中只有一个不合法的情况，即在一个类实现的两个接口中使用了完全一致的静态变量。使用同名函数能够正常工作。  
37.非静态内部类的创建需要捕获一个指向外围类的对象引用，因此不能脱离外围类对象创建内部类对象。内部类对象能访问器外围对象的所有成员，而且不需要任何特殊条件（即使为private），且嵌套层数不影响其对外围对象成员的访问，此外，内部类还拥有器外围类的所有元素的访问权。  
38.创建非静态内部类的两种方法：  
     *Outer out = new Outer();  
      Outer.Inner in = out.createInner();  
     *Outer out = new Outer();  
      Outer.Inner in = out.new Inner();  
     不能是Outer.Inner in = new Inner();或者Outer.Inner in = out.new Outer.Inner();
39.静态内部类（嵌套类）不需要对外部类对象的引用  
40.匿名内部类没有构造器，但可以使用非静态块达到实例初始化的目的。如果想在匿名内部类中使用外部方法的变量/参数，则要求这些变量/参数是final的。（传给基类构造器的使用不在此列）  
41.匿名内部类只能实现一个接口或者继承一个基类（2选1）  
42.为什么需要内部类：由于内部类可以继承自一个类且可以对外围类进行操作，这意味着多个内部类可以实现更为贴近真正意义上的多重继承，即允许继承多个非接口类型  
43.继承内部类B的派生类A必须提供一个以外部类C作为参数的构造器，并在A的构造器内调用c.super();  
44.通过对容器使用泛型，可以在编译期防止将错误类型对象放入容器中  
45.Object默认的toString()方法=类名+hashcode()  
46.List,Set,Map都是实现了Collection接口的容器，其中  
     *ArrayList和LinkedList维护插入顺序  
     *HashSet维护乱序，TreeSet维护升序，LinkedHashSet维护插入顺序  
     *HashMap维护乱序，TreeMap维护升序，LinkedHashMap维护插入顺序  
47.Java的迭代器跟C++类似，主要包括方法：  
     *collection.iterator();获得当前容器的迭代器，相当于C++的containter.begin();  
     *collection.next();获取下一个迭代器位置，相当于C++的itr++;  
     *collection.hasNext();检查序列中是否还有元素，相当于itr!=container.end();  
48.继承Collection就必须实现iterator()方法，该方法要求返回一个Iterator< A >对象，可以使用匿名内部类方式返回该迭代器对象。  
49.如果想要在foreach中使用某种容器类型（除数组外），要求该容器实现Iterable接口，该接口包含一个能够产生Iterator的iterator()方法，实现iterator()的方法可以参考48。一个好的例子见Thinking in Java P243  
50.使用Arrays.asList()需要谨慎，直接使用该方法返回的容器对象和使用该容器对象构造新的容器对象会产生不同的效果  
     *Integer[] ia = {1, 2, 3};  
     *List<Integer> list1 = Array.asList(ia);//对list1的排序会影响ia数组  
     *List<Integer> list2 = new ArrayList<Integer>(Array.asList(ia));//对list2的排序不会影响底层ia数组  
51.异常处理通常有两种理论模型：终止与恢复。但通常采用终止方法，因为恢复方法需要知道异常抛出地点信息，在大型程序中会增大代码编写和维护的困难。  
52.异常说明(必检异常)形如void f() throws Exception1, Exception2{...}，在程序中手动/再次抛出异常使用throw ex1;。声明抛出异常的方法可以不抛出异常，只为占位。异常说明可能有两种意思：一是代码可能产生某些异常需要使用该方法的方法处理，二是代码忽略这些异常，这些异常由方法调用者处理。  
53.异常栈轨迹的栈顶是调用序列的最后一个方法，栈底是调用序列的第一个方法（通常为main()）。  
54.如果把当前异常对象重新抛出，则printStackTrace()方法输出的仍然是原来抛出点的的信息，如果对当前异常对象调用fillInStackTrace()方法并抛出其返回的Throwable对象（Exception实现了Throwable接口），则异常栈顶会改写成当前方法。  
55.捕获异常对象后生成一个与之前异常无关的新异常对象，则与fillInStackTrace()效果类似。如果使用旧一场对象构造新异常对象，则构成异常链。  
56.RuntimeException及其任何派生类都属于免检异常，如果在异常的抛出全过程中都没有捕获免检异常的代码，则到达main()程序退出时会调用printStackTrace()。  
57.finally块应用的场合与finalize()不同，用于清理除内存外的其他系统资源占用，如已打开的文件和网络连接等。  
58.需要注意构造器中的异常处理，在处理完之后应该再次抛出以保证不会误导使用者。  
59.在生成需要清理对象阶段应该是try嵌套块来保证对象未生成抛出异常/对象生成了总是能被清理，如  
     try{  
          // create file  
          try{  
               // do something  
          }catch(Exception e){  
               // exception  
          }finally{  
               // close file operation  
          }  
     }catch(Exception e){  
          // create file failed  
     }  
60.注意catch块的书写顺序，防止屏蔽现象。  
61.对于多次字符串累加操作，最好使用java.lang.StringBuilder来提高性能。如果需要线程安全特性，使用StringBuffer。  
62.重写toString()要小心无意识递归调用。String类的多数方法在没有改变原对象的情况下会返回原对象引用，改变了则返回新对象的引用。  
63.如果需要格式化输出，System.out.format()和System.out.printf()都可以提供C风格的格式输出，使用String.format()可以用C风格构造一个String对象。%(+-)width.precision type，正负号表示右对齐/左对齐，width表示输出宽度，precision表示输出精度，type表示输出类型。  
64.Java使用Class对象来执行其RTTI，每个.class文件都会有一个对应该类的Class对象，所有的类都是对其第一次使用时动态加载到JVM中的。原生类加载器会首先加载可信类（如Java API类）。当使用一个类时，类加载器首先检查这个类的Class对象是否已经加载，如果尚未加载，默认的类加载器就会根据类名查找.class文件  
65.获取Class对象引用的两种方式：Class.forName()和A.class，前者是Class的静态方法，会触发类的加载；后者是类A的static和final的编译器常量，不会触发类的加载。  
66.可以使用classobj.newInstance()来生成一个新的对象，注意该类必须具有默认构造函数。如果classobj是一个Class<Type>对象，则newInstance可以返回一个Type对象，如果classobj是一个Class对象，则返回一个Object对象。  
67.RTTI的另一个种形式是使用instanceof关键字，语法 obj instance classname ，返回值为boolean。instanceof方式检测了所有可能，包括当前类及其派生类，而使用Class对象比较仅检测了当前类是否相等的情况。  
68.反射也必须建立在JVM能够获知当前处理的对象的类型的基础之上，Class与java.lang.reflect类库一起对反射的概念进行了支持，该类库包含了、Method已经Constructor类（每个类都实现了Member接口）  
69.RTTI和反射之间真正的区别只在于，RTTI是编译器在编译时打开和检查.class，反射是.class在编译时是不可获取，运行时打开和检查文件。  
70.RTTI带来的副作用之一是破坏了API提供者想通过Interface来控制客户端程序员访问权限的机会，客户端程序员可以通过向下转型访问到本不属于接口内的方法，使得这部分的API也成为事实上的公共接口，增大了维护成本。  
71.动态代理：在动态代理上所做的所有调用都会被重定向到单一的调用处理器上，它的工作是解释调用的类型并确定相应的对策。原理总结：  
     *代理类和被代理类实现同一个（或多个）接口InterfaceX  
     *DynamicProxyHandler类实现接口InvocationHandler，并持有一个被代理类的对象，主要通过invoke()方法来联结代理对象和被代理对象  
     *java.lang.reflect.Proxy类的newProxyInstance()方法可以动态生成代理对象，该方法的三个参数分别是类加载器，接口列表和Handler对象  
     *e.g. InterfaceX proxy = (InterfaceX)Proxy.newProxyInstance(InterfaceX.class.getClassLoader(),new Class[]  {InterfaceX.class}, new         DynamicProxyHandler(proxied));  
72.泛型类与泛型方法：  
     *是否是泛型类和是否有泛型方法没有必然联系，但static方法要使用泛型能力必须使自己成为泛型方法  
     *使用泛型类时必须显式的指定类型参数，而使用泛型方法则不用，编译器会自动进行参数类型推断  
     *泛型类只需要在类名后用<>指定参数类型，泛型方法除了在参数/返回值里使用<>指定参数类型，而且在返回值之前也要用<>指定参数类型  
73.泛型擦除：在泛型代码内部，无法获得任何有关泛型参数类型的信息，在使用泛型时任何具体类型信息都被擦除，List<String>和List<Integer>在运行时事实上是相同的类型，都被擦除成相同类型List，尽管如此，编译器可以确保方法或类中使用的类型的内部一致性。  
     *Class c1 = new ArrayList<String>().getClass();  
     *Class c2 = new ArrayList<Integer>().getClass();  
     *c1 == c2 为 true  
74.无法在对泛型进行的操作：instanceof，new，转型（将T转型为其他类型），因为类型擦除导致类型信息丢失，但可以使用Class<T>对象用newInstance()方法创建对象。创建数组可以通过 T[] array = (T[])new Object[size];进行，切记array在运行时类型只能是Object[]  
75.Arrays使用方法，equals()比较相等，fill()填充，sort()排序，binarySearch()在排序数组中查找，toString()，hashcode()。此外还有System.arraycopy()，这种拷贝方式比用for快很多。  
76.通过实现java.lang.Comparable接口可以让容器和数组的sort()对自定义类进行排序，需要实现campareTo()方法  
77.Object使用对象的地址作为hashcode()和equals ()的默认实现。如果要使用自己的类作为HashMap的键，必须同时重载hashCode()和equals()方法。  
78.equals()决定了在链式冲突中寻找唯一对象的标准。equals()的重载方法  
     public boolean equals(Object o){  
          return o instance of Type && (member1 == ((Type)o).member1)&&..&&(memberk == ((Type)o).memberk)  
     }  
79.hashcode()的重载方法  
     public int hashCode(){  
          int result = 17;  
          result = result * 37 + member1.hashCode();  
          result = result * 37 + c(member2);  
          result = result * 37 + c(memberk);  
     }  
     其中c()是对基本类型的整数化操作。  
85.File类不仅仅指单一文件名称，也可能是一个文件集合  
86.System.out和System.err已经被包装成PrintStream对象，因此可以直接输出，但System.in还是原始的InputStream，因此需要输入包装类才能使其正常工作，如BufferReader in = new BufferReader(new InputStreamReader(System.in));  
87.System.setIn(InputStream), System.setOut(PrintStream)和System.setErr(PrintStream)可以重定向标准输入/输出流  
88.对于文件流对象，可以调用FileLock fl = ((FileOuputStream)fos).getChannel().tryLock();注意tryLock()是非阻塞方法，而lock()是阻塞方法，release()可以释放锁。  
89. 一般的序列化需求就实现Serializable接口，如果是需要自己控制序列化过程则实现Externalizable接口。transient关键字可以用于不想被序列化的类成员之前防止被序列化。  
90.Java的线程机制是抢占式的（但协作式有着更小的代价）。实现一个线程类  
     public class A implements Runnable{  
          // Constructer omit  
          public void run(){  
               // Do something  
               Thread.yeild();//通知线程调度器自己任务已经完成（可选）  
          }  
     }  
但是一个实现Runnable接口的类并不具有产生线程的能力，要实现线程行为，必须显式地将一个任务附在线程上。具体做法是使用Runnable对象作为参数生成一个Tread对象，再调用Thread对象的start()方法，该方法是一个非阻塞方法，启动线程后立即返回。如果直接调用Runnable的run()方法，则会阻塞调用线程(如main线程)。  
91.Java提供线程池ExecutorService类，创建一个线程池的代码如下  
     ExecutorService exec1 = Executors.newCachedTreadPool();//创建一个大小可变的线程池，合理的首选  
     ExecutorService exec2 = Executors.newFixedTreadPool(5);//创建一个固定大小的线程池，一次性执行线程分配，效率高  
     ExecutorService exec3 = Executors.newSingleTreadPool();//创建大小为1的线程池，可以提交多个线程，但线程会排队，用于socket监听类任务  
     提交线程任务  
     exec1.execute(new A());//A是90中提到的Runnable实现类  
     阻止一个线程池继续工作的方法  
     exec1.shutdown();//无法继续提交新的线程任务，旧的会继续执行完毕  
92.通过Tread.currentThread()可以获得当前执行线程的对象引用。JDK有10个进程优先级，而Windows有7个优先级且不是固定的。  
93.线程分为后台线程和非后台线程，后台线程往往是提供通用服务的线程，不属于不可获取的部分，非后台线程反之，如main线程。当所有非后台线程结束工作时，程序终止，后台线程会立即被终止，即使finally也不能得到保证（非后台线程的finally一定会执行）。  
94.通过把方法标记为synchronized来防止资源冲突，如方法f()和g()都会对某个关键资源操作，则加上synchronized关键字  
     synchronized void f(){}  
     synchronized void g(){}  
所有对象都含有单一的锁，对于某个特定的对象，所有的synchronized方法共享一个锁，事实上等同synchronized(this)，一个synchronized方法被调用，其他线程便不能调用其他synchronized方法。在使用并发时，将域设置为private非常重要，否则synchronized关键字不能防止其他任务直接访问域，进而产生冲突。  
95.JVM负责跟踪对象的加锁次数，一个任务可以多次获得对象的锁。如果一个方法又调用了自身的一个同步方法，则加速次数加1，一个对象被解锁当且仅当其加锁计数减为0。针对每个类也有一个锁，对static方法适用synchronized关键字可以防止静态方法冲突。  
96.可以使用synchronized(otherObject)来实现互斥，但要保证所有使用同一个关键资源的代码块在同一个对象上加锁。  
97.使用java.util.concurrent.locks类实现互斥，相比使用synchronized关键字能够实现更加灵活，粒度更细的加锁  
     Lock lock = new ReentrantLock();  
     lock.lock();  
     try{  
          // Do something  
          return;  
     }finally{  
          lock.unlock();  
     }  
98.volatile关键字可以让域得到立即更新，因而同步性得到一定保障，如果一个域完全被synchronized包括，则不必使用volatile。  
99.线程状态：  
     *新建(new)：已被分配资源且初始化，等待调度  
     *就绪(runnable)：只要调度就可运行  
     *阻塞(blocked)：因某个条件被阻止运行，调度器忽略此线程  
     *死亡(dead)：不可运行  
100.造成阻塞的原因包括  
     *sleep()休眠，可以中断  
     *wait()挂起,等待notify()或notifyAll()方法，在挂起期间锁会被释放  
     *等待IO，不可中断  
     *试图获得锁但未成功，不可中断  
101.向持有某对象锁的所有线程发送通知  
     synchronized(x){  
          x.notifyAll();//所有等待线程竞争该锁  
          //x.notify; 仅通知单一线程来获取该锁  
     }  
102.生产者消费者代码示例见Thinking in java P709  
103.如果使用BlockingQueue和PriorityBlockingQueue可以直接使用put()和take()方法而不同担心手动同步问题，容器已经在内部同步。  
104.产生死锁的四个必要条件  
     *互斥条件，即使用的资源中至少有一个不能共享  
     *等待，即至少有一个线程掌握一部分资源的同时等待另外一些资源  
     *资源不能被抢占，即只能等待别人释放  
     *循环等待  
只需要破坏一个即可阻止死锁  
105.正常的锁（来自concurrent.locks或内建的synchronized锁）在任何时刻都只允许一个任务访问一项资源，而Semaphore则允许n个线程同时访问这个资源  
     Semaphore sem = new Semaphore(size, true);  
     sem.acquire();  
     sem.release();  
106.线程相对与进程更加轻量级，切换仅需要100条左右的指令，而进程需要上千条，同时进程需要切换上下文，而线程只需要改变程序的执行序列和局部变量。  
107.线程的缺陷：  
     *等待临界资源时效率降低  
     *处理线程带来的额外CPU开销  
     *增加了程序的复杂度  
     *有可能产生病态行为，如饿死，竞争，死锁，活锁（多个线程各自运行导致无法完成整体任务）等  
     *平台不一致性  

注：由于时间原因和过于复杂的内容跳过泛型一章P389页之后的内容、枚举类型一章，注解一章的内容，以后补完。  

