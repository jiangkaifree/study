## 16. 利用箭头函数隐式返回

```js
// 原来代码
function getAge(age) {
  return 2 * age
}

//优化代码
const getAge = diameter => (
  2 * age
)
```

## 17.函数默认参数

```js
//原本写法
function add(test1, test2) {
  if (test1 === undefined)
    test1 = 1;
  if (test2 === undefined)
    test2 = 2;
  return test1 + test2;
}
//优化写法
add = (test1 = 1, test2 = 2) => (test1 + test2);
add() //output: 3
```

## 18.对象相同键值对的赋值

```js
// 原来代码
const name = 'kay';
const age = 12;
const info = {
    name: name,
    age: age
}

// 优化代码
const [age,name] = [12,'kay']
const info = {
    name,
    age
}
```

## 19.解构赋值缩写法

```js
// 原来写法
const test1 = this.data.test1;
const test2 = this.data.test2;
const test2 = this.data.test3;
//优化写法
const { test1, test2, test3 } = this.data;
```

## 20. 对象转化数组

```js
const data = { test1: 'abc', test2: 'cde', test3: 'efg' };
const arr = Object.entries(data);
console.log(arr);
/** 打印:
[ [ 'test1', 'abc' ],
  [ 'test2', 'cde' ],
  [ 'test3', 'efg' ]
]
```

## 21.`Object.values()`和 `Object.keys()`

```js
const data = { test1: 'abc', test2: 'cde' };
const val = Object.values(data);
const key = Object.keys(data)
console.log(val)     // ['abc','cde']
console.log(key)     // ['test1','test1']
```

## 22.找出一个数组中最大和最小的值

```js
const arr = [1, 2, 3];
Math.max(…arr); // 3
Math.min(…arr); // 1
```

## 23. 重复一个字符串多次

```js
let test = '';
for(let i = 0; i < 5; i ++) {
  test += 'test ';
}
console.log(str); // test test test test test
//shorthand
'test '.repeat(5);
```

## 24.类数组转化数组

```js
let arrayLike = {
    0: 'tom',
    1: '65',
    2: '男',
    3: ['jane','john','Mary'],
    'length': 4
}

let arr = Array.from(arrayLike)
console.log(arr) // ['tom','65','男',['jane','john','Mary']]
```

## 25. switch 对应的缩写法

```js
 原来写法
switch (data) {
  case 1:
    test1();
  break;

  case 2:
    test2();
  break;

  case 3:
    test();
  break;
  // And so on...
}

优化写法
var data = {
  1: test1,
  2: test2,
  3: test
};

data[something] && data[something]();

或者or

const data = new Map([
  [1,()=>{
    console.log('test1')
}],
  [2, ()=>{
      console.log('test2')
}]
]);
console.log(data.get(1)())
```

后续继续总结更新，感谢关注。
