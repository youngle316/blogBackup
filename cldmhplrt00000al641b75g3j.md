# Day 19 二叉树 - 最大二叉树

## 654\. 最大二叉树

```typescript
function constructMaximumBinaryTree(nums: number[]): TreeNode | null {
	const buildTree = (arr: number[], left: number, right: number) => {
		if (left > right) {
			return null;
		};
		let maxIndex = -1;
		let maxValue = -1;
		for (let i = left; i <= right; i++) {
			if (arr[i] > maxValue) {
				maxValue = arr[i];
				maxIndex = i;
			}
		}
		let tree = new TreeNode(maxValue);
		tree.left = buildTree(arr, left, maxIndex - 1);
		tree.right = buildTree(arr, maxIndex + 1, right);
		return tree;
	};
	let root = buildTree(nums, 0, nums.length - 1);
	return root;
};
```

## 617\. 合并二叉树

```typescript
function mergeTrees(root1: TreeNode | null, root2: TreeNode | null): TreeNode | null {
	const dfs = (root1, root2) => {
		if (root1 === null) {
			return root2;
		}
		if (root2 === null) {
			return root1;
		};
		root1.val += root2.val;
		root1.left = dfs(root1.left, root2.left);
		root1.right = dfs(root1.right, root2.right);
		return root1;
	}
	return dfs(root1, root2)
};
```

## 700\. 二叉搜索树中的搜索

```typescript
function searchBST(root: TreeNode | null, val: number): TreeNode | null {
	while (root) {
		if (root.val > val) {
			root = root.left;
		} else if (root.val < val) {
			root = root.right;
		} else {
			return root;
		}
	}
	return null;
};
```

## 98\. 验证二叉搜索树

```typescript
function isValidBST(root: TreeNode | null): boolean {
	if (root === null) {
		return true;
	}
	let cur = root;
	let pre = null;
	const stack = [];
	while (cur || stack.length) {
		if (cur) {
			stack.push(cur);
			cur = cur.left;
		} else {
			const node = stack.pop();
			if (pre !== null && node.val <= pre.val) {
				return false;
			}
			pre = node;
			cur = node.right;
		}
	}
	return true;
};
```