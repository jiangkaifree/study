## 数组遍历

随着 JS 的不断发展，截至 ES7 规范已经有十多种遍历方法。下面按照功能类似的方法为一组，来介绍数组的常用遍历方法。

1. `for` `forEach` `for...of` `map`

```js
const list = [1, 2, 3, 4, 5, 6, 7, 8,, 10, 11];

for (let i = 0, len = list.length; i < len; i++) {
  if (list[i] === 5) {
    break; // 1 2 3 4
    // continue; // 1 2 3 4 6 7 8 undefined 10 11
  }
  console.log(list[i]);
}

for (const item of list) {
  if (item === 5) {
    break; // 1 2 3 4
    // continue; // 1 2 3 4 6 7 8 undefined 10 11
  }
  console.log(item);
}


list.forEach((item, index, arr) => {
  if (item === 5) return;
  console.log(index); // 0 1 2 3 5 6 7 9 10
  console.log(item); // 1 2 3 4 6 7 8 9 10
});
```

小结
* `forEach` 无法跳出循环，`for` 和 `for ...of` 可以使用 break 或者 continue 跳过或中断。 map 不能使用break 会报错。
* `for ...of` 直接访问的是实际元素，`for` 遍历数组索引，`forEach` 回调函数参数更丰富，元素、索引、原数组都可以获取。

2. `some`、`every`

```js
const list = [
  { name: 'a', age: 16 },
  { name: 'b', age: 24 },
  { name: 'c', age: 25 },
];

const someAge = list.some(item => item.age >= 18) // true

const everyAge = list.every(item => item.age >= 18) // false
```

小结
* 二者都是用来做数组条件判断的，都是返回一个布尔值。
* 二者都可以被中断。
* `some` 若某一元素满足条件，返回 `true`，循环中断。所有元素不满足条件，返回 `false`
* `every` 与 `some` 相反，若有一元素不满足条件，返回 `false`，循环中断。所有元素满足条件，返回 `true`。

3. `filter`、`map`

```js
const list = [
  { name: 'a', age: 16 },
  { name: 'b', age: 24 },
  { name: 'c', age: 25 },
];

const resultList = list.filter(item => {
  console.log(item);
  return item.type >=  18;
});
console.log(resultList)
// [ { name: 'b', age: 24 },
  { name: 'c', age: 25 },]

console.log(list.map((item) => {
    if(item.age >= 18) return item
 }))

 // [undefined, { name: 'b', age: 24 },
  { name: 'c', age: 25 },]

```

小结
* 二者都是生成一个新数组，都不会改变原数组
* 二者都会跳过空元素。
* `map` 会将回调函数的返回值组成一个新数组，数组长度与原数组一致。`filter` 会将符合回调函数条件的元素组成一个新数组。
* `map` 生成的新数组元素可自定义。`filter` 生成的新数组元素不可自定义，与对应原数组元素一致。

4. `findIndex`、`find`

```js
const list = [
  { name: 'a', age: 16 },
  { name: 'b', age: 24 },
  { name: 'c', age: 25 },
];

const result = list.find(item => item.age === 16);
// { name: 'a', age: 16 }

const result = list.findIndex(item => item.age === 16);
// 0
```
小结

* 二者都是用来查找数组元素。
* `find` 方法返回数组中满足 `callback` 函数的第一个元素的值。如果不存在返回 `undefined`
* `findIndex` 它返回数组中找到的元素的索引，而不是其值，如果不存在返回 -1

5. `reduce`、`reduceRight`

```js
const list = [
  { name: 'a', age: 16 },
  { name: 'b', age: 24 },
  { name: 'c', age: 25 },
];
const total = list.reduce((currentTotal, item) => {
  return currentTotal + item.age;
}, 0);
//  65
```

小结

* `reduceRight` 方法除了与 `reduce` 执行方向相反外(从右往左)，其他完全与其一致。

## 对象的遍历

6. `for in`

```js
const obj = { name: 'kobe', age: 16 }
Object.prototype.paly = () => {};
for (const i in obj) {
  console.log(i, ':', obj[i]);
}
// name: kobe
// age: 16
// paly: () => {}
```
小结
* 使用 `for in` 循环时，返回的是所有能够通过对象访问的、可枚举的属性，既包括存在于实例中的属性，也包括存在于原型中的实例。

7. Object.keys

```js
const obj = { name: 'kobe', age: 16 },
Object.prototype.paly = () => {};
console.log(Object.keys(obj))
// ['name','age']

// 也可以数组
const arr = ['a', 'b'];
console.log(Object.keys(arr));
// ['0', '1']
```
小结
* 用于获取对象自身所有的可枚举的属性值，但**不包括原型中的属性**，然后返回一个由属性名组成的数组。

8. `Object.values`

```js
const obj = { name: 'kobe', age: 16 }
Object.prototype.paly = () => {};
console.log(Object.values(obj))
// ['kobe','16']

// 也可以数组
const arr = ['a', 'b'];
console.log(Object.values(arr));
// ['a', 'b']
```

小结
* 用于获取对象自身所有的可枚举的属性值，**但不包括原型中的属性**，然后返回一个由属性值组成的数组。

9.`Object.entries`

```js
const obj = { name: 'kobe', age: 16 }
Object.prototype.paly = () => {};
console.log(Object.entries(obj)) // [['name','kobe'],['age',16]]

// 也可以数组
const arr = ['a', 'b'];
console.log(Object.entries(arr));
// [['0','a'], ['1','b']]
```
小结
* 用于获取对象自身所有的可枚举的属性值，但不包括原型中的属性，然后返回二维数组。每一个子数组由对象的属性名、属性值组成。可以同时拿到属性名与属性值的方法。

> 注意⚠️: 它们都是无序的并不能保证拿到的顺序是一一对应的。

