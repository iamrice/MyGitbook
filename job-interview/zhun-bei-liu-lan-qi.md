# 准备：浏览器

## 1. DOM 基本操作

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

## 2. 引起reflow的原因

1. 页面第一次渲染
2. DOM树变化
3. Render树变化
4. 浏览器窗口resize
5. 获取元素的某些属性

## 3. 引起repaint的原因

1. reflow
2. 背景色、颜色、字体改变

## 4. 减少以上两种回流的方法

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

## HTTP 缓存

http 缓存主要分为强缓存和协商缓存，二者是可以共同存在的。

强缓存是指服务端对一个资源设定过期时间，客户端根据过期时间判断再次加载时是否向服务端发出请求。强缓存设计的参数有 cache-control，expires，二者的主要区别是时间的表达方式，前者设置的是资源寿命长度，后者设置的是资源过期时间。后者在前后端时间不统一的情况下会出现不符合预期的情况。

**cache-control :** max-age=，no-cache,no-store,private/public,

  


