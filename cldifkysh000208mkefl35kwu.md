# Day 16 二叉树 - 完全二叉树的节点个数

> 二叉树的最大最小深度已在 Day 14 完成

## 222\. 完全二叉树的节点个数

```typescript
function countNodes(root: TreeNode | null): number {
	if (root === null) {
		return 0;
	}
	const queue = [root];
	let res = 0;
	while (queue.length) {
		const node = queue.shift();
		res += 1;
		node.left && queue.push(node.left);
		node.right && queue.push(node.right);
	};
	return res;
};
```