# LRU算法：最近最少使用

核心： HashMap+双向链表     
实现： [可以使用`LinkedHashMap`简单的实现](http://www.importnew.com/25103.html)   

```
public class LRUCache extends LinkedHashMap
{
    public LRUCache(int maxSize)
    {
        super(maxSize, 0.75F, true); // 最后一个参数为true
        maxElements = maxSize;
    }

    // 当超过 maxElements元素时，就将元素删除 
    protected boolean removeEldestEntry(java.util.Map.Entry eldest)
    {
        return size() > maxElements;
    }
 
    private static final long serialVersionUID = 1L;
    protected int maxElements;
}
```

1. LinkedList首先它是一个Map，Map是基于K-V的，和缓存一致；   
2. LinkedList提供了一个boolean值可以让用户指定是否实现LRU

```
// accessOrder: 
// false，所有的Entry按照插入的顺序排列
// true，所有的Entry按照访问的顺序排列: LRU就是使用了该特性
public LinkedHashMap(int initialCapacity, float loadFactor,
                     boolean accessOrder) {
    super(initialCapacity, loadFactor);
    this.accessOrder = accessOrder;
}
```


