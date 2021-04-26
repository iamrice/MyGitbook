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

### Component & PureComponent

#### **React.PureComponent 与 React.Component 很相似。两者的区别在于 React.Component 并未实现 shouldComponentUpdate\(\)，而 React.PureComponent 中以浅层对比 prop 和 state 的方式来实现了该函**数**。**

虽然官网说”浅拷贝“、“忽略子组件的更新”等弊端，但我在测试中发现purecomponent好像没有这些问题，可能是因为框架更新了（？）官网说，purecomponent适合用于简单的展示组件，不适用于复杂组件。

### 事件机制

本节参考：[https://www.cnblogs.com/forcheng/p/13187388.htm](https://www.cnblogs.com/forcheng/p/13187388.htm)

**合成事件**

React 为了抹平不同浏览器的差异，重新封装了事件对象。因此， React 组件的事件不由浏览器调用，而是集中到React 自定义的事件分发函数中一并调用。

**事件注册**

React 组件的事件绑定与原生DOM 事件绑定不同，后者将事件函数绑定在DOM 结点上，而前者使用事件委托的方式，将所有事件函数绑定在document上。

注册方式是在document结点上addEventListener（'click', DispathchEvent\)，dispatchEvent 是React定义的事件分配函数。

**储存事件回调函数**

React 有一个事件集合 listenerBank ,通过键值对的方式储存 React 元素与事件函数的对应关系。同一种事件类型的函数会被存在同一个 Bank 对象。

在 初次加载渲染阶段，React 会检查每个元素的 props 上是否有事件回调，如果有，将其添加到对应的EventBank，并且在document 注册对应事件的回调。虽然dispatchEvent会被多次注册，但因为回调函数是同一个，所以实际上只会保留一个。

**事件分发流程**

1. 触发事件，开始 DOM 事件流，先后经过三个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段
2. 当事件冒泡到 document 时，触发统一的事件分发函数  `ReactEventListener.dispatchEvent`
3. 根据原生事件对象（nativeEvent）找到当前节点（即事件触发节点）对应的 ReactDOMComponent 对象
4. 事件的合成
   1. 根据当前事件类型生成对应的合成对象
   2. 封装原生事件对象和冒泡机制
   3. 查找当前元素以及它所有父级
   4. 在 listenerBank 中查找事件回调函数并合成到 events 中
5. 批量执行合成事件（events）内的回调函数
6. 如果没有阻止冒泡，会将继续进行 DOM 事件流的冒泡（从 document 到 window），否则结束事件触发

### **高阶组件**

一个入参和返回值为react组件的函数是高阶组件。

常用于抽离组件间共用的功能，例如获取鼠标点击坐标。高阶组件为入参的组件包裹了一层父级组件，维护可抽离的特性，在渲染阶段将 props 和 state 一并传给子组件。

应该还有其他功能没了解到，先占个坑。有个文章如此总结：高阶组件可用于代码重用、逻辑和引导抽象 ；渲染劫持；state 抽象和操作； props 处理

### Render props

render props 同样可以用来抽离复用逻辑，但它的做法是在实例组件的渲染函数内插入抽象组件，与高阶组件恰恰相反。高阶组件是将抽象组件的状态传给实例组件，而render props是将实例组件的渲染函数传给抽象组件。

当然，除了通过props 的属性传递渲染内容，也可以在抽象组件的开闭标签之间传递渲染内容，不过这需要抽象组件主动获取props.child。

参考：[https://zhuanlan.zhihu.com/p/111873208](https://zhuanlan.zhihu.com/p/111873208#:~:text=%E9%AB%98%E9%98%B6%E7%BB%84%E4%BB%B6%EF%BC%88HOC%EF%BC%89%E6%98%AF%20React%20%E4%B8%AD%E7%94%A8%E4%BA%8E%E5%A4%8D%E7%94%A8%E7%BB%84%E4%BB%B6%E9%80%BB%E8%BE%91%E7%9A%84%E4%B8%80%E7%A7%8D%E9%AB%98%E7%BA%A7%E6%8A%80%E5%B7%A7%E3%80%82%20HOC%20%E8%87%AA%E8%BA%AB%E4%B8%8D%E6%98%AF,React%20API%20%E7%9A%84%E4%B8%80%E9%83%A8%E5%88%86%EF%BC%8C%E5%AE%83%E6%98%AF%E4%B8%80%E7%A7%8D%E5%9F%BA%E4%BA%8E%20React%20%E7%9A%84%E7%BB%84%E5%90%88%E7%89%B9%E6%80%A7%E8%80%8C%E5%BD%A2%E6%88%90%E7%9A%84%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E3%80%82%20%E5%8F%AF%E8%83%BD%E4%B9%8B%E5%89%8D%E6%B2%A1%E6%9C%89%E8%AF%A6%E7%BB%86%E4%BA%86%E8%A7%A3%E8%BF%87HOC%E7%9A%84%E6%9C%8B%E5%8F%8B%EF%BC%8C%E7%9C%8B%E5%88%B0%E8%BF%99%E4%B8%AA%E5%90%8D%E8%AF%8D%E4%BC%9A%E6%AF%94%E8%BE%83%E9%99%8C%E7%94%9F%EF%BC%8C%E4%BB%A5%E4%B8%BA%E8%87%AA%E5%B7%B1%E6%B2%A1%E7%94%A8%E8%BF%87%EF%BC%8C%E5%85%B6%E5%AE%9E%EF%BC%8CHOC%E6%97%A9%E5%B7%B2%E5%9C%A8%E4%BD%A0%E7%9A%84%E9%A1%B9%E7%9B%AE%E4%B8%AD%E5%B9%BF%E6%B3%9B%E8%A2%AB%E7%94%A8%E5%88%B0%EF%BC%8C%E5%8F%AA%E4%B8%8D%E8%BF%87%E6%98%AF%E4%BD%A0%E6%B2%A1%E6%9C%89%E7%95%99%E5%BF%83%E6%B3%A8%E6%84%8F%E8%80%8C%E5%B7%B2%E3%80%82)

### **Hook**

Hook 只能用于 函数组件，其最大的优势在于代码的可复用性高。

在传统的 class component 中，由于事件回调和状态必须在 class 中定义，使得代码的可复用性很低。高阶组件和 render props 可以解决此问题，他们的基本思路是将原本一个组件拆分为两个组件，一个为个性化的实例组件，另一个为共用的抽象组件，两个组件嵌套在一起可以实现完整的功能。

Hook 的出现提供了一种新的解决思路，Hook 可以不使用组件来维护一个状态，这得益于 useState 和 useEffect。对于需要复用的代码逻辑，可以创建一个自定义Hook useXXX， 在函数内通过useState 创建维护一组状态，使用useEffect 设计状态的更新机制，例如添加事件监听器、跟踪异步事件的流程。

参考：[https://blog.csdn.net/fengqiuzhihua/article/details/103511209](https://blog.csdn.net/fengqiuzhihua/article/details/103511209)

### 生命周期函数

1. componentWillMount
2. componentDidMount
3. **shouleComponentUpdate\(nextProps,nextState\){return boolean;}**

   触发此函数有两种可能：~~父组件传递的props有变化~~，（勘误：只要父组件重新渲染，无论props是否改变，当前组件都会触发此函数），当前组件的state有变化。  
   两个参数分别指这两种情况，传入的参数是整个props和整个state。  
   如果返回true，则更新数据并重新渲染到虚拟 DOM 上  
   如果返回false，不更新数据，不渲染。当父组件props没有改变时，应该返回false，此处涉及到浅对比和深对比。因为react没有帮忙做这个对比，所以我认为最好是做个深层比较吧。

4. componentWillUpdate
5. componentDidUpdate
6. componentWillUnmount
7.  componentWillReceiveProps

### 组件状态

setState 在事件处理函数内部是异步的，如果parent 和 child 在同一个click事件都调用了setState，那react会在浏览器事件结束后批量更新，保证child不会被渲染两次。（注：this.state 和 this.props 都代表已经渲染了的值，因此，this.state 的更新意味着组件渲染发生。是的，即使渲染函数内部并没有用到state或props，仍然会渲染到虚拟DOM上，但经过diff算法比对后不会渲染到真实DOM上）

渲染两次这个可以这么想，触发组件渲染的条件除了第一次加载，还有父组件props更改和自身state更改，所以如果在事件内部同步更新，那子组件自然就会渲染两次。

在渲染没有发生之前，使用this.state往往会得到”错误的值“，可以通过往setstate的参数传入函数解决这个问题。 

总结：react 在组件渲染和真实 DOM 修改设了两道坎，降低工作量，提高效率。

参考：[https://zh-hans.reactjs.org/docs/faq-state.html\#what-does-setstate-do](https://zh-hans.reactjs.org/docs/faq-state.html#what-does-setstate-do)

### 虚拟DOM

virtual DOM 是 DOM 在内存的体现/副本，本质是JavaScript 对象。react 在一个**事件流程**后将 DOM 的修改合并在虚拟 DOM 中，通过diff 算法计算修改方案后，一次性修改到 DOM 中。虚拟 DOM 发生在渲染函数被调用和元素在屏幕上显示之间。

### diff 算法

diff 和渲染函数可以说是同步进行的，一个组件如果在shouldComponentUpdate 中返回了 false ，那意味着该组件不进行渲染，也意味着不进行 diff 对比。

**tree diff**

只对比同一层级的元素。也就是说，通过先序遍历的方式调用 component diff。  
之所以在将 tree diff 作为三大组成之一，是因为，每一个 react  元素都会生成一个树状结构，可能不止一层，这些子节点中，有的是html 元素，可以直接对比，有的是 react 元素，调用 component diff 对其对比。

**component diff**

对于一个元素，（猜想）如果是基础元素，那直接比较innerHtml。  
如果是 react 元素：如果类型不同，则删除添加新元素，如果类型相同，则触发 shouldCompnentUpdate 。

对于子元素，如果子元素没有key 属性（默认为空），则对子组件进行component diff；如果key 属性不为空，则进行element diff 。

**element diff**

三种操作：移动、增加、删除

1. 遍历新的列表
   1. 如果在旧列表中找到了对应的元素，对比旧列表的位置索引 index 和当前 lastindex（初始化为0）
      1. index &gt;= lastindex，不移动，更新lastindex为index
      2. index &lt; lastindex，将元素移动到 lastindex 的位置
   2. 如果找不到对应元素，则在lastindex 位置添加新元素
2. 遍历旧的列表，如果有未出现在新列表的元素，则删除。

不能用index 作为 key，万万不能，后果是渲染队列基本不移动，组件状态会有错误。

数组渲染要加key，否则，每个element 都要单独进行 diff。

