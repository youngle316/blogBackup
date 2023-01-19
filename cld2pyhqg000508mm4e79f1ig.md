# Day 09 字符串 - 找出字符串中第一个匹配项的下标

## 28\. 找出字符串中第一个匹配项的下标

`indexOf` 直接解决，KMP二刷再看

```typescript
function strStr(haystack: string, needle: string): number {
	return haystack.indexOf(needle);
};
```

## 459\. 重复的子字符串

```typescript
function repeatedSubstringPattern(s: string): boolean {
	let str = '';
	for (let i = 0; i < s.length - 1; i++) {
		str += s[i];
		if (str.repeat(Math.floor(s.length / str.length)) === s) {
			return true;
		}
	}
	return false;
};
```