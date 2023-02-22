# Day 39 动态规划 - 不同路径

## 62\. 不同路径

```typescript
function uniquePaths(m: number, n: number): number {
	let dp = new Array(m).fill(1).map(() => new Array(n).fill(1));
	
	for (let i = 1; i < m; i++) {
		for (let j = 1; j < n; j++) {
			dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
		}
	}
	
	return dp[m - 1][n - 1]
};
```

## 63\. 不同路径 ii

```typescript
function uniquePathsWithObstacles(obstacleGrid: number[][]): number {
	let dp = new Array(obstacleGrid.length).fill(0).map(() => new Array(obstacleGrid[0].length).fill(0))
	
	for (let i = 0; i < obstacleGrid.length && obstacleGrid[i][0] === 0; i++) {
		dp[i][0] = 1;
	}
	
	for (let j = 0; j < obstacleGrid[0].length && obstacleGrid[0][j] === 0; j++) {
		dp[0][j] = 1;
	}
	
	for (let i = 1; i < obstacleGrid.length; i++) {
		for (let j = 1; j < obstacleGrid[0].length; j++) {
			if (obstacleGrid[i][j] === 1) {
				continue;
			}
			dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
		}
	}
	
	return dp[obstacleGrid.length - 1][obstacleGrid[0].length - 1];
};
```