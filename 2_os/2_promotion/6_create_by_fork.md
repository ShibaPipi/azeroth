## 使用 fork 系统调用创建进程

>### 使用 fork 系统调用创建进程
* fork 系统调用是用于创建进程的
* fork 创建的进程初始化状态与父进程一样（包括拥有的变量，逻辑空间）
* 系统会为 fork 的进程分配新的资源（包括内存资源，`CPU` 资源）
* fork 系统调用无参数
* fork 会 `返回两次`，分别返回子进程 id 和 0
* 返回子进程 id 的是父进程，返回 0 的是子进程

>### 例子
* 假设有如下 `C++` 代码 fork_demo.cpp

    ```C++
        #include <iostream>
        #include <cstring>
        #include <stdio.h>
        #include <unistd.h>
        
        using namespace std;
        
        int main()
        {
            pid_t pid;
        
            int num = 888;
            pid = fork();
        
            if(pid == 0){
                cout << "这是一个子进程." << endl;
                cout << "num in son process: " << num << endl;
                while(true){
                    num += 1;
                    cout << "num in son process: " << num << endl;
                    sleep(1);
                }
            }
            else if(pid > 0){
                cout << "这是一个父进程." << endl;
                cout << "子进程id: " << pid << endl;
                cout << "num in father process: " << num << endl;
                while(true){
                    num -= 1;
                    cout << "num in father process: " << num << endl;
                    sleep(1);
                }
            }
            else if (pid < 0){
                cout << "创建进程失败." << endl;
            }
            return 0;
        }
    ```
