# 二叉树-05

## **二叉搜索树的最近公共祖先**

二叉搜索树中序遍历的结果是个有序序列，求两个节点的最近公共祖先，其实就是找这两个节点值所形成的区间里是否有节点值落在其中，有则说明其是个公共祖先。反证，如果当前节点不在中间区间，若当前节点的值都小于两个节点，则要去右子树找答案，反之是去左子树找答案。

### 递归

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
    if(root.val > p.val && root.val > q.val)
        return lowestCommonAncestor(root.left, p , q);
    else if(root.val < p.val && root.val < q.val) 
        return lowestCommonAncestor(root.right, p , q);
    return root;
};
```

### 迭代

```text
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
    while(1) {
        if(root.val > p.val && root.val > q.val)
            root = root.left;
        else if(root.val < p.val && root.val < q.val)
            root = root.right;
        else
            break;
    }
    return root;
};
```

## **二叉搜索树中的插入操作**

**遍历，就是遍历罢了，遍历到一个合适的空姐点位置，将要加入的节点取代即可。**

### 有返回值的递归

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
var insertIntoBST = function (root, val) {
    const setInOrder = (root, val) => {
        if (root === null) {
            let node = new TreeNode(val);
            return node;
        }
        if (root.val > val)
            root.left = setInOrder(root.left, val);
        else if (root.val < val)
            root.right = setInOrder(root.right, val);
        return root;
    }
    return setInOrder(root, val);
};
```

### 无返回值的递归

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
var insertIntoBST = function (root, val) {
    let parent = new TreeNode(0);
    const preOrder = (cur, val) => {
        if (cur === null) {
            let node = new TreeNode(val);
            if (parent.val > val)
                parent.left = node;
            else
                parent.right = node;
            return;
        }
        parent = cur;
        if (cur.val > val)
            preOrder(cur.left, val);
        if (cur.val < val)
            preOrder(cur.right, val);
    }
    if (root === null)
        root = new TreeNode(val);
    preOrder(root, val);
    return root;
};
```

### 迭代

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
var insertIntoBST = function (root, val) {
    if (root === null) {
        root = new TreeNode(val);
    } else {
        let parent = new TreeNode(0);
        let cur = root;
        while (cur) {
            parent = cur;
            if (cur.val > val)
                cur = cur.left;
            else
                cur = cur.right;
        }
        let node = new TreeNode(val);
        if (parent.val > val)
            parent.left = node;
        else
            parent.right = node;
    }
    return root;
};
```

## **删除二叉搜索树中的节点**

### **递归**

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
 * @param {number} key
 * @return {TreeNode}
 */
var deleteNode = function (root, key) {
    if (root === null)
        return root;
    if (root.val === key) {
        if (!root.left)
            return root.right;
        else if (!root.right)
            return root.left;
        else {
            let cur = root.right;
            while (cur.left) {
                cur = cur.left;
            }
            cur.left = root.left;
            let temp = root;
            root = root.right;
            delete root;
            return root;
        }
    }
    if (root.val > key)
        root.left = deleteNode(root.left, key);
    if (root.val < key)
        root.right = deleteNode(root.right, key);
    return root;
};
```

## 修剪二叉搜索树

### 递归

区间是两边都是闭合的，先处理左右两边，如果当前节点的值大于`high`，则考虑向左边寻找答案，如果当前节点的值小于`low`，则考虑向右边寻找答案，如果当前的节点的值处于`low`和`high`之间，则其节点的左右子树应当继续寻求答案，做相关剪枝的操作。

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
 * @param {number} low
 * @param {number} high
 * @return {TreeNode}
 */
var trimBST = function (root, low, high) {
    if (root === null)
        return null;
    if (root.val < low) {
        return trimBST(root.right, low, high);
    }
    if (root.val > high) {
        return trimBST(root.left, low, high);
    }
    root.left = trimBST(root.left, low, high);
    root.right = trimBST(root.right, low, high);
    return root;
};
```

### 迭代

利用二叉搜索树的有序性

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
 * @param {number} low
 * @param {number} high
 * @return {TreeNode}
 */
var trimBST = function (root, low, high) {
    // 确保root的值在low和high之间
    if (root === null)
        return null;
    while (root !== null && (root.val > high || root.val < low)) {
        if (root.val > high)
            root = root.left;
        else
            root = root.right;
    }

    // 剪枝左子树
    let curNode = root;
    while (curNode !== null) {
        while (curNode.left && curNode.left.val < low)
            curNode.left = curNode.left.right;
        curNode = curNode.left;
    }

    // 剪枝右子树
    curNode = root;
    while (curNode !== null) {
        while (curNode.right && curNode.right.val > high)
            curNode.right = curNode.right.left;
        curNode = curNode.right;
    }
    return root;
};
```

## **将有序数组转换为二叉搜索树**

### 递归

> `int mid = left + ((right - left) / 2);`的写法相当于是如果数组长度为偶数，中间位置有两个元素，取靠左边的。

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
var sortedArrayToBST = function (nums) {
    const buildTree = (Arr, left, right) => {
        if (left > right)
            return null;

        let mid = Math.floor(left + (right - left) / 2);

        let root = new TreeNode(Arr[mid]);
        root.left = buildTree(Arr, left, mid - 1);
        root.right = buildTree(Arr, mid + 1, right);
        return root;
    }
    return buildTree(nums, 0, nums.length - 1);
};
```

javascript 要注意其整除，在该题需要使用`Math.floor`向下取整。

## 把二叉搜索树转换为累加树

### 递归

反中序遍历该二叉树，然后顺序累加，用`pre`来记录前一个节点的值。

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
 * @return {TreeNode}
 */
var convertBST = function(root) {
    let pre = 0;
    const ReverseInOrder = (cur) => {
        if(cur) {
            ReverseInOrder(cur.right);
            cur.val += pre;
            pre = cur.val;
            ReverseInOrder(cur.left);
        }
    }
    ReverseInOrder(root);
    return root;
};
```

### 迭代

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
 * @return {TreeNode}
 */
var convertBST = function (root) {
    let pre = 0;
    let cur = root;
    let stack = [];
    while (cur !== null || stack.length !== 0) {
        while (cur !== null) {
            stack.push(cur);
            cur = cur.right;
        }
        cur = stack.pop();
        cur.val += pre;
        pre = cur.val;
        cur = cur.left;
    }
    return root;
};
```

