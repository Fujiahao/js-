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
原型分为隐式原型 _proto_
	       显式原型 prototype  这个是方法原型对象所特有的
	通过原型找到自己的父级，子代可以使用父级的方法

	new Foo() 做了哪些事情
	1.首先构建一个空的对象
	2.把空对象上的隐式原型指向构造函数的原型对象
	3.调用构造函数的call，传递context
	4.把整个对象返回给对象

	对象的隐式全部指向Object的原型对象，Object的原型对象指向于怒null

	Object.create()做的事情
	1.创造一个完全是空的函数 任何属性都没有挂载，只有一个construct
	2.使空函数F指向Foo，然后执行new F的过程，注意此处调用的call不是Foo的call，而是F的call

	函数的原型是直接指向Object的原型对象，对象的原型对象是先指向函数的原型对象

	new和Object.create的不同：
	function Foo(){
		this.xxx = "xxxx"; // 这个属性是属于Foo独有的
	}

	var newfoo = new Foo();
	var createfoo = Object.create(Foo);

	console.log(newfoo.xxx);   //输出 "xxxx"  //Foo.call(Foo)
	console.log(createfoo.xxx); //输出undefined  //createfoo.call(Foo)注意此处的Foo为Object.create创造的Foo，是一个空的

	Foo.xxx = "xxx"; //全局挂载xxx属性，这个属性是属于Foo的上级的，即Foo原型对象，而非Foo内部的this调用的
	console.log(createfoo.xxx); //输出"xxx"  //createfoo的隐式原型指向了Foo的原型对象

	Foo.prototype.yyy = "yyy";  //挂载到Foo的peototype上，他的父级是挂在了Function的原型对象上
	console.log(newfoo.yyy);   //输出 "xyyy" //call（Foo）
	console.log(createfoo.yyy); //输出undefined //由于挂在到了Function的原型对象上，所以无法找到



	function Fn(){
		name:"lsd",
		age:18
	}
	var createFn1 = Object.create(Fn);
	var createFn2 = Object.create(Fn);

	console.log(createFn1.name+" "+createFn1.age); //lsd 18
	console.log(createFn2.name+" "+createFn2.age); //lsd 18

	createFn1.name = "lihua";
	console.log(createFn1.name+" "+createFn1.age); //lihua 18
	console.log(createFn2.name+" "+createFn2.age); //lsd 18
