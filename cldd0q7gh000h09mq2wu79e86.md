# Day 12 栈与队列 - 滑动窗口最大值

## 239\. 滑动窗口最大值

```typescript
function maxSlidingWindow(nums: number[], k: number): number[] {
	const result = [];
	const queue = [];
	for (let i = 0; i < nums.length; i++) {
		// 队列存在，当前的值比队尾的值大，就将队尾的值 pop 出去（将最大的值放在队首）
		while (queue.length && nums[i] >= nums[queue[queue.length - 1]]) {
			queue.pop();
		}
		// 将值的索引 push 到队列中
		queue.push(i);

		// 当队首的值不在滑动窗口时，需要将其 shift 出去
		if (queue[0] <= i - k) {
			queue.shift();
		}

		// 如果当前的索引满足了滑动窗口的 k 时，将队首的值添加到结果集中
		if (i >= k - 1) {
			result.push(nums[queue[0]]);
		}

	}
	return result;
};
```

## 347\. 前 K 个高频元素

```typescript
function topKFrequent(nums: number[], k: number): number[] {
	const map = new Map();
	for (let num of nums) {
		map.set(num, (map.get(num) || 0) + 1)
	};
	
	return [...map.entries()]
		.sort((a, b) => b[1] - a[1])
		.slice(0, k)
		.map((item) => item[0]);
};
```