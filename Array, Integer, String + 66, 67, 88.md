# Array, Integer, String 的方法总结

一般这种题是easy的题型。

## Array常用方法总结：

#### Array和Collections相互转化方法：
- Arrays.asList() 
   List<String> stooges = Arrays.asList("Larry", "Moe", "Curly");
- String[] y = x.toArray() 假设x是一个String的Collection list
  
#### array之间的copy操作：
- int [] copyResult = Arrays.copyOfRange(int[] original, int from, int to);
  把original从from到to这段copy到一个新的array叫做copyResult去
  
- System.arraycopy(Object src, int start, Object dest, int m, int length)
  把src这个array，从start这个index开始整个array，copy到dest的从index m开始往前的 length 个位置。
  
  例题 88. Merge Sorted Array解法一
```
  class Solution {
  public void merge(int[] nums1, int m, int[] nums2, int n) {
    System.arraycopy(nums2, 0, nums1, m, n);
    Arrays.sort(nums1);
  }
}
不推荐 - 复杂度分析
时间复杂度 : O((n + m)\log(n + m))O((n+m)log(n+m))。
空间复杂度 : O(1)O(1)。
```
例题 88. Merge Sorted Array解法二， 要学习后面的那部分：如果加到最短数组的最后，后面的那些怎么挪到result里面去？
```
class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
     
        int [] nums1copy = Arrays.copyOfRange(nums1, 0, m);
        int p1 = 0, p2 = 0, index = 0;
        
        while (p1<m && p2<n) {
            
           if (nums1copy[p1] <= nums2[p2]) {
               nums1[index] = nums1copy[p1]; 
               p1++;
           }  else {
               nums1[index] = nums2[p2]; 
               p2++;
        }
            index ++;
    }
       
        //if (m>n) { 一开始写的用m和n判断，但是还是用p1和p2才能实现
        if (p1<m) { 
            System.arraycopy ( nums1copy, p1, nums1, p1+p2, m+n-p1-p2 ); //这里要用p1+p2 和 m+n-p1-p2！！！
        }  
        
        if (p2<n) {
            System.arraycopy ( nums2, p2, nums1, p1+p2, m+n-p1-p2); //这里要用p1+p2 和 m+n-p1-p2！！！
        } 
   }
}
```


### String
- StringBuilder的reverse()方法结合逆序遍历是处理字符串问题的利器：

### Integer
- 十进制转化为其他进制：

* 二进制：Integer.toBinaryString(int i);
* 八进制：Integer.toOctalString(int i);
* 十六进制：Integer.toHexString(int i);

- 其他进制转化为十进制：
* 二进制：Integer.valueOf("0101",2).toString;
* 八进制：Integer.valueOf("376",8).toString;
* 十六进制：Integer.valueOf("FFFF",16).toString;
* 使用Integer类中的parseInt()方法和valueOf()方法都可以将其他进制转化为10进制。不同的是parseInt()方法的返回值是int类型，而valueOf()返回值是Integer对象。


### 关于String和Integer的转化：

- String转化Integer:   
* Integer.valueOf(String s)
* Integer.parseInt(String s)
* 不同的是parseInt()方法的返回值是int类型，而valueOf()返回值是Integer对象。
                      
- Integer转化String:  
* Integer.toString(int i)

- String转化Long:   
* Long.valueOf(String s)


## 题型一：插入型问题

### 1. Integers：  Usually the addition operation is not allowed for such a case. Use Bit Manipulation Approach. Here is an example: Add Binary.

### 2. Strings： Use bit by bit computation. Note, sometimes it might not be feasible to come up a solution with the constant space for languages with immutable strings。

#### 例题：67. Add Binary
第一种方法，Not Accepted: 想法是可以把string转化成integer, 但有overflow
如果字符串超过 3333 位，不能转化为 Integer
如果字符串超过 6565 位，不能转化为 Long
如果字符串超过 500000001500000001 位，不能转化为 BigInteger

```
class Solution {
    public String addBinary(String a, String b) {
        
        //自己的想法：可以把string转化成integer, 但有overflow，这时候要问限制条件
        int aNumber = Integer.valueOf(a,2);
        int bNumber = Integer.valueOf(b,2);
        
        return Integer.toBinaryString(aNumber + bNumber);
    }
}
```
第二种解法：
```
class Solution {
    public String addBinary(String a, String b) {
        
        StringBuilder result = new StringBuilder ("");
        int extraBit = 0; //是否进一位 
        
        
        for (int i = a.length()-1, j = b.length()-1; i>=0 || j>=0; i--, j--){
            int sum = extraBit;
            // 获取字符串a对应的某一位的值a.charAt(i) 即0或1 
                if (i>=0) { //当i>=0时，取原值 ‘1’的char类型和‘0’的char类型刚好相差为1
                    sum += a.charAt(i) - '0'; 
                } else { //当i<0是 sum+=0（向前补0）
                    sum += 0;
                }
            
            // 获取字符串b对应的某一位的值b.charAt(i) 即0或1 
                if (i>=0) { //当i>=0时，取原值 ‘1’的char类型和‘0’的char类型刚好相差为1
                    sum += b.charAt(i) - '0'; 
                } else { //当i<0是 sum+=0（向前补0）
                    sum += 0;
                }
        //上面两段代码可以写成：
        //sum += (i >= 0 ? a.charAt(i) - '0' : 0);
        //sum += ( j >= 0 ? b.charAt(j) - '0' : 0);    
               
                result.append(sum % 2); //如果二者都为1  那么sum%2应该刚好为0 否则为1
                extraBit = sum / 2;   //如果二者都为1  那么ca 应该刚好为1 否则为0
        }
        
        // 判断最后一次计算是否有进位  有则在最前面加上1 否则原样输出
        result.append(extraBit == 1 ? extraBit : ""); 
        
        return result.reverse().toString();
    }
} 
```



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
