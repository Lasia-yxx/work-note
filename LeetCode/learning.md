# Leetcode 刷题记录

## #5709 最大升序子数组和 `(Easy)`

### 描述

- 简述：

    给你一个正整数组成的数组 nums ，返回 nums 中一个 升序 子数组的最大可能元素和。

    子数组是数组中的一个连续数字序列。

    已知子数组 [nums<sub>l</sub>, nums<sub>l+1</sub>, ..., nums<sub>r-1</sub>,nums<sub>r</sub>] ，若对所有 i（l <= i < r），nums<sub>i</sub> < nums<sub>i+1</sub> 都成立，则称这一子数组为 升序 子数组。注意，大小为 1 的子数组也视作 升序 子数组

- 示例一：

    ```shell
    # input -> nums = [10,20,30,5,10,50]
    # output -> 65
    # 解释：[5,10,50] 是元素和最大的升序子数组，最大元素和为 65 。
    ```

- 示例二：

    ```shell
    # input -> nums = [10,20,30,40,50]
    # output -> 150
    # 解释：[10,20,30,40,50] 是元素和最大的升序子数组，最大元素和为 150 。
    ```

### 题解

- 题目分析：

    对于求解子数组的情况，我们可以考虑使用滑动窗口的方法去进行求解，我们遍历这个数组，如果当前元素比前一个元素的值要大，则将这个元素放入滑动窗口中，并更新滑动窗口的总值，如果出现比前一个值小的情况，就将滑动窗口的起点放在这个元素的位置上，同时比较刚才窗口的总和值是不是当前最大值，不断维护，最终就可以输出所有滑动窗口中最大的总和值。

- 代码：

    ```javaScript
    /**
     * @param {number[]} nums
     * @return {number}
     */
    var maxAscendingSum = function(nums) {
      let res = 0
      let temp = 0
      for(let i = 0;i<nums.length;i++){
          if(nums[i] < nums[i+1]){
              temp += nums[i]
          }else{
              temp += nums[i]
              res = res > temp ? res : temp
              temp = 0
          }
      }
      return res
    };
    ```

- 执行速度：

    `O(n)` 的线性时间复杂度，所以时间效率还可以。

    [![6ooskd.png](https://z3.ax1x.com/2021/03/22/6ooskd.png)](https://imgtu.com/i/6ooskd)

## #456 123 模式 `(Medium)`

### 描述

- 简述：

    给你一个整数数组 nums ，数组中共有 n 个整数。132 模式的子序列 由三个整数 nums[i]、nums[j] 和 nums[k] 组成，并同时满足：i < j < k 和 nums[i] < nums[k] < nums[j] 。

    如果 nums 中存在 132 模式的子序列 ，返回 true ；否则，返回 false 。

    进阶：很容易想到时间复杂度为 O(n^2) 的解决方案，你可以设计一个时间复杂度为 O(n log<sup>n</sup>) 或 O(n) 的解决方案吗？

- 示例 1：

    ```shell
    # input -> nums = [1,2,3,4]
    # output -> false
    # 解释：序列中不存在 132 模式的子序列。
    ```

- 示例 2：

    ```shell
    # input -> nums = [3,1,4,2]
    # output -> true
    # 解释：序列中有 1 个 132 模式的子序列： [1, 4, 2] 。
    ```

### 题解

- 思路：

    这道题使用暴力解法是最简单明了的方式，但是暴力法的时间复杂度是 *O(n<sup>2</sup>)*，这一点明显不符合题目中的进阶规范，所以我们需要有一个时间复杂度较低的方法。

    观察 `123模式` 我们可以得出一个结论，如果我们在迭代数组中维护 `3` 这个位置的话，那么我们就期望 `3` 左边的数字足够小，右边的数字足够大（最起码要比左边的大），因此，我们可以维护一个单调栈，此单调栈从栈底到栈顶单调递减。

    我们设置一个足够小的数为 `last`，我们从数组尾部开始循环，当 `num[i]` 的值要小于 last 时直接返回 *true*。然后与单调栈的栈顶元素进行比较，如果栈顶元素要小于 `nums[i]` 时，就将栈顶元素弹出，并赋值给 `last`（这里是个循环），当循环结束后，将 `nums[i]` 放入栈中，此时 `last` 的值就为 `3` 左边最大的值，这也印证了，当 `nums[i] < last` 时可以直接返回 *true* 的原因。

- 代码：

    ```javaScript
    /**
     * @param {number[]} nums
    * @return {boolean}
    */
    var find132pattern = function(nums) {
      let stack = []
      let last = Number.MIN_SAFE_INTEGER
      if(nums.length < 3){return false}
      for(let i = nums.length - 1;i>-1;i--){
        if(nums[i] < last){
          return true
        }
        while(stack.length && stack[stack.length - 1] < nums[i]){
          last = stack.pop()
        }
        stack.push(nums[i])
      }
      return false
    };
    ```

- 执行速度

    线性的时间复杂度，执行速度还算理想。

    [![6bcQ2T.png](https://z3.ax1x.com/2021/03/24/6bcQ2T.png)](https://imgtu.com/i/6bcQ2T)
