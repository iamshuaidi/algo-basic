#### 题目描述

在一个由 0 和 1 组成的二维矩阵内，找到只包含 1 的最大正方形，并返回其面积。

**示例**

```
输入: 

1 0 1 0 0
1 0 1 1 1
1 1 1 1 1
1 0 0 1 0

输出: 4
```

#### 解法一：暴力法

在一个二维矩形中，如果我们要确定一个矩阵，我们只需要知道确定它的**左上角**和**右下角**就可以了，而正方形相当于边相等的矩阵。这道题暴力法还是比较好做，就是把矩阵中的每一个点，都充当**左上角**来遍历搜索一下。

例如我刚开始把（0，0）这个点当左上角，然后向右下角搜索

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOS8xMC8zMC8xNmUxZDJkNjUwZjM2NzVh?x-oss-process=image/format,png)

搜索的过程中，用一个变量来记录最大正方形的面积。接着用（0，1）作为左上角，不断着向右下角搜索

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOS8xMC8zMC8xNmUxZDJmYTFjYTdkYzRi?x-oss-process=image/format,png)

当然，（0，1）这个位置本身就是 0 ，所以是没有搜索的必要的，我这里只是做个演示。最终的代码如下，代码中也有详细的介绍

```
    public int maximalSquare(char[][] matrix) {
        // 如果矩阵长或宽少于1则直接返回0
        if(matrix.length < 1 || matrix[0].length < 1)
            return 0;
        
        int rows = matrix.length;
        int cols = matrix[0].length;
        // 记录最大边长
        int max = 0;
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                // 把（i，j）作为左上角向右下角搜索
                if (matrix[i][j] == '1') {
                    // 此时正方形的边长
                    int sqlen = 1;
                    boolean flag = true;//记录是否遇到0的位置
                    while (sqlen + i < rows && sqlen + j < cols && flag) {
                        for (int k = j; k <= sqlen + j; k++) {
                            if (matrix[i + sqlen][k] == '0') {
                                flag = false;
                                break;
                            }
                        }
                        for (int k = i; k <= sqlen + i; k++) {
                            if (matrix[k][j + sqlen] == '0') {
                                flag = false;
                                break;
                            }
                        }
                        if (flag)
                            sqlen++;
                    }
                    if (max < sqlen) {
                        max = sqlen;
                    }
                }
            }
        }
        return max * max;
    }
```

- 时间复杂度：O((mn)^2) 
- 空间复杂度：O(1)

#### 解法二：动态规划

对于动态规划，大部分情况下我们都会定义一个二维数组dp，然后定义dp[i][j] 的含义，接着推导 dp[i][j] 与 dp[i-1][j]、dp[i][j-1]、dp[i-1][j-1] 之间的关系。当然，也可以是推导 dp[i][j] 与 dp[i+1][j]、dp[i][j+1]、dp[i+1][j+1] 之间的关系，下面我们讲下用 dp 该怎么解这道题。

1、首先我们定义 dp[i][j] 含义为**正方形以 dp[i][j] 作为右下角时的最大边长值**。

2、接着我们来推导他们的关系

显然，对于任意一点 dp[i][j]，由于该点是正方形的右下角，所以该点的右边，下边，右下边都不用考虑，关心的是左边，上边，和左上边，也就是我们要推导 dp[i][j] 与 dp[i-1][j]、dp[i][j-1]、dp[i-1][j-1] 之间的关系。他们有如下关系

dp[i][j] = min( dp[i-1][j], dp[i-1][j-1], dp[i][j-1] ）+ 1

这个关系其实也不算难推，毕竟不能有 0 存在，所以只能取交他们三个点的交集。你们可以画个图，可能就比较好理解了。

代码如下：

```
    public int maximalSquare(char[][] matrix) {
        // 如果矩阵长或宽少于1则直接返回0
        if(matrix.length < 1 || matrix[0].length < 1)
            return 0;
        int rows = matrix.length;
        int cols = matrix[0].length;
        
        int[][] dp = new int[rows + 1][cols + 1];
        int max = 0;
        for (int i = 1; i <= rows; i++) {
            for (int j = 1; j <= cols; j++) {
                if (matrix[i-1][j-1] == '1'){
                    dp[i][j] = Math.min(Math.min(dp[i][j - 1], dp[i - 1][j]), dp[i - 1][j - 1]) + 1;
                    max = Math.max(max, dp[i][j]);
                }
            }
        }
        return max * max;
    }
```

- 时间复杂度：O(n*m) 
- 空间复杂度：O(n*m)

#### 解法三：动态规划优化

用动态规划时，可以说 80% 都是用二维数组，但是 80% 也都可以优化成一维数组，这很容易理解，大家看这个公式

dp[i][j] = min( dp[i-1][j], dp[i-1][j-1], dp[i][j-1] ）+ 1

通过上面的公式我们可以知道，我们要算 dp[i][j] 的值时，只需要用到 dp[i-1][j], dp[i][j-1], dp[i-1][j-1] 三个值就可以了。也就是说，我们在算矩阵 dp 第 i 行的值时，只需要用第 （i - 1） 行的值，至于（i-2）的值根本就不需要用到

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOS8xMC8zMC8xNmUxZDRjN2FkM2YzOGQ5?x-oss-process=image/format,png)

所以我们只需要用一个一维数组就可以了，然后每次算出第 i 行的值，就马上用一维数组 dp[0....n] 把这行值保存起来，供计算 i+1 行时使用。

如下图
![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOS8xMC8zMC8xNmUxZDU1Y2I0MjE5NGNj?x-oss-process=image/format,png)

new_dp[i] 相当于二维矩阵的 dp[i][j]

dp[i] 相当于 dp[i-1][j]

dp[i-1] 相当于 dp[i-1][j]

pre 相当于 dp[i-1][j-1]。

然后用一维矩阵的话，我们每次计算出 new_dp[i] 后，就马上用 new_dp[i] 覆盖 dp[i] 的值，并且还要用一个变量 pre 来保存dp[i-1][j-1]的值。

> 好吧，估计你也给我绕晕了，如果不大理解，强烈建议画图模拟一下

最终代码如下

```
    public int maximalSquare(char[][] matrix) {
        if(matrix.length < 1 || matrix[0].length < 1)
            return 0;

        int rows = matrix.length;
        int cols = matrix[0].length;
        int[] dp = new int[cols + 1];
        int max = 0, prev = 0;
        for (int i = 1; i <= rows; i++) {
            for (int j = 1; j <= cols; j++) {
                int temp = dp[j];
                if (matrix[i - 1][j - 1] == '1') {
                    dp[j] = Math.min(Math.min(dp[j - 1], prev), dp[j]) + 1;
                    max = Math.max(max, dp[j]);
                } else {
                    dp[j] = 0;
                }
                prev = temp;
            }
        }
        return max * max;
    }
```

- 时间复杂度：O(n*m) 
- 空间复杂度：O(n)

#### 额外话

动态规划是一个比较难的算法思想，特别是对于初学者，遇到动态规划的题基本凉，我刚开始也被搞过，后来能看懂关于动态规划的答案，但是自己写不出，一气之下做了几十道动态规划的题，发现做来做去套路都差不多，于是总结出了自己的一个套路模板，从此 80% 的动态规划题都会做。所以呢，后面找个时间我得写一写我的经验，这个经验适合**看得懂动态规划，但又不知道怎么下手的人**，不过写这篇文章估计需要挺长时间，所以几时写还没确定，，，，大家也可以学我，直接做 50 道动态规划的题，准稳。

学习更多**算法** + **计算机基础知识**，欢迎关注我的微信公众号，每天准时推送技术干货

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306223728524.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)



