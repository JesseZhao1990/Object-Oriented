## js中的面向对象
面向对象的语言一般都会有类的概念，类又是个什么东西呢？用一句话可以这样概括，类就是通过其自身可以创建具有相同属性和方法的对象的一个东西。可以举个生活中的栗子说明一下。你可以把人类看成类，可以把具体的一个人看作对象，比如说小明，小红同学，就是一个实实在在的对象。可以用人的标准来产生无数的具体人。

然而js是一门比较灵活的语言，它并没有类的概念。不过以js现在的进化速度，不久的将来肯定会出现类的概念。js虽然没有类的概念，但是有构造函数，你可以把构造函数理解为js中的类。把new某个构造函数理解为产生了某个类的对象。ok。先简单的这样理解。下面介绍下创建对象和类的继承

### 创建对象
面向对象编程和面向过程编程的最大不同就是面向对象编程是通过创建对象、使用对象来编写程序的。好处就是分类清晰，模块化，方便维护和管理。代码量少。下面总结一下js中用到的创建对象的方法

**工厂模式**

工厂模式是一种广为人知的设计模式。从它的名字就可以知道其大概的实现思路。由于js中无法创建类，工程师们就发明了一种方法，就是封装函数来达到产生对象的目的。就像火腿肠场似的。活猪进去，出厂就变成了猪肉火腿肠，活鸡进去，出厂就变成了鸡肉火腿肠。如下图的栗子所示。

```
function createPerson(name,age,job){
	var obj = new Object();
	obj.name = name;
	obj.age = age;
	obj.job = job;
	obj.sayName = function(){
		alert(this.name);
	}
	return obj;
}

var person1 = createPerson("xiaoming",21,"police");
var person2 = createPerson("xiaohong",23,"engineer");

```

这个createPerson能够产生具体的人，也就是对象。虽然解决了创建多个相似对象的问题，但没有解决对象识别的问题。通俗来讲就是你并不知道你产生的xiaoming和xiaohong是属于人类呢还是狗类呢？学术语就是if（xiaoming instanceof human）；

**构造函数模式**

构造函数模式先进的地方就是解决了xiaoming和xiaohong这样的对象的归属问题。开头咱们就说过，你可以把构造函数看做js中的类，可以创建特定类型的对象。js中有一些原生的构造函数（就是已经别人写好的，内置到引擎中的），比如Object、Array、String。这些都是js已经写好的。会自动出现在运行环境中。这里我们要讲的就是如何自定义构造函数。还说举例子，这里我们用自定义构造函数的方法重写上边那个例子。

```
function Person(name,age,job){
	this.name = name;
	this.age = age;
	this.job = job;
	this.sayName = function(){
		alert(this.name);
	}
}

var person1 = new Person("xiaoming",21,"police");
var person2 = new Person("xiaohong",23,"engineer");

```
那现在验证一下咱们创建的xiaoming和xiaohong是属于人类还说狗类的问题？

```
alert(person1 instanceof Person); //true;
alert(person2 instanceof Person); //true;
alert(person1 instanceof Array); //false;
alert(person2 instanceof String); //false;

```
这里的instanceof不具体展开，涉及到继承的问题。
解释一下new操作符把。使用new操作符的时候，具体发生了些什么呢？

1.创建一个对象

2.将构造函数的作用域赋给了新对象，因此this就指向了新的对象。

3.执行构造函数中的代码（为这个新对象添加属性和方法）

4.返回新对象

思考一下构造函数创建对象目前有没有发现不大优雅的地方？假如人类都具有贪欲、食欲、同理心、嫉妒、这些人性的话，那我创建xiaoming和xiaohong的时候，甚至在创建以后别的具体人的时候，是不是在构造函数里都要一个个的加进去？虽然我可以在构造函数，也就是类中直接添加上。也不是什么麻烦事，但是在创建每个具体对象时。程序都会执行一遍创建。程序累啊。

**原型模式**

我们创建的每个函数都有一个prototype（原型）属性。这个属性是一个指针。指向一个对象。而这个对象的用途就是存放所有实例共享的属性和方法。也就是说原型模式能解决我们刚刚抛出的问题。可以把人类都具有的贪欲、食欲、同理心、嫉妒。这些公共共有的东西放到人这个类的原型上。这样xiaoming和xiaohong就天然具有了作为人应该有的这些特征。

**组合使用构造函数模式和原型模式**

创建自定义类型（类）的最常用的方式就是结合使用构造函数和原型。构造函数用于定义实例属性，而原型用于定义方法和共享的属性。还说重写前面的例子

```
function Person(name,age,job){
	this.name = name;
	this.age = age;
	this.job = job;
	this.organ = ["手","眼","嘴巴"]
}

Person.prototype = {
	constructor: Person,
	humanity:["贪欲","食欲","同理心","嫉妒"],
	sayName = function(){
		alert(this.name);
	}
}

var person1 = new Person("xiaoming",21,"police");
var person2 = new Person("xiaohong",23,"engineer");

person1.humanity.push("善良");
console.log(person2.humanity); // "贪欲","食欲","同理心","嫉妒"
console.log(person1.humanity); // "贪欲","食欲","同理心","嫉妒","善良"

person1.organ.push("尾巴");
console.log(person2.organ); //"手","眼","嘴巴"
console.log(person1.organ); //"手","眼","嘴巴","尾巴"

```

好了，今天就写js中的创建对象，明天总结一下js中的类的继承。什么叫继承呢？举个栗子，人类同时也是地球上的生物对不？那人类应该继承生物这个类。具有生物的特质。明天再细讲。












































