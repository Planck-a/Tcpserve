# Tcpserve
- 项目简介：本项目为C++11编写的基于select模型的多线程、高并发服务器，服务器端对客户端的登录信息和其他信息进行简单回复，解决了TCP粘包问题，同时基于多线程的服务器可以每秒收发包效率达到20万条，此外实现了内存池+对象池+线程池
- 项目目的：巩固C++语法和编码规范、学习了C++11智能指针和auto关键字的使用、学习网络编程技术和TCP/IP协议相关知识、熟悉select/poll/epoll的概念区别和使用场景，学习了多线程间同步，select支持跨平台所以也学习了Linux相关命令，除此之外用git进行项目管理，熟悉了git版本控制和内部机制。

示意图如下：
![1](https://github.com/Planck-a/Tcpserve/blob/master/img/%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%A8%A1%E5%9E%8B%E7%A4%BA%E6%84%8F%E5%9B%BE.jpg)
