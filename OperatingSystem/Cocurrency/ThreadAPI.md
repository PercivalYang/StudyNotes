[toc]
# Thread API
## Create a thread
```c
#include <pthread.h>
int
pthread_create(
        pthread_t   *thread,
        const   pthread_attr_t *attr,
        void    *(*start_routine)(void*),
        void    *arg);
```
- 返回类型为`int`，代表？
- 内部参数：
    - `*thread`: 一个指向`pthread_t`类型结构体的指针
    - `*attr`: 一般传入`NULL`，用于设置`thread`的一些属性例如：stack size, 每个thread的priority
    - `start_routine`: 也叫做`funtion pointer`，指向需要调用的函数，前面的为函数返回类型，后面的`(void*)`是函数输入参数的类型
        - 例如返回整型，输入参数为double类型：
        ```c
        ...
        int (*start_routine) (double),
        double arg);
        ```
    - `arg`: 指向调用的函数所需的参数
- 为什么用`void pointer`?
    - 当`void *`用作返回类型时，可以允许我们返回any type of result；同时若输入参数的类型为`void *`，表示允许使用any type of arguments。

## Thread Completion
- `thread` 创建好后如何调用线程，并接受线程返回的函数值呢？接下来就要用到`pthread_join`
    ```c
    int pthread_join(pthread_t thread, void **value_ptr);
    ```
    - `thread`：代表我们创建好的`pthread_t`类型的`thread`
    - `value_ptr`：代表调用线程时函数返回的value,如果不需要函数的返回值可以直接传入`NULL`
- 来看一个示例程序：  
    ```c
    #include <assert.h>
    #include <stdio.h>
    #include <pthread.h>
    typedef struct {
        int a;
        int b;
    } myarg_t;

    void *mythread(void *arg) {
        // 不能返回local variable,例如这里返回的rvals是提前
        // 通过Malloc在内存中申请地址后再返回的
        myret_t *rvals = Malloc(sizeof(myret_t));
        rvals->x = 1;
        rvals->y = 2;
        return (void *) rvals;
    }

    int main(int argc, char *argv[]) {
        pthread_t p;
        myarg_t args = { 10, 20 };

        int rc = pthread_create(&p, NULL, mythread, &args);
        assert(rc == 0);
        (void) pthread_join(p, NULL);
        printf("done\n");
        return 0;
    }
    ```

## Lock
- 用来保护**critical section**的code,防止出现race condition的情况
```c
int pthread_mutex_lock(pthread_mutex_t *mutex);
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```
- 常见用法  
    ```c
    pthread_mutex_t lock;
    // 对lock初始化
    int rc = pthread_mutex_init(&lock, NULL);
    // 保证初始化成功
    assert(rc == 0); // always check success!
    // Lock critical section
    pthread_mutex_lock(&lock);
    x = x + 1; // critical section
    pthread_mutex_unlock(&lock);
    // 使用完Lock后记得destroy
    pthread_mutex_destroy()
    ```
    - 对`Lock`初始化的方法有动态和静态，上述初始化为动态方法(即在run-time阶段初始化)，下述为静态初始化:
    ```c
    pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;
    ```
- **Check error**
    - 上述常见用法里缺少对critical section代码的查错，在运行多线程代码前应保证code block的正确性避免出现意想不到的错误，查错方法如下
    ```c
    // Keeps code clean; only use if exit() OK upon failure
    void Pthread_mutex_lock(pthread_mutex_t *mutex) {
        int rc = pthread_mutex_lock(mutex);
        assert(rc == 0);
    }
    ```
