# 二叉树-01

## 二叉树的递归遍历

#### **递归算法的三个要素**：

1. **确定递归函数的参数和返回值：** 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。
2. **确定终止条件：** 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。
3. **确定单层递归的逻辑：** 确定每一层递归需要处理的信息。即是会重复调用自己来实现递归的过程。

###  二叉树的前序遍历

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
var preorderTraversal = function (root) {
    const res = [];
    const preorder = (root) => {
        if (root) {
            res.push(root.val);
            preorder(root.left);
            preorder(root.right);
        }
    }
    preorder(root);
    return res;
};
```

### 二叉树的中序遍历

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
var inorderTraversal = function (root) {
    const res = [];
    const inorder = (root) => {
        if (root) {
            inorder(root.left);
            res.push(root.val);
            inorder(root.right);
        }
    }
    inorder(root);
    return res;
};
```

### 二叉树的后序遍历

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
var postorderTraversal = function (root) {
    const res = [];
    const postorder = (root) => {
        if (root) {
            postorder(root.left);
            postorder(root.right);
            res.push(root.val);
        }
    }
    postorder(root);
    return res;
};
```

## 二叉树的迭代遍历

### 二叉树的前序遍历

进栈顺序：中右左，因为这样出栈顺序才会是中左右，一般先把节点放进去，然后pop出，取其右子节点push进去，再push左节点，这样pop节点的顺序就会是左右的了。

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
var preorderTraversal = function (root) {
    const res = [];
    if (!root)
        return res;
    const stack = [root];
    while (stack.length) {
        cur = stack.pop();
        res.push(cur.val);
        cur.right && stack.push(cur.right);
        cur.left && stack.push(cur.left);
    }
    return res;
};
```

### 二叉树的中序遍历

借助指针的遍历来帮助访问节点，栈则用来处理节点上的元素。

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
var inorderTraversal = function (root) {
    const res = [];
    if (!root)
        return res;
    const stack = [];
    while (stack.length || root) {
        while (root) {
            stack.push(root);
            root = root.left;
        }
        root = stack.pop();
        res.push(root.val);
        root = root.right;
    }
    return res;
};
```

### 二叉树的后序遍历

后序的迭代和前序的相反，左右顺序进栈，则会是右左顺序出栈。得到的结果最后需要逆序。因为前序遍历是中左右，而后序遍历是左右中。

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
var postorderTraversal = function (root) {
    const res = [];
    if (!root)
        return res;
    const stack = [root];
    while (stack.length) {
        cur = stack.pop();
        cur.left && stack.push(cur.left);
        cur.right && stack.push(cur.right);
        res.push(cur.val);
    }
    return res.reverse();
};
```

## 二叉树的统一迭代法

将访问的节点放入栈中，把要处理的节点也放入栈中但是要做标记（就是将要处理的节点放入栈之后，紧接着放入一个空指针作为标记）， 这种方法也称做标记法。

### 二叉树的中序遍历---统一风格

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
var inorderTraversal = function (root) {
    const res = [];
    if (!root)
        return res;
    const stack = [root];
    while (stack.length) {
        let cur = stack.pop();
        stack.push(cur);
        if (cur != null) {
            stack.pop();                            // 将当前节点弹出，避免重复操作，下面操作再将其添加进栈中
            cur.right && stack.push(cur.right);     // 将右节点push进栈（空节点不push）
            stack.push(cur);                        // 将当前节点push进栈
            stack.push(null);                       // push个空节点作为标记
            cur.left && stack.push(cur.left);       // 将左节点push进栈（空节点不push）
        } else {
            stack.pop();                            // 将当前空节点pop出
            cur = stack.pop();
            res.push(cur.val);
        }
    }
    return res;
};
```

### 二叉树的前序遍历---统一风格

仅改变中序遍历中一行代码的位置即可

进栈顺序：右中左 ---&gt; 右左中

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
var preorderTraversal = function (root) {
    const res = [];
    if (!root)
        return res;
    const stack = [root];
    while (stack.length) {
        let cur = stack.pop();
        stack.push(cur);
        if (cur != null) {
            stack.pop();                            // 将当前节点弹出，避免重复操作，下面操作再将其添加进栈中
            cur.right && stack.push(cur.right);     // 将右节点push进栈（空节点不push）
            cur.left && stack.push(cur.left);       // 将左节点push进栈（空节点不push）
            stack.push(cur);                        // 将当前节点push进栈
            stack.push(null);                       // push个空节点作为标记
        } else {
            stack.pop();                            // 将当前空节点pop出
            cur = stack.pop();
            res.push(cur.val);
        }
    }
    return res;
};
```

### 二叉树的后序遍历---统一风格

仅改变中序遍历中两行代码的位置即可

进栈顺序：右中左 ---&gt; 中右左

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
var postorderTraversal = function (root) {
    const res = [];
    if (!root)
        return res;
    const stack = [root];
    while (stack.length) {
        let cur = stack.pop();
        stack.push(cur);
        if (cur != null) {
            stack.pop();                            // 将当前节点弹出，避免重复操作，下面操作再将其添加进栈中
            stack.push(cur);                        // 将当前节点push进栈
            stack.push(null);                       // push个空节点作为标记
            cur.right && stack.push(cur.right);     // 将右节点push进栈（空节点不push）
            cur.left && stack.push(cur.left);       // 将左节点push进栈（空节点不push）
        } else {
            stack.pop();                            // 将当前空节点pop出
            cur = stack.pop();
            res.push(cur.val);
        }
    }
    return res;
};
```

## 二叉树的层序遍历

借助队列完成遍历 BFS

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
 * @return {number[][]}
 */
var levelOrder = function (root) {
    let res = [];
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
        res.push(curLevel);             // 更新结果
    }
    return res;
};
```

## 二叉树的层序遍历Ⅱ

直接将上提代码最后返回的 res 逆序即可。

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
 * @return {number[][]}
 */
var levelOrderBottom = function (root) {
    let res = [];
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
        res.push(curLevel);             // 更新结果
    }
    return res.reverse();
};
```

## 二叉树的右视图

层序遍历的时候，判断是否遍历到单层的最后的元素。

如果是，就放进result数组中，随后返回result就可以了。

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
var rightSideView = function(root) {
    let res = [];
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
        res.push(curLevel.pop());       // 将每层的最后一个元素添加进 res 数组中
    }
    return res;
};
```

## **二叉树的层平均值**

层序遍历的时候，将每层的数据做个平均值，将计算的结果添加进 res 数组中。

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
var averageOfLevels = function(root) {
    let res = [];
    if (!root)
        return res;
    let queue = [];
    queue.push(root);
    while (queue.length) {
        let curLength = queue.length;   // 记录当前层的节点个数
        let sum = 0;
        for (let i = 0; i < curLength; ++i) {
            let cur = queue.shift();
            sum += cur.val;
            cur.left && queue.push(cur.left);
            cur.right && queue.push(cur.right);
        }
        res.push(sum / curLength);       // 将计算出的平均值加入 res 中
    }
    return res;
};
```

## **N 叉树的层序遍历**

和二叉树的层序遍历相差不大，修改添加节点那块的代码即可。

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
 * @return {number[][]}
 */
var levelOrder = function (root) {
    let res = [];
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
            for (let item of cur.children) {
                item && queue.push(item);
            }
        }
        res.push(curLevel);             // 更新结果
    }
    return res;
};
```

## 在每个树行中找最大值

使用 Math 中的 max/min 方法, 可以使用apply来实现, apply传入的是一个数组。

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
var largestValues = function (root) {
    let res = [];
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
        res.push(Math.max.apply(null, curLevel));
    }
    return res;
};
```

## 填充每个节点的下一个右侧节点指针

层序遍历，利用两个指针将遍历的元素一个个串起来。

```javascript
/**
 * // Definition for a Node.
 * function Node(val, left, right, next) {
 *    this.val = val === undefined ? null : val;
 *    this.left = left === undefined ? null : left;
 *    this.right = right === undefined ? null : right;
 *    this.next = next === undefined ? null : next;
 * };
 */

/**
 * @param {Node} root
 * @return {Node}
 */
var connect = function (root) {
    if (!root)
        return root;
    let queue = [root];
    while (queue.length) {
        const curLength = queue.length;
        let nodePre;
        let node;
        for (let i = 0; i < curLength; ++i) {
            if (i == 0) {
                nodePre = queue.shift();
                node = nodePre;
            } else {
                node = queue.shift();
                nodePre.next = node;
                nodePre = nodePre.next;
            }
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
        nodePre.next = null;
    }
    return root;
};
```

## **填充每个节点的下一个右侧节点指针 II**

和上题代码一样，主要可以使用相同的逻辑来解决问题。

```javascript
/**
 * // Definition for a Node.
 * function Node(val, left, right, next) {
 *    this.val = val === undefined ? null : val;
 *    this.left = left === undefined ? null : left;
 *    this.right = right === undefined ? null : right;
 *    this.next = next === undefined ? null : next;
 * };
 */

/**
 * @param {Node} root
 * @return {Node}
 */
var connect = function (root) {
    if (!root)
        return root;
    let queue = [root];
    while (queue.length) {
        const curLength = queue.length;
        let nodePre;
        let node;
        for (let i = 0; i < curLength; ++i) {
            if (i == 0) {
                nodePre = queue.shift();
                node = nodePre;
            } else {
                node = queue.shift();
                nodePre.next = node;
                nodePre = nodePre.next;
            }
            node.left && queue.push(node.left);
            node.right && queue.push(node.right);
        }
        nodePre.next = null;
    }
    return root;
};
```

## 翻转二叉树

递归法 交换左右节点即可

 **递归的中序遍历是不行的，因为使用递归的中序遍历，某些节点的左右孩子会翻转两次。**

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
var invertTree = function (root) {
    if (!root)
        return root;
    let temp = root.left;
    root.left = root.right;
    root.right = temp;
    invertTree(root.left);
    invertTree(root.right);
    return root;
};
```



