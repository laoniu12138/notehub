# LeetCode Num.349 两个数组的交集

## 难度：简单

描述：

```
给定两个数组，编写一个函数来计算它们的交集。

示例 1：
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]

示例 2：
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]

说明：
输出结果中的每个元素一定是唯一的。
我们可以不考虑输出结果的顺序。
```

方法一：第一个反应是直接遍历nums1，寻找nums1的每一个数字是否在nums2中出现，得出一个数组，然后去重。但这样做的话，时间复杂度是O(m*n)加上去重的时间。由于时间复杂度较高，而且需要后续操作，这里的代码就略过。

方法二：在一个思路的基础上，遍历nums1时利用哈希方法把去重提前，同时在哈希表中记录，这样只需要再遍历一次nums2就可以得出交集数组，时间复杂度是O(m+n)。

```go
func intersection(nums1 []int, nums2 []int) []int {
    samenum := []int{}
	nums1map := map[int]bool{}
	for _, num1 := range nums1 {
		nums1map[num1] = true
	}
	for _, num2 := range nums2 {
		if nums1map[num2] {
			nums1map[num2] = false
			samenum = append(samenum, num2)
		}
	}
	return samenum
}
```

方法三：先将两个数组进行排序，使得数据有序，然后分别用两个指针进行遍历，找到交集数组，

这样时间复杂度就是O(mlogm + nlogn + m + n) ，也就是O(mlogm + nlogn)。

```go
func intersection(nums1 []int, nums2 []int) []int {
    samenum := []int{}
	sort.Ints(nums1)
	sort.Ints(nums2)
	for i, j := 0, 0; i < len(nums1) && j < len(nums2); {
		x, y := nums1[i], nums2[j]
		if x == y {
			if len(samenum) == 0 || x > samenum[len(samenum)-1] {
				samenum = append(samenum, x)
			}
			i++
			j++
		} else if x < y {
			i++
		} else {
			j++
		}
	}

	return samenum
}
```



