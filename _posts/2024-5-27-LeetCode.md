---
layout: post
title: "找出缺失的观测数据"
date: 2024-5-27
tags: [leetcode]
comments: true
author: zxy
---

**题目：**

现有一份 `n + m` 次投掷单个 **六面** 骰子的观测数据，骰子的每个面从 `1` 到 `6` 编号。观测数据中缺失了 `n` 份，你手上只拿到剩余 `m` 次投掷的数据。幸好你有之前计算过的这 `n + m` 次投掷数据的 **平均值** 。给你一个长度为 `m` 的整数数组 `rolls` ，其中 `rolls[i]` 是第 `i` 次观测的值。同时给你两个整数 `mean` 和 `n` 。

返回一个长度为 `n` 的数组，包含所有缺失的观测数据，且满足这 `n + m` 次投掷的 **平均值** 是 `mean` 。如果存在多组符合要求的答案，只需要返回其中任意一组即可。如果不存在答案，返回一个空数组。`k` 个数字的 **平均值** 为这些数字求和后再除以 `k` 。注意 `mean` 是一个整数，所以 `n + m` 次投掷的总和需要被 `n + m` 整除。

**示例 1：**

```
输入：rolls = [3,2,4,3], mean = 4, n = 2
输出：[6,6]
解释：所有 n + m 次投掷的平均值是 (3 + 2 + 4 + 3 + 6 + 6) / 6 = 4 。
```

**示例 2：**

```
输入：rolls = [1,2,3,4], mean = 6, n = 4
输出：[]
解释：无论丢失的 4 次数据是什么，平均值都不可能是 6 。
```

**提示：**

- `m == rolls.length`
- `1 <= n, m <= 105`
- `1 <= rolls[i], mean <= 6`

**思路：**

```
根据题目描述，数组 rolls 的长度为 m，记录了 m 个观测数据，还有 n 个观测数据缺失，共有 n + m 个观测数据。由于所有观测数据的平均值为 mean，因此所有观测数据之和为 mean × (n + m)。

根据所有观测数据之和与数组 rolls 中的 m 个观测数据，可知缺失的 n 个观测数据之和。将缺失的 n 个观测数据之和记为 missingSum。

由于每次观测数据的范围是 1 到 6，因此如果存在符合要求的答案，则一定有 n ≤ missingSum ≤ 6 × n。如果 missingSumm 不在上述范围内，则不存在符合要求的答案，返回空数组。

当 missingSum 满足 n ≤ missingSum ≤ 6 × n 时，一定存在一种符合要求的答案，由 n 个在 [1,6] 范围内的整数组成且这 n 个整数之和为 missingSum。记 quotient = missingSum / n，remainder = missingSum mod n，则可以构造一种符合要求的答案：在缺失的 n 个观测数据中，有 remainder 个观测数据是 quotient + 1，其余观测数据都是 quotient。
```

**代码：**

```cpp
class Solution {
public:
    vector<int> missingRolls(vector<int>& rolls, int mean, int n) {
        int m = rolls.size();
        int sum = mean * (n + m);
        int missingSum = sum;
        for (int & roll : rolls) {
            missingSum -= roll;
        }
        if (missingSum < n || missingSum > 6 * n) {
            return {};
        }
        int quotient = missingSum / n, remainder = missingSum % n;
        vector<int> missing(n);
        for (int i = 0; i < n; i++) {
            missing[i] = quotient + (i < remainder ? 1 : 0);
        }
        return missing;
    }
};
```

