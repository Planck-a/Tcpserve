[Thread](https://github.com/Planck-a/Tcpserve/blob/master/code/CELLThread.hpp)
class CELLThread
{
private:
	 typedef std::function<void(CELLThread*)> EventCall;
   //C++11提供的function,相当于函数指针，用来接收lamada匿名函数，该匿名函数要满足返回值为空、参数为CELLThread*
   EventCall _onCreate;
	 EventCall _onRun;
	 EventCall _onDestory;//三个函数的入口地址，由上层编写的lamada表达式传给start()
	
	std::mutex _mutex;//不同线程中改变数据时需要加锁
	CELLSemaphore _sem;	//控制线程的终止、退出	
	bool	_isRun = false;//线程是否启动运行中
   
public:
    void Start(	EventCall onCreate = nullptr,	EventCall onRun = nullptr,EventCall onDestory = nullptr);//启动线程,确定三个函数的入口地址
    void Close();//关闭线程
    void OnWork();//依次执行_onCreate，_onRun，_onDestory，当Onrun()循环执行结束后，意味着线程结束，这时会调用wakeup解除阻塞关闭线程
    void Close();//调用wait阻塞，等待解除
}

```cpp
关于lamada表达式和function的使用：

    typedef std::function<void(CELLThread *)> function;
    void func(function f)
    {
       f(this);
    }
    
    void main()
    {
      func( [this](CELLThread *p){ Onrun() });//捕获this指针，使得{}中可以调用本类的方法
    }
 ```
