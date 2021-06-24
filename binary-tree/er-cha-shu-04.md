# 二叉树-04

## 合并二叉树

合并的大致操作也可以说是做了两次遍历的同时进行比较，然后重新设置。二叉树的三种前中后序遍历都可以使用，递归最好。

### 前序遍历的递归

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
 * @param {TreeNode} root1
 * @param {TreeNode} root2
 * @return {TreeNode}
 */
var mergeTrees = function (root1, root2) {
    const preOrder = (root1, root2) => {
        if (!root1)
            return root2
        if (!root2)
            return root1;
        root1.val += root2.val;
        root1.left = preOrder(root1.left, root2.left);
        root1.right = preOrder(root1.right, root2.right);
        return root1;
    }
    return preOrder(root1, root2);
};
```

## 二叉搜索树中的搜索

这太简单了，就是二分思想。

### 递归

左小右大中同，递归解决最好。因为只需要寻找一条边，所以必须要有返回值。

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
 * @param {number} val
 * @return {TreeNode}
 */
var searchBST = function (root, val) {
    if (!root || root.val === val) {
        return root;
    }
    if (root.val > val)
        return searchBST(root.left, val);
    if (root.val < val)
        return searchBST(root.right, val);
    return null;
};
```

### 迭代

二叉搜索树结构的独特也是得它的迭代遍历很简洁

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
 * @param {number} val
 * @return {TreeNode}
 */
var searchBST = function (root, val) {
    while (root !== null) {
        if (root.val > val)
            root = root.left;
        else if (root.val < val)
            root = root.right;
        else 
            return root;
    }
    return root;
};
```

## 验证二叉搜索树

利用二叉搜索树的特性：其中序遍历的结果应是递增的。

中序遍历二叉树得到一个结果序列，判断该序列是否递增。递增则该二叉树为搜索树，反之不是。

### 辅助数组解决

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
var isValidBST = function (root) {
    let arr = [];
    const buildArr = (root) => {
        if (root) {
            buildArr(root.left);
            arr.push(root.val);
            buildArr(root.right);
        }
    }
    buildArr(root);
    for (let i = 1; i < arr.length; ++i) {
        if (arr[i] <= arr[i - 1])
            return false;
    }
    return true;
};
```

### 递归中判断

可以在中序遍历的递归中每次的判断是否符合二叉搜索的特性，对于每一个节点来说，左边的所有节点都比它小，右边的都比它大。

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
let pre = null;
var isValidBST = function (root) {
    let pre = null;
    const inOrder = (root) => {
        if (root === null)
            return true;
        let left = inOrder(root.left);

        if (pre !== null && pre.val >= root.val)
            return false;
        pre = root;

        let right = inOrder(root.right);
        return left && right;
    }
    return inOrder(root);
};
```

## 二叉搜索树的最小绝对差

可以把该题看作是验证二叉搜索树的一个扩展，增添个计算最小绝对差的功能。

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
var getMinimumDifference = function (root) {
    let arr = [];
    const buildArr = (root) => {
        if (root) {
            buildArr(root.left);
            arr.push(root.val);
            buildArr(root.right);
        }
    }
    buildArr(root);
    let diff = arr[arr.length - 1];
    for (let i = 1; i < arr.length; ++i) {
        if (diff > arr[i] - arr[i - 1])
            diff = arr[i] - arr[i - 1];
    }
    return diff;
};
```

## **二叉搜索树中的众数**

一个最直接的想法将树全部遍历一次，使用`map`这个数据结构记录结果即可。

另一种则是利用其二叉搜索树的特点：其中序遍历的结果是单调递增的。

在遍历过程中，多个`pre`指针记录前一个节点，和`cur` 指针指向的节点作比较，相等更新`count`值，反之重置`count`的值。

`splice`方法能够对数组进行增添和删除操作，常用来将数组重置为空数组。

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
 * @return {number[]}
 */
var findMode = function (root) {
    let maxCount = 0;
    let curCount = 0;
    let pre = null;
    let res = [];
    const inOrder = (root) => {
        if (root === null)
            return;
        inOrder(root.left);

        if (pre === null)
            curCount = 1;
        else if (pre.val === root.val)
            curCount++;
        else
            curCount = 1;
        pre = root;

        if (curCount === maxCount)
            res.push(root.val);

        if (curCount > maxCount) {
            maxCount = curCount;
            res.splice(0, res.length);
            res.push(root.val);
        }

        inOrder(root.right);
        return;
    }
    inOrder(root);
    return res;
};
```

##  **二叉树的最近公共祖先** 

后序遍历是天然的回溯过程，实现从低向上的遍历方式。

```javascript
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {TreeNode} p
 * @param {TreeNode} q
 * @return {TreeNode}
 */
var lowestCommonAncestor = function(root, p, q) {
    if(root === p || root === q || root === null)
        return root;
    let left = lowestCommonAncestor(root.left, p , q);
    let right = lowestCommonAncestor(root.right, p, q);
    if(left && right)
        return root;
    if(!left)
        return right;
    return left;
};
```

\*\*\*\*

