### vuex是什么
  Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用`集中式`**存储管理应用的所有组件的状态，并以相应的规则保证状态以一种**`可预测`的方式发生变化。

### vuex配置初始化
安装 vuex

npm install vuex --save 

在src路径下创建store文件夹，然后创建index.js文件
```js
import Vue from 'vue'; 
import Vuex from 'vuex';

Vue.use(Vuex);

const store = new Vuex.Store({
  
});

export default store; //向外暴露store 
```

在main.js中配置
```js
import store from './store'; // 引入导出的 store 对象

new Vue({
  store, // 将store 挂到vue的实例上
})
```

### state 用来记录状态
在store文件夹下创建state.js
```js
//state.js
export default {
  //定义
}
```
在store的index.js中引入state.js
```js
// store/index.js
import state from './state'; //引入state

// 在store中加入
const store = new Vuex.Store({
  state,  //加入state
});

```
state的使用
```js
  // 调用state中的数据
  computed:{
        getName() {
            return this.$store.state.name;
        }
    },
  this.getName 
  // 官网推荐放在计算属性中，也可以直接调用 this.$store.state.name

  //也可以用辅助函数 mapState
  import { mapState } from 'vuex'; // 先从vuex中导入mapState

  computed:{
    ...mapState(['name']) //经过解构后，自动就添加到了计算属性中
  }
  
  this.name //可以直接调用
```

### mutations 要修改 state 中的属性，只能通过mutations,而且只能是同步操作，异步操作只能在actions中执行
在store文件夹下创建 mutations.js
```js
//muttaions.js
import state from './state'; //引入state

// console.log(state);

const mutations = {
  //定义修改state中属性的方法（同步）
  //自定义方法
  //无传参
  changeName(state){
    state.name = '星桦';
  },
  //有传参 payload 载荷 就是参数
  changeNameParams(state,payload){
    state.name = payload + state.name; 
  }
}

// 自动生成 存入state中对应值 的方法 
Object.keys(state).map(key => {
  mutations[`set${key.replace(/^[a-z]/i, val => val.toUpperCase())}`] = (state, value) =>{
    state[key] = value;
   };
});

export default mutations
```

在store的index.js中引入state.js
```js
import mutations from './mutations'; //引入mutations

const store = new Vuex.Store({
  state,
  mutations,
});

``` 

在页面中使用mutations的方法
```js
//1. 直接用.commit使用
this.$store.commit('changeName');

//2. 通过辅助函数mapMutations()
import {mapState, mapMutations} from 'vuex' //引入
// 在methods 中引用 因为是方法 
methods:{
  ...mapMutations(['changeName']),
}
// 如果用自动写入的方法,直接加上即可 以 set 开头 大驼峰命名 Age  -> setAge(值)  在 state 中必须有 age 这个属性 
methods:{
  ...mapMutations(['changeName','setAge']),
}

this.changeName();
this.setAge(18);

//普通调用
this.$store.commit('changeName');
this.$store.commit('changeNameParams', '参数'); //有参数

```

### getters 相当于vue的计算属性， 也是修饰器， 可以快捷访问state的属性
在store文件夹下创建 getters.js
```js
//getters.js
import state from './state';

const getters = {};

Object.keys(state).map(key => {
  getters[key] = state => state[key];  //快捷访问
});


// 修饰state中的值
// getters: {
//   getMessage(state){ // 获取修饰后的name，第一个参数state为必要参数，必须写在形参上
//       return `hello${state.name}`;
//   }
// }

export default getters
```

在store的index.js中引入getters.js
```js
import getters from './getters'; //引入getters

const store = new Vuex.Store({
  state,
  mutations,
  getters,
});
```

在页面中使用getters的方法
```js
// 辅助函数mapGetters
import {mapState, mapMutations,mapGetters} from 'vuex' //引入
//解构在计算属性中
computed: {
    ...mapGetters(['age'])
  },
//调用
this.age

//同理也可用直接调用的方法 
this.$store.getters.age
```

### actions 异步操作，异步调用mutations的方法
创建一个actions.js文件
```js
import mutations from './mutations'; //引入mutations

console.log(mutations);
const actions = {
  setNowTime (content) {  // 增加setNowTime方法，默认第一个参数是content，其值是复制的一份store
    setTimeout(() => {
      content.commit('changeTime');  //直接用 .commit的方式调用mutations的方法
    }, 2000);
  }
};

export default actions;
```
在store的index.js中引入actions.js
```js
import actions from './actions'; //引入actions

const store = new Vuex.Store({
  state,
  mutations,
  getters,
  actions
});
```

通过辅助函数 mapActions 调用
```js
import {mapState, mapMutations,mapGetters, mapActions} from 'vuex'

 methods:{
    ...mapActions(['setNowTime'])
  },

this.setNowTime();

//也可直接调用    通过 .dispatch
this.$store.dispatch('setNowTime');
```

