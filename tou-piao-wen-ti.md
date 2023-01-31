# 投票问题

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

方法1:  Map

```
时间复杂度：O(n), 空间复杂度：O(n)
```

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

方法2: 摩尔投票法

```
时间复杂度：O(n), 空间复杂度：O(1)
```

```javascript
var majorityElement = function (nums) {
  let middle = nums.length >> 1;
  let candidate,
    counter = 0;

  for (let i = 0, len = nums.length; i < len; i++) {
    if (counter === 0) {
      candidate = nums[i];
      counter++;
    } else {
      if (nums[i] === candidate) {
        counter++;
      } else {
        counter--;
      }
    }
  }

  if (counter) {
    counter = 0; // 清零用于统计candidate票数
    for (let i = nums.length - 1; i >= 0; i--) {
      if (nums[i] === candidate) counter++;
    }
    if (counter > middle) {
      return candidate;
    }
  }
  // console.log('candidate ', candidate, counter);
  return -1; // 不存在
};

console.assert(majorityElement([1, 2, 5, 9, 5, 9, 5, 5, 5]) === 5);
console.assert(majorityElement([3, 2]) === -1);
console.assert(majorityElement([2, 2, 1, 1, 1, 2, 2]) === 2);
```
