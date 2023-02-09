# Day 26 回溯算法 - 组合总和

## 39\. 组合总和

```typescript
function combinationSum(candidates: number[], target: number): number[][] {
	let result = [];
	let path = [];
	let sum = 0;
	
	const backtracking = (startIndex: number) => {
		if (sum > target) {
			return;
		}
		
		if (sum === target) {
			result.push([...path]);
			return;
		}
		
		for (let i = startIndex; i < candidates.length; i++) {
			path.push(candidates[i]);
			sum += candidates[i];
			backtracking(i);
			path.pop();
			sum -= candidates[i];
		}
	};
	
	backtracking(0);
	return result;
};
```

## 40\. 组合总和 ii

```typescript
function combinationSum2(candidates: number[], target: number): number[][] {
	let result = [];
	let path = [];
	let sum = 0;
	
	candidates.sort((a, b) => a - b)
	
	const backtracking = (startIndex: number) => {
		if (sum > target) {
			return;
		}
		
		if (sum === target) {
			result.push([...path]);
			return;
		}
		
		for (let i = startIndex; i < candidates.length; i++) {
			if (i > startIndex && candidates[i] === candidates[i - 1]) {
				continue;
			}
			
			path.push(candidates[i]);
			sum += candidates[i];
			backtracking(i + 1);
			path.pop();
			sum -= candidates[i];
		}
	};
	
	backtracking(0);
	return result;
};
```

## 131\. 分割回文串

```typescript
function partition(s: string): string[][] {
	let result = [];
	let path = [];
	
	const backtracking = (startIndex: number) => {
		if (startIndex >= s.length) {
			result.push([...path]);
			return;
		}
		
		for (let i = startIndex; i < s.length; i++) {
			if (isPalindrome(s, startIndex, i)) {
				const str = s.substring(startIndex, i + 1);
				path.push(str);
				backtracking(i + 1);
			} else {
				continue;
			}
			path.pop();
		}
	};
	
	const isPalindrome = (str: string, left: number, right: number) => {
		while (left < right) {
			if (str[left] !== str[right]) {
				return false;
			}
			left++;
			right--;
		}
	return true;
	}
	
	backtracking(0);
	return result;
};
```