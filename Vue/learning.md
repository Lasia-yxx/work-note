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
