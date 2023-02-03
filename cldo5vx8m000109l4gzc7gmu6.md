# Day 20 二叉树 - 二叉搜索树的最小绝对值

## 530\. 二叉搜索树的最小绝对值

```typescript
function getMinimumDifference(root: TreeNode | null): number {
	let res = Infinity;
	let pre = null;
	const dfs = (node: TreeNode | null) => {
		if (node === null) {
			return;
		};
		dfs(node.left);
		if (pre !== null) {
			res = Math.min(res, node.val - pre.val);
		};
		pre = node;
		dfs(node.right);
	};
	dfs(root);
	return res;
};
```

## 501\. 二叉搜索树中的众数

```typescript
function findMode(root: TreeNode | null): number[] {
	let res = [];
	let pre = null;
	let maxCount = 1;
	let count = 0;
	
	const dfs = (node) => {
		if (node === null) {
			return;
		}
		dfs(node.left);
		if (pre === null) {
			count = 1;
		} else if (pre.val === node.val) {
			count++;
		} else {
			count = 1;
		}
		pre = node;
		if (count === maxCount) {
			res.push(node.val);
		}
		if (count > maxCount) {
			res = [];
			maxCount = count;
			res.push(node.val);
		}
		dfs(node.right);
	};
	
	dfs(root);
	return res;
};
```

## 236\. 二叉树的最近公共祖先

```typescript
function lowestCommonAncestor(root: TreeNode | null, p: TreeNode | null, q: TreeNode | null): TreeNode | null {
	if (root === p || root === q || root === null) {
		return root;
	}
	
	const left = lowestCommonAncestor(root.left, p , q);
	const right = lowestCommonAncestor(root.right, p , q);
	
	if (left !== null && right !== null) {
		return root;
	}
	if (!left) {
		return right;
	}
	return left;
};
```