## VUE 2 组件通信

1： props 和$emit 父组件向子组件传递数据是通过 prop 传递的，子组件传递数据给父组件是通过$emit 触发事件来做到的

2：- $parent,$children 获取当前组件的父组件和当前组件的子组件

3： $attrs 和$listeners A->B->C。Vue 2.4 开始提供了$attrs 和$listeners 来解决这个问题

4： 父组件中通过 provide 来提供变量，然后在子组件中通过 inject 来注入变量。(官方不推荐在实际业务中使用，但是写组件库时很常用)

5: $refs 获取组件实例

5: envetBus 兄弟组件数据传递 这种情况下可以使用事件总线的方式

7: vuex 状态管理

## VUE 3 组件通信

1： props/$emit
