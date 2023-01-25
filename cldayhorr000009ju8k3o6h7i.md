# Day 11 栈与队列 - 有效的括号

## 20\. 有效的括号

```typescript
function isValid(s: string): boolean {
	if (s.length % 2 === 1) {
		return false;
	};
	const map = {
		'(': ')',
		'[': ']',
		'{': '}',
	};
	const stack = [];
	for (let i = 0; i < s.length; i++) {
		if (s[i] === '(' || s[i] === '{' || s[i] === '[') {
			stack.push(map[s[i]]);
		} else {
			let popStr = stack.pop();
			if (popStr !== s[i]) {
				return false;
			}
		};
	}
	return stack.length === 0;
};
```

## 1047\. 删除字符串中的所有相邻重复项

```typescript
function removeDuplicates(s: string): string {
	const stack = [];
	for (let i = 0; i < s.length; i++) {
		let popValue = stack.pop();
		if (popValue !== s[i]) {
			stack.push(popValue);
			stack.push(s[i]);
		}
	};
	return stack.join('');
};
```

## 150\. 逆波兰表达式求值

```typescript
function evalRPN(tokens: string[]): number {
	const stack = [];
	for (let value of tokens) {
		if (value === '+' || value === '-' || value === '*' || value === '/') {
			const popValueOne = stack.pop();
			const popValueTwo = stack.pop();
			if (value === '+') {
				stack.push(popValueTwo + popValueOne);
			}
			if (value === '-') {
				stack.push(popValueTwo - popValueOne);
			}
			if (value === '*') {
				stack.push(popValueTwo * popValueOne);
			}
			if (value === '/') {
				stack.push(popValueTwo / popValueOne | 0);
			}
		} else {
			stack.push(Number(value));
		}
	}
	return stack[0];
};
```