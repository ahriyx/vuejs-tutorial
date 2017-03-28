# Vue.js  Tutorial

---

## 搭建案例演示自动刷新环境

创建一个 `package.josn` 文件：

```bash
npm init -y
```

安装 `browser-sync`:

```bash
# npm install --save-dev browser-sync
# 将 browser-sync 包保存到开发依赖中
# 就可以执行 npm install 的时候加入 --production 选项不安装这个包
npm i -D browser-sync
```

在 package.json 文件中加入以下内容：

```json
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "start": "browser-sync start --server --directory --files \"code/*.html\""
}
```

打开终端，启动开发预览服务：

```bash
npm start
```

---

## 安装

- Vue.js 不支持 IE8 及其以下版本，因为 Vue.js 使用了 IE8 不能模拟的 ECMAScript 5 特性
- 每个版本的更新日志见：https://github.com/vuejs/vue/releases
- 独立 js 文件
  + 开发版本（未压缩）：http://vuejs.org/js/vue.js
  + 生产版本（压缩）：http://vuejs.org/js/vue.min.js
- CDN：https://unpkg.com/vue
- NPM: `npm install vue`
- Bower: `bower install vue`
- 兼容 AMD 规范
  + 独立下载版本或通过 Bower 安装的版本已用 UMD 包装，因此它们可以直接用作 AMD 模块。

---

## Vue 实例

- 每个 Vue.js 应用都是通过构造函数 Vue 创建的
- 在实例化 Vue 时，需要传入一个选项对象，它可以包含数据、模板、挂载元素、方法、生命周期钩子等选项
  + data: 属性数据
  + computed: 计算属性
  + methods: 方法
  + el: 挂载点
  + directives: 自定义指令
  + filters: 自定义过滤器
  + ...
  + 全部选项可以在 API 文档中查看：https://cn.vuejs.org/v2/api/

- 实例选项：data
  + https://cn.vuejs.org/v2/guide/instance.html#属性与方法
  + [选项/数据 - data](https://cn.vuejs.org/v2/api/#data)
  + [深入响应式原理](https://cn.vuejs.org/v2/guide/reactivity.html)
  + 作用：根据视图抽象数据模型
  + 注意：视图中使用的数据必须在 data 中初始化
  + 每个 VUe 实例都会代理其 data 对象里所有的属性
    * 也可以通过 vue 实例的 $data 属性访问 data 中的数据
    * 建议：如果要使用 vm 读取或修改  data 中的数据，建议加上 vm.$data 去访问
  + 只有被初始代理的属性是响应式的
    * 如果是在实例创建之后添加新的属性到实例上，它不会触发视图更新
  + Vue 不允许在已经创建的实例上动态添加新的根级响应式属性
  - Vue 不能检测到对象属性的动态添加或删除
    + 也就是说动态添加或删除的对象属性不是响应式的
    + 如果希望动态添加和删除对象的属性是响应式的则需要通过：
      * `Vue.set( object, key, value )`
      * 或 `vm.$set( object, key, value )`
    + 如果删除对象属性是响应式的：
      * `Vue.delete( object, key )`
      * 或 `vm.$delete( object, key )`
    

- 实例选项：methods
  + https://cn.vuejs.org/v2/api/#methods
  + 作用：为视图交互提供行为函数
  + 可以在行为函数中通过 `this` 访问 data 中的数据成员
  + 注意：methods 中的行为函数不要写箭头函数
    * 因为这样会改变内部 this 的指向

- 实例属性
  + https://cn.vuejs.org/v2/api/#实例属性
  + $data
    * Vue 实例观察的数据对象。Vue 实例代理了对其 data 对象属性的访问。
  + $el
    * Vue 实例使用的根 DOM 元素

- 实例方法/数据
  + https://cn.vuejs.org/v2/api/#实例方法-数据
  + $watch
  + $set  Vue.set 的别名
  + $delete Vue.delete 的别名

---

## 模板语法

### 插值

#### 文本

- 响应插值：
  + `<span>Message: {{ msg }}</span>`
  + 注意： Mustache 语法不能在 HTML 属性中使用，应使用 `v-bind` 指令
- 一次性插值：
  + `<span v-once>This will never change: {{ msg }}</span>`
  + 注意：会影响该节点及内部节点所有的绑定

#### 纯 HTML

双大括号会将数据解释为纯文本，而非 HTML 。为了输出真正的 HTML ，你需要使用 v-html 指令：

```html
<div v-html="rawHtml"></div>
```

- 为什么不直接输出 HTML
- 什么是 XSS 攻击：跨站脚本攻击
  + 后天或者后后天补课

#### 属性

**注意：Mustache 不能在 HTML 属性中使用，应使用 v-bind 指令：**

```html
<div v-bind:id="dynamicId"></div>
```

这对布尔值的属性也有效 —— 如果条件被求值为 false 的话该属性会被移除：

```html
<button v-bind:disabled="someDynamicCondition">Button</button>
```

#### 使用 JavaScript 表达式

Vue.js 都提供了完全的 JavaScript 表达式支持：

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```

这些表达式会在所属 Vue 实例的数据作用域下作为 JavaScript 被解析。
有个限制就是，每个绑定都只能包含单个表达式，所以下面的例子都不会生效:

```html
<!-- 这是语句，不是表达式 -->
{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```

### 指令

### Vue 内置指令

- v-text
  + 和 {{}} 效果是一样
  + 但是 {{}} 会闪烁
  + 解决方法就是利用 v-text 绑定数据
  + 如果又想用 {{}}} 还需要避免闪烁
  + 使用 v-cloak 处理
- v-html
  + 默认 {{}} 和 v-text 会把 html 格式字符串原样输出
  + 可以使用 v-html 将 html 格式字符串作为 HTML 渲染到节点中
- v-show
  + 是否显示和隐藏
- v-if
  + 是否渲染和移除
- v-else
  + v-if 的 else 块
- v-else-if
  + 是 v-if 的逻辑块
  + 同样的，也必须紧跟着 v-if 
- v-for
  + 循环遍历输出
- v-on
  + DOM 元素的事件绑定
  + 例如：`v-on:click`、`v-on:blur`
- v-bind
  + 动态绑定 HTML 属性
  + 例如：`v-bind:title`、`v-bind:class`
- v-model
  + 和表单控件进行双向数据绑定
- v-pre
  + 不处理指定节点及内部所有节点的 vue 规则
  + 例如可以用来显示原始的 Mustache 标签
  + 作用：跳过大量没有指令的节点可以加快编译速度
- v-cloak
  + 可以处理表达式闪烁的问题
- v-once
  + 一次性绑定，后续数据的更新不会响应

指令（Directives）是带有 `v-` 前缀的特殊属性。
指令属性的值预期是单一 JavaScript 表达式（除了 `v-for`，之后再讨论）。指令的职责就是当其表达式的值改变时相应地将某些行为应用到 DOM 上。

```html
<p v-if="seen">Now you see me</p>
```

这里， v-if 指令将根据表达式 seen 的值的真假来移除/插入 <p> 元素。

#### 参数

一些指令能接受一个“参数”，在指令后以冒号指明。
例如， v-bind 指令被用来响应地更新 HTML 属性：

```html
<a v-bind:href="url"></a>
```

在这里 `href` 是参数，告知 `v-bind` 指令将该元素的 `href` 属性与表达式 `url` 的值绑定。

另一个例子是 v-on 指令，它用于监听 DOM 事件：

```html
<a v-on:click="doSomething">
```

在这里参数是监听的事件名：`click`。

#### 修饰符

修饰符（Modifiers）是以半角句号 . 指明的特殊后缀，用于指出一个指令应该以特殊方式绑定。
例如，.prevent 修饰符告诉 v-on 指令对于触发的事件调用 event.preventDefault()：

```html
<div>
  <input type="text" v-on:keyup.enter="xxx">
</div>
```

```html
<form v-on:submit.prevent="onSubmit"></form>
<input type="text" v-on:keyup.enter="addTodo">
```

### 过滤器

> 注意：Vue 2.x 中，过滤器只能在 mustache 绑定和 v-bind 表达式（从 2.1.0 开始支持）中使用，
> 因为过滤器设计目的就是用于文本转换。为了在其他指令中实现更复杂的数据变换，你应该使用 **计算属性**。

- Vue.js 允许你自定义过滤器，可被用作一些常见的文本格式化
- 过滤器可以用在两个地方：`mustache 插值` 和 `v-bind 表达式`

全局过滤器：

```js
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})
```

局部过滤器：

```js
new Vue({
  // ...
  filters: {
    capitalize: function (value) {
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
```

过滤器使用格式：

```html
<!-- in mustaches -->
{{ message | capitalize }}

<!-- in v-bind -->
<div v-bind:id="rawId | formatId"></div>
```

过滤器可以串联：

```html
{{ message | filterA | filterB }}
```

过滤器是 JavaScript 函数，因此可以接受参数：

```html
{{ message | filterA('arg1', arg2) }}
```

这里，字符串 'arg1' 将传给过滤器作为第二个参数，arg2 表达式的值将被求值然后传给过滤器作为第三个参数。

### 缩写

#### v-bind 缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```

#### v-on 缩写

```html
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a>
```

---

## 计算属性

模板内的表达式是非常便利的，但是它们实际上只用于简单的运算。
在模板中放入太多的逻辑会让模板过重且难以维护。例如：

```html
<div id="example">
  {{ message.split('').reverse().join('') }}
</div>
```

在这种情况下，模板不再简单和清晰。
这就是对于任何复杂逻辑，你都应当使用计算属性的原因。

#### 基础例子：反转字符串

```js
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

你可以像绑定普通属性一样在模板中绑定计算属性。 
Vue 知道 `vm.reversedMessage` 依赖于 `vm.message` ，
因此当 `vm.message` 发生改变时，所有依赖于 `vm.reversedMessage` 的绑定也会被重新计算进行更新。

---

## Class 与 Style 绑定

在 `v-bind` 用于 `class` 和 `style` 时， Vue.js 专门增强了它。
表达式的结果类型除了 **字符串** 之外，还可以是 **对象** 或 **数组** 。

### 绑定 HTML Class

#### 对象语法

```html
<div v-bind:class="{ active: isActive }"></div>

<!-- v-bind:class 指令可以与普通的 class 属性共存 -->
<div class="static"
     v-bind:class="{ active: isActive, 'text-danger': hasError }">
</div>
```

也可以直接绑定数据里的一个对象：

```html
<div v-bind:class="classObject"></div>
<script>
  new Vue({
    data: {
      classObject: {
        active: true,
        'text-danger': false
      }
    }
  })
</script>
```

#### 数组语法

```html
<!-- 可以把一个数组传给 v-bind:class ，以应用一个 class 列表 -->
<div v-bind:class="[activeClass, errorClass]">

data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}

<!-- 根据条件切换列表中的 class ，可以用三元表达式： -->
<div v-bind:class="[isActive ? activeClass : '', errorClass]">

<!-- 可以在数组语法中使用对象语法： -->
<div v-bind:class="[{ active: isActive }, errorClass]">
```

### 绑定内联样式

```html
<!-- CSS 属性名可以用驼峰式（camelCase）或名短横分隔命（kebab-case） -->
<div v-bind:style="{ color: activeColor, 'font-size': fontSize + 'px' }"></div>

data: {
  activeColor: 'red',
  fontSize: 30
}

<!-- 直接绑定到一个样式对象 -->
<div v-bind:style="styleObject"></div>
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}

<!-- v-bind:style 的数组语法可以将多个样式对象应用到一个元素上 -->
<div v-bind:style="[baseStyles, overridingStyles]">
```

---

## 条件渲染

### v-if-else-elseif

```html
<!-- 基本用法 -->
<h1 v-if="ok">Yes</h1>

<!-- 
  通过 template 包装多个元素，渲染结果不包含 template 
  v-else 元素必须紧跟在 v-if 或者 v-else-if 元素的后面——否则它将不会被识别。
-->
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>

<!-- 使用 v-else 指令来表示 v-if 的“else 块” -->
<div v-if="Math.random() > 0.5">
  Now you see me
</div>
<div v-else>
  Now you don't
</div>

<!-- 
  v-else-if，顾名思义，充当 v-if 的“else-if 块”。可以链式地使用多次： 
  v-else,，v-else-if 必须紧跟在 v-if 或者 v-else-if 元素之后
-->
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

---

## 列表渲染

---

## 事件处理器

---

## 表单控件绑定

---

## 组件

组件是 Vue.js 最强大的功能，组件可以扩展自定义 HTML 元素，封装可重用代码。

### 组件的命名

- 如果一个单词就只写一个单词即可
- 如果是多个单词组成的名字
  + 建议使用短横杠的方式
- 如果使用的是驼峰命名
  + 则在 DOM 模板中必须将 驼峰命名的组件名改为短横杠的方式
  + 在 字符串模板中，无论是驼峰还是短横杠都行

### 组件基础

- 注册全局组件
- 注册局部组件
- 组件的模板
- 组件的 data

#### 注册全局组件：`Vue.component(tagName, options)`

注册：

```js
Vue.component('my-component', {
  template: '<div>A custom component!</div>'
})
```

使用：

```html
<div id="example">
  <my-component></my-component>
</div>
```

渲染为：

```html
<div id="example">
  <div>A custom component!</div>
</div>
```

#### 组件的 template

组件的 template 必须具有一个根节点，否则，模板编译报错。

- 可以是内联模板
- 可以是 script 标签模板
- 可以是 .vue 模板

#### 局部注册组件：实例选项的 `components`

不必在全局注册每个组件。
通过使用组件实例选项注册，可以使组件仅在另一个实例/组件的作用域中可用：

```html
<body>
  <div id="app">
    <!-- 渲染为 <div>局部组件</div> -->
    <my-component></my-component>
  </div>
  <script src="../node_modules/vue/dist/vue.js"></script>
  <script>
    new Vue({
      el: '#app',
      components: {
        'my-component': {
          template: '<div>局部组件</div>'
        }
      },
      data: {
      },
    })
  </script>
</body>
```

#### 在 DOM 模板中使用组件注意事项

当使用 DOM 作为模版时（例如，将 `el` 选项挂载到一个已存在的元素上）,
你会受到 HTML 的一些限制，
因为 Vue 只有在浏览器解析和标准化 HTML 后才能获取模版内容。
尤其像这些元素 `<ul>` ， `<ol>`， `<table>` ， `<select>` 限制了能被它包裹的元素， 
`<option>` 只能出现在其它元素内部。

在自定义组件中使用这些受限制的元素时会导致一些问题，例如：

```html
<table>
  <my-row>...</my-row>
</table>
```

自定义组件 `<my-row>` 被认为是无效的内容，因此在渲染的时候会导致错误。
变通的方案是使用特殊的 `is` 属性：

```html
<table>
  <tr is="my-row"></tr>
</table>
```

**应当注意，如果您使用来自以下来源之一的字符串模板，这些限制将不适用：**

- `<script type="text/x-template">`
- JavaScript内联模版字符串
- `.vue` 组件

因此，推荐使用字符串模板。

#### 组件的 `data` 必须是函数

在组件中，data 必须是函数，下面是错误的方式：

```js
Vue.component('my-component', {
  template: '<span>{{ message }}</span>',
  data: {
    message: 'hello'
  }
})
```

正确的方式：

```js
data: function () {
  return {
    message: '组件的 data 必须是函数返回一个json字面量对象'
  }
}
```

### 组件通信

- 使用 prop 传递数据
- props 命名规则
  + camelCase 和 kebab-case
- 动态 prop
  + v-bind
- 字面量语法 vs 动态语法
- 单向数据流

组件意味着协同工作，通常父子组件会是这样的关系：组件 A 在它的模版中使用了组件 B 。
它们之间必然需要相互通信：

- 父组件要给子组件传递数据
- 子组件需要将它内部发生的事情告知给父组件

然而，在一个良好定义的接口中尽可能将父子组件解耦是很重要的。
这保证了每个组件可以在相对隔离的环境中书写和理解，也大幅提高了组件的可维护性和可重用性。

在 Vue.js 中，父子组件的关系可以总结为 `props down, events up`:

- 父组件通过 `props` 向下传递数据给子组件
- 子组件通过 `events` 给父组件发送消息

![img/props-events.png](img/props-events.png)

#### 使用 prop 传递数据

组件实例的作用域是孤立的。
这意味着不能(也不应该)在子组件的模板内直接引用父组件的数据。
要让子组件使用父组件的数据，我们需要通过子组件的props选项。

子组件要显式地用 `props` 选项声明它期待获得的数据：

```js
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 就像 data 一样，prop 可以用在模板内
  // 同样也可以在 vm 实例中像 “this.message” 这样使用
  template: '<span>{{ message }}</span>'
})
```

然后我们可以这样向它传入一个普通字符串：

```html
<child message="hello!"></child>
```

#### camelCase 和 kabab-case 命名规则

#### 动态 prop

#### 字面量语法 vs 动态语法

#### 单向数据流

prop 是单向绑定的：

- 当父组件的属性发生变化时，将传导给子组件
  + 子组件动态绑定的 prop，当父组件更新，子组件所有的 prop 都会得到更新
- 但是不会反过来
  + 也就是说，在子组件内部修改 prop 数据
    * 子组件内部会响应更新
    * 更新不会传导给父组件
    * 同时 Vue 会在控制台发出警告
    * 对象和数组除外
    * 如果 prop 是一个对象或数组，在子组件内部修改它会影响父组件的状态
    * 如果直接给 prop 中的对象或数组类型数据重新赋值，父组件也不会得到更新
  + 这是为了防止子组件无意间修改了父组件的状态
  + 这会让数据流的走向变得混乱而难以理解

为什么我们会有修改 prop 中数据的冲动呢？

1. prop 作为初始值传入后，子组件想要把它当作局部数据来用
2. prop 作为初始值传入后，由子组件处理成其它数据输出

对于这两种原因，正确的方式是：

1. 定义一个局部变量，并用 prop 的值初始化它

```js
props: ['initialCounter'],
data: function () {
  return {
    counter: this.initialCounter 
  }
}
```

2. 定义一个计算属性，处理 prop 的值并返回

```js
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

#### Prop 验证

我们可以为组件的 props 指定验证规格。
如果传入的数据不符合规格，Vue 会发出警告。
当组件给其他人使用时，这就很有用了。

要指定验证规格，需要使用对象的形式，而不能用字符串数组：

```js
Vue.component('example', {
  props: {
    // 基础类型检测 （`null` 意思是任何类型都可以）
    propA: Number,
    // 多种类型
    propB: [String, Number],
    // 必传且是字符串
    propC: {
      type: String,
      required: true
    },
    // 数字，有默认值
    propD: {
      type: Number,
      default: 100
    },
    // 数组／对象的默认值应当由一个工厂函数返回
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
})
```

`type` 可以是下面原生的数据类型：

- String
- Number
- Boolean
- Function
- Object
- Array

`type` 也可以是一个自定义构造器函数（例如 Person），
Vue 会使用 `instanceof` 对数据进行检测。

当 prop 验证失败，Vue会在抛出警告 (如果使用的是开发版本)。

### 自定义事件（父子通信）

- 使用 v-on 绑定自定义事件
- 使用自定义事件的表单输入组件
- 非父子组件通信

父组件是使用 props 传递数据给子组件，
但如果子组件要把数据传递回去，应该怎样做？
那就是自定义事件！

#### 使用 v-on 绑定自定义事件

每个 Vue 实例都实现了事件接口：

- 使用 $on(eventName) 监听事件
- 使用 $emit(eventName) 触发事件

父组件可以在使用子组件的地方直接使用 `v-on` 监听子组件发射的事件。
注意：不能使用 `$on` 侦听子组件抛出的事件，而必须在模板里直接使用 `v-on` 绑定。

```html
<body>
  <div id="app">
    <p>{{ total }}</p>
    <child v-on:increment="incrementTotal"></child>
    <child v-on:increment="incrementTotal"></child>
  </div>
  <script src="../node_modules/vue/dist/vue.js"></script>
  <script>
    Vue.component('child', {
      template: `
      <div>
        <span>{{ counter }}</span>
        <button @click="increment">increment</button>
      </div>`,
      data () {
        return {
          counter: 0
        }
      },
      methods: {
        increment () {
          this.counter += 1
          this.$emit('increment')
        }
      }
    })
    new Vue({
      el: '#app',
      data: {
        total: 0
      },
      methods: {
        incrementTotal () {
          this.total += 1
        }
      }
    })
  </script>
</body>
```

在本示例中，子组件已经和它外部完全解耦了。
它所做的只是报告自己的内部事件，至于父组件是否关心则与它无关。

有时候，可能想要在某个组件的根元素上绑定一个原生事件。
可以使用 `.native` 修饰 `v-on`。例如：

```html
<my-component v-on:click.native="doTheThing"></my-component>
```

#### 非父子组件通信

有时候两个组件也需要通信(非父子关系)。
在简单的场景下，可以使用一个空的 Vue 实例作为中央事件总线：

```html
<body>
  <div id="app">
    <child-a></child-a>
    <child-b></child-b>
  </div>
  <script src="../node_modules/vue/dist/vue.js"></script>
  <script>
    const bus = new Vue()
    Vue.component('child-a', {
      template: `
      <div>
        <p>组件 child A</p>
        <button @click="emitDataToB">发射数据到</button>
      </div>
      `,
      methods: {
        emitDataToB() {
          // 在组件 A 中通过 $emit 发射 data 事件，组件 B 中的钩子监听了 data 事件
          bus.$emit('data', '组件a传递过来的数据')
        }
      }
    })
    Vue.component('child-b', {
      template: `
      <div>
        <p>组件 child B</p>
        <p>{{ message }}</p>
      </div>
      `,
      created() {
        const vm = this
          // 在组件 B 的钩子中通过 bud 的 $on 监听事件
        bus.$on('data', function (data) {
          vm.message = data
        })
      },
      data() {
        return {
          message: 'hello child b'
        }
      }
    })
    new Vue({
      el: '#app',
      data: {},
    })
  </script>
</body>
```

### 使用 Slot 分发内容

- 编译作用域
- 单个 Slot
- 具名 Slot
- 作用域插槽

在使用组件的时候，我们常常要像这样组合它们：

```html
<app>
  <app-header></app-header>
  <app-footer></app-footer>
</app>
```

注意两点：

1. `<app>` 组件不知道它的挂载点会有什么内容。挂载点的内容是由 `<app>` 的父组件决定的
2. `<app>` 组件很可能有它自己的模板

为了让组件可以组合，我们需要一种方式来混合父组件的内容和子组件自己的模板。
这个过程被称为 **内容分发**（或 “transclusion”）。

Vue.js 实现了一个内容分发 API，参照了当前 Web 组件规范草案，
使用特殊的 `<slot>` 元素作为原始内容的插槽。

#### 编译作用域

#### 单个 Slot

- 如果子组件没有 `<slot>` 插口，否则父组件的内容会被丢弃
- 当子组件模板只有一个没有属性的 slot 时
  + 父组件整个内容片段都将插入到 slot 所在的 DOM 位置
  + 并替换掉 slot 标签本身
  + 在 slot 标签中的任何内容都被视为 备用内容
  + 备用内容在子组件的作用域内编译
  + 并且只有宿主元素为空的时候，备用内容才会被编译显示出来

示例如下：

```html
<body>
  <div id="app">
    <bs-panel title="面板1">
      面板1的内容
    </bs-panel>
    <bs-panel title="面板2">
      面板2的内容
    </bs-panel>
    <bs-panel title="没有分发内容的面板">
    </bs-panel>
  </div>
  <script src="../node_modules/vue/dist/vue.js"></script>
  <script>
    Vue.component('bs-panel', {
      template: `
      <div class="panel panel-default">
        <div class="panel-heading">
          <h3 class="panel-title">{{ title }}</h3>
        </div>
        <div class="panel-body">
          <slot>
            只有才没有分发的内容时才会显示
          </slot>
        </div>
      </div>
      `,
      props: {
        title: { type: String, required: true }
      }
    })
    new Vue({
      el: '#app',
      data: {},
    })
  </script>
</body>
```

#### 具名 slot

- 在组合组件时，内容分发 API 是非常有用的机制
- `<slot>` 元素可以用一个特殊的属性 `name` 来配置如何分发内容
- 多个 slot 可以有不同的名字。
- 具名 slot 将匹配内容片段中有对应 slot 特性的元素
- 仍然可以有一个匿名 slot，它是默认 slot
  + 作为找不到匹配的内容片段的备用插槽
  + 如果没有默认的 slot，这些找不到插槽的内容片段将被抛弃

```html
<body>
  <div id="app">
    <app-layout>
      <h1 slot="header">顶部</h1>
      <p>内容段落</p>
      <p>内容段落</p>
      <p slot="footer">底部信息</p>
    </app-layout>
  </div>
  <script src="../node_modules/vue/dist/vue.js"></script>
  <script>
    Vue.component('app-layout', {
      template: `
        <div class="container">
          <header>
            <slot name="header"></slot>
          </header>
          <main>
            <slot></slot>
          </main>
          <footer>
            <slot name="footer"></slot>
          </footer>
        </div>
      `
    })
    new Vue({
      el: '#app',
      data: {},
    })
  </script>
</body>
```

#### 作用域插槽

- 目的：作用域插槽的目的就是可以将子组件内部的数据传递到外部
- 在子组件中，在 `slot` 标签上通过属性的方式将 prop 数据传递到外部
- 在父组件中，通过具有特殊属性 `scope` 的 `<template>` 元素，表示它是作用域插槽的模板
  + `scope` 的值对应一个临时变量名
  + 该变量接收来自子组件中通过 `slot` 元素属性传递的 prop 数据

示例如下：

```html
<body>
  <div id="app">
    <child>
      <template scope="props">
        <p>hello from parent</p>
        <p>{{ props.text }}</p>
        <p>{{ props.message }}</p>
      </template>
    </child>
  </div>
  <script src="../node_modules/vue/dist/vue.js"></script>
  <script>
    Vue.component('child', {
      template: `
        <div class="child">
          <input v-model="message" />
          <slot text="hello from child" :message="message"></slot>
        </div>
      `,
      data () {
        return {
          message: 'child message'
        }
      }
    })
    new Vue({
      el: '#app',
      data: {
      },
    })
  </script>
</body>
```


作用域插槽更具代表性的用例是列表组件，允许组件自定义应该如何渲染列表每一项：

```html
<body>
  <div id="app">
    <my-awesome-list :todos="todos">
      <template slot="item" scope="props">
        <li class="my-fancy-item">{{ props.text }}</li>
      </template>
    </my-awesome-list>
  </div>
  <script src="../node_modules/vue/dist/vue.js"></script>
  <script>
    Vue.component('my-awesome-list', {
      props: ['todos'],
      template: `
        <ul>
          <slot name="item"
            v-for="item in todos"
            :text="item.title">
            <!-- fallback content here -->
          </slot>
        </ul>
      `
    })
    new Vue({
      el: '#app',
      data: {
        todos: [
          { id: 1, title: '吃饭' },
          { id: 2, title: '睡觉' },
          { id: 3, title: '打豆豆' },
        ]
      },
    })
  </script>
</body>
```

### 动态组件

通过保留的 `<component>` 元素，动态的绑定到它的 is 特性，
我们让多个组件可以使用同一个挂载点：

简单示例

```html
<body>
  <div id="app">
    <select v-model="currentView">
      <option value="home">home</option>
      <option value="posts">posts</option>
      <option value="archive">archive</option>
    </select>
    <component v-bind:is="currentView"></component>
  </div>
  <script src="../node_modules/vue/dist/vue.js"></script>
  <script>
    new Vue({
      el: '#app',
      data: {
        currentView: 'home'
      },
      components: {
        home: {
          template: '<div>home</div>',
        },
        posts: {
          template: '<div>posts</div>',
        },
        archive: {
          template: '<div>archive</div>',
        }
      }
    })
  </script>
</body>
```

登陆注册示例：

```html
<body>
  <div id="app">
    <ul>
      <li><a href="JavaScript:void(0)" @click="defaultView = 'register'">注册</a></li>
      <li><a href="JavaScript:void(0)" @click="defaultView = 'login'">登陆</a></li>
    </ul>
    <div>
      <component :is="defaultView"></component>
    </div>
    <hr><hr><hr><hr>
    <div>
      <!-- 可以使用 keep-alive 保持组件状态 -->
      <keep-alive>
        <component :is="defaultView"></component>
      </keep-alive>
    </div>
  </div>
  <script src="../node_modules/vue/dist/vue.js"></script>
  <script>
    new Vue({
      el: '#app',
      components: {
        login: {
          template: `
            <form action="">
              <div>
                <label for="">用户名</label>
                <input type="text" />
              </div>
              <div>
                <label for="">密码</label>
                <input type="password" />
              </div>
              <div>
                <input type="submit" value="点击登陆" />
              </div>
            </form>
          `
        },
        register: {
          template: `
            <form action="">
              <div>
                <label for="">用户名</label>
                <input type="text" />
              </div>
              <div>
                <label for="">密码</label>
                <input type="password" />
              </div>
              <div>
                <label for="">确认密码</label>
                <input type="password" />
              </div>
              <div>
                <label for="">验证码</label>
                <input type="password" />
              </div>
              <div>
                <input type="submit" value="点击注册" />
              </div>
            </form>
          `
        }
      },
      data: {
        defaultView: 'login'
      },
    })
  </script>
</body>
```















---

## 使用 Vue 的一些经验

### 解决表达式闪烁

1. 将所有 `{{}}` 通过 `v-text` 替换
2. 使用 `v-cloak` 解决

第一，在要渲染处理的 DOM 节点上添加一个指令 `v-cloak`：

```html
<div id="app" ng-cloak>
  {{ message }}
</div>
```

第二，在 style 中加入一个属性选择器样式：

```css
[v-cloak] { 
  display: none; 
}
```

第三，解析执行机制：

1. 当浏览器解析处理到添加了 `v-cloak` 属性的节点的时候，属性样式被作用上来，也就是说默认这个容器就是隐藏着的
2. 然后当 Vue 程序解析渲染完HTML模板的时候，自动将容器上的 `v-cloak` 属性移除


