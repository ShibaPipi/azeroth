## 线程同步之条件变量

>### 条件变量工作原理
* 条件变量是一种相对复杂的线程同步方法
* 条件变量允许线程睡眠，直到满足某种条件
* 当满足条件时，可以向该线程发送信号，通知唤醒

>### 生产者——消费者模型的漏洞
* 缓冲区小于等于 0 时，不允许消费者消费，消费者必须得到等待
    * `当生产者生产一个产品时，唤醒可能等待的消费者`
* 缓冲区满时，不允许生产者往缓冲区生产，生产者必须等待
    * `当消费者消费一个产品时，唤醒可能等待的生产者`

>### 条件变量实践（C语言）
* 需要用到的 C 函数
    * 条件变量的定义 `pthread_cond_t`，配合 `互斥量` 使用
    * 条件变量的等待 `pthread_cond_wait`（等待条件被满足）
    * 条件变量的唤醒 `pthread_cond_signal`（等待被唤醒）
* 例子
    * 假设有如下 `C++` 代码 condition_demo.cpp

        ```C++
            #include <iostream>
            #include <stdio.h>
            #include <stdlib.h>
            #include <vector>
            #include <queue>
            #include <unistd.h>
            #include <pthread.h>
            
            int MAX_BUF = 100;
            int num = 0;
            
            
            pthread_cond_t cond = PTHREAD_COND_INITIALIZER;
            pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
            
            void* producer(void*){
                while(true){
                    pthread_mutex_lock(&mutex);
                    // 如果缓冲区满了
                    while (num >= MAX_BUF){
                        // 等待
                        printf("缓冲区满了, 等待消费者消费...\n");
                        pthread_cond_wait(&cond, &mutex);
                    }
                    num += 1;
                    printf("生产一个产品，当前产品数量为：%d\n", num);
                    sleep(1);
                    pthread_cond_signal(&cond);
                    printf("通知消费者...\n");
                    pthread_mutex_unlock(&mutex);
                    sleep(1);
                }
            }
            
            void* consumer(void*){
                while(true){
                    pthread_mutex_lock(&mutex);
                    // 如果缓冲区没有数据
                    while (num <= 0){
                        // 等待
                        printf("缓冲区空了, 等待生产者生产...\n");
                        pthread_cond_wait(&cond, &mutex);
                    }
                    num -= 1;
                    printf("消费一个产品，当前产品数量为：%d\n", num);
                    sleep(1);
                    pthread_cond_signal(&cond);
                    printf("通知生产者...\n");
                    pthread_mutex_unlock(&mutex);
                }
            }
            
            int main(){
                pthread_t thread1, thread2;
                pthread_create(&thread1, NULL, &consumer, NULL);
                pthread_create(&thread2, NULL, &producer, NULL);
                pthread_join(thread1, NULL);
                pthread_join(thread2, NULL);
                return 0;
            }
        ```
