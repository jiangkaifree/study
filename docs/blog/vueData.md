## 1. Vue 无法检测实例被创建时不存在于 data 中的 property

**原因：**  由于 Vue 会在初始化实例时对 `property` 执行 `getter/setter` 转化，所以 `property`必须在 `data` 对象上存在才能让 Vue 将它转换为响应式的。       **简单的说就是没有在data中进行一个定义**

### 错误

```
var vm = new Vue({
  data:{},
  // 页面不会变化
  template: '<div>{{message}}</div>'
})
vm.message = 'Hello!' // `vm.message` 不是响应式的
```

### 正确解决

```
var vm = new Vue({
  data: {
    // 声明 a、b 为一个空值字符串
    message: '',
  },
  template: '<div>{{ message }}</div>'
})
vm.message = 'Hello!'
```

## 2.Vue 无法检测对象 property 的添加或移除

**原因：** 由于 JavaScript（ES5） 的限制，Vue.js 不能检测到对象属性的添加或删除。因为 Vue.js 在初始化实例时将属性转为 getter/setter，所以属性必须在 data 对象上才能让 Vue.js 转换它，才能让它是响应的。
**简单地说就是vue无法捕捉到对象，已经数组督办函对象当中更深层次属性的变化**

### 错误

```
var vm = new Vue({
  data:{
    obj: {
      id: 001
    }
  },
  // 页面不会变化
  template: '<div>{{ obj.message }}</div>'
})

vm.obj.message = 'hello' // 不是响应式的
delete vm.obj.id       // 不是响应式的
```

### 正确解决

**使用$set方法**

```
// 动态添加 - Vue.set
Vue.set(vm.obj, propertyName, newValue)

// 动态添加 - vm.$set
vm.$set(vm.obj, propertyName, newValue)

// 动态添加多个
// 代替 Object.assign(this.obj, { a: 1, b: 2 })
this.obj = Object.assign({}, this.obj, { a: 1, b: 2 })

// 动态移除 - Vue.delete
Vue.delete(vm.obj, propertyName)

// 动态移除 - vm.$delete
vm.$delete(vm.obj, propertyName)
```

## 3. Vue 不能检测通过数组索引直接修改一个数组项

由于 JavaScript 的限制，Vue 不能检测数组和对象的变化；尤雨溪 - 性能代价和获得用户体验不成正比。

### 错误

```js
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // 不是响应性的
```

### 正确解决

```js
Vue.set(vm.items, indexOfItem, newValue)

// vm.$set
vm.$set(vm.items, indexOfItem, newValue)

// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
```

**tips:**  **Object.defineProperty()**  可以监测数组的变化。但对数组新增一个属性（index）不会监测到数据变化，因为无法监测到新增数组的下标（index），删除一个属性（index）也是。

```js
var arr = [1, 2, 3, 4]
arr.forEach(function(item, index) {
    Object.defineProperty(arr, index, {
    set: function(value) {
      console.log('触发 setter')
      item = value
    },
    get: function() {
      console.log('触发 getter')
      return item
    }
  })
})
arr[1] = '123'  // 触发 setter
arr[1]          // 触发 getter 返回值为 "123"
arr[5] = 5      // 不会触发 setter 和 getter
```

## 4.Vue 不能监测直接修改数组长度的变化

### 错误

```
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items.length = 2 // 不是响应性的
```

### 正确解决

```
vm.items.splice(newLength)
```

**tips**  vue 官方为我们提供了数组的方法 如 ```splice() slice() pop() shift() unshfit() push() pop()```

## 5. 在异步更新执行之前操作 DOM 数据不会变化

Vue 在更新 DOM 时是异步执行的。只要侦听到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个 watcher 被多次触发，只会被推入到队列中一次。这种在缓冲时去除重复数据对于避免不必要的计算和 DOM 操作是非常重要的。然后，在下一个的事件循环“tick”中，Vue 刷新队列并执行实际 (已去重的) 工作。 **简单地说 就是数据更新了，然后又更新的dom 覆盖了前面的数据更新**

### 错误

```
<div id="example">{{message}}</div>
```

```
var vm = new Vue({
  el: '#example',
  data: {
    message: '123'
  }
})
vm.message = 'new message' // 更改数据
vm.$el.textContent === 'new message' // false  操作dom
vm.$el.style.color = 'red' // 页面没有变化
```

### 正确解决 使用$nextTick()在dom更新之后去更新数据

```
var vm = new Vue({
  el: '#example',
  data: {
    message: '123'
  }
})
vm.message = 'new message' // 更改数据
//使用 Vue.nextTick(callback) callback 将在 DOM 更新完成后被调用
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
  vm.$el.style.color = 'red' // 文字颜色变成红色
})
```

## 6. 循环嵌套层级太深，视图不更新？

数据结果太深了 比如

### 错误

```
[
 {
     id: 1,
        children: [
         id: "",
            item: [
             ···甚至更多
            ],
        ]
    }
]
```

### 正确解决

```
vm.$forceUpdate()    //使用 $forceUpdate()
```

## 7. 拓展：路由参数变化时，页面不更新（数据不更新）

路由视图组件引用了相同组件时，当路由参会变化时，会导致该组件无法更新，也就是我们常说中的页面无法更新的问题。

```html
<div id="app">
  <ul>
    <li><router-link to="/home/foo">To Foo</router-link></li>
    <li><router-link to="/home/baz">To Baz</router-link></li>
    <li><router-link to="/home/bar">To Bar</router-link></li>
  </ul>
  <router-view></router-view>
</div>
```

```js
onst Home = {
  template: `<div>{{message}}</div>`,
  data() {
    return {
      message: this.$route.params.name
    }
  }
}

const router = new VueRouter({
  mode:'history',
    routes: [
    {path: '/home', component: Home },
    {path: '/home/:name', component: Home }
  ]
})

new Vue({
  el: '#app',
  router
})
```

上段代码中，我们在路由构建选项 routes 中配置了一个动态路由 `'/home/:name'`，它们共用一个路由组件`Home`，这代表他们复用 `RouterView`。
当进行路由切换时，页面只会渲染第一次路由匹配到的参数，之后再进行路由切换时，`message` 是没有变化的。

### 解决

1. 通过 `watch` 监听 `$route` 的变化。 **推荐使用**

```
 const Home = {
      template: `<div>{{message}}</div>`,
      data() {
        return {
          message: this.$route.params.name
        }
      },
      watch: {
       '$route': function() {
       this.message = this.$route.params.name
        }
        }
    }
    ...
    new Vue({
      el: '#app',
      router
    })
```

2.给`<router-view>` 绑定 `key`属性，这样 `Vue` 就会认为这是不同的 `<router-view>`。

**弊端：** 如果从 /home 跳转到 /user 等其他路由下，我们是不用担心组件更新问题的，所以这个时候 key 属性是多余的。

```js
    <div id="app">
      ...
  <router-view :key="key"></router-view>
    </div>
```
