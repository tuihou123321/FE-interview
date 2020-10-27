## vue



[TOC]



### vue双向数据绑定原理？

vue双向数据绑定是通过【数据劫持】+【发布者-订阅者模式】方式来实现的 ；

vue数据劫持通过：Object.defineProperty() 方法，它有两个描述属性set（添加触发），get（调用触发）;

当对象在添加属性的时候会触发 set方法，再读取的时候会触发get方法；

mvvm包含两方面的内容：

view到model:

1. 监听input框中的事件，把改变后的值重置到model中；

model到view:

1. model数据改变的时候会触发Object.defineProperty()的set方法
2. 再通过更新后 的数据展示在view中就可以了；

实现过程：

1. 实现一个监听器Observer,用来劫持监听所有的属性，如有变动通知订阅者；
2. 实现一个订阅者Watcher, 收到属性变化的通知执行相应的函数，从而更新视图
3. 实现一个解析器Compile,可以扫描和解析每个节点的相关指令，





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