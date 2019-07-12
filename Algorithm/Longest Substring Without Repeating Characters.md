# Leetcode 3  无重复字符的最长子串
## 题目描述：给定一个字符串，找出其中不含有重复字符的最长子串的长度
方法与思路：开一个boolean类型的数组，开始均为false，然后设置两个指针left和right，开始都指向数组第一个元素。
开始扫描，扫描过的，boolean数组变为true。首先，right指针往后走，当走到重复出现的字母时，记录max值，max的值应为max={max, right-left}, 
然后开始挪动left指针，当left指针与right指针所指的值相同时停止。最后将left，right同时加1，然后进行下一次的上述操作。
代码实现：
```java
public int lengthOfLongestSubString(String s){
    if(s == null || s.length() == 0) return 0;
    int left = 0, right = 0;
    int n = s.length();
    //只有字母，ASCII值就需要128个
    boolean[] used = new boolean[128];
    int max = 0;
    while(right < n){
        if(used[s.charAt(right)] == false){
            used[s.charAt(right)] = true;
            right++;
        }else{
            max = Math.max(max, right - left);
            while(left < right && s.charAt(left) != s.charAt(right)){
                used[s.charAt(left)] = false;
                left++;
            }
            left++;
            right++;
        }
    }
    max = Math.max(max, right - left);
    return max;
}
```
