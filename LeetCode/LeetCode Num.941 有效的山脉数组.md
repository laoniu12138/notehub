# LeetCode Num.941 有效的山脉数组

## 难度：简单

描述：

```
给定一个整数数组 A，如果它是有效的山脉数组就返回 true，否则返回 false。

让我们回顾一下，如果 A 满足下述条件，那么它是一个山脉数组：

A.length >= 3
在 0 < i < A.length - 1 条件下，存在 i 使得：
	A[0] < A[1] < ... A[i-1] < A[i]
	A[i] > A[i+1] > ... > A[A.length - 1]
	
示例 1：
输入：[2,1]
输出：false

示例 2：
输入：[3,5,5]
输出：false

示例 3：
输入：[0,3,2,1]
输出：true

提示：
0 <= A.length <= 10000
0 <= A[i] <= 10000 
```

方法一：因为必为递增或者递减，所以只需要在走完后验证是否是尽头就可以了

```go
func validMountainArray(A []int) bool {
    i := 0
	for ; i+1 < len(A) && A[i] < A[i+1]; i++ {
	}
	if i == 0 || i == len(A)-1 {
		return false
	}
	for ; i+1 < len(A) && A[i] > A[i+1]; i++ {
	}
	return i == len(A)-1
}
```

方法二：双指针法，一个从前面爬另一个从后边爬，都爬到顶进行验证即可

```go
func validMountainArray(A []int) bool {
    if len(A) < 3 {
		return false
	}
	i, j := 0, len(A)-1
	for i < j {
		if A[i] < A[i+1] {
			i++ // 这个地方如果换成 i+=1 会速度快一些，不知道为什么
		} else if A[j] < A[j-1] {
			j-- // 这个地方如果换成 j-=1 会速度快一些，不知道为什么
		} else {
			return false
		}
	}
	if i == 0 || j == len(A)-1 {
		return false
	}
	return true
}
```

