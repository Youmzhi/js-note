
#### ❤这个人一定是你砰然心动后感性的沦陷 也是你扪心自问后理性的选择❤  

```javascript
function Foo() {
  Foo.a = function(sex) {
    console.log(1)
  }
  this.a = funciton() {
    console.log(2)
  }
}
Function.prototype.a = function(age) {
  console.log(3)
}
Foo.prototype.a = function(name) {
  console.log(4)
}
Foo.a()
const foo = new Foo()
foo.a()
Foo.a()
```
结果：3 2 1  
解析：1.Function是对象可以给Function添加prototype(原型)添加属性 2. 实例化Foo对象 foo.a() 先找this.a 再找原型Foo.prototype.a 3. 函数也是对象可以挂载属性Foo.a

```javascript
function A(n) {
  this.n = n
}
A.prototype.n = 1
function B(n) {
  this.n = n
}
B.prototype = new A()
const a = new A(11)
const b = new B(12)
delete b.n
console.log(a.n)
console.log(b.n)

```
结果：11 undefined  
解析：delete b.n 删除的是B函数的this.n; 对象属性比原型对象属性高，所以获取的不是A.prototype.n,而是A构造函数中的this.a

```javascript
var obj = {
  name: 'xxx'
}
var o = obj
o.age = o = 25
console.log(o.age)
console.log(o)
console.log(obj)
```
结果：undefined 25 { name: 'xxx', age: 25 }  
解析：a.age = 25 o = 25 赋值优先级 虽然赋值是从右往左，但是.的优先级高，所以先赋值o.age 然后赋值o。o.age赋值的时候还是引用类型，所以obj对象新增age=25的属性。后面o赋值25, 开辟一块新的内存o指向新开辟的内存,并不会影响原来的obj对象。

```javascript
let obj = {
  num1: 117
}
let res = obj
obj.child = obj = {num2: 935}
var x = y = res.child.num2
console.log(obj.child)
console.log(res.num1)
console.log(y)
```
结果：undefined 117 935  
解析：编译后 res 指向内存中 { num1: 117; child: {num2: 935} } obj指向内存中 {num2: 935 }, 所以obj.child是未定义的。res.num1输出为117。y输出为935。

```javascript
var a = 1
function fn(a) {
  console.log(a)
  var a = 2
  console.log(a)
}
fn(a)
console.log(a)
```
结果：1 2 1  
解析：存在变量提升，优先级。函数提升比变量提升高。fn(a) 形参 > 局部变量，所是形参a为全局变量，形参a已经声明了，所以var函数var失效, 函数体a=2不会影响全局a。

```javascript
var a = 1
function a(a) {
  console.log(a)
  var a = 2
  console.log(a)
}
a(a)
console.log(a)
```
结果：报错 a is not a function
解析：函数提升和变量提升，函数优先级高于变量 function a......; var a; 因为a已经存在，所以var不存在。执行阶段a = 1把函数覆盖了，所以a(a)报错，因为不是函数。

```javascript
function a(a) {
  console.log(a)
  var a = 2
  console.log(a)
}
a(a = 1)
console.log(a)
```
结果：1 2 1
解析：a(a=1) a代表全局变量便且赋值为1

```javascript
function A() {
  this.a = 1
  this.get = function() {
    console.log(this.a)
  }
}
function B() {
  this.a = 2
}
B.prototype = new A()
const b = new B()
b.get()
```
结果：2  
解析：同名子类优先级高于父类，虽然调用的是父类的方法，拿的却是子类的属性a。

```javascript
console.log('value' + '' ? false : true || 1 + 2 && 'false')
```
结果：false  
解析：（运算符直接优先级）// + 计算后： value ? false : ture || 1 + 2 && 'false'; && 计算后： value ? false : true || 'false'; || 计算后： value ? false : 'false'; 三元运算符计算后：false

```javascript
function out(x) {
  var temp = 2
  function inside(y) {
    console.log(x + y + (temp--))
  }
  inside(5)
}
out(3)
```
结果：10  
解析：--运算符前置和后置的区别;如果该运算符作为后置操作符，则返回它递减之前的值。如果该运算符作为前置操作符，则返回它递减之后的值。

```javascript
for (var a = 0; a < 5; a++) {
  setTimeout(function() {
    console.log(new Date, a)
  }, 1000)
}
console.log(new Date, a)
```
结果：
Fri Mar 25 2022 11:07:59 GMT+0800 (GMT+08:00) 5  
Fri Mar 25 2022 11:08:00 GMT+0800 (GMT+08:00) 5  
Fri Mar 25 2022 11:08:00 GMT+0800 (GMT+08:00) 5  
Fri Mar 25 2022 11:08:00 GMT+0800 (GMT+08:00) 5  
Fri Mar 25 2022 11:08:00 GMT+0800 (GMT+08:00) 5  
Fri Mar 25 2022 11:08:00 GMT+0800 (GMT+08:00) 5  
解析：a是取全局作用域，for循环已经执行完成了最后a=5;可以使用let解决这个问题。循环五次结果都是间隔1秒后输出来,可以了解一下：[JavaScript 运行机制](http://www.ruanyifeng.com/blog/2014/10/event-loop.html)

```javascript
let arr = [1,2,3]
function play(arr) {
  arr[3] = 4
  arr = [100]
}
play(arr)
console.log(arr)
```
结果： [1, 2, 3, 4]  
解析： arr指向了内存中新的arr数组, 切断了内部作用域arr和全局arr得关系，分别指向不同的数组。
