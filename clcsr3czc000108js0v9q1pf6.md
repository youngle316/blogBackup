# Day 02 有序数组的平方-长度最小的子数组-螺旋矩阵 二

## 977\. 有序数组的平方

### 暴力解法

暴力解法很简单，数组依次平方再排序即可，只是时间复杂度有点高

### 双指针法

看了文章讲解后，发现双指针这种方法确实很妙，没有算法经验的就想不出来这种，还是得多刷多做多看多想

**思路**：由于数组是升序的，可能包含负数，所以计算平方后，最大的数就在两端。使用双指针指向头尾，比较平方后的大小，将较大的数插入新数组的尾部

```typescript
function sortedSquares(nums: number[]): number[] {
	const length = nums.length;
	let result = new Array(length).fill(0);
	let left = 0;
	let right = length - 1;
	let index = length - 1;
	while (left <= right) {
		let leftValue = nums[left] * nums[left];
		let rightValue = nums[right] * nums[right];
		if (leftValue < rightValue) {
			result[index--] = rightValue;
			right--;
		} else {
			result[index--] = leftValue;
			left++;
		};
	}
	return result;
};
```

### 209\. 长度最小的子数组

知道是使用双指针，指针的开始指向位置也想着是对着，但是两个指针在什么情况下该怎么移动搞得不清楚。

**思路**：双指针一开始都指向数组下标为0的元素，当两个指针的和小于`target`时，右指针向右移动，直到和大于等于`target`，记录两个指针之间元素的个数，并与之前满足条件的个数进行比较，选出最小的。然后需要将左指针向前移动，在这之前需要计算当前和减去左指针指向的元素。当右指针指向最后一个元素后即结束。这是如果没有满足要求的子数组返回0，否则返回记录的最小长度

```typescript
function minSubArrayLen(target: number, nums: number[]): number {
	let left = 0;
	let right = 0;
	let sum = 0;
	let result = Infinity;
	while (right < nums.length) {
		sum += nums[right];
		while (sum >= target) {
			result = Math.min(result, right - left + 1)
			sum -= nums[left];
			left++;
		};
		right++;
	}
	return result === Infinity ? 0 : result;
};
```

## 59\. 螺旋矩阵 二

之前有按随想录目录顺序刷到过数组里面这道题，不会写看了答案感觉有点复杂就放弃了。这次静下心来看了一遍视频讲解摸索着还给写出来了☺️

**思路**：这题需要注意的是对每一条边遍历时的取值范围，为了不混乱采用**左闭右开**的范围。我是从输出倒推二维数组的 `result[i][j]` 的 `i` 和 `j` 该是什么值。

![image.png](https://obsidian-picgo-le.oss-cn-hangzhou.aliyuncs.com/img/20230112144535.png align="left")

`输出：[[1,2,3],[8,9,4],[7,6,5]]`

1. 确定循环条件（旋转的圈数 ）
    

```typescript
function generateMatrix(n: number): number[][] {
	let result = new Array(n).fill(0).map(() => new Array(n).fill(0));
	let loop = Math.floor(n / 2);
	while (loop--) {}
}
```

1. 每一条边的遍历
    

对每一条边遍历的时候，不取该条边的最后一个元素

```typescript
function generateMatrix(n: number): number[][] {
	let result = new Array(n).fill(0).map(() => new Array(n).fill(0));
	let loop = Math.floor(n / 2);
	// 循环的起始位置
	let startx = 0;
	let starty = 0;
	// 旋转需要使用的变量
	let i = 0;
	let j = 0;
	// 左闭右开需要减去的元素个数
	let offset = 1;
	// 数组元素的值
	let count = 1;

	while (loop--) {
		// 从左上到右上 （如上图只需要遍历1，2，而1，2在输出的数组中是[0][0]和[0][1],所以只需要 j 去改变）
		for (; j < n - offset; j++) {
			result[i][j] = count++;
		}
		// 从右上到右下（需要遍历3，4，而3，4在输出的数组中是[0][2],[1][2]，在上一个for循环后，j 已经是 2了）
		for (; i < n - offset; i++) {
			result[i][j] = count++;
		}
		// 从右下到左下（与上面同理）
		for (; j > starty; j--) {
			result[i][j] = count++;
		}
		// 从左下到左上（与上面同理）
		for (; i > startx; i--) {
			result[i][j] = count++;
		}
	}
}
```

1. 循环一次完成后，需要将起始位置和右开减去的元素个数进行修改
    

```typescript
function generateMatrix(n: number): number[][] {
	let result = new Array(n).fill(0).map(() => new Array(n).fill(0));
	let loop = Math.floor(n / 2);
	// 循环的起始位置
	let startx = 0;
	let starty = 0;
	// 旋转需要使用的变量
	let i = 0;
	let j = 0;
	// 左闭右开需要减去的元素个数
	let offset = 1;
	// 数组元素的值
	let count = 1;

	while (loop--) {
		// 从左上到右上 （如上图只需要遍历1，2，而1，2在输出的数组中是[0][0]和[0][1],所以只需要 j 去改变）
		for (; j < n - offset; j++) {
			result[i][j] = count++;
		}
		// 从右上到右下（需要遍历3，4，而3，4在输出的数组中是[0][2],[1][2]，在上一个for循环后，j 已经是 2了）
		for (; i < n - offset; i++) {
			result[i][j] = count++;
		}
		// 从右下到左下（与上面同理）
		for (; j > starty; j--) {
			result[i][j] = count++;
		}
		// 从左下到左上（与上面同理）
		for (; i > startx; i--) {
			result[i][j] = count++;
		}

		// 转一圈后起始位置 +1
		startx++;
		starty++;
		// 转一圈后offset需要 +1
		offset++;
	}
}
```

1. 如果 n 是奇数，中间会还有一个数
    

```typescript
function generateMatrix(n: number): number[][] {
	let result = new Array(n).fill(0).map(() => new Array(n).fill(0));
	let loop = Math.floor(n / 2);
	// 循环的起始位置
	let startx = 0;
	let starty = 0;
	// 旋转需要使用的变量
	let i = 0;
	let j = 0;
	// 左闭右开需要减去的元素个数
	let offset = 1;
	// 数组元素的值
	let count = 1;
	// 中间元素在数组中的位置
	let loop = Math.floor(n / 2);

	while (loop--) {
		// 从左上到右上 （如上图只需要遍历1，2，而1，2在输出的数组中是[0][0]和[0][1],所以只需要 j 去改变）
		for (; j < n - offset; j++) {
			result[i][j] = count++;
		}
		// 从右上到右下（需要遍历3，4，而3，4在输出的数组中是[0][2],[1][2]，在上一个for循环后，j 已经是 2了）
		for (; i < n - offset; i++) {
			result[i][j] = count++;
		}
		// 从右下到左下（与上面同理）
		for (; j > starty; j--) {
			result[i][j] = count++;
		}
		// 从左下到左上（与上面同理）
		for (; i > startx; i--) {
			result[i][j] = count++;
		}

		// 转一圈后起始位置 +1
		startx++;
		starty++;
		// 转一圈后offset需要 +1
		offset++;
	}
	// 是奇数时，中间会有一个数
	if (n % 2 === 1) {
		result[mid][mid] = count;
	}
	return result;
}
```

通过以上四步就可以输出正确的矩阵，很考察代码水平和逻辑能力