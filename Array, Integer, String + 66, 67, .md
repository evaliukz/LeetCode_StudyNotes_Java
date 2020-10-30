# Array, Integer, String 的方法总结

一般这种题是easy的题型。

## 各种常用方法总结：

### StringBuilder: 

- String转化Integer:  Integer.valueOf(String s)
                      Integer.parseInt(String s)
- Integer转化String:  Integer.toString(int i)



- String转化Integer:  Integer.valueOf(String s)
- String转化Long:   Long.valueOf(String s)



## 题型一：插入型问题

### 1. Integers：  Usually the addition operation is not allowed for such a case. Use Bit Manipulation Approach. Here is an example: Add Binary.

### 2. Strings： Use bit by bit computation. Note, sometimes it might not be feasible to come up a solution with the constant space for languages with immutable strings, e.g. for Java and Python. Here is an example: Add Binary.

### 3. Linked Lists： Sentinel Head + Schoolbook Addition with Carry. Here is an example: Plus One Linked List.

### 4. Arrays： Schoolbook addition with carry.

#### 例题：66. Plus One
情况1:把最后一位加1； 情况2: 如果某一位是9，那么前面一位要+1，这一位变成0； 情况3: 如果前面一位不存在，那么建立一个新的array 是原来array长度+1，把第1位变成1，后面自然是0.

```
class Solution {
    public int[] plusOne(int[] digits) {
        
        int n = digits.length -1;
         //第1种情况:
        digits[n] +=1;
        
        while (digits [n] == 10){
           //第2种情况：
           if (n-1 >= 0) { 
                digits[n-1] +=1;
                digits [n] = 0;
                n--;
           //第3种情况:
           } else {
                int[] extendeddigits = new int [digits.length +1];
                extendeddigits[0] = 1;
                return extendeddigits;
           }
        }
        return digits;
    }
}
```
