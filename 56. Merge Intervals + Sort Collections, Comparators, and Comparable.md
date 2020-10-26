# LeetCode_StudyNotes_Java
This repository keeps my study notes from practicing LeetCode questions. 

##Collection Framework的简介

请参考：For more information about Java Collections interface, please visit: https://www.javatpoint.com/collections-in-java#:~:text=Java%20Collection%20means%20a%20single,HashSet%2C%20LinkedHashSet%2C%20TreeSet).

那么这里要讨论的是，如何sort一个collection object？

##第一种Interface：

public void sort(List list): is used to sort the elements of List. List elements must be of the Comparable type.

1. Collection.sort () 可以sort一个ArrayList里面的elements
```
ArrayList<String> al=new ArrayList<String>();  
al.add("Viru");  
al.add("Saurav");  
al.add("Mukesh");  
al.add("Tahir");  
  
Collections.sort(al);  
Iterator itr=al.iterator();  
while(itr.hasNext()){  
System.out.println(itr.next());  
 }  
```
2. 如果反向sort： Collections.sort(al,Collections.reverseOrder());

3. 可以sort一个自定义class如果这个class只有一个值传入
```
import java.util.*;  
  
class Student implements Comparable<Student> {  
    public String name;  
  public Student(String name) {  
    this.name = name;  
  }  
  public int compareTo(Student person) {  
    return name.compareTo(person.name);  
      
  }   
}  
public class TestSort4 {  
  public static void main(String[] args) {  
      ArrayList<Student> al=new ArrayList<Student>();  
      al.add(new Student("Viru"));  
      al.add(new Student("Saurav"));  
      al.add(new Student("Mukesh"));  
      al.add(new Student("Tahir"));  
      
    Collections.sort(al);  
    for (Student s : al) {  
      System.out.println(s.name);  
    }  
  }  
}  
```
但是这种Collections.sort () 是有局限性的，它只能sort一个element. 

##第二种方法: Comparable
在属性里面implements Comparable Interface 和定义 compareTo(Object obj) method

public int compareTo(Object obj): It is used to compare the current object with the specified object. It returns
- positive integer, if the current object is greater than the specified object.
- negative integer, if the current object is less than the specified object.
- zero, if the current object is equal to the specified object.

```
class Student implements Comparable<Student>{  
int rollno;  
String name;  
int age;  
Student(int rollno,String name,int age){  
this.rollno=rollno;  
this.name=name;  
this.age=age;  
}  
```
```
public int compareTo(Student st){  
if(age==st.age)  
return 0;  
else if(age>st.age)  
return 1;  
else  
return -1;  
}  
}  
```
```
import java.util.*;  
public class TestSort1{  
public static void main(String args[]){  
ArrayList<Student> al=new ArrayList<Student>();  
al.add(new Student(101,"Vijay",23));  
al.add(new Student(106,"Ajay",27));  
al.add(new Student(105,"Jai",21));  
  
Collections.sort(al);  
for(Student st:al){  
System.out.println(st.rollno+" "+st.name+" "+st.age);  
}  
}  
}  
```
如果需要reverse order, 则需要改写public int compareTo(Student st)里面的返回值：
```
 public int compareTo(Student st){    
 if(age==st.age)    
 return 0;    
 else if(age<st.age)    
 return 1;    
 else    
 return -1;    
 }    
 }    
 ```
 ##第三种方法：用Comparator Interface
 因为Comparator是个interface，不能直接new一个就好，而是每次使用要定义成为一个class。
 此时，原来的class就不需要使用上一种方法来implements Comparable了，这个Comparator直接写在应用的code那个class里。
 
 应用举例： 56. Merge Intervals
```
class Solution {
    public int[][] merge(int[][] intervals) {
        //这里的错误，比较的element是int[]
        Comparator<int[]> sortarray = new Comparator<int[]>() { 
            public int compare(int[] first, int[] second){
                if (first[0] < second[0]) {
                    return -1; //注意这里return的正负数，return -1的是最后想要的位置
                } 
                else if (first[0] == second[0]) {
                    return 0;
                } else {
                    return 1;
                }
            }
        }; //这里要加;
        
        Collections.sort(Arrays.asList(intervals), sortarray);
        
        LinkedList <int[]> merged = new LinkedList<int[]>();
        
        for (int[] entry: intervals){
            
            if (merged.isEmpty()|| merged.getLast()[1] < entry[0]) {
                merged.add(entry);
            } else {
                merged.getLast()[1] = Math.max(merged.getLast()[1],entry[1]);
            }
        }
       
        return merged.toArray(new int[merged.size()][]);
    }
}
``` 
 
