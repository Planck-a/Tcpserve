* [INetEvent]()
* [CELLServe](https://github.com/Planck-a/serve-in-cpp/blob/master/code/CELLServer.hpp)

```cpp
class CELLServer
{
private:
	
	std::map<SOCKET, CELLClient*> _clients;//正式客户队列	
	std::vector<CELLClient*> _clientsBuff;//缓冲客户队列，加锁，取出放到正式客户队列中	
	std::mutex _mutex;//缓冲队列的锁
  bool _clients_change = true;//客户列表是否有变化
	fd_set _fdRead_bak;//备份客户socket fd_set	
	INetEvent* _pNetEvent;//网络事件对象
	CELLTaskServer _taskServer;
	
	SOCKET _maxSock;	
	time_t _oldTime = CELLTime::getNowInMilliSec();//旧的时间戳	
	CELLThread _thread;	
	int _id = -1;
	
	
public:
	CELLServer(int id);//id号标识第几个cellserve
  ~CELLServer();
  void setEventObj(INetEvent* event);//INetEvent,设置网络事件，
  void Close();//
  void OnRun(CELLThread* pThread);//核心，循环处理网络消息，用select判断整个fdread,更新后的fdRead调用ReadData()和write();
  void ReadData(fd_set& fdRead);//检查整个fdRead，挨个调用CELLClient的[RecvData()]()，触发OnNetRecv，OnNetMsg
  void WriteData(fd_set& fdWrite);//调用CELLClient的[SendDataReal()]()
  void CheckTime();//心跳检测，看正式队列中的csock是否都在线，还活着
  void Start()；//启动_taskServer，同时启动Onrun()线程
  
  virtual void OnNetMsg(CELLClient* pClient, netmsg_DataHeader* header);//调用的是EasyTCPServe中的事件处理函数，由于最终是Serve创建的对象，
  //所以不只是计数，而是调用Serve中的方法
  void OnClientLeave(CELLClient* pClient)；
  

}
```
* ` setEventObj本类对4个网络事件的处理方法，在EasyTCPServe中初始化化时有ser->setEventObj(this),所以最终调用的是EasyTCPServe中的事件处理函数，
由于最终是Serve创建的对象，所以不只是计数，而是调用Serve中的方法。`
* `把缓冲区中的csock拿到正式队列中，如果客户端有变化的话，把正式队列的csock加入到fdRead中，并备份，如果没变化的话，
则直接把备份的内容拿给fdRead，省去了一次遍历`
