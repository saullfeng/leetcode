#### [168. Excel表列名称](https://leetcode-cn.com/problems/excel-sheet-column-title/)

难度简单445收藏分享切换为英文接收动态反馈

给你一个整数 `columnNumber` ，返回它在 Excel 表中相对应的列名称。

例如：

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

 

**示例 1：**

```
输入：columnNumber = 1
输出："A"
```

**示例 2：**

```
输入：columnNumber = 28
输出："AB"
```

**示例 3：**

```
输入：columnNumber = 701
输出："ZY"
```

**示例 4：**

```
输入：columnNumber = 2147483647
输出："FXSHRXW"
```

 

### %运算

这是一道从 11 开始的的 2626 进制转换题。

对于一般性的进制转换题目，只需要不断地对 columnNumber进行 % 运算取得最后一位，然后对 columnNumber进行 / 运算，将已经取得的位数去掉，直到 columnNumber为 0 即可。

一般性的进制转换题目无须进行额外操作，是因为我们是在「每一位数值范围在 [0,x)的前提下进行「逢 x 进一」。

但本题需要我们将从 1开始，因此在执行「进制转换」操作前，我们需要先对 columnNumber执行减一操作，从而实现整体偏移



```java
class Solution {
    public String convertToTitle(int cn) {
    StringBuilder res =new StringBuilder();
        while(cn >0){
            cn--;
            //cn % 26剩余数为字母
            res.append((char)(cn % 26 +'A'));
            cn /=26;
        }
        //反转
        res.reverse();
        return res.toString();
    }
}
```

时间复杂度：O(log 26 columnNumber）。时间复杂度即为将 columnNumber转换成 26 进制的位数。

空间复杂度O(1)。返回值不计入空间复杂度。注意，在某些语言（比如 Java、C#、JavaScript）中字符串是不可变的，因此我们需要额外的 O(log 26 columnNumber）的空间来存储返回值的中间结果。但是我们忽略这一复杂度分析，因为这依赖于语言的细节。

