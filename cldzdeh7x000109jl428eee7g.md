# Day 28 回溯算法 - 递增子序列

## 491\. 递增子序列

```typescript
function findSubsequences(nums: number[]): number[][] {
	let result = [];
	let path = [];
	
	const backtracking = (startIndex) => {
		if (path.length >= 2) {
			result.push([...path]);
		}
		
		let unset = [];
		for (let i = startIndex; i < nums.length; i++) {
			if (path.length > 0 && nums[i] < path[path.length - 1] || unset[nums[i] + 100]) {
				continue;
			} else {
				unset[nums[i] + 100] = true;
				path.push(nums[i]);
				backtracking(i + 1);
				path.pop();
			}
		}
	};
	
	backtracking(0);
	return result;
};
```

## 46\. 全排列

```typescript
function permute(nums: number[]): number[][] {
	let result = [];
	let path = [];
	
	const backtracking = (used) => {
		if (path.length === nums.length) {
			result.push([...path]);
			return;
		}
		
		for (let i = 0; i < nums.length; i++) {
			if (used[nums[i]]) {
				continue;
			}
			
			used[nums[i]] = true
			path.push(nums[i]);
			backtracking(used);
			path.pop();
			used[nums[i]] = false;
		}
	};
	
	backtracking([]);
	return result;
};
```

## 47\. 全排列 ii

```typescript
function permuteUnique(nums: number[]): number[][] {
	let result = [];
	let path = [];
	
	nums.sort((a, b) => a - b);
	
	const backtracking = (used: boolean[]) => {
		if (path.length === nums.length) {
			result.push([...path]);
			return;
		}
		
		for (let i = 0; i < nums.length; i++) {
			if (used[i - 1] === false && i > 0 && nums[i] === nums[i - 1]) {
				continue;
			}
			
			if (!used[i]) {
				used[i] = true;
				path.push(nums[i]);
				backtracking(used);
				path.pop();
				used[i] = false;
			}
		}
	};
	
	backtracking([]);
	return result;
};
```