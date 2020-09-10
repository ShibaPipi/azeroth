## 进程同步之 Unix 域套接字

>### 进程同步之 Unix 域套接字
* 域套接字是一种高级的进程间通信的方法
* Unix 域套接字可以用时同一机器进程间通信
* 套接字（socket）原是网络通信中使用的术语
* Unix 系统提供的域套接字提供了网络套接字类似的功能

>### Unix 域套接字使用方法

<div align="center">
    <img src="img/socket.png" height="494" alt="" />
</div>

* 例子

<div align="center">
    <img src="img/socket_example.png" height="169" alt="" />
</div>

* 假设有如下 `C++` 代码
    * server.cpp
    
        ```C++
            #include <stdio.h>
            #include <sys/types.h>
            #include <sys/socket.h>
            #include <sys/un.h>
            #include <strings.h>
            #include <string.h>
            #include <netinet/in.h>
            #include <stdlib.h>
            #include <unistd.h>
            
            #include <iostream>
            
            // 域套接字
            #define SOCKET_PATH "./domainsocket"
            #define MSG_SIZE 2048
            
            int main()
            {
                int socket_fd, accept_fd;
                int ret = 0;
                socklen_t addr_len;
                char msg[MSG_SIZE];
                struct sockaddr_un server_addr;
            
                // 1. 创建域套接字
                socket_fd = socket(PF_UNIX,SOCK_STREAM,0);
                if(-1 == socket_fd){
                    std::cout << "Socket create failed!" << std::endl;
                    return -1;
                }
                // 移除已有域套接字路径
                remove(SOCKET_PATH);
                // 内存区域置0
                bzero(&server_addr,sizeof(server_addr));
                server_addr.sun_family = PF_UNIX;
                strcpy(server_addr.sun_path, SOCKET_PATH);
            
                // 2. 绑定域套接字
                std::cout << "Binding socket..." << std::endl;
                ret = bind(socket_fd,(sockaddr *)&server_addr,sizeof(server_addr));
            
                if(0 > ret){
                    std::cout << "Bind socket failed." << std::endl;
                    return -1;
                }
                
                // 3. 监听套接字
                std::cout << "Listening socket..." << std::endl;
                ret = listen(socket_fd, 10);
                if(-1 == ret){
                    std::cout << "Listen failed" << std::endl;
                    return -1;
                }
                std::cout << "Waiting for new requests." << std::endl;
                accept_fd = accept(socket_fd, NULL, NULL);
                
                bzero(msg,MSG_SIZE);
            
                while(true){
                    // 4. 接收&处理信息
                    recv(accept_fd, msg, MSG_SIZE, 0);
                    std::cout << "Received message from remote: " << msg <<std::endl;
                }
            
                close(accept_fd);
                close(socket_fd);
                return 0;
            }
        ```
    * client.cpp
    
        ```C++
        #include <stdio.h>
        #include <sys/types.h>
        #include <sys/socket.h>
        #include <sys/un.h>
        #include <strings.h>
        #include <string.h>
        #include <netinet/in.h>
        #include <stdlib.h>
        #include <unistd.h>
        
        #include <iostream>
        
        #define SOCKET_PATH "./domainsocket"
        #define MSG_SIZE 2048
        
        int main()
        {
            int socket_fd;
            int ret = 0;
            char msg[MSG_SIZE];
            struct sockaddr_un server_addr;
        
            // 1. 创建域套接字
            socket_fd = socket(PF_UNIX, SOCK_STREAM, 0);
            if(-1 == socket_fd){
                std::cout << "Socket create failed!" << std::endl;
                return -1;
            }
            
            // 内存区域置0
            bzero(&server_addr,sizeof(server_addr));
            server_addr.sun_family = PF_UNIX;
            strcpy(server_addr.sun_path, SOCKET_PATH);
        
            // 2. 连接域套接字
            ret = connect(socket_fd, (sockaddr *)&server_addr, sizeof(server_addr));
        
            if(-1 == ret){
                std::cout << "Connect socket failed" << std::endl;
                return -1;
            }
        
            while(true){
                std::cout << "Input message>>> ";
                fgets(msg, MSG_SIZE, stdin);
                // 3. 发送信息
                ret = send(socket_fd, msg, MSG_SIZE, 0);
            }
        
            close(socket_fd);
            return 0;
        }
        ```

>### 回顾
* Unix 域套接字提供了单机简单可靠的进程通信同步服务
* 只能在单机使用，不能跨机器使用
