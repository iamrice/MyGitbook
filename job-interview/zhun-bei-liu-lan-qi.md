# 准备：浏览器

## DOM 基本操作

* 查找节点
  * getElementById, getElementsByClassName, getElementsByTagName, getElementsByName                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
  * document/element.querySelector, document/element.querySelectorAll
  * document.documentElement, document.body
* 新建节点
  * createElement
  * createAttribute, createTextNode, createComment, createDocumentFragment
* 添加新节点
  * appendChild, insertBefore, setAttribute, setAttributeNode
* 删除节点
  * removeChild, removeAttribute, removeAttributeNode
* 修改节点
  * replaceChild

## 回流

### 引起reflow的原因

1. 页面第一次渲染
2. DOM树变化
3. Render树变化
4. 浏览器窗口resize
5. 获取元素的某些属性

### 引起repaint的原因

1. reflow
2. 背景色、颜色、字体改变

### 减少以上两种回流的方法

1. 用transform做形变和位移
2. 避免逐个修改节点样式
3. 将需要多次修改的DOM设为display:none,即将其移出render tree，等修改后再加入
4. 避免多次读取某些属性

## 事件机制

这个其实比较简单，三个过程：捕获、目标阶段、冒泡。

stopPropagation 阻止冒泡  
preventDefault 阻止默认事件  
return false 阻止两者（但我在chrome的试验中，两者都没有阻止）

事件回调函数：  
element.addEventListener\('click',func,false\);  
element.removeEventListener\('click',func,false\);

## 缓存

http 缓存主要分为强缓存和协商缓存，二者是可以共同存在的。

### **强缓存**

是指服务端对一个资源设定过期时间，客户端根据过期时间判断再次加载时是否向服务端发出请求。强缓存设计的参数有 cache-control，expires，二者的主要区别是时间的表达方式，前者设置的是资源寿命长度，后者设置的是资源过期时间。后者在前后端时间不统一的情况下会出现不符合预期的情况。资源过期时间一般由服务端设置，但如果前端坚持把请求中的cache-control 改为no-cache 的话，也是可以向服务端发送请求的。例如 ctrl + f5 可以强制刷新页面，不使用缓存。

**cache-control :** max-age=60，no-cache（不可强缓存）,no-store（不可复用）,private/public（代理服务器是否可以缓存）  
参数含义：[https://www.jianshu.com/p/54cc04190252](https://www.jianshu.com/p/54cc04190252)  
客户端和服务端设置 cache-control 的区别：[https://www.jianshu.com/p/f7ccad796058](https://www.jianshu.com/p/f7ccad796058)

![](../.gitbook/assets/image%20%2811%29.png)

### **协商缓存**

如果强缓存被设置为no-cache，或是资源过期，那么客户端会向服务端发送请求。这个过程涉及到了**协商缓存**，服务端可以在响应报文头部加入 last-modify和 Etag 字段，分别代表资源最后修改时间和资源哈希编码。客户端缓存这一信息之后，在下一次请求时，就可以将这两个信息放在字段 if-modified-since 和 if-not-match 中，服务端可以根据这两个信息比对当前资源的情况。一般来说哈希码更精确，但计算复杂度大。如果修改时间在服务端没有被记录、或者分布式服务器对修改时间的记录不一致、或者资源在一秒内被修改了多次，这些情况需要使用哈希码来对比更准确。服务端是优先校验Etag的。协商缓存虽然不能避免请求-响应，但是可以在适当的时候减小响应文的长度，也是对性能提升有帮助的。如果服务端确认资源可以复用，会向客户端返回 304 响应码，客户端会去disk cache 中取出缓存。

### 最佳实践

对于频繁变动的资源，设置 cache-control: no-cache，通过last modified 和 Etag 去判断是否使用缓存。  
对于不常改变的资源，设置 cache-control为一个很大的值，当资源变动时，则通过修改url 来达到更新的目的。

### 浏览器缓存

浏览器缓存的优先级以此为 service worker, memory cache, disk cache, push cache。

内存和硬盘缓存比较好理解，一个比较快，一个空间大。浏览器缓存的不仅仅是文件，还包括缓存规则。  


![](../.gitbook/assets/image%20%2810%29.png)

