### juc（java.util.concurrent ）

locks、countdownlatch、线程池、ConcurrentHashMap

#### 线程池

- 线程池相关类
    - Executor、Executors、ExecutorService
    - 常用的线程池：FixedThreadPool、CachedThreadPool、ScheduledThreadPool、SingleThreadExecutor
    
- 能获取子线程的运行结果
    - Callable、Future、FutureTask
    
- 线程池参数：

    corePoolSize：核心线程数量，线程池维护线程的最少数量

    maximumPoolSize：线程池维护线程的最大数量

    keepAliveTime：线程池除核心线程外的其他线程的最长空闲时间，超过该时间的空闲线程会被销毁

    workQueue：线程池所使用的任务缓冲队列

    threadFactory：线程工厂，用于创建线程

    handler：线程池对拒绝任务的处理策略

- 处理流程

    1. 如果此时线程池中的数量小于corePoolSize，即使线程池中的线程都处于空闲状态，也要创建新的线程来处理被添加的任务。
    2. 如果此时线程池中的数量等于corePoolSize，但是缓冲队列workQueue未满，那么任务被放入缓冲队列。
    3. 如果此时线程池中的数量大于等于corePoolSize，缓冲队列workQueue满，并且线程池中的数量小于maximumPoolSize，建新的线程来处理被添加的任务。
    4. 如果此时线程池中的数量大于corePoolSize，缓冲队列workQueue满，并且线程池中的数量等于maximumPoolSize，那么通过 handler所指定的策略来处理此任务。
    5. 当线程池中的线程数量大于 corePoolSize时，如果某线程空闲时间超过keepAliveTime，线程将被终止。这样，线程池可以动态的调整池中的线程数。

    处理任务判断的优先级为 核心线程corePoolSize、任务队列workQueue、最大线程maximumPoolSize，如果三者都满了，使用handler处理被拒绝的任务。

- 线程池关闭

    - shutdown() 不接收新任务,会处理已添加任务
    - shutdownNow() 不接受新任务,不处理已添加任务,中断正在处理的任务

- 线程池状态：

    - RUNNING(接受并处理任务中)
    - SHUTDOWN(不接受新任务但处理排队任务)
    - STOP(不接受新任务 也不处理排队任务 并中断正在进行的任务)
    - TIDYING、TEMINATED(运行完成)

- 如何设置初始化线程池大小：

    - CPU 密集型任务：设置为 CPU 核数 + 1
    -  IO 密集型任务： CPU 核心数 * 2

- 使用线程池的好处：
    - 降低资源消耗
    - 提高响应速度
    - 提高线程的可管理性

#### Lock

### jvm

jvm内存模型

jvm有哪些垃圾回收器

类加载模型

### Lambda 表达式



