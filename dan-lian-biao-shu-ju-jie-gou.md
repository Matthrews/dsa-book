# 单链表数据结构

1. 单链表节点定义

```text
export class Node {
    public ele: any;
    public next: null;

    constructor(ele) {
        this.ele = ele;
        this.next = null;
    }
}
```

2. 单链表基本操作

* 插入  \`insert\(ele, pos\)\`  
* 查询  \`get\(pos\)\`
* 删除  \`remove\(pos\)\`
* 获取头节点  \`getHead\(\)\`
* 获取链表长度  \`getSize\(\)\`
* 判断链表是否为空  \`isEmpty\(\)\`
* 获取链表节点数组  \`getList\(\)\`

3. 基本操作实现

```text
export class LinkedList {
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

