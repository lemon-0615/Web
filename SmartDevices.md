
# 项目：
## ***\*无人智能终端管理系统\****

###项目简述：实现面向多个无人智能设备的管理监控平台。项目内容主要包括对无人车和无人机等多个智能终端设备在协同作用时对其进行实时监控管理，发布任务进行控制，实时状态监控监测，摄像头信息展示，历史
轨迹展示，数据的检索下载供分析等功能。
###项目职责：负责系统的前端 UI 设计和布局实现（基于 Vue 框架+Element-UI 库+Webpack）；实现无人智能设备的界面以及实现发布任务控制智能设备的运动；所有无人智能设备的信息数据检索，筛选功能。
几个重要的界面
 * 登录注册界面
 * 首页-展示无人智能设备的拍摄画面以及地图显示
 * 设备管理界面：对于设备的添加和删除，修改以及连接和断开连接
 * 历史数据界面：对于设备的行程和拍摄画面数据按照时间和设备编号进行查询
 * 用户管理界面：不同的用户会用不同的权限,如果是管理员登录，会有用户管理和权限管理的界面，可以对普通用户的权限进行更改操作，包括添加，删除，编辑普通用户信息。
 * 数据统计界面：对于无人设备的历史路程数据可以进行获取，检索，绘制成图表
#### 登录/退出功能
 1. 登录业务流程
    + 在登录页面输入用户名和密码
    + 调用后台接口进行验证
    + 通过验证后，根据后台的响应状态跳转到项目页面
 2. 登录页面的相关技术点
    + http是无状态
    + 通过cookie在客户端记录
    + 通过seeion在服务器端记录状态
    + 通过token方式维持状态
![token原理分析](https://github.com/lemon-0615/vue_shop/blob/master/image/token%E5%8E%9F%E7%90%86%E5%88%86%E6%9E%90.png)   
+ 登录界面
![登录界面](https://note.youdao.com/yws/public/resource/a590917cb48dcdcd15365607a22b2b6c/xmlnote/E73085EAFF034AB884250A2FAF91412C/5831)
### 表单内容数据验证
+ 为<el-form>通过属性绑定指定一个rules校验对象
+ 在data数据中定义校验对象rules，每一个属性对应一个规则
+ 为不同表单项，通过prop指定不同验证规则进行验证

```
   loginRules: {
        username: [{ required: true, trigger: 'blur', validator: validateUsername }],
        password: [{ required: true, trigger: 'blur', validator: validatePassword }]
         },
```
 ### WebSocket
 那么如果要实时的展现数据的变化，那么我们有2种方法

 1. 使用poll(不断的轮询)，这么将是一个低效的方法

 2. 就是在后台保持一个长连接，然后被动的触发，当有数据更新时

 基于第二种方案，我们不得不在后台打开一个原始的tcp socket连接,那么当这个TCP连接有数据接收时，那么就被动的触发了数据。所以这种方式是高效的，因为是基于事件的，而不是基于轮询的

 那么在最新的HTML5里，有一个websocket的组件，能够打开一个TCP的链接，并且是异步的，但是建立websocket的，我们需要交换一些密钥来建立链接

 所以我们不得不交换密钥，在链接建立之初，HTTP是基于TCP的，所以，我们的TCP server是完全可以接收来自浏览器的HTTP的请求

 例如如果，你建立一个TCP的server，绑定了192.168.1.1：8000端口

 那么你在浏览器打开http://192.168.1.1：8000,返回的值就是你的outputstream里的数据

那么现在唯一个问题，就是读取浏览器的请求数据，然后交互加密密钥，那么websocket就建立成功了
* 为什么你们项目组中使用WebSocket技术
  当我们在处理页面数据自动更新的时候，在使用js不断的请求服务器，查看是否有新数据，如果有就获取到新数据，进行对页面信息的跟新，但是当页面长时间没有更新数据时，这样就会存在资源浪费的情况，所以才会使用WebSocket来解决。websocket进行前后端通讯：websocket是html5的新协议，基于TCP，在一次握手后，建立http连接，实现客户端与服务端全双工通信。相比较轮询机制，节约资源，不需要频繁的请求。
* 什么是WebSocket?
  WebSocket是HTML5一种新的协议，WebSocket是真正实现了全双工通信的服务器向客户端推的互联网技术，是一种在单个TCP连接上进行全双工通讯协议。
* UDP和TCP协议的概念
  TCP是事先为所发送的数据开辟出连接好的通道，然后再进行数据发送；而UDP则不为IP提供可靠性、流控或差错恢复功能。
  一般来说，TCP对应的是可靠性要求高的应用，而UDP对应的则是可靠性要求低、传输经济的应用。
* WebSocket和Socket的区别是什么？
 Socket是应用层与TCP/IP协议通信的中间软件抽象层，它是一组接口。而WebSocket则不同，它是一个完整的应用层协议，包含一套标准的API。
* Http与WebSocket的区别？
http协议是短链接，因为请求之后，都会关闭连接，下次重新请求数据，需要再次打开连接。WebSocket协议是一种长连接，只需要通过一次请求来初始化链接，然后所有的请求和响应都是通过这个TCP链接进行通信。
* WebSocket中的常用注解有哪些？
@ServerEndpoint 类似与servlet中的 RequestMapping
@OnOpen类似与servlet中的 init（）初始化
@OnClose类似与servlet中的destroy() 销毁
@OnMessage类似于servlet中的service请求(意思就是发送数据的方式@doPost()/@doGet()组合)*
 前端的数据是在于
 WebSocket 实现数据实时刷新：
  * 创建一个WebSocket 对象：浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，连接建立以后，客户端和服务器端就可以通过 TCP 连接直接交换数据。
  * WebSocket 事件：当你获取 WebSocket 连接后，你可以通过send()方法来向服务器发送数据，send()方法是客户端和服务端建立链接时触发，此时可向服务端传递参数，客户端收到服务端发来的消息时，会触发onmessage事件，参数res.data中包含server传输过来的数据。客户端收到服务端发送的关闭连接的请求时，触发onclose事件。如果出现连接，处理，接收，发送数据失败的时候就会触发onerror事件。
  * WebSocket方法，客户端和服务器端的WebSocket连接建立起来后，双方就可以通过这个连接通道自由的传递信息，并且这个连接会持续存在直到客户端或者服务器端的某一方主动的关闭连接。
 前端的数据是和后端通过websocket来连接进行实时数据的传输，后端的数据是来源于无人智能终端设备通过基于tcp的socket进行传输，在智能终端设备是基于ros操作系统的，里面节点的数据可以进行订阅和发布，一旦我们对设备坐标的数据进行订阅，设备的位置改变，其数据就会被发布，更新后的数据就会通过socket连接传输给后端，后端和前端建立的websocket连接就进行了数据传输到前端界面上。
### 数据统计界面
使用echarts，使用步骤:
  * 导入echarts对应的包
  * 准备一个echarts的DOM区域
  * 调用echarts的init函数，将div区域初始化为echarts的图表实例myChart
  * 准备数据和配置项
  * 用option指定图表的配置项和数据 var option={}
  * 将myChart实例调用一个setOption函数，把对应的数据放置进去，展示数据
  * 用http的get请求获取数据，将服务器返回的数据和options进行合并才可以得到完整的数据 
### VUE BAIDU MAP
  * 在使用vue做项目的时候，有用到百度地图，使用了vue-baidu-map插件
  * 由于要在地图中画出很多点比较影响加载速度，查看官方文档，发现有提供加载海量点的功能BmPointCollection，用这个加快速度
 ```
    <!--所有目标--> 
    <bm-point-collection :points="markerPoints" shape="BMAP_POINT_SHAPE_STAR" color="red"></bm-point-collection>
 ```
