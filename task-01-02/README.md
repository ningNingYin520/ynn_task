**简答题**
1. 描述引用计数的工作原理和优缺点。
   工作原理:设置引用计数器，引用关系改变时修改引用数，判断当前引用数是否为0，引用数字为0时立即回收。
   优点：发现垃圾时立即回收；最大限度减少程序暂停。
   缺点：无法回收循环引用的对象；时间开销大。

 2. 描述标记整理算法的工作流程。
   标记阶段：遍历所有对象，找到所有可访问的对象，做个标记。
   清除阶段：先执行整理，移动对象位置，把未被标记的对象回收。

 3. 描述 V8 中新生代存储区垃圾回收的流程。
   回收过程采用复制算法和标记整理算法；
   新生代内存区分为二个等大小空间，使用空间为From，空闲空间为To
   活动对象存储于From空间；
   标记整理后将活动对象拷贝至To；
   From和To交换空间，完成释放。

 4. 描述增量标记算法在何时使用，及工作原理。
    增量标记算法适用于 V8 老新生代存储区垃圾回收时，采用增量标记进行效率优化。
    工作原理：为了降低老生代的垃圾回收而造成的卡顿，V8将标记过程分为一个个的子标记过程，同时让垃圾回收标记和 JavaScript 应用逻辑交替进行，直到标记阶段完成。使用增量标记算法，可以把一个完整的垃圾回收任务拆分为很多小的任务，这些小的任务执行时间比较短，可以穿插在其他的JavaScript任务中间执行，这样就不会让用户因为垃圾回收任务而感受到页面的卡顿了。


**代码题1**

基于以下代码完成下面的 4 个练习

const fp = require('lodash/fp')
// 数据
// horsepower 马力， dollar_value 价格，in_stock 库存
const cars = [
  {name: "Ferrari FF", horsepower: "660", dollar_value: "700000", in_stock: true},
  {name: "Spyker C12 Zagato", horsepower: "650", dollar_value: "648000", in_stock: false},
  {name: "Jaguar XKR-S", horsepower: "550", dollar_value: "132000", in_stock: false},
  {name: "Audi R8", horsepower: "525", dollar_value: "114200", in_stock: false},
  {name: "Aston Martin One-77", horsepower: "750", dollar_value: "1850000", in_stock: true},
  {name: "Pagani Huayra", horsepower: "700", dollar_value: "1300000", in_stock: false}
]

练习1:
使用函数组合 fp.flowRight() 重新实现下面这个函数

let isLastInStock = function(cars) {
  // 获取最后一条数据
  let last_car = fp.last(cars)
  // 获取最后一条数据的 in_stock 属性值
  return fp.prop('in_stock', last_car)
}

答案：
let isLastInStock = fp.flowRight(fp.prop('in_stock'),fp.last)


练习2:
使用函数组合 fp.flowRight()、fp.prop() 和 fp.first() 获取第一个 car 的 name
答案：
let firstCarName = fp.flowRight(fp.prop('name'),fp.first)

练习3:
使用帮助函数_average重构 averageDollarValue，使用函数组合的方式实现
let _average = function(xs) {
  return fp.reduce(fp.add, 0, xs) / xs.length
}
// <-无需改动

let averageDollarValue = function(cars) {
  let dollar_values = fp.map(function(car){
    return car.dollar_value
  }, cars)
  return _average(dollar_values)
}

答案：
let averageDollarValue = fp.flowRight(_average,fp.map(car=> car.dollar_value))

练习4：
使用 flowRight 写一个 sanitizeNames() 函数，返回一个下划线连接的小写字符串，
把数组中的 name 转换为这种形式：例如：sanitizeNames(["Hello World"]) => ["hello_world"]
let _underscore = fp.replace(/\W+/g, '_')//<--无须改动，并在sanitizeNames中使用它

答案：
let sanitizeNames = fp.flowRight(
  fp.map(item => ({ ...item, name: _underscore(item.name) })),
  fp.map(item => ({ ...item, name: fp.lowerCase(item.name) }))
)

**代码题2**

基于以下代码完成下面的 4 个练习
// support.js
class Container {
  static of (value) {
    return new Container(value)
  }
  constructor(value) {
    this._value = value
  }
  map(fn) {
    return Container.of(fn(this._value))
  }
}
class Maybe {
  static of (x) {
    return new Maybe(x)
  }
  isNothing(){
    return this._value === null || this._value === undefined 
  }
  constructor(x){
    this._value = x
  }
  map(fn) {
    return this.isNothing() ? this : Maybe.of(fn(this._value))
  }
}
module.exports = {
  Maybe,
  Container
}

练习1:
使用 fp.add(x,y) 和 fp.map(f,x) 创建一个能让 functor 里的值增加的函数 ex1
const fp = require('lodash/fp')
const { Maybe,Container } = require('./support')
let maybe = Maybe.of([5,6,1]) 

答案：
let ex1 = 

练习2:
实现一个函数 ex2，能够使用 fp.first 获取列表的第一个元素


练习3:
实现一个函数 ex3，能够使用 safeProp 和 fp.first 找到 user 的名字的首字母
const fp = require('lodash/fp)
const {Maybe, Contaiber} = require('./supports)

let safeProp = fp.curry(function(x,o){
  return Maybe.of(o[x])
})
let user = {id: 2, name: 'Albert'}

答案：


练习4:
实现 Maybe 重写 ex4，不要有 if 语句

const fp = require('lodash/fp')
const { Maybe, Contaiber } = require('./support')
let ex4 = function (n) {
  if (n) {return parseInt(n)}
}

答案：
let ex4 = Maybe.of(n).map(n=>parseInt(n))

