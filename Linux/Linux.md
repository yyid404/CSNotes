## Linux

### 命令

#### 用户/组

**$**：普通管理员

**\#**：超级管理员

**su 用户名**：切换用户

**groupadd 组名**：创建组

**useradd 用户名 -g 组名**：创建用户同时加入组

**usermod -g 组名 用户名**：修改用户所在主组

**chmod [u/g/o] [+/-] [r/w/x]**：修改文件权限。相关参数：

- u：文件拥有者
- g：文件拥有者所在的组
- o：其他用户
- +：增加权限
- -：删除权限
- r：读权限
- w：写权限
- x：执行权限

**ssh [-p portId] user@host**：以指定用户远程登录到指定主机；若本地用户名与远程用户名一致，登录时可以省略用户名；可以添加-p portId指定登录请求发送到的远程主机端口号，默认为22。

#### 编辑

**pwd**：显示当前目录

**tree**：树形结构显示目录，需安装tree包

**ls**：显示文件或目录。相关参数：

- -l：列出文件详细信息

- -a：列出当前目录下包含隐藏的所有文件及目录

**stat**：显示指定文件的超详细信息

**cd ~**：进入home下当前用户的文件夹

**cd**：切换到指定文件夹

**find**：在当前查找某文件

**cat**：查看文件内容

**more/less**：分页显示文本文件内容

**head/tail**：显示文件头、尾内容

**grep**：在文本文件中查找某个关键词

**wc**：统计文本中行数、字符、字符数

**touch** 文件名：创建空文件

**echo**：创建带有内容的文件

**ctrl+insert**：复制

**cp**：复制

**shift+insert**：粘贴

**rm**：删除。相关参数：

- -r：递归删除子目录及文件

- -f：强制删除

**mv**：移动

**mkdir**：创建文件夹。相关参数：

- -p：若无父目录，则创建

**rmdir**：删除空文件夹。相关参数：

- -p：递归删除空的父目录

**esc+q**：退出当前输入

**clear**：清屏

#### 压缩

**tar**：压缩

1. 必须选一
    1. -c：创建压缩档案（只打包不压缩）
    2. -x：解压缩 
    3. -t：查看内容
    4. -r：向压缩归档文件末尾追加文件

2. 压缩方式
    1. -z：使用gzip压缩
    2. -j：使用bzip2压缩

3. -v：显示压缩/解压缩过程

4. -f：打包成指定文件名

#### 网络

**ifconfig**：查看网络情况

**netstat**：显示网络状态信息

**ping**：测试连通性

#### 进程

**kill 端口号**：杀死进程。相关参数：

- -9 端口号：彻底杀死进程

**ps**：ps命令：提供系统过去进程信息的一次性快照。相关参数：

- ps H -eo user,pid,ppid,tid,time,%cpu,cmd --sort=%cpu 打印用户、进程id、父进程id、线程id、运行时间、CPU使用率、启动命令，并按CPU使用率排序。
- ps -ef | grep 关键字：查看相关进程

**top**：反应的是系统进程动态信息，默认10s更新一次。ps和top都是从/proc目录下读取进程的状态信息，内核把当前系统进程的各种有用信息都放在这个伪目录下。

- VIRT 虚拟内存用量
- RES 物理内存用量
- SHR 共享内存用量
- %MEM 内存用量

**jskack**：用于打印出给定的java进程ID或core file或远程调试服务的Java堆栈信息。

- 如果java程序崩溃生成core文件，jstack工具可以用来获得core文件的java stack和native stack的信息；

- 如果要排查正在运行的程序：
    1. top查看进程，找出pid 
    2. top -H -p PID查看该进程下的线程，找出tid
    3. 因为pid展示的thread _id是十进制的，在jstack文件里是16进制的，所以通过printf “0x%x” tid将线程id进行16进制转换。
    4. 运行jstack 进程id（十进制）> stack.log，将堆栈内容打印重定向到stack.log中。
    5. 使用vim stack.log进行查看，或者grep 关键词 文件名，搜索nid=线程号（16进制）。


>例如：可以看到s0、s1、eden、old、metaspace都已经爆了，且FGC次数一直在增加，但是却没有回收到任何空间，导致FGC一直在跑，进入循环，涉及回内存泄漏。

### 项目部署

​		打成jar包，将项目target目录下面的项目jar包，拷贝到linux环境的要部署的目录下，将要使用的配置文件同样拷贝在这一目录下（可以指定另一个目录），并修改相应的配置参数的值，执行nohup java -jar 项目.jar application.yml & 命令。相关参数：

- &：后台运行 关闭控制台不影响

- nohup：永久运行 退出shell也不影响

### vim编辑器

**esc**：切到命令行模式

**:**：切换到底行模式

**a/i/o**：在命令行模式下切换到插入模式

**:wq**：保存文件并退出

**:wq 文件名**：保存为指定文件并退出

**:q!**：不保存文件并强制退出

**dd**：删除

**yy**：复制

**pp**：粘贴

**u**：撤销

**\>**：重定向

**\>>**：追加内容重定向

**<< xxx**：从键盘输入内容到文件，以xxx结束输入

### 定位Java性能问题

​		如果是Java应用出现了问题，看日志，看是什么地方抛出了什么异常。

#### 工具

**top命令**：top 命令使我们最常用的Linux命令之一，它可以实时的显示当前正在执行的进程的CPU使用率，内存使用率等系统信息。`top -Hp pid` 可以查看线程的系统资源使用情况。

**vmstat命令**：vmstat是一个指定周期和采集次数的虚拟内存检测工具，可以统计内存，CPU，swap的使用情况，它还有一个重要的常用功能，用来观察进程的上下文切换。字段说明如下：

- r: 运行队列中进程数量（当数量大于CPU核数表示有阻塞的线程）
- b: 等待IO的进程数量
- swpd: 使用虚拟内存大小
- free: 空闲物理内存大小
- buff: 用作缓冲的内存大小(内存和硬盘的缓冲区)
- cache: 用作缓存的内存大小（CPU和内存之间的缓冲区）
- si: 每秒从交换区写到内存的大小，由磁盘调入内存
- so: 每秒写入交换区的内存大小，由内存调入磁盘
- bi: 每秒读取的块数
- bo: 每秒写入的块数
- in: 每秒中断数，包括时钟中断。
- cs: 每秒上下文切换数。
- us: 用户进程执行时间百分比(user time)
- sy: 内核系统进程执行时间百分比(system time)
- wa: IO等待时间百分比
- id: 空闲时间百分比

**pidstat命令**：pidstat 是 Sysstat 中的一个组件，也是一款功能强大的性能监测工具，`top` 和 `vmstat` 两个命令都是监测进程的内存、CPU 以及 I/O 使用情况，而 pidstat 命令可以检测到线程级别的。`pidstat`命令线程切换字段说明如下：

- UID ：被监控任务的真实用户ID。

- TGID ：线程组ID。

- TID：线程ID。

- cswch/s：主动切换上下文次数，这里是因为资源阻塞而切换线程，比如锁等待等情况。

- nvcswch/s：被动切换上下文次数，这里指CPU调度切换了线程。

**jstack命令**：jstack是JDK工具命令，它是一种线程堆栈分析工具，最常用的功能就是使用 `jstack pid` 命令查看线程的堆栈信息，也经常用来排除死锁情况。

**jstat 命令**：它可以检测Java程序运行的实时情况，包括堆内存信息和垃圾回收信息，我们常常用来查看程序垃圾回收情况。常用的命令是`jstat -gc pid`。信息字段说明如下：

- S0C：年轻代中 To Survivor 的容量（单位 KB）；

- S1C：年轻代中 From Survivor 的容量（单位 KB）；

- S0U：年轻代中 To Survivor 目前已使用空间（单位 KB）；

- S1U：年轻代中 From Survivor 目前已使用空间（单位 KB）；

- EC：年轻代中 Eden 的容量（单位 KB）；

- EU：年轻代中 Eden 目前已使用空间（单位 KB）；

- OC：老年代的容量（单位 KB）；

- OU：老年代目前已使用空间（单位 KB）；

- MC：元空间的容量（单位 KB）；

- MU：元空间目前已使用空间（单位 KB）；

- YGC：从应用程序启动到采样时年轻代中 gc 次数；

- YGCT：从应用程序启动到采样时年轻代中 gc 所用时间 (s)；

- FGC：从应用程序启动到采样时 老年代（Full Gc）gc 次数；

- FGCT：从应用程序启动到采样时 老年代代（Full Gc）gc 所用时间 (s)；

- GCT：从应用程序启动到采样时 gc 用的总时间 (s)。

**jmap命令**：jmap也是JDK工具命令，他可以查看堆内存的初始化信息以及堆内存的使用情况，还可以生成dump文件来进行详细分析。查看堆内存情况命令`jmap -heap pid`。

**mat内存工具**：MAT(Memory Analyzer Tool)工具是eclipse的一个插件(MAT也可以单独使用)，它分析大内存的dump文件时，可以非常直观的看到各个对象在堆空间中所占用的内存大小、类实例数量、对象引用关系、利用OQL对象查询，以及可以很方便的找出对象GC Roots的相关信息。

**idea的JProfiler插件**

#### CPU占满

若CPU负载飙高，一般出现了死循环。解决方案有：

- 优化程序内部的线程使用，确保无冗余的线程配置；
- 增加虚拟机或容器的CPU配置，提升机器总的计算能力。

直接写一个死循环计算消耗CPU，模拟CPU占满：

> ```java
> //模拟CPU占满
> @GetMapping("/cpu/loop")
> public void testCPULoop() throws InterruptedException {
> 	System.out.println("请求cpu死循环");
> 	Thread.currentThread().setName("loop-thread-cpu");
>  int num = 0;
>  while (true) {
>      num++;
>      if (num == Integer.MAX_VALUE) {
>          System.out.println("reset");
>      }
>      num = 0;
>  }
> }
> ```
>
> 请求接口地址测试`curl localhost:8080/cpu/loop`,发现CPU立马飙升到100%
>
> ![](E:/CS/GitHub/JavaGuide-master/docs/java/images/performance-tuning/java-performance1.png)
>
> 通过执行`top -Hp 32805` 查看Java线程情况：
>
> ![](E:/CS/GitHub/JavaGuide-master/docs/java/images/performance-tuning/java-performance2.png)
>
> 执行 `printf '%x' 32826` 获取16进制的线程id，用于`dump`信息查询，结果为 `803a`。最后我们执行`jstack 32805 |grep -A 20 803a `来查看下详细的`dump`信息：
>
> ![](E:/CS/GitHub/JavaGuide-master/docs/java/images/performance-tuning/java-performance3.png)
>
> 这里`dump`信息直接定位出了问题方法以及代码行，这就定位出了CPU占满的问题。

#### 内存溢出/泄漏

​		若内存溢出，如果服务挂了，使用JDK自带的命令行工具jmap；服务没挂，使用jstack，把JVM的内存结构dump下来，得到一个dump格式的文件。使用JDK自带的visualvm工具打开，看看哪些对象占用内存较高，若同一个类型的对象占用内存较高，可以认为该对象在不断的创建，回去找到相应代码改掉即可。若整个内存分配很正常，则只是单纯的内存不够，此时可以考虑优化JVM的启动参数，如-Xmx。或者发现mirrorGC或majorGC很频繁，此时可以调节JVM young区和old区的比例，或者调整JVM垃圾回收的暂停时间。

​		模拟内存泄漏借助了ThreadLocal对象来完成，ThreadLocal是一个线程私有变量，可以绑定到线程上，在整个线程的生命周期都会存在，但是由于ThreadLocal的特殊性，ThreadLocal是基于ThreadLocalMap实现的，ThreadLocalMap的Entry继承WeakReference，而Entry的Key是WeakReference的封装，换句话说Key就是弱引用，弱引用在下次GC之后就会被回收，如果ThreadLocal在set之后不进行后续的操作，因为GC会把Key清除掉，但是Value由于线程还在存活，所以Value一直不会被回收，最后就会发生内存泄漏。

> ```java
> //模拟内存泄漏
> @GetMapping(value = "/memory/leak")
> public String leak() {
>  System.out.println("模拟内存泄漏");
>  ThreadLocal<Byte[]> localVariable = new ThreadLocal<Byte[]>();
>  localVariable.set(new Byte[4096 * 1024]);// 为线程添加变量
>  return "ok";
> }
> ```
>
> 我们给启动加上堆内存大小限制，同时设置内存溢出的时候输出堆栈快照并输出日志。
>
> ```
> java -jar -Xms500m -Xmx500m -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/heapdump.hprof  -XX:+PrintGCTimeStamps -XX:+PrintGCDetails -Xloggc:/tmp/heaplog.log analysis-demo-0.0.1-SNAPSHOT.jar
> ```
>
> 启动成功后我们循环执行100次，`for i in {1..500}; do curl localhost:8080/memory/leak;done`，还没执行完毕，系统已经返回500错误了。查看系统日志出现了如下异常：
>
> ```
> java.lang.OutOfMemoryError: Java heap space
> ```
>
> 我们用`jstat -gc pid` 命令来看看程序的GC情况。
>
> ![](E:/CS/GitHub/JavaGuide-master/docs/java/images/performance-tuning/java-performance4.png)
>
> 很明显，内存溢出了，堆内存经过45次 Full Gc 之后都没释放出可用内存，这说明当前堆内存中的对象都是存活的，有GC Roots引用，无法回收。那是什么原因导致内存溢出呢？是不是我只要加大内存就行了呢？如果是普通的内存溢出也许扩大内存就行了，但是如果是内存泄漏的话，扩大的内存不一会就会被占满，所以我们还需要确定是不是内存泄漏。我们之前保存了堆 Dump 文件，这个时候借助我们的MAT工具来分析下。导入工具选择`Leak Suspects Report`，工具直接就会给你列出问题报告。
>
> ![](E:/CS/GitHub/JavaGuide-master/docs/java/images/performance-tuning/java-performance5.png)
>
> 这里已经列出了可疑的4个内存泄漏问题，我们点击其中一个查看详情。
>
> ![](E:/CS/GitHub/JavaGuide-master/docs/java/images/performance-tuning/java-performance6.png)
>
> 这里已经指出了内存被线程占用了接近50M的内存，占用的对象就是ThreadLocal。如果想详细的通过手动去分析的话，可以点击`Histogram`,查看最大的对象占用是谁，然后再分析它的引用关系，即可确定是谁导致的内存溢出。
>
> ![](E:/CS/GitHub/JavaGuide-master/docs/java/images/performance-tuning/java-performance7.png)
>
> 上图发现占用内存最大的对象是一个Byte数组，我们看看它到底被那个GC Root引用导致没有被回收。按照上图红框操作指引，结果如下图：
>
> ![](E:/CS/GitHub/JavaGuide-master/docs/java/images/performance-tuning/java-performance8.png)
>
> 我们发现Byte数组是被线程对象引用的，图中也标明，Byte数组对像的GC Root是线程，所以它是不会被回收的，展开详细信息查看，我们发现最终的内存占用对象是被ThreadLocal对象占据了。这也和MAT工具自动帮我们分析的结果一致。

#### 死锁

​		死锁会导致耗尽线程资源，占用内存，表现就是内存占用升高，CPU不一定会飙升(看场景决定)，如果是直接new线程，会导致JVM内存被耗尽，报无法创建线程的错误，这也是体现了使用线程池的好处。

> ```java
> ExecutorService service = new ThreadPoolExecutor(4, 10,
>          0, TimeUnit.SECONDS, new LinkedBlockingQueue<Runnable>(1024),
>          Executors.defaultThreadFactory(),
>          new ThreadPoolExecutor.AbortPolicy());
>  //模拟死锁
>  @GetMapping("/cpu/test")
>  public String testCPU() throws InterruptedException {
>      System.out.println("请求cpu");
>      Object lock1 = new Object();
>      Object lock2 = new Object();
>      service.submit(new DeadLockThread(lock1, lock2), "deadLookThread-" + new Random().nextInt());
>      service.submit(new DeadLockThread(lock2, lock1), "deadLookThread-" + new Random().nextInt());
>      return "ok";
>  }
> 
> public class DeadLockThread implements Runnable {
>  private Object lock1;
>  private Object lock2;
> 
>  public DeadLockThread1(Object lock1, Object lock2) {
>      this.lock1 = lock1;
>      this.lock2 = lock2;
>  }
> 
>  @Override
>  public void run() {
>      synchronized (lock2) {
>          System.out.println(Thread.currentThread().getName()+"get lock2 and wait lock1");
>          try {
>              TimeUnit.MILLISECONDS.sleep(2000);
>          } catch (InterruptedException e) {
>              e.printStackTrace();
>          }
>          synchronized (lock1) {
>              System.out.println(Thread.currentThread().getName()+"get lock1 and lock2 ");
>          }
>      }
>  }
> }
> ```
>
> 我们循环请求接口2000次，发现不一会系统就出现了日志错误，线程池和队列都满了,由于我选择的当队列满了就拒绝的策略，所以系统直接抛出异常。
>
> ```
> java.util.concurrent.RejectedExecutionException: Task java.util.concurrent.FutureTask@2760298 rejected from java.util.concurrent.ThreadPoolExecutor@7ea7cd51[Running, pool size = 10, active threads = 10, queued tasks = 1024, completed tasks = 846]
> ```
>
> 通过`ps -ef|grep java`命令找出 Java 进程 pid，执行`jstack pid` 即可出现java线程堆栈信息，这里发现了5个死锁，我们只列出其中一个，很明显线程`pool-1-thread-2`锁住了`0x00000000f8387d88`等待`0x00000000f8387d98`锁，线程`pool-1-thread-1`锁住了`0x00000000f8387d98`等待锁`0x00000000f8387d88`,这就产生了死锁。
>
> ```JAVA
> Java stack information for the threads listed above:
> ===================================================
> "pool-1-thread-2":
>      at top.luozhou.analysisdemo.controller.DeadLockThread2.run(DeadLockThread.java:30)
>         - waiting to lock <0x00000000f8387d98> (a java.lang.Object)
>         - locked <0x00000000f8387d88> (a java.lang.Object)
>         at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
>         at java.util.concurrent.FutureTask.run(FutureTask.java:266)
>         at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
>         at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
>         at java.lang.Thread.run(Thread.java:748)
> "pool-1-thread-1":
>         at top.luozhou.analysisdemo.controller.DeadLockThread1.run(DeadLockThread.java:30)
>         - waiting to lock <0x00000000f8387d88> (a java.lang.Object)
>         - locked <0x00000000f8387d98> (a java.lang.Object)
>         at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
>         at java.util.concurrent.FutureTask.run(FutureTask.java:266)
>         at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
>         at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
>         at java.lang.Thread.run(Thread.java:748)
>           
>  Found 5 deadlocks.
> ```

#### 线程频繁切换

​		上下文切换会导致将大量CPU时间浪费在寄存器、内核栈以及虚拟内存的保存和恢复上，导致系统整体性能下降。当你发现系统的性能出现明显的下降时候，需要考虑是否发生了大量的线程上下文切换。

> ```java
> //模拟线程切换
> @GetMapping(value = "/thread/swap")
>  public String theadSwap(int num) {
>      for (int i = 0; i < num; i++) {
>          new Thread(new ThreadSwap1(new AtomicInteger(0)),"thread-swap"+i).start();
>      }
>      return "ok";
>  }
> public class ThreadSwap1 implements Runnable {
>  private AtomicInteger integer;
> 
>  public ThreadSwap1(AtomicInteger integer) {
>      this.integer = integer;
>  }
> 
>  @Override
>  public void run() {
>      while (true) {
>          integer.addAndGet(1);
>          Thread.yield(); //让出CPU资源
>      }
>  }
> }
> ```
>
> 这里我创建多个线程去执行基础的原子+1操作，然后让出 CPU 资源，理论上 CPU 就会去调度别的线程，我们请求接口创建100个线程看看效果如何，`curl localhost:8080/thread/swap?num=100`。接口请求成功后，我们执行`vmstat 1 10，表示每1秒打印一次，打印10次，线程切换采集结果如下：
>
> ```
> procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
> r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
> 101  0 128000 878384    908 468684    0    0     0     0 4071 8110498 14 86  0  0  0
> 100  0 128000 878384    908 468684    0    0     0     0 4065 8312463 15 85  0  0  0
> 100  0 128000 878384    908 468684    0    0     0     0 4107 8207718 14 87  0  0  0
> 100  0 128000 878384    908 468684    0    0     0     0 4083 8410174 14 86  0  0  0
> 100  0 128000 878384    908 468684    0    0     0     0 4083 8264377 14 86  0  0  0
> 100  0 128000 878384    908 468688    0    0     0   108 4182 8346826 14 86  0  0  0
> ```
>
> 这里我们关注4个指标，`r`,`cs`,`us`,`sy`。
>
> r=100，说明等待的进程数量是100，线程有阻塞。
>
> cs=800多万，说明每秒上下文切换了800多万次，这个数字相当大了。
>
> us=14，说明用户态占用了14%的CPU时间片去处理逻辑。
>
> sy=86，说明内核态占用了86%的CPU，这里明显就是做上下文切换工作了。
>
> 我们通过`top`命令以及`top -Hp pid`查看进程和线程CPU情况，发现Java线程CPU占满了，但是线程CPU使用情况很平均，没有某一个线程把CPU吃满的情况。
>
> ```
> PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                                                                            
> 87093 root      20   0 4194788 299056  13252 S 399.7 16.1  65:34.67 java 
> ```
>
> ```
> PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND                                                                             
> 87189 root      20   0 4194788 299056  13252 R  4.7 16.1   0:41.11 java                                                                                
> 87129 root      20   0 4194788 299056  13252 R  4.3 16.1   0:41.14 java                                                                                
> 87130 root      20   0 4194788 299056  13252 R  4.3 16.1   0:40.51 java                                                                                
> 87133 root      20   0 4194788 299056  13252 R  4.3 16.1   0:40.59 java                                                                                
> 87134 root      20   0 4194788 299056  13252 R  4.3 16.1   0:40.95 java 
> ```
>
> 结合上面用户态CPU只使用了14%，内核态CPU占用了86%，可以基本判断是Java程序线程上下文切换导致性能问题。
>
> 我们使用`pidstat`命令来看看Java进程内部的线程切换数据，执行`pidstat -p 87093 -w 1 10 `,采集数据如下：
>
> ```
> 11:04:30 PM   UID       TGID       TID   cswch/s nvcswch/s  Command
> 11:04:30 PM     0         -     87128      0.00     16.07  |__java
> 11:04:30 PM     0         -     87129      0.00     15.60  |__java
> 11:04:30 PM     0         -     87130      0.00     15.54  |__java
> 11:04:30 PM     0         -     87131      0.00     15.60  |__java
> 11:04:30 PM     0         -     87132      0.00     15.43  |__java
> 11:04:30 PM     0         -     87133      0.00     16.02  |__java
> 11:04:30 PM     0         -     87134      0.00     15.66  |__java
> 11:04:30 PM     0         -     87135      0.00     15.23  |__java
> 11:04:30 PM     0         -     87136      0.00     15.33  |__java
> 11:04:30 PM     0         -     87137      0.00     16.04  |__java
> ```
>
> 根据上面采集的信息，我们知道Java的线程每秒切换15次左右，正常情况下，应该是个位数或者小数。结合这些信息我们可以断定Java线程开启过多，导致频繁上下文切换，从而影响了整体性能。
>
> 为什么系统的上下文切换是每秒800多万，而 Java 进程中的某一个线程切换才15次左右？
>
> 系统上下文切换分为三种情况:
>
> - 多任务：在多任务环境中，一个进程被切换出CPU，运行另外一个进程，这里会发生上下文切换。
>
>
> - 中断处理：发生中断时，硬件会切换上下文。在vmstat命令中是`in`
>
>
> - 用户和内核模式切换：当操作系统中需要在用户模式和内核模式之间进行转换时，需要进行上下文切换,比如进行系统函数调用。
>
>
> Linux 为每个 CPU 维护了一个就绪队列，将活跃进程按照优先级和等待 CPU 的时间排序，然后选择最需要 CPU 的进程，也就是优先级最高和等待 CPU 时间最长的进程来运行。也就是vmstat命令中的`r`。
>
> 那么，进程在什么时候才会被调度到 CPU 上运行呢？
>
> - 进程执行完终止了，它之前使用的 CPU 会释放出来，这时再从就绪队列中拿一个新的进程来运行
> - 为了保证所有进程可以得到公平调度，CPU 时间被划分为一段段的时间片，这些时间片被轮流分配给各个进程。当某个进程时间片耗尽了就会被系统挂起，切换到其它等待 CPU 的进程运行。
> - 进程在系统资源不足时，要等待资源满足后才可以运行，这时进程也会被挂起，并由系统调度其它进程运行。
> - 当进程通过睡眠函数 sleep 主动挂起时，也会重新调度。
> - 当有优先级更高的进程运行时，为了保证高优先级进程的运行，当前进程会被挂起，由高优先级进程来运行。
> - 发生硬件中断时，CPU 上的进程会被中断挂起，转而执行内核中的中断服务程序。
>
> 结合我们之前的内容分析，阻塞的就绪队列是100左右，而我们的CPU只有4核，这部分原因造成的上下文切换就可能会相当高，再加上中断次数是4000左右和系统的函数调用等，整个系统的上下文切换到800万也不足为奇了。Java内部的线程切换才15次，是因为线程使用`Thread.yield()`来让出CPU资源，但是CPU有可能继续调度该线程，这个时候线程之间并没有切换，这也是为什么内部的某个线程切换次数并不是非常大的原因。

#### 接口响应慢

### JDK监控和故障处理工具

#### JDK命令行工具

这些命令在 JDK 安装目录下的 bin 目录下：

- **jps**：JVM Process Status。类似 UNIX 的 `ps` 命令。用户查看所有 Java 进程的启动类、传入参数和 Java 虚拟机参数等信息。
- **jstat**：JVM Statistics Monitoring Tool。用于收集 HotSpot 虚拟机各方面的运行数据。
- **jinfo**：Configuration Info for Java。显示虚拟机配置信息。
- **jmap**：Memory Map for Java。生成堆转储快照。
- **jhat**：JVM Heap Dump Browser。用于分析 heapdump 文件，它会建立一个 HTTP/HTML 服务器，让用户可以在浏览器上查看分析结果。
- **jstack**：Stack Trace for Java。生成虚拟机当前时刻的线程快照，线程快照就是当前虚拟机内每一条线程正在执行的方法堆栈的集合。

##### jps

​		jps（JVM Process Status）命令类似 UNIX ps 命令。查看所有 Java 进程。显示虚拟机执行主类名称以及这些进程的本地虚拟机唯一 ID（Local Virtual Machine Identifier，LVMID）。

`jps -q`：只输出进程的本地虚拟机唯一 ID。

```powershell
C:\Users\SnailClimb>jps
7360 NettyClient2
17396
7972 Launcher
16504 Jps
17340 NettyServer
```

`jps -l`：输出主类的全名，如果进程执行的是 Jar 包，输出 Jar 路径。

```powershell
C:\Users\SnailClimb>jps -l
7360 firstNettyDemo.NettyClient2
17396
7972 org.jetbrains.jps.cmdline.Launcher
16492 sun.tools.jps.Jps
17340 firstNettyDemo.NettyServer
```

`jps -v`：输出虚拟机进程启动时 JVM 参数。

`jps -m`：输出传递给 Java 进程 main() 函数的参数。

##### jstat

​		jstat（JVM Statistics Monitoring Tool） ，用于监视虚拟机各种运行状态信息。 它可以显示本地或者远程（需要远程主机提供 RMI 支持）虚拟机进程中的类信息、内存、垃圾收集、JIT 编译等运行数据，在没有 GUI，只提供了纯文本控制台环境的服务器上，它将是运行期间定位虚拟机性能问题的首选工具。

`jstat` 命令使用格式：

```powershell
jstat -<option> [-t] [-h<lines>] <vmid> [<interval> [<count>]]
```

> 比如 `jstat -gc -h3 31736 1000 10`表示分析进程 id 为 31736 的 gc 情况，每隔 1000ms 打印一次记录，打印 10 次停止，每 3 行后打印指标头部。

常见的 option 如下：

- `jstat -class vmid` ：显示 ClassLoader 的相关信息；
- `jstat -compiler vmid` ：显示 JIT 编译的相关信息；
- `jstat -gc vmid` ：显示与 GC 相关的堆信息；
- `jstat -gccapacity vmid` ：显示各个代的容量及使用情况；
- `jstat -gcnew vmid` ：显示新生代信息；
- `jstat -gcnewcapcacity vmid` ：显示新生代大小与使用情况；
- `jstat -gcold vmid` ：显示老年代和永久代的行为统计，从jdk1.8开始,该选项仅表示老年代，因为永久代被移除了；
- `jstat -gcoldcapacity vmid` ：显示老年代的大小；
- `jstat -gcpermcapacity vmid` ：显示永久代大小，从jdk1.8开始,该选项不存在了，因为永久代被移除了；
- `jstat -gcutil vmid` ：显示垃圾收集信息；

另外，加上 `-t`参数可以在输出信息上加一个 Timestamp 列，显示程序的运行时间。

##### jinfo

​		使用 jinfo 可以在不重启虚拟机的情况下，可以动态的修改 jvm 的参数。尤其在线上的环境特别有用。

`jinfo vmid`：实时地查看和调整虚拟机各项参数。输出当前 jvm 进程的全部参数和系统属性 (第一部分是系统的属性，第二部分是 JVM 的参数)。

`jinfo -flag name vmid`：输出对应名称的参数的具体值。

> 比如输出 MaxHeapSize、查看当前 jvm 进程是否开启打印 GC 日志 ( `-XX:PrintGCDetails` :详细 GC 日志模式，这两个都是默认关闭的)。

```powershell
C:\Users\SnailClimb>jinfo  -flag MaxHeapSize 17340
-XX:MaxHeapSize=2124414976
C:\Users\SnailClimb>jinfo  -flag PrintGC 17340
-XX:-PrintGC
```

`jinfo -flag [+|-]name vmid`：开启或者关闭对应名称的参数。

```powershell
C:\Users\SnailClimb>jinfo  -flag  PrintGC 17340
-XX:-PrintGC

C:\Users\SnailClimb>jinfo  -flag  +PrintGC 17340

C:\Users\SnailClimb>jinfo  -flag  PrintGC 17340
-XX:+PrintGC
```

##### jmap

​		jmap（Memory Map for Java），用于生成堆转储快照。 如果不使用 jmap 命令，要想获取 Java 堆转储，可以使用 `“-XX:+HeapDumpOnOutOfMemoryError”` 参数，可以让虚拟机在 OOM 异常出现之后自动生成 dump 文件，Linux 命令下可以通过 `kill -3` 发送进程退出信号也能拿到 dump 文件。

​		jmap 的作用并不仅仅是为了获取 dump 文件，它还可以查询 finalizer 执行队列、Java 堆和永久代的详细信息，如空间使用率、当前使用的是哪种收集器等。和jinfo一样，jmap有不少功能在 Windows 平台下也是受限制的。

> 例如：将指定应用程序的堆快照输出到桌面。后面，可以通过 jhat、Visual VM 等工具分析该堆文件。

```powershell
C:\Users\SnailClimb>jmap -dump:format=b,file=C:\Users\SnailClimb\Desktop\heap.hprof 17340
Dumping heap to C:\Users\SnailClimb\Desktop\heap.hprof ...
Heap dump file created
```

##### jhat

​		 jhat 用于分析 heapdump 文件，它会建立一个 HTTP/HTML 服务器，让用户可以在浏览器上查看分析结果。

```powershell
C:\Users\SnailClimb>jhat C:\Users\SnailClimb\Desktop\heap.hprof
Reading from C:\Users\SnailClimb\Desktop\heap.hprof...
Dump file created Sat May 04 12:30:31 CST 2019
Snapshot read, resolving...
Resolving 131419 objects...
Chasing references, expect 26 dots..........................
Eliminating duplicate references..........................
Snapshot resolved.
Started HTTP server on port 7000
Server is ready.
```

##### jstack

​		jstack（Stack Trace for Java）命令用于生成虚拟机当前时刻的线程快照。线程快照就是当前虚拟机内每一条线程正在执行的方法堆栈的集合。

​		生成线程快照的目的主要是定位线程长时间出现停顿的原因，如线程间死锁、死循环、请求外部资源导致的长时间等待等都是导致线程长时间停顿的原因。线程出现停顿的时候通过`jstack`来查看各个线程的调用堆栈，就可以知道没有响应的线程到底在后台做些什么事情，或者在等待些什么资源。

> 例如：下面是一个线程死锁的代码。我们下面会通过 `jstack` 命令进行死锁检查，输出死锁信息，找到发生死锁的线程。
>
> ```java
> public class DeadLockDemo {
>  private static Object resource1 = new Object();//资源 1
>  private static Object resource2 = new Object();//资源 2
> 
>  public static void main(String[] args) {
>      new Thread(() -> {
>          synchronized (resource1) {
>              System.out.println(Thread.currentThread() + "get resource1");
>              try {
>                  Thread.sleep(1000);
>              } catch (InterruptedException e) {
>                  e.printStackTrace();
>              }
>              System.out.println(Thread.currentThread() + "waiting get resource2");
>              synchronized (resource2) {
>                  System.out.println(Thread.currentThread() + "get resource2");
>              }
>          }
>      }, "线程 1").start();
> 
>      new Thread(() -> {
>          synchronized (resource2) {
>              System.out.println(Thread.currentThread() + "get resource2");
>              try {
>                  Thread.sleep(1000);
>              } catch (InterruptedException e) {
>                  e.printStackTrace();
>              }
>              System.out.println(Thread.currentThread() + "waiting get resource1");
>              synchronized (resource1) {
>                  System.out.println(Thread.currentThread() + "get resource1");
>              }
>          }
>      }, "线程 2").start();
>  }
> }
> ```
>
> 输出结果：
>
> ```
> Thread[线程 1,5,main]get resource1
> Thread[线程 2,5,main]get resource2
> Thread[线程 1,5,main]waiting get resource2
> Thread[线程 2,5,main]waiting get resource1
> ```
>
> 线程 A 通过 synchronized (resource1) 获得 resource1 的监视器锁，然后通过` Thread.sleep(1000);`让线程 A 休眠 1s 为的是让线程 B 得到执行然后获取到 resource2 的监视器锁。线程 A 和线程 B 休眠结束了都开始企图请求获取对方的资源，然后这两个线程就会陷入互相等待的状态，这也就产生了死锁。
>
> 通过 `jstack` 命令分析：
>
> ```powershell
> C:\Users\SnailClimb>jps
> 13792 KotlinCompileDaemon
> 7360 NettyClient2
> 17396
> 7972 Launcher
> 8932 Launcher
> 9256 DeadLockDemo
> 10764 Jps
> 17340 NettyServer
> 
> C:\Users\SnailClimb>jstack 9256
> ```
>
> 输出的部分内容如下：
>
> ```powershell
> Found one Java-level deadlock:
> =============================
> "线程 2":
> waiting to lock monitor 0x000000000333e668 (object 0x00000000d5efe1c0, a java.lang.Object),
> which is held by "线程 1"
> "线程 1":
> waiting to lock monitor 0x000000000333be88 (object 0x00000000d5efe1d0, a java.lang.Object),
> which is held by "线程 2"
> 
> Java stack information for the threads listed above:
> ===================================================
> "线程 2":
>      at DeadLockDemo.lambda$main$1(DeadLockDemo.java:31)
>         - waiting to lock <0x00000000d5efe1c0> (a java.lang.Object)
>         - locked <0x00000000d5efe1d0> (a java.lang.Object)
>         at DeadLockDemo$$Lambda$2/1078694789.run(Unknown Source)
>         at java.lang.Thread.run(Thread.java:748)
> "线程 1":
>         at DeadLockDemo.lambda$main$0(DeadLockDemo.java:16)
>         - waiting to lock <0x00000000d5efe1d0> (a java.lang.Object)
>         - locked <0x00000000d5efe1c0> (a java.lang.Object)
>         at DeadLockDemo$$Lambda$1/1324119927.run(Unknown Source)
>         at java.lang.Thread.run(Thread.java:748)
> 
> Found 1 deadlock.
> ```
>
> 可以看到 `jstack` 命令已经帮我们找到发生死锁的线程的具体信息。

#### JDK可视化分析工具

##### JConsole

​		JConsole 是基于 JMX 的可视化监视、管理工具。可以很方便的监视本地及远程服务器的 java 进程的内存使用情况。你可以在控制台输入`console`命令启动或者在 JDK 目录下的 bin 目录找到`jconsole.exe`然后双击启动。

###### 连接 Jconsole

如果需要使用 JConsole 连接远程进程，可以在远程 Java 程序启动时加上下面这些参数：

```shell
-Djava.rmi.server.hostname=外网访问 ip 地址 
-Dcom.sun.management.jmxremote.port=60001   //监控的端口号
-Dcom.sun.management.jmxremote.authenticate=false   //关闭认证
-Dcom.sun.management.jmxremote.ssl=false
```

在使用 JConsole 连接时，远程进程地址如下：

```shell
外网访问 ip 地址:60001 
```

###### 内存监控

​		JConsole 可以显示当前内存的详细信息。不仅包括堆内存/非堆内存的整体信息，还可以细化到 eden 区、survivor 区等的使用情况。

​		点击右边的“执行 GC(G)”按钮可以强制应用程序执行一个 Full GC。出现了 Major GC 经常会伴随至少一次的 Minor GC（并非绝对），Major GC 的速度一般会比 Minor GC 的慢 10 倍以上。

###### 线程监控

​		类似我们前面讲的 `jstack` 命令，不过这个是可视化的。最下面有一个"检测死锁 (D)"按钮，点击这个按钮可以自动为你找到发生死锁的线程以及它们的详细信息 。

##### Visual VM

​		VisualVM 是多合一故障处理工具。提供在 Java 虚拟机 (Java Virutal Machine, JVM) 上运行的 Java 应用程序的详细信息。在 VisualVM 的图形用户界面中，您可以方便、快捷地查看多个 Java 应用程序的相关信息。

> 《深入理解 Java 虚拟机》中的描述：
>
> VisualVM（All-in-One Java Troubleshooting Tool）是到目前为止随 JDK 发布的功能最强大的运行监视和故障处理程序，官方在 VisualVM 的软件说明中写上了“All-in-One”的描述字样，预示着他除了运行监视、故障处理外，还提供了很多其他方面的功能，如性能分析（Profiling）。VisualVM 的性能分析功能甚至比起 JProfiler、YourKit 等专业且收费的 Profiling 工具都不会逊色多少，而且 VisualVM 还有一个很大的优点：不需要被监视的程序基于特殊 Agent 运行，因此他对应用程序的实际性能的影响很小，使得他可以直接应用在生产环境中。这个优点是 JProfiler、YourKit 等工具无法与之媲美的。

​		 VisualVM 基于 NetBeans 平台开发，因此他一开始就具备了插件扩展功能的特性，通过插件扩展支持，VisualVM 可以做到：

- 显示虚拟机进程以及进程的配置、环境信息（jps、jinfo）。
- 监视应用程序的 CPU、GC、堆、方法区以及线程的信息（jstat、jstack）。
- dump 以及分析堆转储快照（jmap、jhat）。
- 方法级的程序运行性能分析，找到被调用最多、运行时间最长的方法。
- 离线程序快照：收集程序的运行时配置、线程 dump、内存 dump 等信息建立一个快照，可以将快照发送开发者处进行 Bug 反馈。
- 其他 plugins 的无限的可能性......

### Linux性能分析工具

​		有时候会遇到一些疑难杂症，并且监控插件并不能一眼立马发现问题的根源。这时候就需要登录服务器进一步深入分析问题的根源。如果我们有一套好的分析工具，那将是事半功倍，能够帮助大家快速定位问题，节省大家很多时间做更深入的事情。

套用5W2H方法，可以提出性能分析的几个问题：

- What-现象是什么样的
- When-什么时候发生
- Why-为什么会发生
- Where-哪个地方发生的问题
- How much-耗费了多少资源
- How to do-怎么解决问题

#### CPU

​		针对应用程序，我们通常关注的是内核CPU调度器功能和性能。线程的状态分析主要是分析线程的时间用在什么地方，而线程状态的分类一般分为：

- a. on-CPU：执行中，执行中的时间通常又分为用户态时间user和系统态时间sys。
- b. off-CPU：等待下一轮上CPU，或者等待I/O、锁、换页等等，其状态可以细分为可执行、匿名换页、睡眠、锁、空闲等状态。

**分析工具**

**uptime/w**：查看服务器运行时间、平均负载

**top**：监控每个进程的CPU用量分解

**vmstat**：系统的CPU平均负载情况

**mpstat**：查看多核CPU信息

**sar -u**：查看CPU过去或未来时点CPU利用率

**pidstat**：查看每个进程的用量分解

##### uptime

​		uptime 命令可以用来查看服务器已经运行了多久，当前登录的用户有多少，以及服务器在过去的1分钟、5分钟、15分钟的系统平均负载值。

​		第一项是当前时间，up 表示系统正在运行，6:47是系统启动的总时间，最后是系统的负载load信息。w 同上，增加了具体登陆了那些用户及登陆时间。

##### top

​		常用来监控Linux的系统状况，比如cpu、内存的使用，显示系统上正在运行的进程。

命令行选项：

- top：每隔3秒显式所有进程的资源占用情况

- top -u oracle -c：按照用户显示进程、并显示完整命令 

- top -d 2：每隔2秒显式所有进程的资源占用情况

- top -c：每隔3秒显式进程的资源占用情况，并显示进程的命令行参数(默认只有进程名)

- top -p 12345 -p 6789：每隔3秒显示pid是12345和pid是6789的两个进程的资源占用情况

- top -d 2 -c -p 123456：每隔2秒显示pid是12345的进程的资源使用情况，并显式该进程启动的命令行参数

- top -n：设置显示多少次后就退出

补充：

​		top命令是Linux上进行系统监控的首选命令，但有时候却达不到我们的要求，比如当前这台服务器，top监控有很大的局限性。这台服务器运行着websphere集群，有两个节点服务，就是【top视图 01】中的老大、老二两个java进程，top命令的监控最小单位是进程，所以看不到我关心的java线程数和客户连接数，而这两个指标是java的web服务非常重要的指标，通常我用ps和netstate两个命令来补充top的不足。

1. 系统运行时间和平均负载：

    top命令的顶部显示与uptime命令相似的输出。这些字段显示：

    - 当前时间

- 系统已运行的时间
    - 当前登录用户的数量
    - 相应最近5、10和15分钟内的平均负载。

2. 任务：

    第二行显示的是任务或者进程的总结。进程可以处于不同的状态。这里显示了全部进程的数量。除此之外，还有正在运行、睡眠、停止、僵尸进程的数量（僵尸是一种进程的状态）。这些进程概括信息可以用’t’切换显示。

3. CPU状态：

    下一行显示的是CPU状态。 这里显示了不同模式下的所占CPU时间的百分比。这些不同的CPU时间表示：

    - us, user： 运行(未调整优先级的) 用户进程的CPU时间
    - sy，system: 运行内核进程的CPU时间
    - ni，niced：运行已调整优先级的用户进程的CPU时间
    - wa，IO wait: 用于等待IO完成的CPU时间
    - hi：处理硬件中断的CPU时间
    - si: 处理软件中断的CPU时间
    - st：这个虚拟机被hypervisor偷去的CPU时间（译注：如果当前处于一个hypervisor下的vm，实际上hypervisor也是要消耗一部分CPU处理时间的）。

4. 内存使用：

    接下来两行显示内存使用率，有点像’free’命令。第一行是物理内存使用，第二行是虚拟内存使用(交换空间)。

    物理内存显示如下：全部可用内存、已使用内存、空闲内存、缓冲内存。

    交换部分显示的是：全部、已使用、空闲和缓冲交换空间。

    > 这里要说明的是不能用windows的内存概念理解这些数据，如果按windows的方式此台服务器“危矣”：8G的内存总量只剩下530M的可用内存。Linux的内存管理有其特殊性，复杂点需要一本书来说明，这里只是简单说点和我们传统概念（windows）的不同。
    >
    > 第四行中使用中的内存总量（used）指的是现在系统内核控制的内存数，空闲内存总量（free）是内核还未纳入其管控范围的数量。纳入内核管理的内存不见得都在使用中，还包括过去使用过的现在可以被重复利用的内存，内核并不把这些可被重新使用的内存交还到free中去，因此在[linux](http://lib.csdn.net/base/linux)上free内存会越来越少，但不用为此担心。
    >
    > 如果出于习惯去计算可用内存数，这里有个近似的计算公式：第四行的free + 第四行的buffers + 第五行的cached。
    >
    > 对于内存监控，在top里我们要时刻监控第五行swap交换分区的used，如果这个数值在不断的变化，说明内核在不断进行内存和swap的数据交换，这是真正的内存不够用了。

5. 字段/列：

    | 进程的属性 | 属性含义                                                     |
    | ---------- | ------------------------------------------------------------ |
    | PID        | 进程ID，进程的唯一标识符                                     |
    | USER       | 进程所有者的实际用户名。                                     |
    | PR         | 进程的调度优先级。这个字段的一些值是’rt’。这意味这这些进程运行在实时态。 |
    | NI         | 进程的nice值（优先级）。越小的值意味着越高的优先级。         |
    | VIRT       | 进程使用的虚拟内存。                                         |
    | RES        | 驻留内存大小。驻留内存是任务使用的非交换物理内存大小。       |
    | SHR        | SHR是进程使用的共享内存。                                    |
    | S          | 这个是进程的状态。它有以下不同的值:<br>D–不可中断的睡眠态、R–运行态、S–睡眠态、T–被跟踪或已停止、Z – 僵尸态 |
    | %CPU       | 自从上一次更新时到现在任务所使用的CPU时间百分比。            |
    | %MEM       | 进程使用的可用物理内存百分比。                               |
    | TIME+      | 任务启动后到现在所使用的全部CPU时间，精确到百分之一秒。      |
    | COMMAND    | 运行进程所使用的命令。                                       |

    还有许多在默认情况下不会显示的输出，它们可以显示进程的页错误、有效组和组ID和其他更多的信息。

    常用交互命令：

    **‘B’**：一些重要信息会以加粗字体显示(高亮)。这个命令可以切换粗体显示。

    **‘b’**：

    **‘D’ / ’S‘**：你将被提示输入一个值（以秒为单位），它会以设置的值作为刷新间隔。如果你这里输入了1，top将会每秒刷新。 top默认为3秒刷新

    **‘l’、‘t’、‘m’**：切换负载、任务、内存信息的显示，这会相应地切换顶部的平均负载、任务/CPU状态和内存信息的概况显示。

    **‘z’**：切换彩色显示

    **‘x’ / ‘y’**：切换高亮信息：’x’将排序字段高亮显示（纵列）；’y’将运行进程高亮显示（横行）。依赖于你的显示设置，你可能需要让输出彩色来看到这些高亮。

    **‘u’**：特定用户的进程

    **‘n’ / ‘#’**：任务的数量

    **‘k’**：结束任务

##### vmstat

​	vmstat命令是最常见的Linux/Unix监控工具，可以展现给定时间间隔的服务器的状态值,包括服务器的CPU使用率，内存使用，虚拟内存交换情况,IO读写情况。

​	一般vmstat工具的使用是通过两个数字参数来完成的，第一个参数是采样的时间间隔数，单位是秒，第二个参数是采样的次数。每个参数的含义：

**Procs（进程）**：

- r：运行队列中进程数量，这个值也可以判断是否需要增加CPU。（长期大于1）
- b：等待IO的进程数量。

**Memory（内存）**：

- swpd：使用虚拟内存大小，如果swpd的值不为0，但是SI，SO的值长期为0，这种情况不会影响系统性能。
- free：空闲物理内存大小。
- buff：用作缓冲的内存大小。
- cache：用作缓存的内存大小，如果cache的值大的时候，说明cache处的文件数多，如果频繁访问到的文件都能被cache处，那么磁盘的读IO bi会非常小。

**Swap**：

- si：每秒从交换区写到内存的大小，由磁盘调入内存。
- so：每秒写入交换区的内存大小，由内存调入磁盘。

> 注意：内存够用的时候，这2个值都是0，如果这2个值长期大于0时，系统性能会受到影响，磁盘IO和CPU资源都会被消耗。有些朋友看到空闲内存（free）很少的或接近于0时，就认为内存不够用了，不能光看这一点，还要结合si和so，如果free很少，但是si和so也很少（大多时候是0），那么不用担心，系统性能这时不会受到影响的。因为linux总是先把内存用光.

**IO**：

- bi：每秒读取的块数
- bo：每秒写入的块数

>注意：随机磁盘读写的时候，这2个值越大（如超出1024k)，能看到CPU在IO等待的值也会越大。

**system（系统）**：

- in：每秒中断数，包括时钟中断。
- cs：每秒上下文切换数。

>注意：上面2个值越大，会看到由内核消耗的CPU时间会越大。

**CPU（以百分比表示）**：

- us：用户进程执行时间百分比(user  time) us的值比较高时，说明用户进程消耗的CPU时间多，但是如果长期超50%的使用，那么我们就该考虑优化程序算法或者进行加速。
- sy：内核系统进程执行时间百分比(system  time) sy的值高时，说明系统内核消耗的CPU资源多，这并不是良性表现，我们应该检查原因。
- wa：IO等待时间百分比  wa的值高时，说明IO等待比较严重，这可能由于磁盘大量作随机访问造成，也有可能磁盘出现瓶颈（块操作）。
- id：空闲时间百分比。

##### mpstat

​	mpstat是一个实时监控工具，主要报告与CPU相关统计信息,在多核心cpu系统中，不仅可以查看cpu平均信息，还可以查看指定cpu信息。

常用命令：

- mpstat -P ALL：查看全部CPU的负载情况。 
- mpstat 2 5：可指定间隔时间和次数。

CPU相关：

- CPU: 处理器编号。关键字all表示统计信息计算为所有处理器之间的平均值。
- ％usr: 显示在用户级（应用程序）执行时发生的CPU利用率百分比。
- ％nice: 显示以优先级较高的用户级别执行时发生的CPU利用率百分比。
- ％sys: 显示在系统级（内核）执行时发生的CPU利用率百分比。请注意，这不包括维护硬件和软件的时间中断。
- ％iowait: 显示系统具有未完成磁盘I / O请求的CPU或CPU空闲的时间百分比。
- ％irq: 显示CPU或CPU用于服务硬件中断的时间百分比。
- %soft: 显示CPU或CPU用于服务软件中断的时间百分比。
- %steal: 显示虚拟CPU或CPU在管理程序为另一个虚拟处理器提供服务时非自愿等待的时间百分比。
- %guest: 显示CPU或CPU运行虚拟处理器所花费的时间百分比。

##### sar

​		系统活动情况报告，可以从多方面对系统的活动进行报告，包括：文件的读写情况、系统调用的使用情况、磁盘I/O、CPU效率、内存使用状况、进程活动及IPC有关的活动等。

CPU相关：

- sar -p：查看全天

- sar -u 1 10：1表示每隔一秒，10表示写入10次。

- CPU：all，表示统计信息为所有 CPU 的平均值。

- %user：显示在用户级别(application)运行使用 CPU 总时间的百分比。

- %nice：显示在用户级别，用于nice操作，所占用 CPU 总时间的百分比。

- %system：在核心级别(kernel)运行所使用 CPU 总时间的百分比。

- %iowait：显示用于等待I/O操作占用 CPU 总时间的百分比。

- %steal：管理程序(hypervisor)为另一个虚拟进程提供服务而等待虚拟 CPU 的百分比。

- %idle：显示 CPU 空闲时间占用 CPU 总时间的百分比。


##### pidstat

​		用于监控全部或指定进程的cpu、内存、线程、设备IO等系统资源的占用情况。pidstat 默认显示了所有进程的cpu使用率。pidstat 和 pidstat -u -p ALL 是等效的。

详细说明：

- PID：进程ID

- %usr：进程在用户空间占用cpu的百分比

- %system：进程在内核空间占用cpu的百分比

- %guest：进程在虚拟机占用cpu的百分比

- %CPU：进程占用cpu的百分比

- CPU：处理进程的cpu编号

- Command：当前进程对应的命令


#### 内存

​		内存是为提高效率而生，实际分析问题的时候，内存出现问题可能不只是影响性能，而是影响服务或者引起其他问题。同样对于内存有些概念需要清楚：

- 主存
- 虚拟内存
- 常驻内存
- 地址空间
- OOM
- 页缓存
- 缺页
- 换页
- 交换空间
- 交换
- 用户分配器libc、glibc、libmalloc和mtmalloc
- LINUX内核级SLUB分配器

分析工具：

- free：查看内存的使用情况
- top：监控每个进程的内存使用情况
- vmstat：虚拟内存统计信息
- sar -r：查看内存
- sar：查看CPU过去或未来时点CPU利用率
- pidstat：查看每个进程的内存使用情况

##### free

​		free 命令显示系统内存的使用情况，包括物理内存、交换内存(swap)和内核缓冲区内存。从应用程序的角度来说，available = free + buffer + cache。可用内存=系统free memory+buffers+cached。

- Mem 行(第二行)是内存的使用情况。
- Swap 行(第三行)是交换空间的使用情况。
- total 列显示系统总的可用物理内存和交换空间大小。
- used 列显示已经被使用的物理内存和交换空间。
- free 列显示还有多少物理内存和交换空间可用使用。
- shared 列显示被共享使用的物理内存大小。
- buff/cache 列显示被 buffer 和 cache 使用的物理内存大小。
- available 列显示还可以被应用程序使用的物理内存大小。

常用命令：

- free   


- free -g ：以GB显示
- free -m：以MB显示
- free -h：自动转换展示
- free -h -s  3：有时我们需要持续的观察内存的状况，此时可以使用 -s 选项并指定间隔的秒数

##### top

##### vmstat

##### sar

常用命令：

- sar -r：#查看内存使用情况


详解：

- kbmemfree：空闲的物理内存大小

- kbmemused：使用中的物理内存大小

- %memused：物理内存使用率

- kbbuffers：内核中作为缓冲区使用的物理内存大小，kbbuffers和kbcached:这两个值就是free命令中的buffer和cache。

- kbcached：缓存的文件大小

- kbcommit：保证当前系统正常运行所需要的最小内存，即为了确保内存不溢出而需要的最少内存（物理内存							+Swap分区）

- commt：这个值是kbcommit与内存总量（物理内存+swap分区）的一个百分比的值


##### pidstat

- pidstat -r：查看内存使用情况 pidstat将显示各活动进程的内存使用统计

- PID：进程标识符

- Minflt/s：任务每秒发生的次要错误，不需要从磁盘中加载页

- Majflt/s：任务每秒发生的主要错误，需要从磁盘中加载页

- VSZ：虚拟地址大小，虚拟内存的使用KB

- RSS：常驻集合大小，非交换区五里内存使用KB

- Command：task命令名


#### 磁盘IO

​		磁盘通常是计算机最慢的子系统，也是最容易出现性能瓶颈的地方，因为磁盘离 CPU 距离最远而且 CPU 访问磁盘要涉及到机械操作，比如转轴、寻轨等。访问硬盘和访问内存之间的速度差别是以数量级来计算的，就像1天和1分钟的差别一样。要监测 IO 性能，有必要了解一下基本原理和 Linux 是如何处理硬盘和内存之间的 IO 的。

在理解磁盘IO之前，同样我们需要理解一些概念，例如：

- 文件系统
- VFS
- 文件系统缓存
- 页缓存page cache
- 缓冲区高速缓存buffer cache
- 目录缓存
- inode
- inode缓存
- noop调用策略

分析工具：

- iostat：磁盘详细统计信息
- iotop：按进程查看磁盘IO统计信息
- pidstat：查看每个进程的磁盘IO使用情况

##### iostat

​		iostat工具将对系统的磁盘操作活动进行监视。它的特点是汇报磁盘活动统计情况，同时也会汇报出CPU使用情况。

CPU属性：

- %user：CPU处在用户模式下的时间百分比。

- %nice：CPU处在带NICE值的用户模式下的时间百分比。

- %system：CPU处在系统模式下的时间百分比。

- %iowait：CPU等待输入输出完成时间的百分比。

- %steal：管理程序维护另一个虚拟处理器时，虚拟CPU的无意识等待时间百分比。

- %idle：CPU空闲时间百分比。


备注：

- 如果%iowait的值过高，表示硬盘存在I/O瓶颈

- 如果%idle值高，表示CPU较空闲

- 如果%idle值高但系统响应慢时，可能是CPU等待分配内存，应加大内存容量。

- 如果%idle值持续低于10，表明CPU处理能力相对较低，系统中最需要解决的资源是CPU。


Device属性：

- tps：该设备每秒的传输次数

- kB_read/s：每秒从设备（drive expressed）读取的数据量；

- kB_wrtn/s：每秒向设备（drive expressed）写入的数据量；

- kB_read： 读取的总数据量；

- kB_wrtn：写入的总数量数据量


常用命令：

- iostat 2 3：每隔2秒刷新显示，且显示3次

- iostat -m：以M为单位显示所有信息

- 查看设备使用率（%util）、响应时间（await）

- iostat -d -x -k 1 1


##### iotop

​		在一般运维工作中经常会遇到这么一个场景，服务器的IO负载很高（iostat中的util），但是无法快速的定位到IO负载的来源进程和来源文件导致无法进行相应的策略来解决问题。如果你想检查那个进程实际在做 I/O，那么运行 `iotop` 命令加上 `-o` 或者 `--only` 参数。

```powershell
iotop --only 
```

##### pidstat

​		显示各个进程的IO使用情况

**pidstat -d**

​		报告IO统计显示以下信息：

- PID：进程id
- kB_rd/s：每秒从磁盘读取的KB
- kB_wr/s：每秒写入磁盘KB
- kB_ccwr/s：任务取消的写入磁盘的KB。当任务截断脏的pagecache的时候会发生。
- COMMAND:task的命令名

#### 网络

分析工具：

- ping：测试网络的连通性
- netstat：检验本机各端口的网络连接情况
- hostname：查看主机和域名

##### ping

常用命令参数：

- -d：使用Socket的SO_DEBUG功能。
- -f：极限检测。大量且快速地送网络封包给一台机器，看它的回应。
- -n：只输出数值。
- -q：不显示任何传送封包的信息，只显示最后的结果。
- -r：忽略普通的Routing Table，直接将数据包送到远端主机上。通常是查看本机的网络接口是否有问题。
- -R：记录路由过程。
- -v：详细显示指令的执行过程。
- -c 数目：在发送指定数目的包后停止。
- -i 秒数：设定间隔几秒送一个网络封包给一台机器，预设值是一秒送一次。

- -I 网络界面：使用指定的网络界面送出数据包。

- -l 前置载入：设置在送出要求信息之前，先行发出的数据包。

- -p 范本样式：设置填满数据包的范本样式。

- -s 字节数：指定发送的数据字节数，预设值是56，加上8字节的ICMP头，一共是64ICMP数据字节。

- -t 存活数值：设置存活数值TTL的大小。

> ping -b 192.168.120.1   --ping网关
>
> ping -c 10 192.168.120.206  --ping指定次数
>
> ping -c 10 -i 0.5 192.168.120.206   --时间间隔和次数限制的ping

##### netstat

​		netstat命令是一个监控TCP/IP网络的非常有用的工具，它可以显示路由表、实际的网络连接以及每一个网络接口设备的状态信息。

netstat选项：

```
-a或--all：显示所有连线中的Socket。 
-a：all，显示所有选项，默认不显示LISTEN相关。
-t：tcp)，仅显示tcp相关选项。
-u：udp)，仅显示udp相关选项。
-n：拒绝显示别名，能显示数字的全部转化成数字。
-l：仅列出有在 Listen (监听) 的服务状态。
-p：显示建立相关链接的程序名
-r：显示路由信息，路由表
-e：显示扩展信息，例如uid等
-s：按各个协议进行统计
-c：每隔一个固定时间，执行该netstat命令。
```

常用命令：

列出所有端口情况

```powershell
netstat -a      # 列出所有端口
netstat -at     # 列出所有TCP端口
netstat -au     # 列出所有UDP端口
```

列出所有处于监听状态的 Sockets

```powershell
netstat -l   # 只显示监听端口
netstat -lt  # 显示监听TCP端口
netstat -lu  # 显示监听UDP端口
netstat -lx  # 显示监听UNIX端口
```

显示每个协议的统计信息

```powershell
netstat -s     # 显示所有端口的统计信息
netstat -st    # 显示所有TCP的统计信息
netstat -su    # 显示所有UDP的统计信息
```

显示 PID 和进程名称

```powershell
netstat -p
```

显示网络统计信息

```powershell
netstat -s
```

统计机器中网络连接各个状态个数

```powershell
netstat` `-an | ``awk` `'/^tcp/ {++S[$NF]} END {for (a in S) print a,S[a]} '
```
