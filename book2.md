##对象

对象的继承（原型链的继承）

###继承方法

```javascript

  function inherit(p) {

  	if( p == null ) throw TypeError();

  	if( Object.create )

  		return Object.create(p);

  	var t = typeof p;

  	if( t !== "object" && t !== "function") throw TypeError();

  	function f() {};

  	f.prototype = p;

  	return new f();

  }
  
```

###对象中的属性检测

这里主要介绍对象中几个检测对象属性的方法

```javascript

  var o = { x : 1};

  "x" in o; 	//true "x"是o的属性

  "y" in o;     //false "y"是o的属性

  "toString" in o; //true "tostring"是继承原型中的属性

  //hasOwnProperty是只列举出对象中的自有属性

  o.hasOwnProperty("x")  //true "x"是o自有的属性

  o.hasOwnProperty("y")  //false "y"是o自有的属性

  o.hasOwnProperty("tostring")  //false "tostring"不是o自有的属性，是继承来的属性

  //
  
```

###对象属性的枚举

我们众所周知对象的属性的枚举可以使用for/in的方法

现在来介绍两种属性枚举方法

Object.keys(object)返回对象中可以枚举的自有属性

Object.getOwnPropertyNames()返回包括不可枚举的自有属性


##对象的三个属性

###原型属性

原型属性在对象创建之初就被创建好。比如我们创建的对象直接量（var a = {}）,的原型属性就是Object.prototype;

 通过Object.create()创建的对象，把第一个参数作为他们的原型属性。

在ES5中我们可以通过Object.getPrototypeOf()去查找参数对象的原型。

在我们用new创建的对象中，通常会继承一份constructor属性，这个属性指向创建这个对象的构造函数。

如果我们想要检测一个对象是否是另一个对象的原型，使用isPrototype()方法。

例如：

```javascript

  var p = {x: 1};

  var o = Object.create(p);

  p.isPrototype(o); //true: o继承自p

  Object.prototype.isPrototype(o); //true: p继承自object
  
```

在Mozilla中暴露了一个属性__proto__，用于直接查询/设置对象的原型。

但是这个是不建议使用，因为IE和opera还不支持这些属性。

###类属性

对象的类属性是一个字符串，用来表示对象的类型信息。在ES中没有提供一个方法去查询它。只有通过一种方法去间接查询即使用toString()(继承自Object.prototype)返回了[object class];

```javascript

  //classof函数

  function classof(o) {

    if(o == null) return "NUll";

    if(o == undefined) return "Undefined";

    return Object.prototype.call(o).slice(8,-1);

  }

  classof({}) //Object;

  classof(1) //Number;

  function f() {} //自定义一个构造函数

  class(new f()) //返回Object
  
```

在上面的例子中因为在继承下来的对象中都对同toString()方法进行了改写，所以需要调用call方法

###可拓展对象

可拓展对象是javascript中的JSON.parse()和JSON.stringify(); 这个不用多说了






