五种I/O模型对比

一、分为同步和异步

IO整个的过程归纳起来，分为两步：1 等待数据就绪， 2 数据搬迁。 

同步：

- 阻塞
- 非阻塞
- 信号驱动
- 多路转接

异步：

简单来说：同步I/o就是自己等（自己发起的，自己进行数据迁移）；

		异步I/O就是别人帮忙自己等（自己发起，别人帮忙数据迁移）；

五种i/o模型

1. 阻塞I/O
   1. 在数据准备就绪之前什么都不做，相当于被挂起，直至等待数据就绪；
   2. 程序调用recv接口这个过程是：
      1. 应用程序调用recv接口
      2. 操作系统没有准备好数据，应用程序进行等待；
      3. 数据准备就绪，应用程序进行数据拷贝，返回；
   3. 优点：
      1. 简单，一般默认情况下套接字都是阻塞式的；
      2. 数据能够及时返回，不延迟；
   4. 缺点：性能比较差；
2. 非阻塞io
   1. 如果数据没有就绪，直接返回，需要以轮训的方式来检测数据有没有就绪；
   2. 图示：俩人去海底捞；
   3. 应用程序调用recv接口过程
      1. 应用程序调用recv接口
      2. 操作系统没有准备好数据，应用程序直接返回；
      3. 过一段时间再去检测数据是否准备，若还没有再返回；
      4. 直至检测时数据已经准备就绪，应用程序进行数据拷贝，返回；
   4. 优点：可以再等待期间做别的事情；
   5. 缺点：需要轮训的方式进行等待，很麻烦；
3. 信号驱动
   1. 再数据就绪的时候，操作系统发送SIGIO的信号告诉应用程序数据准备就绪；
   2. 调用recv接口过程：
      1. 应用程序建立SIGIO信号处理程序；
      2. 操作系统等待数据就绪时候，递交SIGIO信号；
      3. 应用程序调用recv接口；
      4. 完成数据拷贝，返回；
   3. 优点：在等待数据就绪期间可以做别的事情了；
   4. 缺点：需要建立维护信号处理程序；
4. 多路转接
   1. 在等待数据就绪的过程中，不再一次等待一个文件描述符，而是一次等待多个文件描述符，其中任何一个准备就绪都可以进行数据拷贝搬迁；
   2. 调用recv接口，这个过程是：
      1. 应用程序调用select/poll/epoll接口；
      2. 操作系统等待数据接续，返回；
      3. 应用程序调用recv接口
      4. 完成数据拷贝，返回；
   3. 优点：一次可以等待多个文件描述符，缩短了等待时间使得IO更加高效；
   4. 为了实现多路转接需要消耗部分资源；
5. 异步IO
   1. 在操作系统完成数据搬迁之后通知应用程序，应用程序本身不参加io的相关细节；
   2. 在网络中的整个过程是：
      1. 应用程序调用接口；
      2. 操作系统等待数据，数据准备就绪；
      3. 操作系统完成数据拷贝；
      4. 操作系统想应用程序递交相关信号，告知已完成数据拷贝；
   3. 优点：应用程序不需要参与IO的整个过程 都由内核完成；
   4. 缺点：异步io的简单只是对于API的使用者而言；但是很多系统都不支持异步io；
