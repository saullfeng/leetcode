#### [171. Excel 表列序号](https://leetcode-cn.com/problems/excel-sheet-column-number/)

难度简单281收藏分享切换为英文接收动态反馈

给你一个字符串 `columnTitle` ，表示 Excel 表格中的列名称。返回该列名称对应的列序号。

 

例如，

```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
```

 

**示例 1:**

```
输入: columnTitle = "A"
输出: 1
```

**示例 2:**

```
输入: columnTitle = "AB"
输出: 28
```

**示例 3:**

```
输入: columnTitle = "ZY"
输出: 701
```

**示例 4:**

```
输入: columnTitle = "FXSHRXW"
输出: 2147483647
```

 

**提示：**

- `1 <= columnTitle.length <= 7`
- `columnTitle` 仅由大写英文组成
- `columnTitle` 在范围 `["A", "FXSHRXW"]` 内

### 进制

标签：字符串遍历，进制转换
初始化结果 ans = 0，遍历时将每个字母与 A 做减法，因为 A 表示 1，所以减法后需要每个数加 1，计算其代表的数值 num = 字母 - ‘A’ + 1
因为有 26 个字母，所以相当于 26 进制，每 26 个数则向前进一位
所以每遍历一位则ans = ans * 26 + num
以 ZY 为例，Z 的值为 26，Y 的值为 25，则结果为 26 * 26 + 25=701

如果题目是 10 进制转换，那么你会很容易想到如下转换过程：从高位向低位处理，起始让 ans 为 0，每次使用当前位数值更新 ansans，更新规则为 ans=ans∗10+val i

 

举个🌰，假设存在某个十进制数字，编码为 ABCDABCD（字母与数字的映射关系与本题相同），转换过程如下：

ansans = 0
ans = ans * 10 + 1ans=ans∗10+1 => A
ans = ans * 10 + 2ans=ans∗10+2 => B
ans = ans * 10 + 3ans=ans∗10+3 => C
ans = ans * 10 + 4ans=ans∗10+4 => D

同理，本题只是将 10 进制换成 26进制



```java
class Solution {
    public int titleToNumber(String s) {
        char[] cn = s.toCharArray();
        int n =cn.length;
        int ans =0;
        //cn[i]-'A'+1 等于cn[i]字母本身
        for(int i=0;i<n;i++){
            ans=ans*26+(cn[i]-'A'+1);
        }
        return ans;
        
    }
}
```

![image-20210806103512048](C:\Users\solfeng\AppData\Roaming\Typora\typora-user-images\image-20210806103512048.png)

时间复杂度：O(n)
空间复杂度：因为 toCharArray 会创建与 s 等长的数组，因此使用 charAt 代替 toCharArray 的话为 O(1)，否则为 O(n)



![img](https://pic.leetcode-cn.com/a5e8e39fa19491e3e1d82c6aba3dec24e080c368d0400bf57012548b0fdb2af4-frame_00002.png)

![img](https://pic.leetcode-cn.com/da62003ebc140532fe1e42ff2c46d5c920101d6de50fd3c6910eee1e9d9c7df5-frame_00003.png)