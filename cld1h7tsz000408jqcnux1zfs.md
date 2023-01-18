# Day 08 字符串 - 反转字符串

## 344\. 反转字符串

写算法题，当用提供好的方法能一步或几步解决时，不要使用。自己实现

```typescript
function reverseString(s: string[]): void {
	let left = 0;
	let right = s.length - 1;
	while (left < right) {
		let temp = s[left];
		s[left] = s[right];
		s[right] = temp;
		left++;
		right--;
	}
};
```

## 541\. 反转字符串 ii

```typescript
function reverseStr(s: string, k: number): string {
const reverse = (str: string, start: number, end: number) => {
	let left = start;
	let right = end;
	let arr = str.split('');
	while (left < right) {
		let temp = arr[left];
		arr[left] = arr[right];
		arr[right] = temp;
		left++;
		right--;
	}
	return arr.join('');
};

let newStr = s;

for (let i = 0; i < s.length; i += (2 * k)) {
	if (i + k <= s.length - 1) {
		newStr = reverse(newStr, i, i + k - 1);
		continue;
	}
		newStr = reverse(newStr, i, s.length - 1);
	}
return newStr;
};
```

## 剑指Offer 05. 替换空格

本来是想用正则替换的，`replaceAll` 还不识别。就改用双指针了

```typescript
function replaceSpace(s: string): string {
	let blankNum = 0;
	
	for (let i = 0; i < s.length; i++) {
		if (s[i] === ' ') {
			blankNum++;
		}
	}
	
	let strArr = s.split('');
	let left = strArr.length - 1;
	let right = strArr.length - 1 + blankNum * 2;
	
	while (left >= 0) {
		if (strArr[left] === ' ') {
			strArr[right--] = '0';
			strArr[right--] = '2';
			strArr[right--] = '%';
			left--;
		} else {
			strArr[right--] = strArr[left--];
		}
	}
	return strArr.join('');
};
```

## 151\. 反转字符串中的单词

思路：

1. 去除所有多余的空格
    
2. 反转整个字符串
    
3. 反转单个单词
    

```typescript
function reverseWords(s: string): string {

	const removeExtraBlank = (arr: string[]) => {
	let left = 0;
	let right = 0;
	while (right < arr.length) {
		if (arr[right] === ' ' && (right === 0 || arr[right - 1] === ' ')) {
				right++;
			} else {
				arr[left++] = arr[right++];
			}
		}
		// 移除末尾空格
		arr.length = arr[left - 1] === ' ' ? left - 1 : left;
	}

	const reverse = (arr: string[], start: number, end: number) => {
		let left = start;
		let right = end;
		while (left < right) {
			let temp = arr[left];
			arr[left] = arr[right];
			arr[right] = temp;
			left++;
			right--;
		}
	}

	let sArr = s.split('');
	// 清除多余的空格
	removeExtraBlank(sArr);
	// 反转整个数组
	reverse(sArr, 0 , sArr.length - 1);
	// 反转每一个单词
	let start = 0;
	for (let i = 0; i <= sArr.length; i++) {
		if (sArr[i] === ' ' || i === sArr.length) {
			reverse(sArr, start, i - 1);
			start = i + 1;
		}
	}
	return sArr.join('');
};
```

## 剑指Offer 58. 左旋转字符串

思路

1. 先反转 `0-k`
    
2. 再反转 `k - length`
    
3. 再反转整个字符串
    

```typescript
function reverseLeftWords(s: string, n: number): string {
	const reverse = (arr: string[], start: number, end: number) => {
		let left = start;
		let right = end;
		while (left < right) {
			let temp = arr[left];
			arr[left] = arr[right];
			arr[right] = temp;
			left++;
			right--;
		}
	}

	let sArr = s.split('');
	reverse(sArr, 0, n - 1);
	reverse(sArr, n, s.length - 1);
	reverse(sArr, 0, s.length - 1);
	return sArr.join('');
};
```