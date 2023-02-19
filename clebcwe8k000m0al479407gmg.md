# Day 35 贪心算法 - 无重叠区间

## 435\. 无重叠区间

```typescript
function eraseOverlapIntervals(intervals: number[][]): number {
	let result = 0;
	
	intervals.sort((a, b) => a[0] - b[0]);
	
	for (let i = 1; i < intervals.length; i++) {
		if (intervals[i][0] < intervals[i - 1][1]) {
			result++;
			intervals[i][1] = Math.min(intervals[i - 1][1], intervals[i][1])
		}
	}
	
	return result;
};
```

## 763\. 划分字母区间

```typescript
function partitionLabels(s: string): number[] {
	const map = {};
	
	for (let i = 0; i < s.length; i++) {
		map[s[i]] = i;
	};
	
	let result = [];
	let left = 0;
	let right = 0;
	
	for (let i = 0; i < s.length; i++) {
		right = Math.max(right, map[s[i]]);
		if (right === i) {
			result.push(right - left + 1);
			left = i + 1;
		}
	}
	
	return result;
};
```

## 56\. 合并区间

```typescript
function merge(intervals: number[][]): number[][] {
	intervals.sort((a, b) => a[0] - b[0]);
	
	let result = [intervals[0]];
	
	for (let i = 1; i < intervals.length; i++) {
		if (intervals[i][0] <= result[result.length - 1][1]) {
			result[result.length - 1][1] = Math.max(result[result.length - 1][1], intervals[i][1])
			} else {
			result.push(intervals[i]);
		}
	}
	
	return result;
};
```