Es6 class

```js
//js语言中，生成实例对象的传统方法是通过构造函数
function Point(x,y){
  this.x = x;
  this.y = y;
}
Point.prototype.toString = function(){
  return  '(' + this.x + ', ' + this.y + ')';
}
var p = new Point(1,2)
p.toString()
"(1, 2)"
```

es6提供了更接近传统语言的写法，引入class（类）这个概念，作为对象的模版，通过class关键字，可以定义类，相当于一个语法糖,新的`class`写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

```js
class Point{
  constructor(x,y){
    this.x = x;
    this.y = y;
  }
  toString(){
      return  '(' + this.x + ', ' + this.y + ')';
  }
}
//如上定义了一个类，constructor方法即为构造函数，而this关键字则代表实例对象，也就是说，es5的构造函数 Point，对应es6的Point类的构造方法。
Point类除了构造方法，还定义了一个toString方法。注意，定义“类”的方法的时候，前面不需要加上function这个关键字，直接把函数定义放进去了就可以了。另外，方法之间不需要逗号分隔，加了会报错。
```

```javascript
typeof Point // "function"
Point === Point.prototype.constructor // true
类的数据类型就是函数，类本身就指向构造函数
构造函数的prototype属性，在es的类上面继续存在。事实上，类的所有方法都定义在类的prototype属性上面。
class Point{
  constructor(){
    //...
  }
  toString(){
    //...
  }
  toValue(){
    
  }
}
//等同于
Point.prototype = {
  constructor(){},
  toString(){},
  toValue(){}
}
class B {}
let b = new B();
b.constructor === B.prototype.constructor
b是B类的实例
```

由于类的方法都定义在`prototype`对象上面，所以类的新方法可以添加在`prototype`对象上面。`Object.assign`方法可以很方便地一次向类添加多个方法。

```js
class Point {
  constructor(){
    //...
  }
}
Object.assign(Point.prototype,{
  toString(){},
  toValue(){}
})

```

```javascript
Point.prototype.constructor === Point // true
```

另外，类的内部所有定义的方法，都是不可枚举的（non-enumerable）。

constructor方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类必须有constructor方法，如果没有显示定义，一个空的constructor方法会被默认添加

````js
class Point{
  
}
//等同于
class Point{
  constructor(){}
}
constructor方法默认返回实例对象(即this)，完全可以指定返回另外一个对象。
class Foo{
  constructor(){
    return Object.create(null)
  }
}
new Foo() instanceof Foo //false
constructor函数返回一个全新的对象，结果导致实例对象不是Foo类的实例。
类必须使用new调用，否则会报错。这是它跟普通构造函数的一个主要区别，后者不用new也可以执行
````

类的实例

```
生成类的实例的写法，与es5完全一样，也是使用new命令。
实例的属性除非显示定义在其本身（即定义在this对象上），否则都是定义在原型上
```

```javascript
//定义类
class Point {

  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }

}
var point = new Point(2, 3);

point.toString() // (2, 3)

point.hasOwnProperty('x') // true
point.hasOwnProperty('y') // true
point.hasOwnProperty('toString') // false
point.__proto__.hasOwnProperty('toString') // true
```

上面代码中，`x`和`y`都是实例对象`point`自身的属性（因为定义在`this`变量上），所以`hasOwnProperty`方法返回`true`，而`toString`是原型对象的属性（因为定义在`Point`类上），所以`hasOwnProperty`方法返回`false`。这些都与 ES5 的行为保持一致。

```js
类的所有实例共享一个原型对象
var p1 = new Point(2,3)
var p2 = new Point(3,2)
p1._proto_===p2._proto_
```

Object.getPrototypeOf来获取实例对象的原型，

然后再来为原型添加方法／属性。

```javascript
var p1 = new Point(2,3);
var p2 = new Point(3,2);

p1.__proto__.printName = function () { return 'Oops' };

p1.printName() // "Oops"
p2.printName() // "Oops"

var p3 = new Point(4,2);
p3.printName() // "Oops"
```

上面代码在`p1`的原型上添加了一个`printName`方法，由于`p1`的原型就是`p2`的原型，因此`p2`也可以调用这个方法。而且，此后新建的实例`p3`也可以调用这个方法。这意味着，使用实例的`__proto__`属性改写原型，必须相当谨慎，不推荐使用，因为这会改变“类”的原始定义，影响到所有实例。

取值函数getter和存值函数setter

在“类”的内部可以使用`get`和`set`关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为。

```js
class MyClass {
  constructor(){
    //...
  }
  get prop(){
    return 'getter'
  }
  set prop(value){
    console.log('setter:'+value)
  }
}
let inst = new MyClass()
inst.prop = 123
// setter: 123
inst.prop
// 'getter'
```

```js
const MyClass = class Me {
  getClassName(){
    return Me.name
  }
}
let obj = new MyClass()
obj.getClassName()
//"Me"
```

```js
let person = new class {
  constructor(name){
    this.name = name
  }
  sayName(){
    console.log(this.name)
  }
}('张三')
//上述，person是一个立即执行的类的实例
person.sayName(); // "张三"
```

> 类不存在变量提升hoist

```js
{
  let Foo = class {};
	class Bar extends Foo{}
}
//Bar继承Foo
```

> this的指向

类的方法内部如果含有this，它默认指向类的实例。

```js
class Logger {
  printName(name='there'){
    this.print(`hello ${name}`)
  }
  print(text){
    console.log(text)
  }
}
const logger = new Logger()
const { printName } = logger;
printName(); // TypeError: Cannot read property 'print' of undefined
```

上面代码中，`printName`方法中的`this`，默认指向`Logger`类的实例。但是，如果将这个方法提取出来单独使用，`this`会指向该方法运行时所在的环境（由于 class 内部是严格模式，所以 this 实际指向的是`undefined`），从而导致找不到`print`方法而报错。

箭头函数内部的this总是指向定义时所在的对象。

还有一种解决方法是使用`Proxy`，获取方法的时候，自动绑定`this`。

class可以通过extends关键字实现继承

```js
class Point {
}
class ColorPoint extends Point{
}
//子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类自己的this对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用super方法，子类就得不到this对象。
```



