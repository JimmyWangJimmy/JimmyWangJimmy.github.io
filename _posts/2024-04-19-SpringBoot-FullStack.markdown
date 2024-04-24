---
layout: post
title:  "SpringBoot Full Stack"
date:   2024-04-19 15:32:42 +0800
categories: jekyll update
---
## Resouces
-[1��㶨SpringBoot+Vueȫջ����](https://www.bilibili.com/video/BV1nV4y1s7ZN/)

## Spring Boot Full Stack

### Spring Boot

###### IDEAʵ��������
-TO BE DONE


###### ʹ��Swagger ����Restful���API
1.�������
swagger2  swagger-ui
2.Apioperation��''��
3.������
4.����http://localhost:8080/swagger-ui.html

###### ORM
ORM(Object Relational Mapping)�����ϵӳ��
����������͹�ϵ�����ݿ�֮��Ĳ�ƥ������
MyBatis ���ݳ־ò�Ŀ��

������
1.MyBatis
2.Mysql
3.druid

###### Mybatis-plus
�����ѯ�����ݿ��ﲻ���ڸ��ֶΣ������ע�⣺
@TableField(exist = false)

��������ѯ������ʹ�ã�
[����ѯ](E:\MyGithubWeb\SpringBoot-FullStack\����ѯ.png)

���û��Ͷ���Ϊ����
һ���û���Ӧ���������һ��������ӦΨһһ���û���

���û���ʵ�����У���orders�������ֶΣ��������ݿ��в�û�С�����ͨ��
���û��в�ѯ���û������ж�����������UserMapper������д:
��ѯ�����û������ж���
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
����OrderMapper�е�selectByuid�����������û���id�����ظ��û������ж�����

��������OrderMapper
@Select("select * from t_order where uid=#{uid}")
List<Order> selectByuid(int uid);
��ѯ������ֵ�ύ�������orders���ԣ���ɸ�ֵ��

�ܽ�һ�£���ѯ�����û��������Լ��Ķ�����������Userʵ�����м�������orders��Ȼ����UserMapper��дһ����ѯ�����û������ǵĶ����ķ���������OrderMapper��дһ����ѯĳ���û������ж����ķ�����

������ѯ QueryWrapper
@GETMapping("/user/find")
public List<User> findByCond(){
    QueryWrapper<User> queryWrapper= new QueryWrapper();
    queryWrapper.eq("username","zhangsan");
    return userMapper.selectList(queryWrapper);
}

��ҳ��ѯ��
1.
@GETMapping("/user/findByPage")
public IPage findByPage(){
    Page<User> page = new Page<>(0,2);
    IPage ipage = userMapper.selectPage(page,null);
    return ipage;
}
2.����������
��ӷ�ҳ������


### Vue
�¼��󶨣�
v-on:click �ȼ��� @:click

������Ⱦ��
v-if="bool"
���ݵ���һ��boolֵ�����Ϊtrue������Ⱦ��������Ⱦ��
v-show="bool"

���ߵĲ��죺���Ϊfalse��v-if������Ⱦ��v-show����Ⱦ��ֻ�ǲ���ʾ��

�б���Ⱦ��
v-for="(item,index) in items"
items��һ�����飬item�������е�Ԫ�أ�index��Ԫ�ص��±ꡣ

key����
<li v-for="(item,index) in items" :key="uniquekey"></li>
˫����뵥���
v-model �� :value

###### Vue CLI��ʹ�ã�
npm install -g @vue/cli
-g��ʾȫ�ְ�װ

vue create projectname

Manually select features
ȡ�� Linter / Formatter

In package.json

N

����1��vue: The term 'vue' is not recognized as a name of a cmdlet, function, script file, or executable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
������������п�����node�����⣬�л�node�汾����������nvm��<a href="#nvm">nvm����</a>
����2�� ERROR  Error: command failed: yarn
Error: command failed: yarn
���������npm install -g yarn

###### Vue ����
1.���
�����Vue�ĺ��ģ��������Ϊһ���Զ����ǩ��������һ��ҳ���ж��ʹ�á�
2.ָ��
ָ����Vue���������ԣ������ڱ�ǩ����ӣ�����ʵ��һЩ����Ĺ��ܡ�����v-if,v-show,v-for�ȡ�
�ٸ����ӣ�
v-if="bool"
v-show="bool"
v-for="(item,index) in items"
3.�¼�
�¼���Vue���������ԣ������ڱ�ǩ����ӣ�����ʵ��һЩ����Ĺ��ܡ�����@click,@change�ȡ�
�ٸ����ӣ�
@click="handleClick"
@change="handleChange"
4.���
�����Vue���������ԣ��������������ӣ�����ʵ��һЩ����Ĺ��ܡ�����slot��
�ٸ����ӣ�
<slot></slot>
5.������
��������Vue���������ԣ������ڱ�ǩ����ӣ�����ʵ��һЩ����Ĺ��ܡ�����filter��
�ٸ����ӣ�
{{msg | filter}}
ʵ����һ������������msg��ֵ����filter�У�Ȼ�󷵻�һ���µ�ֵ��
6.·��
·����Vue���������ԣ��������������ӣ�����ʵ��һЩ����Ĺ��ܡ�����router-link��
�ٸ����ӣ�
<router-link to="/home">Home</router-link>
ʵ����һ��·�ɣ����Home����ת��homeҳ�档
7.״̬����
״̬������Vue���������ԣ��������������ӣ�����ʵ��һЩ����Ĺ��ܡ�����vuex��
�ٸ����ӣ�
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
ʵ����һ��״̬����count��ֵΪ0�������ť��count��ֵ��1��

###### VScode vue��ɫ��� vetur
Vetur��һ��ר��ΪVue.js�����ߴ���Ĳ�����ṩ�˷ḻ���﷨���������ܸ�֪��Emmet�ȹ��ܡ�



###### Element-UI ForVue2
������[Element-UI for vue2](https://element.eleme.cn/#/zh-CN)
��Ŀ�����أ�
npm install element-ui
main.js���룺
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

��ʹ��ȫ��ע�᣺
Vue.use(ElementUI);

###### ͼ�� Font Awesome
[fontawesome����](https://fontawesome.dashgame.com/)
[FontawesomeӢ��](https://fontawesome.com/)
��װ��
npm install font-awesome
��main.js�����룺

import 'font-awesome/css/font-awesome.min.css'

ʹ��ͼ�꣺
������
<i class="fa fa-user"></i>
ע�⣬���е�ͼ�궼����fa��ͷ�ġ�

�õģ�������һ�����ֵ�ͼ�꣺
fa-handshake-o
ֱ�Ӹ�������Ĵ��뵽��Ҫ�ĵط����ɡ�
<i class="fa fa-handshake-o"></i>

ͼ���С�ĵ�����
fa-lg ��33% fa-2x ��200% fa-3x ��300% fa-4x ��400% fa-5x ��500%
��������ԭͼ��33%
<i class="fa fa-user fa-lg"></i>

###### axios
axios��һ������promise��HTTP�⣬���������������node.js�С�
�ڲ���ʵ�Ƕ�ԭ����XMLHttpRequest����ķ�װ��
axios��ajax�Ĺ�ϵ��axios�Ƕ�ajax�ķ�װ��ʹ��axios����ʹ��ajax��


���أ�
npm install axios

���룺
main.js�����룺
import axios from 'axios'

�����������
import axios from 'axios'

ʹ�ã�
axios.get('url').then(res=>{
    console.log(res)
}).catch(err=>{
    console.log(err)
})

֧��ES6��д����Async/Await
������ͬ���ķ�ʽд�첽���롣
async function getData(){
    try{
        let res = await axios.get('url')
        console.log(res)
    }catch(err){
        console.log(err)
    }
}

###### Vue��������
Vueʵ����һ���������������ڣ�Ҳ���Ǵӿ�ʼ��������ʼ�����ݡ�����ģ�塢����DOM����Ⱦ-����-���ٵ�һϵ�й��̣����ǳ�����Vue���������ڡ�

created��mounted:


������	����
beforeCreate	��ʵ����ʼ��֮�����ݹ۲�(data observer)��event/watcher�¼�����֮ǰ�����á�
created	ʵ���Ѿ��������֮�󱻵��á�����һ����ʵ����������µ����ã����ݹ۲�(data observer)�����Ժͷ��������㣬watch/event�¼��ص���Ȼ�������ؽ׶λ�û��ʼ��$el����Ŀǰ���ɼ���
beforeMount	�ڹ��ؿ�ʼ֮ǰ�����ã���ص�render�����״α����á�
mounted	el���´�����vm.$el�滻�������ص�ʵ����ȥ֮����øù��ӡ�
beforeUpdate	���ݸ���ʱ���ã�����������DOM������Ⱦ�ʹ򲹶�֮ǰ��
updated	�������ݸ��ĵ��µ�����DOM������Ⱦ�ʹ򲹶�������֮�����øù��ӡ�
beforeDestroy	ʵ������֮ǰ���á�����һ����ʵ����Ȼ��ȫ���á�
destroyed	Vueʵ�����ٺ���á����ú�Vueʵ��ָʾ�����ж��������󶨣����е��¼��������ᱻ�Ƴ������е���ʵ��Ҳ�ᱻ���١�
activated	keep-alive�������ʱ���á�
deactivated	keep-alive���ͣ��ʱ���á�

�ٸ����ӣ�
����һ��HelloWorld.vue�����Ȼ�������������������ں�����
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
��App.vue������HelloWorld.vue�����
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
��������д򿪿���̨�����Կ����������ں�����ִ��˳��

###### NVM- Node Version Manager <a name="nvm">
[��װnvm����ʹ��nvm��װnodejs�����û�������](https://blog.csdn.net/JJ_Smilewang/article/details/127823953)


### ��������
###### �����������
���ʹ��CORS����������⣺

CORS��Cross-Origin Resource Sharing��������Դ������һ�ֻ��ƣ���ʹ�ö����HTTPͷ�����������������һ����ҳ�ķ�����һ����վ����Դ��

������ķ���������
������ͷ�м�һ��Origin�ֶΣ���ʾ�����Դ��ַ��
���������յ����������Ӧͷ�м�һ��Access-Control-Allow-Origin�ֶΣ���ʾ�����Դ��ַ��

�Ǽ�����ķ���������
��������ȷ���һ��OPTIONS����ѯ�ʷ������Ƿ�������򡣣�Ҳ��ΪԤ������Preflight request��
markdown���뱾��ͼƬ:
![ͼƬ](E:\MyGithubWeb\SpringBoot-FullStack\����-�Ǽ�����.png)

��˿���ʹ��SpringBoot����������⣺
����ͨ��������ķ�ʽ����������⣺
addCorsMappings()
Ҳ������Controller��ʹ��ע��@CrossOrigin����������⡣

������������ü�ͷ�������this����������⣬this�������created()�����е�this������һ����������Ҫ�ü�ͷ���������
�ü�ͷ�������this����������⣺
���ӣ�
```javascript
methods:{
    getData(){
        axios.get('url').then(res=>{
            console.log(this)
        })
    }
}
```

###### ����Baseurl
��main.js������axios��baseURL��
```javascript
axios.defaults.baseURL = 'http://localhost:8080'
Vue.prototype.$http = axios
```
����һ�����ǵ�ʹ���оͿ���
this.$http.get(/user) �ȼ��� axios.get(http://localhost:8080/user)
