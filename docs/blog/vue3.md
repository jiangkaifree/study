## 1.建立data

### vue2

vue2我们就不说了 ，data中放入两个属性 `username` `password`。

```
export default {
  props: {
    title: String
  },
  data () {
    return {
      username: '',
      password: ''
    }
  }
}

```

### vue3

在Vue3.0，我们就需要使用一个新的setup()方法，此方法在组件初始化构造的时候触发。
为了可以让开发者对反应型数据有更多的控制，我们可以直接使用到 Vue3 的反应API（reactivity API）

**使用以下三步来建立反应性数据:**

1. 从`vue`引入`reactive`

2. 使用`reactive()`方法来声明我们的数据为反应性数据
3. 使用`setup()`方法来返回我们的反应性数据，从而我们的template可以获取这些反应性数据
上一波代码，让大家更容易理解是怎么实现的。

```js
import { reactive } from 'vue'

export default {
  props: {
    title: String
  },
  setup () {
    const state = reactive({
      username: '',
      password: ''
    })

    return { state }  //记得return
  }
}
```

## 2.`methods` 的编写

### vue2

vue2中我们直接放到`methods`这个方法里面

```js
export default {
  props: {
    title: String
  },
  data () {
    return {
      username: '',
      password: ''
    }
  },
  methods: {
    login () {
      // 登陆方法
    }
  }
}
```

### vue3

vue3中`setup()`方法也是可以用来操控methods的。创建声明方法其实和声明数据状态是一样的。 我们需要先声明一个方法然后在setup()方法中返回(return)， 这样我们的组件内就可以调用这个方法了。

```
export default {
  props: {
    title: String
  },
  setup () {
    const state = reactive({
      username: '',
      password: ''
    })

    const login = () => {
      // 登陆方法
    }
    return {
      login,
      state
    }
  }
}
```

## 3.生命周期函数

在 Vue2，我们可以直接在组件属性中调用Vue的生命周期的钩子。以下使用一个组件已挂载`mounted` `befortMounted`生命周期触发钩子。

```js
export default {
  props: {
    title: String
  },
  data () {
    return {
      username: '',
      password: ''
    }
  },
  created(){},
  mounted () {
    console.log('组件已挂载')
  },
  methods: {
    login () {
      // login method
    }
  }
}
```

### vue3

首先`setup()`中自带 `beforeCreated` `created` 也就是说 我们不用在想vue2中一样去`created`中去调用获取数据了。但是在 Vue3 生周期钩子不是全局可调用的了，需要另外从vue中引入。和刚刚引入`reactive`一样，生命周期的挂载钩子叫`onMounted`。
引入后我们就可以在`setup()`方法里面使用`onMounted`挂载的钩子了

```js
import { reactive, onMounted } from 'vue'

export default {
  props: {
    title: String
  },
  setup () {
    // ..
    const getData = ()=> {
     $axios() //获取数据
    }

    onMounted(() => {
      console.log('组件已挂载')
    })

    // ...
    return {
     getData
    }
  }
}
```

## 4. 计算属性 - `Computed`

### vue2

在 Vue2 中实现，我们只需要在组件内的选项属性中添加即可

```js
export default {
  // ..
  computed: {
    lowerCaseUsername () {
      return this.username.toLowerCase()
    }
  }
}
```

### vue3

Vue3 的设计模式给予开发者们按需引入需要使用的依赖包。这样一来就不需要多余的引用导致性能或者打包后太大的问题。Vue2就是有这个一直存在的问题。
所以在 Vue3 使用计算属性，我们先需要在组件内引入`computed`。

```js
import { reactive, onMounted, computed } from 'vue'
//引入 computed
export default {
  props: {
    title: String
  },
  setup () {
    const state = reactive({
      username: '',
      password: '',
      lowerCaseUsername: computed(() => state.username.toLowerCase())
    })

    // ...
  }
  ```

## 4.接受props

### vue2

在 Vue2，`this`代表的是当前组件，不是某一个特定的属性。所以我们可以直接使用`this`访问`prop`属性值。就比如下面的例子在挂载完成后打印出当前传入组件的参数`title`。

```js
export default {
  props: {
    title: String
  },
  data () {
    return {
      username: '',
      password: ''
    }
  },
  created(){},
  mounted () {
    console.log('组件已挂载')
  },
  methods: {
    login () {
      // login method
     console.log( this.props.title )  //打印title
    }
  }
}
```

### vue3

vue3中取消了this。不过全新的`setup()`方法可以接收两个参数：

1. `props` 组件参数
2. `context` vue3 暴露属性 比如  `emit` `attrs`
所以 vue3中接受`props`

```js
setup (props) {
    // ...

    onMounted(() => {
      console.log('title: ' + props.title)
    })

    // ...
}
```

## 5.emit

### vue2中

 Vue2 中我们会调用到`this.$emit`然后传入事件名和参数对象。

```js
login () {
  this.$emit('login', {   //触发父组件方法
    username: this.username,
    password: this.password
  })
}
```

### vue3

在`setup()`中的第二个参数`context`对象中就有`emit`，这个是和`this.$emit`是一样的。那么我们只要在`setup()`接收第二个参数中使用分解对象法取出emit就可以在`setup`方法中随意使用了。

```js
setup (props, { emit }) {
    // ... 暴露属性 emit
  //相当于 this.$emit = emit
    const login = () => {
      emit('login', {
        username: state.username,
        password: state.password
      })
    }
    // ...
}
```

## 总结一下

总结一下，我觉得 Vue3 给我们前端开发者带来了全新的开发体验，更好的使用弹性，可控度也得到了大大的提升。如果你是一个学过或者接触过 **React** 然后现在想使用**Vue**的话，应该特别兴奋，因为很多使用方式都和**React hooks**非常相近了 🎉,**vue3** 借鉴了很多**react hooks**。**vue3**中不再提供 **this**  。对于 **this** 而言 ，相对于初学者来说是固定的 **this** 指向当前这个实例，我需要啥就去**this**中拿， 容易理解使用。而**vue3** 更自定义 ，我需要啥就要先引入或者先暴露啥，不再是像**vue2**中一样 `this` 中都给你包含了。这也就使得 `vue3` 的代码体积来的要小。
