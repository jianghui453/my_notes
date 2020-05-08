# select poll epoll

## select

使用数组保存文件描述符，当有文件描述符状态改变时返回结果。
需要遍历整个描述符集合来获取可操作的描述符。

```
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

使用红黑树保存文件描述符。
描述符状态改变时调用回调函数，将描述符挂载到一个链表上。
从链表上读取相关描述符，避免了遍历整个描述符集合。
使用mmap避免了频繁的内核态、用户态的数据拷贝。

```
void Server::Run()
{
    fds[0].fd = listen_fd;        // 添加监听描述符
    fds[0].events = POLLIN;
    nfds = 0;

    for(int i=1; i<MAX_FD; ++i)
        fds[i].fd = -1;

    while(1)
    {
        int nums = poll(fds, nfds+1, -1);
        if(nums < 0)
        {
            cout << "poll() error!";
            exit(1);
        }

        if(nums == 0)
        {
            continue;
        }

        if(fds[0].revents & POLLIN)
            Accept();   // 有新的客户端请求
        else
            Recv();
    }
}

void Server::Recv()
{
    for(int i=1; i<MAX_FD; ++i)
    {
        if(fds[i].fd < 0)
            continue;
        if(fds[i].revents & POLLIN)       // 读就绪
        {
            int fd = fds[i].fd;
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
                fds[i].fd = -1;
            }
        }
    }   
}
```

## 引用

1. [IO多路复用：select、poll、epoll示例](https://blog.csdn.net/lisong694767315/article/details/51328062)
