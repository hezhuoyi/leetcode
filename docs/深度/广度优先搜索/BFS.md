## 515. 在每个树行中找最大值
您需要在二叉树的每一行中找到最大的值。

```js
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
    if (!root) return [];
    const nodes = [root];
    const res = [];
    while (nodes.length) {
        let len = nodes.length;
        let maxValue = Number.MIN_SAFE_INTEGER;
        while (len--) {
            const node = nodes.shift();
            maxValue = Math.max(maxValue, node.val);
            if (node.left) nodes.push(node.left);
            if (node.right) nodes.push(node.right);
        }
        res.push(maxValue);
    }
    return res;
};
```

## 22. 括号生成
数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

```js
/**
 * @param {number} n
 * @return {string[]}
 */
// BFS
var generateParenthesis = function (n) {
    const queue = ['('];
    const res = [];
    while (queue.length) {
        const str = queue.shift();
        const left = str.split('').filter(s => s === '(').length; // '(' 的数量
        const right = str.split('').filter(s => s === ')').length; // ')' 的数量
        if (left === n && right === n) res.push(str);
        if (left < n) queue.push(str + '(');
        if (right < left) queue.push(str + ')');
    }
    return res;
};
// 回溯 深度优先遍历
var generateParenthesis = function (n) {
    const res = [];
    const backtrack = (path, start, end) => {
        if (path.length === n * 2) return res.push(path);
        if (start < n) backtrack(path + '(', start + 1, end);
        if (end < start) backtrack(path + ')', start, end + 1);
    };
    backtrack('', 0, 0);
    return res;
};
```