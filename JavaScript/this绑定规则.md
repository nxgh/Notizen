
## this
 
 - `this`的设计目的是在函数体内部，指代函数当前的运行环境；
 - `this`提供了更为优雅的机制来隐式地传递一个对象引用

在JavaScript中， 当一个函数被调用时，会创建一个活动记录（执行环境）。这个执行环境包含函数是从何处被调用的(call-stack)，函数是如何被调用的，被传递了什么参数信息。它的属性之一就是在函数执行期间将被使用的`this`引用

执行环境：
 - 全局执行环境
 - 函数执行环境
 - 每个执行环境包含三部分：变量对象/活动对象、作用域链、`this`的值

`this`的设计和内存里的数据结构有关

`var obj = { foo: 5 }`
这段代码将一个对象赋值给`obj`，JS引擎会先在内存里生成一个对象`{foo: 5}`, 然后把这个对象的内存地址赋值给变量`obj`，也就是说变量`obj`是一个reference。

当要读取`obj.foo`的时候，引擎先从`obj`拿到内存地址，然后再从该地址读出原始的对象，返回它的`foo`属性
```js
// foo 保存方式
// foo 属性的值保存在属性描述对象的value属性中
{
	foo: {
		[[value]]: 5
		[[writable]]: true
		[[enumerable]]: true
		[[configurable]]: true
	}
}

```

而当属性的值是一个函数的时候
`var obj = {foo: function() {} }`
引擎会将函数单独保存在内存中,然后再将函数的地址赋值给foo的value属性
```js
{
	foo: {
		[[value]]: 函数的地址
		...
	}
}
```

由于函数是一个单独的值，所以它可以在不同的上下文中执行
```js
var foo = function() {}
var obj = { foo: foo }
// 单独执行
foo()
// obj 环境执行
obj.foo()
```

此时就需要有一种机制，能够在函数体内部获得当前的运行环境(context)。所以`this`就出现了。


 - `this`的设计目的是在函数体内部，指代函数当前的运行环境；
 - `this`提供了更为优雅的机制来隐式地传递一个对象引用


```js
let foo = function() {
	console.log(this.x) // this.x 指当前运行环境的x
}
```

```js
// 显式地将环境对象传递给identify() 和 speak()
function identify(context) {
    return context.name.toUpperCase()
}

function speak(context) {
    var greeting = `Hello, I'm ${identify(context)}`
    console.log(greeting)
}

let me = {
    name: 'ME'
}

let you = {
    name: 'YOU'
}

console.log(identify(you)) // YOU
speak(me) // ME

```
```js
// 使用 this
function identify() {
	return this.name.toUpperCase();
}

function speak() {
	var greeting = "Hello, I'm " + identify.call( this );
	console.log( greeting );
}

var me = {
	name: "ME"
};

var you = {
	name: "YOU"
};

identify.call( me ); // ME
identify.call( you ); // YOU

speak.call( me ); // Hello, I'm ME
speak.call( you ); // Hello, I'm YOU
```


- `this`不是函数自身的引用，也不是函数词法作用域的引用
- `this`在运行时绑定，依赖于函数调用的上下文条件
- `this`绑定与函数声明的位置无关，和函数被调用的方式有关
- `this`的指向完全由函数被调用的调用点来决定
  



## this绑定规则 

## 全局环境

无论是否在严格模式下，在全局执行环境中（在任何函数体外部）this 都指向全局对象。
```js
console.log(this === window) // true
this.b = 1024
console.log(window.b) // 1024
console.log(b) // 1024

```

## 函数环境
 在函数内部，this的值取决于被调用的方式
 - 简单调用(默认绑定): 全局对象或`undefiend`
 - call & apply (显示绑定)
 - bind (硬绑定)
 - => ：箭头函数中，this与封闭词法环境的this保持一致。
 - 作为对象的方法 (隐式绑定)
   - 将函数调用中的this绑定到这个上下文对象
 - 原型链中的this
 - getter与setter中的this
 - 作为构造函数(new 绑定)
 - 作为DOM事件处理函数
 - 作为内联事件处理函数


### 简单调用(默认绑定)

一般情况下，`this`绑定到全局对象(window)
```js

function foo() {
    console.log(this.a) // this 默认指向全局对象
}

var a = 2
foo() // 2
```

严格模式下，this将保持他进入执行环境时的值

```js
// strict mode
function foo() {
    'use strict';
    console.log(this.a) // 严格模式下this 为 undefined
}

var a = 2
foo() // TypeError, this is undefined

```
严格模式下，如果 `this` 没有被执行环境（execution context）定义，那它将保持为 `undefined`。

### `call` & `apply`
当需要把this 的值从一个环境传到另一个，就要用 `call`或者`apply` 方法

```js
function foo() {
    console.log(this.a) // this 默认指向全局对象
}
var obj = { a: "Custom" }
var a = "Global"

foo()  // "Global"
foo.call(obj)  // Custom
foo.apply(obj) // Custom

```
```js
function add(c, d) {
  return this.a + this.b + c + d;
}

var o = {a: 1, b: 3};

// 第一个参数是作为‘this’使用的对象
// 后续参数作为参数传递给函数调用
add.call(o, 5, 7); // 1 + 3 + 5 + 7 = 16

// 第一个参数也是作为‘this’使用的对象
// 第二个参数是一个数组，数组里的元素用作函数调用中的参数
add.apply(o, [10, 20]); // 1 + 3 + 10 + 20 = 34
```
使用 call 和 apply 函数的时候要注意，如果传递给 this 的值不是一个对象，JavaScript 会尝试使用内部 ToObject 操作将其转换为对象。

```js
function bar() {
  console.log(Object.prototype.toString.call(this));
}

//原始值 7 被隐式转换为对象(new Number(7))
//原始值 `foo` 被隐式转换为对象(new String('foo'))
bar.call(7); // [object Number]
bar.call('foo') // [object String]
```
### bind

ES5 中引入了`Function.prototype.bind`。调用f.bind(someObject)会创建一个与f具有相同函数体和作用域的函数，但是在这个新函数中，this将永久地被绑定到了bind的第一个参数，无论这个函数是如何被调用的。
```js
function f(){
  return this.a;
}

var g = f.bind({a:"azerty"});
console.log(g()); // azerty

var j = g.bind({a:'yoo'}); // bind只生效一次！
console.log(j()); // azerty

var h = f.bind({a:'yoo'});
console.log(h()); // yoo

var o = {a:37, f:f, g:g, j:j,h:h};
console.log(o.f(), o.g(), o.h(), o.j()); 
// 37, azerty, yoo, azerty
```

bind(..)返回一个硬编码的新函数，它使用你指定的this环境来调用原本的函数。

> 在ES6中，bind(..)生成的硬绑定函数有一个名为.name的属性，它源自于原始的 目标函数（target function）。举例来说：bar = foo.bind(..)应该会有一个bar.name属性，它的值为"bound foo"，这个值应当会显示在调用栈轨迹的函数调用名称中。


### 箭头函数
在箭头函数中，this与封闭词法环境的this保持一致。
```js
// 在全局代码中，它将被设置为全局对象
var globalObject = this;
var foo = (() => this);
console.log(foo() === globalObject); // true

// 作为对象的一个方法调用
var obj = { foo: foo }
console.log(obj.foo() === globalObject) // true

// 尝试使用 call 来设定 this
console.log(foo.call(obj) === globalObject) // true

// 尝试使用 bind 来设定 this
foo = foo.bind(obj)
console.log(foo(obj) === globalObject) // true
```
无论如何，foo 的 this 被设置为它被创建时的环境（在上面的例子中，就是全局对象）。

这同样适用于在其他函数内创建的箭头函数：这些箭头函数的this被设置为封闭的词法环境的。

```js
var obj = {
  bar: function() {
    var x = (() => this);
    return x;
  }
};

// 作为obj对象的一个方法来调用bar，把它的this绑定到obj。
// 将返回的函数的引用赋值给fn。
var fn = obj.bar();

// 直接调用fn而不设置this，
// 通常(即不使用箭头函数的情况)默认为全局对象
// 若在严格模式则为undefined
console.log(fn() === obj); // true
// fn() // this {bar: f}

// 但是注意，如果你只是引用obj的方法，
// 而没有调用它
var fn2 = obj.bar;
console.log(fn2()() == window); // true
// fn2()() // this Window
// 那么调用箭头函数后，this指向window，因为它从 bar 继承了this。

```
### 作为对象的方法节

当函数作为对象里的方法被调用时，它们的 `this` 是调用该函数的对象。

```js
var o = {
  prop: 37,
  f: function() {
    return this.prop;
  }
};

// 当 o.f()被调用时，函数内的this将绑定到o对象
console.log(o.f()); // logs 37
```
this 的绑定只受最靠近的成员引用的影响。
```js
var o = {prop: 37};

function independent() {
  return this.prop;
}
o.b = {g: independent, prop: 42};
console.log(o.b.g()); // 42
```
### 原型链中的`this`

如果该方法存在于一个对象的原型链上，那么`this`指向的是调用这个方法的对象，就像该方法在对象上一样。
```js
var o = {
   a: 2,
   b:5,
  f: function() { 
    return this.a + this.b; 
  }
};
var p = Object.create(o);
p.a = 1;
p.b = 4;

// p 的 f 属性继承自它的原型。
// 查找过程首先从 p.f 的引用开始，所以函数中的 this 指向p。
// 也就是说，因为f是作为p的方法调用的，所以它的this指向了p。
console.log(p.f()); // 5
console.log(o.f()); // 7
```
### getter 与 setter中的`this`
用作 `getter` 或 `setter` 的函数都会把 `this` 绑定到设置或获取属性的对象。

```js
function sum() {
  return this.a + this.b + this.c;
}

var o = {
  a: 1,
  b: 2,
  c: 3,
  get average() {
    return (this.a + this.b + this.c) / 3;
  }
};

Object.defineProperty(o, 'sum', {
    get: sum, enumerable: true, configurable: true});

console.log(o.average, o.sum); // logs 2, 6

```
### 作为构造函数
当一个函数用作构造函数时（使用new关键字），它的this被绑定到正在构造的新对象。

```js
function C(){
  this.a = 37;
}

var o = new C();
console.log(o.a); // logs 37


function C2(){
  this.a = 37; // 绑定的默认对象被抛弃，语句执行，没有任何影响，僵尸代码
  return {a:38}; // 手动设置返回对象
}
o = new C2(); 
console.log(o.a); // logs 38
```
### 作为DOM时间处理函数
当函数被用作事件处理函数时，它的this指向触发事件的元素



1. 隐式绑定


```js
// 在 foo() 被调用的位置上，它被冠以一个指向 obj 的对象引用
function foo() {
    console.log(this.a) // obj.a
}
var obj = {
    a: 2,
    foo: foo
}

obj.foo() // 2
```

只有对象属性引用链的最后一层是影响调用点的
```js
function foo() {
    console.log(this.a)
}
var obj2 = {
    a: 42,
    foo: foo
}
var obj1 = {
    a: 2,
    obj2: obj2
}

obj1.obj2.foo() // 42

```

**隐式丢失(Implicity Lost)**

考虑这段代码
```js
function foo() {
    console.log(this.a)
}
var obj = {
    a: 2,
    foo: foo
}
var bar = obj.foo
var a = "global"
bar() // global
```

`bar`实际上是另一个`foo`自己的引用，这里的起调点是`bar`，此时为默认绑定

```js
function foo() {
	console.log( this.a );
}

function doFoo(fn) {
	// `fn` 只不过`foo`的另一个引用

	fn(); // <-- 调用点!
}

var obj = {
	a: 2,
	foo: foo
};

var a = "oops, global"; // `a`也是一个全局对象的属性

doFoo( obj.foo ); // "oops, global"
```

3. 显式绑定

函数拥有`call()`和`apply()`方法，它们接收的第一个参数都是一个用于`this`的对象，之后使用这个指定的`this`来调用函数

```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2
};

foo.call( obj ); // 2

```
通过foo.call(..)使用显式绑定来调用foo，允许我们强制函数的this指向obj。

**硬绑定(hard binding)**

```js
function foo() {
	console.log( this.a );
}

var obj = {
	a: 2
};

var bar = function() {
	foo.call( obj );
};

bar(); // 2
setTimeout( bar, 100 ); // 2

// `bar`将`foo`的`this`硬绑定到`obj`
// 所以它不可以被覆盖
bar.call( window ); // 2
```

我们创建了一个函数bar()，在它的内部手动调用foo.call(obj)，由此强制this绑定到obj并调用foo。无论你过后怎样调用函数bar，它总是手动使用obj调用foo。
```js
function foo(something) {
	console.log( this.a, something );
	return this.a + something;
}

// 简单的`bind`帮助函数
function bind(fn, obj) {
	return function() {
		return fn.apply( obj, arguments );
	};
}

var obj = {
	a: 2
};

var bar = bind( foo, obj );

var b = bar( 3 ); // 2 3
console.log( b ); // 5
```


ES5的内建工具中提供了`Function.prototype.bind`

```js
function foo(something) {
	console.log( this.a, something );
	return this.a + something;
}

var obj = {
	a: 2
};

var bar = foo.bind( obj );

var b = bar( 3 ); // 2 3
console.log( b ); // 5
```
