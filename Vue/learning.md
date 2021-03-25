# Vue Learning Notes

## `v-for` 指令所期望的数据类型

`v-for` 指令可以迭代循环一个数据，然后生成 dom 元素，其中 `v-for` 期望如下几种数据类型：

- Array 数组类型：

    `v-for` 指令迭代传入的数组，将数组的元素作为输入值

- Object 数据类型：

    `v-for` 将会迭代对象中所有的 key 作为输出值。

- Number 数据类型：

    `v-for` 在接收数字类型作为参数时，如 `v-for="i in 10"` 会输出 `1，2，3...9，10` 的结果。

- String 类型：

    `v-for` 本质上可以迭代传入的数据，所以 `v-for` 会迭代字符串，将字符串里面的每一个字符作为输出值。

## Vue 在组件中修改 Props 数据报错

Props 传进来的数据为单向数据流，无法修改，此时我们修改 Props 的数据将会产生如下报错：

```shell
[Vue warn]: Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders. Instead, use a data or computed property based on the prop's value.

# 避免直接改变属性，因为每当父组件重新渲染时，该值将被覆盖。相反，使用基于属性值的数据或计算属性。
 ```

可以看到错误信息中让我们使用计算属性去解决这个问题。

- 使用计算属性解决报错：

    在这个思路中，我们使用一个计算属性，让其 return Props 的值，然后进行修改，但是如果直接这样操作会产生以下问题：

    ```shell
    [Vue warn]: Computed property "variable" was assigned to but it has no setter.

    # 计算属性已经被分配，但是没有 setter
    ```

    什么是 setter ？在 Vue 中，computed 的属性可以被视为是 data 一样，可以读取和设值，因此在 computed 中可以分成 getter（读取） 和 setter（设值），一般情况下是没有 setter 的，computed 预设只有 getter ，也就是只能读取，不能改变设值。

    参考官方文档可以得出，Compute 属性的正确声明方法如下：

    ``` javaScript
    computed: {
      count: {
        get() {
          return this.Props.data;
        },
        set(v) {
          return v;
        }
      };
    }
    ```

- 在 data 中注册 Props 的值，使用 `this.$emit()` 的方法去返回修改的值到父组件进行修改。

    使用 compute 方法可能会造成逻辑较为复杂的情况，推荐使用此种方法

    ```javaScript
    data(){
      return {
        data : this.Props.data,
      };
    },
    ```

## `v-if` 与 `v-show` 的区别

一直对这两者之间的功能有误解，我一直以为 `v-if` 命令只是单纯的将元素样式设置为 `display: none` 而 `v-show` 则将元素样式设置为 `display: hidden`。最后发现这两者都是将元素样式设置为 `display: none`。

- v-if：

    > `v-if` 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 *truthy* 值的时候被渲染

    从 Vue 的官方文档对于 `v-if` 的解释来说，`v-if` 在渲染之前进行判断，如果返回值为 *false* 的元素就不会被渲染，进而不会出现在 `DOM` 之中。此时打开调试工具，我们是无法在 `DOM` 树中找到这个元素的。当 `v-if` 的值为 *truthy* 时，此时元素会被插入到 `DOM` 中，比较消耗性能。

- v-show：

    >另一个用于根据条件展示元素的选项是 `v-show` 指令。用法大致一样。不同的是带有 `v-show` 的元素始终会被渲染并保留在 `DOM` 中。`v-show` 只是简单地切换元素的 *CSS property display*。

    官方文档对于 `v-show` 的解释的十分清楚了，`v-show` 不会去判断一个元素是否要被渲染，而是将元素设置为 `display: none`。这意味着元素任然存在于 `DOM` 中，此时打开控制台工具是能在 `DOM` 中查看到该元素的。

- 总结：

    - 共同点：

        都能控制元素的显示和隐藏。

    - 不同点：

        实现本质⽅法不同，`v-show` 本质就是通过控制 *css* 中的 `display` 设置为 `none`，控制隐藏，只会编译⼀次；`v-if` 是动态的向DOM树内添加或者删除 `DOM` 元素，若初始值为 *false*，就不会编译了。⽽且 `v-if` 不停的销毁和创建⽐较消耗性能。

    - 总结：

        如果要频繁切换某节点，使⽤ `v-show` *(切换开销⽐较⼩，初始开销较⼤)*。如果不需要频繁切换某节点使⽤ `v-if`*（初始渲染开销较⼩，切换开销⽐较⼤）*。

## Vue 事件总线

父子组件之间的通信可是使用 `$Emit` 方法进行传递，但是当我们需要让两个不相关的组件之间进行通信或者一个父组件中嵌套了很多子孙组件，使用 `$Emit` 方法就会变得难以管理。

如果有这样一种方法，当一个组件产生一个事件后就发出一个广播，让在监听这个事件的组件能收到这个广播，这样就完成了组件之间越级的事件传递，这个方法就是事件总线，即 `EventBus`

### 创建事件总线

- 创建全局事件总线：

    - 在 `main.js` 文件中创建 `EventBus`：

        ```javaScript
        Vue.prototype.$EventBus = new Vue()
        ```

    - 在组件中引用：

      ```javaScript
      this.$EventBus.$emit('eventName',{/* props */})
      ```

- 创建单独的事件总线：

    - 创建一个单独的 `event-bus.js` 文件：

        ```javaScript
        import Vue from 'vue';
        let EventBus = new Vue()
        export default EventBus
        ```

    - 在组件中引用：

        ```Html
        <script>
          import EventBus from ${url}
          EventBus.$emit('eventName', {/* props */})
        </script>
        ```

### 监听事件

- 子组件事件：

    ```javaScript
    handler(){
      this.$EventBus.$emit('click',{ /* props */})
    }
    ```

- 父组件监听：

    ```javaScript
    mounted(){
      this.$EventBus.$on('click',(props) => {

      })
    }
    ```

可以看到，子事件使用 `$emit` 触发自定义事件，而父组件使用 `$on` 去监听自定义事件。

那么什么是 `EventBus` 呢，为什么我们在初始化他的时候，使用的是 `new Vue()` 去新建一个 `Vue` 实例呢？

通过查看官方文档，我们可以看到官方文档中是没有 `EventBus` 的，然而我们注意到官方文档中定义了 `vm.$emit` 和 `vm.$on` 方法。而实际上这就是 `EventBus` 所做的事情，即新建一个 `Vue` 实例去调用这两个方法。

> 实质上EventBus是一个不具备 DOM 的组件，它具有的仅仅只是它实例方法而已，因此它非常的轻便。

### 移除监听

可以直接使用 `EventBus.$off(eventName)` 来移除监听 `eventName` 事件。

```javaScript
this.$EventBus.$off('eventName');
```

同时你也可以使用 `EventBus.$off()` 用来关闭所有正在监听的事件，不需要添加任何参数。
