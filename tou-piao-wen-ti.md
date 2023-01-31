# 投票问题

我们先来看一个题目。

```
给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 最多出现两次 ，返回删除后数组的新长度。
不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。


示例 1：

输入：[1,2,5,9,5,9,5,5,5]
输出：5
示例 2：

输入：[3,2]
输出：-1
示例 3：

输入：[2,2,1,1,1,2,2]
输出：2

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/find-majority-element-lcci
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
```

看到这道题，我知道很小多小伙伴很快就能写出来了

```javascript
var majorityElement = function (nums) {
  let middle = nums.length >> 1;
  let map = new Map();

  for (let i = nums.length - 1; i >= 0; i--) {
    if (!map.has(nums[i])) {
      map.set(nums[i], 1);
    } else {
      map.set(nums[i], map.get(nums[i]) + 1);
    }
  }

  for (const [key, value] of map) {
    if (value > middle) return key;
  }
  return -1;
};

console.assert(majorityElement([1, 2, 5, 9, 5, 9, 5, 5, 5]) === 5);
console.assert(majorityElement([3, 2]) === -1);
console.assert(majorityElement([2, 2, 1, 1, 1, 2, 2]) === 2);
```

