---
toc: true
title: JavaScript之混淆的类
categories: 
 - 博客
tags:
 - JavaScript
---

#### 序言

学习JavaScript时，如果你已经对C++、Java等传统面向对象编程语言有一定了解，或者老师教导过你使用类(类是面向对象编程的实现核心)把过程化风格的代码转换成结构清晰的代码，那么你很自然地会想在js中使用类。如在万物皆为类的Java中，严格区分类和对象，对象由类实例化而成，继承实际上是类的扩展。但JavaScript中只有对象，没有类，这是本质上的区别，可在JavaScript中有许多模拟类的语法糖在试图掩盖这个事实，很多教程中并未对此作详细解释，而是直接教初学者使用这些语法糖，导致初学者会在无形中出现困惑。

<!-- more -->

虽然JavaScript中没有类，但俗话说“没有的才是最好的”，开发者们通过不断探索总结，成功地模拟出了“类”。由于大家定义类的方法五花八门，风格不一。对于模拟面向对象的封装、继承、多态，更有许多研究，实现办法更加晦涩，不利于JavaScript新手使用。

这就引出了本文的话题：JavaScript中类的机制。

#### 类的封装、继承与多态

说到面向对象编程，不得不先了解类。

根据维基百科的定义，**类**(class)在面向对象编程中是一种面向对象计算机编程语言的构造，是创建对象的蓝图，描述了所创建的对象共同的属性和方法。有封装性、继承性、多态性三个最重要的特性。

类与**继承**抽象了一种代码的组织形式，一种编程领域对真实世界中问题的建模。比如，轮船可以被看做交通工具的一个特例，后者是更广泛的类，可以用`Vehicle`和`Steamer`两个类进行建模。Vehicle可以定义引擎、载人能力等几乎所有交通工具都具有的属性，而在具体的交通工具类中，定义同样的属性是没有意义的，所以在定义`Steamer`类时，只需声明它继承了`Vehicle`这个基类，那么它就能拥有基类的属性及方法。

有了`Steamer`类，就有了轮船的所有属性和行为，这便是类的**封装**。我们迫不及待地想上船航行，可类好比蓝图，正如轮船的图纸，并非真正可以交互的轮船，只有根据图纸建造出物理实物，才能上船。真的轮船便是蓝图的物理实例，本质上是对蓝图的复制，即**实例化**。

类的另一个核心概念是**多态**，指父类(基类)的通用行为可以被子类用特殊行为重写。如Vehicle类中为所有交通工具定义了一个`decelerate()`减速方法，默认操作是踩刹车，但在轮船的减速中，可能还需要抛锚，所以在`Steamer`类中，可以重写`decelerate()`方法，在引用`Vehicle`类中`decelerate()`方法的基础上再加一步抛锚操作。即任何方法都可引用继承层次中高层的方法(方法名可以不同)。

有以下伪代码：

```javascript
class Vehicle{
    engines=1
    startEngine(){
        console.log('start engine 1')
    }
    drive(){
        startEngine()
        console.log('driving')
    }
}
class Steamer extends Vehicle{
    engines=2//轮船有两个引擎
    startEngine(){
    	super.startEngine()//实现相对多态，调用父类的同名方法
        console.log('start engine 2')//增加自身需要的代码
    }
    drive(){//重写了父类的方法
        startEngine()
        console.log('sailing')
    }
}
```

上面的代码中`Steamer`下的`drive`方法会调用自身相对多态的`startEngine`方法，即多态性取决于引用的实例所来自的类。

可以看出，类是一种设计模式，只是Java等许多语言提供了面向类的原生语法，所以一般不为我们所感知，JavaScript也有类似的语法，但和其他语言中的类完全不同，这便是本文想解释清楚的地方。

类代表着复制，在Java等语言中，类被实例化时，它的行为会被复制到实例中，被继承时行为也会被复制到子类中，多态也是如此。但JavaScript的对象系统基于原型，而不是类，不会自动生成对象的副本。

#### 原型继承

要想在JavaScript中优雅地使用“类”，首先需要了解JavaScript的“类”本质上是基于原型的继承。

**原型(Prototype)**是JS对象的一个特殊内置属性，是对于其他对象的引用。创建一个方法时，会根据一组特定规则为该方法添加一个``prototype`属性，这个属性指向方法的原型对象。在默认情况下，原型对象会自动生成一个`constructor`属性，这个属性包含一个指向`prototype`属性所在函数对象的指针，如图所示。

![](https://cdn.jsdelivr.net/gh/radoapx/rad-figure-bed/PicGo/blogs/js混淆的类/20200514215003.jpg)

需访问一个对象的属性时，先在对象的本身上查找，如果找不到引擎就会继续在``prototype`所关联的对象上继续查找，直到找到为止，`prototype`最终都指向`Object.prototype`。这一系列查找对象的链接即是**原型链**。如下图。

![](https://cdn.jsdelivr.net/gh/radoapx/rad-figure-bed/PicGo/blogs/js混淆的类/20200514215009.jpg)

##### 继承属性

```javascript
let f = function () {
   this.a = 1
   this.b = 2
}
let g = new f(); // {a: 1, b: 2}
// 在f函数的原型上定义属性
f.prototype.b = 3;
f.prototype.c = 4;

// 可得原型链如下: 
// {a:1, b:2} ---> {b:3, c:4} ---> Object.prototype---> null

console.log(g.a) // 1
// a是g自身的属性，值为 1

console.log(g.b) // 2
// b是g自身的属性，值为 2
// 原型上也有一个'b'属性，但是它不会被访问到。

console.log(g.c) // 4
// c不是g的自身属性，那看看它的原型上有没有
// c是g.[[Prototype]]属性该属性的值为 4

console.log(g.d) // undefined
// d 不是 g 的自身，那看看它的原型上有没有
// d 不是 g.[[Prototype]] 的属性，那看看它的原型上有没有
// g.[[Prototype]].[[Prototype]] 为 null，停止搜索
// 无 d 属性，故为 undefined
```

上例中`console.log(g.b)`时输出2而不是3，即'b'属性既出现在g中也出现在了g的原型链上层，那么会触发**屏蔽**，即g中的'b'属性会屏蔽原型链上层的所有'b'属性。这种情况相当于其他语言的方法重写。

##### 继承方法

```js
let o = {
  a: 2,
  m: function(){
    return this.a + 1;
  }
}
console.log(o.m()) // 3
// 当调用 o.m 时，'this' 指向了 o.

let p = Object.create(o)
// p是一个继承自 o 的对象
p.a = 4 // 创建 p 的自身属性 'a'
console.log(p.m()) // 5
// 调用 p.m 时，'this' 指向了 p
// 又因为 p 继承了 o 的 m 函数
// 所以，此时的 'this.a' 即 p.a，就是 p 的自身属性 'a' 
```

JavaScript没有那些基于类的语言定义的“方法”。JavaScript中，任何函数(方法)都可以添加到对象上作为对象的属性。函数的继承与其他的属性继承没有差别，包括上面的“**屏蔽**”。继承的函数被调用时，this指向当前继承的对象，而不是继承的函数所在的原型对象。

Object.create(obj)返回一个与obj的`prototype`关联的对象，实现了p对于o的“继承”，虽然p对象并无a属性，但访问时若原对象无此属性，便会顺着其prototype链一直查找，直到prototype的尽头——Object.prototype，因此顺着prototype链访问到了o的a属性，是不是有点继承的感觉了？

#### 模拟“类”

所以，在JavaScript中使用类，ES6之前大多为用函数模拟。回到上面Vehicle与Steamer的伪代码，用js代码实现：

```javascript
function Vehicle(props) {
    this.id = props.id||'not bind'
    this.engines=props.engines||'not bind'
}
Vehicle.prototype.startEngine = function () {
    console.log('start engine ')
}

function Steamer(props) {
    // 调用Vehicle构造函数，绑定this
    Vehicle.call(this, props)
    this.cabin = props.cabin || 1
}
let titanic = new Steamer({ cabin: 10, id: '12412321', engines: 10 })
console.log(titanic);
console.log(Object.getPrototypeOf(titanic));
console.log(Object.getPrototypeOf(Object.getPrototypeOf(titanic)));
console.log(Object.getPrototypeOf(Object.getPrototypeOf(Object.getPrototypeOf(titanic))));
//Steamer { id: '12412321', engines: 10, cabin: 10 }
//Steamer {}
//{}
//null
```

但调用了Vehicle()作为“构造函数”不代表继承了Vehicle，Steamer创建的对象原型是``new Steamer()--> Steamer.prototype--> Object.prototype--> null`，继承关系的原型链应为`new Steamer() --> Steamer.prototype--> Vehicle.prototype--> Object.prototype--> null`，这样新的Steamer对象不仅能调用`Steamer.prototype`绑定的方法，也可以使用`Vehicle.prototype`绑定的方法。但要想达到这个状态，直接`Steamer.prototype = Vehicle.prototype`是不行的，这样两者指向同一个对象，继承关系就不存在了。此时需要借助一个中间对象，如下：

```javascript
function Vehicle(props) {
    this.id = props.id||'not bind'
    this.engines=props.engines||'not bind'
}
Vehicle.prototype.startEngine = function () {
    console.log('start engine ')
}

function Steamer(props) {
    // 调用Vehicle构造函数，绑定this
    Vehicle.call(this, props)
    this.cabin = props.cabin || 1
}

function Tmp() {
}
Tmp.prototype = Vehicle.prototype
//把Steamer的原型指向一个新Tmp对象，Tmp对象的原型正好指向Vehicle.prototype
Steamer.prototype = new Tmp()
// 把Steamer原型的“构造函数”变回Steamer
Steamer.prototype.constructor = Steamer

Steamer.prototype.getCabin = function () {
    return this.cabin
}

let titanic=new Steamer({
    cabin:9999,
    engines:4,
    id:'21435452378454'
})
console.log(titanic.cabin)//9999
console.log(titanic.engines)//4
//继承关系验证
titanic instanceof Steamer//true
titanic instanceof Vehicle//true
//查看原型链
console.log(Object.getPrototypeOf(titanic));
//Steamer { constructor: [Function: Steamer], getCabin: [Function] }
console.log(Object.getPrototypeOf(Object.getPrototypeOf(titanic)));
//Vehicle { startEngine: [Function] }
```

由于Tmp()仅用于两者继承的连接，所以可以用一个函数把这个行为封装起来：

```javascript
function(f){
    function Tmp(){}
    Tmp.prototype=f
    return new Tmp()
}
```

这即是`Object.create()`的简单实现。`Object.create()`创建一个新对象，使用现有的对象来提供新创建的对象的prototype。

`Steamer.prototype.constructor = Steamer`的作用是补上constructor属性(默认对象的prototype都有这个属性，可以理解为“构造函数”，一般指向函数自身，即`new Son()`时调用`Son()`来“构造”一个新对象，这个新对象的prototype与Son.prototype相关联)，如果将这里也封装进函数：

```javascript
function extend(son, father) {
  var prototype = Object.create(father.prototype);// 创建对象，注意！这里和Tmp()方式相同
  prototype.constructor = son;
  son.prototype = prototype;
}
```

抽离出来的`extend`方法被称作**寄生组合式继承**，是目前最成熟的方法，仅使用`Object.create()`而不补上`constructor`被称作**原型式继承**，缺点是原型链继承多个实例的引用类型属性指向相同，可能被篡改，以及无法传递参数。还有**混入**、**寄生式**等几种方式实现继承，各有各的优缺点，就不一一赘述了。

下面你会读到`extends`关键字，什么？和上面的`extend`很像？没错，`extends`关键字的核心实现就是**寄生组合式继承**，不然怎么叫语法糖。

#### class 关键字

ECMAScript 6规范中，引入了`class`的概念。使得 JS 开发者终于告别了直接使用原型对象模仿面向对象中的类和类继承的时代。但是 `class` 仅仅只是对原型对象运用语法糖，如果认为它像其它面向类语言中的`class`那样，使用时只会增加新手的困惑。

```javascript
class P {
  // ...
}
typeof P // function
```

可以看出，一个class实际上就是function。

```javascript
class Vehicle {
  constructor(props) {
    this.engines = props.engines;
    this.id = props.id;
  }
  startEngine() {
    console.log("start ",this.engines,' engines');
  }
}
class Steamer extends Vehicle {
  constructor(props) {
    super(props);
    this.cabin = props.cabin;
  }
  getCabin() {
    this.startEngine();
    console.log("I have", this.cabin, " cabins");
  }
}

let titanic = new Steamer({
  cabin: 9999,
  engines: 4,
  id: "21435452378454"
});
titanic.getCabin(); 
//start  4  engines
//I have 9999  cabins
```

当titanic调用 `getCabin`方法时，titanic自身没有需要的 `startEngine`方法，所以会到 `titanic.prototype` 原型对象上查找，最后调用``Vehicle.prototype`原型对象上的`startEngine`方法。调用时，`this` 指向的是titanic对象。

实际上，ECMAScript 6中的`class`仍然遵循你了解的JavaScript模式，继承的原理还和以前一样基于原型链，方法添加在原型上，只是用了更简单的关键字来代替，却隐藏了许多问题，要说优点可能只有一个:可以打更少的字。

#### 总结

传统基于类的面向对象思维在一定程度上妨碍了大家对JavaScript面向对象特性的理解，虽然这些机制和Java等传统面向类语言中的“类初始化”“继承”很像，但JavaScript有一个本质区别就是不会进行复制，对象之间通过内部的prototype链进行关联。所以说，在一定程度上JavaScript模拟类是得不偿失的，可解决当前问题，更可埋下隐患。实际上，对象之间的关系用**委托**形容更加贴切。

#### 参考文献

>+   类 (计算机科学)——维基百科
>
>+  《你不知道的JavaScript》——KYLE SIMPSON
>+  《JavaScript继承机制研究》—— 周 岚
>+  《JavaScript需要类吗?》——紫云飞
>+  《继承与原型链》——MDN web docs
>+  《原型继承》——廖雪峰的官方网站
>+  《JavaScript常用八种继承方案》——木易杨说
