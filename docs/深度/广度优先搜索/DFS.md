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

## 733. 图像渲染
有一幅以二维整数数组表示的图画，每一个整数表示该图画的像素值大小，数值在 0 到 65535 之间。
给你一个坐标 (sr, sc) 表示图像渲染开始的像素值（行 ，列）和一个新的颜色值 newColor，让你重新上色这幅图像。
为了完成上色工作，从初始坐标开始，记录初始坐标的上下左右四个方向上像素值与初始坐标相同的相连像素点，接着再记录这四个方向上符合条件的像素点与他们对应四个方向上像素值与初始坐标相同的相连像素点，……，重复该过程。将所有有记录的像素点的颜色值改为新的颜色值。
最后返回经过上色渲染后的图像。

```js
/**
 * @param {number[][]} image
 * @param {number} sr
 * @param {number} sc
 * @param {number} newColor
 * @return {number[][]}
 */
var floodFill = function (image, sr, sc, newColor) {
    const oldColor = image[sr][sc];
    if (oldColor === newColor) return image;
    const dfs = (x, y) => {
        if (x < 0 || y < 0 || x >= image.length || y >= image[0].length || image[x][y] !== oldColor) return;
        image[x][y] = newColor;
        dfs(x - 1, y);
        dfs(x, y - 1);
        dfs(x + 1, y);
        dfs(x, y + 1);
    };
    dfs(sr, sc);
    return image;
};

// BFS
var floodFill = function (image, sr, sc, newColor) {
    const oldColor = image[sr][sc];
    if (oldColor === newColor) return image;
    const queue = [[sr, sc]];
    while (queue.length) {
        let len = queue.length;
        while (len--) {
            const [x, y] = queue.shift();
            image[x][y] = newColor;
            if (x - 1 >= 0 && image[x - 1][y] === oldColor) queue.push([x - 1, y]); 
            if (y - 1 >= 0 && image[x][y - 1] === oldColor) queue.push([x, y - 1]);
            if (x + 1 < image.length && image[x + 1][y] === oldColor) queue.push([x + 1, y]); 
            if (y + 1 < image[0].length && image[x][y + 1] === oldColor) queue.push([x, y + 1]);
        }
    }
    return image;
};
```

## 463. 岛屿的周长
给定一个 row x col 的二维网格地图 grid ，其中：grid[i][j] = 1 表示陆地， grid[i][j] = 0 表示水域。
网格中的格子 水平和垂直 方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。
岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
const islandPerimeter = (grid) => {
    const dfs = (i, j) => {
        if (i < 0 || j < 0 || i >= grid.length || j >= grid[0].length || grid[i][j] === 0) return 1;
        if (grid[i][j] === 2) return 0;
        grid[i][j] = 2;
        return dfs(i - 1, j) + dfs(i + 1, j) + dfs(i, j - 1) + dfs(i, j + 1);
    };
    for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[0].length; j++) {
            if (grid[i][j] === 1) {
                return dfs(i, j);
            }
        }
    }
};
// 解法II 思路：总周长 = 4 * 土地个数 - 2 * 接壤边的条数
const islandPerimeter = (grid) => {
    let land = border = 0;
    for (let i = 0; i < grid.length; i++) {
        for (let j = 0; j < grid[0].length; j++) {
            if (grid[i][j] === 1) {
                land++;
                // 根据遍历顺序只需考虑要向右向下两个方向
                if (i + 1 < grid.length && grid[i + 1][j]) border++;
                if (j + 1 < grid[0].length && grid[i][j + 1]) border++;
            }
        }
    }
    return land * 4 - border * 2;
};
```