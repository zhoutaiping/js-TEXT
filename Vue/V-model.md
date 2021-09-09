## v-model 的原理

### v-model 本质上是语法糖，即利用 v-model 绑定数据， 其实就是既绑定了数据，又添加了一个 input 事件监听

- text 和 textarea 元素使用 value property 和 input 事件；
- checkbox 和 radio 使用 checked property 和 change 事件；
- select 字段将 value 作为 prop 并将 change 作为事件。

```javascript
在普通标签上
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>

```

```javascript
组件上

<currency-input v-model="price"></currentcy-input>

<!--上行代码是下行的语法糖
 <currency-input :value="price" @input="price = arguments[0]"></currency-input>
-->


<!-- 子组件定义 -->
Vue.component('currency-input', {
 template: `
  <span>
   <input
    ref="input"
    :value="value"
    @input="$emit('input', $event.target.value)"
   >
  </span>
 `,
 props: ['value'],
})


```
