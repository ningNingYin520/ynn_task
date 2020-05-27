一、简答题
1. 请说出下列最终执行结果，并解释为什么？
var a =[];
for(var i = 0;i < 10;i++){
    a[i] = function(){
        console.log(i);
    };
}
a[6]()

执行结果：10
实际上i打印的是全局作用的i，当for之后i已经变成了10，所以无论打印哪个元素的click他的结果都是一样的

2.请说出下列最终执行结果，并解释为什么？
var temp = 123;
if(true){
    console.log(temp)
    let temp;
}

执行结果：控制台报错

3.结合ES6新语法，用最简单的方式找出数组中的最小值。
var  arr = [12,34,32,89,4]

var newArray = Array.from( new Set(arr) );
var minNum = Math.min(...newArray);

Array.from（）将set转为数组，再使用扩展运算符将数组转换成列表。利用Math.min()方法求最小值，该方法的参数是一个数值列表。

4.请详细说明var， let ，const 三种声明变量的方式之间的具体差别？
var：全局范围；变量提升。
let：块级作用域；声明的变量是不存在变量提升的，要在声明变量之后使用。
const: 在let基础上多了只读属性；声明后不允许修改：不允许在声明了之后再重新指向一个新的内存地址，能够修改恒量中的属性成员；声明时赋值。
一般使用时：不用var，主用const，配合let，用const会更明确代码中声明的成员会不会被修改。

5.请说出下列代码最终输出的结果，并解释为什么？

var a = 10;
var obj = {
    a: 20,
    fn(){
        setTimeout(()=>{
            console.log(this.a)
        })
    }
}
obj.fn();

输出结果：20
fn()通过obj调用，this指向obj

6.简述Symbol类型的用途？
Symbol表示一个独一无二的值，为对象添加独一无二的属性名；使用Symbol来替代常量。

7.说说什么是浅拷贝，什么是深拷贝？
变量存储类型分两类：①基本类型：基本数据类型，直接存储在栈中的数据，②引用类型：复杂数据类型，将该对象引用地址存储在栈中，然后对象里面的数据存放在堆中。
浅拷贝：其实复制的是的栈内存的引用地址，而并非堆里面的值。
深拷贝：复制的是的栈内存的引用地址，在堆内存中也开辟一个新的内存专门为存放值。

8.谈谈你是如何理解JS异步编程的，Event Loop是做什么的，什么是宏任务？什么是微任务？
就是代码执行的顺序并不是按照从上到下的顺序一次性一次执行，而是在不同的时间段执行。
Event Loop：主线程去任务队列读取任务到执行栈中去执行，这个过程是循环往复的，这便是 Event Loop，事件循环。
宏任务:每次执行栈执行的代码就是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行,每一个宏任务会从头到尾将这个任务执行完毕，不会执行其它）包括整体代码script，setTimeout，setInterval。
微任务:在当前宏任务执行结束后立即执行的任务 包括Promise，process.nextTick。

9.将下面的异步代码使用Promise改进。

setTimeout(function(){
    var a = 'hello';
    setTimeout(function(){
        var b = 'lagou';
        setTimeout(function(){
            var c = 'I ♥ U'
            console.log(a + b + c);
        },10)
    },10)
},10)

Promise改进之后：
const promise1 = new Promise((resolve, reject) => {
    setTimeout(resolve, 10, 'hello');
});
const promise2 = new Promise((resolve, reject) => {
    setTimeout(resolve, 10, 'lagou');
});
const promise3 = new Promise((resolve, reject) => {
    setTimeout(resolve, 10, 'I ♥ U');
});
  
Promise.all([promise1, promise2, promise3]).then((values) => {
    console.log(values[0] + values[1] + values[2]);
});


10.请简述TypeScript和JavaScript 之间的关系？
TypeScript是JavaScript的超集，主要提供了类型系统和对 ES6 的支持，任何一种JavaScript的运行环境都支持。功能强大，生态健全完善；属于渐进式。

11.谈谈你所认为的TypeScript优缺点？
优点：增加了代码的可读性和可维护性；生态比较完善。
缺点：语言本身多了很多概念，增加了学习内容；项目初期增加了项目的成本；

