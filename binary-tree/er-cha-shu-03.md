# 二叉树-03

## 左叶子之和

无法通过当前节点来判断其是否是个左叶子节点，必须依靠其父节点来判断出其是左叶子节点。

如果该节点的左节点不为空，该节点的左节点的左节点为空，该节点的左节点的右节点为空，则找到了一个左叶子，判断代码如下：

```cpp
if (node->left != NULL && node->left->left == NULL && node->left->right == NULL) {
    左叶子节点处理逻辑
}
```

### 递归

后序遍历（左右中），通过递归函数的返回值来达到累加所有左叶子节点之和的目的。

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
var sumOfLeftLeaves = function(root) {
    const postorder = (root) => { 
        if(root === null)
            return 0;

        let leftValue = postorder(root.left);
        let rightValue = postorder(root.right);

        let Value = 0;    
        if(root.left && root.left.left === null && root.left.right === null)
            Value = root.left.val;

        let sum = Value + leftValue + rightValue;
        return sum;
    }
    return postorder(root);
};
```

### 迭代

不是递归的话，前中后遍历都可以使用，下面采用前序遍历。

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
var sumOfLeftLeaves = function(root) {
    const res = [];
    if (!root)
        return res;
    let sum = 0;
    const stack = [root];
    while (stack.length) {
        cur = stack.pop();
        if(cur.left && cur.left.left === null && cur.left.right === null)
            sum += cur.left.val;
        cur.right && stack.push(cur.right);
        cur.left && stack.push(cur.left);
    }
    return sum;
};
```

## 找树左下角的值

该题需要找到最深一行，再去找最左边的元素。层序遍历解决这题很简单，遍历到最后一层即可。

### 递归

需要遍历完这棵树，找到最深深度，同时更新答案值。

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
var findBottomLeftValue = function(root) {
    let maxPath = 0, resNode = 0;
    const preorder = (node, curPath) => {
        if(!node.left && !node.right) {
            if(curPath > maxPath) {
                maxPath = curPath;
                resNode = node.val;
            }
        }
        node.left && preorder(node.left, curPath + 1);
        node.right && preorder(node.right, curPath + 1);
    }
    preorder(root, 1);
    return resNode;
};
```

### 迭代

层序遍历，更新答案值即可。

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
var findBottomLeftValue = function(root) {
    let resNode = 0;
    if (!root)
        return res;
    let queue = [];
    queue.push(root);
    while (queue.length) {
        let curLength = queue.length;   // 记录当前层的节点个数
        let curLevel = [];              // 保存当前层的数据
        for (let i = 0; i < curLength; ++i) {
            let cur = queue.shift();
            curLevel.push(cur.val);
            cur.left && queue.push(cur.left);
            cur.right && queue.push(cur.right);
        }
        resNode = curLevel[0];
    }
    return resNode;
};
```

## 路径之和

> 给你二叉树的根节点 root 和一个表示目标和的整数 targetSum ，判断该树中是否存在 根节点到叶子节点 的路径，这条路径上所有节点值相加等于目标和 targetSum 。

### 递归

> **如果需要搜索整颗二叉树，那么递归函数就不要返回值，如果要搜索其中一条符合条件的路径，递归函数就需要返回值，因为遇到符合条件的路径了就要及时返回。**

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
 * @param {number} targetSum
 * @return {boolean}
 */
var hasPathSum = function(root, targetSum) {
    const hasResult = (root, sum) => {
        if(!root)
            return false;
        if(!root.left && !root.right && sum === root.val)
            return true;
        return hasResult(root.left, sum - root.val) || hasResult(root.right, sum - root.val); 
    }
    return hasResult(root, targetSum); 
};
```



## 路径之和Ⅱ

> 给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。  
> 叶子节点 是指没有子节点的节点。

### 递归

因为要找到所有的路径，递归要遍历所有节点，因此递归函数没有返回值。

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
 * @param {number} targetSum
 * @return {number[][]}
 */
var pathSum = function(root, targetSum) {
    let resPath = [], curPath = [];
    const dfs = (cur, sum) => {        curPath.push(cur.val);
        sum -= cur.val;
        if(!cur.left && !cur.right && sum === 0) {
            resPath.push([...curPath]);
        }
        cur.left && dfs(cur.left, sum);
        cur.right && dfs(cur.right, sum);
        let temp = curPath.pop();
    }
    if(!root)
        return resPath;
    dfs(root, targetSum);
    return resPath;
};
```

## 从中序与后序遍历序列构造二叉树

切割法，以后序遍历得到的数组的最后一个元素先作为分割点，以此来切中序遍历得到的数组，再根据中序数组，反过来切后序数组。一层一层切下去，每次后序数组最后一个元素就是节点元素。

![](../.gitbook/assets/image%20%283%29.png)

> **`slice()`** 方法返回一个新的数组对象，这一对象是一个由 `begin` 和 `end` 决定的原数组的**浅拷贝**（包括 `begin`，不包括`end`）。原始数组不会被改变。

> ```javascript
> arr.slice([begin[, end]])
> ```

> **`findIndex()`**方法返回数组中满足提供的测试函数的第一个元素的**索引**。若没有找到对应元素则返回-1。

> ```
> arr.findIndex(callback[, thisArg])
> ```

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
 * @param {number[]} inorder
 * @param {number[]} postorder
 * @return {TreeNode}
 */
var buildTree = function(inorder, postorder) {
    if(!postorder.length)
        return null;
    let root = new TreeNode(postorder[postorder.length - 1]);
    let mid = inorder.findIndex((number) => number === root.val);
    root.left = buildTree(inorder.slice(0, mid), postorder.slice(0, mid));
    root.right = buildTree(inorder.slice(mid + 1, inorder.length), postorder.slice(mid, postorder.length - 1));
    return root;
};
```

## 从前序与中序遍历序列构造二叉树

解题思路和上题大致差不多，具体看代码。

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
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function(preorder, inorder) {
    if(!preorder.length)
        return null;
    let root = new TreeNode(preorder[0]);
    let mid = inorder.findIndex((number) => number === root.val);
    root.left = buildTree(preorder.slice(1, mid + 1), inorder.slice(0, mid));
    root.right = buildTree(preorder.slice(mid + 1, preorder.length), inorder.slice(mid + 1, inorder.length));
    return root;
};
```

## 最大二叉树

### 递归

每次调用时要检查数组的最大值，以此值作为划分点来划分数组。

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
 * @param {number[]} nums
 * @return {TreeNode}
 */
var constructMaximumBinaryTree = function (nums) {
    const BuildTree = (arr, left, right) => {
        if (left > right)
            return null;
        let maxValue = -1;
        let maxIndex = -1;
        for (let i = left; i <= right; ++i) {
            if (arr[i] > maxValue) {
                maxValue = arr[i];
                maxIndex = i;
            }
        }
        let root = new TreeNode(maxValue);
        root.left = BuildTree(arr, left, maxIndex - 1);
        root.right = BuildTree(arr, maxIndex + 1, right);
        return root;
    }
    let root = BuildTree(nums, 0, nums.length - 1);
    return root;
};
```

