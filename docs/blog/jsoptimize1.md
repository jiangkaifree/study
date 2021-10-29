## 1. 单行 `if else`语句

常见的 `if else` 语句

```javascript
cosnt flag = true
if(flag){
    console.log(`${flag} is true`)
}else{
    console.log(`${flag} is false`)
}
```
使用 三元运算符 更简洁明了

```javascript
cosnt flag = true
flag ？console.log（“True”）：console.log（“False”）
```

## 2. 合并数组

使用扩展运算符（...）将一个数组的元素扩展为另一个数组

```javascript
    const list = [10, 20, 30, 40];
    const newList = [...list, 50, 60, 70, 80];
    consele.log(newList)    // 打印 [10, 20, 30, 40, 50, 60, 70, 80]
```

## 3. 数组删除重复项

```javascript
    cosnt list = [1,1,2,3,4,4,5]
    const newList = [...new Set(list)]
    console.log(newList)    // 打印 [1,2,3,4,5]
```

## 4. 转化 `boolean`值

   除了true和false之外，JavaScript还将其他类型视为真或假。

   * 0，""，null，undefined，NaN，和false总是假 。
   * 其他一切都是真的。

   使用一元运算符 （!）取反。

   ```javascript
       console.log(!undefined)    // true
       console.log(!null)         // true
       console.log(!NaN)          // true
       console.log(!'')           // true
       console.log(!0)            // true
       console.log(!false)        // true
       console.log(!true)        // flase
       console.log(!'0')        // false
   ```
## 5. 不使用第三个变量交换两个变量的值

   ```javascript
       let x = 1;
       let y = 2;
       [x, y] = [y, x];
       console.log(x, y);      // 2,1

   ```

## 6. `Number` 转化 `String`

   ```javascript
   const num = 1 +“”;
   console.log（typeof num）;      // string
   console.log（num）;       // 1

   ```

## 7.字符串转换为数字

  ```javascript

 const numStr = "1";
const num = +numStr;
console.log(typeof num);     // number
console.log(num);            // 1
  ```

## 8.使用模版字符串

```
   原来的写法
   const age = 14;
   const info = 'I'm' + age + 'years old';
   console.log(info)       //  打印 I'm 14 years old

   优化写法
const info = `I'm ${age} years old`;
console.log(info);       // 打印 I'm 14 years old
```

## 9.使用扩展运算符（...）将字符串拆分为数组

   ```
const str = "string"
console.log(str)       // 打印 ['s','t','r','i','n','g']
   ```

## 10. 使用 `ES2020`可选链

   假设你有一个data对象 代码如下

   ```
   const data = {
       info: {
           age: 11,
       }
   }
   ```

   在我们获取 `age` 之前我们要 判断 `info` 是否存在。如果 `info` 不存在 我们代码汇发生报错。因为你无法读取 `undefined` 属性。于是我们就有了一些的代码：

   ```
   // 原本代码
   const data = {info：{age：1}}
if（data && data.info）{
      console.log（data.info.age）;      // 11
}

   // 优化代码
   const value = data?.info?.age;
   console.log(value)     // 11

   ```
   tips： 由于是最新的 `es` 提案标准，目前很多浏览器均未支持可选链 `(?.)` 操作符。但是我们可以通过安装 `balel` 来配置使用。

## 11.多个`if`条件的判断

   ```javascript
   // 原来代码
   if (x === 'abc' || x === 'def' || x === 'ghi' || x ==='jkl') {
    // do something
    }

    // 优化代码
    if (['abc', 'def', 'ghi', 'jkl'].includes(x)) {
       // do something
    }
   ```

## 12. 同时声明多个变量

   ```javascript
   // 原来代码
    let test1;
    let test2 = 1;

    // 优化代码
    let test1, test2 = 1;

    // 声明且赋值 原来代码
    let a = 1
    let b = 2

    //优化代码 使用解构赋值
    let [a,b] = [1,2]
   ```

## 13.空值合并操作符

   ```javascript

   const foo = null ?? 'default string';
   console.log(foo);     // 打印 default string

   const value = undefined ?? 'string'
   console.log(value).   //打印 string

   const age = 0 ?? 12
   console.log(age)    // 打印 0

   ```

## 14.`true`状态下调用函数

   ```javascript
   // 原来代码
   if (test1) {
      callMethod();
   }

   // 优化代码
  test1 && callMethod();

   ```

## 15. 简短的函数调用语句

   ```javascript
   function test1() {
       console.log('test1');
   };

    function test2() {
        console.log('test2');
    };

    var test3 = 1;

    if (test3 == 1) {
      test1();
    } else {
      test2();
    }

    // 优化代码
    test3 === 1? test1():test2();
    or
    (test3 === 1? test1:test2)();
   ```