# io 模型

## linux 五种 io 模型

![](images/io_multiplexing/1.png)

## select

![](images/io_multiplexing/2.png)

## select

1. 使用数组保存文件描述符，默认最大限制是1024。
2. 使用 select 监听文件描述符的状态。
3. 当有一个文件描述符状态改变时，遍历所有的文件描述符，找到可读的文件描述符。

```
struct PACKET_HEAD
{
    int length;
};

class Server
{
private:
    struct sockaddr_in server_addr;
    socklen_t server_addr_len;
    int listen_fd;    // 监听的fd
    int max_fd;       // 最大的fd
    fd_set master_set;   // 所有fd集合，包括监听fd和客户端fd   
    fd_set working_set;  // 工作集合
    struct timeval timeout; 
public:
    Server(int port);
    ~Server();
    void Bind();
    void Listen(int queue_len = 20);
    void Accept();
    void Run();
    void Recv(int nums);
};

void Server::Run()
{
    max_fd = listen_fd;   // 初始化max_fd
    FD_ZERO(&master_set);
    FD_SET(listen_fd, &master_set);  // 添加监听fd

    while(1)
    {
        FD_ZERO(&working_set);
        memcpy(&working_set, &master_set, sizeof(master_set));

        timeout.tv_sec = 30;
        timeout.tv_usec = 0;

        int nums = select(max_fd+1, &working_set, NULL, NULL, &timeout);
        if(nums < 0)
        {
            cout << "select() error!";
            exit(1);
        }

        if(nums == 0)
        {
            //cout << "select() is timeout!";
            continue;
        }

        if(FD_ISSET(listen_fd, &working_set))
            Accept();   // 有新的客户端请求
        else
            Recv(nums); // 接收客户端的消息
    }
}

void Server::Recv(int nums)
{
    for(int fd=0; fd<=max_fd; ++fd)
    {
        if(FD_ISSET(fd, &working_set))
        {
            bool close_conn = false;  // 标记当前连接是否断开了

            PACKET_HEAD head;
            recv(fd, &head, sizeof(head), 0);   // 先接受包头，即数据总长度

            char* buffer = new char[head.length];
            bzero(buffer, head.length);
            int total = 0;
            while(total < head.length)
            {
                int len = recv(fd, buffer + total, head.length - total, 0);
                if(len < 0)
                {
                    cout << "recv() error!";
                    close_conn = true;
                    break;
                }
                total = total + len;
            }

            if(total == head.length)  // 将收到的消息原样发回给客户端
            {
                int ret1 = send(fd, &head, sizeof(head), 0);
                int ret2 = send(fd, buffer, head.length, 0);
                if(ret1 < 0 || ret2 < 0)
                {
                    cout << "send() error!";
                    close_conn = true;
                }
            }

            delete buffer;

            if(close_conn)  // 当前这个连接有问题，关闭它
            {
                close(fd);
                FD_CLR(fd, &master_set);
                if(fd == max_fd)  // 需要更新max_fd;
                {
                    while(FD_ISSET(max_fd, &master_set) == false)
                        --max_fd;
                }
            }
        }
    }   
}
```

## poll

和 select 一样，只是用链表保存文件描述符。

## epoll

1. 使用红黑树保存文件描述符。
2. 使用 epoll 监听文件描述符状态。
3. 当有文件描述符状态改变时，通过回调将可读的文件描述符挂到就绪链表上。
4. 遍历就绪链表上读取数据。

### 主要系统调用函数

1. int epoll_create(int size);
1. int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
1. int epoll_wait(int epfd, struct epoll_event *events, int maxevents, int timeout);

### 示例

```
struct PACKET_HEAD
{
    int length;
};

class Server
{
private:
    struct sockaddr_in server_addr;
    socklen_t server_addr_len;
    int listen_fd;    // 监听的fd
    int epfd;         // epoll fd
    struct epoll_event events[EPOLLSIZE];   // epoll_wait返回的就绪事件
public:
    Server(int port);
    ~Server();
    void Bind();
    void Listen(int queue_len = 20);
    void Accept();
    void Run();
    void Recv(int fd);
};

void Server::Run()
{
    epfd = epoll_create(1);   // 创建epoll句柄

    struct epoll_event event;
    event.data.fd = listen_fd;
    event.events = EPOLLIN;
    epoll_ctl(epfd, EPOLL_CTL_ADD, listen_fd, &event);   // 注册listen_fd 

    while(1)
    {
        int nums = epoll_wait(epfd, events, EPOLLSIZE, -1);
        if(nums < 0)
        {
            cout << "poll() error!";
            exit(1);
        }

        if(nums == 0)
        {
            continue;
        }

        for(int i=0; i<nums; ++i)  // 遍历所有就绪事件
        {
            int fd = events[i].data.fd;
            if((fd == listen_fd) && (events[i].events & EPOLLIN))
                Accept();    // 有新的客户端请求
            else if(events[i].events & EPOLLIN)
                Recv(fd);    // 读数据 
            else
                ;
        }       
    }
}

void Server::Recv(int fd)
{
    bool close_conn = false;  // 标记当前连接是否断开了

    PACKET_HEAD head;
    recv(fd, &head, sizeof(head), 0);   // 先接受包头，即数据总长度

    char* buffer = new char[head.length];
    bzero(buffer, head.length);
    int total = 0;
    while(total < head.length)
    {
        int len = recv(fd, buffer + total, head.length - total, 0);
        if(len < 0)
        {
            cout << "recv() error!";
            close_conn = true;
            break;
        }
        total = total + len;
    }

    if(total == head.length)  // 将收到的消息原样发回给客户端
    {
        int ret1 = send(fd, &head, sizeof(head), 0);
        int ret2 = send(fd, buffer, head.length, 0);
        if(ret1 < 0 || ret2 < 0)
        {
            cout << "send() error!";
            close_conn = true;
        }
    }

    delete buffer;

    if(close_conn)  // 当前这个连接有问题，关闭它
    {
        close(fd);
        struct epoll_event event;
        event.data.fd = fd;
        event.events = EPOLLIN;
        epoll_ctl(epfd, EPOLL_CTL_DEL, fd, &event);  // Delete一个fd
    }
}
```

## 区别

1. select 使用数组保存文件描述符有数量限制，epoll 使用红黑树保存无数量限制。
2. select 每次需要遍历所有文件描述符查找哪个文件描述符可读，epoll 只需要遍历就绪链表。
3. select 每次调用都需要把文件描述符数组复制到内核，epoll 只需要在注册的时候复制一次。

## 引用

1. [IO多路复用：select、poll、epoll示例](https://blog.csdn.net/lisong694767315/article/details/51328062)
