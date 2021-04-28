# 面试：字节跳动

## 一面

1. 304 是什么意思
   1. 协商缓存命中时，服务端返回304响应，客户端接收后去缓存中取出资源。
2. 进程和线程的概念。
   1. 进程是程序运行的基本单位，它包含程序运行所需的代码，数据，资源
   2. 线程是作用于CPU 上的实体，不同线程之间共享地址空间，线程包含线程ID，program counter, register set and a stack.
3. 操作系统内存管理的堆和栈
   1. 栈：存储函数的返回地址、临时变量、
   2. 堆：程序中动态申请的存储空间
4. 网络模型
   1. OSI：物理层、数据链路层、网络层、传输层、会话层、表示层、应用层
      1. 物理层和数据层负责数据在物理层面上的传输
      2. 网络层通过路由协议和网络协议，找到数据从起点到终点的传输路经
      3. 传输层作用于通讯实体，介于应用层和网络层之间，
   2. TCP/IP：数据链路层（network access\)、网络层、传输层、应用层
5. cookie 和 local stroage

   我猜，面试官把这两个放在一起，其实是想考察我知不知道把用户的密码缓存起来。来提示了那么多，可惜，我确实没这么做过，所以也不好说，不然一深入就完蛋了。

6. map 和 foreach 的区别：map 有返回值
7. 数组的方法：map, filter, reduce, foreach, slice, join, concat, includes, indexOf
8. String 的方法：split, match, slice
9. 识别url 参数

   我在面试时是暴力分割。先用？分开路经和参数，然后用&分开参数，然后用=分开参数名和值，太不优雅了。

```text
iter=a.matchAll(/[a-zA-Z0-9_]+=[a-zA-Z0-9_]+/g)
while(true){
    item=iter.next()
    if(item.done==true)
        break;
    para=item.value[0];
    [key,value]=para.split('=')
    console.log(key,value)
}
```



