### 2407.Longest-Increasing-Subsequence-II

本题思考的切入点其实是DP。对于`nums[i]=x`，我们需要检查i之前的所有位置j，如果有`y=nums[j]`的值满足`[x-k,x-1]`之间，那么就有`dp[i]=max(dp[j]+1)`。显然这是一个N^2的解法，该如何改进呢？

我们可以发现，在索引上，j的位置是不确定的，但是在值域上y的范围是连续的。我们是否可以将求dp[j]转变为求dp[y]呢？

对于一个连续的区间的最大值，我们有两个想法：线段树求区间最值，或者双端队列求sliding window max。在本题里，随着i的变化，x不是单调的，所以[x-k,x-1]也不是单向移动的sliding window。所以这道题的解题工具就是线段树。

此时我们再观察所有元素的值域范围是0到1e5，这就意味着在内存里开辟这么大的线段树是完全可行的。对于线段树的叶子节点v，我们存储以v为结尾的、符合条件的subsequence有多少。注意，v是指数值，而不是index。

所以我们的算法就是反复做两步操作：
1. 对于当前`nums[i]=x`，我们在线段树里寻找[x-k,x-1]里最大值len
2. 于是我们可以对线段树的节点x更新为1+len。

最终答案就是整棵线段树里叶子节点的最大值。