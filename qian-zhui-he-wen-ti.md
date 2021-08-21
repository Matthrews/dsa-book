# 前缀和问题

### 什么是前缀和？

一维前缀和

假设`x`为一数组，`y`为数组`x`的前缀和数组，则`x`和`y`满足如下关系:

$$
y_{0\ }=\ x_0\ ,\ y_1\ =x_0\ +\ \ x_1\ ,\ y_2\ =x_0\ +\ \ x_1\ +\ \ x_2 ,  y_n\ = x_0\ +\ \ x_1\ +\ \ x_2 \ +\ ... \ +\ x_n
$$

二维前缀和

假设`x`为二维数组，`y`为数组`x`的二维前缀和数组，则`x`和`y`满足如下关系:

$$
y_{0,0}=x_{0,0}, 
 y_{0,1}=x_{0,0}+a_{0,1}, 
 y_{1,0}=x_{0,0}+x_{1,0}, 
 y_{1,1}=x_{0,0}+x_{0,1}+x_{1,0}+x_{1,1}\dots
$$

![&#x4E8C;&#x4F4D;&#x524D;&#x7F00;&#x548C;&#x6570;&#x7EC4;&#x793A;&#x610F;&#x56FE;](.gitbook/assets/1629547646-1-%20%281%29.jpg)

如上图，右侧二位前缀和数组标红元素 $$y_{2, 2}$$ 的值等于左侧二维数组标红区域元素之和

### 如何计算

一维前缀和

$$
y_n=y_{n-1}+x_n
$$

代码实现

```text
for (let i = 0; i < n; i++) {
    if (i === 0) {
        y[i] = x[i]
    } else {
        y[i] = y[i - 1] + x[i]
    }
}
```

二位前缀和



$$
y_{i, j}=y_{i-1,j}+y_{i,j-1}-y_{i-1,j-1}+x_{i,j}
$$

代码实现

```text
// n * m
for (let i = 0; i < n; i++) {
    for (let j = 0; j < m; j++) {
        if (i === 0 && j === 0) {
            y[j][i] = x[j][i]
        } else if (j === 0) {
            y[j][i] = y[j][i - 1] + x[j][i]  // 一维数组前缀和
        } else if (i === 0) {
            y[j][i] = y[j - 1][i] + x[j][i]  // 一维数组前缀和
        } else {
            y[j][i] = y[j - 1][i] + y[j][i - 1] - y[j - 1][i - 1] + x[j][i]
        }
    }
}
```

### 前缀和应用

[参考链接](https://zhuanlan.zhihu.com/p/375675761)

* 连续子数组求和问题

> 给你一个正整数数组 arr ，请你计算所有可能的奇数长度子数组的和

```text
let preSum = [arr[0]], res = 0, n = arr.length;
for (let i = 1; i < n; i++) {
    preSum[i] = preSum[i - 1] + arr[i]
}
preSum.unshift(0)
console.log('preSum', preSum)

for (let i = 0; i < n; i++) {
    for (let j = 1; i + j - 1 < n; j += 2) {
        // console.log('ele', arr.slice(i, i + j))
        // i = 0的时候，preSum[0]是人为补上的
        res += preSum[i + j] - preSum[i]  
    }
}
return res;
```



