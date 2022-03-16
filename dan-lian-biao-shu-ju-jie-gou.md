# 单链表问题

1. 单链表节点定义

```
class Node {
    public ele: any;
    public next: null;

    constructor(ele) {
        this.ele = ele;
        this.next = null;
    }
}
```

2\. 单链表基本操作

* 插入  \`insert(ele, pos)\` &#x20;
* 查询  \`get(pos)\`
* 删除  \`remove(pos)\`
* 获取头节点  \`getHead()\`
* 获取链表长度  \`getSize()\`
* 判断链表是否为空  \`isEmpty()\`
* 获取链表节点数组  \`getList()\`

3\. 基本操作实现

```
class LinkedList {
    private head: any;
    private length: number;

    constructor() {
        this.head = null;
        this.length = 0;
    }

    /**
     * 查询指定位置节点
     * @param pos 待搜索节点位置
     * @returns {Node} 找到返回节点,否则返回-1
     */
    get(pos) {
        if (pos < 0 || pos > this.length) {
            console.warn("索引位置非法！");
            return -1;
        }
        let tmp = this.head,
            index = 0;
        while (index++ < pos) {
            tmp = tmp.next;
        }
        return tmp;
    }

    /**
     * 追加节点
     * @param {Node} ele 待插入节点
     * @returns {Boolean} 插入成功返回true，否则false
     */
    append(ele) {
        let newNode = new Node(ele);

        if (!this.head) {
            this.head = newNode;
        } else {
            let tmp = this.head;
            while (tmp.next) {
                tmp = tmp.next;
            }
            tmp.next = newNode;
        }
        this.length++;
        return true;
    }

    /**
     * 指定位置插入节点
     * @param {Node} ele 待插入节点
     * @param {Number} pos 待插入位置  默认为length
     * @returns {Boolean} 插入成功返回true，否则false
     */
    insert(ele, pos?) {
        let validPos = pos ?? this.length; // pos为null/undefined是取右边的值
        if (validPos < 0 || validPos > this.length) {
            console.warn("插入位置非法！");
            return false;
        }

        let newNode = new Node(ele),
            tmp = this.head,
            index = 0,
            prev;

        if (validPos == 0) {
            newNode.next = tmp;
            // 更新头节点
            this.head = newNode;
        } else {
            while (index++ < validPos) {
                prev = tmp;
                tmp = tmp.next;
            }
            // console.log('test', prev, tmp);
            // newNode要插在prev和tmp之间
            // newNode下一个指向tmp
            // prev下一个指向newNode
            // 从而把链条拼接上
            newNode.next = tmp;
            prev.next = newNode;
        }
        this.length++;
        return true;
    }

    /**
     * 指定位置删除节点
     * @param {Number} pos 待删除位置
     * @returns {Boolean} 删除成功返回true，否则false
     */
    remove(pos) {
        if (pos < 0 || pos > this.length) {
            console.warn("删除位置非法！");
            return false;
        }

        // 找到要删除节点以及其之前的节点
        let tmp = this.head,
            index = 0,
            prev;
        if (pos === 0) {
            this.head = this.head.next;
        } else {
            while (index++ < pos) {
                prev = tmp;
                tmp = tmp.next;
            }
            // console.log('test', prev, tmp);
            // 直接跳过tmp,和tmp的下一个节点拼接
            prev.next = tmp.next;
        }
        this.length--;
        return true;
    }

    /**
     * 链表是否为空
     * @returns {Boolean} 链表为空返回true,否则false
     */
    isEmpty() {
        return this.length === 0;
    }

    /**
     * 获取头节点
     * @returns {Node} 返回头节点
     */
    getHead() {
        return this.head;
    }

    /**
     * 获取链表长度
     * @returns {Number} 返回链表长度
     */
    getSize() {
        return this.length;
    }

    /**
     * 获取链表元素数组
     * @returns {Array} 返回链表元素数组
     *
     * TODO 需要考虑环状链表
     */
    getList() {
        const ret = [];
        let cur = this.head;
        while (cur.next) {
            ret.push(cur.ele);
            cur = cur.next;
        }
        ret.push(cur.ele);
        return ret;
    }
}
```

4\. 单链表高级操作

4\. 1 链表合并

> 将两个升序链表合并为一个新的升序链表并返回，新链表是通过拼接给定的两个链表的所有节点组成的

```
const mergeTwoList = (r1, r2) => {
    if (r1 === null) return r2;
    if (r2 === null) return r1;

    if (r1.ele <= r2.ele) {
        // 指针后移
        r1.next = mergeTwoList(r1.next, r2)
        return r1
    } else {
        r2.next = mergeTwoList(r2.next, r1)
        return r2
    }
}
```

4\. 2 链表环检测

> 给定一个链表，判断链表中是否有环

方法1： 标记法

遍历链表，给已遍历过的节点加标志位

```
const hasCycle = (r) => {
     while (r) {
         if (r.visited) return true
         r.visited = true
         r = r.next;
     }
     return false
}
```

方法2：\`JSON.stringify\`

\`JSON.stringify\`序列化含有循环引用结构会报错

```
const hasCycle = (r) => {
     try {
         JSON.stringify(r);
         return false
     } catch (e) {
         return true
     }
}
```

方法3：双指针

快慢指针: 遍历单链表，快指针一次走两步，慢指针一次走一步

如果单链表中存在环，则快慢指针终会指向同一个节点，否则直到快指针指向 null 时，快慢指针都不可能相遇

```
const hasCycle = (r) => {
    if (!r || !r.next) return false
    let fast = r.next.next, slow = r.next;

    while (fast !== slow) {
        if (!fast || !fast.next) return false
        fast = fast.next.next
        slow = slow.next
    }
    // 如需找到环入口只需新建一个指针tmp和slow指针同时向前遍历，当两者相等时，入口就是tmp
    return true
}
```

4.3 链表中间元素

> 求链表的中间结点，节点个数为偶数2n时返回n+1所在节点

快慢指针: 遍历单链表，快指针一次走两步，慢指针一次走一步

快指针走到尾节点的时候就是满指针走到中间的时候

```
const getMiddle = (r) => {
    if (!r || !r.next) return r;
    let fast = r, slow = r;
    while (fast && fast.next) {
        fast = fast.next.next;
        slow = slow.next
    }
    return slow
}
```

4.4 链表反转

方法1： 迭代法(三指针法)

使用三个指针prev, cur和next实现

next在遍历链表的时候临时保存cur的下一个节点

prev初始值为null，遍历链表的时候反转cur的指向到prev

prev和cur分别前移

```
const reverse = (r) => {
    if (!r || !r.next) return r;
    let prev = null, cur = r;
    while (cur) {
        // 用于临时存储 cur 后继节点
        // 否则下一步就会抹掉原来的cur.next
        let next = cur.next;  // 4

        // 反转 cur 的后继指针
        cur.next = prev;  // 首个节点的next就变为了null

        // 变更prev、cur
        prev = cur;  // prev前移
        cur = next  // cur前移
    }
    r = prev;
    return r;
}
```

方法2： 递归

```
const reverse = (r) => {
    const _reverse = function (prev, cur) {
        if (!cur) return prev  // 前移的时候cur为null就结束了
        // 临时存储
        let next = cur.next;
        // 核心反转
        cur.next = prev;
        return _reverse(cur, next)  // prev,cur => cur, next
    }

    if (!r || !r.next) return r;

    r = _reverse(null, r);
    return r;
}
```

4.5 链表求和

> 给你两个非空 的链表，表示两个非负的整数。它们每位数字都是按照逆序的方式存储的，并且每个节点只能存储一位数字。\
> &#x20;请你将两个数相加，并以相同形式返回一个表示和的链表。

```
const addTwoNumbers = (l1, l2) => {
    let carry = 0;
    let head = null, tail = null;  // 当carry>0时，tail用于表示进位

    while (l1 || l2) {
        let n1 = l1 ? l1.ele : 0;
        let n2 = l2 ? l2.ele : 0;
        let sum = n1 + n2 + carry;

        if (!head) {
            head = tail = new Node(sum % 10);
        } else {
            tail.next = new Node(sum % 10);
            tail = tail.next
        }

        carry = (sum / 10) >> 0

        // console.log('ele', sum, l1.ele, l2.ele)
        if (l1) {
            l1 = l1.next;
        }
        if (l2) {
            l2 = l2.next;
        }
    }

    // 如果有进位，附加一个新节点，其值为carry
    if (carry) {
        tail.next = new Node(carry)
    }
    return head
}
```

4.6 回文链表

> 给你一个单链表的头节点 head ，请你判断该链表是否为回文链表。

方法1：遍历链表将节点值放入数组，然后双指针\
复杂度：O(n) O(n)

```
var isPalindrome = function (head) {
  const arr = [];
    while (head) {
      arr.push(head.val);
      head = head.next;
    }
    let i = 0,
      len = arr.length,
      j = len - 1;
    for (i, j; i < j; i++, j--) {
      if (arr[i] !== arr[j]) {
        return false;
      }
    }
    return true;
};
```

方法2：前半部分指针反转一下，然后再判断回文\
复杂度：O(n) O(1)

```
var isPalindrome = function (head) {
  let slow = head,
    fast = head;
  let prev = null,
    next = null;

  while (fast !== null && fast.next !== null) {
    fast = fast.next.next;
    next = slow.next;
    // slow 边走边逆序
    slow.next = prev;
    prev = slow;
    slow = next;
  }

  if (fast !== null) {
    // 链表长度为奇数
    slow = slow.next;
  }

  while (prev !== null && slow !== null) {
    if (prev.val !== slow.val) {
      return false;
    }

    prev = prev.next;
    slow = slow.next;
  }
  return true;
};
```

4.7 链表排序

> 给你链表的头结点 head ，请将其按 升序 排列并返回 排序后的链表 。

```
var sortList = function (head) {
  // 自顶向下归并排序
  // 1. 找到链表的中点，以中点为分界，将链表拆分成两个子链表。
  //      寻找链表的中点可以使用快慢指针的做法，快指针每次移动 22 步，慢指针每次移动 11 步，当快指针到达链表末尾时，慢指针指向的链表节点即为链表的中点
  // 2. 对两个子链表分别排序。
  // 3. 将两个排序后的子链表合并，得到完整的排序后的链表

  return toSortList(head, null);
};
var toSortList = function (head, tail) {
  if (!head || !head.next) {
    return head;
  }

  if (head.next === tail) {
    head.next = null;
    return head;
  }
  //   let slow = head.next;
  //   let fast = head.next.next;
  //   while (fast && fast.next && fast !== tail) {
  //     slow = slow.next;
  //     fast = fast.next.next;
  //   }
  let slow = head,
    fast = head;
  while (fast !== tail) {
    slow = slow.next;
    fast = fast.next;
    if (fast !== tail) {
      fast = fast.next;
    }
  }

  const mid = slow;
  return mergeTwoList(toSortList(head, mid), toSortList(mid, tail));
};

```
