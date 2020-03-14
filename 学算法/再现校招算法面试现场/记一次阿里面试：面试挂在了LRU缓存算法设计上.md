最近春招提前批也要打响了，我会在推送算法+计算机基础的文章下，尽快推一些面试相关的文章

大概是去年的三月份，在找春招实习的时候，面了一次阿里，然后第一面就是写算法题，然而万万没有想到的是，我居然挂在了 LRU 缓存算法上了，这可是我再熟悉不过的算法。。。。。。。今天就来分享一波，说不定，你也做不出！

**一、勤于动脑，懒于行动的祸**

当时做题的时候，自己想的太多了，感觉设计一个 LRU(Least recently used) 缓存算法，不会这么简单啊，于是理解错了题意（我也是服了，还能理解成这样，，，，），自己一波操作写了好多代码，后来卡住了，再去仔细看题，发现自己应该是理解错了，就是这么简单，设计一个 LRU 缓存算法。

不过这时时间就很紧了，按道理如果你真的对这个算法很熟，十分钟就能写出来了，但是，自己虽然理解 LRU 缓存算法的思想，也知道具体步骤，但之前却从来没有去动手写过，导致在写的时候，非常不熟练，也就是说，**你感觉自己会 和你能够用代码完美着写出来是完全不是一回事**。

所以在此提醒各位，如果可以，一定要自己用代码实现一遍自己自以为会的东西。千万不要觉得自己理解了思想，就不用去写代码了，独自撸一遍代码，才是真的理解了，而且面试的时候，需要你再记事本里打代码，不给你编译器的。。。。。。。

不过今天我带大家用代码来实现一遍 LRU 缓存算法，并且提供**最优解**，以后你在遇到这类型的题，保证你完美秒杀它。

#### 题目描述

设计并实现最近最少经用（LRU）缓存的数据结构。它应该支持以下操作：get 和 put。

get(key) - 如果键存在于缓存中，则获取键的值（总是正数），否则返回 -1。

put(key, value) - 如果键不存在，请设置或插入值。当缓存达到其容量时，它应该在插入新项目之前，
使最近最少使用的项目无效。

**进阶：**

你是否可以在 O(1) 时间复杂度内执行两项操作？

示例：

```java
LFUCache cache = new LFUCache( 2 /* capacity (缓存容量) */ );



cache.put(1, 1);

cache.put(2, 2);

cache.get(1);       // 返回 1

cache.put(3, 3);    // 去除 key 2

cache.get(2);       // 返回 -1 (未找到key 2)

cache.get(3);       // 返回 3

cache.put(4, 4);    // 去除 key 1

cache.get(1);       // 返回 -1 (未找到 key 1)

cache.get(3);       // 返回 3

cache.get(4);       // 返回 4
```

#### 基础版：单链表来解决

我们要删的是最近最少使用的节点，一种比较容易想到的方法就是使用单链表这种数据结构来存储了。当我们进行 put 操作的时候，会出现以下几种情况：

1、如果要 put(key,value) 已经存在于链表之中了（根据key来判断），那么我们需要把链表中久的数据删除，然后把新的数据插入到链表的头部。、

2、如果要 put(key,value) 的数据没有存在于链表之后，我们我们需要判断下缓存区是否已满，如果满的话，则把链表尾部的节点删除，之后把新的数据插入到链表头部。如果没有满的话，直接把数据插入链表头部即可。

对于 get 操作，则会出现以下情况

1、如果要 get(key) 的数据存在于链表中，则把 value 返回，并且把该节点删除，删除之后把它插入到链表的头部。

2、如果要 get(key) 的数据不存在于链表之后，则直接返回 -1 即可。

大概的思路就是这样，不要觉得很简单，让你手写的话，十分钟你不一定手写的出来。具体的代码，为了不影响阅读，我在文章的最后面在放出来。

**时间、空间复杂度分析**

对于这种方法，put 和 get 都需要遍历链表查找数据是否存在，所以时间复杂度为 O(n)。空间复杂度为 O(1)。

#### 空间换时间

在实际的应用中，当我们要去读取一个数据的时候，会先判断该数据是否存在于缓存器中，如果存在，则返回，如果不存在，则去别的地方查找该数据（例如磁盘），找到后在把该数据存放于缓存器中，在返回。

所以在实际的应用中，put 操作一般伴随着 get 操作，也就是说，get 操作的次数是比较多的，而且命中率也是相对比较高的，进而 put 操作的次数是比较少的，我们我们是可以考虑采用**空间换时间**的方式来加快我们的 get 的操作的。

例如我们可以用一个额外哈希表（例如HashMap）来存放 key-value，这样的话，我们的 get 操作就可以在 O(1) 的时间内寻找到目标节点，并且把 value 返回了。

然而，大家想一下，用了哈希表之后，get 操作真的能够在 O(1) 时间内完成吗？

用了哈希表之后，虽然我们能够在 O(1) 时间内找到目标元素，可以，我们还需要删除该元素，并且把该元素插入到链表头部啊，删除一个元素，我们是需要定位到这个元素的**前驱**的，然后定位到这个元素的前驱，是需要 O(n) 时间复杂度的。

最后的结果是，用了哈希表时候，最坏时间复杂度还是 O(1)，而空间复杂度也变为了 O(n)。

#### 双向链表+哈希表

我们都已经能够在 O(1) 时间复杂度找到要删除的节点了，之所以还得花 O(n) 时间复杂度才能删除，主要是时间是花在了节点前驱的查找上，为了解决这个问题，其实，我们可以把**单链表**换成**双链表**，这样的话，我们就可以很好着解决这个问题了，而且，换成双链表之后，你会发现，它要比单链表的操作简单多了。

所以我们最后的方案是：**双链表 + 哈希表**，采用这两种数据结构的组合，我们的 get 操作就可以在 O(1) 时间复杂度内完成了。由于 put 操作我们要删除的节点一般是尾部节点，所以我们可以用一个变量 tai 时刻记录尾部节点的位置，这样的话，我们的 put 操作也可以在 O(1) 时间内完成了。

具体代码如下：

```java
// 链表节点的定义
class LRUNode{
    String key;
    Object value;
    LRUNode next;
    LRUNode pre;

    public LRUNode(String key, Object value) {
        this.key = key;
        this.value = value;
    }
}
```

```java
// LRU
public class LRUCache {
    Map<String, LRUNode> map = new HashMap<>();
    LRUNode head;
    LRUNode tail;
    // 缓存最大容量，我们假设最大容量大于 1，
    // 当然，小于等于1的话需要多加一些判断另行处理
    int capacity;

    public LRUCache(int capacity) {
        this.capacity = capacity;
    }

    public void put(String key, Object value) {
        if (head == null) {
            head = new LRUNode(key, value);
            tail = head;
            map.put(key, head);
        }
        LRUNode node = map.get(key);
        if (node != null) {
            // 更新值
            node.value = value;
            // 把他从链表删除并且插入到头结点
            removeAndInsert(node);
        } else {
            LRUNode tmp = new LRUNode(key, value);
            // 如果会溢出
            if (map.size() >= capacity) {
                // 先把它从哈希表中删除
                map.remove(tail.key);
                // 删除尾部节点
                tail = tail.pre;
                tail.next = null;
            }
            map.put(key, tmp);
            // 插入
            tmp.next = head;
            head.pre = tmp;
            head = tmp;
        }
    }

    public Object get(String key) {
        LRUNode node = map.get(key);
        if (node != null) {
            // 把这个节点删除并插入到头结点
            removeAndInsert(node);
            return node.value;
        }
        return null;
    }
    private void removeAndInsert(LRUNode node) {
        // 特殊情况先判断，例如该节点是头结点或是尾部节点
        if (node == head) {
            return;
        } else if (node == tail) {
            tail = node.pre;
            tail.next = null;
        } else {
            node.pre.next = node.next;
            node.next.pre = node.pre;
        }
        // 插入到头结点
        node.next = head;
        node.pre = null;
        head.pre = node;
        head = node;
    }
}
```

这里需要提醒的是，对于链表这种数据结构，头结点和尾节点是两个比较特殊的点，如果要删除的节点是头结点或者尾节点，我们一般要先对他们进行处理。

**这里放一下单链表版本的吧**

```java
// 定义链表节点
class LRUNode{
    String key;
    Object value;
    LRUNode next;

    public LRUNode(String key, Object value) {
        this.key = key;
        this.value = value;
    }
}
// 刚开始把名字写错了，把 LRU写成了RLU
public class LRUCache {
    LRUNode head;
    int size = 0;// 当前大小
    int capacity = 0; // 最大容量

    public LRUCache(int capacity) {
        this.capacity = capacity;
    }

    public Object get(String key) {
        LRUNode cur = head;
        LRUNode pre = head;// 指向要删除节点的前驱
        // 找到对应的节点，并把对应的节点放在链表头部
        // 先考虑特殊情况
        if(head == null)
            return null;
        if(cur.key.equals(key))
            return cur.value;
        // 进行查找
        cur = cur.next;
        while (cur != null) {
            if (cur.key.equals(key)) {
                break;
            }
            pre = cur;
            cur = cur.next;
        }
        // 代表没找到了节点
        if (cur == null)
            return null;

        // 进行删除
        pre.next = cur.next;
        // 删除之后插入头结点
        cur.next = head;
        head = cur;
        return cur.value;
    }

    public void put(String key, Object value) {
        // 如果最大容量是 1，那就没办法了，，，，，
        if (capacity == 1) {
            head = new RLUNode(key, value);
        }
        LRUNode cur = head;
       LRUNode pre = head;
        // 先查看链表是否为空
        if (head == null) {
            head = new RLUNode(key, value);
            return;
        }
        // 先查看该节点是否存在
        // 第一个节点比较特殊，先进行判断
        if (head.key.equals(key)) {
            head.value = value;
            return;
        }
        cur = cur.next;
        while (cur != null) {
            if (cur.key.equals(key)) {
                break;
            }
            pre = cur;
            cur = cur.next;
        }
        // 代表要插入的节点的 key 已存在，则进行 value 的更新
        // 以及把它放到第一个节点去
        if (cur != null) {
            cur.value = value;
            pre.next = cur.next;
            cur.next = head;
            head = cur;
        } else {
            // 先创建一个节点
            LRUNode tmp = new LRUNode(key, value);
            // 该节点不存在，需要判断插入后会不会溢出
            if (size >= capacity) {
                // 直接把最后一个节点移除
                cur = head;
                while (cur.next != null && cur.next.next != null) {
                    cur = cur.next;
                }
                cur.next = null;
                tmp.next = head;
                head = tmp;
            }
        }
    }
}

```

如果要时间，强烈建议自己手动实现一波。

学习更多**算法** + **计算机基础知识**，欢迎关注我的微信公众号，每天准时推送技术干货

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306223728524.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTA3Nzk3,size_16,color_FFFFFF,t_70)



