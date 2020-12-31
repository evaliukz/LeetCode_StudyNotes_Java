# PriorityQueue专题

PriorityQueue 是基于优先堆的一个无界队列，这个优先队列中的元素可以默认natural order或者通过提供的Comparator 在队列实例化的时排序。
PriorityQueue 不允许空值，而且不支持 non-comparable（不可比较）的对象，比如用户自定义的类。优先队列要求使用 Java Comparable 和 Comparator 接口给对象排序，并且在排序时会按照优先级处理其中的元素。
PriorityQueue 的大小是不受限制的，但在创建时可以指定初始大小。当我们向优先队列增加元素的时候，队列大小会自动增加。
注意：
The Iterator provided in method iterator() is not guaranteed to traverse the elements of the priority queue in any particular order.
This implementation is not synchronized. 

### 几种常用的constructor:

public PriorityQueue()

public PriorityQueue(int initialCapacity)

public PriorityQueue(int initialCapacity, Comparator<? super E> comparator)

###几个methods: 

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


### 692. Top K Frequent Words  学习双重comparator写法

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


