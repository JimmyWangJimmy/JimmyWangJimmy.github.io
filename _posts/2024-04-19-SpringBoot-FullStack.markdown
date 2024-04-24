---
layout: post
title:  "SpringBoot Full Stack"
date:   2024-04-19 15:32:42 +0800
categories: jekyll update
---
## Resouces
-[1天搞定SpringBoot+Vue全栈开发](https://www.bilibili.com/video/BV1nV4y1s7ZN/)

## Spring Boot Full Stack

### Spring Boot

###### IDEA实现热启动
-TO BE DONE


###### 使用Swagger 创建Restful风格API
1.添加依赖
swagger2  swagger-ui
2.Apioperation（''）
3.配置类
4.访问http://localhost:8080/swagger-ui.html

###### ORM
ORM(Object Relational Mapping)对象关系映射
解决面向对象和关系型数据库之间的不匹配问题
MyBatis 数据持久层的框架

依赖：
1.MyBatis
2.Mysql
3.druid

###### Mybatis-plus
如果查询的数据库里不存在该字段，则加上注解：
@TableField(exist = false)

对于外表查询，可以使用：
[多表查询](E:\MyGithubWeb\SpringBoot-FullStack\外表查询.png)

以用户和订单为例。
一个用户对应多个订单，一个订单对应唯一一个用户。

在用户的实体类中，有orders这样的字段，但是数据库中并没有。可以通过
在用户中查询该用户的所有订单，可以在UserMapper中这样写:
查询所有用户和所有订单
@Select("select * from t_user)
@Results(
    {
        @Result(column="id", property="id"),
        @Result(column="username", property="username"),
        @Result(password="password", property="password"),
        @Result(birthday="birthday", property="birthday"),
        @Result(column="id", property="orders",
            many=@Many(select="com.example.demo.mapper.OrderMapper.selectByuid")
        )
    }
)
List<User> selectAllUserAndOrders();
调用OrderMapper中的selectByuid方法，传入用户的id，返回该用户的所有订单。

再来看看OrderMapper
@Select("select * from t_order where uid=#{uid}")
List<Order> selectByuid(int uid);
查询出来的值会交给上面的orders属性，完成赋值。

总结一下：查询所有用户及他们自己的订单，首先在User实体类中加入属性orders，然后在UserMapper中写一个查询所有用户及他们的订单的方法，再在OrderMapper中写一个查询某个用户的所有订单的方法。

条件查询 QueryWrapper
@GETMapping("/user/find")
public List<User> findByCond(){
    QueryWrapper<User> queryWrapper= new QueryWrapper();
    queryWrapper.eq("username","zhangsan");
    return userMapper.selectList(queryWrapper);
}

分页查询：
1.
@GETMapping("/user/findByPage")
public IPage findByPage(){
    Page<User> page = new Page<>(0,2);
    IPage ipage = userMapper.selectPage(page,null);
    return ipage;
}
2.在配置类中
添加分页拦截器


### Vue
事件绑定：
v-on:click 等价于 @:click

条件渲染：
v-if="bool"
传递的是一个bool值，如果为true，则渲染，否则不渲染。
v-show="bool"

两者的差异：如果为false，v-if不会渲染，v-show会渲染，只是不显示。

列表渲染：
v-for="(item,index) in items"
items是一个数组，item是数组中的元素，index是元素的下标。

key属性
<li v-for="(item,index) in items" :key="uniquekey"></li>
双向绑定与单向绑定
v-model 与 :value

###### Vue CLI的使用：
npm install -g @vue/cli
-g表示全局安装

vue create projectname

Manually select features
取消 Linter / Formatter

In package.json

N

问题1：vue: The term 'vue' is not recognized as a name of a cmdlet, function, script file, or executable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
解决方案：很有可能是node的问题，切换node版本重新配置了nvm：<a href="#nvm">nvm配置</a>
问题2： ERROR  Error: command failed: yarn
Error: command failed: yarn
解决方案：npm install -g yarn

###### Vue 构成
1.组件
组件是Vue的核心，可以理解为一个自定义标签，可以在一个页面中多次使用。
2.指令
指令是Vue的特殊属性，可以在标签上添加，用来实现一些特殊的功能。比如v-if,v-show,v-for等。
举个例子：
v-if="bool"
v-show="bool"
v-for="(item,index) in items"
3.事件
事件是Vue的特殊属性，可以在标签上添加，用来实现一些特殊的功能。比如@click,@change等。
举个例子：
@click="handleClick"
@change="handleChange"
4.插槽
插槽是Vue的特殊属性，可以在组件中添加，用来实现一些特殊的功能。比如slot。
举个例子：
<slot></slot>
5.过滤器
过滤器是Vue的特殊属性，可以在标签上添加，用来实现一些特殊的功能。比如filter。
举个例子：
{{msg | filter}}
实现了一个过滤器，将msg的值传入filter中，然后返回一个新的值。
6.路由
路由是Vue的特殊属性，可以在组件中添加，用来实现一些特殊的功能。比如router-link。
举个例子：
<router-link to="/home">Home</router-link>
实现了一个路由，点击Home，跳转到home页面。
7.状态管理
状态管理是Vue的特殊属性，可以在组件中添加，用来实现一些特殊的功能。比如vuex。
举个例子：
import Vuex from 'vuex'
Vue.use(Vuex)
const store = new Vuex.Store({
    state:{
        count:0
    },
    mutations:{
        increment(state){
            state.count++
        }
    }
})
实现了一个状态管理，count的值为0，点击按钮，count的值加1。

###### VScode vue着色插件 vetur
Vetur是一个专门为Vue.js开发者打造的插件，提供了丰富的语法高亮、智能感知、Emmet等功能。



###### Element-UI ForVue2
官网：[Element-UI for vue2](https://element.eleme.cn/#/zh-CN)
项目中下载：
npm install element-ui
main.js引入：
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

并使用全局注册：
Vue.use(ElementUI);

###### 图标 Font Awesome
[fontawesome中文](https://fontawesome.dashgame.com/)
[Fontawesome英文](https://fontawesome.com/)
安装：
npm install font-awesome
在main.js中引入：

import 'font-awesome/css/font-awesome.min.css'

使用图标：
样例：
<i class="fa fa-user"></i>
注意，所有的图标都是以fa开头的。

好的，下面是一个握手的图标：
fa-handshake-o
直接复制下面的代码到需要的地方即可。
<i class="fa fa-handshake-o"></i>

图标大小的调整：
fa-lg 大33% fa-2x 大200% fa-3x 大300% fa-4x 大400% fa-5x 大500%
样例：比原图大33%
<i class="fa fa-user fa-lg"></i>

###### axios
axios是一个基于promise的HTTP库，可以用在浏览器和node.js中。
内部其实是对原生的XMLHttpRequest对象的封装。
axios和ajax的关系：axios是对ajax的封装，使用axios就是使用ajax。


下载：
npm install axios

导入：
main.js中引入：
import axios from 'axios'

或者组件导入
import axios from 'axios'

使用：
axios.get('url').then(res=>{
    console.log(res)
}).catch(err=>{
    console.log(err)
})

支持ES6的写法：Async/Await
可以用同步的方式写异步代码。
async function getData(){
    try{
        let res = await axios.get('url')
        console.log(res)
    }catch(err){
        console.log(err)
    }
}

###### Vue生命周期
Vue实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载DOM、渲染-更新-销毁等一系列过程，我们称这是Vue的生命周期。

created和mounted:


函数名	描述
beforeCreate	在实例初始化之后，数据观测(data observer)和event/watcher事件配置之前被调用。
created	实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算，watch/event事件回调。然而，挂载阶段还没开始，$el属性目前不可见。
beforeMount	在挂载开始之前被调用：相关的render函数首次被调用。
mounted	el被新创建的vm.$el替换，并挂载到实例上去之后调用该钩子。
beforeUpdate	数据更新时调用，发生在虚拟DOM重新渲染和打补丁之前。
updated	由于数据更改导致的虚拟DOM重新渲染和打补丁，在这之后会调用该钩子。
beforeDestroy	实例销毁之前调用。在这一步，实例仍然完全可用。
destroyed	Vue实例销毁后调用。调用后，Vue实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
activated	keep-alive组件激活时调用。
deactivated	keep-alive组件停用时调用。

举个例子：
创建一个HelloWorld.vue组件，然后在组件中添加生命周期函数。
```vue
<template>
    <div>
        <h1>Hello World</h1>
    </div>
</template>
<script>
export default {
    beforeCreate(){
        console.log('beforeCreate')
    },
    created(){
        console.log('created')
    },
    beforeMount(){
        console.log('beforeMount')
    },
    mounted(){
        console.log('mounted')
    },
    beforeUpdate(){
        console.log('beforeUpdate')
    },
    updated(){
        console.log('updated')
    },
    beforeDestroy(){
        console.log('beforeDestroy')
    },
    destroyed(){
        console.log('destroyed')
    }
}
</script>
<style>
h1{
    color:red;
}
</style>
```
在App.vue中引入HelloWorld.vue组件。
```vue
<template>
    <div id="app">
        <HelloWorld/>
    </div>
</template>
<script>
import HelloWorld from './components/HelloWorld.vue'
export default {
    components:{
        HelloWorld
    }
}
</script>
<style>
#app{
    font-size:20px;
}
</style>
```
在浏览器中打开控制台，可以看到生命周期函数的执行顺序。

###### NVM- Node Version Manager <a name="nvm">
[安装nvm，并使用nvm安装nodejs及配置环境变量](https://blog.csdn.net/JJ_Smilewang/article/details/127823953)


### 其他问题
###### 解决跨域问题
后端使用CORS解决跨域问题：

CORS（Cross-Origin Resource Sharing）跨域资源共享，是一种机制，它使用额外的HTTP头来告诉浏览器，允许一个网页的访问另一个网站的资源。

简单请求的服务器处理：
在请求头中加一个Origin字段，表示请求的源地址。
服务器在收到请求后，在响应头中加一个Access-Control-Allow-Origin字段，表示允许的源地址。

非简单请求的服务器处理：
浏览器会先发送一个OPTIONS请求，询问服务器是否允许跨域。（也称为预检请求Preflight request）
markdown插入本地图片:
![图片](E:\MyGithubWeb\SpringBoot-FullStack\跨域-非简单请求.png)

后端可以使用SpringBoot解决跨域问题：
可以通过配置类的方式解决跨域问题：
addCorsMappings()
也可以在Controller中使用注解@CrossOrigin解决跨域问题。

网络请求可以用箭头函数解决this作用域的问题，this作用域和created()函数中的this作用域不一样，所以需要用箭头函数解决。
用箭头函数解决this作用域的问题：
例子：
```javascript
methods:{
    getData(){
        axios.get('url').then(res=>{
            console.log(this)
        })
    }
}
```

###### 设置Baseurl
在main.js中设置axios的baseURL：
```javascript
axios.defaults.baseURL = 'http://localhost:8080'
Vue.prototype.$http = axios
```
这样一来我们的使用中就可以
this.$http.get(/user) 等价于 axios.get(http://localhost:8080/user)
