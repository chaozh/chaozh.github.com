title: 面试题总结：子数组问题
tags:
  - 算法
  - 面试
id: 675
categories:
  - 面试总结
date: 2013-09-03 14:42:06
---

公用题干为一个包含正数及负数的数组k

### 1.找到连续的子数组和最大，最小，最接近m（比如0）

最大的dp方程为dp[i]=max{dp[i-1]+k[i], k[i]}，最小情况完全类似
<pre>def submax(k[]):
    smax=MIN, cmax=0
    for i in range(0,len(k)-1):
        cmax=max(cmax+k[i], k[i])
        smax=max(smax, cmax)
    return smax</pre>
最接近m的处理方法，首先考虑特殊的m比如0，此时处理办法：首先用数组sum[0]=0, sum[i+1]=a[0]+a[1]+...+a[i]即包含第一个元素的元素和（这些元素相减可以构成子数组和）进行排序，然后遍历该数组并找到绝对值最小的两个相邻元素。所以对于m也可以采取同样的办法，遍历寻找与m值差绝对值最小的两个相邻元素即可。时间复杂度为O(nlgn)，空间复杂度为O(n)
<pre>def subsum(k[], m):
    sums = []
    sums[0]=0
    for i in range(0, len(k)):
        sums[i+1] = sums[i]+k[i]
    sums.sort()
    smin=m, cmin=0
    for i in range(1,len(k)+1):
        cmin=abs(sums[i]-sums[i-1]-m)
        smin=min(smin, cmin)
    return smin</pre>

### 2.找到几个子数组和最大，最小，最接近m

几个子数组和最大的dp方程为dp[i][j]=max{dp[i][j-1]+k[j], max{dp[i-1][p](0 &lt;p&lt;j)}+k[j]}，即考察k[j]是包含在第i组里面，还是再单独成组。最小情况完全类似。

找到n个数的和最接近m，我们仅考虑小于m且这里不考虑存在负数的情况（因为需要遍历可能的和值），使用类似0-1背包的方法仅有的限制是背包重量不能超过m，时间复杂度为O(n^2*m)
<pre>def subsum(k[], m):
#dp[j][p]代表是否找到j个数使得和等于p
dp =[[0 for j in range(m+2)] for i in range(len(k)+1)]
for i in range(1, len(k)+1):
        for j in range(1, m):
               dp[i][j]=max(dp[i-1][j], dp[i-1][j-k[j]]+k[j])
return dp[len(k)][m+1]</pre>

### 3.分割为两个子数组和最接近，添加正负号使结果最接近0

假设2n的正整数数组和为SUM，则问题变为找到n个数和最接近SUM/2，接近SUM/2就可能大于或小于SUM/2，我们仅考虑小于SUM/2且这里不考虑存在负数的情况（因为需要遍历可能的和值），使用类似0-1背包的方法有两个限制：一个是数目等于n，另一个是背包重量不能超过SUM/2，时间复杂度为O(n^2*m)
<pre>def subsum(k[], m):
#dp[2n][n][m/2]
dp =[[[0 for p in range(m/2+2)] for j in range(len(k)/2+1)] for i in range(len(k)+1)]
for i in range(1, len(k)+1):
    for j in range(1, min(i, len(k)/2)+1):
        for p in range(m/2+1, k[j]+1, -1):
            dp[i][j][p] = max(dp[i-1][j][p], dp[i-1][j-1][p-k[i]]+k[i])
#进行回溯寻找对应元素
i=len(k),j=len(k)/2, p=m/2+1
for i in range(len(k), 0, -1):
    if(dp[i][j][p]==dp[i-1][j-1][p-k[j]]+k[j]):
        print k[i] 
        --j, p-=k[i]
return dp[len(k)][len(k)/2][m/2+1]

#简化算法，只能求出最接近的值
def subsum(k[], m):
#ok[j][p]代表是否找到j个数使得和等于p，初始化为false
ok = [[false for col in range(m/2+1)] for row in range(len(k)/2+1)]
ok[0][0] = true
for i in range(1, len(k)+1):
    for j in range(1, min(i,len(k)/2)+1):
        for p in range(m/2+1, k[j]+1, -1):
            if ok[j-1][i-k[i]]:
               ok[j][p]=true
#找到最接近的值
for i in range(m/2+1, -1, -1):
    if ok[len(k)/2)][i]:
       return i</pre>
如果存在负数，则找到最小的那个负数并将其绝对值加到每个元素上，然后重新求SUM/2

### 4.找到所有和等于m的两个数，三个数，n个数

对于两个数的情况可以使用排序扫描的方法，或是排序再二分，或是使用hash表查找

对于三个数的情况同样使用排序扫描的方法，扫描固定一个值，然后双扫描确定另外两个值，时间复杂度为O(n^2)

n个数的情况和第2问中的最接近m方法相同，必须使用0-1背包

### 5.连续的子数组和最大扩展到二维，n维，循环数组等情况

对于二维可以枚举另一维的子数组区域，时间复杂度为O(n^2)，而降维后只需O(m)即可得到答案，故而总时间复杂度为O(n^2*m)

数组首尾相连可以分为原问题和数组跨界两种情况，后者可以采用数组所有元素总值减去最小子数组和的方法来解决。

### 6\. 在两个数组中找到两个数和等于m，三个数组中找三个数，n个数组的情况

&nbsp;