# LeetCode 刷题记录

## #5709 最大升序子数组和 `Easy`

### 描述

- 简述：

    给你一个正整数组成的数组 nums ，返回 nums 中一C个 升序 子数组的最大可能元素和。

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

- 复杂度分析：

    `O(n)` 的线性时间复杂度，所以时间效率还可以。

    [![6ooskd.png](https://z3.ax1x.com/2021/03/22/6ooskd.png)](https://imgtu.com/i/6ooskd)

## #456 123 模式 `Medium`

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

- 复杂度分析：

    线性的时间复杂度，执行速度还算理想。

    [![6bcQ2T.png](https://z3.ax1x.com/2021/03/24/6bcQ2T.png)](https://imgtu.com/i/6bcQ2T)

## #82 删除排序链表中的重复元素 v2 `Medium`

### 描述

- 简述：

    存在一个按升序排列的链表，给你这个链表的头节点 head ，请你删除链表中所有存在数字重复情况的节点，只保留原始链表中 没有重复出现 的数字。

    返回同样按升序排列的结果链表。

- 示例 1：

    [![6XF7CD.png](https://z3.ax1x.com/2021/03/25/6XF7CD.png)](https://imgtu.com/i/6XF7CD)

    ```shell
    # input -> head = [1,2,3,3,4,4,5]
    # output -> [1,2,5]
    ```

- 示例 2：

    [![6XFqvd.png](https://z3.ax1x.com/2021/03/25/6XFqvd.png)](https://imgtu.com/i/6XFqvd)

    ```shell
    # input -> head = [1,1,1,2,3]
    # output -> [2,3]
    ```

### 题解

- 思路：

    由题目可得知，链表是按照升序排列的，这意味着重复的元素都是紧紧排列在一起的，这大大减少了这道题的难度。

    我们遍历这个链表，比较当前的值和下一个节点的值是否想同，如果不相同就继续迭代到下一个元素，若出现相同的情况就将当前值记录下来，依次比较链表之后节点的值，直到出现不同的值或抵达链表尾部是才将指针指向这个节点。

- 代码：

    ```javaScript
    /**
     * Definition for singly-linked list.
     * function ListNode(val, next) {
     *   this.val = (val===undefined ? 0  : val)
     *   this.next = (next===undefined ? null : next)
     * }
     */
    /**
     * @param {ListNode} head
     * @return {ListNode}
     */
    var deleteDuplicates = function(head) {
      const temp = new ListNode(0,head)
      let cur = temp
      while(cur.next && cur.next.next){
        if(cur.next.val === cur.next.next.val){
          const x = cur.next.val
          while (cur.next && cur.next.val === x) {
            cur.next = cur.next.next;
          }
        }else{
          cur = cur.next
        }
      }
      return temp.next
    };
    ```

- 复杂度分析：

    `O(n)` 的时间复杂度，时间还算可以

    [![6XEVhT.png](https://z3.ax1x.com/2021/03/25/6XEVhT.png)](https://imgtu.com/i/6XEVhT)

## #面试题 17.21 直方图的水量 `Hard`

### 描述

- 简述：

    给定一个直方图(也称柱状图)，假设有人从上面源源不断地倒水，最后直方图能存多少水量?直方图的宽度为 1。

    [![cmnMCj.png](https://z3.ax1x.com/2021/04/02/cmnMCj.png)](https://imgtu.com/i/cmnMCj)

- 示例 1：

    ```shell
    # input -> [0,1,0,2,1,0,1,3,2,1,2,1]
    # output -> 6
    ```

### 题解

- 思路：

    这道题使用动态规范是相对常见的解法，但是动态规划会需要维护一个数组，导致空间复杂度变成 `O(n)`，那么有没有相对空间复杂度较低的解法呢？

    我们可以观察这个直方图，我们从左边到右边遍历一遍，我们可以得出，蓄水的地方一定是左右较高，中间低洼的。那么我们从左边到右边开始遍历，不断的去寻找左边中最高的元素，用最高的元素减去当前的高度，就能得出这一单元格的蓄水量。而且我们不断的去更新最高的高度，就意味着，我们不会出现负值的情况。

    看起来很完美的解法，只需要一次循环就可以得出答案，但是如果最高的单元格在直方图中央的话，那么我们越过之后的循环就会陷入错误之中。

    通过观察，我们发现，在一次循环中，在最高点左边的结果都是正确的，那么我们可以维护那个指针，一个从左边循环，一个从右边循环，当一个指针指向最高点的时候，就让这个指针停止行走，直到两个指针相遇，我们就可以得出答案。

    知道了思路，接下来就是代码实现，但是我们需要先找出最高点所在的位置。

- 代码：

    ```javaScript
    /**
    * @param {number[]} height
    * @return {number}
    */
    var trap = function(height) {
        let ans = 0;
        let left = 0, right = height.length - 1;
        let leftMax = 0, rightMax = 0;
        let stop = 0
        let max = 0
        for(let i = 0;i<height.length;i++){
            if(max < height[i]){
                max = height[i]
                stop = i
            }
        }
        while(left < right){
            leftMax = Math.max(leftMax, height[left]);
            rightMax = Math.max(rightMax, height[right]);
            if(left !== stop){
                ans += leftMax - height[left]
                left++
            }
            if(right !== stop){
                ans += rightMax - height[right]
                right--
            }
        }
        return ans;
    };
    ```

- 复杂度分析：

    我们需要先循环找出最高点的位置，之后两个指针需要进行循环，所以，时间复杂度为 `O(1.5n)`，空间复杂度为 O(1)

    [![cmisDH.png](https://z3.ax1x.com/2021/04/02/cmisDH.png)](https://imgtu.com/i/cmisDH)

## #80 删除有序数组的重复项 v2 `Medium`

### 描述

- 简介：

    给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 最多出现两次 ，返回删除后数组的新长度。

    不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

- 示例 1：

    ```shell
    # input ->  [1,1,1,2,2,3]
    # output -> 5
    # 解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3 。 不需要考虑数组中超出新长度后面的元素。
    ```

- 示例 2：

    ```shell
    # input -> [0,0,1,1,1,1,2,3,3]
    # output -> 7
    # 解释：函数应返回新长度 length = 7, 并且原数组的前五个元素被修改为 0, 0, 1, 1, 2, 3, 3 。 不需要考虑数组中超出新长度后面的元素。
    ```

### 题解

- 思路：

    这道题需要我们在原地对数组进行改变，最后返回新数组的长度，也就是说我们只需要考虑数组在达成题目要求后的长度，而不用去管原数组超出新数组长度的部分。

    由于元素最多不能出现两次，所以新数组的长度一定是小于等于原数组长度的，我们只需要在原数组的前 `res` 段进行修改就可以。

    针对与这道题，我们可以使用双指针的方法，一个快指针，一个慢指针，并将其的值初始化为 2 而不是 0。当快指针指向的数值不等于慢指针减去 2 位置的数值时，就将当前慢指针指向的数值改为快指针指向的数值，并且将慢指针移动。当两个数值相等时，只移动快指针。

    通过不断比较快慢指针的值，我们可以将重复次数大于二的元素只保留两个在数组的头部。

- 代码：

    ```javaScript
    /**
    * @param {number[]} nums
    * @return {number}
    */
    var removeDuplicates = function(nums) {
        if(nums.length <= 2){
            return nums.length
        }

        let fast = 2
        let slow = 2

        while(fast < nums.length){
            if(nums[slow - 2] !== nums[fast]){
                nums[slow] = nums[fast]
                slow++
            }
            fast++
        }
        return slow
    };
    ```

- 复杂度分析：

    使用快慢指针的方法，我们只需要一次遍历就可以得出结果，所以时间复杂度为 `O(n)`，此外我们也没有使用额外的空间，所以空间复杂度为 `O(1)`

    [![cl1XOU.png](https://z3.ax1x.com/2021/04/06/cl1XOU.png)](https://imgtu.com/i/cl1XOU)

## #剑指 Offer 62 圆圈中最后剩下的数字 `Easy`

### 描述

- 简介：

    0,1,···,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字（删除后从下一个数字开始计数）。求出这个圆圈里剩下的最后一个数字。

    例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

- 示例 1：

    ```shell
    # input -> n = 5, m = 3
    # output -> 3
    ```

- 示例 2：

    ```shell
    # input -> n = 10, m = 17
    # output -> 2
    ```

### 题解

- 思路

    本题所给的 `n` 可以看作一个从 `1 - n` 的循环列表，不断的去寻找 `m` 个之后的元素进行删除，最终肯定只有一个元素能够留下来。

    注意，这一题看似简单，但是想要写出完美解法的代码，还是有一定难度。

    按照我们的传统思路，我们可以将其模拟成循环链表，然后进行删值操作，但是这样回到导致其时间复杂度为 `O(mn)`，*一切时间复杂度能做到 O(n) 却没有做到的算法都是不完美的算法。* 这句话是我胡说的。

    那么有没有一种方法能够做到 `O(n)` 的时间复杂度呢？

    当然有！这道题有数学解法，此题是一道经典的数学问题，约瑟夫环问题。

    > 这个问题是以弗拉维奥·约瑟夫命名的，他是1世纪的一名犹太历史学家。他在自己的日记中写道，他和他的40个战友被罗马军队包围在洞中。他们讨论是自杀还是被俘，最终决定自杀，并以抽签的方式决定谁杀掉谁。约瑟夫斯和另外一个人是最后两个留下的人。约瑟夫斯说服了那个人，他们将向罗马军队投降，不再自杀。约瑟夫斯把他的存活归因于运气或天意，他不知道是哪一个。 —— 【约瑟夫问题】维基百科

    我们将 `n` 模拟为一个 `1 到 n` 的递增数组，那么我们不断的删除数组中的元素，那么最后剩下的那个答案在数组中的位置为 `0`,那么我们以此倒推，当数组中剩下两个元素时，那么这个元素需要在哪个位置上，才能让其留在最后呢？不难推理，当 `m` 的值为 3 的时候，数组下标为 1 的元素能够留到最后。

    由上面的递推，我们能推导出一个公式：

    ```shell
    # lastIndex -> 上一步的坐标
    # i -> 上一步的长度
    # currentIndex -> 当前坐标
    # 公式 -> lastIndex = (currentIndex + m) % i
    ```

    由此公式，我们能一层层的递推到最后一个数在初始数组中的下标。

- 代码：

    ```javaScript
    /**
    * @param {number} n
    * @param {number} m
    * @return {number}
    */
    var lastRemaining = function(n, m) {
        let res = 0
        for(let i = 2;i<=n;i++){
            res = (res + m) % i
        }
        return res // 这里 return res 是因为在原数组中，下标就是实际的值
    };
    ```

- 复杂度分析：

    对于时间复杂度来说，我们只循环了 `n - 2` 次，所以时间复杂度为 `O(n)`，同时我们只是用了常数变量，所以空间复杂度为 `O(1)`

    [![cltwOP.png](https://z3.ax1x.com/2021/04/06/cltwOP.png)](https://imgtu.com/i/cltwOP)

## #154 寻找旋转排序数组中的最小值 v2 `Hard`

### 描述

- 简介：

    `2021-04-09` 每日一题

    已知一个长度为 n 的数组，预先按照升序排列，经由 1 到 n 次 旋转 后，得到输入数组。例如，原数组 nums = [0,1,4,4,5,6,7] 在变化后可能得到：

    - 若旋转 4 次，则可以得到 [4,5,6,7,0,1,4]
    - 若旋转 7 次，则可以得到 [0,1,4,4,5,6,7]

    注意，数组 [a[0], a[1], a[2], ..., a[n-1]] 旋转一次 的结果为数组 [a[n-1], a[0], a[1], a[2], ..., a[n-2]] 。

    给你一个可能存在 重复 元素值的数组 nums ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 最小元素 。

- 示例 1：

    ```shell
    # input -> [1,3,5]
    # output -> 1
    ```

- 示例 2：

    ```shell
    # input -> [2,2,2,0,1]
    # output -> 0
    ```

### 题解

- 思路：

    开始之前先吐槽一句，从 `2021-04-07` 到 `2021-04-09` 连续三天出了三道旋转排序数组的问题，真的很诡吊。

    *数组找最小值问题，为什么不用最棒的 `O(n)` 遍历呢？非得折腾二分法。*

    已知数组是排序数组，将数组进行旋转后，我们可以知道，在最小值左边的所有的值都是要大于最小值右边的所有值。我们使用二分法，将指针指向数组中央，同时我们比较中间的值和左右两边值的大小，我们可以得到六种情况。

    在罗列情况之前，我们定义如下规则：

    ```shell
    # leftArr -> 最小值左边的子数组
    # rightArr -> 最小值右边的子数组
    # left -> 指向最左边的指针，初始值为 0
    # right -> 指向最右边的指针，初始值为数组长度。
    # index -> 二分法指针
    ```

    - 中间值大于最右边的值：

        此时我们可以知道，我们一直最小值右边的子数组是递增关系，此时 `index` 指向的位置一定是在最小值的左边，此时，我们可以 `left` 指针移动到 `index` 的位置。

    - 中间值小于最右边的值：

        当 `index` 的值要小于 `right` 的值时，我们根据 `rightArr` 的递增关系可以知道，`index` 右边的所有元素都会大于等于它，所以我们可以将 `right` 指针移动到 `index + 1` 的位置（ps: 这里的 `+1` 非常关键，不能让 `right` 直接等于 `index`，否则会导致无法找到最小值的情况。）

    - 中间值大于最左边的值：

        没什么好说的，直接将 `left` 移动到 `index`

    - 中间值小于最左边的值：

        也是直接将 `left` 改到 `index` 的位置，这里无需同第二种情况一样去做 `-1` 操作，因为在计算二分时，我们采用向下取整的方法，所以即使 `left` 指针指向了最小的值，也不会被忽略。

    - 相等情况：

        与 `left` 相等时，我们无需考虑，同上。与 `right` 相等时，我们需要注意，我们不能直接将 `right` 移动到 `index`，因为这道题之所以不同于前两道题目，作为困题的原因就是其数组的元素不唯一，这会导致在旋转的时候，有一部分相同的元素会旋转到 `leftArr` 中，所以如果我们直接改变 `right` 的话，我们就是永远找不到最小值了。解决办法也很简单，我们仅仅将 `right` 指针向左移动一个单位即可。

- 代码：

    ```javaScript
    /**
    * @param {number[]} nums
    * @return {number}
    */
    var findMin = function(nums) {
        let max = 0
        let min = nums.length - 1
        while(max < min){
            let p = max + Math.floor((min - max) / 2)
            if(nums[p] < nums[min]){
                min = p
            }else if(nums[p] > nums[min]){
                max = p + 1
            }else{
                min = min - 1
            }
        }
        return nums[max]
    };
    ```

- 复杂度分析：

    平均时间复杂度为 `O(n)`, n 为数组的长度。

    [![cNvIRe.png](https://z3.ax1x.com/2021/04/09/cNvIRe.png)](https://imgtu.com/i/cNvIRe)
