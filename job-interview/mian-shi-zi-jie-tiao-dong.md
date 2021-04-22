# 面试：字节跳动

1. 304 是什么意思
2. 操作系统内存管理的堆和栈
   1. 栈：存储函数的返回地址、临时变量、
   2. 堆：程序中动态申请的存储空间
3. 网络模型
   1. OSI：物理层、数据链路层、网络层、传输层、会话层、表示层、应用层
   2. TCP/IP：数据链路层（network access\)、网络层、传输层、应用层
4. cookie 和 local stroage
5. map 和 foreach 的区别：map 有返回值
6. 识别url 参数

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

