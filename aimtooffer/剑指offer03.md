## 题目：数组中重复的数字

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

示例 1：

```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```



从直观思路开始

解法一：先排序，再遍历一次对比前后两个数是否相等就可以了

```go
func findRepeatNumber(nums []int) int {
    sort.Ints(nums)
	for idx := range nums {
		if idx > 0 {
			if nums[idx] == nums[idx-1] {
				return nums[idx]
			}
		}
	}
	return 0
}
```

时间复杂度：O(nlogn)
空间复杂度：O(1)

![](E:\note\本地笔记\picture\jianzhioffer03-1.png)

由于排序一个长度为n的数组需要O(nlogn)的时间，所以时间复杂度为O(nlogn)



