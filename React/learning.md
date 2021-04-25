# React Learning Notes

## useEffect 钩子

### 简介

在 React 中，组件写法有两种，可以使用类的方法来定义组件，同时也可以使用函数的方法。

- 使用类生成组件：

    ```javaScript
    class Welcome extends React.Component {
      render() {
        return <h1>Hello, {this.props.name}</h1>;
      }
    }
    ```

- 使用函数生成组件：

    ```javaScript
    function Welcome(props) {
      return <h1>Hello,{props.name}</h1>
    }
    ```

其中官方推荐使用函数的方式去生成一个组件。React 组件的思想使用的就是函数式编程的思想，所以使用函数的方法去定义更符合 React 的本质。

具体内容可以参考 [FunctionalProgramming](../FuctionalProgramming/learning.md##类和函数的差异) 中 `类和函数的差异` 的内容。

### 那么什么是钩子呢

> 一句话，钩子（hook）就是 React 函数组件的副效应解决方案，用来为函数组件引入副效应。 函数组件的主体只应该用来返回组件的 HTML 代码，所有的其他操作（副效应）都必须通过钩子引入。

由于副效应的情况有很多，所以在 React 中存在很多钩子，如 `useState() 保存状态` 、`useContext() 保存上下文` 、 `useRef() 保存引用` 等。

而 `useEffect()` 是通用的副效应钩子，当找不到对应钩子的时候就可以使用。

### 用法

`useEffect()` 本身就是一个函数，由 React 框架提供，在函数组件内部调用即可。

```javaScript
import React, { useEffect } from 'react';

function Welcome(props) {
  useEffect(() => {
    document.title = 'success'
  });
  return <h1>Hello, {props.name}</h1>;
}
```

`useEffect`作用就是指定一个副作用函数，组件每渲染一次，其就会执行一次。组件首次在网页 `DOM` 加载后，副效应函数也会执行。

### 第二个参数

其中 `useEffect()` 还可以接受第二个参数，当我们不希望 `useEffect()` 在每次渲染都去执行时，我们可以向其中传入第二个参数，此时该函数会与这个参数进行绑定，只有当第二个参数发生改变时，`useEffect()` 才会执行。

```javaScript
function Welcome(props) {
  useEffect(() => {
    document.title = `Hello, ${props.name}`;
  }, [props.name]);
  return <h1>Hello, {props.name}</h1>;
}
```

上面例子中，`useEffect()` 的第二个参数是一个数组，指定了第一个参数（副效应函数）的依赖项`（props.name）`。只有该变量发生变化时，副效应函数才会执行。

如果第二个参数是一个空数组，就表明副效应参数没有任何依赖项。因此，副效应函数这时只会在组件加载进入 DOM 后执行一次，后面组件重新渲染，就不会再次执行。这很合理，由于副效应不依赖任何变量，所以那些变量无论怎么变，副效应函数的执行结果都不会改变，所以运行一次就够了。

### 返回值

副效应是随着组件加载而发生的，那么组件卸载时，可能需要清理这些副效应。
`useEffect()` 允许返回一个函数，在组件卸载时，执行该函数，清理副效应。如果不需要清理副效应，`useEffect()` 就不用返回任何值。

```javaScript
useEffect(() => {
  const subscription = props.source.subscribe();
  return () => {
    subscription.unsubscribe();
  };
}, [props.source]);
```

上面例子中，`useEffect()` 在组件加载时订阅了一个事件，并且返回一个清理函数，在组件卸载时取消订阅。

实际使用中，由于副效应函数默认是每次渲染都会执行，所以清理函数不仅会在组件卸载时执行一次，每次副效应函数重新执行之前，也会执行一次，用来清理上一次渲染的副效应。

## useState 方法

### 简介

在 Vue 中，注册于 `data()` 中的变量都为响应式变量，每当响应式变量改变后，`DOM` 中绑定响应式变量的值就会发生改变，而在 React 中，只声明一个变量是无法做到响应式的，我们需要使用 `useState` 方法去声明一个响应式变量。

```javaScript
import {useState} from "react";

const [data, setData] = useState(0)
```

可以看到我们在声明 `data` 的同时，也声明了一个 `setData` 的方法，这个 `setData` 有什么用处呢？如果对 Vue 中的响应式原理有一定了解的话，看到这行代码大概就能猜到 `setData` 的用处了。

### 什么是 setData

`setData` 与 `data` 一同被声明，`data` 很好理解，就是变量，那么 `setData` 就是改变 `data` 的函数，对于观察者模式以及 Vue 的响应式原理可以参考此处，那么根据响应式原理，我们对于 `setData` 的理解就相对清晰了，当使用 `setData` 函数修改了 `data` 的值之后，与 `data` 有关联的地方都将发生对应改变。

- 示例：

    ```javaScript
    import React, {FC, useState} from 'react';

    const App: FC<any> = () => {
      const [data, setData] = useState(0)

      const handleClick = () => {
        setData(data + 1)
      }

      return(
        <h1 onclick={handleClick}>{data}</h1>
      )
    }
    ```

    我们可以很直观的看出，当点击 `data` 所在的 `<h1>` 区域时，`data` 会自增，并同时展现在 `DOM` 区域。

    值得注意的是，同 Vue 中观察者模式一样， `setData` 是一个异步事件。

### 设置复杂类型

当我们的 `data` 并不仅仅是一个数字类型，而是一个对象时，我们可以参考以下代码：

```javaScript
import React, {FC, useState} from 'react';

  const App: FC<any> = () => {
    const [data, setData] = useState({
      lastName: 'Yan',
      firstName: 'Lasia'
    })

    const handleClick = () => {
      setData({
        lastName: 'Yan',
        firstName: '晓骁'
      })
    }

    return(
      <div>
        <h1 onclick={handleClick}>{data.firstName}</h1>
        <h1 >{data.lastName}</h1>
      </div>
    )
  }
```

### 一些 TypeScript 写法

- 初始化响应式对象：

    ```typeScript
    interface Person{
      lastName: String,
      firstName: String,
      age: Number,
      gender: String
    }

    const [data, setData] = useState<Person>({
      lastName: 'Yan',
      firstName: 'Lasia',
      age: 21,
      gender: 'Male'
    })

    setData({
      lastName: 'Yan',
      firstName: 'Lasia',
      age: 18,
      gender: 'Female'
    })
    ```

- 初始化响应式数组：

    ```typeScript
    const [data, setData] = useState<Array<String>>(['Lasia'])

    setData([...data, 'Yan']) // data -> ['Lasia', 'Yan']
    ```
