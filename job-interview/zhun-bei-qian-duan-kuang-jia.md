# 准备：库/框架

## jQuery

1.  jQuery 屏蔽了浏览器之间的兼容性问题，针对常用功能封装了大量的 API，并支持插件机制，让你写 JS 的效率很高，质量很好。
2. jQuery还提供了CSS选择器、样式读写、事件绑定与执行、动画等特性，后来又加入了ajax、promise等，再加上方便的插件编写机制，对整个js的生态圈产生了重大的影响，可以说是js历史上影响力最大的一个库。
3. JSON全称JavaScript Object Notation

## NodeJS

1. Node.js是用来做什么的？ - 厂长的回答 - 知乎 [https://www.zhihu.com/question/33578075/answer/56951771](https://www.zhihu.com/question/33578075/answer/56951771)
2. 后端node.js和前端js有什么区别
   1. 一个是基于浏览器端的 javascript （前端 JS），一个是基于服务端的 javascript （后端 Node.js），语法一样，组成不一样。
   2.  JavaScript：
      1. ECMAScript（语言基础，如：语法、数据类型结构以及一些内置对象）
      2. DOM（一些操作页面元素的方法）
      3. BOM（一些操作浏览器的方法）
   3. Node.js：
      1. ECMAScript（语言基础，如：语法、数据类型结构以及一些内置对象）
      2. OS（操作系统）
      3. file（文件系统）
      4. net（网络系统）
      5. database（数据库）

## React  

### 生命周期函数

1. componentWillMount
2. componentDidMount
3. shouleComponentUpdate\(nextProps,nextState\){return boolean;}

   触发此函数有两种可能：父组件传递的props有变化，当前组件的state有变化。  
   两个参数分别指这两种情况，传入的参数是整个props和整个state。  
   如果返回true，则更新数据并重新渲染到虚拟 DOM 上  
   如果返回false，不更新数据，不渲染。

4. componentWillUpdate
5. componentDidUpdate
6. componentWillUnmount
7.  componentWillReceiveProps

### 虚拟DOM

virtual DOM 是 DOM 在内存的体现/副本，本质是JavaScript 对象。react 在一个**事件流程（？）**内将 DOM 的修改合并在虚拟 DOM 中，通过diff 算法计算修改方案后，一次性修改到 DOM 中。

### 组件状态

setState 在事件处理函数内部是异步的，如果parent 和 child 在同一个click事件都调用了setState，那react会在浏览器事件结束后批量更新，保证child不会被渲染两次。（注：this.state 和 this.props 都代表已经渲染了的值，因此，this.state 的更新意味着组件渲染发生。是的，即使渲染函数内部并没有用到state或props，仍然会渲染到虚拟DOM上，但经过diff算法比对后不会渲染到真实DOM上）

渲染两次这个可以这么想，触发组件渲染的条件除了第一次加载，还有父组件props更改和自身state更改，所以如果在事件内部同步更新，那子组件自然就会渲染两次。

在渲染没有发生之前，使用this.state往往会得到”错误的值“，可以通过往setstate的参数传入函数解决这个问题。 

### diff 算法

