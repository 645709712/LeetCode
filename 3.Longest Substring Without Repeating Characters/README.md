Given a string, find the length of the **longest substring** without repeating characters.

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

# 思路

```java
public static int lengthOfLongestSubstring(String s) {
    if (s == null || s.length() == 0)
        return 0;
    int start, index = 0;
    int max = 1;

    StringBuilder str = new StringBuilder("" + s.charAt(0));
    for (int i = 1; i < s.length(); i++) {
        if ((index = str.indexOf(new String(new char[]{s.charAt(i)}))) != -1) {
            //调整start的位置
            start = index + 1;
            str = new StringBuilder(str.substring(start, str.length())).append(s.charAt(i));
        } else {
            //继续添加char
            str.append(s.charAt(i));
            max = max > str.length() ? max : str.length();
        }
    }
    return max;
}
```

我的思路是比如说输入是abcabcbb，从左往右扫描，str初始化为a，扫描到b，str中没有b，str置为ab，同理str置为abc，扫描到a，str中有a，把a及其前面的部分截去之后再加上一个a，即str置为bca，如此往复直至全部扫描完成。时间复杂度为O(n)。参考代码如下：

先说明一种我的思路到优化算法，用一个hashmap记录当前字符以及它的index，所以当遇到duplicate字符的时候，我们可以直接通过map定位到index，而不需要indexOf函数，代码如下：

```Java
ublic int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        Map<Character, Integer> map = new HashMap<>(); // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                i = Math.max(map.get(s.charAt(j)), i);
            }
            ans = Math.max(ans, j - i + 1);
            map.put(s.charAt(j), j + 1);
        }
        return ans;
    }
```

还有一种很巧妙的方法，代码如下：

```Java
public int lengthOfLongestSubstring(String s) {
        int result = 0;
        int[] index = new int[128];
        for (int j=0, i=0; j<s.length(); j++){
            i = Math.max(i, index[s.charAt(j)]);
            result = Math.max(result, j-i+1);
            index[s.charAt(j)] = j+1;
        }
        return result;
    }
```

其实本质思路和用hashmap是一样的，只不过通过引入一个index[]，用char的值作为数组的下标值，数组下标值对应的数据值对应索引值，代码很简洁，值得借鉴。