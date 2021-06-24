# 二叉树-02

## 对称二叉树

这里对比的是左右子树是否相等，而不是单纯比较左右节点是否相等。

![](../.gitbook/assets/image%20%281%29.png)

```javascript
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
 * @return {boolean}
 */
var isSymmetric = function (root) {
    const compareNode = function (left, right) {
        if (left === null && right !== null)
            return false;
        else if (left !== null && right === null)
            return false;
        else if (left === null && right === null)
            return true;
        else if (left.val !== right.val)
            return false;
        let Left = compareNode(left.left, right.right);
        let Right = compareNode(left.right, right.left);
        return Left && Right;
    }
    if (!root)
        return true;
    return compareNode(root.left, root.right);
};
```

## 二叉树的最大深度

### 递归法

后序遍历的变形

```javascript
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
 * @return {number}
 */
var maxDepth = function (root) {
    const postorder = (root) => {
        if (!root)
            return 0;
        return 1 + Math.max(postorder(root.left), postorder(root.right));
    }
    return postorder(root);
};
```

### 迭代法

利用二叉树的层序遍历解决，看有多少层，即可得出最大深度。

```javascript
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
 * @return {number}
 */
var maxDepth = function (root) {
    if (!root)
        return 0;
    let queue = [];
    let depth = 0;
    queue.push(root);
    while (queue.length) {
        let curLength = queue.length;   // 记录当前层的节点个数
        for (let i = 0; i < curLength; ++i) {
            let cur = queue.shift();
            cur.left && queue.push(cur.left);
            cur.right && queue.push(cur.right);
        }
        depth++;
    }
    return depth;
};
```

## N叉树的最大深度

无论递归还是迭代，思路和上题差不多。

### 递归法

```javascript
/**
 * // Definition for a Node.
 * function Node(val,children) {
 *    this.val = val;
 *    this.children = children;
 * };
 */

/**
 * @param {Node|null} root
 * @return {number}
 */
var maxDepth = function (root) {
    const postorder = (root) => {
        if (!root)
            return 0;
        let depth = 0;
        for (let i = 0; i < root.children.length; ++i)
            depth = Math.max(depth, postorder(root.children[i]));
        return 1 + depth;
    }
    return postorder(root);
};
```

### 迭代法

```javascript
/**
 * // Definition for a Node.
 * function Node(val,children) {
 *    this.val = val;
 *    this.children = children;
 * };
 */

/**
 * @param {Node|null} root
 * @return {number}
 */
var maxDepth = function (root) {
    if (!root)
        return 0;
    let queue = [];
    let depth = 0;
    queue.push(root);
    while (queue.length) {
        let curLength = queue.length;   // 记录当前层的节点个数
        for (let i = 0; i < curLength; ++i) {
            let cur = queue.shift();
            for (let j = 0; j < cur.children.length; ++j) {
                cur.children[j] && queue.push(cur.children[j]);
            }
        }
        depth++;
    }
    return depth;
};
```

## 二叉树的最小深度

访问真的的叶子节点即可。

### 递归法

```javascript
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
 * @return {number}
 */
var minDepth = function (root) {
    const postorder = (root) => {
        if (!root)
            return 0;
        if (!root.left && root.right)
            return 1 + postorder(root.right);
        if (root.left && !root.right)
            return 1 + postorder(root.left);
        return 1 + Math.min(postorder(root.left), postorder(root.right));
    }
    return postorder(root);
};
```

### 迭代法

```javascript
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
 * @return {number}
 */
var minDepth = function (root) {
    if (!root)
        return 0;
    let queue = [];
    let depth = 0;
    queue.push(root);
    while (queue.length) {
        let curLength = queue.length;
        depth++;
        for (let i = 0; i < curLength; ++i) {
            let cur = queue.shift();
            cur.left && queue.push(cur.left);
            cur.right && queue.push(cur.right);
            if (!cur.left && !cur.right)
                return depth;
        }
    }
    return depth;

```

## 完全二叉树的节点个数

### 普通二叉树做法---递归

```javascript
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
 * @return {number}
 */
var countNodes = function(root) {
    const postorder = (root) => {
        if(!root)
            return 0;
        return 1 + postorder(root.left) + postorder(root.right);
    }
    return postorder(root);
};
```

### 普通二叉树做法--迭代

层序遍历 记录个数

```javascript
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
 * @return {number}
 */
var countNodes = function(root) {
    if(!root)
        return 0;
    const queue = [root];
    let sum = 0;
    while(queue.length) {
        let curLength = queue.length;
        let cur = queue.shift();
        sum++;
        cur.left && queue.push(cur.left);
        cur.right && queue.push(cur.right);
    }
    return sum;
};
```

## 完全二叉树做法

完全二叉树只有两种情况:  
  
    1.  满二叉树;  
         直接用 2^树深度 - 1 来计算，注意这里根节点深度为1。  
    2. 最后一层叶子节点没有满。  
         分别递归左孩子，和右孩子，递归到某一深度一定会有左孩子或者右孩子为满二叉树，然后依然可以按照情况1来计算。

```javascript
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
 * @return {number}
 */
var countNodes = function(root) {
    if(!root)
        return 0;
    const getLeftHeight = (root) => {
        let c = 0;
        while(root) {
            root = root.left;
            c++;
        }
        return c;
    }
    const getRightHeight = (root) => {
        let c = 0;
        while(root) {
            root = root.right;
            c++;
        }
        return c;
    }
    const getCount = (root) => {
        let countl = getLeftHeight(root);
        let countr = getRightHeight(root);
        if(countl === countr) {
            return Math.pow(2, countl) - 1;
        }
        return getCount(root.left) + getCount(root.right) + 1;
    }
    return getCount(root);
};
```

## 平衡二叉树

一个二叉树_每个节点_ 的左右两个子树的高度差的绝对值不超过 1 。

```javascript
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
 * @return {boolean}
 */
var isBalanced = function(root) {
    const getDepth = (root) => {
        if(!root)
            return 0;
        let leftDepth = getDepth(root.left);
        if(leftDepth == -1)
            return -1;      // 说明此时左子树不是个平衡二叉树
        let rightDepth = getDepth(root.right);
        if(rightDepth == -1)
            return -1;      // 说明此事右子树不是个平衡二叉树
        return Math.abs(leftDepth - rightDepth) > 1 ? -1 : 1 + Math.max(leftDepth, rightDepth)
    }
    return getDepth(root) == -1 ? false : true;
};
```

## 二叉树的所有路径

给定一个二叉树，返回所有从根节点到叶子节点的路径。  
**说明:** 叶子节点是指没有子节点的节点。

很简单，利用前序遍历，在每次遍历的过程中，只要访问到一个叶子节点就停下，记录下当前经过的路径，直至访问至最后一个叶子节点，便返回所有结果。

```javascript
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
 * @return {string[]}
 */
var binaryTreePaths = function(root) {
    // DFS 深度遍历
    let res = [];
    const getPath = (node, curPath) => {
        if(!node.left && !node.right) {
            curPath += node.val;
            res.push(curPath);
            return;
        }
        curPath += node.val + "->";
        node.left && getPath(node.left, curPath);
        node.right && getPath(node.right, curPath);
    }
    getPath(root, "");
    return res;
};
```

