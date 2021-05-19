## DD每周前端七题详解-第三期

## 系列介绍

你盼世界，我盼望你无`bug`。Hello 大家好！我是霖呆呆！

呆呆每周都会分享七道前端题给大家，系列名称就是「DD每周七题」。

系列的形式主要是：`3道JavaScript` + `2道HTML` + `2道CSS`，帮助我们大家一起巩固前端基础。

所有题目也都会整合至 [LinDaiDai/niubility-coding-js](https://github.com/LinDaiDai/niubility-coding-js/issues) 的`issues`中，欢迎大家提供更好的解题思路，谢谢大家😁。

一起来看看本周的七道题吧。



## 正题

### 一、使用delete删除数组元素，其长度会改变吗？

(题目来源：[https://github.com/haizlin/fe-interview/issues/2462](https://github.com/haizlin/fe-interview/issues/2462))

咱来写个案例🌰看看就知道了：

```javascript
var arr = [1, 2, 3]
delete arr[1]
console.log(arr)
console.log(arr.length)
```

结果如下：

![](https://user-gold-cdn.xitu.io/2020/6/7/1728f5bf677c341f?w=468&h=266&f=jpeg&s=19210)

通过结果，我们可以得出结论：**使用`delete`删除数组元素，其长度是不会改变的。**

关于这一点，大家可以把数组理解为是一个特殊的对象，其中的每一项转换为对象的伪代码为：

```javascript
key: value
// 对应:
0: 1
1: 2
2: 3
length: 3
```

所以我们使用`delete`操作符删除一个数组元素时，相当于移除了数组中的一个属性，被删除的元素已经不再属于该数组。但是这种改变并不会影响数组的`length`属性。

**扩展：**

- 如果你想让一个数组元素继续存在但是其值是 `undefined`，那么可以使用将 `undefined` 赋值给这个元素而不是使用 `delete`。例如：`arr[1] = undefined`
- 如果你想通过改变数组的内容来移除一个数组元素，请使用[`splice()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) 方法。例如：`arr.splice(1, 1)`

[https://github.com/LinDaiDai/niubility-coding-js/issues/15](https://github.com/LinDaiDai/niubility-coding-js/issues/15)



### 二、创建一个函数batches返回能烹饪出面包的最大值

(题目来源：[https://github.com/30-seconds/30-seconds-of-interviews](https://github.com/30-seconds/30-seconds-of-interviews))

```javascript
/**
batches函数接收两个参数：
1. recipe 制作一个面包需要的各个材料的值
2. available 现有的各个材料的值
要求传入 recipe 和 available，然后根据两者计算出能够烹饪出面包的最大值
**/

// 0个面包，因为 butter黄油需要50ml，但是现在只有48ml
batches(
  { milk: 100, butter: 50, flour: 5 },
  { milk: 132, butter: 48, flour: 51 }
)
batches(
  { milk: 100, butter: 4, flour: 10 },
  { milk: 1288, butter: 9, flour: 95 }
)

// 1个面包
batches(
  { milk: 100, butter: 50, flour: 10 },
  { milk: 198, butter: 52, flour: 10 }
)

// 2个面包
batches(
  { milk: 2, butter: 40, flour: 20 },
  { milk: 5, butter: 120, flour: 500 }
)
```

这道题的解题思路其实就是比较`recipe`和`available`两个对象的每一个属性值，用后者的属性值除以前者的属性值，然后得到一个数，例如0个面包中的：

- `available.milk / recipe.milk`，得到`1.32`；
- `available.butter / recipe.butter`，得到`0.96`
- `available.flour / recipe.flour`，得到`10.2`

然后取三个结果中的最小值`0.96`，再向下取整，得出最终能制作的面包个数为`0`。

所以我们可以得出第一种解题方法：

```javascript
const batches = (recipe, available) =>
  Math.floor(
    Math.min(...Object.keys(recipe).map(k => available[k] / recipe[k] || 0))
  )
```

过程分析：

- `Object.keys(recipe)`，迭代`recipe`对象，得到所有的`key`：`['milk', 'butter', 'flour']`
- 之后使用`map`遍历刚刚所有的`key`，并返回`available[k]/recipe[k]`的值：`[1.32, 0.96, 10.2]`
- 需要得出上面一步数组中的最小值，所以可以使用`Math.min(...arr)`方法来获取
- 最后将最小值`0.96`向下取整得到`0`。

当然这道题你也可以使用`Object.entries()`，效果是一样的：

```javascript
const batches = (recipe, available) =>
  Math.floor(
    // Math.min(...Object.keys(recipe).map(k => available[k] / recipe[k] || 0))
    Math.min(...Object.entries(recipe).map(([k, v]) => available[k] / v || 0))
  )
```

[https://github.com/LinDaiDai/niubility-coding-js/issues/16](https://github.com/LinDaiDai/niubility-coding-js/issues/16)



### 三、typeof typeof 0 的结果？

(题目来源：[https://github.com/30-seconds/30-seconds-of-interviews](https://github.com/30-seconds/30-seconds-of-interviews))

这道题其实不难，最终结果是`"string"`。

过程分析：

- `typeof 0`的结果为`"number"`，是个字符串
- 所以`typeof "number"`的结果是`"string"`

[https://github.com/LinDaiDai/niubility-coding-js/issues/17](https://github.com/LinDaiDai/niubility-coding-js/issues/17)



### 四、target="_blank"有哪些问题？

**存在问题：**

1. 安全隐患：新打开的窗口可以通过`window.opener`获取到来源页面的`window`对象即使跨域也可以。某些属性的访问被拦截，是因为跨域安全策略的限制。 但是，比如修改`window.opener.location`的值，指向另外一个地址，这样新窗口有可能会把原来的网页地址改了并进行页面伪装来欺骗用户。
2. 新打开的窗口与原页面窗口共用一个进程，若是新页面有性能不好的代码也会影响原页面

**解决方案：**

1. 尽量不用`target="_blank"`

2. 如果一定要用，需要加上`rel="noopener"`或者`rel="noreferrer"`。这样新窗口的`window.openner`就是`null`了，而且会让新窗口运行在独立的进程里，不会拖累原来页面的进程。(不过，有些浏览器对性能做了优化，即使不加这个属性，新窗口也会在独立进程打开。不过为了安全考虑，还是加上吧。)

（参考来源：[慎用target="_blank"](https://juejin.im/post/5eb8ed20e51d454db55fb353)）

[https://github.com/LinDaiDai/niubility-coding-js/issues/18](https://github.com/LinDaiDai/niubility-coding-js/issues/18)



### 五、children以及childNodes的区别

- `children`只获取该节点下的所有`element`节点
- `childNodes`不仅仅获取`element`节点还会获取元素标签中的空白节点
- `firstElementChild`只获取该节点下的第一个`element`节点
- `firstChild`会获取空白节点

[https://github.com/LinDaiDai/niubility-coding-js/issues/19](https://github.com/LinDaiDai/niubility-coding-js/issues/19)



### 六、float:left对比position:absolute

相同点：

- 脱离文档流，也就是将元素从普通的布局排版中拿走，其他盒子在定位的时候，会当做脱离文档流的元素不存在而进行定位。
- 包裹性：也就是都会让元素`inline-block`化。等同于没有高度与宽度的`inline-block`元素。
  - 对于块状元素默认的宽度为`100%`，若设置为了绝对定位则宽度由内容决定
  - 对于内联元素原本设置`width`属性无效，若设置了浮动则可以设置`width`值
- 破坏性：都会导致父级高度塌陷。但若是设置了绝对定位的话，其父级即使设置为`float:left;`也还是不能解决高度塌陷的问题。

不同点：

- 虽然它们都会脱离文档流，但是使用`float`脱离文档流时，其他盒子会无视这个元素，但其他盒子内的文本依然会为这个元素让出位置，环绕在周围。而对于使用`position:absolute`脱离文档流的元素，其他盒子与其他盒子内的文本都会无视它。

[https://github.com/LinDaiDai/niubility-coding-js/issues/20](https://github.com/LinDaiDai/niubility-coding-js/issues/20)



### 七、float:left对比inline-block

- **文档流**：浮动会脱离文档流，且使得被覆盖的元素的文字内容会环绕在周围；而`inline-block`不会脱离文档流也就不会覆盖其它元素。浮动也会引发父级高度塌陷问题。
- **水平位置**：不能给有浮动元素的父级设置`text-align:center`使得子集浮动元素居中，而`inline-block`却可以。
- **垂直对齐**：`inline-block`元素沿着默认的基线对齐(`baseline`)，若是两个元素的`font-size`不同则可能会看到一高一低，你可以通过设置`vertical-align: top或者bottom;`来使得它们基于顶线或者底线对齐(注意这个是设置到元素本身而不是设置到它们的父级)。而浮动元素紧贴顶部，不会有这个问题。
- **空白**：`inline-block`包含`html`空白节点。如果你的`html`中一系列元素每个元素之间都换行了，当你对这些元素设置`inline-block`时，这些元素之间就会出现空白。而浮动元素会忽略空白节点，互相紧贴。

针对第三点，垂直对齐可以看下面👇这个案例：

默认情况下：

*css代码为：*

```css
.sub {
  background: hotpink;
  display: inline-block;
}
```

![](https://user-gold-cdn.xitu.io/2020/6/7/1728f5c2724a713f?w=302&h=192&f=jpeg&s=15299)

设置了`vertial-align: top;`后：

*css代码为：*

```css
.sub {
  background: hotpink;
  display: inline-block;
  vertical-align: top;
}
```

![](https://user-gold-cdn.xitu.io/2020/6/7/1728f5c467fb3e50?w=296&h=186&f=jpeg&s=13657)

[https://github.com/LinDaiDai/niubility-coding-js/issues/21](https://github.com/LinDaiDai/niubility-coding-js/issues/21)



## 后语

你盼世界，我盼望你无`bug`。这篇文章就介绍到这里。

您每周也许会花`48`小时的时间在工作💻上，会花`49`小时的时间在睡觉😴上，也许还可以再花`20`分钟的时间在呆呆的7道题上，日积月累，我相信我们都能见证彼此的成长😊。

什么？你问我为什么系列的名字叫`DD`？因为`呆呆`呀，哈哈😄。

喜欢**霖呆呆**的小伙还希望可以关注霖呆呆的公众号 `LinDaiDai` 或者扫一扫下面的二维码👇👇👇。

![](https://user-gold-cdn.xitu.io/2020/5/27/17254d5d0a277620?w=900&h=500&f=gif&s=1632550)

我会不定时的更新一些前端方面的知识内容以及自己的原创文章🎉

你的鼓励就是我持续创作的主要动力 😊。