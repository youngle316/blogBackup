# Day 40 动态规划 - 整数拆分

## 343\. 整数拆分

```typescript
function integerBreak(n: number): number {
	let dp = new Array(n + 1).fill(0);
	
	dp[2] = 1;
	
	for(let i = 3; i <= n; i++) {
		for(let j = 1; j <= i / 2; j++) {
			dp[i] = Math.max(dp[i], dp[i - j] * j, (i - j) * j);
		}
	}
	
	return dp[n];
};
```

## 96\. 不同的二叉搜索树

```typescript
function numTrees(n: number): number {
	let dp = new Array(n + 1).fill(0);
	dp[0] = 1;
	dp[1] = 1;
	
	for (let i = 2; i <= n; i++) {
		for (let j = 1; j <= i; j++ ) {
			dp[i] += dp[j - 1] * dp[i - j];
		}
	}
	
	return dp[n];
};
```