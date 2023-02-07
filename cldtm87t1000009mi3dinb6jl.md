# Day 24 回溯算法 - 组合

## 77\. 组合

```typescript
function combine(n: number, k: number): number[][] {
	let result = [];
	let path = [];
	
	const backTracking = (n: number, k: number, startIndex: number) => {
		if (path.length === k) {
			result.push([...path]);
			return;
		};
		for (let i = startIndex; i <= n - (k - path.length) + 1; i++) {
			path.push(i);
			backTracking(n, k, i + 1);
			path.pop();
		}
	};
	
	backTracking(n, k, 1);
	return result;
};
```