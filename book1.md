##二进制浮点数和四舍五入错误

实数个数有无数个，但是在javascript中表示实数个数的是有限多个。所以在javascript中很多
实数是用其近似的值来表示。

javascript通过一种二进制的表示方法来表示浮点数，它可以精确的表示1/2、1/8等分数，但是我们很多
常用的分数都是十进制的分数如1/10、1/100等。二进制不能很精确的表示类似于0.1这样的分数，

所以这才出现了像我们如下常见的问题：

```javascript

  var x =  .3 - .2;
  
  var y = .2 - .1;
  
  x == y //fasle
  
  x == .1; //false
  
  y == .1 // true
  
```

如上， x和y的数值是非常接近的。

这个问题不仅仅在Javascript中存在，在多数使用二进制浮点数编程语言中都会存在。

##null和undefined

null和undefined都是表示“空值”，但是null是属于javascript的关键字，而undefined不是，在
因为undefined是在javascript预定义时候的一个全局变量(也可以说是全局对象中的一个属性)，在ES3的时候undefined是可读写的变量（注意啊～～变量），这个奇葩
的bug在ES5的时候才修复成只是可读的。

但是在我们平时认为null和undefined都是相等的喽～～


```javascript

  null == undefined; //true
  
```
这是为什么？

以下的例子会更清晰一点：

```javascript

  typeof null; //object
  
  typeof undefined; //undefined
  
```

没错你没有看错，这就是他们之间的区别。

所以呢，我们一般认为null为一种特殊的对象值，他叫“非对象”（别问我为什么～它就叫这个）,

但是实际上他是它也属于一种类型，这种类型只有他自己null，一般情况我们用它表示数字、字符串等是“无值”

而undefined呢，它的值就是表示“未定义”，比如我们要查询某个对象的属性或某个数组的时候返回
undefined的时候，这表示它不存在或未定义。

虽然null和undefined是不相同的，但是他们可以互换。如果我们想严格区分他们，用严格相等（===）运算
符来区分他们.


##函数作用域和声明提前

这里我们注重讲解声明提前，废话不多说直接上例子：

```javascript

var scope = "globel";

function f(){
    console.log(scope) //输出undefined，但不是globel
    
    var scope = "local";
    
    console.log(scope); //输出local
};

```

按照我们对函数作用域的理解我们一般都是认为第一个console的地方输出”globel“，但是输出了
undefined，这个特性被叫为“声明提前”，即javascript函数里面声明的所有变量都被提前到函数体顶部
这个过程是在Javascript引擎预编译是进行的，在代码开始运行前。

##eval()

eval()函数只接受一个参数。如果参数不是字符串，则直接返回这个参数。如果这个参数是字符串
直接当成javascript代码进行编译，如果编译失败则抛出一个异常。如果编译成功则开始执行这一段代码。

eval()中有一点需要我们去理解，eval()使用了调用它的变量作用域环境。

我们可以这么理解：

```javascript
//code 1
function foo(){
    var a = "foo";
    eval("a = 'foo1'")
}

//code 2

function foo(){
    var a = "foo";
    a = "foo1";
}

```

如上代码code1和code2运行效果是一样的。也就是eval()可以查找，读和写它所在作用于中的变量。
同时也可以声明变量和函数。

同理，如果如果我们将eval()写在代码顶部，则它所作用的作用域是全局。

###1.全局eval()

如上我们说过，eval()可以更改它所在作用域的变量和函数的能力。

但是如果我们把eval()命名一个别名呢，先看如下代码：

```javascript

var geval = eval;

var x = "globel",y = "globel";

function f(){
    var x = "local";
    eval("x += 'change';");
    return x;
}

function g(){
    var y = "local";
    geval("y += 'changed'");
    return y;
}

console.log(f(), x);  //更改了局部变量： 输出了“localchanged globel”
console.log(g(), y); //更改了全局变量： 输出了“local globelchanged”

```

如上面的代码我们将eval重名为一个geval，我们就发现它更改的是全局x的变量。而直接使用eval
它所作用在其所在作用域内的变量，这个是eval中的一些特性，需要我们注意，但是一般情况下
我们很少去运用eval这个字面量。

###2.严格eval()

在ES5严格模式下，我们对eval （）增加了更多的限制。在严格限制下或者代码以‘use strict’开头的。
eval代码可以查找或者更改局部变量，但是不能定义变量和函数。

同时，不能用一个别名来覆盖eval()函数，变量名、函数名都不能命名为eval。因为它将作为一个保留字来处理。
