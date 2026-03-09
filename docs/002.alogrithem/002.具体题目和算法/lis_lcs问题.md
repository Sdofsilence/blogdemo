# 最长增长子序列和最长公共子序列

* [最长上升子序列详解(LCS,LIS) ](https://www.cnblogs.com/wenyutao1/p/17839521.html)

## 注意的细节

* lis 可以实现nlogn 的复杂度，且增加一个辅助数组len来记录每个位置的元素为结尾的最长上升子序列的长度，就可以获取最长上升子序列。从后往前贪心。（每次找到len[i] = max_len的值,就是以max_len为最长子序列长度的结尾元素，让后max_len -= 1，继续往前找，直到max_len = 0）
* lcs 的时间复杂度为O(n*m) 使用动态规划
