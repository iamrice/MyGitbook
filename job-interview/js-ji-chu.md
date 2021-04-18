# 准备：JavaScript 基础

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
          },//arguments是一个类数组
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

###  this指向

* this始终指向调用它的对象
* 在new的作用下，构造函数的this指向赋值的对象
* call， apply：改变this指向，call有多个参数，apply只有两个参数，第二个参数为数组。
* 箭头函数没有this，this是继承的，所以this取决于上下文中的this。
* react中this绑定的三种方法
  * this.func=this.func.bind\(this\) //绑定this
  * onClick={\(\)=&gt;this.func\(\)}  //在class上下文中调用，this指向class
  * func=\(\)=&gt;{...}  //箭头函数没有this，因此this继承了当前上下文，即class
* 只有函数才关心this指向，因为函数是一个新的上下文。

## 2. js 的变量对象

* 函数的所有形参、函数声明、变量声明
* 进入执行上下文时，首先会处理函数的所有形参，其次是函数声明，最后是变量声明。
* 如果函数名称和已存在的变量（包括参数）相同，则完全替换；
* 如果变量名称跟已经声明的**形参或函数**相同，则变量声明不会干扰已经存在的这类属性。举个例子，如果一个函数的参数是 a ，如果函数内声明了 var a=4;，首先在变量提升阶段，var a 不会干扰到形参，在函数执行阶段，a=4 可以成功修改形参的值。这一点表现在arguments（形参总是和arguments同步）。
* 补充一点，形参是形式参数，指参数那个符号变量，实参是实际参数，指参数的实际值。
* 本节参考：[https://github.com/mqyqingfeng/Blog/issues/5](https://github.com/mqyqingfeng/Blog/issues/5)

###  js的变量提升

* 所有的声明（function, var, let, const, class）都会被“提升”。function sayHi\(\) {} 会提升function。 var helloWorld = function\(\){} 会提升var。只有使用var关键字声明的变量才会被初始化undefined值。
* 本节参考：[https://juejin.cn/post/6844903895341219854](https://juejin.cn/post/6844903895341219854)  
* 观察下列代码：

```text
// var 造成的内存泄露
for(var i=0;i<5;i++){
    setTimeout(()=>{
        console.log(i);
    },1000);
}
//5 5 5 5 5

for(let i=0;i<5;i++){
    setTimeout(()=>{
        console.log(i);
    },1000);
}
//0 1 2 3 4 
```

* 从作用域链的角度思考什么两段代码，第一段，var i 声明后被提升到当前上下文的变量中，因此在settimeout回调函数执行时，他们直接找到了上下文中的变量环境，因此输出是5.
* 第二段代码中，let i 声明在for内的块级作用域中，因此回调函数在块级作用域内就找到了i，因此输出的值是遍历的值。
* 另外，如果变量声明没有关键字，那么声明的是全局变量，即使在构造函数中，也是指向全局！

## 3. 作用域链

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

* 本节参考：[https://github.com/mqyqingfeng/Blog/issues/9](https://github.com/mqyqingfeng/Blog/issues/9)5. 

## 5. JS 异步解决方案

### **六大方法**

1. callback
2. 事件监控
3. 发布订阅
4. promise
5. async
6. 生成器

### **insight**

真正让JS执行脚本不阻塞的是AJAX、setTimeout等异步方法，是他们让出主线程，等任务完成后再将回调函数加入任务队列，他们和 eventloop 共同造就了js的异步特性。而 promise 等异步方案的作用在于为这些异步方法返回的结果进行处理，如果是同步的脚本代码完全不需要promise 。

### promise

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
* Promise\(\)的原型是Promise.prototype,有then\(\), catch\(\),final\(\)等函数。
* **then\(func\)**函数在runAsync这个异步任务执行完成之后被执行，这是promise结构最主要的特性。then的参数同样是一个函数，函数有一个参数，then会为其传入promiseResult。
* catch函数即接收reject的值，也接收代码异常。
* race函数和all函数的用法此处不详述。
* 本节参考：[https://www.cnblogs.com/lvdabao/p/es6-promise-1.html](https://www.cnblogs.com/lvdabao/p/es6-promise-1.html)

### async

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

* await 捕捉错误

```text
function test () {
    getJSON().then((res) => {
      var data = JSON.parse(res) // 此处可能出错
    }).catch((err)=>{
    ...
    })
}


async function test () {
  try {
    var data = JSON.parse(await getJSON())
  } catch (e) {
    ...
  }
}
```

* await 链式调用

```text
function test () {
  var value1
  var value2
  return promise1()
    .then(vle => {
        value1 = vle
        return promise2(value1)
    })
    .then(vle => {
        value2 = vle
        return promise3(value1, value2)
    })
}

async function test () {
  var value1 = await promise1()
  var value2 = await promise2(value1)
  return promise3(value1, value2)
}
```

* 从上面可以看出，async和await的搭配可以大大简化代码，使得代码更加美观！
* 本节参考：[https://objcer.com/2017/10/11/Async-Await/](https://objcer.com/2017/10/11/Async-Await/)

![](../.gitbook/assets/image%20%288%29.png)

## 7. 深浅拷贝

* 赋值运算符 `=` 实现的是浅拷贝，只拷贝对象的引用值；
* 深浅拷贝只针对引用数据类型，即object和function（理论上function是原型也是object），number和string属于基本数据类型
* JavaScript 中数组和对象自带的拷贝方法都是“首层浅拷贝”；
* `JSON.stringify` 实现的是深拷贝，但是对目标对象有要求；
* 若想真正意义上的深拷贝，请递归。
* 本节参考：[https://github.com/axuebin/articles/issues/20](https://github.com/axuebin/articles/issues/20)
* 以下为代码实现，参考了一篇写的非常好的文章：[https://juejin.cn/post/6844903929705136141\#heading-9](https://juejin.cn/post/6844903929705136141#heading-9)
* 我没有实现函数的拷贝，因为我觉得不同变量引用同一个函数完全没有问题。

```text
//part 1: 判断对象类型
function getType(obj){
    return Object.prototype.toString.call(obj).slice(8,-1);
    //返回值为形如"[object Array]"的字符串
}

//part 2: 定义不可遍历类型
notItrType=['Number','String','Undefined','Null','Boolean','Symbol','Function']

//part 3: 定义可遍历类型
ItrType=['Array','Set','Map','Object']

//part 4: 定义类型初始化
function getInit(obj){
    let func=obj.constructor;
    return new func();
}

//part 5: 可遍历类型的拷贝
function ItrClone(obj,res){
    let type=getType(obj);
    if(type=='Array'){
        for(item of obj){
            res.push(deepClone(item));
        }
    }
    else if(type=='Set'){   
        for(item of obj){
            res.add(deepClone(item));
        }
    }
    else if(type=='Map'){   
        obj.forEach((value,key)=>{
            res.set(key,deepClone(value));
        });
        //其实foreach的使用挺简单的，array和set不使用到key，所以没用这个。
    }
    else if(type=='Object'){
        for(item of Object.keys(obj)){
            res[item]=deepClone(obj[item]);
        }
    }
    return res;
}        

//part 6: 拷贝函数
function deepClone(target){
    let type=getType(target);
    if(notItrType.includes(type)==true){
        return target;
    }else if(ItrType.includes(type)==true){
        return ItrClone(target);
    }else{
        //其他复杂类型，返回空
        return null
    }
}

//part 7: 循环引用
refer=new Map()
function deepClone(target){
    let type=getType(target);
    if(notItrType.includes(type)==true){
        return target;
    }else if(ItrType.includes(type)==true){
        if(refer.get(target))
            return refer.get(target); 
            
        var res=getInit(target);
        refer.set(target,res)        
         
        ItrClone(target,res);
        return res;
    }else{
        //其他复杂类型，返回空
        return null
    }
}
```

### JS的四种for循环方式

1. `for ... in` : 遍历对象的属性或数组的索引，如果对象的原型链上添加了新属性，那么新属性也同样会被遍历到。如果只想遍历对象本身的属性值，那么要搭配hasOwnProperty\(\)函数使用。
2. `for ... of` : 遍历可迭代对象（不包括字典）本身的属性值。
3. `arr.forEach(callback(value,key,arr){})` : 遍历可迭代对象本身的属性值。
4. `for key of Object.keys(obj)`：适用于所有对象，且返回的是对象本身的属性值，最好搭配for ... of 使用。
5. 参考：[https://segmentfault.com/a/1190000015619348](https://segmentfault.com/a/1190000015619348) [https://blog.csdn.net/wuyujin1997/article/details/88743955](https://blog.csdn.net/wuyujin1997/article/details/88743955)

## 8. 节流与防抖

防抖：当持续触发事件时，一定时间段内没有再触发事件，事件处理函数才会执行一次，否则将一直延时。

```text
function debounce(func,wait){
    var timeout=null
    return function(){
        var context=this;
        var args=arguments;
        if(timeout!=null){
            clearTimeout(timeout)
            timeout=null;
        }
        
        timeout=setTimeout(function(){
            func.apply(context,args);
            timeout=null;
        },wait)       
    }
}
//注意 this 指向的问题，凡是对函数进行封装的操作，都要注意还原其 this 指向
```

节流：当持续触发事件时，保证一定时间段内只调用一次事件处理函数。

```text
function throttle(func,wait){
    var timeout=null;
    return function(){
        if(timeout==null)
            timeout=setTimeOut(func,wait);
    }
}

-------------以上是错误示范----------------

function throttle(func,wait){
    var timeout=null;
    return function(){
        var context=this
        var args=arguments 
        //用args储存的原因是，settimeout的回调函数也有arguments，优先于这一层的arguments
        if(timeout==null){
            timeout=setTimeout(function(){
                func.apply(context,args);
                timeout=null
            },wait)
        }
    }
}

--------------以上是定时器版本-------------

function throttle(func,wait){
    var timeout=null;
    var pre=Date.now()
    return function(){
        var context=this
        var args=arguments
        var remain=wait-(Date.now()-pre)
        
        clearTimeout(timeout)
        
        if(remain<=0){
            func.apply(context,args);
            pre=Date.now()
        }else{
            timeout=setTimeout(function(){
                func.apply(context,args);
                pre=Date.now()
            },remain)
        }
    }
}

-------------以上是定时器+时间戳版本，保证了最后一次滚动能触发函数-----------
```

## 9. eventLoop

* macro-task\(宏任务\)：包括整体代码script，setTimeout，setInterval
* micro-task\(微任务\)：Promise，process.nextTick\(nodejs\)，promise.then/catch/final
* 不同类型的任务会进入对应的Event Queue，比如`setTimeout`和`setInterval`会进入相同的Event Queue。
* 进入整体代码\(宏任务\)后，开始第一次循环。接着执行所有的微任务。然后再次从宏任务开始，找到其中一个任务队列执行完毕，再执行所有的微任务。
* 异步任务进入Event Table后会注册函数，当指定的事情完成时，Event Table会将这个函数移入Event Queue。这里的注册函数通常指回调函数，例如settimeout的第一个参数、ajax的success函数等。
* 对于`setInterval(fn,ms)`来说，不是每过`ms`秒会执行一次`fn`，而是每过`ms`秒，会有`fn`进入Event Queue。一旦`setInterval`的回调函数`fn`执行时间超过了延迟时间`ms`，那么就完全看不出来有时间间隔了。
* eventLoop 造就了JavaScript的异步特性。

### promise 在 eventloop 中的执行

* new promise\(func\) 是立即执行的，但then，catch，final都属于微任务，异步执行
* 当promise链式调用时，从第一个then到链条完成会一并在一次eventloop中完成。
* 思考微任务执行顺序时，心里要有一个队列，举个例子，看下面的代码，”promise1“之后，微任务队列中有一个then，紧接着“then11”，“promise2”，此时微任务加了第二个then，但当前task还没执行完，当前task跳过then之后来到了return，return之后遇到下一个then，也加入队列。随后输出“then21”，“then12”，1，“then23”

```text
new Promise((resolve,reject)=>{
    console.log("promise1")
    resolve()
}).then(()=>{
    console.log("then11")
    new Promise((resolve,reject)=>{
        console.log("promise2")
        resolve()
    }).then(()=>{
        console.log("then21")
    }).then(()=>{
        console.log("then23")
    })
    return 1;
}).then((i)=>{
    console.log("then12",i)
})

作者：小美娜娜
链接：https://juejin.cn/post/6844903808200343559
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## 10. 函数式编程

* 区别于命令式编程、面向对象编程
* 特点：
  * 声明式编程
  * 惰性执行：函数只在需要的时候执行，即不产生无意义的中间变量。
  * **数据不可变**：它要求你所有的数据都是不可变的，这意味着如果你想修改一个对象，那你应该创建一个新的对象用来修改，而不是修改已有的对象。**没有副作用**
  * **无状态**： 主要是强调对于一个函数，不管你何时运行，它都应该像第一次运行一样，给定相同的输入，给出相同的输出，完全不依赖外部状态（例如全局变量，this 指针，IO 操作等）的变化。
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

```text
//柯里化实现
function curry(func,...args){
    console.log(args)
    if(args.length==func.length){
        return func.apply(this,args);
    }else{
        return function(...paras){
            return curry.apply(this,[func].concat(args.concat(paras)))
        }
    }
}
//很有趣，没有用到闭包，但用到了递归的思想
```

## 11. 箭头函数

* \(args\)=&gt;{statement}
* \(args\)=&gt;result;
* \(\)=&gt;{statement};
* args=&gt;{statement}
* \(...args\)=&gt;{console.log\(...args\)}
* 不绑定this和arguments

## 12. 内存泄露

定义：当一个内存不再被引用，但却没有被释放时，就造成了内存泄露

1. 意外的全局变量
2. 被遗忘的计时器或回调函数
3. 脱离DOM的引用
4. 闭包

### 垃圾回收机制

JavaScript有一个引用表，记录一个内存被引用的次数，当一个变量不再被引用时，内存会被释放。



## 13. == 和 ===

前者在比较前会强制类型转换，后者不做类型转换。后者要求比较对象的值和类型都相同，在日常使用中更受推荐。

## 14. 原型链

![](../.gitbook/assets/image%20%284%29.png)

* \_\_proto\_\_和constructor是对象独有的
* prototype是函数独有的
* 函数也是对象
* js七大数据类型：Number, String, Boolean, NULL, undefine, Symbol, Object（后加入bigInt）
* 其中，Object是引用类型，常用的有Array, Function, Date，Set，Map
* 下面这段代码介绍了一种继承方法，有助于原型链思考。来自：[https://www.php.cn/js-tutorial-410582.html](https://www.php.cn/js-tutorial-410582.html)
* 值得注意的是，平常看到的Object, Array等，都是构造函数，想要设置属性，请找到他们的prototype。
* 观察下列例子中的child函数，虽然a,b,c都有修改值，但a被修改时是this.a=2; 这个时候，child实例对象不会去原型链上找parent，而是给自己添加了a属性，而b，c被修改时都带有push或demo，不是一个声明新变量的形式，因此会去原型链上找对应属性。这就是为什么child1和child2只有a属性是不同的。

```text
function Parent() {
    this.a = 1;
    this.b = [1, 2, this.a];
    this.c = { demo: 5 };
    this.show = function () {
        console.log(this.a , this.b , this.c.demo );
    }
}

function Child() {
    this.a = 2;
    this.change = function () {
        this.b.push(this.a);
        this.a = this.b.length;
        this.c.demo = this.a++;
    }
}

Child.prototype = new Parent();
var parent = new Parent();
var child1 = new Child();
var child2 = new Child();

child1.a = 11;
child2.a = 12;

parent.show();//1,[1,2,1],5
child1.show();//11,[1,2,1],5
child2.show();//12,[1,2,1],5

child1.change();
child2.change();

parent.show();//1,[1,2,1],5
child1.show();//5,[1,2,1,11,12],5
child2.show();//6,[1,2,1,11,12],5
```

### instanceof

```text
function instance_of(l,r){
    let a=l.__proto__
    let b=r.prototype
    while(a!==null){
        if(a===b)
            return true;
        a=a.__proto__;
    }
    return false;
}
```

### js继承

* js没有class，只有构造函数和对象，但可以做到类似的功能，对象可储存变量和函数。
* 原型链继承 注：prototype 不能直接指向 SuperType.prototype，当子类有添加方法时，这样写会直接改变父类的方法。所以一般指向一个实例。

```text
function SuperType() {
	this.property = true;
}
SuperType.prototype.getSuperValue = function() {
	return this.property;
};
function SubType() {
	this.subproperty = false;
}
// 继承SuperType
SubType.prototype = new SuperType();
```

* 构造函数继承

```text
function SuperType(name) {
  this.colors = ["red","blue","green"];
  this.name = name;
 }
function SubType(name) {
  SuperType.call(this,name);
}
```

* 组合继承

```text
function SuperType(name){
	this.name = name;
	this.colors = ["red","blue","green"];
}
SuperType.prototype.sayName = function() {
	console.log(this.name);
};
function SubType(name, age){
	// 继承属性 第二次调用
	SuperType.call(this, name);
	this.age = age;
}
// 继承方法  第一次调用
SubType.prototype = new SuperType();
```

## 20. 模拟实现call, apply, bind

1. 模拟实现call \* 如果call的第一个参数是null或undefine，那this指向window \* 如果call的第一个参数是Number, String, Boolean, Symbol，则this指向特定的数据对象，如Number\(0\), Boolean\(true\). 虽然这样的指定没有太大意义，但这是合法的。 \* 如果call的第一个参数是Object（包含function），则是最常见的使用场景。

```text
Function.prototype.call2 = function (context) {
    var context = context || window;
    context.fn = this;

    var args = [];
    for(var i = 1, len = arguments.length; i < len; i++) {
        args.push('arguments[' + i + ']');
    }

    var result = eval('context.fn(' + args +')');
    // 如果是ES6，可以使用...arguments 来传参
    delete context.fn
    return result;
}

作者：冴羽
链接：https://juejin.cn/post/6844903476477034510
来源：掘金
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

2. 模拟实现bind  
    \* 这操作着实让人眼前一亮，我原本一直在疑惑，函数的this指向是js内核决定的，怎么把他bind到另一个对象上呢？这一看才知道，噢！源赖氏佐田（x），原来是返回一个闭包，在闭包内使用apply方法。  
    \*而且，有一点需要注意的是，bind函数是修改返回函数的this指向，不是对传入函数进行修改。  


```text
function bind2(context){
    var func=this
    var context=context || window
    return function(...args){
        func.apply(context,args)
    }
}
//这里使用...args而不使用arguments的原因是，后者是类数组，如果想将其转为真正的数组，需要
//Array.prototype.slice.call(arguments)

f.prototype.bind=bind2;

g=f.bind(obj)

链接：https://juejin.cn/post/6844903476623835149
```

```text
------------------可 new 可继承的 bind --------------
Function.prototype.bind =function(context){
    var func=this
    var context=context || window
    var res= function(...args){
        if(this instanceof res){
            func.apply(this,args)
        }else{
            func.apply(context,args)
        }
    }
    res.prototype=func.prototype
    return res;
}
```



