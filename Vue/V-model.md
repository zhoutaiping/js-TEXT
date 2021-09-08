## v-model 的原理

```javascript

v-model本质上是语法糖，即利用v-model绑定数据，
其实就是既绑定了数据，又添加了一个input事件监听


<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
>

```
