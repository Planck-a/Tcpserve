```cpp
class CELLBuffer
{
private:
	char* _pBuff = nullptr;	//第二缓冲区 发送缓冲区	
	//list<char*> _pBuffList;//可以用链表或队列来管理缓冲数据块	
	int _nLast = 0;//缓冲区的数据尾部位置，已有数据长度	
	int _nSize = 0;//缓冲区总的空间大小，字节长度
	int _fullCount = 0;//缓冲区写满次数计数
  
public:
    CELLBuffer(int nSize = 8192);
    ~CELLBuffer();
    bool push(const char* pData, int nLen);//返回值表示满了没有
    void pop(int nLen);//弹出一定量的数据
    int write2socket(SOCKET sockfd);////缓冲区有数据,则调用send
    int read4socket(SOCKET sockfd);// 接收客户端数据
        
}
```
```cpp
class CELLClient
{
private:
	
	SOCKET _sockfd;	// 客户端sock
	CELLBuffer _recvBuff;//第二缓冲区 接收消息缓冲区
	CELLBuffer _sendBuff;	//发送缓冲区	
	time_t _dtHeart;//心跳死亡计时	
	time_t _dtSend;//上次发送消息数据的时间 	
	int _sendBuffFullCount = 0;//发送缓冲区遇到写满情况计数
  
public:
    int id = -1;//所属serverid	
	  int serverId = -1;
    CELLClient(SOCKET sockfd = INVALID_SOCKET):	_sendBuff(SEND_BUFF_SZIE),	_recvBuff(RECV_BUFF_SZIE);
    ~CELLClient();
    int RecvData();//调用read4socket(_sockfd);
    int SendDataReal();//立即将发送缓冲区的数据发送给客户端,调用.write2socket(_sockfd);
    int SendData(netmsg_DataHeader* header);//push进_sendBuff
    bool checkHeart(time_t dt);//心跳检测
    bool checkHeart(time_t dt);//超时的话，立即将发送缓冲区的数据发送出去，SendDataReal
    
}
```
