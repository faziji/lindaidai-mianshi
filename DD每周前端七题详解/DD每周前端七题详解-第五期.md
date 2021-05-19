## DD每周前端七题详解-第五期

## 系列介绍

你盼世界，我盼望你无`bug`。Hello 大家好！我是霖呆呆！

呆呆每周都会分享七道前端题给大家，系列名称就是「DD每周七题」。

系列的形式主要是：`3道JavaScript` + `2道HTML` + `2道CSS`，帮助我们大家一起巩固前端基础。

所有题目也都会整合至 [LinDaiDai/niubility-coding-js](https://github.com/LinDaiDai/niubility-coding-js/issues) 的`issues`中，欢迎大家提供更好的解题思路，谢谢大家😁。

一起来看看本周的七道题吧。



## 正题

### 一、实现mask函数将"123456"转为"##3456",只保留最后四个字符

(题目来源：[https://github.com/30-seconds/30-seconds-of-interviews](https://github.com/30-seconds/30-seconds-of-interviews))

首先介绍一下题目的意思吧😄，案例🌰如下：

```javascript
const mask = (str, maskChar = '#') => {
	// 代码
}
console.log(mask('123456')); // '##3456'
console.log(mask('lindaidai')); // '#####idai'
```

这道题的难度应该没有那么大，处理方式也有很多。呆呆这边主要是讲解一下如何使用`padStart`来实现的。

简单介绍一下`padStart`方法吧，它是`ES8`新增的实例函数，与它作用差不多的还有一个叫`padEnd`的函数：

- `String.prototype.padStart`
- `String.prototype.padEnd`

**作用**：允许将空字符串或其他字符串添加到原始字符串的开头或结尾。

**语法**：

```javascript
padStart(targetLength, [padString])
```

- `targetLength`: 必填，当前字符串需要填充到的目标长度。如果这个数值小于当前字符串的长度，则返回当前字符串本身。
- `padString`: 可选，填充字符串。如果字符串太长，使填充后的字符串长度超过了目标长度，则只保留最左侧的部分，其他部分会被截断，此参数的缺省值为`" "`。

例如：

```javascript
'x'.padStart(4, 'ab') // 'abax'
'x'.padEnd(5, 'ab') // 'xabab'
```

哈哈，另外想要了解如何实现一个`padStart`的小伙伴可以看呆呆之前一篇文章哟：[DD每周前端七题详解-第二期](https://juejin.im/post/5ed732dfe51d457879333878#heading-3)

**言归正传**，让我们回到这道题目哈，首先让我们来处理一下输入参数边界的情况，例如输入的`str`不存在或者长度小于`4`的时候：

```javascript
const mask = (str, maskChar = '#') => {
  if (!str || str.length <= 4) return str;
}
```

其次，我们可以只保留住`str`的末尾四个字符，然后使用`padStart`将这四个字符填充至`str.length`即可，如下：

```javascript
const mask = (str, maskChar = '#') => {
  if (!str || str.length <= 4) return str;
  return str.slice(-4).padStart(str.length, maskChar);
}
```

- `slice`不会影响原本的字符串
- 使用`padStart`填充即可

[https://github.com/LinDaiDai/niubility-coding-js/issues/30](https://github.com/LinDaiDai/niubility-coding-js/issues/30)



### 二、介绍一下NaN并实现一个isNaN

**介绍一下NaN**：

- `NaN`属性是代表非数字值的特殊值，该属性用于指示某个值不是数字；
- `NaN`是不等于`NaN`的，即`NaN === NaN`的结果是`false`；
- 使用`Object.is()`来比较两个`NaN`结果是`true`，即`Object.is(NaN, NaN)`的结果是`true`；
- `typeof NaN`为`"number"`；
- 方法`parseInt()`和`parseFloat()`在不能解析指定的字符串时就返回这个值；
- 可以使用`isNaN`来判断一个变量是不是`NaN`，它是`JS`内置对象`Number`上的静态方法。

(关于第三点，大家可以看一下我之前的一篇文章哟，里面的「第二补：JS类型检测-`Object.is()和===的区别`」有提到：[读《三元-JS灵魂之问》总结,给自己的一份原生JS补给(上)](https://juejin.im/post/5e8dc6fd6fb9a03c97753dea#heading-9))

**实现一个isNaN**:

对于`isNaN`的`polyfill`实现起来就比较简单了，只需要利用`NaN`不等于它自身的这一点即可：

```javascript
const isNaN = v => v !== v;
```

[https://github.com/LinDaiDai/niubility-coding-js/issues/31](https://github.com/LinDaiDai/niubility-coding-js/issues/31)



### 三、按位取反，为什么`~2 = -3`?

接下来，分享一道与`JavaScript`原生无关的题目吧，主要也是看到群里有小伙伴问了关于按位取反`~`的用法，这边统一科普一下，😁。

正常一个数字，例如`1`和`2`，或者`-1`和`-2`。

如果我们对它们进行按位取反的话，结果会是这样：

- `~1 = -2`
- `~2 = -3`
- `~-1 = 0`
- `~-2 = 1`

看不懂没关系，让我们来一步步看看实现的过程哈。

在这里其实是分了**正数和负数**的，因为符号不同取反的过程也会不同。



#### 1.1 正数按位取反

先让我们来看看正数的按位取反。

比如先看看`~1 = -2`，过程如下：

**1. 十进制转为二进制原码**

首先将十进制的`1`转化为二进制原码为：`0000 0001`



**2. 二进制原码按位取反**

之后将原码按位取反：

也就是将`0000 0001` => `1111 1110`

 (取反应该知道啥意思吧？就是`0`换成`1`，`1`换成`0`)



**3. 取反后的二进制转为原码**

再将取反后的二进制码转为原码二进制：

也就是将`1111 1110` => `1000 0010`

这里你估计看着都点懵了，当我们将**取反后的二进制**转为**原码二进制**的时候，其实是有以下两步的：

1. 需要判断取反后的二进制的第一个位是不是`1`，这个第一位我们称之为**符号位**，如果是`1`的话就表示即将要转成的数是一个负数，如果是`0`的话表示即将要转的数是一个正数，这个符号位是不能动的；在这里我们可以看到`1111 1110`的第一位是`1`，所以表示即将要转的数是一个负数，同时我们不动它。
2. 然后将除了第一位以外其它位数取反并`+1`。所以会有这么两个过程：
   - `1111 1110` => `1000 0001`
   - `1000 0001` => `1000 0010` (这步是对上一步的结果`+1`，因为上一步的最后一个数是`1`，所以它再加上`1`就需要向前进一位了，因此变成了`1000 0010`)



**4. 将原码二进制转为十进制**

最后一步就是将我们前面得到的`1000 0010`这个二进制转化为十进制了。

第一位符号位，是`1`，则表示是个负数，所以结果为`-2`。



OK👌，搞懂了这个步骤之后再让我们自己来转换一下`~2 = -3`吧：

```
1. 0000 0010
2. 1111 1101
3. 1000 0011
4. -3
```



**正数按位取反总结**

1. 十进制转为二进制原码
2. 二进制原码按位取反
3. 符号位保留，其余位取反+1
4. 二进制原码转为十进制



#### 1.2 负数按位取反

负数的按位取反和上面就有些不一样了，主要是第二步和第三步调换一下顺序：

1. 十进制转为二进制原码
2. 符号位保留，其余位取反+1
3. 二进制原码按位取反
4. 二进制原码转为十进制



例如：`~-1 =0 `的转换过程：

**1. 十进制转为二进制原码**

这步和正数按位取反是一样的：

`-1` => `1000 0001`



**2. 符号位保留，其余位取反+1**

转换过程：

- `1000 0001` => `1111 1110` (取反)
- `1111 1110` => `1111 1111` (取反后 + 1)



**3. 二进制原码按位取反**

将刚刚得到的再进行按位取反：

`1111 1111` => `0000 0000`



**4. 二进制原码转为十进制**

`0000 0000` => `0`



OK👌，现在自己来转换一下`~-2 = 1`吧：

```
1. 1000 0010
2. 1111 1110
3. 0000 0001
4. 1
```

这里没啥诀窍，关键就是要记住转换的过程然后不断的练习吧 😂。

另外关于`~~`的用法还可以看呆呆的另一篇文章哟[《JS中按位取反运算符~及其它运算符》](https://www.jianshu.com/p/3c0f56f3190a)

[https://github.com/LinDaiDai/niubility-coding-js/issues/32](https://github.com/LinDaiDai/niubility-coding-js/issues/32)



### 四、知道`insertAdjacentHTML`方法吗？

这个方法是呆呆最近在看公司项目代码时了解到的，之前一直没有注意它。

首先对于它的用法：

> **`insertAdjacentHTML()`** 方法将指定的文本解析为 [`Element`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element) 元素，并将结果节点插入到DOM树中的指定位置。它不会重新解析它正在使用的元素，因此它不会破坏元素内的现有元素。这避免了额外的序列化步骤，使其比直接使用innerHTML操作更快。

其次它的作用对象是一个元素`element`，例如`const container = document.getElementById('container')`

语法上呢：

```
element.insertAdjacentHTML(position, text);
```

- `position`：一个`DOMString`，也就是表示插入内容相对元素的位置，且必须是下面的字符串之一：
  - `'beforebegin'`：元素自身的前面。
  - `'afterbegin'`：插入元素内部的第一个子节点之前。
  - `'beforeend'`：插入元素内部的最后一个子节点之后。
  - `'afterend'`：元素自身的后面。
- `text`：是要被解析为HTML或XML元素，并插入到DOM树中的 `DOMString`。

**案例**：

让我们来看看它的用法，例如🌰现在有一个`HTML`的结构为：

```html
<div id="one">我是one</div>
```

`JavaScript`代码中加上这段话：

```javascript
const one = document.getElementById('one');
one.insertAdjacentHTML('afterend', '<div id="two">我是two</div>');
```

现在最终的渲染结果就变成了这样：

```html
<div id="one">我是one</div><div id="two">我是two</div>
```

**工作上的用法**：

在项目中，主要可以应用于这样的场景：一个空的容器(你可以理解为一个`div`)，开始需要一个`loading`的效果，在数据加载完毕之后，需要把`loading`取掉且清空容器内的元素并以其它方式重新渲染出容器的内容。

这里呆呆就以定时器来模拟一下数据加载的过程，实现代码如下：

```html
<body>
	<div id="container"></div>
</body>
<script>
	const container = document.getElementById('container');
  const loading = '<div id="loading">loading</div>'; // loading可能是一个组件
  container.insertAdjacentHTML('beforeend', loading);
  setTimeout(() => {
    container.innerHTML = ''
  }, 2000)
</script>
```

(当然，我们不要为了刻意用而去用，适合自己的才是最好的)

**安全问题**：

- 使用 `insertAdjacentHTML` 插入用户输入的HTML内容的时候，需要转义之后才能使用。

  例如：

  ```javascript
  const one = document.getElementById('one');
  // 由于 encodeURI('<div id="two">我是two</div>')会被转译为：
  // %3Cdiv%20id=%22two%22%3E%E6%88%91%E6%98%AFtwo%3C/div%3E
  // 因此最终会被当成 "%3Cdiv%20id=%22two%22%3E%E6%88%91%E6%98%AFtwo%3C/div%3E"字符串渲染
  one.insertAdjacentHTML('afterend', encodeURI('<div id="two">我是two</div>'));
  ```

- 如果只是为了插入文本内容（而不是HTML节点），不建议使用这个方法，建议使用[`node.textContent`](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/textContent) 或者 [`node.insertAdjacentText()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/insertAdjacentText)。因为这样不需要经过HTML解释器的转换，性能会好一点。(这里是引用的[MDN-insertAdjacentHTML](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/insertAdjacentHTML)上的内容)

[https://github.com/LinDaiDai/niubility-coding-js/issues/33](https://github.com/LinDaiDai/niubility-coding-js/issues/33)



### 五、`insertAdjacentHTML`和`insertAdjacentElement`的区别

第二个参数的类型不同， 前者接收的是`是要被解析为HTML或XML元素的字符串`，而后者接收的是一个`element`元素。

```javascript
const one = document.getElementById('one');
one.insertAdjacentHTML('afterend', '<div id="two">我是two</div>');

const one = document.getElementById('one');
const two = document.createElement('div')
two.innerHTML = '我是two';
one.insertAdjacentElement('afterend', two);
```

[https://github.com/LinDaiDai/niubility-coding-js/issues/34](https://github.com/LinDaiDai/niubility-coding-js/issues/34)





### 六、实现九宫格布局

实现效果如下：

![](https://user-gold-cdn.xitu.io/2020/6/24/172e3902d22427a3?w=162&h=160&f=png&s=7460)

先来看一下`HTML`方面的代码：

```html
<div id="container">
    <div class="item item-1">1</div>
    <div class="item item-2">2</div>
    <div class="item item-3">3</div>
    <div class="item item-4">4</div>
    <div class="item item-5">5</div>
    <div class="item item-6">6</div>
    <div class="item item-7">7</div>
    <div class="item item-8">8</div>
    <div class="item item-9">9</div>
</div>
```

还有一些`item`上的基础`css`代码：

```css
#container {
    /* css代码 */
}

.item {
    font-size: 2em;
    text-align: center;
    border: 1px solid #e5e4e9;
}

.item-1 {
    background-color: #ef342a;
}

.item-2 {
    background-color: #f68f26;
}

.item-3 {
    background-color: #4ba946;
}

.item-4 {
    background-color: #0376c2;
}

.item-5 {
    background-color: #c077af;
}

.item-6 {
    background-color: #f8d29d;
}

.item-7 {
    background-color: #b5a87f;
}

.item-8 {
    background-color: #d0e4a9;
}

.item-9 {
    background-color: #4dc7ec;
}
```

**方案一**：

第一种方案可以使用浮动+百分比：

```css
#container {
    width: 150px;
    height: 150px;
}
.item {
    float: left;
    width: 33.33%;
    height: 33.33%;
    box-sizing: border-box;
    font-size: 2em;
    text-align: center;
    border: 1px solid #e5e4e9;
}
```

**方案二**：

还可以使用`flex`布局的方式：

```css
#container {
    width: 150px;
    height: 150px;
    display: flex;
    flex-wrap: wrap;
}
.item {
    width: 33.33%;
    height: 33.33%;
    box-sizing: border-box;
    font-size: 2em;
    text-align: center;
    border: 1px solid #e5e4e9;
}
```

**方案三**：

另外的话，也许还可以试试`grid`？

```css
#container {
    display: grid;
    grid-template-columns: 50px 50px 50px;
    grid-template-rows: 50px 50px 50px;
}
.item {
    font-size: 2em;
    text-align: center;
    border: 1px solid #e5e4e9;
}
```

答案参考：[https://github.com/haizlin/fe-interview/issues/2270](https://github.com/haizlin/fe-interview/issues/2270)

[https://github.com/LinDaiDai/niubility-coding-js/issues/35](https://github.com/LinDaiDai/niubility-coding-js/issues/35)



### 七、说说will-change

`will-change`是`CSS3`新增的标准属性，它的作用很单纯，就是`"增强页面渲染性能"`，当我们在通过某些行为触发页面进行大面积绘制的时候，浏览器往往是没有准备，只能被动的使用CPU去计算和重绘，由于事先没有准备，对于一些复杂的渲染可能会出现掉帧、卡顿等情况。

而`will-change`则是在真正的行为触发之前告诉浏览器可能要进行重绘了，相当于浏览器把CPU拉上了，能从容的面对接下来的变形。

常用的语法主要有：

- `whil-change: scroll-position;` 即将开始滚动
- `will-change: contents;` 内容要动画或者变化了
- `will-transform;` transform相关的属性要变化了(常用)

注意：

- `will-change`虽然可以开启加速，但是一定要适度使用
- 开启加速的代价为手机的耗电量会增加
- 使用时遵循最小化影响原则，可以对伪元素开启加速，独立渲染
- 可以写在伪类中，例如`hover`中，这样移出元素的时候就会自动`remove`掉`will-change`了
- 如果使用`JS`添加了`will-change`，注意要及时`remove`掉，方式就是`style.willChange = 'auto'`

[https://github.com/LinDaiDai/niubility-coding-js/issues/36](https://github.com/LinDaiDai/niubility-coding-js/issues/36)



## 参考文章

知识无价，支持原创。

参考文章：

- [《浪里行舟-盘点ES7、ES8、ES9、ES10新特性》](https://juejin.im/post/5dda2b5e6fb9a07a83691766#heading-7)
- [《MDN-isNaN》](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/isNaN)
- [《使用CSS3 will-change提高页面滚动、动画等渲染性能》](https://www.zhangxinxu.com/wordpress/2015/11/css3-will-change-improve-paint/)



## 后语

你盼世界，我盼望你无`bug`。这篇文章就介绍到这里。

您每周也许会花`48`小时的时间在工作💻上，会花`49`小时的时间在睡觉😴上，也许还可以再花`20`分钟的时间在呆呆的7道题上，日积月累，我相信我们都能见证彼此的成长😊。

什么？你问我为什么系列的名字叫`DD`？因为`呆呆`呀，哈哈😄。

喜欢**霖呆呆**的小伙还希望可以关注霖呆呆的公众号 `LinDaiDai` 或者扫一扫下面的二维码👇👇👇。

[![img](https://user-gold-cdn.xitu.io/2020/6/24/172e396f92646b65?w=900&h=500&f=gif&s=411532)

我会不定时的更新一些前端方面的知识内容以及自己的原创文章🎉

你的鼓励就是我持续创作的主要动力 😊。