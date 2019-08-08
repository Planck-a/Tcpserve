1、epoll的三个函数
---
(1)epoll_creat:创建一个文件描述符来标识一个事件表（一棵红黑树）和一个链表，红黑树 用mmap在内核和用户空间共享内存

  int epoll_create(int size); // 创建epoll句柄 

(2) epoll_ctl：往事件表上注册/修改/删除事件，并给这个fd注册一个回调函数，这个回调函数会把就绪的fd加入一个就绪链表
  
  int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event); // 事件注册函数

(3)epoll_wait：在就绪链表中查看有没有就绪的fd

  int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
  
  
  2、epoll相比起select的改进
  ---
  （1）每次注册新的事件到epoll句柄中时（在epoll_ctl中指定EPOLL_CTL_ADD），会把所有的fd拷贝进内核，而不是在epoll_wait的时候重复拷贝。由于mmap共享内存机制，epoll保证了每个fd在整个过程中只会拷贝一次。	
  
  （2）不用遍历整个fd_set，而是当设备就绪，，就会调用这个回调函数，而这个回调函数会把就绪的fd加入一个就绪链，唤醒等待队列上的等待者时，等待者只需要检查就绪队列即可
  
  （3）没有文件描述符数量的限制，在1GB内存的机器上大约是10万左右个文件描述符
