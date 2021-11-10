# css居中

今天突然想到面试的时候时常问到 `css` 上下左右居中的问题。 我一下子没有了头绪，然后总结了一下， 下面是同样的 `html` 结构：

```html
<div class="box">
    <div class="children"></div>
</div>
```

## 1. 知道元素的大小

### 
```css
.box {
    width: 200px;
    height: 200px;
    border: 1px solid red;
    position: relative;
}
.children {
    position: absolute;
    width: 100px;
    height: 100px;
    background: yellow;
    left: 50%;
    top: 50%;
    margin-left: -50px;
    margin-top: -50px; 
}
```

```css
.box {
    width: 200px;
    height: 200px;
    border: 1px solid red;
    position: relative;
}
.children {
    position: absolute;
    width: 100px;
    height: 100px;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: yellow;
    margin: auto;
}
```

## 2. 不知道元素大小

```css
.box {
    width: 200px;
    height: 200px;
    border: 1px solid red;
    position: relative;
}
.children {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    background: yellow;
}
```

```css
.box {
    width: 200px;
    height: 200px;
    border: 1px solid red;
    display: flex;
    justify-content: center;
    align-items: center;
}
.children {
    background: yellow;
}
```

## 3. 其他一些方式

这些方式只是大概了解，实际中并不会大多数的运用，你可以说出来不用说出细节的代码内容。

使用 `table-cell`
```css
.box {
  width: 200px;
  height: 200px;
  border: 1px solid red;
  display: table-cell;
  text-align: center;
  vertical-align: middle;
}
.children {
  background: yellow;
}
```

使用 `grid`
```css
 .box {
  width: 200px;
  height: 200px;
  border: 1px solid red;
  display: grid;
}
.children {
  background: yellow;
  margin: auto;
}
```

不知道这种回答方式是否算上是一种合格的方式。