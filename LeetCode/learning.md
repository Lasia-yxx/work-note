# Leetcode 刷题记录

## #5709 最大升序子数组和 `(Easy)`

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
