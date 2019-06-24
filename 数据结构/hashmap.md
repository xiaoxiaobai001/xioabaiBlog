### 原理
HashMap使用哈希表来存储，采用了链地址法解决冲突问题。链地址法，就是数组加链表的结合。在每个数组元素上都一个链表结构，当数据被Hash后，得到数组下标，把数据放在对应下标元素的链表上。
当我们往 HashMap 中 put 元素的时候，先根据 key 的 hashCode 重新计算 hash 值，根据 hash 值再通过高位运算和取模运算得到这个元素在数组中的位置（即下标），如果数组该位置上已经存放有其他元素了，那么在这个位置上的元素将以链表的形式存放，新加入的放在链头，最先加入的放在链尾，如果该链表超出8个的话，就转换成红黑树。如果数组该位置上没有元素，就直接将该元素放到此数组中的该位置上。

### 类继承关系
<pre>
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable 
</pre>

### 存储
HashMap使用内部类类存储数据：`Entry<K, V>`。这个entry是一个简单的简直对，有两个额外属性。一个是指向其他entry的引用，另一个是代表关键字的hash值，存储hash值是为了防止重复计算。
<pre>
static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;

        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }

        public final K getKey()        { return key; }
        public final V getValue()      { return value; }
        public final String toString() { return key + "=" + value; }

        public final int hashCode() {
			//hash值为键与值的hash值（分别调用各自的hashcode方法）异或得到，为了混合两者的信息？
            return Objects.hashCode(key) ^ Objects.hashCode(value);
        }

        public final V setValue(V newValue) {
            V oldValue = value;
            value = newValue;
            return oldValue;
        }

        public final boolean equals(Object o) {
            if (o == this)
                return true;
			//键值都相同才相同，底层调用的是比较对象的equal方法。
            if (o instanceof Map.Entry) {
                Map.Entry<?,?> e = (Map.Entry<?,?>)o;
                if (Objects.equals(key, e.getKey()) &&
                    Objects.equals(value, e.getValue()))
                    return true;
            }
            return false;
        }
    }
</pre>


`HashMap`将数据存储到多个单链表中（也称为`buckets`或`bins`）。默认容量是16。所有`key`的`hash`值相同的值都被放在bucket中。冲突的放入链表。
![](https://i.imgur.com/6G4Vsyj.jpg)

### 下标确定
生成bucket下标的方法有三步(hash)：
<pre>
static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);//扰动函数，自己的高半区和低半区做异或，为了混合原始
							//哈希值的高位和低位，以此来加大低位的随机性。而且混合后的低位掺杂了高位的部分特征，这样高位的信息
                            //也变相保留了下来。
    }
    p = tab[i = (n - 1) & hash//插入时与数组长度做并运算保留低位信息，在putval方法里。
								//等价于对 length 取模，也就是h % length，但是 & 比 % 具有更高的效率。

</pre>

`key.hashCode()`函数调用的时`key`键值类型自带的哈希函数，返回`int`类型的散列表。散列表范围很大，但用之前还要先做对数组的长度取模运算，得到余数才能用来访问数组下标。这也正好解释了hashmap为什么要将数组长度取2的整次幂。因为数组长度减1正好相当于一个低位掩码。将散列值的高位全部归零，只保留低位值，用来做数组下标访问。
![](https://i.imgur.com/v5ZpR9X.png)

### putval函数

<pre>
final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
        if ((tab = table) == null || (n = tab.length) == 0)//如果当前map为空，执行resize方法。并且返回长度  
            n = (tab = resize()).length;
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);//如果要插入的键值对要存放的这个位置没有元素，封装成Node对象此处 
        else {
            Node<K,V> e; K k;
			//如果这个元素的key与要插入的一样，那么就替换
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode) 如果当前节点是TreeNode类型的数据，执行putTreeVal方法 
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
				//遍历链表上的数据
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
						//判断，是否可能执行treeifyBin方法  
                        if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))//如果已经存在则停止遍历
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null) //更新存在值
                    e.value = value;
                afterNodeAccess(e); //空，为LinkedHashMap(HashMap的子类)留的回调函数
                return oldValue;
            }
        }
        ++modCount; //将修改记录
        if (++size > threshold) //当前值大于threshold，扩容
            resize();
        afterNodeInsertion(evict); //空，为LinkedHashMap(HashMap的子类)留的回调函数
        return null;
    }
</pre>

### 容量及扩容
默认容量大小为16。可以在初始化时设置容量大小和`loadFactor`(默认0.75）。
<pre>
public HashMap(int initialCapacity, float loadFactor)
</pre>
对容量有个方法，可以保证返回一个比给定整数大且最接近的2的幂次方的整数。(利用位操作和或操作）
<pre>
static final int tableSizeFor(int cap) {
        int n = cap - 1;
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
</pre>
当你向`map`中添加新的`key/value`时，将会检查是否需要扩容。`map`存了两个数据：`map`的`size`和`threshold`。`size`表示在map中存的entry的数目。这个值在每次插入和删除时都会更新。`threshold`等于数组容量*`loadFactor`。在每次扩容后刷新。

扩容的函数如下：
<pre>
// 扩容兼初始化
final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length; //数组原来的长度
        int oldThr = threshold; //原来的临界值
        int newCap, newThr = 0;
        if (oldCap > 0) {
            // 扩容
			// 原数组长度大于最大容量(1073741824) 则将threshold设为Integer.MAX_VALUE=2147483647
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
				// 新数组长度 是原来的2倍，临界值也扩大为原来2倍，在满足条件的情况下
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
				// 如果原来的thredshold大于0则将容量设为原来的thredshold
                // 在第一次带参数初始化时候会有这种情况（see带参的初始化函数）
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
			// 在默认无参数初始化会有这种情况
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
			 //如果新的容量等于0
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;// 将临界值设置为新临界值
        @SuppressWarnings({"rawtypes","unchecked"})
            Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
		//如果原来的table有数据，则将数据复制到新的table中
        if (oldTab != null) {
			// 根据容量进行循环整个数组，将非空元素进行复制
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
					// 如果链表只有一个，则进行直接赋值
                    if (e.next == null)
                        newTab[e.hash & (newCap - 1)] = e; //确定元素存放位置
                    else if (e instanceof TreeNode) //红黑树存放（后详）
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
						// 进行链表复制
                        // 没有重新计算元素在数组中的位置
                        // 而是采用了原始位置加原数组长度的方法计算得到位置
						//命名lo和hi，就是将新table分为了两部分，原先大小的部分（lo）和新扩容大小的部分（hi）。
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
							//计算元素的在数组中的位置是否需要移动，确定当前节点时放在lo还是hi中。可进行数学推导。详见参考5
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                        if (loTail != null) {
                            loTail.next = null;// 将链表的尾节点 的next 设置为空
                            newTab[j] = loHead;
                        }
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }
</pre>

### 线程安全
HashMap和Hashtable都实现了Map接口，但决定用哪一个之前先要弄清楚它们之间的分别。主要的区别有：线程安全性，同步(synchronization)，以及速度。
HashMap几乎可以等价于Hashtable，除了HashMap是非synchronized的，并可以接受null(HashMap可以接受为null的键值(key)和值(value)，而Hashtable则不行)。
HashMap是非synchronized，而Hashtable是synchronized，这意味着Hashtable是线程安全的，多个线程可以共享一个Hashtable；而如果没有正确的同步的话，多个线程是不能共享HashMap的。Java 5提供了ConcurrentHashMap，它是HashTable的替代，比HashTable的扩展性更好。
另一个区别是HashMap的迭代器(Iterator)是fail-fast迭代器，而Hashtable的enumerator迭代器不是fail-fast的。所以当有其它线程改变了HashMap的结构（增加或者移除元素），将会抛出ConcurrentModificationException，但迭代器本身的remove()方法移除元素则不会抛出ConcurrentModificationException异常。但这并不是一个一定发生的行为，要看JVM。这条同样也是Enumeration和Iterator的区别。
由于Hashtable是线程安全的也是synchronized，所以在单线程环境下它比HashMap要慢。如果你不需要同步，只需要单一线程，那么使用HashMap性能要好过Hashtable。
HashMap不能保证随着时间的推移Map中的元素次序是不变的。
注：
**Fail-fast和iterator迭代器相关。如果某个集合对象创建了Iterator或者ListIterator，然后其它的线程试图“结构上”更改集合对象，将会抛出ConcurrentModificationException异常。但其它线程可以通过set()方法更改集合对象是允许的，因为这并没有从“结构上”更改集合。但是假如已经从结构上进行了更改，再调用set()方法，将会抛出IllegalArgumentException异常。
结构上的更改指的是删除或者插入一个元素，这样会影响到map的结构。**

### 参考
1. [How does a HashMap work in JAVA](http://coding-geek.com/how-does-a-hashmap-work-in-java/)
2. [jdk源码中hashmap的hash方法原理是什么](https://www.zhihu.com/question/20733617/answer/111577937)
3. [HashMap中tableSizeFor的一个精巧的算法](https://blog.csdn.net/dagelailege/article/details/52972970)
4. [Java 1.8中HashMap的resize()方法扩容部分的理解](https://blog.csdn.net/u013494765/article/details/77837338)
5. [jdk1.8 HashMap put时确定元素位置的方法与扩容拷贝元素确定位置的方式冲突？](https://www.zhihu.com/question/44460053)
6. [哈希表](http://www.cnblogs.com/jiewei915/archive/2010/08/09/1796042.html)