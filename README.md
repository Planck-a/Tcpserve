# Tcpserve
本项目为C++11编写的基于select模型的多线程、高并发服务器，服务器端对客户端的登录信息和其他信息进行简单回复，解决了TCP粘包问题，同时基于多线程的服务器可以每秒收发包效率达到20万条，此外实现了内存池+对象池+线程池
