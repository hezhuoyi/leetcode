## 93. 复原IP地址
给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。
有效的 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。
例如："0.1.2.201" 和 "192.168.1.1" 是 有效的 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效的 IP 地址。

```js
/**
 * @param {string} s
 * @return {string[]}
 */
// 回溯 DFS
var restoreIpAddresses = function (s) {
    const res = [];
    if (s.length < 4 || s.length > 12) return res;
    function dfs(path, idx, len) {
        if (len === 4 && idx === s.length) return res.push(path);
        for (let i = 1; i < 4; i++) {
            const str = s.substring(idx, idx + i); // 每次截取1到3个
            if ((str[0] === '0' && str.length > 1) || Number(str) > 255 || len === 4) return;
            dfs(path + str + (len === 3 ? '' : '.'), idx + i, len + 1);
        }
    }
    dfs('', 0, 0);
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