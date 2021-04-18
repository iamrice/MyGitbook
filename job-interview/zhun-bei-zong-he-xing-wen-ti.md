---
description: 这一部分比较杂，不属于特别原理性的知识点
---

# 准备：综合性问题

### 判断数组的方法

1. constructor
2. Array.isArray
3. instance of
4. object.prototype.toString.call\(\)

### 面向对象、面向过程、函数式编程

面向对象是指将特定的属性和方法封装到对应的类中，在主函数调用时，无需关心方法的具体实现。  
面向对象的三大特性是：封装、继承、多态。

面向过程是具体化的，流程化的。

函数式编程，把所有方法封装在纯函数中，通过流水线的方式执行逻辑。  
纯函数的两大特性，无副作用和无状态（时不变），可以很好地提高编程效率，提高代码质量，降低维护难度。  
ES6 推出的箭头函数，没有 this 指向，这保证了函数的无状态。  
React 强推 Hook ，鼓励开发者用 pure function 编写组件。但我认为，react 组件不算真正意义上的纯函数，毕竟还是需要维护状态的。没有副作用倒是真的。

### 二叉树：层序遍历

力扣：[https://leetcode-cn.com/problems/binary-tree-level-order-traversal/submissions/](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/submissions/)  
一层遍历使用两个队列交替记录；两层遍历使用一个队列记录  
js 数组的 shift 方法可以取出第一个元素并移除队列

### 二叉树：后序遍历

使用递归方法比较简单。  
使用栈记录可以在循环内完成遍历。

### ES6 class 与 ES5 function

class 的功能完全可以用 ES5 的函数和原型链实现

```text
// ES6 class
class A{
    static a=1;
    b=2;
}

class B extends A{
    constructor(){
        super();
        this.c=5;
    }
    func1(){
        console.log(this.constructor.a,this.b,this.c)
    }
}

// test example
b=new B()
b.func1()

//ES5 function
function A(){
    this.b=2;
}
A.a=1;

function B(){
    A.call(this);
    this.c=5;
}
B.prototype=A.prototype

B.prototype.func1=function(){
    console.log(this.constructor.a,this.b,this.c)
}
```

