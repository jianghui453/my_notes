# Linux 下的 SOCKET 使用

## 流程

![](images/SOCKET/process.jpg)

## 实例

```

/* File Name: server.c */
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<errno.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<netinet/in.h>
#define DEFAULT_PORT 8000
#define MAXLINE 4096
int main(int argc, char** argv)
{
    int    socket_fd, connect_fd;
    struct sockaddr_in     servaddr;
    char    buff[4096];
    int     n;
    //初始化Socket
    if( (socket_fd = socket(AF_INET, SOCK_STREAM, 0)) == -1 ){
    printf("create socket error: %s(errno: %d)\n",strerror(errno),errno);
    exit(0);
    }
    //初始化
    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);//IP地址设置成INADDR_ANY,让系统自动获取本机的IP地址。
    servaddr.sin_port = htons(DEFAULT_PORT);//设置的端口为DEFAULT_PORT
 
    //将本地地址绑定到所创建的套接字上
    if( bind(socket_fd, (struct sockaddr*)&servaddr, sizeof(servaddr)) == -1){
    printf("bind socket error: %s(errno: %d)\n",strerror(errno),errno);
    exit(0);
    }
    //开始监听是否有客户端连接
    if( listen(socket_fd, 10) == -1){
    printf("listen socket error: %s(errno: %d)\n",strerror(errno),errno);
    exit(0);
    }
    printf("======waiting for client's request======\n");
    while(1){
//阻塞直到有客户端连接，不然多浪费CPU资源。
        if( (connect_fd = accept(socket_fd, (struct sockaddr*)NULL, NULL)) == -1){
        printf("accept socket error: %s(errno: %d)",strerror(errno),errno);
        continue;
    }
//接受客户端传过来的数据
    n = recv(connect_fd, buff, MAXLINE, 0);
//向客户端发送回应数据
    if(!fork()){ /*紫禁城*/
        if(send(connect_fd, "Hello,you are connected!\n", 26,0) == -1)
        perror("send error");
        close(connect_fd);
        exit(0);
    }
    buff[n] = '\0';
    printf("recv msg from client: %s\n", buff);
    close(connect_fd);
    }
    close(socket_fd);
}
```

```

/* File Name: client.c */
 
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<errno.h>
#include<sys/types.h>
#include<sys/socket.h>
#include<netinet/in.h>
 
#define MAXLINE 4096
 
 
int main(int argc, char** argv)
{
    int    sockfd, n,rec_len;
    char    recvline[4096], sendline[4096];
    char    buf[MAXLINE];
    struct sockaddr_in    servaddr;

    if( argc != 2){
    printf("usage: ./client <ipaddress>\n");
    exit(0);
    }

    if( (sockfd = socket(AF_INET, SOCK_STREAM, 0)) < 0){
    printf("create socket error: %s(errno: %d)\n", strerror(errno),errno);
    exit(0);
    }

    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_port = htons(8000);
    if( inet_pton(AF_INET, argv[1], &servaddr.sin_addr) <= 0){
    printf("inet_pton error for %s\n",argv[1]);
    exit(0);
    }

    if( connect(sockfd, (struct sockaddr*)&servaddr, sizeof(servaddr)) < 0){
    printf("connect error: %s(errno: %d)\n",strerror(errno),errno);
    exit(0);
    }

    printf("send msg to server: \n");
    fgets(sendline, 4096, stdin);
    if( send(sockfd, sendline, strlen(sendline), 0) < 0)
    {
    printf("send msg error: %s(errno: %d)\n", strerror(errno), errno);
    exit(0);
    }
    if((rec_len = recv(sockfd, buf, MAXLINE,0)) == -1) {
       perror("recv error");
       exit(1);
    }
    buf[rec_len]  = '\0';
    printf("Received : %s ",buf);
    close(sockfd);
    exit(0);
}
```

## 两种工作模式

### LT

水平触发时默认的工作方式，如果一个文件描述符就绪了，不进行任何操作，后续还会继续通知。

### ET

边缘触发模式会在文件描述符从未就绪变为就绪时会触发一次，且只触发一次，后续不再通知。

## 引用

1. [epoll的ET和LT模式详解](https://blog.csdn.net/eroswang/article/details/4481521)
