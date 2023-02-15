# Day 32 贪心算法 - 买卖股票的最佳时机 ii

## 122\. 买卖股票的最佳时机 ii

```typescript
function maxProfit(prices: number[]): number {
	let result = 0;
	
	for (let i = 1; i < prices.length; i++) {
		result += Math.max(0, prices[i] - prices[i - 1]);
	};
	
	return result;
};
```

## 55\. 跳跃游戏

```typescript
function canJump(nums: number[]): boolean {
	let cover = 0;
	
	if (nums.length === 1) {
		return true;
	}
	
	for (let i = 0; i <= cover; i++) {
		cover = Math.max(i + nums[i], cover);
		if (cover >= nums.length - 1) {
			return true;
		}
	}
	
	return false;
};
```

## 45\. 跳跃游戏 ii

```typescript
function jump(nums: number[]): number {
	let steps = 0;
	let curStepsMaxReach = 0;
	let oneMoreStepsMaxReach = nums[0];
	
	for (let i = 1; i < nums.length; i++) {
		if (i > curStepsMaxReach) {
			steps++;
			curStepsMaxReach = oneMoreStepsMaxReach;
		}
		oneMoreStepsMaxReach = Math.max(oneMoreStepsMaxReach, i + nums[i]);
	}
	
	return steps;
};
```