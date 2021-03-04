### leetcode

以下题目皆来源leetcode。

**2.18**   在仅包含 0 和 1 的数组 A 中，一次 K 位翻转包括选择一个长度为 K 的（连续）子数组，同时将子数组中的每个 0 更改为 1，而每个 1 更改为 0。 返回所需的 K 位翻转的最小次数，以便数组没有值为 0 的元素。如果不可能，返回 -1。
解：

```go
//方法未通过，因为超时，但是方法还是正确的
func minKBitFlips(A []int, K int) int {
    if len(A)>30000 {
        fmt.Println("the length of the array called A is beyond the limit value.")
        return -1
    }
    for i:=0;i<len(A);i++ {
        a := A[i]
        if a>1 {
            fmt.Println("this array is improper.")
            return -1
        }
    }
    if K==1 {
        var count int 
        for _,b :=range A {
            if b==0 {count++}
        }
        min :=count
        return min
    }
    var num int
    var con int
    for i:=range A {
        if A[i]==0 && (i+K-1)<len(A) {
            b := A[i:i+K]
            for j:=range b {
                if b[j]==0 {
                    b[j]=1
                } else { b[j]=0 }
            }
            num++
        }
    }
    for _,examine := range A {
        if examine==1 { con++ }
    }
    if con==len(A) {
        return num
    } else {
        return -1
    }
}
```

**2.22** 今天，书店老板有一家店打算试营业 customers.length 分钟。每分钟都有一些顾客（customers[i]）会进入书店，所有这些顾客都会在那一分钟结束后离开。

在某些时候，书店老板会生气。 如果书店老板在第 i 分钟生气，那么 grumpy[i] = 1，否则 grumpy[i] = 0。 当书店老板生气时，那一分钟的顾客就会不满意，不生气则他们是满意的。

书店老板知道一个秘密技巧，能抑制自己的情绪，可以让自己连续 X 分钟不生气，但却只能使用一次。

请你返回这一天营业下来，最多有多少客户能够感到满意的数量。

解：

```go
//滑动窗口算法的应用，所以需要我后期对算法有一定的了解
func maxSatisfied(customers []int, grumpy []int, X int) int {
	length :=len(customers)
    var total,increase int
    for i:=range grumpy {
        switch grumpy[i] {
            case 0:
                total +=customers[i]
            case 1:
        }
    }
    for i:=0;i<X;i++ {
        increase +=customers[i]*grumpy[i]
    }
    maxIncrease :=increase 
    for i:=X;i<length;i++ {
        increase = increase-customers[i-X]*grumpy[i-X]+customers[i]*grumpy[i]
        maxIncrease =max(maxIncrease,increase)
    }
    return total+maxIncrease
}
 func max(a,b int) int {
        var sum int
        switch {
            case a<=b:  sum=b
            case a>b:   sum=a
        }
        return sum
}
```

**2.23**  给定一个二进制矩阵 A，我们想先水平翻转图像，然后反转图像并返回结果。

水平翻转图片就是将图片的每一行都进行翻转，即逆序。例如，水平翻转 [1, 1, 0] 的结果是 [0, 1, 1]。

反转图片的意思是图片中的 0 全部被 1 替换， 1 全部被 0 替换。例如，反转 [0, 1, 1] 的结果是 [1, 0, 0]。

解：

```go
//方法1（本人想出来的）
func flipAndInvertImage(A [][]int) [][]int {
  n :=len(A)
    for i :=range A {
        for j:=0;j<(n+1)/2;j++ {
            m:=A[i][j]
            A[i][j]=A[i][n-j-1]
            A[i][n-j-1]=m
        }
    }
    for i:=0;i<n;i++ {
        for j:=0;j<n;j++ {
            if A[i][j]==0 {
                A[i][j]=1
            } else {A[i][j]=0}
        }
    }
    return A
}
//方法2（大佬相出来的）
func flipAndInvertImage(A [][]int) [][]int {
    n:=len(A)
    half :=(len(A)+1)/2
    for i :=range A {
        for j:=0;j<half;j++ {
            A[i][j],A[i][n-j-1]=A[i][n-j-1]^1,A[i][j]^1
        }
    }
    return A
}
```

**2.24**给你一个二维整数数组 `matrix`， 返回 `matrix` 的 **转置矩阵** 。

矩阵的 **转置** 是指将矩阵的主对角线翻转，交换矩阵的行索引与列索引。

![img](https://assets.leetcode.com/uploads/2021/02/10/hint_transpose.png)

 解：

```go
 func transpose(matrix [][]int) [][]int {
    m :=len(matrix)
    n :=len(matrix[0])
    b :=make([][]int, n)
    if m==n {
        for i:=range matrix {
            for j:=n-1;j>i;j-- {
                matrix[i][j],matrix[j][i] = matrix[j][i],matrix[i][j]
            }
        } 
        return matrix
    } else {
        for j:=0;j<n;j++ {
            for i :=range matrix {
                b[j] =append(b[j],matrix[i][j])
            }
        }
        return b   
    }
}
方法2（官方的直接将行列互换就行了）
func transpose(matrix [][]int) [][]int {
    n, m := len(matrix), len(matrix[0])
    t := make([][]int, m)
    for i := range t {
        t[i] = make([]int, n)
        for j := range t[i] {
            t[i][j] = -1
        }
    }
    for i, row := range matrix {
        for j, v := range row {
            t[j][i] = v
        }
    }
    return t
}
```

2.25~3.2只有2.27没有做，其他的都做了但未写在这上面

