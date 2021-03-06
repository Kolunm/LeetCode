# [（中等）274. H指数](https://leetcode-cn.com/problems/h-index/)
## **题目描述**
>给定一位研究者论文被引用次数的数组（被引用次数是非负整数）。编写一个方法，计算出研究者的 h 指数。

>h 指数的定义：h 代表“高引用次数”（high citations），一名科研人员的 h 指数是指他（她）的 （N 篇论文中）总共有 h 篇论文分别被引用了至少 h 次。且其余的 N - h 篇论文每篇被引用次数 不超过 h 次。

>例如：某人的 h 指数是 20，这表示他已发表的论文中，每篇被引用了至少 20 次的论文总共有 20 篇。

### **示例1**
**输入：**
>citations = [3,0,6,1,5]

**输出：**
>3

**解释：**
>给定数组表示研究者总共有 5 篇论文，每篇论文相应的被引用了 3, 0, 6, 1, 5 次。
     由于研究者有 3 篇论文每篇 至少 被引用了 3 次，其余两篇论文每篇被引用 不多于 3 次，所以她的 h 指数是 3。

## **思路：**
>第一时间想到的就是将数组排序，而后直接遍历就好了，但是对于这种题直接调用排序下意识觉得是一种很low的做法，可以时间也没啥别的好的想法，就直接用排序写了，没想到官方解答也是直接用的排序。。。

## **解决方案**
### **方法一：排序**
>将数组降序排序后，如果citations[*i*]>*i*，说明第0到第*i*篇文章都至少被引用了*i*+1次，所以只要遍历数组，找到满足citations[*i*]>*i*的最大的*i*，则*h*=*i*+1。
```
//Java
class Solution {
  public int hIndex(int[] citations) {
    Arrays.sort(citations);
    int n = citations.length;
    for(int i=0;i<n;i++) {
      if (citations[i] >= n - i) 
            return n - i;
    }
    return 0;
  }
}

```
## **复杂度分析**
- 时间复杂度：*O*(*n*log*n*)
- 空间复杂度：*O*(1)。
### **方法二：计数**
>这种方法没太看懂，详见[官方解法](https://leetcode-cn.com/problems/h-index/solution/hzhi-shu-by-leetcode/)。

### **方法三：二分法**
>这种方法实际上即为第[275题（H指数II）](https://leetcode-cn.com/problems/h-index-ii/)的解法，这道题为本题的进阶版，给定已经升序排列好的数组，要求优化算法到***对数***时间复杂度。原本的线性遍历法时间复杂度为*O*(*n*)，那么我们采用二分法便可达到题目要求，具体实现如下。
```
//Java
class Solution {
  public int hIndex(int[] citations) {
    int low = 0, high = citations.length-1;
    int n = citations.length;
    while(low<=high){
        int mid = (high-low)/2 + low;
        if(citations[mid]==n-mid)   
            return n-mid;
        else if(citations[mid]<n-mid)
            low = mid+1;
        else
            high = mid-1;
    }
    return n - low;
  }
}
```
## **复杂度分析**
- 时间复杂度：*O*(log*N*)
- 空间复杂度：*O*(1)
