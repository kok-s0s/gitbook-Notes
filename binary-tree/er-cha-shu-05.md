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



