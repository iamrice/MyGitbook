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

#### **React.PureComponent 与 React.Component 很相似。两者的区别在于 React.Component 并未实现 shouldCo**mpon**entUpdate\(\)，而 React.PureComponent 中以浅层对比 prop 和 state 的方式来实现了该函数。**

虽然官网说”浅拷贝“、“忽略子组件的更新”等弊端，但我在测试中发现purecomponent好像没有这些问题，可能是因为框架更新了（？）官网说，purecomponent适合用于简单的展示组件，不适用于复杂组件。

### 事件机制

（我突然意识到一个事情，其实虽说onclick是在父组件定义的，但实际上它是作为props的一部分传入组件的，就算是button，button也是一个组件）

**合成事件**

**React** 为了抹平不同浏览器的差异，重新封装了事件对象。因此， React 组件的事件不由浏览器调用，而是集中到React 自定义的事件分发函数中一并调用。

**事件注册**

**事件回调**

**事件分发**

1. React 组件的事件绑定与原生DOM 事件绑定不同，后者将事件函数绑定在DOM 结点上，而前者使用事件委托的方式，将所有事件函数绑定在document上。注册方式是在document结点上addEventListener（'click', DispathchEvent\)，dispatchEvent 是React定义的事件分配函数。
2. React 有一个事件集合 listenerBank ,通过键值对的方式储存 React 元素与事件函数的对应关系。同一种事件类型的函数会被存在同一个 Bank 对象。
3. 浏览器中，事件流程分为捕获、目标对象、冒泡三个阶段，由于dispathEvent注册时没有指定第三个参数，所以React 事件是发生在冒泡阶段的。
4. 当事件发生时，从windows对象向下直到目标对象，再向上冒泡直到document对象这段流程中，普通的事件函数会按照原生的方式执行。随后，触发dispatchEvent 函数，此函数会完成所有 react 事件函数的执行。在这之后，事件继续冒泡，执行document、windows上绑定的事件函数。（当然，在document 上dispatchEvent 和其他事件函数的先后顺序还是要看注册的顺序，不一定谁先谁后）
5. 在dispatchEvent 函数内部，首先他会根据nativeEvent的事件类型生成对应的合成事件，然后找到目标对象，从目标对象向父级元素逐个执行 react 事件。

**为什么需要这样的机制**

### 生命周期函数

1. componentWillMount
2. componentDidMount
3. shouleComponentUpdate\(nextProps,nextState\){return boolean;}

   触发此函数有两种可能：~~父组件传递的props有变化~~，（勘误：只要父组件重新渲染，无论props是否改变，当前组件都会触发此函数），当前组件的state有变化。  
   两个参数分别指这两种情况，传入的参数是整个props和整个state。  
   如果返回true，则更新数据并重新渲染到虚拟 DOM 上  
   如果返回false，不更新数据，不渲染。当父组件props没有改变时，应该返回false，此处涉及到浅对比和深对比。因为react没有帮忙做这个对比，所以我认为最后是做个深层比较吧。

4. componentWillUpdate
5. componentDidUpdate
6. componentWillUnmount
7.  componentWillReceiveProps

### 虚拟DOM

virtual DOM 是 DOM 在内存的体现/副本，本质是JavaScript 对象。react 在一个**事件流程**后将 DOM 的修改合并在虚拟 DOM 中，通过diff 算法计算修改方案后，一次性修改到 DOM 中。虚拟 DOM 发生在渲染函数被调用和元素在屏幕上显示之间。

### 组件状态

setState 在事件处理函数内部是异步的，如果parent 和 child 在同一个click事件都调用了setState，那react会在浏览器事件结束后批量更新，保证child不会被渲染两次。（注：this.state 和 this.props 都代表已经渲染了的值，因此，this.state 的更新意味着组件渲染发生。是的，即使渲染函数内部并没有用到state或props，仍然会渲染到虚拟DOM上，但经过diff算法比对后不会渲染到真实DOM上）

渲染两次这个可以这么想，触发组件渲染的条件除了第一次加载，还有父组件props更改和自身state更改，所以如果在事件内部同步更新，那子组件自然就会渲染两次。

在渲染没有发生之前，使用this.state往往会得到”错误的值“，可以通过往setstate的参数传入函数解决这个问题。 

总结：react 在组件渲染和真实 DOM 修改设了两道坎，降低工作量，提高效率。

### diff 算法

