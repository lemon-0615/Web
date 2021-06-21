
# 项目：
## ***\*无人智能终端管理系统\****

###项目简述：实现面向多个无人智能设备的管理监控平台。项目内容主要包括对无人车和无人机等多个智能终端设备在协同作用时对其进行实时监控管理，发布任务进行控制，实时状态监控监测，摄像头信息展示，历史
轨迹展示，数据的检索下载供分析等功能。
###项目职责：负责系统的前端 UI 设计和布局实现（基于 Vue 框架+Element-UI 库+Webpack）；实现无人智能设备的界面以及实现发布任务控制智能设备的运动；所有无人智能设备的信息数据检索，筛选功能。
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
 那么如果要实时的展现数据的变化，那么我们有2种方法

1,使用poll(不断的轮询)，这么将是一个低效的方法

2,就是在后台保持一个长连接，然后被动的触发，当有数据更新时


基于第二种，方案，我们不得不在后台打开一个原始的tcp socket连接,那么当这个TCP连接有数据接收时，那么就被动的触发了数据

所以这种方式是高效的，因为是基于事件的，而不是基于轮询的


那么在最新的HTML5里，有一个websocket的组件，能够打开一个TCP的链接，并且是异步的

但是建立websocket的，我们需要交换一些密钥来建立链接

所以我们不得不交换密钥，在链接建立之初


而HTTP是基于TCP的，所以，我们的TCP server是完全可以接收来自浏览器的HTTP的请求

例如如果，你建立一个TCP的server，绑定了192.168.1.1：8000端口

那么你在浏览器打开http://192.168.1.1：8000,返回的值就是你的outputstream里的数据

那么现在唯一个问题，就是读取浏览器的请求数据，然后交互加密密钥，那么websocket就建立成功了
