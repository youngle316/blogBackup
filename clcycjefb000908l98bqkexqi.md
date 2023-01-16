# Day 06 哈希表 - 有效的字母异位词

## 242\. 有效的字母异位词

思路：

1. 将字符串 `s` 中字母的类型及个数存储在 `Map` 中
    
2. 遍历字符串 t ，如果字符串中的字母不存在 Map 中，返回 false
    
3. 如果存在，获取字母的个数，将个数减一，如果减去一之后的值小于0，返回 false
    
4. 如果不小于一，将减后的值再赋值给 Map
    
5. 返回 true
    

```typescript
function isAnagram(s: string, t: string): boolean {
	if (s.length !== t.length) {
		return false;
	}
	let map = new Map();
	for (let key of s) {
		map.set(key, (map.get(key) || 0) + 1);
	}

	for (let key of t) {
		if (map.has(key)) {
			let value = map.get(key);
			value--;
			if (value < 0) {
				return false;
			}
			map.set(key, value);
		} else {
			return false;
		}
	}
	return true;
};
```

## 349\. 两个数组的交集

思路：

1. 定义一个 `Map` 和 结果数组
    
2. 将 `nums1` 的元素存到 `Map` 中
    
3. 如果 `nums2` 的元素在 `Map` 中存在但是在 `result` 中不存在，添加到 `result` 中
    
4. 返回 `result`
    

```typescript
function intersection(nums1: number[], nums2: number[]): number[] {
	if (nums1.length === 0 || nums2.length === 0) {
		return [];
	};
	let result = [];
	let map = new Map();
	for (let key of nums1) {
		map.set(key, (map.get(key) || 0) + 1);
	};
	for (let key of nums2) {
		if (map.has(key) && !result.includes(key)) {
			result.push(key);
		}
	}
	return result;
};
```

## 202\. 快乐数

思路：

1. 循环获取每一次计算出来的值
    
2. 如果值为 1，则直接返回 `true`
    
3. 如果值不为 1，并且在 `Map` 中不存在则存入 `Map` 中
    
4. 存在的话，表示已经无限循环了，返回 `false`
    

```typescript
function isHappy(n: number): boolean {
	const getSum = (n: number): number => {
		let sum = 0;
		while (n) {
			sum += (n % 10) * (n % 10);
			n = Math.floor(n / 10);
		}
		return sum;
	};

	let map = new Map();
	while (true) {
		let sum = getSum(n);
		if (sum === 1) {
			return true;
		}
		if (map.has(sum)) {
			return false;
		} else {
			map.set(sum, '');
		};
		n = sum;
	}
};
```

## 1\. 两数之和

思路：

1. 将数组的元素及其下标存储到 `Map` 中
    
2. 遍历数组，确定差的值
    
3. 判断差的值是否存在于 `Map` 中并且下标不同
    
4. 返回下标
    

```typescript
function twoSum(nums: number[], target: number): number[] {
	let map = new Map();
	for (let i = 0; i < nums.length; i++) {
		map.set(nums[i], i);
	};
	for (let i = 0; i < nums.length; i++) {
		const need = target - nums[i];
		if (map.has(need) && i !== map.get(need)) {
			return [i, map.get(need)]
		}
	}
};
```