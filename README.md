# -前端知识点
#### 1.js基本数据类型
五大基本类型： String, Number, Boolean, Null, Undefined (es6引入Symbol) 存放的是实际值

#### 2.js引用数据类型

引用数据类型的变量保存的是对它的引用。即指针，(object,array,function)

#### 3.基本数据类型和引用数据类型的区别
区别两种类型的直接特征：存储位置
* 如果某种变量是直接存储在栈(stack)中的简单数据段，即为基本数据类型，他们的值直接存储
在变量的访问位置
* 如果是存储在堆(heap)中的对象，则为引用值类型
#### 4.栈和堆的区别
* 栈的空间小，但是可以被解释器直接检索，响应速度快。
* 堆空间大，但不能被程序栈直接检索，响应速度稍慢。

#### 5.垃圾回收机制
JavaScript具有自动进行垃圾回收的机制。原理是：找出不再使用的变量，然后释放掉其占用的内存
垃圾回收器会按照固定的时间间隔周期性的执行，这里不再使用的变量只能是局部变量，全局变量的
生命周期直至浏览器卸载页面才会结束。局部变量只在函数执行过程中存在，而在这个过程中会为局
部变量在堆或者栈中分配相应的空间，直到函数结束。一旦函数结束，局部变量没有必要存在了，就
可以释放它们的内存。

#### 6.内存优化
对于全局变量而言，JavaScript不确定他在后面还能不能被用到，所以从他声明开始就一直存在于内存
中，直到手动释放或者关闭页面，这导致了不必要的内存消耗，
1. 使用立即执行函数(通过定义一个匿名函数，创建了一个新的函数作用域，相当于创建了一个“私有”的命名空间，该命名空间的变量和方法，不会破坏污染全局的命名空间。此时若是想访问全局对象，将全局对象以参数形式传进去即可)
2. 对于某些需要一直存在的变量，我们可以将他挂载在window下
	(function(window){
	    window.xx
	 })(window)
3. 闭包容易产生的问题，我们可以采用回调函数来代替闭包访问内部变量，回调函数的好处是：执行完成后会自动释放其中的变量

#### 7.闭包
简单讲，闭包就是指有权访问另一个函数作用域中的变量的函数。
* 闭包的作用域链包含着它自己的作用域，以及包含它的函数的作用域和全局作用域。
* 通常，函数的作用域及其所有变量都会在函数执行结束后被销毁。但是，在创建了一个闭包以后，这个函数的作用域就会一直保存到闭包不存在为止。
* 闭包只能取得包含函数中任何变量的最后一个值，这是因为闭包所保存的是整个变量对象，而不是某个特殊的变量。
* 匿名函数最大的用途是创建闭包，并且还可以构建命名空间，以减少全局变量的使用。从而使用闭包模块化代码，减少全局变量的污染。
* 闭包的缺点就是常驻内存会增大内存使用量，并且使用不当很容易造成内存泄露。
* 如果不是因为某些特殊任务而需要闭包，在没有必要的情况下，在其它函数中创建函数是不明智的，因为闭包对脚本性能具有负面影响，包括处理速度和内存消耗。

#### 8.构造函数
构造函数与其他函数唯一区别，就在于调用它们的方式不同。不过，构造函数毕竟也是函数，不
存在定义构造函数的特殊语法。任何函数，只要通过 new 操作符来调用，那它就可以作为构造函数；而
任何函数，如果不通过 new 操作符来调用，那它跟普通函数也不会有什么两样。
使用构造函数的主要问题，就是每个方法都要在每个实例上重新创建一遍。

#### 9.原型对象
无论什么时候，只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个 prototype
属性，这个属性指向函数的原型对象。所有原型对象都会自动获得一个 constructor
（构造函数）属性，这个属性包含一个指向 prototype 属性所在函数的指针。

#### 10.原型链
原型链是实现继承的主要方法。每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数想指针(constructor)，而实例对象都包含一个指向原型对象的内部指针(__proto__)。如果让原型对象等于另一个类型的实例，此时的原型对象将包含一个指向另一个原型的指针(__proto__)，另一个原型也包含着一个指向另一个构造函数的指针(constructor)。假如另一个原型又是另一个类型的实例……这就构成了实例与原型的链条。

	function a () {
		this.type = 'xxx'
	}

	var b = new a()

	原型链
	b.__proto__ === a.prototype
	a.prototype.__proto__ === Object.prototype
	Object.prototype.__proto__ === null

#### 11.继承
	// 定义一个动物类
	function Animal (name) {
	  // 属性
	  this.name = name || 'Animal';
	  // 实例方法
	  this.sleep = function(){
	    console.log(this.name + '正在睡觉！');
	  }
	}
	// 原型方法
	Animal.prototype.eat = function(food) {
	  console.log(this.name + '正在吃：' + food);
1.原型链继承
核心：将父类的实例作为子类的原型

	function Cat(){ 
	}
	Cat.prototype = new Animal();
	Cat.prototype.name = 'cat';
特点：
	1.非常纯粹的继承关系，实例是子类的实例，也是父类的实例
	2.父类新增原型方法/原型属性，子类都能访问到
	3.简单，易于实现










