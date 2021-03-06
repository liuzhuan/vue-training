# Vue 实例

## 创建

Vue 系应用须以 `Vue` 函数开启：

```js
var vm = new Vue({
  // options
})
```

创建 Vue 实例后，可传入 options 选项。所有选项参数[见此][api]。

Vue 系应用包含一个 **root Vue** 实例，以 `new Vue` 创建。以及一些嵌套的，可复用的组件。比如，以下是一个 todo 应用的组件树示例：

```
Root Instance
+-- TodoList
  +-- TodoItem
  | +-- DeleteTodoButton
  | +-- EditTodoButton
  +-- TodoListFooter
    +-- ClearTodosButton
    +-- TodoListStatistics
```

Vue 组件亦为 Vue 实例，两者选项参数大部相同，仅有些许选项只可为 root Vue 实例可用。

## 数据和方法

创建 Vue 实例后，`data` 选项的所有属性均自动加入 Vue 的**响应式系统**。当这些数值变化时，对应的视图会随之刷新。

```js
var data = { a: 1 }

// The object is added to a Vue instance
var vm = new Vue({
  data: data
})

// These reference the same object!
vm.a === data.a // => true

// Setting the property on the instance
// also affects the original data
vm.a = 2
data.a // => 2

// ... and vice-versa
data.a = 3
vm.a // => 3
```

数据变化，视图重新渲染。注意，只有实例创建时 `data` 的属性是响应式的。如果你在后期增加属性：

```js
vm.b = 'hi'
```

`b` 的改变无法触发任何视图的渲染。如果后期需要某些参数，可以设定初始值：

```js
data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
```

当对象被 `Object.freeze()` 冻结后，响应式系统会失灵。比如：

```js
var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data() {
    return {
      obj
    }
  }
})
```

```html
<div id="app">
  <p>{{ obj.foo }}</p>
  <!-- 点击按钮并不会导致视图刷新 -->
  <button @click="obj.foo = 'baz'">Change it</buton>
</div>
```

除 `data` 属性外，Vue 实例还暴露一些其他的属性和方法。它们以 `$` 开头，方便与用户定义的数据作区分。比如：

```js
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

vm.$watch('a', function(newValue, oldValue) {
  // 当 a 属性变化时被调用。
})
```

## 实例生命周期钩子

每个 Vue 实例创建时都会经历一系列初始化步骤，比如设定数据观察，编译模板，把实例加载到 DOM，数据变化时更新 DOM 等。在这些过程中，Vue 会在特定时间节点执行一些函数，称之为“生命周期钩子函数”（`lifecycle hooks`），用户有机会在其中加入自定义代码。

比如，`created` 钩子可在实例创建后调用：

```js
new Vue({
  data: {
    a: 1
  },
  created: function() {
    // this 指向新创建的 vm 实例
    console.log('a is: ' + this.a)
  }
})
```

其他阶段还有其他的钩子函数可供使用，比如 `mounted`, `updated`, `destroyed` 等。所有的钩子函数的 `this` 都指向新创建的 Vue 实例。

⚠️ 注意：**不要使用箭头函数作为钩子的回调函数**，比如：`created: () => console.log(this.a)` 或 `vm.$watch('a', newValue => this.myMethod())`。因为箭头函数绑定在父级上下文，`this` 不会指向期望的 Vue 实例，经常导致如下错误：`Uncaught TypeError: Cannot read property of undefined` 或 `Uncaught TypeError: this.myMethod is not a function`。

## 生命周期流程图

![lifecycle diagram](https://vuejs.org/images/lifecycle.png?_sw-precache=6f2c97f045ba988851b02056c01c8d62)

## REF

- [The Vue Instance][instance]
- [Options - Data][api]

[instance]: https://vuejs.org/v2/guide/instance.html
[api]: https://vuejs.org/v2/api/#Options-Data