### 一、结构体定义
```C
typedef void *(*TASK_DEAL_FUNC)(void *args, int nThreadNum);

typedef struct {
    TASK_DEAL_FUNC fun;  /* 处理函数指针 */
    void *args;          /* 处理函数参数 */
} task_t;

typedef struct {
    pthread_mutex_t thread_pool_mutex;  /* 任务队列管理线程锁 */
    pthread_cond_t thread_pool_cond;    /* 任务队列管理条件变量 */
    void *task_queue;                   /* 任务队列指针 */
    int keepalive;                      /* 置 0 关闭线程池 */
    int threads;                        /* 线程数 */
} thread_pool_t;
```

### 二、创建线程池
1. 创建线程池结构体
2. 初始化锁、条件变量
3. 创建空任务队列
4. 创建 threads 个线程

### 三、线程函数：
1. pthread_detach()
2. 获取线程编号（全局变量递增）
3. while(pool->keepalive)
    1. 加锁
    2. 队列为空就 wait
    3. 获取队首任务
    4. 释放锁
    5. 执行任务
    6. 释放任务
4. 退出线程，pool->threads-- 