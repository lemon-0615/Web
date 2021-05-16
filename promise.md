# ES6

## Promise

### promise的状态

pending（待完成）=>fulfilled (成功)：通过resolve（）变成成功，同时将

pending（待完成）=>rejected(失败)：通过reject（）变为失败

当任务的状态一旦改变就定型了称之位已定型（resolved）

当任务的状态变成resolved（表示成功的状态），就调用then中的回调函数，如果状态变成rejected（即发生错误的时候）就调用catch中的回调函数

##### promise.then()

```js
.then() 根据定义，.then(function(){},function(){})接收两个参数，第一个参数是状态变成resolved时的回调函数（必须），第二个是状态变成rejected时的回调函数（可选），一般来说不采用这种方法，而是使用采用.catch()方法来调用状态变成rejected的回调函数
注意1：resolve函数和reject都可以带有参数，传递给回调函数，其中resolve函数的参数可以是promise对象，
如下：
var p1 = new Promise((resolve,reject)=>{
  resolve()
})
var p2 = new Promise((resolve,reject)=>{
  resolve(p1)//p1的状态决定决定p2的状态，当p1的状态改变了，才会执行相应的回调函数
})
注意2:.then返回的仍然是一个promise的实例，
在.then 的链式写法中，第一个回调函数的结果会当成第二个回调函数的参数，后一个回调函数会等待前一个promise对象的状态发生变化调用相应的回调函数
核心：.then 是对promise实例的处理，所以说后面的一个then是对前面return 回来的promise对象做的处理，如果没有return就是对.then()对应的promise对象进行进行处理
代码:let p1 = new Promise((resolve,reject)=>{
        resolve('成功')
    })
     .then(value=>{
       0   return new Promise((resolve, reject) => {
             //resolve('解决了')//开始处理，下面的then可以接收到上面的值
             reject('失败了')
         })
       1 .then(null,reason => {
            return reason
        })//对前面return的promise进行处理，对0做处理

     },reason=>{})
   2 .then(value=>{
        console.log('成功'+value)//对前面的return回来的promise的处理所以输出的结果为成功失败了
    },reason => {
        console.log('失败'+reason)
    })
```

promise封装ajax异步请求

```js
let ajax = function(){
    return new Promise((resolve, reject) => {
        let xhr = new XMLHttpRequest()
        xhr.open('GET',url)
        xhr.send()
        xhr.onreadystatechange=function(){
            if(this.readyState===4){
                if(this.status===200){
                    resolve(JSON.parse(this.responseText))
                }else{
                    reject('加载失败')
                }
            }
        }
    })
}
```



##### promise.catch

用于指定发生错误时的回调函数，就是调用reject（）函数将状态变为rejected的时候用

```js
new Promise((resolve,reject)=>{
  resolve()//但是在resolve后面抛出错误，catch是不会补获的，因为resolve()以后就已经定型了,错误会冒泡所以会被最后一个catch补获到所以一般.catch可以写在最后
  reject(new Error('test'))
}).then(()=>{console.log('reslove:成功')})
  .catch(err=>{console.log(err)})//catch也会捕获前面.then方法中的错误
注意：.catch和try ..catch的不同就在于，如果没有指定.catch的回调函数，promise的错误不会抛到外层
```

promise对象的.then和.catch就涉及到了微任务队列，setTimeout是宏任务队列，微任务队列比宏任务队列的优先级高

优先级：同步>微任务队列>宏任务队列

promise里面的代码是属于同步的所以promise一旦被创建就会被执行

```js
setTimeout(()=>{console.log('time')}，1000)//定时器是宏任务队列
new Promise((resolve,reject)=>{
  resolve()//此时下面对应的.then方法会放到微任务队列里面
  console.log("promise")
  //reject(new Error('test'))
}).then(()=>{console.log('reslove:成功')})
  .catch(err=>{console.log(err)})
console.log('success')
//执行顺序
//从上往下执行：先执行同步的，所以先输出promise，再输出success，从上往下遇到resolve(),将then 放到微任务队列然后中执行,然后再执行宏任务队列里面的内容输出time
```

```js
setTimeout(()=>{console.log('time')}，1000)//定时器是宏任务队列
new Promise((resolve,reject)=>{
  setTimeout(()=>{
    resolve()//此时下面对应的.then方法会放到微任务队列里面
    console.log('time')}，1000)//定时器是宏任务队列，但是微任务在宏任务之中要先执行宏任务才能触发微任务，改变任务状态所以先输出time
  
  console.log("promise")
  //reject(new Error('test'))
}).then(()=>{console.log('reslove:成功')})
  .catch(err=>{console.log(err)})
console.log('success')//输出结果：promise success time 成功
```



##### promise.all

将多个promise实例包装成一个新的promise对象

```js
var p = Promise.all([p1,p2,p3])
只有p1，p2，p3都变成fulfilled，p才是fulfilled，此时p1，p2，p3的返回值组成一个数组传递给p的回调函数,调用all中的then
只有p1，p2，p3中有一个变成rejected p才是rejected，第一个rejected实例的返回值传递给p的回调函数，调用all中的catch，可以捕获上面p1，p2，p3的错误
```

实例

```js
let p1 = new Promise((resolve, reject) => {
        setTimeout(()=>{
            //resolve('第一个')
            reject('fail')

        },0)
    }).catch(err=>{
        console.log(err)//捕获了err，此时catch的promise对象是成功的此时会返回undefined，第二个
    })
    let p2 = new Promise((resolve, reject) => {
        setTimeout(()=>{
            resolve('第二个')
        },0)
    })
    Promise.all([p1,p2])
                       .then(result=>{
                           console.log(result)
                       })//catch如果写在这里.catch(err=>{console.log(err)//捕获了上面的err，打印fail
      

```



```js
//根据用户名批量获取用户资料
    function getUserInfo(name){
        let promises = name.map(name=>{
            return ajax(`http://localhost:8088/php/uesr.php?username=${username}`)//模版字符串嵌入了变量

        })
        return Promise.all(promises)
    }
    getUserInfo(['zs','ls']).then(name=>{
        console.log(name)
    })
```

##### promise.race()

```js
var p = Promise.race([p1,p2,p3])将多个promise包装成一个promise对象，表示p1，p2,p3其中有一个状态先改变了，那么率先改变的返回值就传递个p的回调函数
```

```js
  let promises = [
        ajax(`http://localhost:8888/php/ueser.php?name=zs`),
        new Promise((resolve, reject) => setTimeout(()=>{
            reject('请求超时')
        }),2000)//这个2s后执行，如果ajax请求超过两秒说明请求超时执行这个
    ]
    Promise.race(promises).then(value => { 
        console.log(value)
        
    })
    .catch(reason => {
        console.log(reason)//返回的时第二个promise对象的返回值即reject
    })
```

##### Promise.resolve()

将现有的对象转变成promise对象，状态时resolved

```js
参数可以是：
1、带有then方法的对象，返回一个状态为resolved的promise对象
2、参数是一个promise对象原封不动的返回这个对象
3、不带参数或者不是对象，返回一个状态为promise的对象
```

##### promise.reject()

返回一个新的promise实例，状态为rejected

promise队列

原理:每一个返回的都是一个新的promise对象，下一个promise的执行依赖于上面一个状态的改变

```js
//实现队列，每隔一秒输出数组中的数字
function queue(num) {
        let promise = Promise.resolve();
        num.map(v=>{
            promise= promise.then(()=>{//每一个then返回的promise都赋给promise变量
                return new Promise(resolve => {
                    setTimeout(()=>{
                        console.log(v)
                        resolve()
                    },1000)
                })
            })
        })

    }
    queue([1,2,3,4])
```

### promise的链式调用的原理

通过.then来实现链式调用，then返回的还是一个promise对象

```js
function Promise(excutor) {
  var self = this
  self.onResolvedCallback = []
  function resolve(value) {
    setTimeout(() => {
      self.data = value
      self.onResolvedCallback.forEach(callback => callback(value))
    })
  }
  excutor(resolve.bind(self))
}
Promise.prototype.then = function(onResolved) {
  var self = this
  return new Promise(resolve => {
    self.onResolvedCallback.push(function() {
      var result = onResolved(self.data)
      if (result instanceof Promise) {
        result.then(resolve)
      } else {
        resolve(result)
      }
    })
  })
}
```



# Class

```js
class Point{
        constructor(props) {
            this.props=props
        }

    }//是构造函数的另一种写法Point.prototype.constructor===Point
```

class不存在变量提升

私有方法：利用Symbol值的唯一性将私有方法的名字命名成一个Symbol值

```js
const bar = Symbol('bar')
const snaf = Symbol('snaf')
export defalut class myClass{
//公有方法
foo(baz){}
[bar](baz){
return this[snaf]=baz
}
}
```

私有属性：在class中用#表示私有属性

关键字get method(),Set method()类中存取属性的方法

```js
class Point{
static mystaticprops= 42
        constructor(props) {
            this.props=props
        }
        static classmethod(){}//静态方法
        子类可以继承父类的静态方法

    }
    class Son extends Point{
    
    }
```

class内部只有静态方法，没有静态属性

实例属性可以直接可以在class内部而不在constructor()内部定义

```
class Point{
static mystaticprops= 42//静态属性
state：2//实例属性 
       
    
    }
```

