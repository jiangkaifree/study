`Map` 和 `Object` 类似，它们都允许你按键存取一个值、删除键、检测一个键是否绑定了值。但是 `map` 想比较而言，`Map`的`key` 可以是 任何值 (对象或者原始值) 都可以成为一个 `key` 或者 `value`

## 1. `Map` 初始化

```js
1、let map = new Map();
```

## 2. `Map` 的方法属性

```js
map.set('a', 555);		// 插入

// 插入对象key
let keyObj = {};
myMap.set(keyObj, "和键'a string'关联的值");

// 插入函数key
let keyFunc = function() {};
myMap.set(keyFunc, "和键keyFunc关联的值");


map.get(['a'])			// console.log(555) 不存在返回undefined

map.delete('a');		// 删除

map.has('a')			// 判断是否存在 true

map.keys()			// 返回['a',····]

map.values()			// 返回[555,····]

map.size			// 1
```

注意：NaN 也可以作为 Map 对象的键。虽然 NaN 和任何值甚至和自己都不相等(NaN !== NaN 返回 true)，但下面的例子表明，NaN 作为 Map 的键来说是没有区别的:

```js
let map = new Map();
map.set(NaN, "not a number");

map.get(NaN); // "not a number"

let otherNaN = Number("foo");
map.get(otherNaN); // "not a number"
```

只有对对象的引用 `Map` 才认为它们是同一个 `key`。

```js
let map = new Map();
map.set(['a'], 555);
map.get(['a'])		// undefined

const obj = {}
map.set(obj, "和键'obj'关联的值");
map.get(obj)		// '和键'obj'关联的值'
mao.get({}) 		// undefined

cosnt fun = ()=>{}
map.set(fun, "和键'fun'关联的值");
map.get(fun)        // '和键'fun'关联的值'
mao.get(()=>{})         // undefined
```

## 3. `Map` 和对象的区别

运用 `MDN` 的一张图 [原文传送门](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map)

| 区别     | Map | Object|
| -------- | -------------------------------------------- |------------------------------------------------------------- |
| 意外的键 | Map 默认情况不包含任何键。只包含显式插入的键 | 一个 Object 有一个原型, 原型链上的键名有可能和你自己在对象上的设置的键名产生冲突。 |
| 键的类型 | 一个 Map 的键可以是任意值，包括函数、对象或任意基本类型。 | 一个 Object 的键必须是一个 String 或是 Symbol。 |
| 键的顺序 | Map 中的 key 是有序的。因此，当迭代的时候，一个 Map 对象以插入的顺序返回键值。 | 一个 Object 的键是无序的 |
| Size | Map 的键值对个数可以轻易地通过 size 属性获取 | Object 的键值对个数只能手动计算 |
| 迭代 | Map 是 iterable 的，所以可以直接被迭代。 | 迭代一个 Object 需要以某种方式获取它的键然后才能迭代。 |
| 性能 | 在频繁增删键值对的场景下表现更好。 | 在频繁添加和删除键值对的场景下未作出优化。 |
