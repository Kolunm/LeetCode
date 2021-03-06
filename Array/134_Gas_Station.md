# [（中等）134. 加油站](https://leetcode-cn.com/problems/gas-station/)
## **题目描述**
>在一条环路上有 N 个加油站，其中第 i 个加油站有汽油 gas[i] 升。

>你有一辆油箱容量无限的的汽车，从第 i 个加油站开往第 i+1 个加油站需要消耗汽油 cost[i] 升。你从其中的一个加油站出发，开始时油箱为空。

>如果你可以绕环路行驶一周，则返回出发时加油站的编号，否则返回 -1。
### **说明**
- 如果题目有解，该答案为唯一答案。
- 输入数组均为非空数组，且长度相同。
- 输入数组中的元素均为非负数

### **示例1**
**输入：**
>gas  = [1,2,3,4,5]

>cost = [3,4,5,1,2]

**输出：**
>3

**解释：**
>从 3 号加油站(索引为 3 处)出发，可获得 4 升汽油。此时油箱有 = 0 + 4 = 4 升汽油

>开往 4 号加油站，此时油箱有 4 - 1 + 5 = 8 升汽油

>开往 0 号加油站，此时油箱有 8 - 2 + 1 = 7 升汽油

>开往 1 号加油站，此时油箱有 7 - 3 + 2 = 6 升汽油

>开往 2 号加油站，此时油箱有 6 - 4 + 3 = 5 升汽油

>开往 3 号加油站，你需要消耗 5 升汽油，正好足够你返回到 3 号加油站。

>因此，3 可为起始索引。


## **解决方案**
### **方法一：暴力遍历**
>最容易想到的方法就是从头开始对每个可能的起始位置进行考察，判断从当前位置开始是否能环路一周。因此算法框架很清晰，是一个简单的遍历：
```
//Java
public int canCompleteCircuit(int[] gas, int[] cost) {
    for(int i=0;i<gas.length;i++){
        if(onceCompleteCircult(gas,cost,i)==true)
            return i;
    }
    return -1;
}
```
>其中onceCompleteCircult这个方法的作用就是验证从当前索引开始能否成功环路一次，其具体实现如下：
```
//Java
public boolean onceCompleteCircult(int[] gas, int[] cost, int index){
    int n = gas.length;
    int i = index;
    int tgas = 0;

    //为了对下面的循环条件做出让步，所以在循环体之前先进行一次运算，使得i>index
    tgas += gas[i];
    if(tgas-cost[i]>=0){
        tgas -= cost[i];
        i = (i+1)%n;
    }
    else
        return false;

    while(i!=index){    //当i再次回到索引位置时停止遍历，此时成功环路一周
        tgas += gas[i];
        if(tgas-cost[i]>=0){
            tgas -= cost[i];
            i = (i+1)%n;    //由于有环路存在，故i自增时要对数组长度n取模
        }
        else
            break;
    }
    if(i==index)
        return true;
    else 
        return false;
}
```
## **复杂度分析**
- 时间复杂度：*O(n<sup>2</sup>)*
- 空间复杂度：*O*(1)。
### **方法二：（改进）一次遍历**
>这里可参阅本题[官方解答](https://leetcode-cn.com/problems/gas-station/solution/jia-you-zhan-by-leetcode-solution/)其中使用数学方法证明了这样一个事实：**假设我们此前发现，从加油站 *x* 出发，每经过一个加油站就加一次油（包括起始加油站），最后一个可以到达的加油站是 *y*（不妨设 *x<y*），从 *x*,*y* 之间的任何一个加油站出发，都无法到达加油站 *y* 的下一个加油站。** 故我们的算法就是：首先检查第0个加油站，判断能否环绕一周；如果不能，就直接从第一个无法到达的加油站开始检查。
```
//Java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int n = gas.length;
        int i = 0;
        while (i < n) {
            int sumOfGas = 0, sumOfCost = 0;
            int cnt = 0;
            while (cnt < n) {
                int j = (i + cnt) % n;
                sumOfGas += gas[j];
                sumOfCost += cost[j];
                if (sumOfCost > sumOfGas) {
                    break;
                }
                cnt++;
            }
            if (cnt == n) {
                return i;
            } else {
                i = i + cnt + 1;
            }
        }
        return -1;
    }
}

```
## **复杂度分析**
- 时间复杂度：*O(N)*，*N* 为数组长度。
- 空间复杂度：*O*(1)。
