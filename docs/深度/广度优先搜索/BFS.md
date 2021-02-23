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

## 695. 岛屿的最大面积
给定一个包含了一些 0 和 1 的非空二维数组 grid 。
一个 岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。
找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0 。)

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var maxAreaOfIsland = function (grid) {
    if (!grid.length) return 0;
    const m = grid.length, n = grid[0].length;
    let count = max = 0;
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] === 1) {
                count = 0;
                bfs(i, j);
                max = Math.max(max, count);
            }
        }
    }
    return max;
    function bfs(i, j) {
        const queue = [[i, j]];
        grid[i][j] = 0; // 重置当前grid 下同
        count++;
        while (queue.length) {
            const len = queue.length;
            for (let k = 0; k < len; k++) {
                const [row, col] = queue.shift();
                if (row + 1 < m && grid[row + 1][col] === 1) {
                    grid[row + 1][col] = 0;
                    count++;
                    queue.push([row + 1, col]);
                }
                if (col + 1 < n && grid[row][col + 1] === 1) {
                    grid[row][col + 1] = 0;
                    count++;
                    queue.push([row, col + 1]);
                }
                if (row - 1 >= 0 && grid[row - 1][col] === 1) {
                    grid[row - 1][col] = 0;
                    count++;
                    queue.push([row - 1, col]);
                }
                if (col - 1 >= 0 && grid[row][col - 1] === 1) {
                    grid[row][col - 1] = 0;
                    count++;
                    queue.push([row, col - 1]);
                }
            }
        }
    }
};

var maxAreaOfIsland = function (grid) {
    if (!grid.length) return 0;
    const m = grid.length, n = grid[0].length;
    let count = max = 0;
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if (grid[i][j] === 1) {
                count = 0;
                dfs(i, j);
                max = Math.max(max, count);
            }
        }
    }
    return max;

    function dfs(i, j) {
        grid[i][j] = 0;
        count++;
        if (i + 1 < m && grid[i + 1][j] === 1) dfs(i + 1, j);
        if (j + 1 < n && grid[i][j + 1] === 1) dfs(i, j + 1);
        if (i - 1 >= 0 && grid[i - 1][j] === 1) dfs(i - 1, j);
        if (j - 1 >= 0 && grid[i][j - 1] === 1) dfs(i, j - 1);
    }
};
```