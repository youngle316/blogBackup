# Day 33 贪心算法 - K次取反后最大化的数组和

## 1005\. K次取反后最大化的数组和

```typescript
function largestSumAfterKNegations(nums: number[], k: number): number {
	nums.sort((a, b) => Math.abs(b) - Math.abs(a)); 
	
	for (let i = 0; i < nums.length; i++) {
		if (nums[i] < 0 && k > 0) {
			nums[i] = -nums[i];
			k--;
		}
	}
	
	while (k > 0) {
		nums[nums.length - 1] = -nums[nums.length - 1];
		k--;
	}
	
	return nums.reduce((a, b) => a + b)
};
```

## 134\. 加油站

```typescript
function canCompleteCircuit(gas: number[], cost: number[]): number {
	const gasLen = gas.length
	let start = 0
	let curSum = 0
	let totalSum = 0
	
	for(let i = 0; i < gasLen; i++) {
		curSum += gas[i] - cost[i]
		totalSum += gas[i] - cost[i]
		if(curSum < 0) {
			curSum = 0
			start = i + 1
		}
	} 
	
	if (totalSum < 0) return -1  
	
	return start;
};
```

## 135\. 分发糖果

```typescript
function candy(ratings: number[]): number {
	let candys = new Array(ratings.length).fill(1)
	
	for(let i = 1; i < ratings.length; i++) {
		if (ratings[i] > ratings[i - 1]) {
			candys[i] = candys[i - 1] + 1
		}
	}
	
	for(let i = ratings.length - 2; i >= 0; i--) {
		if (ratings[i] > ratings[i + 1]) {
			candys[i] = Math.max(candys[i], candys[i + 1] + 1)
		}
	}
	
	let count = candys.reduce((a, b) => {
		return a + b
	})
	
	return count
};
```