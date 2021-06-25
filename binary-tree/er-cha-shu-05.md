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

