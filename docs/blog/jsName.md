# 前言
在项目中， 我们大部分时间都是在 **命名**， 给变量 **命名** ，给方法 **命名** 等等。怎样生成 言简意赅的命名呢？ 一个言简意赅的命名，可以让我们很通俗的联想到 这是要做什么（TO DO）的。而且使得我们项目更加的规范化。 下面 记录一下 在 `js` 中的来声明他们， 当然对任何的编程语言也是通用的。

## 一，命名变量

### 1. 在命名你的变量和函数时要使用英语

不管喜欢与否，英语是编程中的主导语言：所有编程语言的语法都是用英语编写的，还有无数的文档和教育材料。通过用英语编写代码，您可以显着提高其凝聚力。

```js

/* Bad */

const primerNombre = 'Gustavo'

const amigos = ['Kate', 'John']


/* Good */

const firstName = 'Gustavo'

const friends = ['Kate', 'John']


```

### 2. 命名规范

选择一种命名约定并遵循它。它可以是 `camelCase`、`PascalCase`、`snake_case` 或其他任何形式，只要它保持一致即可。许多编程语言在命名约定方面都有自己的传统， 我们可以在 `Github` 上面找到有些官方推荐，或者热门的库进行参考。

```js
/* Bad */
const page_count = 5
const shouldUpdate = true

/* Good */
const pageCount = 5
const shouldUpdate = true

/* Good as well */
const page_count = 5
const should_update = true
```

### 3. S-I-D

名称必须简短、直观且具有描述性。

* **`short`**  名称不得花费很长时间，因此请尽量简短。
* **`Intuitive`**  名字必须读起来自然，尽可能接近常用语。
* **`Descriptive`** 名称必须以最有效的方式反映它所做的/拥有的

```js
/* Bad */
const a = 5 // "a" 不具有实际的表达性
const isPaginatable = a > 10 // 听起来不自然
const shouldPaginatize = a > 10 // 使用自己编造的单词

/* Good */
const postCount = 5
const hasPagination = postCount > 10
const shouldPaginate = postCount > 10 // alternatively
```

### 4. 避免缩写

不要使用收缩。它们只会降低代码的可读性。找到一个简短的、描述性的名称可能很难，但收缩并不是不这样做的借口。

```js
/* Bad */
const onItmClk = () => {}

/* Good */
const onItemClick = () => {}
```

### 5. 避免上下文重复

名称不应与定义它的上下文重复。如果这不会降低其可读性，请始终从名称中删除上下文。

```js
class MenuItem {
  /* 存在 上下文 MenuItem 应当删除 */
  handleMenuItemClick = (event) => { ... }

  /* 很好的看作了 `MenuItem.handleClick()` */
  handleClick = (event) => { ... }
}
```

### 6. 反映预期结果

名称应反映预期结果

```js
/* Bad */
const isEnabled = itemCount > 3
return <Button disabled={!isEnabled} />

/* Good */
const isDisabled = itemCount <= 3
return <Button disabled={isDisabled} />
```

## 二， 命名函数

命名函数时可以遵循一个有用的模式。

### 1.  A/HC/LC 模式

```js
prefix? + action (A) + high context (HC) +   low context? (LC)
 前缀      动词/动作        强描述（主要内容）   低描述（辅助内容）
```

参照下表看看

|Name                   | Prefix   | Action (A) | High context (HC) | Low context (LC) |
| ---------------------- | -------- | ---------- | ----------------- | ---------------- |
| `getUser`              |          | `get`      | `User`            |                  |
| `getUserMessages`      |          | `get`      | `User`            | `Messages`       |
| `handleClickOutside`   |          | `handle`   | `Click`           | `Outside`        |
| `shouldDisplayMessage` | `should` | `Display`  | `Message`||

注意： 上下文的顺序会影响一个变量的含义。例如，shouldUpdateComponent意味着你即将更新一个组件，而 `shouldComponentUpdate` 则告诉你该组件将自行更新，而你不过是控制它何时更新。换句话说，高上下文强调了一个变量的意义。

### 2. Action 行动

你的函数名称中的动词部分。最重要的部分，负责描述该函数的作用。

#### 2.1 `get`

立即访问,获取数据（即内部数据的速记 getter）

```js
 function getFruitCount() {
  return this.fruits.length
}
```

#### 2.2 `set`

以声明方式设置变量，将值 `A` 设置为值 `B`

 ```js
 let fruits = 0

function setFruits(nextFruits) {
  fruits = nextFruits
}

setFruits(5)
console.log(fruits) // 5
```

#### 2.3 `reset`

将一个变量设置为其初始值或状态。

```js
const initialFruits = 5
let fruits = initialFruits
setFruits(10)
console.log(fruits) // 10

function resetFruits() {
  fruits = initialFruits
}

resetFruits()
console.log(fruits) // 5
```

#### 2.4 `fetch`

请求一些数据，这需要一些不确定的时间（即异步请求）。 个人觉得使用 `request` 也是可以的。

```js
function fetchPosts(postCount) {
  return fetch('https://api.dev/posts', {...})
}
```

#### 2.5 `remove`

从某处移除某项。

例如，如果你在搜索页面上有一个所选过滤器的集合，从集合中删除其中一个过滤器就是removeFilter，而不是deleteFilter（在英语中自然也是这样说的）。

```js
function removeFilter(filterName, filters) {
  return filters.filter((name) => name !== filterName)
}

const selectedFilters = ['price', 'availability', 'size']
removeFilter('price', selectedFilters)
```

#### 2.6 `delete`

从存在的领域中完全抹去一些东西。

想象一下，你是一个内容编辑，有那个臭名昭著的帖子，你想把它删掉。一旦你点击了一个闪亮的 "删除帖子 "按钮，CMS执行的是 `deletePost` 动作，而不是 `removePost`。

```js
function deletePost(id) {
  return database.find({ id }).delete()
}
```

#### 2.7 `compose`

从现有的数据中创建新的数据。主要适用于字符串、对象或函数。

```js
function composePageUrl(pageName, pageId) {
  return (pageName.toLowerCase() + '-' + pageId)
}
```

#### 2.8 `handle`

处理一个动作。通常在命名回调方法时使用。

```js
function handleLinkClick() {
  console.log('Clicked a link!')
}

link.addEventListener('click', handleLinkClick)
```

### 3. Context 内容

函数操作的上下文

一个函数通常是对某物的操作。重要的是要说明它的可操作域是什么，或者至少是一个预期的数据类型。

```js
/* 一个通用的 过滤操作的函数 */
function filter(list, predicate) {
  return list.filter(predicate)
}

/* 准确地使用在获取最近帖子上操作的方法  */
function getRecentPosts(posts) {
  return filter(posts, (post) => post.date === Date.now())
}
```

### 4. Prefixes 前缀

前缀增强了变量的含义。它很少在函数名中使用。

#### 4.1 `is`

描述了当前环境的一个特征或状态（通常是布尔值）。

```js
const color = 'blue'
const isBlue = color === 'blue' // characteristic
const isPresent = true // state

if (isBlue && isPresent) {
  console.log('Blue is present!')
}
```

#### 4.2 `has`

描述当前上下文是否拥有某个值或状态（通常是布尔值）

```js
/* Bad */
const isProductsExist = productsCount > 0
const areProductsPresent = productsCount > 0

/* Good */
const hasProducts = productsCount > 0
```

#### 4.3 `should`

反映了一个积极的条件语句（通常是布尔型）与某个行动的结合。

```js
function shouldUpdateUrl(url, expectedUrl) {
  return url !== expectedUrl
}
```

#### 4.4 `max/min`

代表一个最小或最大的值。在描述边界或限制时使用。

```js
/**
 * 车险随机数帖子
 * max、min边界
 */
function renderPosts(posts, minPosts, maxPosts) {
  return posts.slice(0, randomBetween(minPosts, maxPosts))
}
```

### 4.5 `prev/next`

表明变量在当前环境中的上一个或下一个状态。在描述状态转换时使用。

```js
function fetchPosts() {
  const prevPosts = this.state.posts

  const fetchedPosts = fetch('...')
  const nextPosts = concat(prevPosts, fetchedPosts)

  this.setState({ posts: nextPosts })
}
```

### 5. 单数和复数 （Singular and Plurals）

像前缀一样，变量名可以做成单数或复数，这取决于它们是持有一个值还是多个值。

```js
/* Bad */
const friends = 'Bob'
const friend = ['Bob', 'Tony', 'Tanya']

/* Good */
const friend = 'Bob'
const friends = ['Bob', 'Tony', 'Tanya']
```

翻译引用至： [Github ](https://github.com/kettanaito/naming-cheatsheet)
