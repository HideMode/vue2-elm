## Directive

### v-html
> 双大括号会将数据解释为纯文本，而非 HTML 。为了输出真正的 HTML ，你需要使用 v-html 指令
```html
<div v-html="rawHtml"></div>
```

### v-if
> 类似于 v-else，v-else-if 必须紧跟在 v-if 或者 v-else-if 元素之后。
```html
<div id="app-3">
  <p v-if="seen">现在你看到我了</p>
</div>
<div v-else-if="ss1">
    else if block
</div>
<div v-else>
    else block
</div>
```

```javascript
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
// 设置seen属性
app3.seen = false;
```

```html
<template v-if="loginType === 'username'">
  <label>Username</label>
  <input placeholder="Enter your username" key="username-input">
</template>
<template v-else>
  <label>Email</label>
  <input placeholder="Enter your email address" key="email-input">
</template>
```
### v-show
> 注意， v-show 不支持 <template> 语法，也不支持 v-else。


### v-for
> 0-based index

```html
<!-- 为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性。理想的 key 值是每项都有的且唯一的 id。这个特殊的属性相当于 Vue 1.x 的 track-by ，但它的工作方式类似于一个属性，所以你需要用 v-bind 来绑定动态值（在这里使用简写）： -->
<ul id="example-2">
  <li v-for="(item, index) in items" :key="item.id">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>>
<!-- 遍历对象 -->
<div v-for="(value, key, index) in object">
  {{ index }}. {{ key }}: {{ value }}
</div>

<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```javascript
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '整个牛项目' }
    ]
  }
})
```


### v-on 绑定事件 => `@click`

```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">逆转消息</button>
</div>
```

```javascript
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```


### v-model 双向数据绑定

```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

```javascript
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: 'Hello Vue!'
  }
})
```

### v-bind 绑定属性 => `:href`
```html
<div v-bind:id="value" v-bind:disabled="avlue"></div>
<div v-bind:id=="'list-' + value">
```


## Component

```javascript

Vew.component('todo-item', {
    // 组件接受的属性，在父类同通过v-bind:prop-name 传入进来
    props: ['todo'],
    template: '<li>{{ todo.text }}</li>'
});

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: '蔬菜' },
      { id: 1, text: '奶酪' },
      { id: 2, text: '随便其他什么人吃的东西' }
    ]
  }
})
```

```html
<div id="app-7">
  <ol>
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id">
    </todo-item>
  </ol>
</div>
```

## 修饰符
> 修饰符（Modifiers）是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()

```html
<form v-on:submit.pervent="onSubmit"></form>


<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联  -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身（比如不是子元素）触发时触发回调 -->
<div v-on:click.self="doThat">...</div>
<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>

<!-- 只有在 keyCode 是 13 时调用 vm.submit() -->
<input v-on:keyup.13="submit">
<!-- 同上 -->
<input v-on:keyup.enter="submit">
<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

### 按键别名
* enter
* .tab
* .delete (捕获 “删除” 和 “退格” 键)
* .esc
* .space
* .up
* .down
* .left
* .right



## 实例方法
> `$`前缀

```javascript
var data = {a: 1}
var vm = new Vue({
    el: '#exapmle',
    data: data
})

vm.$data == data // true
vm.$el == document.getElementById('example')
// 脏值检测
vm.$watch('a', (newVal, oldVal){

})
```
## 属性
### [计算属性 computed](https://cn.vuejs.org/v2/guide/computed.html)
> ** 嵌套v-for下面，无法使用计算属性 **
> 具有缓存，在值变换时才重新计算，默写开销大的求职，可以使用这个

```javascript
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
})
```

### Watched属性 观察者
> Vue 确实提供了一种更通用的方式来观察和响应 Vue 实例上的数据变动：watch 属性。当你有一些数据需要随着其它数据变动而变动时，你很容易滥用 watch——特别是如果你之前使用过 AngularJS。然而，通常更好的想法是使用 computed 属性而不是命令式的 watch 回调。

> 虽然计算属性在大多数情况下更合适，但有时也需要一个自定义的 watcher。这是为什么 Vue 通过 watch 选项提供一个更通用的方法，来响应数据的变化。当你想要在数据变化响应时，执行异步操作或开销较大的操作，这是很有用的。

**不应该使用箭头函数来定义 watcher 函数**


## Style & Class

```html
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
<!-- 此例始终添加 errorClass ，但是只有在 isActive 是 true 时添加 activeClass。 -->
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
<div v-bind:class="[{ active: isActive }, errorClass]"></div>

<div v-bind:style="[baseStyles, overridingStyles]"></div>
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>

```

### 脏值检测

> 由于 JavaScript 的限制，Vue 不能检测以下变动的数组：
>    1. 当你利用索引直接设置一个项时，例如： vm.items[indexOfItem] = newValue
>    2. 当你修改数组的长度时，例如： vm.items.length = newLength

#### 解决方案

```javascript
// Vue.set
Vue.set(example1.items, indexOfItem, newValue)
// Array.prototype.splice
example1.items.splice(indexOfItem, 1, newValue)

// 为了解决第二类问题，你可以使用 splice 
example1.items.splice(newLength)

```

## 优先级

### v-for with v-if  v-for的优先级高一点

```html
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>

<ul v-if="todos.length">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
<p v-else>No todos left!</p>

```

## [表单form](https://cn.vuejs.org/v2/guide/forms.html)


###　v-model
> `v-model` 会忽略所有表单元素的 value、checked、selected 特性的初始值。因为它会选择 Vue 实例数据来作为具体的值。你应该通过 JavaScript 在组件的 data 选项中声明初始值。


### options

```html
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Selected: {{ selected }}</span>
```