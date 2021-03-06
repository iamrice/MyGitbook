<h1>JS基础</h1>

#### 0. 前端工程师是一份怎么样的职业

https://www.cnblogs.com/lvdabao/p/5229640.html

#### 1. js 的执行上下文

   - 执行上下文是代码执行的环境，包括全局执行上下文、函数执行上下文、Eval函数执行上下文。

   - 执行上下文三个重要属性： 变量对象（variable object)、作用域链、this指向

     ```js
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
                     length: 0
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

   - 执行上下文的创建阶段分为三个步骤：**this绑定、创建词法环境组件、创建变量环境组件** 。

   - this一般指向全局或者被引用的对象，可以理解为上下文栈的上一个上下文。

   - 本节参考：https://github.com/mqyqingfeng/Blog/issues/8

#### 2. js 的变量对象：

   - 函数的所有形参、函数声明、变量声明
   - 进入执行上下文时，首先会处理函数声明，其次会处理变量声明，如果如果变量名称跟已经声明的形式参数或函数相同，则变量声明不会干扰已经存在的这类属性。
   - 本节参考：https://github.com/mqyqingfeng/Blog/issues/5

#### 3. 作用域链：

   - 当查找变量的时候，会先从**当前上下文的变量对象**中查找，如果没有找到，就会从父级**（词法层面上的父级）**)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链。
   - 函数创建时，首先复制父级的作用域链，再将自己初始化后的VO加入到作用域链中。
   - 本节参考：https://github.com/mqyqingfeng/Blog/issues/6

#### 4. 闭包：

   - 满足两个条件：即使创建它的上下文已经销毁，它依然存在（大部分情况是从函数中返回）；在代码中引用了自由变量。

   - ```js
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

   - 本节参考：https://github.com/mqyqingfeng/Blog/issues/9

#### 5. promise:

   - 首先，放一段代码，从代码中去理解这个东西

     ```js
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

     

   - 首先认识到，Promise()是一个构造函数，有一个形参，接受一个函数对象。使用new Promise()构造一个Promise对象。

   - 传入promise的函数一般有两个形参，分别为resolve和reject，这两个参数分别表示异步操作执行成功后的**回调函数**和异步操作执行失败后的**回调函数**。

   - Promise()的原型是Promise,有then(), catch()等函数。

   - **then(func)**函数在runAsync这个异步任务执行完成之后被执行，这是promise结构最主要的特性。then的参数同样是一个函数，函数有一个参数，then会为其传入promiseResult。

   - catch函数即接收reject的值，也接收代码异常。

   - race函数和all函数的用法此处不详述。

   - 本节参考：https://www.cnblogs.com/lvdabao/p/es6-promise-1.html

#### 6. async

   - 调用异步函数时会返回一个 promise 对象。

     ```js
     async function test () {
       return 'ok'
     }
     test().then(console.log, console.error)//ok
     ```

   - await: 使异步函数暂停执行并等待 promise 解析传值后，继续执行异步函数并返回解析值

     ```js
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

   - 本节参考：https://objcer.com/2017/10/11/Async-Await/

#### 7. 深浅拷贝

   - 赋值运算符 `=` 实现的是浅拷贝，只拷贝对象的引用值；
   - JavaScript 中数组和对象自带的拷贝方法都是“首层浅拷贝”；
   - `JSON.stringify` 实现的是深拷贝，但是对目标对象有要求；
   - 若想真正意义上的深拷贝，请递归。
   - 本节参考：https://github.com/axuebin/articles/issues/20

#### 



 

