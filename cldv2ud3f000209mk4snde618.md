# Day 25 回溯算法 - 组合总和 iii

## 216\. 组合总和 iii

```typescript
function combinationSum3(k: number, n: number): number[][] {
	let result = [];
	let path = [];
	let sum = 0;
	
	const backTracking = (k: number, startIndex: number) => {
		if (path.length === k && sum === n) {
			result.push([...path]);
			return;
		};
		
		for (let i = startIndex; i <= 9 - (k - path.length) + 1; i++) {
			path.push(i);
			sum += i;
			if (sum > n) {
				path.pop();
				sum -= i;
				return;
			}
			backTracking(k, i + 1);
			path.pop();
			sum -= i;
		}
	};
	
	backTracking(k, 1);
	return result;
};
```

## 17\. 电话号码的字母组合

```typescript
function letterCombinations(digits: string): string[] {
	let result = [];
	let path = [];
	const k = digits.length;
	const map = ["", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"];
	
	if (k === 0) {
		return result;
	}
	
	if (k === 1) {
		return map[digits].split('');
	}
	
	const backtracking = (index) => {
		if (path.length === k) {
			result.push(path.join(''));
			return;
		}
		
		for(let value of map[digits[index]]) {
			path.push(value);
			backtracking(index + 1);
			path.pop();
		}
	};
	
	backtracking(0);
	return result;
};
```