## ClassLoader加载流程及作用

#### ClassLoader是用来加载class文件到JVM以供程序使用
* java程序可以动态加载类定义，而这个动态加载的机制就是通过ClassLoader来实现的
* 当运行一个程序的时候，JVM启动，运行bootstrap classloader, 该ClassLoader加载核心的API（ExtClassLoader和AppCLassLoader也在此时被加载），然后ExtClassLoader加载扩展API，最后AppClassLoader加载CLASSPATH目录下定义的Class，这就是程序的基本流程
* bootstrap classloader（启动类加载器）: 不是java语言编写而是JVM实现的一部分,在JVM运行的时候加载java核心的API以满足java程序的最基本需求
* ExtClassLoader:java程序实现的，用户定义的ClassLoader，用来加载Java的扩展API也就是/lib/ext中的类
*AppCladdLoader:java程序实现的，用户定义的ClassLoader，用来加载用户机器上CLASSPATH设置目录中的calss，通常在没有指定ClassLoader的情况下，用户自定义的类就由该ClassLoader进行加载


#### ClassLoader加载方式
* ClassLoader使用双亲委托模式进行类加载
* 每个自定义ClassLoader都必须继承ClassLoader这个抽象类，而没个ClassLoader都会有一个parentClassLoader， ClassLoader这个抽象类中有一个getParent()方法，这个方法用来返回当前ClassLoader的parent，这里需要注意，这个parent不是指被继承的类，而是在是实例化该ClassLoader时指定的一个ClassLoader，如果这个parent为null，那么就默认该ClassLoader的parent为bootstrap classloader 在任何一个自定义ClassLoader加载一个类之前，都会先委托它的父类ClassLoader进行加载，只有当父ClassLoader无法加载成功的时候，才会由自己加载，比如：java.lang.String是属于java核心API的一个类，当使用用户自定义类加载它的时候，该ClassLoader会先委托父ClassLoader进行加载，上面说过当ClassLoader的parent为null ClassLoader的parent为bootstrap classloader，所以ClassLoader的最顶层就是bootstrap classloader，因此最终委托到bootstrap classloader的时候，bootstrap classloader就会返回String的Class

#### 为什么要使用双亲委托模式
* 使用双亲委托模式的原因由两点
（1）避免重复加载，当父类已经加载该类的时候，就没必要子类在加载一次
（2）考虑到安全因素，如果不使用双亲委托模式，那我们就可以随时使用自定义的String来动态替代java核心API中定义的类


#### JVM加载类的三步骤
* 1.装载：找到相应的class文件读入JVM
* 2.连接：
（1）验证class是否符合规则
（2）准备 为变量分配内存的同时设置默认初始值
（3）解释 可选 根据loadClass方法第二个参数判断是否需要解释，根据类中的符号引用查找相应的实体，在把符号引用替换成一个直接引用的过程
* 3.初始化：初始化


