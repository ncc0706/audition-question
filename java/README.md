# Java 常见面试题

## 集合
Collection是集合顶层接口，已知子接口List、Set、Queue

### List

List是一个有序集合，可以包含重复元素，提供了索引的访问方式，继承`Collection`，有两个重要的实现类: ArrayList和LinkedList

[ArrayList的实现原理](http://www.cnblogs.com/ITtangtang/p/3948555.html)

ArrayList、LinkedList、Vector都实现了List接口，主要区别在于实现方式不同，不同的操作有不同的效率。

* ArrayList是一个可变大小是数组，当更多的元素加入到ArrayList中，其大小将会动态的增长。内部元素可以直接通过get或set方法进行访问,ArrayList本质上就是数组
* LinkedList是一个双向链表，在添加、删除元素时比ArrayList性能更好，但get、set弱于ArrayList，LinkedList还实现了Queue接口，该接口比List提供了更多的方法......
* Vector和ArrayList类似，但属于强同步类，Vector的所有操作方法被同步了，Vector和ArrayList在更多元素添加进来时会请求更大的空间。Vector每次请求其大小的双倍空间，ArrayList每次对size增长50%。


> ArrayList、LinkedList是非线程安全的

> Vector是线程安全的

**ArrayList、LinkedList 遍历方式性能分析**
主要介绍ArrayList和LinkedList这两种list的常用循环遍历方式，各种方式的性能分析。熟悉java的知道，常用的list的遍历方式有以下几种：

1. for-each
``` java
List<String> testList = new ArrayList<String>();
	for (String tmp : testList) {	
	  //use tmp;
}
```
这种遍历方式是最常用的遍历方式，因为书写比较方便，而且不需要考虑数组越界的问题，Effective-Java中推荐使用此种写法遍历。

2. 迭代器方式
``` java
List<String> testList = new ArrayList<String>();
for (Iterator<String> iterator = testList.iterator(); iterator.hasNext();){
    //String tmp = iterator.next();
}
```

3. 下标递增或递减循环
``` java
List<String> testList = new ArrayList<String>();
for (int i = 0; i < testList.size(); i++;){
    //String tmp = testList.get(i);
}
```

下标递增或者递减循环是最早接触到的遍历方式，会经常出现数组越界的问题。

以上三种遍历方式是在使用list时最常用的方式，那么这三种方式在遍历的速度已经性能上又有什么区别呢？我们从数据的底层实现上来进行分析。

List底层储存都是使用数组来进行存储的，ArrayList是直接通过数组来进行存储，而LinkedList则是使用数组模拟指针，来实现链表的方式，因此从这里就可以总结出，ArrayList在使用下标的方式循环遍历的时候性能最好，通过下标可以直接取数据，速度最快。而LinkedList因为有一层指针，无法直接取到对应的下标，因此在使用下标遍历时就需要计算对应的下面是哪个元素，从指针的头一步一步的走，所以效率就很低。想到指针就会联想到迭代器，迭代器可以指向下一个元素，而迭代器就是使用指针来实现的，因此LinkedList在使用迭代器遍历时会效率最高，迭代器直接通过LinkedList的指针进行遍历，ArrayList在使用迭代器时，因为要通过ArrayList先生成指针，因此效率就会低于下标方式，而for-each又是在迭代器基础上又进行了封装，因此效率会更低一点，但是会很接近迭代器。

总结：在进行list遍历时，如果是对ArrayList进行遍历，推荐使用下标方式，如果是LinkedList则推荐使用迭代器方式。

### Set

无序不重复的集合。

对集合中存在的对象的访问和操作是通过对象的引用进行的，因此集合中不存放重复对象。

* HashSet：哈希表是通过使用称为散列法的机制来存储信息的，元素并没有以某种特定顺序来存放；
* LinkedHashSet：以元素插入的顺序来维护集合的链接表，允许以插入的顺序在集合中迭代；
* TreeSet：提供一个使用树结构存储Set接口的实现，对象以升序顺序存储，访问和遍历的时间很快。

#### HashSet
1. HashSet是采用hash表来实现的。其中的元素没有按顺序排列，add()、remove()以及contains()等方法都是复杂度为O(1)的方法。

#### LinkedHashSet
1. LinkedHashSet 是 HashSet 的子类。
2. LinkedHashSet 集合根据元素的 hashCode 值来决定元素的存储位置，但它同时使用链表维护元素的次序，这使得元素看起来是以插入顺序保存的。
3. LinkedHashSet 性能插入性能略低于 HashSet，但在迭代访问 Set 里的全部元素时有很好的性能。
4. LinkedHashSet 有序但不允许集合元素重复。
5. LinkedHashSet介于HashSet和TreeSet之间。它也是一个hash表，但是同时维护了一个双链表来记录插入的顺序。基本方法的复杂度为O(1)。

#### TreeSet

1. TreeSet 是 SortedSet 接口的实现类，TreeSet 可以确保集合元素处于排序状态。
2. TreeSet 支持两种排序方法：自然排序和定制排序。默认情况下，TreeSet 采用自然排序。
3. TreeSet是采用树结构实现(红黑树算法)。元素是按顺序进行排列，但是add()、remove()以及contains()等方法都是复杂度为O(log (n))的方法。它还提供了一些方法来处理排序的set，如first(), last(), headSet(), tailSet()等等。

## Map

它有四个实现类,分别是HashMap Hashtable LinkedHashMap 和TreeMap.

### HashMap

1. HashMap 是一个最常用的Map,它根据键的HashCode值存储数据,根据键可以直接获取它的值，具有很快的访问速度，遍历时，取得数据的顺序是完全随机的。
2. HashMap最多只允许一条记录的键为Null;允许多条记录的值为 Null;
3. HashMap不支持线程的同步，即任一时刻可以有多个线程同时写HashMap;可能会导致数据的不一致。如果需要同步，可以用 Collections的synchronizedMap方法使HashMap具有同步的能力，或者使用ConcurrentHashMap


### HashTable

Hashtable与 HashMap类似,它继承自Dictionary类，不同的是:它不允许记录的键或者值为空;它支持线程的同步，即任一时刻只有一个线程能写Hashtable,因此也导致了 Hashtable在写入时会比较慢。

### LinkedHashMap

LinkedHashMap 是HashMap的一个子类，保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的.也可以在构造时用带参数，按照应用次数排序。在遍历的时候会比HashMap慢，不过有种情况例外，当HashMap容量很大，实际数据较少时，遍历起来可能会比 LinkedHashMap慢，因为LinkedHashMap的遍历速度只和实际数据有关，和容量无关，而HashMap的遍历速度和他的容量有关。

### TreeMap

TreeMap实现SortMap接口，能够把它保存的记录根据键排序,默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator 遍历TreeMap时，得到的记录是排过序的。好处：一次二分查找就能找到指定范围的数据


**Hashtable和HashMap的区别**

1. 不同主要是历史原因。Hashtable是基于陈旧的Dictionary类的，HashMap是Java 1.2引进的Map接口的一个实现。
2. 也许最重要的不同是Hashtable的方法是同步的，而HashMap的方法不是
3. 只有HashMap可以让你将空值作为一个表的条目的key或value。

## javaBean为什么要实现 序列化 Serializable接口？

实现 serializable后方便把对象直接存储或者进行网络传输 

## WEB

### redirect和forward 区别？？

	forward方式：
	request.getRequestDispatcher("/somePage.jsp").forward(request, response);

	redirect方式：
	response.sendRedirect("/somePage.jsp");

forward是服务器内部重定向，程序收到请求后重新定向到另一个程序，客户机并不知道。

redirect则是服务器收到请求后发送一个状态头给客户，客户将再请求一次，这里多了次网络通信的来往。

forward会将request state ,bean等等信息带往下一个jsp。redirect是送到client端后再一次request,所以资料不被保留.

**总结了一些区别，可以从以下几个方面来看：**

* 从地址栏显示来说

forward是服务器请求资源,服务器直接访问目标地址的URL,把那个URL的响应内容读取过来,然后把这些内容再发给浏览器.浏览器根本不知道服务器发送的内容从哪里来的,所以它的地址栏还是原来的地址.

redirect是服务端根据逻辑,发送一个状态码,告诉浏览器重新去请求那个地址.所以地址栏显示的是新的URL.所以redirect等于客户端向服务器端发出两次request，同时也接受两次response。

* 从数据共享来说

forward:转发页面和转发到的页面可以共享request里面的数据.
redirect:不能共享数据.

redirect不仅可以重定向到当前应用程序的其他资源,还可以重定向到同一个站点上的其他应用程序中的资源,甚至是使用绝对URL重定向到其他站点的资源.forward,方法只能在同一个Web应用程序内的资源之间转发请求.forward 是服务器内部的一种操作.redirect 是服务器通知客户端,让客户端重新发起请求.所以,你可以说 redirect是一种间接的请求, 但是你不能说"一个请求是属于forward还是redirect "

* 从运用地方来说

forward:一般用于用户登陆的时候,根据角色转发到相应的模块.
redirect:一般用于用户注销登陆时返回主页面和跳转到其它的网站等.

* 从效率来说

forward:高.
redirect:低.










