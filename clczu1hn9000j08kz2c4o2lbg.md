# Day 07 哈希表 - 四数相加 二

## 454\. 四数相加 二

思路：

1. 循环遍历 `nums1` 和 `nums2` 数相加的结果存在 `Map` 中，`key` 为和， `value` 为和的次数
    
2. 再遍历 `num3` 和 `nums4`，计算 `sum`，判断 0-`sum` 是否存在于 `Map` 中
    

```typescript
function fourSumCount(nums1: number[], nums2: number[], nums3: number[], nums4: number[]): number {
	let map = new Map();
	let count = 0;
	for (let n1 of nums1) {
		for (let n2 of nums2) {
			const sum = n1 + n2;
			map.set(sum, (map.get(sum) || 0) + 1);
		}
	}
	
	for (let n3 of nums3) {
		for (let n4 of nums4) {
			const sum = n3 + n4;
			count += (map.get((0 - sum)) || 0);
		}
	} 
	
	return count;
};
```

## 383\. 赎金信

```typescript
function canConstruct(ransomNote: string, magazine: string): boolean {
	let map = new Map();
	for (let key of magazine) {
		map.set(key, (map.get(key) || 0) + 1);
	};
	for (let key of ransomNote) {
		if (map.has(key) && map.get(key) > 0) {
			let value = map.get(key);
			value--;
			map.set(key, value);
		} else {
			return false;
		}
	}
	return true;
};
```

## 15\. 三数之和

一开始使用的哈希表做的，去重真的难做。看了题解使用了双指针

```typescript
function threeSum(nums: number[]): number[][] {
	let result = [];
	nums.sort((a, b) => a - b);
	for (let i = 0; i < nums.length; i++) {
		let left = i + 1;
		let right = nums.length - 1;
		let iNum = nums[i];
		if (iNum > 0) {
			return result;
		}
		// 去重
		if (iNum === nums[i - 1]) {
			continue;
		};
		
		while (right > left) {
			let leftNum = nums[left];
			let rightNum = nums[right];
			let totalNum = leftNum + rightNum + iNum;
			if (totalNum > 0) {
				right--;
			} else if (totalNum < 0) {
				left++;
			} else {
				result.push([iNum, leftNum, rightNum])
				// 去重
				while (left < right && leftNum === nums[left + 1]) {
					left++;
				}
				while (left < right && rightNum === nums[right - 1]) {
					right--;
				}
				right--;
				left++;
			}
		}
	}
	return result;
};
```

## 18.四数之和

四数之和同三数之和一样，只是多套了一层 `for` 循环。需要注意的是：`i` 和 `j` 去重的条件也有不同

```typescript
function fourSum(nums: number[], target: number): number[][] {
	let result = [];
	if (nums.length < 4) {
		return result;
	};
	
	nums.sort((a, b) => a - b);
	
	for (let i = 0; i < nums.length - 3; i++) {
		// 去重 i
		if (i > 0 && nums[i] === nums[i - 1]) {
			continue;
		};
		for (let j = i + 1; j < nums.length - 2; j++) {
			let left = j + 1;
			let right = nums.length - 1;
			// 去重 j
			if (j > i + 1 && nums[j] === nums[j - 1]) {
				continue;
			}
			
			while (right > left) {
				let totalNum = nums[i] + nums[j] + nums[left] + nums[right];
				if (totalNum > target) {
					right--;
				} else if (totalNum < target) {
					left++;
				} else {
					result.push([nums[i], nums[j], nums[left], nums[right]]);
					while (right > left && nums[left] === nums[left + 1]) {
						left++;
					};
					while (right > left && nums[right] === nums[right - 1]) {
						right--;
					}
					left++;
					right--;
				}
			}
		}
	}
	return result;
};
```