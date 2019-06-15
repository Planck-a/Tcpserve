```cpp
//网络事件接口,纯虚函数，
class INetEvent
{
public:	
	virtual void OnNetJoin(CELLClient* pClient) = 0;//客户端加入事件  
	virtual void OnNetLeave(CELLClient* pClient) = 0;	//客户端离开事件	
	virtual void OnNetMsg(CELLServer* pServer, CELLClient* pClient, netmsg_DataHeader* header) = 0;//客户端消息事件	
	virtual void OnNetRecv(CELLClient* pClient) = 0;//recv事件
};
```
