title: HashMap 和 HashTable的区别
tags:
  - Hashmap
  - hashtable
id: 181
categories:
  - Java
date: 2015-05-11 14:03:57
---

1，类的定义
<pre>Public class Hashtable extends Dictionary implements Map, Cloneable, java.io.Serializable

Public class HashMap extends AbstractMap implements Map, Cloneable, Serializable</pre>
可见Hashtable继承自Dictionary而HashMap继承自AbstractMap

Hashtable的put方法
<pre>public synchronized V put(K key, V value){
    if(value == null){
    throws new NullPointerException();
 }
    Entry tab[] = table;
    int hash = key.hashCode();
    int index = (hash $ 0x7FFFFFFF)% tab.length;
    for(Entry e = tab[index]; e != null; e=e.next){
        if((e.hash == hash)&amp;&amp;e.key.equals(key)){
        V old = e.value;
        e.value = value;
        return old;
    }
  }
    modCount++;
    if(count &gt;= threshold){
    rehash();
    tab = table;
    index = (hash &amp; 0x7FFFFFFF) % tab.length;
    }
    Entry e = tab[index];
    tab[index] = new Entry(hash,key,value,e);
    count++;
    return null;
}</pre>
1，put方法时同步的

2，方法不允许value==null

3，方法调用了key的hashCode方法，如果key == null，会抛出空指针异常

HashMap的put方法
<pre>public V put(K key, V value){//#####
    if(key == null)//####
        return putForNullKey(value);
    int hash = hash(key.hashCode());
    int i = indexFor(hash, table.length);
    for(Entry e=table[i];e!=null; e=e.next){
         Object k;
         if(e.hash == hash &amp;&amp; ((k = e.key)==key||key.equals(k))){
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
        }
    }
    modCount++;
    addEntry(hash, key, value, i);//###
    return null;
}</pre>
1，方法为非同步的

2，方法允许key==null

3，方法并没有对value值做任何调整，所以允许null
<table>
<tbody>
<tr>
<td></td>
<td>HashMap</td>
<td>HashTable</td>
</tr>
<tr>
<td>父类</td>
<td>AbstractMap</td>
<td>Dictionary</td>
</tr>
<tr>
<td>Value是否为空</td>
<td>可为空</td>
<td>不可</td>
</tr>
</tbody>
</table>
HashMap是HashTable的轻量级实现（非线程安全的实现），他们都完成了Map接口。

主要区别在于HashMap允许空（null）键值（key），由于非线程安全，效率上可能高于HashTable。

HashMap把Hashtable的contains方法去掉，改成containsvalue和containsKey。因为contains方法容易让人引起误解。

Hashtable继承自Dictionary类，而HashMap是Map interface的接口。

最大的不同是，Hashtable的方法时Synchronize的，而HashMap不是，在多线程访问Hashtable时，不需要自己为它的方法同步，而HashMap就必须为之提供同步Collection.synchronizedMap。

Hashtable和HashMap采用hash/rehash算法都大概一样，所以性能不会有很大的差异。