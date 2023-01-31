# Day 17 二叉树 - 平衡二叉树

## 110\. 平衡二叉树

```typescript
function isBalanced(root: TreeNode | null): boolean {
	if (root === null) {
		return true;
	};
	const dfs = (node: TreeNode | null) => {
		if (node === null) {
			return 0;
		}
		const leftDepth = dfs(node.left);
		if (leftDepth === -1) {
			return -1;
		}
		const rightDepth = dfs(node.right);
		if (rightDepth === -1) {
			return -1;
		}
		if (Math.abs(leftDepth - rightDepth) > 1) {
			return -1;
		} else {
			return 1 + Math.max(leftDepth, rightDepth)
		}
	};
	return dfs(root) !== -1;
};
```

## 257\. 二叉树的所有路径

```typescript
function binaryTreePaths(root: TreeNode | null): string[] {
	let res = [];
	const dfs = (node: TreeNode | null, path: string) => {
		if (node.left === null && node.right === null) {
			path += node.val;
			res.push(path);
			return;
		};
		path += node.val + '->';
		node.left && dfs(node.left, path);
		node.right && dfs(node.right, path);
	};
	dfs(root, '');
	return res;
};
```

## 404\. 左叶子之和

### 递归法

```typescript
function sumOfLeftLeaves(root: TreeNode | null): number {
	if (root.left === null && root.right === null) {
	return 0;
	}
	let res = 0;
	const dfs = (node: TreeNode | null) => {
	if (node === null) {
	return;
	}
	if (node.left && node.left.left === null && node.left.right === null) {
	res += node.left.val;
	}
	node.left && dfs(node.left);
	node.right && dfs(node.right);
	};
	dfs(root);
	return res;
};
```

### 迭代法

```typescript
function sumOfLeftLeaves(root: TreeNode | null): number {
	if (root.left === null && root.right === null) {
		return 0;
	}
	let res = 0;
	let queue = [root];
	while (queue.length) {
		const node = queue.shift();
		if (node.left !== null && node.left.left === null && node.left.right === null) {
			res += node.left.val;
		}
		node.left && queue.push(node.left);
		node.right && queue.push(node.right)
	}
	return res;
};
```