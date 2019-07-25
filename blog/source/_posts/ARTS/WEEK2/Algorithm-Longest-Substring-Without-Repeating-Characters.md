---
title: Algorithm-Longest-Substring-Without-Repeating-Characters
date: 2019-07-20 23:23:12
tags: ARTS Algorithm
categories:
- ARTS
- WEEK2
---

# Algorithm

## LeetCode No 3. Longest Substring Without Repeating Characters

> Given a string, find the length of the longest substring without repeating characters.

**Example 1:**

```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3.
```

**Example 2:**

```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

**Example 3:**

```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

翻译一下：给定一个字符串，找出其中不含有重复字符的**最长子串**（要求连续）的长度。

---

### 思路

#### 1. Brute-force 暴力求解

获取该字符串的所有子串，然后对每一个子串，判断有没有重复字符

假设字符串总长度为N，所有的子串长度从1~N

从头开始遍历字符串，对于第i个字符（i从0开始），以它开头的子串有N-i个

对每一个子串，判断是否有重复字符，最坏情况需要遍历N次

所以该方法的时间复杂度为O(N^3)

具体实现如下：

```java
class Solution{
    public int lengthOfLongestSubstring(String s) {
        char[] chars = s.toCharArray();
        int maxLength = 0;
        // 1. 从头遍历字符串
        for (int i = 0; i < chars.length; i++) {
            //2. 找到第i个字符开头的所有子串（包括左不包括右）
            for (int j = chars.length; j >= i + 1; j--) {
                int subStringLength = findSubStringLength(chars, i, j);
                maxLength = Math.max(maxLength, subStringLength);
                // 剪枝：当子串长度与本身长度相等时，就不再继续往下找了
                if (subStringLength == j - i) {
                    break;
                }
            }
            // 剪枝：同理当某个字符开头的长度与本身长度相等时，就不再继续往下找了
            if (maxLength == chars.length - i) {
                break;
            }
        }
        return maxLength;
    }

    /**
    * 对从left到right的子串，找到从左到右的最长无重复字符个数
    */
    private int findSubStringLength(char[] chars, int left, int right) {
        Set<Character> set = new HashSet<>();
        int length = 0;
        for (int i = left; i < right; i++) {
            if (set.contains(chars[i])) {
                return length;
            } else {
                set.add(chars[i]);
                length++;
            }
        }
        return length;
    }
}
```

这种暴力求解的方法只是提供一个思路，如果你在LeetCode里提交的话，肯定会因为超时报异常。当然在面试里可以提一下，为了体现你的思考也是好的

#### 2. 滑动窗口

聪明的你一定想到肯定有简化的算法。官方给出的方法叫做**滑动窗口**，通俗地解释下：

给出两个指针left和right，left和right之间所谓**滑动窗口**，从第一个字符开始，遇到无重复的字符，向右扩展窗口，遇到重复的字符，缩小左边窗口，从头滑到尾，窗口的最大长度就是问题的解。

那么问题来了，如何判断窗口中是否有重复元素，以及重复元素到底在哪儿呢？

使用散列表（HashMap）是一个比较好的方案，key存储字符，value存储字符在字符串数组中的位置。

从头遍历字符串数组，假如没有重复，就往HashMap里put这个字符以及它的位置。

如果有重复字符怎么办？

获取到重复字符之前的位置了，我要不要把这个字符以及之前的元素都从Map里删去呢？如果不删去，那么后面再遇到之前有的重复的元素怎么办？

比如abcdba这个字符串，找到b时重复时要不要把abcd都从map里去掉呢？

**其实并不需要的。**这个问题一开始也困扰了我好久，但是仔细想想，有一个left指针没有用到啊！！！

我只需要把left指针往后就行了，指到之前那个重复字符的后一个位置，然后更新这个重复元素在map中的值。

这样的话还有一个问题，重复元素之前的字符没有从map里去掉，后面再遇到怎么办？虽然在map里有值，但是在当前窗口中并没有重复呢！！！

`划重点！这个时候，我们的left指针实际上是比从map里获取到的重复元素的index要大！！！`

所以这种情况，我们不需要动left的位置。只需要更新这个字符在map里的value就可以。这样就确保了滑动窗口里永远没有重复元素。

好嘞，shut up and show me the code!

```java
class Solution {
        public int lengthOfLongestSubstring(String s) {
            char[] chars = s.toCharArray();
            Map<Character, Integer> charIndexMap = new HashMap<>();
            int left = 0;
            int maxLength = 0;
            for (int right = 0; right < chars.length; right++) {
                char c = chars[right];
                if (!charIndexMap.containsKey(c)) {
                    charIndexMap.put(c, right);
                } else {
                    int indexOfRepeatChar = charIndexMap.get(c);
                    left = Math.max(left, indexOfRepeatChar + 1);
                    charIndexMap.put(c, right);
                }
                maxLength = Math.max(maxLength, right - left + 1);
            }
            return maxLength;
        }
}
```

代码如果看起来有困难的话，结合上面讲解一起食用更好哦！