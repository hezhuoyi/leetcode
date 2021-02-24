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

## 剑指 Offer 13. 机器人的运动范围
地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

```js
/**
 * @param {number} m
 * @param {number} n
 * @param {number} k
 * @return {number}
 */
// BFS
var movingCount = function (m, n, k) {
    let total = 0;
    const obj = { '0 0': true }; // 记录位置 防止重新访问
    const queue = [[0, 0]];
    const isValid = (x, y) => (x + '' + y).split('').reduce((a, b) => Number(a) + Number(b)) <= k;
    while (queue.length) {
        let len = queue.length;
        while (len--) {
            const [x, y] = queue.shift();
            total++;
            // 只需要考虑往右和下的方向
            if (x < m - 1 && !obj[`${x + 1} ${y}`] && isValid(x + 1, y)) {
                queue.push([x + 1, y]);
                obj[`${x + 1} ${y}`] = true;
            }
            if (y < n - 1 && !obj[`${x} ${y + 1}`] && isValid(x, y + 1)) {
                queue.push([x, y + 1]);
                obj[`${x} ${y + 1}`] = true;
            }
        }
    }
    return total;
};

var movingCount = function (m, n, k) {
    let total = 0;
    const obj = {};
    function dfs(i, j) {
        // 边界直接返回
        if (i < 0 || j < 0 || i >= m || j >= n) return;
        const sum = (i + '' + j).split('').reduce((a, b) => Number(a) + Number(b));
        const cur = `${i} ${j}`; // 记录位置 防止重新访问
        if (sum <= k && !obj[cur]) {
            total++;
            obj[cur] = true;
            dfs(i + 1, j);
            dfs(i, j + 1);
        }
    }
    dfs(0, 0);
    return total;
};
```