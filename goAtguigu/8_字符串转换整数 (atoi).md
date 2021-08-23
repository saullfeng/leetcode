#### [8. 字符串转换整数 (atoi)](https://leetcode-cn.com/problems/string-to-integer-atoi/)

难度中等1169收藏分享切换为英文接收动态反馈

请你来实现一个 `myAtoi(string s)` 函数，使其能将字符串转换成一个 32 位有符号整数（类似 C/C++ 中的 `atoi` 函数）。

函数 `myAtoi(string s)` 的算法如下：

- 读入字符串并丢弃无用的前导空格
- 检查下一个字符（假设还未到字符末尾）为正还是负号，读取该字符（如果有）。 确定最终结果是负数还是正数。 如果两者都不存在，则假定结果为正。
- 读入下一个字符，直到到达下一个非数字字符或到达输入的结尾。字符串的其余部分将被忽略。
- 将前面步骤读入的这些数字转换为整数（即，"123" -> 123， "0032" -> 32）。如果没有读入数字，则整数为 `0` 。必要时更改符号（从步骤 2 开始）。
- 如果整数数超过 32 位有符号整数范围 `[−231, 231 − 1]` ，需要截断这个整数，使其保持在这个范围内。具体来说，小于 `−231` 的整数应该被固定为 `−231` ，大于 `231 − 1` 的整数应该被固定为 `231 − 1` 。
- 返回整数作为最终结果。

**注意：**

- 本题中的空白字符只包括空格字符 `' '` 。
- 除前导空格或数字后的其余字符串外，**请勿忽略** 任何其他字符。

 

**示例 1：**

```
输入：s = "42"
输出：42
解释：加粗的字符串为已经读入的字符，插入符号是当前读取的字符。
第 1 步："42"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："42"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："42"（读入 "42"）
           ^
解析得到整数 42 。
由于 "42" 在范围 [-231, 231 - 1] 内，最终结果为 42 。
```

**示例 2：**

```
输入：s = "   -42"
输出：-42
解释：
第 1 步："   -42"（读入前导空格，但忽视掉）
            ^
第 2 步："   -42"（读入 '-' 字符，所以结果应该是负数）
             ^
第 3 步："   -42"（读入 "42"）
               ^
解析得到整数 -42 。
由于 "-42" 在范围 [-231, 231 - 1] 内，最终结果为 -42 。
```

**示例 3：**

```
输入：s = "4193 with words"
输出：4193
解释：
第 1 步："4193 with words"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："4193 with words"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："4193 with words"（读入 "4193"；由于下一个字符不是一个数字，所以读入停止）
             ^
解析得到整数 4193 。
由于 "4193" 在范围 [-231, 231 - 1] 内，最终结果为 4193 。
```

**示例 4：**

```
输入：s = "words and 987"
输出：0
解释：
第 1 步："words and 987"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："words and 987"（当前没有读入字符，因为这里不存在 '-' 或者 '+'）
         ^
第 3 步："words and 987"（由于当前字符 'w' 不是一个数字，所以读入停止）
         ^
解析得到整数 0 ，因为没有读入任何数字。
由于 0 在范围 [-231, 231 - 1] 内，最终结果为 0 。
```

**示例 5：**

```
输入：s = "-91283472332"
输出：-2147483648
解释：
第 1 步："-91283472332"（当前没有读入字符，因为没有前导空格）
         ^
第 2 步："-91283472332"（读入 '-' 字符，所以结果应该是负数）
          ^
第 3 步："-91283472332"（读入 "91283472332"）
                     ^
解析得到整数 -91283472332 。
由于 -91283472332 小于范围 [-231, 231 - 1] 的下界，最终结果被截断为 -231 = -2147483648 。
```

### 方法一

这时候一个比较推荐的做法是先对要求进行提炼整理：

忽略前导空格
首字符只能是 正号/负号/数字，否则不合法（返回 0）
继续往后匹配字符，直到结尾或不为数字为止（匹配过程中如果出现溢出，根据正负直接返回 Integer.MAX_VALUE 或 Integer.MIN_VALUE）。
当整理出来具体的逻辑之后，记得再次检查是否有遗漏掉某些边界情况。

一个 32 位有符号整数 即int类型为临界值 可能溢出

```go
func myAtoi(s string) int {
	var (
		negative  bool // 负数
		i         int32
		signed    bool // 是否出现过'-'，'+'
		startZero bool // 是否开始是0,再出现'-'，'+'，return 0
	)
	for k, v := range s { // 去除前空格
		if v != ' ' {
			s = s[k:]
			break
		}
	}
	for _, v := range s {
		if v == '0' && !signed && i == 0 { // 0000-898
			startZero = true
			continue
		}
		if startZero && (v == '+' || v == '-') {
			break
		}
		if v == '+' && !signed { // +
			signed = true
			continue
		}
		if v == '-' && !signed && i == 0 { // -
			negative = true
			signed = true
			continue
		}
		if v < '0' || v > '9' {
			break
		}
		if negative { //负数
			if -i < (math.MinInt32+(v-'0'))/10 {
				i = math.MinInt32
				break
			}
		} else {
			if i > (math.MaxInt32-(v-'0'))/10 {
				i = math.MaxInt32
				break
			}
		}
		i = v - '0' + i*10
	}
	if negative {
		i = -i
	}
	return int(i)
}
```

![image-20210823113601201](C:\Users\solfeng\AppData\Roaming\Typora\typora-user-images\image-20210823113601201.png)

### 自动机

![image-20210823113823465](C:\Users\solfeng\AppData\Roaming\Typora\typora-user-images\image-20210823113823465.png)

![fig1](https://assets.leetcode-cn.com/solution-static/8/fig1.png)

![image-20210823113845636](C:\Users\solfeng\AppData\Roaming\Typora\typora-user-images\image-20210823113845636.png)

```go
func myAtoi(str string) int {
	automaton := initAutomaton()
	length := len(str)
	for i := 0; i < length; i++ {
		automaton.get(str[i])
	}
	return automaton.Sign * int(automaton.Ans)
}

type Automaton struct {
	Sign  int // 1;
	Ans   int64
	State string
	table map[string][]string //= new HashMap<String, String[]>() {{
}

func initAutomaton() Automaton {
	return Automaton{
		Sign:  1,
		Ans:   0,
		State: "start",
		table: map[string][]string{
			"start":     []string{"start", "signed", "in_number", "end"},
			"signed":    []string{"end", "end", "in_number", "end"},
			"in_number": []string{"end", "end", "in_number", "end"},
			"end":       []string{"end", "end", "end", "end"},
		},
	}

}

func (a *Automaton) get(c uint8) {
	a.State = a.table[a.State][getCol(c)]
	if "in_number" == a.State {
		a.Ans = a.Ans*10 + int64(c-'0')
		if a.Sign == 1 {
			a.Ans = min(a.Ans, math.MaxInt32)
		} else {
			a.Ans = min(a.Ans, -math.MinInt32)
		}
	} else if "signed" == a.State {
		if c == '+' {
			a.Sign = 1
		} else {
			a.Sign = -1
		}
	}
}
func min(a, b int64) int64 {
	if a >= b {
		return b
	}
	return a
}

func getCol(c uint8) int {
	if c == ' ' {
		return 0
	}
	if c == '+' || c == '-' {
		return 1
	}
	if c >= '0' && c <= '9' {
		return 2
	}
	return 3
}
```

![image-20210823113601201](C:\Users\solfeng\AppData\Roaming\Typora\typora-user-images\image-20210823113601201.png)
