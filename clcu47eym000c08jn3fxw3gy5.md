# Day 03 移除链表元素-设计链表-反转链表

## 链表的定义

```typescript
Class ListNode {
	val: number
	next: ListNode | null
	constructor(val:? number, next?: ListNode | null) {
		this.val = (val === undefined ? 0 : val)
		this.next = (next === undefined ? null : next)
	}
}
```

## 203\. 移除链表元素

[题目链接](https://leetcode.cn/problems/remove-linked-list-elements/)

简单题，只需要修改 `next` 的指向即可。

JS 中没有链表，开始写的时候不知道该返回什么，返回 `listNode` 还是 [`listNode.next`](http://listNode.next)

使用虚拟头节点

```typescript
/**
* Definition for singly-linked list.
* class ListNode {
*   val: number
*   next: ListNode | null
*   constructor(val?: number, next?: ListNode | null) {
*     this.val = (val===undefined ? 0 : val)
*     this.next = (next===undefined ? null : next)
*   }
* }
*/
  

function removeElements(head: ListNode | null, val: number): ListNode | null {
  // 虚拟的头节点
	const listNode = new ListNode(0, head);
	let cur = listNode;
	while (cur.next) {
		if (cur.next.val === val) {
			cur.next = cur.next.next
		} else {
			cur = cur.next
		}
	}
	return listNode.next;
};
```

## 707\. 设计链表

一道题包含了很多内容，JS 的类也复习了一遍，不论是删还是增，都有很多情况需要去考虑

```typescript
class NewListNode {
	val: number;
	next: NewListNode | null;
	constructor(val?: number, next?: NewListNode | null) {
		this.val = val === undefined ? 0 : val;
		this.next = next === undefined ? null : next;
	}
} 
class MyLinkedList {
	// 链表长度
	private length: number;
	// 链表头部
	private head: NewListNode | null;
	// 链表尾部
	private tail: NewListNode | null;
	constructor() {
		this.length = 0;
		this.head = null;
		this.tail = null;
	}

	get(index: number): number {
		if (index < 0 || index >= this.length) {
			return -1;
		}
		const curNode = this.getNode(index);
		return curNode.val;
	} 

	addAtHead(val: number): void {
		let newListNode = new NewListNode(val, this.head);
		this.head = newListNode;
		if (!this.tail) {
			this.tail = newListNode;
		}
		this.length++;
	}

	addAtTail(val: number): void {
		let newListNode = new NewListNode(val, this.tail);
		if (this.tail) {
			this.tail.next = newListNode;
		} else {
			this.head = newListNode;
		}
		this.tail = newListNode;
		this.length++;
	}

	addAtIndex(index: number, val: number): void {
		// 添加到尾部
		if (index === this.length) {
		this.addAtTail(val);
			return;
		}
		if (index > this.length) {
			return;
		}
	
		// 小于零的都添加到头部
		if (index <= 0) {
			this.addAtHead(val);
			return;
		}
		// 插入第 index 节点之前，需要减一
		const curNode = this.getNode(index - 1);
		let newListNode = new NewListNode(val, curNode?.next || null);
		curNode.next = newListNode;
		this.length++;
	}

	deleteAtIndex(index: number): void {
		if (index < 0 || index >= this.length) {
			return;
		}
		// 处理头节点
		if (index === 0) {
			this.head = this.head?.next || null;
			// 如果链表中只有一个元素，删除头节点后，需要处理尾节点
			if (index === this.length - 1) {
				this.tail = null;
			}
			this.length--;
			return;
		}
		let curNode = this.getNode(index - 1);
		curNode.next = curNode.next?.next || null;
		
		// 处理尾节点
		if (index === this.length - 1) {
			this.tail = curNode;
		}
		this.length--;
	}

	private getNode(index: number): NewListNode {
		let newListNode = new NewListNode(0, this.head);
		for (let i = 0; i <= index; i++) {
			newListNode = newListNode!.next;
		}
		return newListNode;
	}
}
```

## 206\. 反转链表

使用双指针法

```typescript
function reverseList(head: ListNode | null): ListNode | null {
	if (!head || !head.next) {
		return head;
	}
	let cur = head;
	let pre = null;
	let temp = null;
	while (cur) {
		temp = cur.next;
		cur.next = pre;
		pre = cur;
		cur = temp;
	}
	return pre;
};
```