## 线程同步之读写锁

* 假设临界资源 `多读少写`
* 读取的时候 `并不会改变临界资源的值`
* 是否存在 `效率更高` 的同步方法？

>### 读写锁
* `读写锁`
    * `读写锁` 是一种特殊的 `自旋锁`
    * 允许多个读者访问资源以提高 `读性能`
    * 对于写操作是 `互斥的`
    
    <div align="center">
        <img src="img/rwlock.png" height="233" alt="" />
    </div>
    
* 例子
    * 假设有如下 `C++` 代码

        ```C++
            #include <stdio.h>
            #include <stdlib.h>
            #include <unistd.h>
            #include <pthread.h>
            #include <vector>
            
            int num = 0;
            
            pthread_rwlock_t rwlock = PTHREAD_RWLOCK_INITIALIZER;
            
            void *reader(void*) {
                int times = 10000000;
                while(times--) {
                    // 加读锁
                    pthread_rwlock_rdlock(&rwlock);
                    if (times % 1000 == 0) {
                        // 延迟执行 10 微秒
                        usleep(10);
                    }
                    // 释放锁
                    pthread_rwlock_unlock(&rwlock);
                }
            }
            
            void *writer(void*) {
                int times = 10000000;
                while(times--) {
                    // 加写锁
                    pthread_rwlock_wrlock(&rwlock);
                    num += 1;
                    // 释放锁
                    pthread_rwlock_unlock(&rwlock);
                }
            }
            
            int main() {
                printf("Start in main function.\n");
                pthread_t thread1, thread2, thread3;
                // 线程 1，2 为读操作，线程 3 为写操作，为模拟多读少写的场景
                pthread_create(&thread1, NULL, &reader, NULL);
                pthread_create(&thread2, NULL, &reader, NULL);
                pthread_create(&thread3, NULL, &writer, NULL);
                pthread_join(thread1, NULL);
                pthread_join(thread2, NULL);
                pthread_join(thread3, NULL);
                printf("Print in main function: num = %d\n", num);
                return 0;
            }
        ```

        * 使用 `time` 命令查看程序的执行时间
    * 与 `互斥量` 执行时间对比（[代码参考](./1_mutex.md)，同时需要改动代码，同样启动 2 读 1 写进程）
        
        ```C++
            #include <stdio.h>
            #include <stdlib.h>
            #include <unistd.h>
            #include <pthread.h>
            #include <vector>
            
            int num = 0;
            
            pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
            
            void *reader(void*) {
                int times = 10000000;
                while(times--) {
                    pthread_mutex_lock(&mutex);
            	    if (times % 1000 == 0) {
                        // 延迟执行 10 微秒
                        usleep(10);
                    }
                    pthread_mutex_unlock(&mutex);
                }
            }
            
            void *writer(void*) {
                int times = 10000000;
                while(times--) {
                    pthread_mutex_lock(&mutex);
            	    num += 1;
                    pthread_mutex_unlock(&mutex);
                }
            }
            
            
            int main() {
                printf("Start in main function.\n");
                pthread_t thread1, thread2, thread3;
                // 线程 1，2 为读操作，线程 3 为写操作，为模拟多读少写的场景
                pthread_create(&thread1, NULL, &reader, NULL);
                pthread_create(&thread2, NULL, &reader, NULL);
                pthread_create(&thread3, NULL, &writer, NULL);
                pthread_join(thread1, NULL);
                pthread_join(thread2, NULL);
                pthread_join(thread3, NULL);
                printf("Print in main function: num = %d\n", num);
                return 0;
            }

        ```

        * 可得出结论：读写锁在 `多读少写` 的场景性能提升较为明显
