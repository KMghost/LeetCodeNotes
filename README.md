### 1、两数和

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并**返回它们的数组下标**。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

---

**解题思路：**

1.	初始化一个与 nums 等长的map类型
2.	遍历 nums 使用 par 接收差值
3.	判断 par 能不能在 map 中找到对应答案，并且所找到的值是不是当前的数
4.	没找到把当前比那里的数值存入 map 中

```go
func twoSum(nums []int, target int) []int {
    num2index := make(map[int]int,len(nums))
    for i,num :=range nums{
        par := target - num
      	// 判断是不是同一个位置的数，在 num2index 中有没有找到对应的 par
        if j,ok := num2index[par]; i!=j && ok{
            return []int{i,j}
        }
        num2index[num] = i
    } 
    return nil
}
```



### 2、两数相加

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

---

**解题思路:**

1. 创建一个新的链表指针 dummy 保存链表头部，因为 循环中 curr会自我覆盖
2. curr 等于 链表指针 dummy  ，使用 curr 链表记录每次处理的结果
3. carry 是记录进位的数值，carry % 10 是当前汇总的数值，(int)carry / 10 是进位后需要加上的数值。

```go
/**
 * Definition for singly-linked list.
 * type ListNode struct {
 *     Val int
 *     Next *ListNode
 * }
 */
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    dummy := new(ListNode)
    curr := dummy
    carry := 0

    for (l1 != nil || l2 != nil || carry > 0) {
        curr.Next = new(ListNode)
        curr = curr.Next
        if l1 != nil {
            carry += l1.Val
            l1 = l1.Next
        }
        if l2 != nil {
            carry += l2.Val
            l2 = l2.Next
        }
        curr.Val = carry % 10
        carry /= 10
    }
	  // 因为第一个链表是空，为了简化代码
    return dummy.Next
}
```



### 3、无重复字符的最长子串

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**

```
输入: s = "abcabcdbb"
输出: 4
解释: 因为无重复字符的最长子串是 "abcd"，所以其长度为 4。
```

---

**解题思路：**

1. 一个指针记录无重复字符串开始位置，
2. 一个指针记录无重复字符串结束位置，
3. 创建一个 map 记录已经出现的字符与位置
4. 如果重复遇到map中记录的相同字符时 判断 该字符历史位置的下一位 是否比 当前指针记录的开始位置 要大，如果大则更新指针记录的位置
5. 结束位置减去开始位置，计算出无重复字符的长度，并且比较计算出最长数值。

```go
func lengthOfLongestSubstring(s string) int {
    start := 0
    max := 0
    stringMap := make(map[string]int)

    for end := 0; end < len(s); end++ {
        // 获取下标字符
        ch := (string)(s[end])
        if _, ok := stringMap[ch]; ok {
            start = getMax(start, stringMap[ch]+1)
        }
        max = getMax(max, end-start+1)
        stringMap[ch] = end
    }
    return max
}
func getMax(a int, b int) int {
    if a > b {
        return a
    }
    return b
}
```


