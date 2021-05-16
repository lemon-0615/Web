# Generator

执行Generator函数会返回一个遍历器对象，返回的遍历器对象可以依次Generator函数内部的每一个状态，

基本语法：

```js
1、创建生成器
function* onegenerator(){//调用生成器函数，函数不执行，返回的一个指向内部对象状态的指针，也就是遍历器对象，内部对象：{value:'hello',done:false}当遇到return时表示遍历结束，如果没有return，value值就是undefined
yield 'hello'
yield 'world'
return 'ending'//yield定义不同的内部状态
}
let one = onegenerator()
```

```
2、调用next()方法,返回一个遍历器对象，每次都从上次停下来的地方开始执行直到约到下一个yeild
one.next()//只有使用了next(),Genenrator函数才能执行
//遇到一个yeild就暂停执行后面的操作，并将紧跟在yield后的表达式的值作为返回对象的value属性
//下一次调用next方法时再继续往下执行，直到遇到下一条yield语句
//如果没有新的yield语句，就一直运行到函数结束，直到return语句为止，并将return后面的表达式的值作为返回对象的value属性值
//如果该函数没有return语句，则返回对象的value属性值就是undefined
```

next()的参数，yield语句本身没有返回值，或者说总时返回undefined，，可以带一个参数看成是上一个yield返回的值

```js
function* fn(){
  for(let i =0;true;i++){
    var reset = yield i 
    if(reset) i= -1
  }
}
let g = fn()
g.next()//返回一个遍历器对象{value:0,done:false}
g.next()//返回一个遍历器对象{value:0,done:false}从上次停下来的地方继续执行从if开始执行
g.next(true) 上一个yield返回的值为true，此时reset为true

```

for...of 不用使用next()就可以返回遍历器对象，但是一旦对象中的done属性变成true for...of循环就会终止

一旦next()方法返回的遍历器对象的done为true的是后，for...of就会终止，且不包含该对象

采用generator为原生的对象加上for...of方法: 添加到Symbol.iterator上

```js
function* objectEntries() {
        let propkeys = Object.keys(this)
        for(let propKey of propkeys){
            yield [propKey,this[propKey]]
        }

    }
    let name = {one:'za',two:'ls'}
    name[Symbol.iterator]=objectEntries
    for (let item of name)
        console.log(`${item}`)
```

generator.throw()

每一个遍历器对象都有一个throw方法，（这里的throw和全局捕捉错误的throw不同）如果generator内部有try..catch就会被捕获，但是catch只能捕获一次，如果生成器中没有try...catch，程序就会报错，中断执行，同时throw方法会附带执行吓一跳yield语句，附带执行一次next方法

```js
yield*
  在一个generator函数内部调用另一个generator函数，在yield语句后面加*表示返回的是一个遍历器对象,会返回遍历器内部的值，而没有*则会直接返回这个遍历器对象
function* outer(){
  yield 'hello'//
  yield*inner()
  yield 'world'
  
}
function* inner(){
  yield 'inner'
}
let gen = outer()
gen.next().value//hello
gen.next().value//inner
gen.next().value//world
```



# Async

async异步编程当遇到await时会等待await后面的promise执行完再执行下面的语句，等于会让出当前的线程



async返回的是一个promise对象，内部的return语句返回的值，会成为then 方法的回调函数的参数，抛出的错误会导致返回的promise对象的状态变成rejected，会被catch方法的回调函数接收到。

```js
async function f(){
throw new Error('error')
return 'hello'
}
f().then(value=>{console.log(value)})//hello
   .catch(err=>{console.log(err)})//Error:error
```

await后面一般是一个promise对象，如果不是也会被转成一个立即resolve的Promise对象，如果await后面的promise对象变为reject状态，则reject的参数会被catch方法的回调函数接收到，此时整个asnyc函数就会中断执行

```js
async function fn(){
return await'data'
}
fn().then(value=>{
console.log(value)
})
async function fn1(){
  await Promise.reject('出错了')//等待后面的异步操作执行完成
}
fn1.catch(err=>{console.log(err)})//出错了
```

如果await后面的promise对象变为reject状态，则reject的参数会被catch方法的回调函数接收到，此时整个asnyc函数，就会中断执行，为了不中断执行可以讲await放在try...catch结构里或者在await后面的promise对象中加一个catch方法

```js
async function f(){
  try {
    await promise.reject('wrong')
  }catch(e){}
  return await Promise.resolve()
}
或者
async function f(){
  
    await promise.reject('wrong')catch(err=>{})//await的作用是让出线程跳出async执行后面的异步操作执行完毕之后再回来执行await后面的操作
  }
  return await Promise.resolve()
}
```

