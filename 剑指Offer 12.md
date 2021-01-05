# [（中等）剑指Offer 12. 矩阵中的路径](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)
## **题目描述**
>请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）

[["a","b","c","e"],

["s","f","c","s"],

["a","d","e","e"]]

>但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。


### **示例1**
**输入：**
>board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"

**输出：**
>true

### **示例2**
**输入：**
>board = [["a","b"],["c","d"]], word = "abcd"

**输出：**
>false

## **解决方案**
### **方法一：深度优先搜索（DFS）**
>本问题可使用递归的深度优先遍历解决，需要注意以下几点：

>- 访问过的元素board[i][j]需要进行标记代表已访问过，防止后面再重复访问，可将其修改为**空字符**''；
>- 搜索下一单元格：朝board[i][j]的上、下、左、右四个方向开启下次搜索，即下层递归，四个方向的递归使用**或**连接（表示只要有一个方向找到可行路径就可以直接返回）；
>- 还原当前矩阵元素：为了防止改变矩阵元素对后续回溯造成影响，在一次递归结束后要还原矩阵元素。

```
//Java
class Solution {
    public boolean exist(char[][] board, String word) {
        char[] words = word.toCharArray();
        for(int i = 0; i < board.length; i++) {
            for(int j = 0; j < board[0].length; j++) {
                //对board矩阵中的所有元素逐个遍历
                if(dfs(board, words, i, j, 0)) return true;
            }
        }
        return false;
    }
    boolean dfs(char[][] board, char[] word, int i, int j, int k) {
        //若i、j不再满足边界条件或下一个元素与word中下一个字符不匹配，直接终止本次搜索
        if(i >= board.length || i < 0 || j >= board[0].length || j < 0 || board[i][j] != word[k]) return false;
        if(k == word.length - 1) return true;
        //对访问过的元素进行标记
        board[i][j] = '\0';
        boolean res = dfs(board, word, i + 1, j, k + 1) || dfs(board, word, i - 1, j, k + 1) || 
                      dfs(board, word, i, j + 1, k + 1) || dfs(board, word, i , j - 1, k + 1);
        //将标记的元素还原
        board[i][j] = word[k];
        return res;
    }
}

```
## **复杂度分析**
- 时间复杂度：*O*(3<sup>*K*</sup>*MN*)。设字符串长度为 *K*，搜索中每个字符有上、下、左、右四个方向可选，除去回头方向，剩下三个选择，故方案数复杂度为*O*(3<sup>*K*</sup>)。而最坏情况下要以矩阵的所有元素分别做一次起点进行搜索，共进行*MN*次，故总时间复杂度为*O*(3<sup>*K*</sup>*MN*)。
- 空间复杂度：*O*(*MN*)。
