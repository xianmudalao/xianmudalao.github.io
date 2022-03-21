---
title: 模块化
date: 2022-01-10 16:01:31
tags:
---

### 一、模块化的理解

#### 1.什么是模块化
* 将一个复杂的程序依据一定的规则(规范)封装成几个模块,并组合在一起
* 块的内部数据与实现是私有的,向外部仅暴露一些接口与外部其他模块通信


#### 2.模块化的进化的过程
* 全局function模式:将不同的功能封装成不同的全局函数
    * 编码:将不同的功能封装成不同的全局函数
    * 问题:污染全局命名空间,容易引起命名冲突或数据不安全,模块之前没有依赖关系
    * 外部函数可以直接修改内部变量
```javascript
function m1(){
    //...
}
function m2(){
    //...
}
```

* namespace模式:简单对象的封装
    * 作用:减少了全局变量,解决命名冲突
    * 问题:数据不安全,外部可以直接修改内部数据
```javascript
let myModule = {
    data:'demo',
    foo(){
        console.log(this.data)
    },
    bar(){
        console.log(`bar()${this.data}`)
    }
}
myModule.data = 'change data' // 直接修改模块内数据
myModule.foo() // change data
```


* IIFE模式:匿名函数自调用(闭包)
    * 作用:数据是私有的,外部只能通过暴露的方法访问模块内部方法 
    * 编码:通过将数据的行为和方法封装到函数的内部,通过给window对象挂载到全局暴露接口
    * 问题:模块间的依赖关系


```javascript
// index.html文件
<script type="text/javascript" src="module.js"></script>
<script type="text/javascript">
    myModule.foo()
    myModule.bar()
    console.log(myModule.data) //undefined 不能访问模块内部数据
    myModule.data = 'xxxx' //不是修改的模块内部的data
    myModule.foo() //没有改变
</script>
```

```javascript
// module.js文件
(function(window){
    let data = 'demo'
    // 操作数据的函数
    function foo(){
        // 暴露当前模块内部函数
        console.log(`${data}`)
        private()
    }

    function private(){
        // 内部私有函数
        console.log('private()')
    }

    window.myModule = { foo }
})(window)
```

#### 3.模块化的好处
* 避免命名冲突(减少命名空间的污染)
* 更好的分离,按需加载
* 更高复用性
* 高可维护性

### 二、模块化规范

#### 1.CommonJS

#### (1)概述
Node应用由模块组成,采用CommonJS规范.每个文件就是一个模块,有自己的作用域.在一个文件里面定义的变量、函数、类都是私有的,对其他文件不可见.在服务器端,模块的加载是运行时同步加载的;在浏览器端,模块是需要提前打包编译处理的.

#### (2)特点
* 所有代码都是在模块作用域,不会污染全局作用域
* 模块可以多次加载,但只在第一次加载时运行一次.然后运行结果就会被缓存下来,之后在加载,就直接读取缓存结果.要想让模块再次运行,必须清除缓存
* 模块加载的顺序,按照其在代码中出现的顺序

#### (3)基本语法
* 暴露模块:module.exports = xxx 或 exports.xxx = xxx
* 引入模块:require(xxx),如果是第三方模块,xxx是模块名;如果是自定义模块,则是相对的文件路径

#### 2.AMD
CommonJS规范加载是同步的,只有加载完成,才能执行后面的操作.AMD规范是非同步加载模块,允许指定回调函数.由于Node.js主要用于服务端编程,模块一般都存于本地硬盘,所以加载起来比较快,不用考虑非同步加载的方式.但是如果是浏览器环境,要从服务端异步加载模块,这就必须要采用非同步模式.

#### 3.CMD
CMD规范专门用于浏览器端的,模块的加载是异步的,只有在模块使用时才会加载执行.CMD整合了CommonJS和AMD的规范的特点.

#### 4.ES6模块化
ES6模块的设计思想是尽量的静态化,使得编译时就能确定模块的依赖关系,以及输入和输出的变量.CommonJS和AMD模块都只能在运行时才能确定依赖关系.比如CommonJS模块就是对象,输入时必须查找对象的属性.

#### ES6与CommonJS模块的差异
他们有两个重大差异:
①CommonJS模块输出的是一值的拷贝,ES6模块输出的是一个值的引用
②CommonJS模块是运行加载的,ES6模块是编译时输出的接口


### 三、总结
* CommonJS规范主要用于服务端编程,加载模块是同步的,这并不适合浏览器环境,因为同步意味着会导致加载阻塞,浏览器资源是异步加载的,因此有了AMD和CMD解决方案.
* AMD规范在浏览器环境中是异步加载模块,而且可以并行加载多个模块.但是AMD开发成本高,代码阅读和书写较为复杂,模块定义方式的语义不太流畅.
* CMD规范与AMD相似,都用与浏览器编程,依赖就近,延迟执行,可以很容易在node.js中运行.依赖SPM打包,模块的加载逻辑偏重.
* ES6在语言标准的层面上,实现了模块功能,而且实现得相当简单.




