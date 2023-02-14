# Day 31 贪心算法 - 分发饼干

## 455\. 分发饼干

```typescript
function findContentChildren(g: number[], s: number[]): number {
	let result = 0;
	g.sort((a, b) => a - b);
	s.sort((a, b) => a - b);
	let sIndex = s.length - 1;
	
	for (let i = g.length - 1; i >= 0; i--) {
		if (sIndex >= 0 && s[sIndex] >= g[i]) {
			result++;
			sIndex--;
		}
	}
	
	return result;
};
```

## 376\. 摆动序列

```typescript
function wiggleMaxLength(nums: number[]): number {
	if (nums.length === 1) {
		return nums.length
	};
	
	let result = 1;
	let preDiff = 0;
	let curDiff = 0;
	
	for (let i = 0; i < nums.length; i++) {
		curDiff = nums[i + 1] - nums[i];
		if ((curDiff > 0 && preDiff <= 0) || (curDiff < 0 && preDiff >= 0)) {
			result++;
			curDiff = preDiff;
		}
	}
	
	return result;
};
```

## 53\. 最大子数组和

```typescript
function maxSubArray(nums: number[]): number {
	let result = -Infinity;
	let count = 0;
	
	for (let i = 0; i < nums.length; i++) {
		count += nums[i];
		if (count > result) {
			result = count;
		}
		if (count < 0) {
			count = 0;
		}
	}
	
	return result;
};
```