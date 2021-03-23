# 准备：JS基础

## 0. 前端工程师是一份怎么样的职业

[https://www.cnblogs.com/lvdabao/p/5229640.html](https://www.cnblogs.com/lvdabao/p/5229640.html)

## 1. js 的执行上下文

* 执行上下文是代码执行的环境，包括全局执行上下文、函数执行上下文、Eval函数执行上下文。
* 执行上下文三个重要属性： 变量对象（variable object\)、作用域链、this指向

  ```javascript
  var scope = "global scope";
  function checkscope(a){
      var scope = "local scope";
      function f(){
          return scope;
      }
      return f();
  }
  checkscope(1);

  =================执行上下文======================
  checkscopeContext = {
      AO: {
          arguments: {
              0:1
              length: 1
          },
          a:1,
          scope: undefined,
          f: reference to function f(){}
      },
      Scope: [AO, globalContext.VO],
      this: undefined
  }
  ================================================
  ```

* 执行上下文的创建阶段分为三个步骤：**this绑定、创建词法环境组件、创建变量环境组件** 。
* this一般指向全局或者被引用的对象，~~可以理解为上下文栈的上一个上下文。~~
* 本节参考：[https://github.com/mqyqingfeng/Blog/issues/8](https://github.com/mqyqingfeng/Blog/issues/8)

## 2. js 的变量对象：

* 函数的所有形参、函数声明、变量声明
* 进入执行上下文时，首先会处理函数的所有形参，其次是函数声明，最后是变量声明。
* 如果函数名称和已存在的变量（包括参数）相同，则完全替换；
* 如果变量名称跟已经声明的**形参或函数**相同，则变量声明不会干扰已经存在的这类属性。
* 补充一点，形参是形式参数，指参数那个符号变量，实参是实际参数，指参数的实际值。
* 本节参考：[https://github.com/mqyqingfeng/Blog/issues/5](https://github.com/mqyqingfeng/Blog/issues/5)

## 3. 作用域链：

* 当查找变量的时候，会先从**当前上下文的变量对象**中查找，如果没有找到，就会从父级**（词法层面上的父级）**\)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链。
* 函数创建时，首先复制父级的作用域链，再将自己初始化后的VO加入到作用域链中。
* 本节参考：[https://github.com/mqyqingfeng/Blog/issues/6](https://github.com/mqyqingfeng/Blog/issues/6)

## 4. 闭包

* 满足两个条件：即使创建它的上下文已经销毁，它依然存在（大部分情况是从函数中返回）；在代码中引用了自由变量。
* ```javascript
  var a=(function(i){
      function t(){
          return 2;
      }
      return function(){
          i=i+t();
          console.log(i);
      }
  })(1);
  a();
  ```

  如上，使用一个立即执行函数来返回一个闭包，立即执行函数执行后销毁，闭包仍可调用i的值。

  闭包总是可以获取到作用域链上自由变量的值。

* 本节参考：[https://github.com/mqyqingfeng/Blog/issues/9](https://github.com/mqyqingfeng/Blog/issues/9)

## 5. promise

* 首先，放一段代码，从代码中去理解这个东西

  ```javascript
  function runAsync1(){
      var p = new Promise(function(resolve, reject){
          //做一些异步操作
          setTimeout(function(){
              console.log('异步任务1执行完成');
              resolve('随便什么数据1');
          }, 1000);
      });
      return p;            
  }
  function runAsync2(data){
      var p = new Promise(function(resolve, reject){
          console.log(data);//做一些异步操作
          setTimeout(function(){
              console.log('异步任务2执行完成');
              resolve('随便什么数据2');
          }, 2000);
      });
      return p;            
  }
  function runAsync3(data){
      //匿名函数的简写方法
      var p = new Promise((resolve, reject)=>{
          console.log(data);//做一些异步操作
          setTimeout(function(){
              console.log('异步任务3执行完成');
              resolve('随便什么数据3');
          }, 2000);
      });
      return p;            
  }

  runAsync1()
  .then(runAsync2)
  .then(runAsync3)
  .then(function(data){
      console.log(data);
  });
  //等价于以下
  var a=runAsync1();
  var b=a.then(runAsync2);
  var c=b.then(runAsync3);
  c.then(function(data){
      console.log(data);
  });
  //a,b,c均为promise对象。由于then的机制，这三行代码是顺序执行的。
  ```

* 首先认识到，Promise\(\)是一个构造函数，有一个形参，接受一个函数对象。使用new Promise\(\)构造一个Promise对象。
* 传入promise的函数一般有两个形参，分别为resolve和reject，这两个参数分别表示异步操作执行成功后的**回调函数**和异步操作执行失败后的**回调函数**。
* Promise\(\)的原型是Promise,有then\(\), catch\(\)等函数。
* **then\(func\)**函数在runAsync这个异步任务执行完成之后被执行，这是promise结构最主要的特性。then的参数同样是一个函数，函数有一个参数，then会为其传入promiseResult。
* catch函数即接收reject的值，也接收代码异常。
* race函数和all函数的用法此处不详述。
* 本节参考：[https://www.cnblogs.com/lvdabao/p/es6-promise-1.html](https://www.cnblogs.com/lvdabao/p/es6-promise-1.html)

## 6. async

* 调用异步函数时会返回一个 promise 对象。

  ```javascript
  async function test () {
    return 'ok'
  }
  test().then(console.log, console.error)//ok
  //then函数可以传入两个函数对象（注意：函数对象是不带（）的），
  //分别对应reslove和reject。
  ```

* await: 使异步函数暂停执行并等待 promise 解析传值后，继续执行异步函数并返回解析值

  ```javascript
  function sleep (second) {
    return new Promise((resolve, reject) => {
      setTimeout(resolve, second * 1000)
    })
  }
  async function test () {
    console.log(new Date())
    await sleep(3)
    console.log(new Date())
  }
  test()
  ```

* 本节参考：[https://objcer.com/2017/10/11/Async-Await/](https://objcer.com/2017/10/11/Async-Await/)

## 7. 深浅拷贝

* 赋值运算符 `=` 实现的是浅拷贝，只拷贝对象的引用值；
* JavaScript 中数组和对象自带的拷贝方法都是“首层浅拷贝”；
* `JSON.stringify` 实现的是深拷贝，但是对目标对象有要求；
* 若想真正意义上的深拷贝，请递归。
* 本节参考：[https://github.com/axuebin/articles/issues/20](https://github.com/axuebin/articles/issues/20)

## 8. 一份学习路线

![v2-06a105b6357971a34736c65a43867479\_720w](https://github.com/iamrice/MyGitbook/tree/f437d6b713cc69dd3fbc521866eec03f8f022438/job-interview/C:/Users/woshi/Desktop/娱乐图片/v2-06a105b6357971a34736c65a43867479_720w.jpg)

## 9. eventLoop

* macro-task\(宏任务\)：包括整体代码script，setTimeout，setInterval
* micro-task\(微任务\)：Promise，process.nextTick
* 不同类型的任务会进入对应的Event Queue，比如`setTimeout`和`setInterval`会进入相同的Event Queue。
* 进入整体代码\(宏任务\)后，开始第一次循环。接着执行所有的微任务。然后再次从宏任务开始，找到其中一个任务队列执行完毕，再执行所有的微任务。
* 异步任务进入Event Table后会注册函数，当指定的事情完成时，Event Table会将这个函数移入Event Queue。这里的注册函数通常指回调函数，例如settimeout的第一个参数、ajax的success函数等。
* 对于`setInterval(fn,ms)`来说，不是每过`ms`秒会执行一次`fn`，而是每过`ms`秒，会有`fn`进入Event Queue。一旦`setInterval`的回调函数`fn`执行时间超过了延迟时间`ms`，那么就完全看不出来有时间间隔了。

## 10. 函数式编程

* 区别于命令式编程、面向对象编程
* 特点：
  * 声明式编程
  * 惰性执行：函数只在需要的时候执行，即不产生无意义的中间变量。
  * 数据不可变：它要求你所有的数据都是不可变的，这意味着如果你想修改一个对象，那你应该创建一个新的对象用来修改，而不是修改已有的对象。**没有副作用**
  * 无状态： 主要是强调对于一个函数，不管你何时运行，它都应该像第一次运行一样，给定相同的输入，给出相同的输出，完全不依赖外部状态（例如全局变量，this 指针，IO 操作等）的变化。
  * 满足以上两点的函数称为**纯函数**
* 柯里化curry：将一个多元函数，转换成一个依次调用的**单元函数**。在定义函数是定义一个curry对象即可。

```text
const add = R.curry((x, y, z) =>  x + y + z);
const add7 = add(7);
add(7)(1) // function
```

* 函数组合compose、pipe
* 流水线构建：
  * curry化把要操作的数据放在最后一个参数
  * 函数组合要求函数单输入
* 本节参考：[https://juejin.cn/post/6844903936378273799\#heading-14](https://juejin.cn/post/6844903936378273799#heading-14)

## 11. 箭头函数

* \(args\)=&gt;{statement}
* \(args\)=&gt;result;
* \(\)=&gt;{statement};
* args=&gt;{statement}
* \(...args\)=&gt;{console.log\(...args\)}

## 12.  提纲

![155de5fd3a30f0d478315f4918099230453](https://github.com/iamrice/MyGitbook/tree/f437d6b713cc69dd3fbc521866eec03f8f022438/job-interview/D:/我/gitbook/images/155de5fd3a30f0d478315f4918099230453.png)

## 13. js的变量提升

* 所有的声明（function, var, let, const, class）都会被“提升”。function sayHi\(\) {} 会提升function。 var helloWorld = function\(\){} 会提升var。只有使用var关键字声明的变量才会被初始化undefined值，而let和const声明的变量则不会被初始化值，状态为uninitialized,class同理。此时称为**Temporal Dead Zone**
* 本节参考：[https://juejin.cn/post/6844903895341219854](https://juejin.cn/post/6844903895341219854)  

## 14. DOM 基本操作

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

## 15. html标签

* ​    div, p, h1~h5, ul, li, ol, a, img

## 16. this指向

* this始终指向调用它的对象
* 在new的作用下，构造函数的this指向赋值的对象
* call， apply：改变this指向，call有多个参数，apply只有两个参数，第二个参数为数组。
* 箭头函数没有this，this是继承的，所以this取决于上下文中的this。
* react中this绑定的三种方法
  * this.func=this.func.bind\(this\) //绑定this
  * onClick={\(\)=&gt;this.func\(\)}  //在class上下文中调用，this指向class
  * func=\(\)=&gt;{...}  //箭头函数没有this，因此this继承了当前上下文，即class
* 只有函数才关心this指向，因为函数是一个新的上下文。

## 17. canvas

* ```text
  var canvas = document.getElementById('canvas');
  var ctx = canvas.getContext('2d');
  ```
* fillrect, stokerect, clearrect
* beginpath, closepath,
* stroke, fill,
* moveto, lineto, arc, arcto
* fillStyle
* measureText

