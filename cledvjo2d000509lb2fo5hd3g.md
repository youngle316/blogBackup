# Day 38 贪心算法 - 斐波那契数

## 509\. 斐波那契数

```typescript
function fib(n: number): number {
	const dp = [0, 1];
	
	for (let i = 2; i <= n; i++) {
		dp[i] = dp[i - 1] + dp[i - 2];
	};
	
	return dp[n];
};
```

## 70\. 爬楼梯

```typescript
function climbStairs(n: number): number {
	const dp = [1, 1];
	
	for (let i = 2; i <= n; i++) {
		dp[i] = dp[i - 1] + dp[i - 2];
	}
	
	return dp[n];
};
```

## 746\. 使用最小花费爬楼梯

```typescript
function minCostClimbingStairs(cost: number[]): number {
	let dp = [0, 0];
	
	for (let i = 2; i <= cost.length; i++) {
		dp[i] = Math.min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2]);
	}
	
	return dp[cost.length];
};
```