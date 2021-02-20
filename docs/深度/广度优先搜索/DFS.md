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