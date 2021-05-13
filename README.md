# Web
学习笔记
# let和const

let块级作用域，不存在变量提升

const用来定义常量

**属性的简写** 如果属性名和后面的属性值的变量名同名，就可以写成一个如下：

```js
let obj = {name}
```

**方法的简写**

```js
let obj={
Sing(){}
}
```

# 解构赋值

```
‘…’的语法在解构赋值中表示数组中剩余的元素
```

### 解构赋值的应用：

1、交换变量的值

```js
let x=1;let y =2;

let [x,y]=[y,x]
```

2、从函数返回多个值

```js
function f(){
return[1,2,3]}
function f2(){//返回一个数组
return{Foo:1,Bar:2}}
let [a,b,c]=f()
let {foo,bar}=f1()
```

3、函数参数的定义

```js
function f2([a,b,c]){}
f2([1,2,3])
```

4、提前json值

5、遍历map结构

```js
for(let [key,value] of map){
                console.log(key+':'+value)
}
```

# 字符串中的扩展

在es6中对js中字符串类型做了扩展

js中字符以UTF-16的格式存储只能识别2个字节16位但是ES6对其方法进行了扩展可以识别四个字节

```js
codePointAt()--charCodeAt()//codePointAt()可以用来测试一个字符是几个字节
fromCodePoint(字符编码)--fromCharCode()//返回4个字节的字符
for...of 可以遍历四个字节的字符
At()--charAt()//可以识别大于0xFFFF的字符（大于2个字节的字符）
str.nomalize()正规化//将字符不同表示方法表示成一种表示方法
includes(substr，开始搜索的位置):是否找到了参数字符串
startsWith(substr，开始搜索的位置):表示参数字符串是否在源字符串的头部
endsWith(substr，开始搜索的位置):表示参数字符串是否在源字符串尾部
repeat(重复的次数n)//返回一个新的字符串，将原字符串重复了n次，n是小数会被取整，负数会报错，0 - -1的数默认为0
padStart(字符串的最小长度，用来补全的子字符串)//头部补全
padEnd(字符串的最小长度，用来补全的子字符串)//尾部补全
//如果源字符串等于或大于指定的长度返回原字符串，如果用来补全的子字符串超过了长度就会截去多余的

模版字符串 //反引号` `
模版字符串可以换行
`javascript
hh`
模版字符串可以绑定变量
var name ='zs'
var age =40
console.log(`name is${name},age is${age}`)
```

# 数组Array的扩展

#### Array.from()

将类数组的结构转变成数组，本质上就是说只要有length属性的对象都可以通过from()转变成数组

```js
Array.from({length:3})//[undefined,undefined,undefined]
```

#### 扩展运算符'...'

```js
可以将数组变成一个以逗号隔开的参数序列：console.log(...[1,2,3])//1,2,3
```

扩展运算符用于数组扩展的时候不能放在第一个会报错

```
const [...[1,2,3],[4,5]]//报错
```

应用

```
对于任何有iterator接口的对象扩展运算符都可以将它转变成真正的数组，没有iterator接口的对象用扩展运算符会报错如
let arr = [...'abc']//['a','b','c']
也可以实现数组的深拷贝
```

### Array.fill()

```
给数组填充数字
new Array(3).fill(7)//[7,7,7]
fill(第一个参数需要填充的数，开始的位置，结束的位置)
Array.of()将一组值转变成数组
```

arr.includes()

数组中是否是否包含给定的值返回的是布尔值

### 数组的其他常用的方法：

```js
//slice:返回一个新的数组不改变原来的数组，返回的是截取出来的数组，如果没有参数就表示拷贝整个数组
//...:返回一个新的数组,不改变原来的数组
//splice：会改变原来的数组，调用方法返回的是删除的元素组成的数组，原来的数组也会发生变化
//concat：数组拼接，不会改变原来的数组，返回是一个新的数组
 let arr = [1,2,3,4,5,6,6]
    console.log(arr)

    let arr1 = arr.slice()//数组的深拷贝，
    arr1.push(0)//
    // arr1=[1, 2, 3, 4, 5, 6, 6, 0]
    //arr = [1, 2, 3, 4, 5, 6, 6]未改变

    let arr2 =[...arr]//深拷贝
    arr2.push(8)//arr1=[1, 2, 3, 4, 5, 6, 6, 8]，arr = [1, 2, 3, 4, 5, 6, 6]未改变

    let arr3 = arr.splice(0,2) //返回的是一个删除元素组成的数组arr3=[1,2] arr=[3, 4, 5, 6, 6]改变
    console.log(arr3);
    console.log(arr);

    let arr4 = arr.concat(7)//[3, 4, 5, 6, 6, 7]
    console.log(arr4,arr)//arr=[3, 4, 5, 6, 6]未改变
```

### 字符串的常用方法：

```js
//slice:slice(i,j)返回一个新的字符串，包含从i到j-1位置的字符
//substring:substring(i,j)返回一个新的字符串，包含从i到j-1位置的字符
//substr:substr(i,j) 返回一个新的字符串，包含截取的从i开始的j个字符
//concat:拼接字符串，不改变原数组，返回的是一个拼接的字符串
//replace:replace(参数1，参数2)；
// 参数1：需要替换的字符，可以是原字符串的子串，也可以是匹配原字符串子串的正则表达式，参数2：你要替换成的目标字符串或者是一个回调函数
// 返回的是一个新的字符串
//repeat(n):返回一个新的字符串，该字符串包含被连接在一起n个的字符串的副本
//search(regx):用于检索字符串中有没有和正则表达式相匹配的字符串，如果没有返回-1，如果有返回匹配子串的起始位置
//split(t):将字符串以t作为分割变成数组
//str.charCodeAt(i)//返回第i个字符的ascii码
 //match():str.match(regx),
//如果使用g标志，则将返回与完整正则表达式匹配的所有结果，但不会返回捕获组。
//如果未使用g标志，则仅返回第一个完整匹配及其相关的捕获组（Array）。 在这种情况下，返回的项目将具有如下所述的其他属性。
    // 在这种情况下，返回值会多包含以下几个属性：groups：如果匹配值中包含捕获组，返回值中将包含捕获组信息的各项值。否则为undefined。
    //index：匹配值在整个字符串中的索引。
    //input：整个字符串。

 let str = 'abn12'
let reg = /\d/g
console.log(str.replace(reg, 'sd'));
console.log(str)
 let str1 = 'dnmcnm'
let str2 = str1.slice(0,2)//str2 ='dn'
let str3 = str1.substring(0,2)//str3 = 'dn'
let str4 = str1.substr(0,2)//str4 ='dn'
let arr = str1.split('c')//[dnm,nm]
let str5 = str1.search(reg)//-1
let str = 'ahbc345lkiol876kkk67'
    let reg = /\d/g//改成 reg = /\d/,返回["3", index: 4, input: "ahbc345lkiol876kkk67", groups: undefined]
    let arr = str.match(reg)
    console.log(arr)//arr=[3,4,5,1,8,7,6,6,7]
console.log(str2,str3,str4,str5,arr)
```



# Object的扩展

```js
object.assign(target, source):实现对对象的浅拷贝，只拷贝第一层的基本类型值，以及第一层的引用类型地址，object.assign()拷贝的是属性值，所以当source中的某个属性值是引用类型时，source改变，target也会改变；
assign()方法只能克隆源对象的属性值不能克隆继承来的值
let a = {
    name: "advanced",
    age: 18
}
let b = {
    name: "muyiy",
    book: {
        title: "You Don't Know JS",
        price: "45"
    }
}
b.name = "change";
b.book.price = "55";
console.log(b);
// {
// 	name: "change",
// 	book: {title: "You Don't Know JS", price: "55"}
// } 

console.log(a)
// {
// 	name: "muyiy",
//  age: 18,
// 	book: {title: "You Don't Know JS", price: "55"}//引用类型的值发生了改变
// } 

```

object.assign()应用于数组，将数组视为属性名为下标值的对象

扩展运算符应用在对象上，只能复制本身的属性不能复制从原型上继承来的属性

**对象的遍历：**

对象中有一个enumerable属性：可枚举性，如果该属性为false，就表示某些操作会忽略当前的属性

for...in :只遍历对象自身的和继承的可枚举的属性，所以可以遍历原型链上的属性

Object.keys（）：返回对象自身的所有可枚举的属性的键名

JSON.stringify();只串行化对象自身的可枚举属性

Object.assign（）：忽略 `enumerable`为false的属性，只拷贝对象自身的可枚举属性

# Map,Set, WeakSet,WeakMap

##### Set 和WeakSet

set中不存在重复的数据

weakSet中的成员只能是对象，不能是其他类型，在weakSet中的引用都是弱引用，也就是说垃圾回收机制不考虑wekset对他的引用，如果某一个对象其他对象对他不引用了垃圾回收机制就会回收它

##### weakmap和map

weakmap只接受对象作为键名

weakmap的键名指向的对象不计入垃圾回收机制

weakmap没有遍历的方法，没有size属性，没有办法列出所有的键名（这也和键名的垃圾回收机制有管）

#  Promise

  用来解决回调地狱问题的

  学习的两个方向：

  1、使用别人封装好的promise api：

 

```
 .then

  axios:这是一个用来发送Ajax请求的苦，这个库支持Promise
```

  

```js
axios({

​      url:””,

​      data:{},

​      method：“get”

}).then(function(data){//回调函数

},function(err)){}//.then ()中有两个回调函数一个是成功的回调函数另一个是失败的回调函数


```

 

  2、自己封装promise给别人使用

**promise的特点**

- promise有三个状态：pending（未完成），fulfilled（已成功），rejected（已失败），只有异步操作的结果可以决定当前是哪一种状态
- 一旦状态改变就不会再变，变化有两种可能：pending 变为Fulfilled，pending变为Rejected只要这两种情况发生此时就称为resolved

**promise的缺点 ：**

**一旦新建了他就会立即执行，无法中途取消**；其次如果不设置会掉函数promise内部抛出的错误不回反应到外部；最后当处于pending状态的时候无法得知当前发展到哪一阶段

**基本语法**

//生成实例

```js
//生成实例
var promise = newPromise（function（resolve, reject）{
         If(/*操作成功*/){
             resolve(value)//resolve函数的作用将pending变成resolved
             return resolve(value)//一般来水调用resolve和reject以后promise的使命就完成了，为了不执行后面的语句直接return，把后面的语句放在then中执行
}else{
          reject(error)//reject函数的作用将pending变成rejected
}
})
```

可以用then方法分别指定resolved状态和rejected状态的回调函数，返回的是一个新的promise实例

```js
promise.then(function(value){
/*success*/},
function(err){//failure 可选

});
```

**一般不使用then方法的第二个参数而采用.catch, .then 可以是链式的可以返回一个promise对象按照需要一定的次序调用回调函数，此时的.catch()相当于对then中的rejected的回调函数可以捕获前面resolved的运行中的错误和处理rejected里面的错误**

**promise对象的错误有冒泡性质，会一直向后传递，直到被捕获，所以说promise对象的错误一定会被下一个catch捕获**

```js
promise.then(function(value){
  //resolved
}).catch(function(err){});	
```

其中如果catch中没有指定或者使用处理错误的回调函数，promise对象抛出错误不会传到外层代码例如

```js
var somefunction = function(){
    return new Promise(function (resolve, reject) {
        resolve(x+2)//x是undefined//浏览器会报错

    })
somefunction().then(function(){
    console.log("evrey is right")
})
}
```



# 继承

ES5的继承，实质是先创造子类的实例对象 `this`,然后再将父类的方法添加到 `this`上面（`parent.appply(this)`）

```js

function Person(name){
 this.name=name;
 this.className="person" 
}
Person.prototype.getName=function(){
 console.log(this.name)
}
function Man(name){
  Person.apply(this,arguments)
}
```

ES6的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到 `this` 上面（所以必须先调用 `super`方法）然后再用子类的构造函数修改 `this`

另一个需要注意的地方是，在子类的构造函数中，只有调用`super`之后，才可以使用`this`关键字，否则会报错。这是因为子类实例的构建，基于父类实例，只有`super`方法才能调用父类实例。因为子类如果要拥有子的this对象，必须先通过父类的构造函数完成塑造，接着再对其进行加工，加上子类自己的实例属性和方法，如果不调用supe方法，子类就得不到this 对象。

```js
class Parent{}
class Son extends Parent{
     constructor(x,y,color)
     super(x,y)
     this.color = color
}
```
