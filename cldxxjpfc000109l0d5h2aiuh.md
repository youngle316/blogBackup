# Day 27 回溯算法 - 复原IP地址

## 93.复原IP地址

```typescript
function restoreIpAddresses(s: string): string[] {
	let result = [];
	let path = [];
	
	const backtracking = (startIndex: number) => {
		if (path.length > 4) {
			return;
		}
		
		if (path.length === 4 && startIndex === s.length) {
			result.push(path.join('.'));
			return;
		};
		
		for (let i = startIndex; i < s.length; i++) {
			const val = s.slice(startIndex, i + 1);
			if (isValid(val)) {
				path.push(val);
				backtracking(i + 1);
				path.pop();
			}
		}
	};
	
	const isValid = (str: string) => {
		let resBool: boolean = true;
		let tempVal: number = Number(str);
		
		if (
			str.length === 0 || isNaN(tempVal) ||
			tempVal > 255 || tempVal < 0 ||
			(str.length > 1 && str[0] === '0')
		) {
			resBool = false;
		}
		return resBool;
	};
	
	backtracking(0);
	return result;
};
```

## 78\. 子集

```typescript
function subsets(nums: number[]): number[][] {
	let result = [];
	let path = [];
	
	const backtracking = (startIndex: number) => {
		result.push([...path]);
		for (let i = startIndex; i < nums.length; i++) {
			path.push(nums[i]);
			backtracking(i + 1);
			path.pop();
		}
	};
	
	backtracking(0);
	return result;
};
```

## 90\. 子集 ii

```typescript
function subsetsWithDup(nums: number[]): number[][] {
	let result = [];
	let path = [];
	
	nums.sort((a, b) => a - b);
	
	const backtracking = (startIndex: number) => {
		result.push([...path]);
		for (let i = startIndex; i < nums.length; i++) {
			if (i > startIndex && nums[i] === nums[i - 1]) {
				continue;
			}
			
			path.push(nums[i]);
			backtracking(i + 1);
			path.pop();
		}
	};
	
	backtracking(0);
	return result;
};
```