## 一，必要性的

官方原话：这些规则会帮你规避错误，所以学习并接受它们带来的全部代价吧。这里面可能存在例外，但应该非常少，且只有你同时精通 JavaScript 和 Vue 才可以这样做。**精通**  嘿嘿嘿！那么问题来了。怎样算精通？

### 1. 组件名应该始终是多个单词的，根组件 `App` 以及 `<transition>`、`<component>` 之类的 `Vue` 内置组件除外。

举例
```javascript
// 反例
app.component('todo', {
  // ...
})

export default {
  name: 'Todo',
  // ...
}
```

```javascript
// 正确
app.component('todo-item', {
  // ...
})

export default {
    name: 'TodoItem',
    ...
}

```

## 2. `props`的定义

举例

```javascript
// 反例
// 简单写传入的属性
props: ['status']

```

```javascript
// 正确
props: {
  status: String
}
or

// 更好的例子
props: {
  status: {
    type: String,   // 加入类型判断
    required: true,  //加入是否必须字段

    //加入验证规则
    validator: value => {
      return [
        'syncing',
        'synced',
        'version-conflict',
        'error'
      ].includes(value)
    }
  }
}

```

### 3.为 `v-for` 设置 `key` 值
永远为 `v-for` 加上 `key`

举例

```javascript
// 错误
<ul>
  <li v-for="todo in todos">
    {{ todo.text }}
  </li>
</ul>

```

```javascript
// 正确
<ul>
  <li
    v-for="todo in todos"
    :key="todo.id"
  >
    {{ todo.text }}
  </li>
</ul>

```

### 4.避免 `v-if` 和 `v-for` 出现在同一个元素上

举例
```javascript
// 反例
<ul>
  <li
    v-for="user in users"
    v-if="user.isActive"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

或者

<ul>
  <li
    v-for="user in users"
    v-if="shouldShowUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

```

```javascript
// 正确
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>
```

### 5.为组件样式设置作用域

举例
```javascript
//错误
<template>
  <button class="btn btn-close">×</button>
</template>

<style>
.btn-close {
  background-color: red;
}
</style>

```

```javascript
//正确
<template>
  <button class="button button-close">×</button>
</template>

<!-- 使用 `scoped` attribute -->
<style scoped>
.button {
  border: none;
  border-radius: 2px;
}

.button-close {
  background-color: red;
}
</style>

或者

<template>
  <button :class="[$style.button, $style.buttonClose]">×</button>
</template>

<!-- 使用 CSS modules -->
<style module>
.button {
  border: none;
  border-radius: 2px;
}

.buttonClose {
  background-color: red;
}
</style>

或者

<template>
  <button class="c-Button c-Button--close">×</button>
</template>

<!-- 使用 BEM 约定 -->
<style>
.c-Button {
  border: none;
  border-radius: 2px;
}

.c-Button--close {
  background-color: red;
}
</style>

```

### 6.私有 `property` 名称

避免改写 `Vue` 实例上的属性及方法

举例
```javascript
//反例
const myGreatMixin = {
  // ...
  methods: {
    update() {
      // ...
    }
  }
}


或者

const myGreatMixin = {
  // ...
  methods: {
    _update() {
      // ...
    }
  }
}

或者

const myGreatMixin = {
  // ...
  methods: {
    $update() {
      // ...
    }
  }
}

或者

const myGreatMixin = {
  // ...
  methods: {
    $_update() {
      // ...
    }
  }
}

```

```javascript
// 推荐
const myGreatMixin = {
  // ...
  methods: {
    $_myGreatMixin_update() {
      // ...
    }
  }
}


//更好的例子
const myGreatMixin = {
  // ...
  methods: {
    publicMethod() {
      // ...
      myPrivateFunction()
    }
  }
}

function myPrivateFunction() {
  // ...
}

export default myGreatMixin

```

## 二, 优先级 B 的规则：强烈推荐 (增强代码可读性)

### 7.组件文件

只要有能够拼接文件的构建系统，就把每个组件单独分成文件。避免使用 `component`去实例化组件

举例
```javascript
// 反例
app.component('TodoList', {
  // ...
})

或者

app.component('TodoItem', {
  // ...
})
```

```javascript
// 推荐
components/
|- TodoList.js
|- TodoItem.js

或者

components/
|- TodoList.vue
|- TodoItem.vue

```

### 8.单文件组件文件的大小写

单文件组件的文件名应该要么始终是单词大写开头 (PascalCase)，要么始终是横线连接。

举例
```javascript
//反例
components/
|- mycomponent.vue

或者

components/
|- myCmponent.vue
```

```javascript
//推荐
components/
|- MyComponent.vue

或者

components/
|- my-component.vue
```

### 9.基础组件名称

应用特定样式和约定的基础组件 (也就是展示类的、无逻辑的或无状态的组件) 应该全部以一个特定的前缀开头，比如 `Base`,`App`或 `V`

举例
```javascript
//反例
components/
|- MyButton.vue
|- VueTable.vue
|- Icon.vue
```

```javascript
//推荐
components/
|- BaseButton.vue
|- BaseTable.vue
|- BaseIcon.vue

或者
components/
|- AppButton.vue
|- AppTable.vue
|- AppIcon.vue

或者

components/
|- VButton.vue
|- VTable.vue
|- VIcon.vue
```

### 10.单组件名称

只应该拥有单个活跃实例的组件应该以 `The` 前缀命名，以示其唯一性。

特点：1.一个页面只出现一次 2.不接受`props`

```javascript
//反例
components/
|- Heading.vue
|- MySidebar.vue
```

```javascript
//推荐
components/
|- TheHeading.vue
|- TheSidebar.vue
```

### 11. 紧密耦合的组件名称

如果某个父组件基于某个子组件进行扩展的，那么他应当这样进行命名。

举例
```javascript
//反例
components/
|- TodoList.vue
|- TodoItem.vue
|- TodoButton.vue


components/
|- SearchSidebar.vue
|- NavigationForSearchSidebar.vue

```

```javascript
//推荐
// 我们应当在文件名上体现出来 他们都是基于同一个Todo.vue来扩展的
components/
|- TodoList.vue
|- TodoListItem.vue
|- TodoListItemButton.vue


components/
|- SearchSidebar.vue
|- SearchSidebarNavigation.vue

```
### 12.组件名称中的单词顺序
组件名称应该以高阶的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾。

```javascript
// 反例
components/
|- ClearSearchButton.vue
|- ExcludeFromSearchInput.vue
|- LaunchOnStartupCheckbox.vue
|- RunSearchButton.vue
|- SearchInput.vue
|- TermsCheckbox.vue

```

```javascript
components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputQuery.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxTerms.vue
|- SettingsCheckboxLaunchOnStartup.vue

```

### 13.自闭合组件

在单文件组件、字符串模板和 JSX 中没有内容的组件应该是自闭合的——但在 DOM 模板里永远不要这样做。

注意：自闭合组件表示它们不仅没有内容，而且刻意没有内容。

举例
```javascript
// 反例
<!-- 在单文件组件、字符串模板和 JSX 中 -->
<MyComponent></MyComponent>

<!-- 在 DOM 模板中                   -->
<my-component/>
```

```javascript
//推荐
<!-- 在单文件组件、字符串模板和 JSX 中 -->
<MyComponent/>

//我的理解应该是指html文件中，这里我也不太明白，知道的可以指出来什么是dom模版
<!-- 在 DOM 模板中                   -->
<my-component></my-component>
```

### 14.模板中的组件名称大小写
对于绝大多数项目来说，在单文件组件和字符串模板中组件名称应该总是 PascalCase（小驼峰） 的——但是在 DOM 模板中总是 kebab-case （短横线）的。

举例
```html
//反例
<!-- 在单文件组件和字符串模板中 -->
<mycomponent/>

<!-- 在单文件组件和字符串模板中 -->
<myComponent/>

<!-- 在 DOM 模板中            -->
<MyComponent></MyComponent>
```
```javascript
//推荐
<!-- 在单文件组件和字符串模板中 -->
<MyComponent/>

<!-- 在 DOM 模板中            -->
<my-component></my-component>

<!-- 在所有地方 -->
<my-component></my-component>

```

### 15.`JS/JSX` 中使用的组件名称

举例
```javascript
// 反例
app.component('myComponent', {
  // ...
})

import myComponent from './MyComponent.vue'

export default {
  name: 'myComponent',
  // ...
}

export default {
  name: 'my-component',
  // ...
}

```

```javascript
//推荐
app.component('MyComponent', {
  // ...
})

app.component('my-component', {
  // ...
})

import MyComponent from './MyComponent.vue'

export default {
  name: 'MyComponent',
  // ...
}
```

### 16.完整单词的组件名称

组件名称应该倾向于完整单词而不是缩写。

举例：

```javascript
反例
components/
|- SdSettings.vue
|- UProfOpts.vue
```

```javascript
// 正确写法
components/
|- StudentDashboardSettings.vue
|- UserProfileOptions.vue
```

### 17.Prop 名称

在声明 prop 的时候，其命名应该始终使用 camelCase，而在模板和 JSX 中应该始终使用 kebab-case。

举例

```javascript
反例

props: {
  'greeting-text': String
}

//模版中
<WelcomeMessage greetingText="hi"/>
```


```javascript
// 正确
props: {
  greetingText: String
}

// 模版中
<WelcomeMessage greeting-text="hi"/>
```

### 18.多个 `attribute` 的元素

多个 `attribute` 的元素应该分多行撰写，每个 `attribute` 一行。

举例

```javascript
// 反例
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">

<MyComponent foo="a" bar="b" baz="c"/>
```

```javascript
// 正确

<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>

<MyComponent
  foo="a"
  bar="b"
  baz="c"
/>
```

### 19.模板中的简单表达式

组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。

```javascript
// 反例
{{
  fullName.split(' ').map((word) => {
    return word[0].toUpperCase() + word.slice(1)
  }).join(' ')
}}

```

```javascript
// 正确
<!-- 在模板中 -->
{{ normalizedFullName }}

// 复杂表达式已经移入一个计算属性
computed: {
  normalizedFullName() {
    return this.fullName.split(' ')
      .map(word => word[0].toUpperCase() + word.slice(1))
      .join(' ')
  }
}
```

### 20.简单的计算属性

应该把复杂计算属性分割为尽可能多的更简单的 property。即 单个计算属性只有单一功能。

```javascript
// 反例
computed: {
  price() {
    const basePrice = this.manufactureCost / (1 - this.profitMargin)
    return (
      basePrice -
      basePrice * (this.discountPercent || 0)
    )
  }
}

```

```javascript

computed: {
  basePrice() {
    return this.manufactureCost / (1 - this.profitMargin)
  },

  discount() {
    return this.basePrice * (this.discountPercent || 0)
  },

  finalPrice() {
    return this.basePrice - this.discount
  }
}
```

### 21.带引号的 `attribute` 值

非空 HTML attribute 值应该始终带引号 (单引号或双引号，选你 JS 里不用的那个)。

在 HTML 中不带空格的 attribute 值是可以没有引号的，但这鼓励了大家在特征值里不写空格，导致可读性变差。

举例：

```javascript
// 反例
<input type=text>

<AppSidebar :style={width:sidebarWidth+'px'}>

```

```javascript
// 正确

<input type="text">

<AppSidebar :style="{ width: sidebarWidth + 'px' }">
```

### 22.指令缩写

指令缩写 (用 : 表示 `v-bind`:，`@` 表示 `v-on`: 和用 `#` 表示 `v-slot`) 应该要么都用要么都不用。

举例
```javascript
// 反例

<input
  v-bind:value="newTodoText"
  :placeholder="newTodoInstructions"
>

<input
  v-on:input="onInput"
  @focus="onFocus"
>

// 不统一风格
<template v-slot:header>
  <h1>Here might be a page title</h1>
</template>

<template #footer>
  <p>Here's some contact info</p>
</template>
```

```javascript
// 正确

<input
  :value="newTodoText"
  :placeholder="newTodoInstructions"
>

<input
  v-bind:value="newTodoText"
  v-bind:placeholder="newTodoInstructions"
>

<input
  @input="onInput"
  @focus="onFocus"
>

<input
  v-on:input="onInput"
  v-on:focus="onFocus"
>

// 统一风格
<template v-slot:header>
  <h1>Here might be a page title</h1>
</template>

<template v-slot:footer>
  <p>Here's some contact info</p>
</template>

<template #header>
  <h1>Here might be a page title</h1>
</template>

<template #footer>
  <p>Here's some contact info</p>
</template>
```
# 三，推荐使用

## 23.组件/实例的选项顺序

这是我们推荐的组件选项默认顺序。它们被划分为几大类，所以你也能知道从插件里添加的新 property 应该放到哪里。属性出现的顺序
如下

1.  `name`  组件以外的知识。

2. `components`,`directives` 模版依赖（模版内使用的资源）

3. `extends`,`mixins`,`provide/inject`。组合 (向选项里合并 property)

4. `inheritAttrs`,`props`,`emits` 。接口（组件的接口）

5. `setup`.组合式 API (使用组合式 API 的入口点)

6.  `data`,`computed`。 local State (原生响应式 property)

7. `watch` 事件 (通过响应式事件触发的回调)。`beforeCreate`,`created`,`beforeMount`,`mounted`,`beforeUpdate`,`updated`,`activated`,`deactivated`,`beforeUnmount`,`unmounted`,`errorCaptured`,`renderTracked`,`renderTriggered`。生命周期钩子 (按照它们被调用的顺序)

8. `methods` 非响应式的 property (不依赖响应性系统的实例 property)

9. `template/render` 渲染 (组件输出的声明式描述)

## 24.元素 attribute 的顺序

元素 (包括组件) 的 attribute 应该有统一的顺序。

1. 定义 (提供组件的选项)

      * `is`

2. 列表渲染

    * `v-for`

3. 条件渲染 (元素是否渲染/显示)

    * `v-if`
    * `v-else-if`
    * `v-else`
    * `v-show`
    * `v-cloak`
4. 渲染修饰符 (改变元素的渲染方式)

    * `v-pre`
    * `v-once`

5. 全局感知 (需要超越组件的知识)

    * `id`

6. 唯一的 Attributes (需要唯一值的 attribute)

    * `ref`
    * `key`

7. 双向绑定 (把绑定和事件结合起来)
    * `v-model`

8.其他 Attributes (所有普通的绑定或未绑定的 attribute)

9. 事件 (组件事件监听器)
    * `v-on`

10. 内容 (覆写元素的内容)
    * `v-html`
    * `v-text`

## 24. 组件/实例选项中的空行

你可能想在多个 property 之间增加一个空行，特别是在这些选项一屏放不下，需要滚动才能都看到的时候。

举例：
```javascript
// 反例
props: {
  value: {
    type: String,
    required: true
  },
  focused: {
    type: Boolean,
    default: false
  },
  label: String,
  icon: String
},
computed: {
  formattedValue() {
    // ...
  },
  inputClasses() {
    // ...
  }
}
```

```javascript
// 推荐

props: {
  value: {
    type: String,
    required: true
  },

  focused: {
    type: Boolean,
    default: false
  },

  label: String,
  icon: String
},

computed: {
  formattedValue() {
    // ...
  },

  inputClasses() {
    // ...
  }
}

```

## 25. 单文件组件的顶级元素的顺序

单文件组件应该总是让 `<script>、<template>` 和 `<style>` 标签的顺序保持一致。且 `<style>` 要放在最后，因为另外两个标签至少要有一个。

举例：

```javascript
// 反例
<style>/* ... */</style>
<script>/* ... */</script>
<template>...</template>

<!-- ComponentA.vue -->
<script>/* ... */</script>
<template>...</template>
<style>/* ... */</style>

<!-- ComponentB.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>
```

```javascript
// 推荐

<!-- ComponentA.vue -->
<script>/* ... */</script>
<template>...</template>
<style>/* ... */</style>

<!-- ComponentB.vue -->
<script>/* ... */</script>
<template>...</template>
<style>/* ... */</style>

或者

<!-- ComponentA.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>

<!-- ComponentB.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>

```

# 四，谨慎使用

## 26. `scoped` 中的元素选择器

元素选择器应该避免在 scoped 中出现。

在 scoped 样式中，类选择器比元素选择器更好，因为大量使用元素选择器是很慢的。

举例

```javascript
//  反例
<template>
  <button>×</button>
</template>

<style scoped>
button {
  background-color: red;
}
</style>
```

```javascript
// 推荐
<template>
  <button class="btn btn-close">×</button>
</template>

<style scoped>
.btn-close {
  background-color: red;
}
</style>
```

## 27. 隐性的父子组件通信

应该优先通过 `prop` 和事件进行父子组件之间的通信，而不是 `this.$parent` 或变更 `prop`。

一个理想的 `Vue` 应用是 `prop` 向下传递，事件向上传递的。遵循这一约定会让你的组件更易于理解。然而，在一些边界情况下 `prop` 的变更或 `this.$parent` 能够简化两个深度耦合的组件。

**问题在于，这种做法在很多简单的场景下可能会更方便。但请当心，不要为了一时方便 (少写代码) 而牺牲数据流向的简洁性 (易于理解)。**

举例

```javascript
// 反例
app.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },

  template: '<input v-model="todo.text">'
})

app.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },

  methods: {
    removeTodo() {
      this.$parent.todos = this.$parent.todos.filter(todo => todo.id !== vm.todo.id)
    }
  },

  template: `
    <span>
      {{ todo.text }}
      <button @click="removeTodo">
        ×
      </button>
    </span>
  `
})
```

```javascript
app.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },

  template: `
    <input
      :value="todo.text"
      @input="$emit('input', $event.target.value)"
    >
  `
})

app.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },

  template: `
    <span>
      {{ todo.text }}
      <button @click="$emit('delete')">
        ×
      </button>
    </span>
  `
})
```

## 27. 非 `Flux` 的全局状态管理

应该优先通过 `Vuex` 管理全局状态，而不是通过 `this.$root` 或一个全局事件总线。