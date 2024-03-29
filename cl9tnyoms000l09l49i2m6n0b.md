# 二叉树的总结

 > 最近用两周的时间完成了 [代码随想录二叉树](https://programmercarl.com/%E4%BA%8C%E5%8F%89%E6%A0%91%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E7%A7%8D%E7%B1%BB)  部分的内容

二叉树也是算法面试中比较重要的一部分，最有趣的就是 `HomeBrew` 的作者在面试 Google 时，由于不能在白板中写出翻转二叉树而不被录用。

以下记录一下二叉树的学习情况

## 二叉树的遍历方式

二叉树的遍历方式有深度优先遍历和广度优先遍历。而深度优先遍历又可以有三种分别是前中后序遍历来实现，广度优先遍历使用层序遍历来实现；而前中后序可以使用递归法与迭代法实现，层序遍历使用迭代法实现。

前端面试二叉树的话可能会考用递归法和迭代法实现中序遍历，因为中序遍历的迭代法稍微复杂一点，相比于其他两种。

![Untitled.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667030908512/y-JOjyx1g.png align="left")

那么这部分就使用递归法和迭代法实现中序遍历，LeetCode的[**94**](https://leetcode.cn/problems/binary-tree-inorder-traversal/)题

### 中序遍历的递归法

写递归时，可按照三部曲来书写，就不会写出内存溢出等有问题的代码

1. 确定递归函数及函数的参数及返回值
2. 确定递归函数的终止条件
3. 书写递归逻辑

```jsx
/**
 * Definition for a binary tree node.
 * function TreeNode(val, left, right) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.left = (left===undefined ? null : left)
 *     this.right = (right===undefined ? null : right)
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[]}
 */
var inorderTraversal = function(root, res = []) {
    if (root === null) {
        return res;
    }
		// 1. 确认递归函数及参数
    const dfs = (root) => {
				// 2. 递归函数的终止条件
        if (root === null) {
            return;
        }
				// 3. 递归逻辑，是中序遍历为左中右
        root.left && dfs(root.left);
        res.push(root.val);
        root.right && dfs(root.right);
    }
    dfs(root);
    return res;
};
```

### 中序遍历的迭代法

中序遍历的迭代法与前、后序遍历不同。是因为中序遍历的迭代法在访问和处理节点的顺序不一致

```jsx
var inorderTraversal = function(root, res = []) {
    const stack = []
    let cur = root;
    while (stack.length || cur) {
        if (cur) {
            stack.push(cur)
            cur = cur.left
        } else {
            const val = stack.pop();
            res.push(val.val);
            cur = val.right
        }
    }
    return res;
};
```

### 层序遍历

使用队列实现层序遍历

如 leetCode [**102 二叉树的层序遍历**](https://leetcode.cn/problems/binary-tree-level-order-traversal/)

```jsx
var levelOrder = function(root) {
    const result = [];
    const queue = [root];
    if (!root) {
        return result;
    }
    while (queue.length) {
        let length = queue.length;
        const curQueue = [];
        for (let i = 0; i < length; i++) {
            const val = queue.shift();
            curQueue.push(val.val)
            val.left && queue.push(val.left);
            val.right && queue.push(val.right);
        }
        result.push(curQueue);
    }
    return result;
};
```

> 二叉树后面所有类型的题都是可以通过前中后序遍历或者层序遍历来解决，就看哪种方式更合适
> 

## 二叉树的属性

![Untitled 1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667031081997/ZJCUenV7G.png align="left")

上图这九种类型的题，都是可以通过层序遍历或前中后序遍历来解决。

如 [101.对称二叉树中](https://leetcode.cn/problems/symmetric-tree/)** 就可以使用层序遍历将根节点的左右子树 push 到队列中，依次取出左右节点，判断节点的值是否相等，但是需要注意的是再往队列中 push 时，需要注意对应关系，先是左节点的左，再是右节点的右，否则会比较错误。

![Untitled 2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667031039013/hmytCN5zH.png align="left")

```jsx
var isSymmetric = function(root) {
    if (!root) {
        return true;
    }
    const queue = [];
    queue.push(root.left);
    queue.push(root.right);
    while (queue.length) {
        const leftNode = queue.shift();
        const rightNode = queue.shift();
        if (leftNode === null && rightNode === null) {
            continue;
        }
        if (leftNode === null || rightNode === null || leftNode.val !== rightNode.val) {
            return false;
        }
        queue.push(leftNode.left);
        queue.push(rightNode.right);
        queue.push(leftNode.right);
        queue.push(rightNode.left);
    }
    return true;
};
```

## 二叉树的修改


![Untitled 3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667031196100/c-rZesx1D.png align="left")

这就到了经典题目，翻转二叉树

翻转二叉树可以使用前序遍历和层序遍历解决，中序遍历不能处理该问题，会导致某个节点被翻转两次

翻转二叉树的迭代法

```jsx
var invertTree = function(root) {
    if (!root) {
        return root;
    }
    const queue = [root];
    while (queue.length) {
        let length = queue.length;
        while (length--) {
            const node = queue.shift();
            [node.left, node.right] = [node.right, node.left]
            node.left && queue.push(node.left)
            node.right && queue.push(node.right)
        }
    }
    return root;
};
```

翻转二叉树的递归法

```jsx
var invertTree = function(root) {
    const dfs = (node) => {
        if (!node) {
            return node
        }
        [node.left, node.right] = [node.right, node.left];
        dfs(node.left)
        dfs(node.right)
    };
    dfs(root);
    return root;
};
```

## 二叉搜索树的属性

![Untitled 4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667031231828/IVEIU2FFB.png align="left")

二叉搜索树是一种特殊的二叉树，有如下特征

- 若它的左子树不空，则左子树上所有结点的值均小于它的根结点的值；
- 若它的右子树不空，则右子树上所有结点的值均大于它的根结点的值；
- 它的左、右子树也分别为二叉搜索树

根据二叉搜索树的描述，它天然就可以使用中序遍历，元素就会由小到大排列。

这其中比较有难度的是[98.验证二叉搜索树题](https://leetcode.cn/problems/validate-binary-search-tree/)，不能单纯的比较左节点小于中间节点，右节点大于中间就完事了。这样就会有问题


![Untitled 5.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667031245732/IiZcC-QBv.png align="left")

有两种方法，**第一种是中序遍历，将元素保存在数组中，判断数组是否是递增的**。但是这样有些麻烦

第二种是就是使用递归时，同时判断

```jsx
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isValidBST = function(root) {
    let pre = null;
    const isBst = (node) => {
        if (!node) {
            return true;
        }
        let left = isBst(node.left);
        if (pre !== null && pre.val >= node.val) {
            return false;
        }
        pre = node;
        let right = isBst(node.right);
        return left && right;
    };
    return isBst(root);
};
```

## 二叉树的祖先问题

![Untitled 6.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1667031608573/7-U-o7fOv.png align="left")

如[236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/) 就可以使用递归来解决

```jsx
var lowestCommonAncestor = function(root, p, q) {
    const getCommon = (root, p, q) => {
        if (root === null || root === q || root === p) {
            return root;
        }
        let left = getCommon(root.left, p, q);
        let right = getCommon(root.right, p, q);
        if (left !== null && right !== null) {
            return root;
        }
        if (left === null) {
            return right
        }
        return left;
    };
    return getCommon(root, p, q)
};
```

二叉树还有一些其他的考题，如二叉树的构造等，但是在面试中都不会去考，只需要熟练的掌握翻转二叉树、二叉树的中序遍历的两种写法即可。