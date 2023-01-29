# 斐波那契数列问题

1. 使用递归实现

```javascript
function fib(n) {
  if ( n <= 1 ) {return 1};

  return fib(n - 1) + fib(n - 2);
}
```

> 时间复杂度为 O(2^n)，空间复杂度取决于栈的深度

2\. 使用动态规划实现

```javascript
function fib(n) {
  if (Number.isInteger(n) === true) {
    let a = [];
    if (n <= 0) {
      return -1;
    } else {
      a[0] = a[1] = 1;
      a[2] = 2;
      for (let i = 3; i < n + 1; i++) {
        a[i] = a[i - 1] + a[i - 2];
      }
    }
    return a[n];
  }
}

```

> 时间复杂度为 O(n)，空间复杂度为 O(n)

3\. 使用两个变量实现

```javascript
function fib(n) {
			if (Number.isInteger(n) === true) {
				let a,b,c
				if (n <= 0) {
					return -1
				} else {
					a = b = 1
					for (let i = 3; i < n + 1; i++) {
						c = a + b
						a = b
						b = c
					}
				}
				return c
			}
		}
```

> 时间复杂度为 O(n)，空间复杂度为 O(1)

4\. 使用ES6尾递归优化

> 严格模式下才会起作用

```javascript
// n代表序列数，ac1代表上一次序列之和，ac2代表本次序列之和
function fib(n , ac1 = 0 , ac2 = 1) {
  if( n === 0 ) return ac1;

  return fib(n - 1, ac2, ac1 + ac2);
}
// fib(5, 0, 1)
```

> 参考：[https://es6.ruanyifeng.com/#docs/function#%E5%B0%BE%E8%B0%83%E7%94%A8%E4%BC%98%E5%8C%96](https://es6.ruanyifeng.com/#docs/function#%E5%B0%BE%E8%B0%83%E7%94%A8%E4%BC%98%E5%8C%96)
>
> 参考：[https://cloud.tencent.com/developer/article/1694405](https://cloud.tencent.com/developer/article/1694405)

