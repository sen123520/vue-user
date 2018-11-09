# vue使用说明

## 1 配置vuex
### 1.1 创建vuex.js
```
'use strict'
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex);//使用方法
const store = new Vuex.Store({
    state: {count: 0},//数据
    getters: {
        doneTodos: state => {
          return state.todos.filter(todo => todo.done)
        }
    },//计算属性，
    mutations: {
        increment (state) {
          state.count++
        }
    },//事件方法，变更state
    actions: {
        increment (context) {
          context.commit('increment')
        }
    },//方法，Action 提交的是 mutation
    modules: {
        a: moduleA,
        b: moduleB
    }//store 分割成模块, store.state.a // -> moduleA 的状态
})
//导出
export default store;
```
## 2 配置路由router.js
### 2.1 router.js
```
//'use strict'
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router)
//子页面
import Home from '../page/Home.vue'
import Index from '../page/Index.vue'

//路由规则
export default new Router({
	routes: [
		{
			path: '/Home',
			name: 'Home',
			component: Home
		},
		{
			path: '/',
			name: "Home",
			component: Home,
			children: [
				{
					path: 'index/:mid/:pid',
					name: 'index',
					component: Index
				}
			]
		}
	]
})
```
## 3 入口文件配置
```
import Vue from 'vue'
import router from './router'
import store from './vuex'
//自定义的页面模块
import App from './App.vue'
//隐藏调试信息
Vue.config.productionTip = false
//实例化
new Vue({
  el: '#app',
  store,
  router,
  template: '<App/>',//表示用<app></app>替换index.html里面的<div id="app"></div>
  components: { App } 
})
```

## 4 子页面路由和vuex的使用
### 4.1 App.vue
```
<template>
    <div id="app">
        <div class="views">
            <transition name="moveRightIn" mode="out-in">
            <router-view class="Router"/>
            </transition>
        </div>
    </div>
</template>

<script>
import { mapState, mapActions } from 'vuex'
import FuncView from './page/Home.vue'
export default {
    name: 'app',
    data(){
        return {}
    },
    //计算属性
    computed: {
        ...mapState(['count'])//展开值到data
    },
    ///函数
    methods: {
        showView: function(){
            //执行vuex的action方法 来改变值
            this.increment();
        },
        ...mapActions(['increment'])//展开方法到this
    },
    created: function () {}，//组件实例创建完成，但是dom未生成
    mounted(){},//挂载后的回调
    watch: {},//监听
    components: {},//引入的模块
    filters: {
    	isstr: function(value){}
    }//过滤器
</script>
```

