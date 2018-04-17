# js-
基础知识总结
1.this的详细解释
this 代表当前正在执行某个方法的对象，如果没有当前方法（或该方法不属于任何其它对象），则是指全局对象。也就是说，this代表调用该方法对象的引用。
var name = "aaa"; 在全局中name == this.name;由于没有其他调用，所以this是指向全局的。
主要讨论函数中的this的问题
首先es5中函数有三种调用方式：function(p1,p2)  obj.child.method(p1,p2)  function.call(context,p1,p2);其中第三种才是最完整的调用方式
其他的两种可以转化成 function(p1,p2) 等价于 function.call(undefined,p1,p2);  obj.child.method(obj.child,p1,p2)
而this就是函数调用中的context，对等的，只要清楚其中的context即可知道this代表的对象
对一般函数的写法的解析过程：function func(){console.log(this);} func();等价于 function func(){console.log(this)} func.call(undefined)
由于浏览器规定，当打印出传入的context不是一个对象的时候，那么window对象就是默认的context，在es6严格模式为undefi
所以上述函数执行结果是 window对象，如果不要打印出window对象，那么调用时 func.call(obj)，则打印出的就是obj
同样的，我们对另一种方式进行转化：var funcObj = {foo:function(){console.log(this)}} funcObj.foo()
变形为 funcObj.foo.call(obj);
面试题：var a = 10; function test(){a = 5;alert(a);alert(this.a);var a;alert(this.a);alert(a);}
执行 test() 的结果依次为: 5 10 10 5
执行 new test()的结果依次为:5 undefined undefined 5
---------------------------------------------------------------------------------------------------------------------------------------------
2.javascript中的原型链
