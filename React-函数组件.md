## 函数组件
  -  1. 组件名字首字母一定是大写的
  -  2. 返回一个jsx
  -  3. jsx依赖React，所以组件内部需要引入React
  -  4. 组件传参
    -  a. 传递. <Component list={ arrData }><Component>
    -  b. 接收. function Component( props ){...}
    -  c. 使用. const { list } = props，list就是参数数据
  -  5.   缺点：【无状态组件】只能实现很简单的视图展示功能，没有自己的内容数据、     没有状态，没有逻辑处理，【没有this】，【没有生命周期】。
  -  6. 16.7以后版本的react有状态和钩子函数提供使用。不过版本过新不推荐用
  内部不用render函数，会自动把return返回结果当做render返回结果【见类组件的必须要求】


  ```javascript
    function Component(props) {
      return <h1>Hello, {props.name}</h1>;
    }

    const element = <Component name="Sara" />;
    ReactDOM.render(
      element,
      document.getElementById('root')
    );
  ```