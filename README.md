## 把数组排成最小的数

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。 

* 先将整型数组转换成String数组，然后将String数组排序，最后将排好序的字符串数组拼接出来。关键就是制定排序规则。
* 排序规则如下：
* 若ab > ba 则 a > b，
* 若ab < ba 则 a < b，
* 若ab = ba 则 a = b；
* 解释说明：
* 比如 "3"<"31"但是"331">"313"，所以要将二者拼接起来进行比较

```java
import java.util.*;

public class Solution {
    public String PrintMinNumber(int [] numbers) {
        String[] strs = new String[numbers.length];
        for(int i=0;i<numbers.length;i++){
            strs[i] = ""+numbers[i];
        }
        Arrays.sort(strs,new Comparator<String>(){
            public int compare(String o1,String o2){
                String c1 = o1+o2;
                String c2 = o2+o1;
                return c1.compareTo(c2);
            }
        });
        StringBuffer sb = new StringBuffer();
        for(int i=0;i<strs.length;i++){
            sb.append(strs[i]);
        }
        return sb.toString();
    }
}
```

## 第一个只出现一次的字符

在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写） 

说一下解题思路哈，其实主要还是hash，利用每个字母的ASCII码作hash来作为数组的index。首先用一个58长度的数组来存储每个字母出现的次数，为什么是58呢，主要是由于A-Z对应的ASCII码为65-90，a-z对应的ASCII码值为97-122，而每个字母的index=int(word)-65，比如g=103-65=38，而数组中具体记录的内容是该字母出现的次数，最终遍历一遍字符串，找出第一个数组内容为1的字母就可以了，时间复杂度为O(n) 

```java
public class Solution {
    public int FirstNotRepeatingChar(String str) {
        int[] arr = new int[58];
        for(int i=0;i<str.length();i++){
            char c = str.charAt(i);
            arr[c-65]++;
        }
        for(int i=0;i<str.length();i++){
            char c= str.charAt(i);
            if(arr[c-65]==1){
                return i;
            }
        }
        return -1;
    }
}
```

## 和为s的连续正数序列

小明很喜欢数学,有一天他在做数学作业时,要求计算出9~16的和,他马上就写出了正确答案是100。但是他并不满足于此,他在想究竟有多少种连续的正数序列的和为100(至少包括两个数)。没多久,他就得到另一组连续正数和为100的序列:18,19,20,21,22。现在把问题交给你,你能不能也很快的找出所有和为S的连续正数序列? Good Luck! 

`//左神的思路，双指针问题`

`//当总和小于sum，大指针继续+`

`//否则小指针+`

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<ArrayList<Integer> > FindContinuousSequence(int sum) {
       ArrayList<ArrayList<Integer> > res  = new ArrayList();
       int l=1;
       int r = 2;
       while(r>l){
           int cur = (l+r)*(r-l+1)/2;
           if(cur==sum){
               ArrayList<Integer> lis = new ArrayList();
               for(int i=l;i<=r;i++){
                   lis.add(i);
               }
               res.add(lis);
               l++;
           }
           if(cur<sum){
               r++;
           }
           if(cur>sum){
               l++;
           }
       }
        return res;
    }
}
```

## 和为s的两个数

输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。 

开始还在纠结乘积最小，后来转念一想，a+b=sum,a和b越远乘积越小，而一头一尾两个指针往内靠近的方法找到的就是乘积最小的情况。 

```java
import java.util.ArrayList;
public class Solution {
    public ArrayList<Integer> FindNumbersWithSum(int [] array,int sum) {
        ArrayList<Integer> res = new ArrayList();
        int l = 0;
        int r = array.length-1;
        while(l<r){
            if(array[l]+array[r]==sum){
                res.add(array[l]);
                res.add(array[r]);
                break;
            }else if(array[l]+array[r]<sum){
                l++;
            }else{
                r--;
            }
        }
        return res;
    }
}
```

## Word Search 

Given a 2D board and a word, find if the word exists in the grid.

The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.

**Example:**

```
board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]

Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.
```

```java
class Solution {
    static int[][] fangxiang = new int[][]{{0,1},{0,-1},{1,0},{-1,0}};
    int[][] visited;
    public boolean exist(char[][] board, String word) {
        visited = new int[board.length][board[0].length];
        for(int i=0;i<board.length;i++){
            for(int j=0;j<board[0].length;j++){
                if(exist(board,word,0,i,j)){
                    return true;
                }
            }
        }
        return false;
        
    }
    
    public boolean fanweinei(int x,int y,char[][] board){
        if(x>=0&&x<board.length &&y>=0&&y<board[0].length){
            return true;
        }
        return false;
    }
    
    public boolean exist(char[][] board, String word, int index, int startx, int starty){
        if(index==word.length()-1){
            return word.charAt(index)==board[startx][starty];
        }
        if(board[startx][starty]==word.charAt(index)){
            visited[startx][starty] = 1;
            for(int i=0;i<4;i++){
            int newx = fangxiang[i][0]+startx;
            int newy = fangxiang[i][1]+starty;
            if(fanweinei(newx,newy,board)){
                if(visited[newx][newy] == 0){
                    if(exist(board,word,index+1,newx,newy)){
                    return true;
                }
            }    
        }
        }
            visited[startx][starty] = 0;
        }
        
        return false;
        
    }
}
```

## 二叉树和为某一值的路径

题目描述

输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

```java
import java.util.ArrayList;
/**
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;

    public TreeNode(int val) {
        this.val = val;

    }

}
*/
public class Solution {
    ArrayList<ArrayList<Integer>> res;
    public ArrayList<ArrayList<Integer>> FindPath(TreeNode root,int target) {
        res = new ArrayList();
        if(root==null){
            return res;
        }
        help(root,target,0,new ArrayList());
        return res;
    }
    
    public void help(TreeNode root,int target,int sum,ArrayList<Integer> list){
        sum+=root.val;
        list.add(root.val);
        boolean isLeaf = root.left==null&&root.right==null;
        if(sum==target&&isLeaf){
            res.add(new ArrayList(list));
        }
        //如果不是叶节点，则遍历子节点
        if(root.left!=null){
            help(root.left,target,sum,list);
        }
        if(root.right!=null){
            help(root.right,target,sum,list);
        }
        //返回父节点前，回溯
        sum-=root.val;
        list.remove(list.size()-1);
        
    }
}
```

## Number of Islands 

https://leetcode.com/problems/number-of-islands/description/

Given a 2d grid map of `'1'`s (land) and `'0'`s (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

```
Input:
11110
11010
11000
00000

Output: 1
```

**Example 2:**

```
Input:
11000
11000
00100
00011

Output: 3
```

```java
class Solution {
    int[][] visited;
    int[][] fangx = new int[][]{{0,1},{0,-1},{1,0},{-1,0}};
    public int numIslands(char[][] grid) {
        if(grid.length==0){
            return 0;
        }
        int count = 0;
        int m = grid.length;
        int n = grid[0].length;
        visited = new int[m+1][n+1];
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]=='1'&&visited[i][j]==0){
                    count++;
                    bfs(grid,i,j);
                }
            }
        }
        return count;
            
    }
    
    public boolean inArea(char[][] grid,int i,int j){
        int m = grid.length;
        int n = grid[0].length;
        return i>=0&&i<m&&j>=0&&j<n;
    }
    
    public void bfs(char[][] grid,int m, int n){
        visited[m][n] = 1;
        for(int i=0;i<4;i++){
            int newx = m+fangx[i][0];
            int newy = n+fangx[i][1];
            if(inArea(grid,newx,newy)&&visited[newx][newy]==0&&grid[newx][newy]=='1'){
                bfs(grid,newx,newy);
            }
        
        }
    }
}
```

