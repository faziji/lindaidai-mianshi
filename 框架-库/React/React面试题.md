## React面试题

### 组件里如果不用setState修改状态会造成什么后果？

如果试图直接修改状态对象，这会对组件的连贯性和性能造成严重的后果，例如：

- 状态改变不会触发组件重渲染
- 以后无论何时调用 setState，之前修改的状态都会渲染到页面上

