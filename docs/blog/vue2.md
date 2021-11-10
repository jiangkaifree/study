
![](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9d410eb017fe4dc993aac2df96c6c509~tplv-k3u1fbpfcp-zoom-1.image)

## 1.优雅更新 `props`

在子组件中不允许直接修改 `props`，因为这种做法不符合单向数据流的原则，在开发模式下还会报出警告。因此大多数人会通过 `$emit` 触发自定义事件，在父组件中接收该事件的传值来更新 `prop`。

```js
//child.vue
export defalut {
    props: {
        title: String
    },
    methods: {
        changeTitle(){
            this.$emit('change-title', 'hello')
        }
    }
}
```

```js
//parent.vue
<child :title="title" @change-title="changeTitle"></child>

export default {
    data(){
        return {
            title: 'title'
        }
    },
    methods: {
        changeTitle(title){
            this.title = title
        }
    }
}
```

这种做法没有问题，就是单纯更新 `props`。没有其他操作。那么使用 .`sync`修饰符让做法变得简单。

```js
<child :title.sync="title"></child>

export defalut {
    props: {
        title: String
    },
    methods: {
        changeTitle(){
            this.$emit('update:title', 'hello')
        }
    }
}

```

只需要在绑定的属性上面添加  `.sync` 在子组件内部就可以触发 `update:`属性名 来更新 prop。可以看到这种手段确实简洁且优雅。TIPS: 但是据说在快要到来的vue3中将不再支持` .sync`修饰符。

## 2.provide/inject

这对选项需要一起使用，以允许一个祖先组件向其所有子孙后代注入一个依赖，不论组件层次有多深，并在其上下游关系成立的时间里始终生效。

```js
//app.vue
export default {
    provide() {
        return {
            app: this
        }
    }
}
```

```js
export default {
    inject: ['app'],
    created() {
        console.log(this.app) // App.vue实例
    }
}
//这样子就可以访问到了
```

如果你想 `inject` 属性变更名称， 可以使用 `from` 来表示起来源：

```js
export default {
    inject: {
        myApp: {
            // from的值和provide的属性名保持一致
            from: 'app',
            default: () => ({})
        }
    },
    created() {
        console.log(this.myApp)
    }
}
```

## 3.小型状态管理器

大型项目中的数据状态会比较复杂，一般都会使用 vuex 来管理。但在一些小型项目或状态简单的项目中，为了管理几个状态而引入一个库，显得有些笨重。
在 2.6.0+ 版本中，新增的 Vue.observable 可以帮助我们解决这个尴尬的问题，它能让一个对象变成响应式数据：

```js
// store.js
import Vue from 'vue'

export const state = Vue.observable({
  count: 0
})
```

```js
<div @click="setCount">{{ count }}</div>

//使用
import {state} from '../store.js'

export default {
    computed: {
        count() {
            return state.count
        }
    },
    methods: {
        setCount() {
            state.count++
        }
    }
}
```

当然你也可以自定义 mutation 来复用更改状态的方法：

```js
import Vue from 'vue'

export const state = Vue.observable({
  count: 0
})

export const mutations = {
  SET_COUNT(payload) {
    if (payload > 0) {
        state.count = payload
    }
  }
}
```

使用：
```js
import {state, mutations} from '../store.js'

export default {
    computed: {
        count() {
            return state.count
        }
    },
    methods: {
        setCount() {
            mutations.SET_COUNT(100)
        }
    }
}
```

## 3.卸载watch观察

通常我们使用 `watch`

```js
export default {
    data() {
        return {
            count: 1
        }
    },
    watch: {
        count(newVal) {
            console.log('count 新值：'+newVal)
        }
    }
}
```

另一种方式：

```js
export default {
    data() {
        return {
            count: 1
        }
    },
    created() {
        this.$watch('count', function(){
            console.log('count 新值：'+newVal)
        })
    }
}
```

它和前者的作用一样，但这种方式使定义数据观察更灵活，而且 `$watch` 会返回一个取消观察函数，用来停止触发回调：

```js
let unwatchFn = this.$watch('count', function(){
    console.log('count 新值：'+newVal)
})
this.count = 2 // log: count 新值：2
unwatchFn()				//
this.count = 3 // 什么都没有发生...
```

`$watch` 第三个参数接收一个配置选项：

```js
this.$watch('count', function(){
    console.log('count 新值：'+newVal)
}, {
    immediate: true // 立即执行watch
})
//第一个参数是监听的 对象，然后是回调函数，最后一个是配置项 有 ` immediate` `deep `
```

## 4.巧用 `template`

相信 `v-if` 在开发中是用得最多的指令，那么你一定遇到过这样的场景，多个元素需要切换，而且切换条件都一样，一般都会使用一个元素包裹起来，在这个元素上做切换。

```js
<div v-if="status==='ok'">
    <h1>Title</h1>
    <p>Paragraph 1</p>
    <p>Paragraph 2</p>
</div>
<div v-if="status==='cancel'">
    <h1>Title</h1>
    <p>Paragraph 1</p>
    <p>Paragraph 2</p>
</div>
```

如果像上面的 div 只是为了切换条件而存在，还导致元素层级嵌套多一层，那么它没有“存在的意义”。
我们都知道在声明页面模板时，所有元素需要放在 `<template> `元素内。除此之外，它还能在模板内使用，`<template>` 元素作为不可见的包裹元素，只是在运行时做处理，最终的渲染结果并不包含它。

```js
<template>
    <div>
        <template v-if="status==='ok'">
          <h1>Title</h1>
          <p>Paragraph 1</p>
          <p>Paragraph 2</p>
        </template>
    </div>
</template>
```

同样的，我们也可以在 `<template>` 上使用 v-for 指令，这种方式还能解决 `v-for` 和 v-if 同时使用报出的警告问题。

## 5.过滤器复用

过滤器被用于一些常见的文本格式化，被添加在表达式的尾部，由“管道”符号指示

```js
<div>{{ text | capitalize }}</div>

export default {
    data() {
        return {
            text: 'hello'
        }
    },
    filters: {
        capitalize: function (value) {
            if (!value) return ''
            value = value.toString()
            return value.charAt(0).toUpperCase() + value.slice(1)
         }
    }
}
```

试想一个场景，不仅模板内用到这个函数，在 method 里也需要同样功能的函数。但过滤器无法通过 this 直接引用，难道要在 methods 再定义一个同样的函数吗？
要知道，选项配置都会被存储在实例的 $options 中，所以只需要获取 this.$options.filters 就可以拿到实例中的过滤器。

使用

```js
export default {
    methods: {
        getDetail() {
            this.$api.getDetail({
                id: this.id
            }).then(res => {
                let capitalize = this.$options.filters.capitalize
                this.title = capitalize(res.data.title)
            })
        }
    }
}
```

## 6.路由懒加载(动态chunkName)

```js
// 未使用懒加载

import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWor
Vue.use(Rout
export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component:HelloWorld
    }
  ]
})
```

路由懒加载作为性能优化的一种手段，它能让路由组件延迟加载。通常我们还会为延迟加载的路由添加“魔法注释”(webpackChunkName)来自定义包名，在打包时，该路由组件会被单独打包出来。

```js
// 写法1  webpack要是2.6版本以上
let router = new Router({
  routes: [
    {
      path:'/login',
      name:'login',
      component: import(/* webpackChunkName: "login" */ `@/views/login.vue`)
    },
    {
      path:'/index',
      name:'index',
      component: import(/* webpackChunkName: "index" */ `@/views/index.vue`)
    },
    {
      path:'/detail',
      name:'detail',
      component: import(/* webpackChunkName: "detail" */ `@/views/detail.vue`)
    }
  ]
})
```

```js
// 方法2  ES6语法写法
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

const HelloWorld = ()=>import("@/components/HelloWorld")
export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component:HelloWorld
    }
  ]
})
```

```js
// 方法2优化，方法2需要写很多的重复代码，我们使用map（）来映射
const routeOptions = [
  {
    path:'/login',
    name:'login',
  },
  {
    path:'/index',
    name:'index',
  },
  {
    path:'/detail',
    name:'detail',
  },
]

const routes = routeOptions.map(route => {
  if (!route.component) {
    route = {
      ...route,
      component: () => import(`@/views/${route.name}.vue`)
    }
  }
  return route
})

let router = new Router({
  routes
})
```

```js
// AMD标准的 require 方法
import Vue from 'vue'
import Router from 'vue-router'
Vue.use(Router)
export default new Router({
  routes: [
        {
          path: '/',
          name: 'HelloWorld',
          component: resolve => require(['@/components/HelloWorld'], resolve)
        }
  ]
})
```

同样的 我们的组件也是可以使用懒加载

```js
// es6标准的import 语法
<template>
  <div class="hello">
  <One-com></One-com>
  1111
  </div>
</template>

<script>
const One = ()=>import("./one");
export default {
  components:{
    "One-com":One
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

```

```js
//  同样的也有 require方式
<template>
  <div class="hello">
  <One-com></One-com>
  1111
  </div>
</template>

<script>
export default {
  components:{
    "One-com":resolve=>(['./one'],resolve)
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

```


## 7.路由参数解耦

一般在组件内使用路由参数，大多数人会这样做：

```js
export default {
    methods: {
        getParamsId() {
            return this.$route.params.id
        }
    }
}
```

在组件中使用 `$route `会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的 URL 上使用，限制了其灵活性。

```js
在路由文件中配置  props: true
const router = new VueRouter({
    routes: [{
        path: '/user/:id',
        component: User,
        props: true
    }]
})
```

将路由的 props 属性设置为 true 后，组件内可通过 props 接收到 params 参数

```js
export default {
    props: ['id'],
    methods: {
        getParamsId() {
            return this.id
        }
    }
}
```

## 8.css样式穿透

在开发中修改第三方组件样式是很常见，但由于 scoped 属性的样式隔离，可能需要去除 scoped 或是另起一个 style 。这些做法都会带来副作用（组件样式污染、不够优雅），样式穿透在css预处理器中使用才生效。
我们可以使用 >>> 或 /deep/ 解决这一问题:

```js
<style scoped>
外层 >>> .el-checkbox {
  display: block;
  font-size: 26px;

  .el-checkbox__label {
    font-size: 16px;
  }
}
</style>

例如
<style scoped>
.nav >>> .el-checkbox {
  display: block;
  font-size: 26px;

  .el-checkbox__label {
    font-size: 16px;
  }
}
</style>

<style scoped>
/deep/ .el-checkbox {
  display: block;
  font-size: 26px;

  .el-checkbox__label {
    font-size: 16px;
  }
}
</style>

```

## 9.事件参数$event

`$event` 是事件对象的特殊变量，在一些场景能给我们实现复杂功能提供更多可用的参数

```js
<template>
    <div>
        <input type="text" @input="inputHandler('hello', $event)" />
    </div>
</template>

export default {
    methods: {
        inputHandler(msg, e) {
            console.log(e.target.value)
        }
    }
}
```