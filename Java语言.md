# 容器类

## 概述

### List接口

继承自Collection接口，用于Collection中的所有方法，在Collection的基础上也添加了许多方法，使得可以在List中插入和删除元素。List有两种基本的实现：ArrayList和LinkedList

- 基本的ArrayList，它适合于随机访问元素，但是在插入和删除元素时就比较慢
- LinkedList适合于在元素插入和删除较频繁时使用，随机访问的速度比较慢

### Set

Set中不保存重复的元素，含义同数学概念上的集合。 Set常用于测试归属性，即查询某个元素是否在某个Set中。正因为如此查找也就成了Set中重要的操作。通常会选择HashSet的实现，它对快速查找进行了优化。Set也有多种不同的实现，不同的Set实现不仅具有不同的行为，而且它们对于可以在特定的Set中防止元素的类型也有不同的要求。

- **Set(interface)**

  存入Set的每个元素必须是唯一对的。**加入Set的元素必须定义equals()方法以确保对象的唯一性**。Set接口不保证维护元素的次序。

- **HashSet**

  为快速查找而设计的Set。存入HashSet的元素必须定义hashCode()。使用HashMap实现。

- **TreeSet**

  保持次序的Set，底层为树结构。使用它可以从Set中提取有序的序列。元素必须实现Comparable接口。使用TreeSet实现。

- **LinkedHashSet**

  具有HashSet的查询速度，且内部使用链表维护元素的顺序（插入的次序）。在使用迭代器遍历该Set时，结果会按照元素插入的次序显示。元素也必须定义hashCode()方法



### Map

#### Map有以下特点:

- Map是将键映射到值的键值对（key-value）接口

- 映射中不能包含重复的键，每个键最多可以映射到一个值，但是一个值可以被多个键映射

- Map提供了三个Set视图供我们访问：键的Set、值的Set和键值对的Set

- 映射的顺序定义为访问的映射Set上的迭代器返回元素的顺序。TreeMa类，可以对映射的顺序做出特定保证；其他的，则不能保证

- 可变对象作为映射键需要非常小心

- Map的实现类应该提供两个“标准“构造函数

  **第一个，void（无参数）构造方法，用于创建空映射**

  **第二个，带有单个 Map 类型参数的构造方法，用于创建一个与其参数具有相同键-值映射关系的新映射**。带有单个 Map 类型参数的构造方法，用于创建一个与其参数具有相同键-值映射关系的新映射

#### Map的几种基本实现：

- **HashMap**

  Map是基于散列表的实现（取代了HashTable）。HashMap使用散列码（对象hashCode()生成的值）来进行快速搜索。

- **LinkedHashMap**

  类似于HashMap，但是迭代的时候，取得键值对的顺序是起插入的顺序，或者是最近最少使用（LRU）的次序。只比HashMap慢一点，而迭代访问的时候更快，因为使用链表维护了内部次序。

- **TreeMap**

  基于红黑树的实现。查看“键”或者“键值对”时，它们会被排序（次序由Comparable或Comparator决定）。TreeMap的特点在于所得到的结果是经过排序的。TreeMap是唯一的带有subMap()的Map，可以返回一个子树。

- **WeakHashMap**

  弱键（weak key）映射，允许释放映射所指向的对象，这是为了解决某类特殊问题而设计的。如果映射之外没有引用指向某个“键”，则此“键”可以被垃圾回收。

- **ConcurrentHashMap**

  一种线程安全的Map，不涉及同步加锁。在并发中还会介绍。

### Stack和Queue

Stack是一个**先进后出（LIFO）**的容器。往盒子中放书，先放进去的最后才拿得出来，最后放进去的第一个就可以取出，这种模型就是栈（Stack）可以描述的。LinkedList中有可以实现栈所有功能的方法，有时也可以直接将LinkedList作为栈使用。

队列是一个典型的先进先出（FIFO）的容器。事物放进容器的顺序和取出的顺序是相同的（优先级队列根据事物优先级出队事物）。队列常被当做一种可靠的将对象从程序的某个区域传输到另一个区域的途径。**队列在并发编程中特别重要**。同样，LinkedList也提供了方法支持队列的行为，并且它实现了Queue接口。

#### 优先级队列PriorityQueue

先进先出描述了典型的队列规则。**队列规则是指在给定一组队列的元素情况下，确定下一个弹出队列的元素的规则**。优先级队列声明的下一个弹出的元素是最需要的元素（具有最高优先级的元素）。

我们可以在PriorityQueue上调用**offer()**方法来插入一个对象，这个对象就会在队列中被排序，默认排序为自然排序，即按插入的先后进行排序，但是我们可以通过提供自己的Comparator来修改这个排序。当调用peek()、poll()和remove()方法时，将获取队列优先级最高的元素。

优先级队列算法实现的数据结构通常是一个堆。



### 迭代器

对于访问容器而言，有没有一种方式使得同一份遍历的代码可以适用于不同类型的容器？要实现这样的目的就可以使用迭代器。**使用迭代器对象，遍历并选择序列中的对象，而客户端不必知道或关心该序列底层的结构**。Java中对迭代器有一些限制，比如Java的Iterator只能单向移动，这个Iterator只能用来：

- 使用next()方法获得序列的下一个元素
- 使用hasNext()方法检查序列中是否还有元素
- 使用remove()方法将迭代器新近返回的元素删除，意味着在调用remove()之前必须先调用next()

![img](https://img2018.cnblogs.com/blog/1099419/201903/1099419-20190302101859824-126942292.png)

API中的Iterator接口中方法如上，实现Iterator对象需要实现hashNext()方法和next()方法，remove方法是一个可选操作。forEachRemaining是Java 1.8（Java SE8）中加入的方法，用于Lambda表达式。

showElement()方法不包含任何有关它遍历的序列类型信息，这就展示了Iterator的好处：**能够将遍历序列的操作与序列底层结构分离**。也可以说，**迭代器统一了对容器的访问方式**。

从容器框架图中我们可以看出，Collection是描述所有序列容器的共性的根接口。但是在C++中，标准的C++类库中没有其他容器的任何公共基类，容器之间的共性都是通过迭代器达成的。在Java中，则将两种方法绑定到了一起，**实现Collection的同时也要实现iterator()方法**（返回该容器的迭代器）。

#### ListIterator

ListIterator是一个更加强大的Iterator子类型，但是它只能用于各种List的访问。

- Iterator只能前向移动，但**ListIterator允许我们可以前后移动**。
- 它还可以产生相对于迭代器在列表中指向当前位置的前一个和后一个索引，并且可以使用set()方法替换它访问过的最后一个元素。
- remove()方法可以删除它访问过的最后一个元素。需要注意，这两处的最后一个元素只的都是调用next()或者previous返回的元素，也就意味着调用set()、remove()这两个方法之前，要先调用next()或者previous()。

需要注意ListIterator在序列中的游标位置与Iterator不同，**Iterator的游标位置始终位于调用previous()将返回的元素和调用next()将返回的元素之间**。长度为n的列表的迭代器的游标位置有n+1个。

![img](https://img2018.cnblogs.com/blog/1099419/201903/1099419-20190302101944888-63890359.png)

使用ListIterator对列表进行正向和返回迭代，以及使用set()替换列表元素的例子：

```java
public class ListIteration {
    public static void main(String[] args) {
        List<Cat> catList = new ArrayList<>();
        for(int i=0; i<5; i++) {
            catList.add(new Cat());
        }
        
        ListIterator<Cat> it = catList.listIterator();
        System.out.println("CatNo.\t nextIndex\t previousIndex");
        
        //正向遍历
        System.out.println("正向遍历：");
        while (it.hasNext()) {
            Cat cat = it.next();
            System.out.println(cat+"\t\t"+it.nextIndex()+"\t\t"+it.previousIndex());
        }
        System.out.println();
        
        System.out.println("当迭代器游标处于最后一个元素末尾时：");
        ListIterator<Cat> it2 = catList.listIterator();
        while (it2.hasNext()) {
            Cat cat = it2.next();
            System.out.println(cat+"\t\t"+it.nextIndex()+"\t\t"+it.previousIndex());
        }
        System.out.println();
        
        //反向遍历
        System.out.println("反向遍历");
        while(it.hasPrevious()) {
            Cat cat = it.previous();
            System.out.println(cat+"\t\t"+it.nextIndex()+"\t\t"+it.previousIndex());
        }
        System.out.println();
        
        //产生指定游标位置的迭代器 从第二个位置开始向前替换列表中的Cat对象
        System.out.println("从第二个位置开始向前替换列表中的Cat对象");
        it = catList.listIterator(2);
        while(it.hasNext()) {
            it.next();
            it.set(new Cat());
        }
        System.out.println(catList);
    }
}
/*
CatNo.   nextIndex   previousIndex
正向遍历：
Cat: 0      1       0
Cat: 1      2       1
Cat: 2      3       2
Cat: 3      4       3
Cat: 4      5       4

当迭代器游标处于最后一个元素末尾时：
Cat: 0      5       4
Cat: 1      5       4
Cat: 2      5       4
Cat: 3      5       4
Cat: 4      5       4

反向遍历
Cat: 4      4       3
Cat: 3      3       2
Cat: 2      2       1
Cat: 1      1       0
Cat: 0      0       -1

从第二个位置开始向前替换列表中的Cat对象
[Cat: 0, Cat: 1, Cat: 5, Cat: 6, Cat: 7]
*/
```

#### foreach与迭代器

foreach语法不仅可以用在数组，也可以用在任何Collection对象。之所以可以用在Collection对象，是因为Java SE5引入了`Iterable`接口，**该接口包含一个能够产生Iterator的iterator()方法，并且Iterable接口被foreach用来在序列中移动**。因此，如果创建了任何实现Iterable的类，都可以将它用于foreach当中。**需要注意，数组虽然可以使用foreach语法遍历，但不意味着数组是Iterable的**。

实现一个可迭代的类，使用foreach方法遍历

```java
public class IterableClass implements Iterable<String>{
    private String[] words = ("This is happy day.").split(" ");
    @Override
    public Iterator<String> iterator() {
        return new Iterator<String>() {
            private int index = 0;
            //判断是否存在下一个元素
            public boolean hasNext() {
                return index < words.length;
            }
            //返回下一个元素
            public String next() {
                return words[index++];
            }
            public void remove() {  //remove可以不用实现
                throw new UnsupportedOperationException();
            }
        };
    }
    
    public static void main(String[] args) {
        //foreach语法遍历实现了Iterable接口的类
        for(String s : new IterableClass()) {
            System.out.println(s);
        }
    }
}
/*
This
is
happy
day.
*/
```