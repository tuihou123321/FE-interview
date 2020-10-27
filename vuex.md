## vuex



[TOC]



### vuex如何使用？

1. 把vuex从根组件直接注入到vue的每个子组件中,Vue.use(vuex)
2. 创建一个vuex的store，并定义好其他的参数（state,getter,mutations,action）
3. 导出vuex创建的对象store;
4. 把导出的store插入到创建的vue实例中
5. 在.vue模板的computed就可以直接过 this.$store.state.value; 来访问store中的数据了；



### vue模板中如何访问vuex中的store(state,action)?

获取state:     

直接获取：this.$store.state.value;

批量获取 ：



引入store页面中

let state=this.$store.state  可以直接获取store中state的值；



import Vue from "vue"

import Vuex from "vuex"

vue.use(Vuex);  //直接把vuex动作中vue中，所以在vue中直接



let store=new Vuex.Store{

​    state:{

​    },

​    getter:{

​    },

​    mutations:{

​    },

​    action:{

​    }

}