---
description: LeetCode 80. 删除有序数组中的重复项 II
---

# 移除有序数组中重复项

```
给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 最多出现两次 ，返回删除后数组的新长度。
不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。code
```

```javascript
var removeDuplicates = function (nums) {
    let k = 2;
    return process(nums, k)
}

function process(nums, k) {
    let ans = 0;
    let len = nums.length;
    if (len < 2) return len;

    for (let i = 0; i < len; i++) {
        if (ans < k || nums[ans - k] !== nums[i]) {
            nums[ans++] = nums[i]
        }
    }
    return ans
}
```
