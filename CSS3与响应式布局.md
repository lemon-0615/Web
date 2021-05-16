# CSS3与响应式布局

### 响应式布局

自适应各种设备，使之能在不同的浏览器页面之间平滑的过渡，CSS3提出了一个完美工具media query。通过媒体查询可以让网站根据窗口大小和设备的不同，无缝地在不同的样式机之间切换。

#### 响应式布局基础

流式布局等比缩放；就是用比例而非像素设定他们的宽度

```html
<style> 
#left{
            width:67%;
            margin-left: 1.6%;
            margin-right: 1.6%;//可以用来适应元素外边距设值的情况,但是边框不能用%做单位可以在等比布局的两个栏内额外添加一个<div>元素用来设置边框
            float:left;
        }
 #right{
            width:28%;
            margin-right: 1.6%;
            float:left;
        }
    </style>
</head>
<body>
<div id="left"></div>
<div id="right"></div>
  

</body>
```

解决方法

1 在元素上加上box-sizing：border-box,边框位于盒子的内侧，无论边框多粗，盒子始终都是67%，指定的任何内边距和边框都将在已设定的宽度和高度内进行绘制width = border+width+padding，box-sizing = content-box width = content width

2、calc()

流式排版为了适应不用大小的页面不采用像素这种绝对的单位而是采用%和em（相对于父类的元素的大小1em=10px）这样的相对的单位，用em单位设定边框，内边距外边距，最大的好处是可以防止元素在小窗口里显示的过大，也就是在移动端显得太过突兀

CSS3将em转换成rem解决元素嵌套时，某个元素的的大小不符合于默认元素的比例，rem= root em 始终是相对于html元素计算大小的

计算方法： 

响应式布局的核心思想：百分比流式布局

实现手段：动态获取当前适口宽度width ，除以一个固定的数n，得到rem的值。 表达式为

`rem = width/n`

**rem和em的区别**

rem和em都是相对长度单位，但是使用rem为元素设定字体大小时仍然是相对大小，但是相对的只是根元素即html元素，通过它既可以做到只修改根元素就成比例地调整所有字体大小，又可以避免字体大小逐层复合的连锁反应

em的值不是固定的，会继承父级元素的字体大小，一半我们要把body选择其中的fontsize设置为62.5%，这样em就是绝对像素除以10

```js
        // 动态为根元素设置字体大小
        function init () {
　　　　　　// 获取屏幕宽度
          var width = document.documentElement.clientWidth
　　　　　　// 设置根元素字体大小。此时为宽的10等分
          document.documentElement.style.fontSize = width / 10 + 'px'
        }
 
　　　　 //
　　　　 首次加载应用，设置一次
        init()
        // 监听手机旋转的事件的时机，重新设置
        window.addEventListener('orientationchange', init)
        // 监听手机窗口变化，重新设置
        window.addEventListener('resize', init)
 
　　理解：上面代码实现了，无论设备可视窗口如何变化，始终设置rem为width的1/10.即实现了百分比布局
4、tip：
1、以上代码需在dom之前写入（可放在head里面第一个script标签）
 
　　2、移动端加上meta标签<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
5、使用体验：
　　我在项目中没有使用flexible.js等此类动态计算rem的插件。另外说明一点，此方法实现的功能也相对简单，只实现了最核心的动态修改rem的值。
object.defineproperty(obj,'data',{
  get:function(){
    return data
  }
  set:function(newdata){
  data = 1
}
})
```



### 视口：viewport

**视口的类型：1、layoutviewport、visualviewport、idealviewport**

- layoutviewport：大于实际屏幕，元素的宽度及成语layoutviewport，用于保证网站的外观特性与桌面浏览器一样。layoutviewport到底多款，每日个人浏览器不同。
- visualviewport：当前显示在屏幕上的页面，即浏览器可是区域的宽度
- idealviewport：为浏览器定义的课完美适配移动端的理想viewport，固定不变，可以认为是设备适口宽度

**viewport的设置（移动端）**

`<meta name= 'viewport' content='width = device-width,initial-scale=1,user-scale=no' /> `

###### 注释：

width:设置的是layoutviewport的宽度

initial-scale设置页面的初始的缩放值，该缩放值是相对于idealviewport缩放的，最终得到的结果会影响到visualviewport和layoutviewport

user-scalable是否允许用户进行缩放的设置

```
// 设定两个变量：  
viewport_1 = width;  
viewport_2 = idealviewport / initial-scale;

// 则：  
layoutviewport = max{viewport_1, viewport_2};  
visualviewport = viewport_2;
如果layoutviewpor===visualviewport，页面下面就不会出现滚动条，默认只把页面放大缩小
```

建立一个所有设备友好的页面就要告诉浏览器不要自动执行视口缩放，视口修改如下

```html
<meta content = "initial-scale=1.0" name = 'viewport>'<!--告诉浏览器不要缩放-->
```

设备像素=物理像素

逻辑像素就是css像素也叫设备独立像素

设备像素比（dpr） = 物理像素/设备独立像素，表示一个逻辑像素需要多少物理像素，如果dpr等于2 那就说明，一个设备独立像素为4个物理像素，所以在css上设置的1px在其屏幕上占据的是2个物理像素，所以css0.5px才能显示其最小的单位，这就是1px在retina屏上变粗的原因，所以解决1px的问题，可以通过压缩的方式来解决，通过设置视口的initial-scale=1/dpr来实现

### 媒体查询（响应式布局）

需要对viewport进行设置，否则根据查询到的尺寸无法正确匹配视觉宽度而导致布局混乱

```html
 <style>
        #left{
            width:67%;
            margin-left: 1.6%;
            margin-right: 1.6%;//可以用来适应元素边框设值的情况
            float:left;
        }
        #right{
            width:28%;
            margin-right: 1.6%;
            float:left;
        }
   //页面变得很窄时应该应用的样式
   //基本写法
   @media(media-feature:){
     符合条件时应该应用的样式
     }
   @media (max-width: 568px) {
            #left{
                float: none;
                width: auto;
            }
            #right{
                float: none;
                width: auto;
            //会叠加在已有的样式上
            }
        }
    </style>
```

经常使用到的样式：max-width：页面很窄的时候

​                                  max-device-width：手机版网站

​                                 orientation：landscape横向/portrait纵向//根据设备的朝向来调整布局

​             

替换整个样式表

```html
<link rel = "stylesheet"media="(max-width:568px)"href ="small_style.css"
```

要注意防止样式叠加可以采用使某一个媒体查询的值变成稍大一点的小数

# CSS3动画

transform, animation,transition

transform：是一个静态的样式，主要用作给元素做变换，主要变换方式：`rotate(旋转)`，`scale(缩放)`，`skew(扭曲)` `translate(移动)` `matrix(矩阵变换)`

语法：

```
transform:none|transform-functions//non表示不做变换，transform-functions表示变换函数可以有多个
```

实例：

```
<div class = 'rotate'> rotate</div>
<style>
  .rotate{
  transform:rotate(360deg)
  }
</style>
```

transition:用来定义某个css属性或者多个css属性的变化的过渡效果只执行一次

```
transition: property duration timing-function delay;
```

|                            |                                                              |
| -------------------------- | ------------------------------------------------------------ |
| transition-property        | 指定CSS属性的name，transition效果: 大小,位置,扭曲等（none没有效果，all所有属性变化都获得过渡效果，property固定属性火的效果 |
| transition-duration        | 规定完成过渡效果需要花费的时间（以秒或毫秒计）。 默认值是 0，意味着不会有效果。 |
| transition-timing-function | linear: 规定以相同速度开始至结束的过渡效果（等于 cubic-bezier(0,0,1,1)）ease: 规定慢速开始，然后变快，然后慢速结束的过渡效果（cubic-bezier(0.25,0.1,0.25,1)）;指定transition效果的转速曲线，ease-in: 规定以慢速开始的过渡效果（等于 cubic-bezier(0.42,0,1,1)）; |
| transition-delay           | 定义transition效果开始的时候                                 |

transition和transform结合实现鼠标移入放大小球下落

```html
//html
<h2>利用transition和transform结合实现动画过渡</h2>
<div class="boll1" id="boll1"></div>
//css
.boll1 {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background: #03A9F4;
  transform: translateY(0px);
  margin-bottom: 300px;
  transition: all cubic-bezier(0.42,0,0.58,1) 2.0s 1.0s;
  cursor: pointer
}
.boll1:hover {
  transform: translateY(200px);
}


```

**animation**：先通过@(-webkit-)keyframes定义动画名称及动画的行为,然后再通过animation的相关属性定义动画的执行效果。可以循环执行

语法：

```css
animation:name duration timing-function delay iteration-count direction fill-mode play-state
```

```
animation:指定要绑定到选择器的关键帧的名称 规动画指定需要多少秒或毫秒完成 设置动画将如何完成一个周期 	设置动画在启动前的延迟间隔 定义动画的播放次数 	指定是否应该轮流反向播放动画 规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式 指定动画是否正在运行或已暂停
```

其中name 指向@keyframes定义的动画

@keyframes语法

`@keyframes animationname {keyframes-selector {css-styles;}}`

| 值                 | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| animationname      | 必需。定义动画的名称。                                       |
| keyframes-selector | 必需。定义动画的名称。 合法的值： 1. 0-100% 2. from（与 0% 相同） 3. to（与 100% 相同）因为可能存在兼容性问题 |
| css-styles         | 必需。一个或多个合法的 CSS 样式属性。                        |

```
//名字为gif的@keyframes ，动画完成需要的总时长为1.4s,刚开始的时候图片旋转为0度，动画完成的时候图片旋转360度
.load-border {
    width: 120px;
    height: 120px;
    border-radius:50%
    border-top:10px solid #3498db
    background: url(../images/loading_icon.png) no-repeat center center;
    -webkit-animation: gif 1.4s infinite linear;
    animation: gif 1.4s infinite linear; 
}
@keyframes gif {
    0% {
        -webkit-transform: rotate(0deg);
        transform: rotate(0deg);
    }
    100% {
        -webkit-transform: rotate(360deg);
        transform: rotate(360deg);
    }
}
```

transform本身是没有过渡效果的,它只是对元素做大小,旋转,倾斜等各种变换,通过和transition或者animation相结合,可以让这一变换过程具有动画的效果,它通常只有一个到达态,中间态的过渡可以通过和transition或者animation相结合实现,也可以通过js设置定时器来实现.

transition一般是定义单个或多个css属性发生变化时的过渡动画,比如width,opacity等.当定义的css属性发生变化的时候才会执行过渡动画,animation动画一旦定义,就会在页面加载完成后自动执行.

transition定义的动画触发一次执行一次,想要再次执行想要再次触发.animation定义的动画可以指定播放次数或者无限循环播放. transition: 需要用户操作,执行次数固定.

transition定义的动画只有两个状态,开始态和结束态,animation可以定义多个动画中间态,且可以控制多个复杂动画的有序执行.