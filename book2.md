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

