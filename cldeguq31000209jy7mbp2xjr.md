# Day 13 二叉树 - 递归与迭代法遍历

## 144\. 二叉树的前序遍历

### 递归法

```typescript
function preorderTraversal(root: TreeNode | null): number[] {
	const result = [];
	const dfs = (node: TreeNode | null) => {
		if (node.val === null) {
			return;
		}
		result.push(node.val);
		node.left && dfs(node.left);
		node.right && dfs(node.right);
	};
	dfs(root);
	return result;
};
```

### 迭代法

```typescript
function preorderTraversal(root: TreeNode | null): number[] {
	if (root === null) {
		return [];
	}
	const stack = [];
	const result = [];
	stack.push(root);
	while (stack.length) {
		const node = stack.pop();
		result.push(node.val);
		node.right && stack.push(node.right);
		node.left && stack.push(node.left);
	};
	return result;
};
```

## 145\. 二叉树的后序遍历

### 递归法

```typescript
function postorderTraversal(root: TreeNode | null): number[] {
	if (!root) {
		return [];
	}
	const result = [];
	const dfs = (node: TreeNode | null ) => {
		if (!node) {
			return;
		}
		node.left && dfs(node.left);
		node.right && dfs(node.right);
		result.push(node.val);
	};
	dfs(root);
	return result;
};
```

### 迭代法

```typescript
function postorderTraversal(root: TreeNode | null): number[] {
	if (!root) {
		return [];
	};
	const stack = [root];
	const result = [];
	while (stack.length) {
		const node = stack.pop();
		result.push(node.val);
		node.left && stack.push(node.left);
		node.right && stack.push(node.right);
	};
	return result.reverse();
};
```

## 94\. 二叉树的中序遍历

### 递归法

```typescript
function inorderTraversal(root: TreeNode | null): number[] {
	if (!root) {
		return [];
	}
	const result = [];
	const dfs = (value: TreeNode | null) => {
		if (!value) {
			return;
		};
		value.left && dfs(value.left);
		result.push(value.val);
		value.right && dfs(value.right);
	};
	dfs(root);
	return result;
};
```

### 迭代法

```typescript
function inorderTraversal(root: TreeNode | null): number[] {
	const result = [];
	let cur = root;
	const stack = [];
	while (cur || stack.length) {
		if (cur) {
			stack.push(cur);
			cur = cur.left;
		} else {
			const node = stack.pop();
			result.push(node.val);
			cur = node.right;
		}
	}
	return result;
};
```