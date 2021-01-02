# PriorityQueue专题

PriorityQueue 是基于优先堆的一个无界队列，这个优先队列中的元素可以默认natural order或者通过提供的Comparator 在队列实例化的时排序。

PriorityQueue 不允许空值，而且不支持 non-comparable（不可比较）的对象，比如用户自定义的类。优先队列要求使用 Java Comparable 和 Comparator 接口给对象排序，并且在排序时会按照优先级处理其中的元素。

PriorityQueue 的大小是不受限制的，但在创建时可以指定初始大小。当我们向优先队列增加元素的时候，队列大小会自动增加。

#### 注意：
The Iterator provided in method iterator() is not guaranteed to traverse the elements of the priority queue in any particular order.
This implementation is not synchronized. 

#### 几种常用的constructor:

public PriorityQueue()

public PriorityQueue(int initialCapacity) 但是这个capacity在刷题时候并没什么用

public PriorityQueue(int initialCapacity, Comparator<? super E> comparator)

#### 几个methods: 

1.	boolean	add(E e)
Inserts the specified element into this priority queue.

2.	void	clear()
Removes all of the elements from this priority queue.

3.	boolean	contains(Object o)
Returns true if this queue contains the specified element.

4.	boolean	offer(E e)
Inserts the specified element into this priority queue.

5.	E	peek()
Retrieves, but does not remove, the head of this queue, or returns null if this queue is empty.

6.	E	poll()
Retrieves and removes the head of this queue, or returns null if this queue is empty.

7.	boolean	remove(Object o)
Removes a single instance of the specified element from this queue, if it is present.

8.	int	size()
Returns the number of elements in this collection.

9.	Object[]	toArray()
Returns an array containing all of the elements in this queue.

#### 215. Kth Largest Element in an Array 注意一下comparator，如果是留下大元素，把小的元素poll出去要 (a, b) -> a-b 但是这个是heap的default的所以不需要写comparator
```
class Solution {
    public int findKthLargest(int[] nums, int k) {
        
        // capacity = k  | smallest , k-1 elements |  45123   12
        
        PriorityQueue <Integer> heap = new PriorityQueue<Integer>();
        
        for (int i = 0; i< nums.length; i++) {
         
            heap.add(nums[i]);
            
            //if (i >= k) { 错误，不能这么写
            if (heap.size() > k){
                heap.poll();
            }
        }
        // return heap.peek() 也一样
        return heap.poll();
    }
}
```


#### 378. Kth Smallest Element in a Sorted Matrix 注意一下comparator，如果是留下小元素，把大的元素poll出去要 (a, b) -> b-a;

```
class Solution {
    public int kthSmallest(int[][] matrix, int k) {
        
        //把大的元素pop出去，留下小的元素
        Comparator <Integer> comp = (a, b) -> b-a;
        
        PriorityQueue <Integer> heap = new PriorityQueue<> (comp);
        
        for (int i = 0; i < matrix.length; i++){
            
            for (int j = 0; j < matrix[0].length; j++){
                
                heap.add(matrix[i][j]);
                
                if (heap.size() >k){
                    heap.poll();
                }
                
            }

        }
        
        return heap.poll();
        
    }
}
```



#### 692. Top K Frequent Words  学习双重comparator写法

```
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        
        List<String> result = new ArrayList<String> ();
        
        HashMap <String, Integer> map = new HashMap <String, Integer>();
        
        for (String word: words){
            map.put(word, map.getOrDefault(word, 0) +1);
        }
        
        //这个comparator要学会写：如果count相等？按照word字母排序从大到小pop : count大的在前
        Comparator<String> comp = (a , b) -> map.get(a).equals(map.get(b)) ? b.compareTo(a) : map.get(a).compareTo(map.get(b));
        // 也可以写成 map.get(a) - map.get(b) 会按照从小到大往外pop
        
        PriorityQueue<String> heap = new PriorityQueue<String> (comp);
        
        for (String s: map.keySet()){
            
            heap.add(s);
            
            if (heap.size() > k) {
                heap.poll();
            }
        }
        
        for (int i = 0; i < k; i++) {
            result.add(heap.poll());
        }
        
        Collections.reverse(result);
        
        return result;
    }
}
```

#### 973. K Closest Points to Origin 学习int[][]的Comparator的写法
```
class Solution {
    public int[][] kClosest(int[][] points, int K) {
        
        int len = points.length;
        //要保留更小的就是b-a 
        //一开始写了b[0]^2 + b[1]^2 - a[0]^2 - a[1]^2 但是不能用，所以以后要写成两个相乘
        Comparator<int[]> comp = (a, b) -> (b[0]*b[0] + b[1]*b[1] - a[0]*a[0] - a[1]*a[1]);
        
        PriorityQueue <int[]> heap = new PriorityQueue<int[]> (comp);
        
        for (int i = 0; i < len; i++) {

            heap.add(points[i]);
            
            if (heap.size()>K){
                int[] temp = heap.poll();
            }
        }
    
        int[][] result = new int[K][2];
    
        for (int i=0; i<K; i++){
            result[i] = heap.poll();
        }
    
    return result;
}
}
```

#### 767. Reorganize String 非常好的一道题，利用PQ来做Greedy，一直拿最大的，第二大的，第三大的交替...

```
class Solution {
    public String reorganizeString(String S) {
        
        char[] string = S.toCharArray();
        
// 第一步：先算数，一边扫一遍检查有没有可能是false
        HashMap <Character, Integer> map = new HashMap<Character, Integer>();
        
        for (char c: string){
            map.put(c, map.getOrDefault(c, 0) +1);
            //实验了一下：如果是偶数 就是 > string.length/2 
            //          偶数时候： string.length/2 = (string.length+1)/2
            //          如果是奇数 就是 string.length/2+1
            //          奇数时候： string.length/2+1 =  (string.length+1)/2
            
             if (map.get(c) > (string.length+1)/2){
                 return "";
             }
        }
        
// 第二步：最大堆 , 想要留下小的，poll出来大的
        Comparator <Character> comp = (a, b) -> map.get(b) - map.get(a);
        PriorityQueue <Character> heap = new PriorityQueue <Character>(comp);
        
        for (char c: map.keySet()){
            heap.add(c);
        }
        
//第三步：交替加上最大的，第二大的，在map里count--，然后再放回heap
        
        StringBuilder result = new StringBuilder();
        
        while (heap.size() > 1) {
            char max = heap.poll();
            result.append(max);
            
            char second = heap.poll();
            result.append(second);
            
            //必须要放在max和second的后面，去heap.add(), 否则max处理完之后，做heap.add()
            //second再poll的时候会再次拿到max
            if (map.get(max) -1 > 0){
                map.put(max, map.get(max)-1);
                heap.add(max);
            }
            
            if (map.get(second) - 1 > 0) {
                map.put(second, map.get(second)-1);
                heap.add(second);
             }
        }
//处理最后一个item如果有的话   
        // 一开始省略了 if heap.size == 1, 但后来发现不可以，如果只有2个元素a,b,那么到这里就没有了，
        // 继续执行会有null 错误
        if (heap. size () == 1) {
            char max = heap.poll();
            result.append(max);
        }
        return result.toString();
    }
}

```


