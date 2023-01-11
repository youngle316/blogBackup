# Day 01 二分查找与移除元素

## Leetcode 704 二分查找

题目链接： [704\. 二分查找](https://leetcode.cn/problems/binary-search/)

这题写过，但是再次看到还是会有点犹豫，这开闭区间当时是怎么写的来着。也暴露了自己没有真正的理解，只是为了 AC 就行。

报这个课就好好的记笔记，要每一个细节都搞懂。

**过程**：二分查找的前提是数组是**有序排列**的，并且**没有**重复元素方可使用二分查找

有两种方式**左闭右闭**和**左闭右开**。

先上左闭右闭的代码

```typescript
function search(nums: number[], target: number): number {
	let left = 0;
	let right = nums.length - 1;
	while (left <= right) {
		const center = left + Math.floor((right - left) / 2);
		if (target > nums[center]) {
			left = center + 1;
		} else if (target < nums[center]) {
			right = center - 1;
		} else {
			return center;
		}
	}
	return -1;
};
```

由于是左闭右闭，两边的值都会取到。当 `target` 是最左或最右的值时， `left === right` 是有意义的。中间值取地板值，当 `target` 小于 `center` 时，就需要去 `center` 的左边去找，就需要改变 `right`，由于是右闭，`center` 当前已经比较过了，所以 `right = center - 1`。`left` 同理

而左闭右开的情况，`center` 就不需要 -1 了

具体步骤：

1. 确定左右取值范围，即左右开闭区间（选择左闭右闭）
    
2. 循环条件（左闭右闭，`left <= right`）
    
3. 确定中间值
    
4. 中间值与 `target` 的大小比较
    
5. 未找到修改区间
    
6. 找到返回下标
    
7. 未找到返回 -1
    

## Leetcode 27 移除元素

### 暴力解法

不使用额外数组原地修改数组。不能正序遍历数组，删除元素后，会导致数组长度改变出现错误。

只需要倒序遍历数组即可。

```typescript
function removeElement(nums: number[], val: number): number {
	for (let i = nums.length - 1; i >= 0; i --) {
		if (nums[i] === val) {
			nums.splice(i, 1)
		}
	}
	return nums.length;
};
```

### 双指针

可以使用双指针中的快慢指针

快指针：按顺序遍历整个数组 慢指针：指向更新后的数组下标

```typescript
function removeElement(nums: number[], val: number): number {
let slowIndex = 0;
	for (let fastIndex = 0; fastIndex < nums.length; fastIndex++) {
		if (nums[fastIndex] !== val) {
			nums[slowIndex++] = nums[fastIndex]
		}
	}
	return slowIndex;
};
```

![27.移除元素-双指针法.gif](https://obsidian-picgo-le.oss-cn-hangzhou.aliyuncs.com/img/27.%E7%A7%BB%E9%99%A4%E5%85%83%E7%B4%A0-%E5%8F%8C%E6%8C%87%E9%92%88%E6%B3%95.gif align="left")