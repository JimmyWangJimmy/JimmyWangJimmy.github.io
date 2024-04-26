---
layout: post
title:  "SpringBoot FullStack FrontEnd Vue"
date:   2024-04-27 00:45:00 +0800
categories: jekyll update
---

### Vue
#### Vue�Ļ���ʹ��
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

#### Vue CLI��ʹ�ã�
npm install -g @vue/cli
-g��ʾȫ�ְ�װ

vue create projectname

Manually select features
ȡ�� Linter / Formatter

In package.json

N

ɾ���ĸ�������
1.components�µ�HelloWorld.vue
2.App.vue�е�`<template>`������`<div>`�е�����
<br>
ɾ��import\
ɾ��components
<br>
����1��vue: The term 'vue' is not recognized as a name of a cmdlet, function, script file, or executable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.\
������������п�����node�����⣬�л�node�汾����������nvm��<a href="#nvm">nvm����</a>
����2�� ERROR  Error: command failed: yarn
Error: command failed: yarn
���������npm install -g yarn

#### Vue ����

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
ʵ����һ��·�ɣ����Home����ת��homeҳ�档\
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

#### VScode vue��ɫ��� vetur
Vetur��һ��ר��ΪVue.js�����ߴ���Ĳ�����ṩ�˷ḻ���﷨���������ܸ�֪��Emmet�ȹ��ܡ�



#### Element-UI ForVue2
������[Element-UI for vue2](https://element.eleme.cn/#/zh-CN)
��Ŀ�����أ�
```shell
npm install element-ui
```
main.js���룺
```javascript  
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
```

��ʹ��ȫ��ע�᣺
```
Vue.use(ElementUI);
```

#### ͼ�� Font Awesome
[fontawesome����](https://fontawesome.dashgame.com/)
[FontawesomeӢ��](https://fontawesome.com/)
��װ��
```shell
npm install font-awesome
```
��main.js�����룺
```javascript
import 'font-awesome/css/font-awesome.min.css'
```

ʹ��ͼ�꣺
������\
`<i class="fa fa-user"></i>`\
ע�⣬���е�ͼ�궼����fa��ͷ�ġ�

�õģ�������һ�����ֵ�ͼ�꣺\
fa-handshake-o\
ֱ�Ӹ�������Ĵ��뵽��Ҫ�ĵط����ɡ�
```html
<i class="fa fa-handshake-o"></i>
```

ͼ���С�ĵ�����
fa-lg ��33%, fa-2x ��200%, fa-3x ��300%, fa-4x ��400%, fa-5x ��500%\
��������ԭͼ��33%\
`<i class="fa fa-user fa-lg"></i>`

#### axios
axios��һ������promise��HTTP�⣬���������������node.js�С�
�ڲ���ʵ�Ƕ�ԭ����XMLHttpRequest����ķ�װ��
axios��ajax�Ĺ�ϵ��axios�Ƕ�ajax�ķ�װ��ʹ��axios����ʹ��ajax��


���أ�
```shell
npm install axios
```

���룺
main.js�����룺
```javascript
import axios from 'axios'
```
�����������
```javascript
import axios from 'axios'
```
ʹ�ã�
```javascript
axios.get('url').then(res=>{
    console.log(res)
}).catch(err=>{
    console.log(err)
})
```
֧��ES6��д����Async/Await
������ͬ���ķ�ʽд�첽���롣
```javascript
async function getData(){
    try{
        let res = await axios.get('url')
        console.log(res)
    }catch(err){
        console.log(err)
    }
}
```
#### Vue��������
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


#### VueRouter
VueRouter����SPA��Ŀ������л�
vue-router �汾 3.X��Ӧvue�汾2 4.X��Ӧvue3
1.VueRouter�İ�װ��ʹ��
ָ���汾��
npm install vue-router@3

����Ŀ��Ŀ¼���½�router�ļ��У�Ȼ����router�ļ������½�index.js�ļ���д����������:
```js
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)
```
������������vue-router��ע�ᣬ������Ҫ�������·�ɽ��й�����

�½����������Home.vue��About.vue��Contact.vue

��index.js������import���������������Ȼ��ʹ������������·��
```js
const router = new VueRouter({
    routes : [
    { path: '/discover', component: Discover },
    { path: '/friend', component: Friend },
    { path: '/my', component: My }
]
})
```
��app.vue������vue-router
```vue
<template>
    <div id="app">
        <router-link to="/discover">��������</router-link>
        <router-link to="/friend">��ע</router-link>
        <router-link to="/my">�ҵ�����</router-link>
        <router-view></router-view>
    </div>
</template>
```
ע�������`<router-view></router-view>`�������ǩ��������ʾ��ǰ·������ġ���һ��ռλ����Ҳ����˵��ǰ·��ƥ�䵽���������ʾ�����


�����main.js�е���router
```js
import router from './router/index'
```
���router�µ��ļ���Ϊindex����ô���Բ���д��Ҳ��������ĵ���ȼ��ڣ�
```js
import router from './router'
```

ͬʱ��new Vue�����router����
```js
new Vue({
    router,
    render: h => h(App),
}).$mount('#app')
```
�����������vue-router�İ�װ��ʹ�á�

�ض�����·�������redirect����
```js
{ path: '/', redirect: '/discover' }
```
����������Ǵ�localhost:8080/���ͻ��Զ���ת��localhost:8080/discover��

Ƕ��·�ɣ������������ʹ��`<router-link>`��`<router-view>`�������Ϳ���ʵ��Ƕ��·�ɡ�

����Ƕ��·��Ҳ��Ҫ��router/index.js�н������ã����·��һ�£������ڸ�·�������children���ԣ�Ȼ���������·�ɡ�
```js
const router = new VueRouter({
    routes : [
    { path: '/discover', component: Discover },
    { path: '/friend', component: Friend },
    { path: '/my', component: My,
        children:[
            { path: '/music', component: Music },
            { path: '/video', component: Video }
        ]
    }
]
})
```

��̬·��* ��һ���ֻ������⣬�޷�ʵ����ת������ʾ���

{ path: '/my/:id', component: Product }

ͨ��{{$route.params.id}}��ȡid

2.��������
ͨ��props���ݲ���
��router/index.js�����props����
{ path: '/my/:id', component: Product, props: true }
�����Product.vue�����props����
```js
<script>
export default {
    name: 'Product',
    props: ['id']
}
```
3.��·��
4.��������
����·�ɷ���Ȩ��
![��������](./images/��������.png)
����ע��һ��ȫ��ǰ������
```js
router.beforeEach((to, from, next) => {
    if (to.path === '/main' && !isAuthenticated) {
        next('/login')
    } else {
        next()
    }
})
```
�����Ϳ���ʵ���ڷ���mainҳ��ʱ�����û�е�¼���ͻ���ת��loginҳ�档

#### Vuex-״̬����
Vuex��һ��רΪVue.jsӦ�ó��򿪷���״̬����⡣�����ü���ʽ�洢����Ӧ�õ����������״̬��
�ܹ�֧�ֶ��Ƕ�׵�������ֵ����֮�䴫ֵ��

�汾��vuex@3��Ӧvue2��vuex@4��Ӧvue3
��װ��
```shell
npm install vuex@3
```

�ҵ���⣺Vuex�Ĺؼ���ʹ��state��Ϊȫ��״̬������view��state�����˰󶨡�
\
\
\
����޸�state�أ�

����һ���������һ����ť�������ť����һ��action��

�������add����mutation�е�һ��������ͨ��commit���ύ��Ȼ��state�е�count�ͻ��1��
$this.store.commit('add')

Getter����������ά��State������һЩ״̬�������state�е����ݽ��й��ˡ�����Ȳ�����

����е���getter��$this.store.getters.������

Action���������첽����
Action����ֱ���޸�State������ͨ���ύmutation���޸ģ����԰��������첽������
��������Ĵ��룺ͨ��commit���ύһ��mutation��Ȼ���޸�state�е�count��
(ע��vuex3������vue2������store�ķ�����new Vuex.store, vuex4������vue3����Ӧ�Ĵ�����������)
```js
const store = createStore({
    state: {
    count: 0
    },
    mutations: {
        increment(state) {
            state.count++
        }
    },
    actions��{
        increment(context) {
            context.commit('increment')
        }
    }   
})
```
���ܽ�һ�£��������Ҫ�޸�state��
��һ�����ַ�action, this.$store.dispatch('increment')
�ڶ�����action���ύmutation��context.commit('increment')
��������mutation���޸�state��state.count++
[vuex](https://vuex.vuejs.org/vuex.png)

һ����Ŀdemo��
�ȴ���һ��vue2��Ŀ���ٰ�װvuex3
```js
npm install vuex@3
```
�½�һ��store�ļ��У�vuex����������½�index.js�ļ�
����vue,����vuex
Vue.use(Vuex)
Ȼ�󴴽�һ��sotre������ʾ��const store = {new Vue.Store({...})}��
��ʹ��export������

��main.js�е���store������

������п����������ķ�ʽ��ȡ��  {{ this.$store.state.count }}

���Ҫ����һ��action����������һ��button�������@click=:"add"���ڱ����methods��дһ��add��������add�����������state��mutations�е�increment�������޸�state����this.$store.commit('increment')

��������:�򵥵�˵������������ﶨ��һ��computed���ԣ�����this.$store.state.count�������Ϳ�����ģ����ֱ��ʹ��{{count}}����ʾstate�е�countֵ��

Ҳ����ʹ��MapState�����������򻯼������Ե�д������Ϊ������[mapState��������](https://vuex.vuejs.org/zh/guide/state.html#mapstate-%E8%BE%85%E5%8A%A9%E5%87%BD%E6%95%B0)
��һ�����������`<script>`�е���mapState
import { mapState } from 'vuex'
�ڶ�������computed��ʹ��mapState


��ӳ��ļ������Ե�������state���ӽڵ�������ͬʱ������ֱ�Ӵ���һ���ַ������顣
```js
computed:{
    mapState(['count'])
}
computed:{
    ...mapState(['count'])
}
```
ʹ��mapGetters,
��һ������store/index.js�ж���getters
�ڶ�����������е���mapGetters,��import { mapState, mapGetters } from 'vuex'
��������������м���������ʹ��չ��mapGetters,����ͨ��չ���﷨չ����ͬʱҪע�⣬�ղŵ�mapState��Ҳ���Խ���չ����
```js
 computed: {
    ...mapState([
    'count','todos'
    ]),
    ...mapGetters(['doneTodos'])
  }
```
Module:
> Vuex �������ǽ� store �ָ��ģ�飨module����ÿ��ģ��ӵ���Լ��� state��mutation��action��getter��������Ƕ����ģ�顪���������½���ͬ����ʽ�ķָ�

