# Day 04 两两交换链表中的节点

## 24\. 两两交换链表中的节点

**思路**：还是使用虚拟头节点进行节点的交换

**注意点**：

1. 循环结束的条件：当是奇数时，`cur.next.next`为`null`就停止；当是偶数时，`cur.next`为`null`就停止。
    
2. 交换时需要提前对指向 1 和 3 的指针进行存储
    

```typescript
function swapPairs(head: ListNode | null): ListNode | null {
	if (!head || !head.next) {
		return head;
	};
	const listNode = new ListNode(0, head);
	let cur = listNode;
	while (cur.next && cur.next.next) {
		let temp = cur.next;
		let temp2 = cur.next.next.next
		cur.next = cur.next.next;
		cur.next.next = temp;
		cur.next.next.next = temp2;
		cur = cur.next.next;
	}
	return listNode.next;
};
```

## 19\. 删除链表倒数的第 N 个节点

思路：

1. 创建虚拟头节点计算出链表的长度
    
2. 正着数要删除的节点为 `length - n + 1`
    
3. 遍历到要删除节点的前一个节点，将该节点的 `next` 指向 [`next.next`](http://next.next)
    
4. 返回链表的头节点
    

```typescript
function removeNthFromEnd(head: ListNode | null, n: number): ListNode | null {
	let listNode = new ListNode(0, head);
	let cur = listNode;
	let listLength = 0;
	let index = 0;
	while (cur.next) {
		listLength++;
		cur = cur.next;
	}
	index = listLength - n + 1;
	let newCur = listNode;
	for (let i = 1; i <= index; i++) {
		if (i === index) {
			newCur.next = newCur.next.next;
		}
		newCur = newCur.next
	}
	return listNode.next;
};
```

## 面试题 02.07. 链表相交

思路：

1. 获取A，B的长度
    
2. 将A作为较长的链表
    
3. 将A，B末尾对其
    
4. 遍历A，B是否有相等的点
    
5. 返回A
    

```javascript
var getIntersectionNode = function(headA, headB) {
	const getLength = (head) => {
		let length = 0;
		let cur = head;
		while (cur) {
			length++;
			cur = cur.next;
		}
		return length;
	}
	let lengthA = getLength(headA);
	let lengthB = getLength(headB);
	let curA = headA;
	let curB = headB;
	if (lengthA < lengthB) {
		[curA, curB] = [curB, curA];
		[lengthA, lengthB] = [lengthB, lengthA];
	};
	let diffLength = lengthA - lengthB;
	while (diffLength--) {
		curA = curA.next;
	};
	while (curA && (curA !== curB)) {
		curA = curA.next;
		curB = curB.next;
	}
	return curA;
};
```

## 142\. 环形链表 二

![IMG_EAEC64B4C5F0-1.jpeg](https://obsidian-picgo-le.oss-cn-hangzhou.aliyuncs.com/img/IMG_EAEC64B4C5F0-1.jpeg align="left")

通过数学推导出 `x=z`，

思路：

1. 通过快慢指针，判断是否有相等的节点
    
2. 如果有相等的节点，将慢节点设置为 `head`；
    
3. 快慢指针再每次移动一，相等时，就是入口节点
    

```typescript
/**
* @param {ListNode} head
* @return {ListNode}
*/
var detectCycle = function(head) {
	if (!head || !head.next) {
		return null;
	}
	let fast = head.next.next;
	let slow = head.next;
	while (fast && fast.next?.next) {
		slow = slow.next;
		fast = fast.next.next;
		if (slow === fast) {
			slow = head;
			while (slow !== fast) {
				slow = slow.next;
				fast = fast.next
			}
			return slow;
		}
	}
	return null;
};
```