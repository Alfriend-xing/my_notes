## map
- Map没有继承Collection接口，Map提供key到value的映射。
- 给定一个键和一个值，你可以将该值存储在一个Map对象. 之后，你可以通过键来访问对应的值。
- 当访问的值不存在的时候，方法就会抛出一个NoSuchElementException异常.
- 当对象的类型和Map里元素类型不兼容的时候，就会抛出一个 ClassCastException异常。
- 当在不允许使用Null对象的Map中使用Null对象，会抛出一个NullPointerException 异常。
- 当尝试修改一个只读的Map时，会抛出一个UnsupportedOperationException异常。
```java
// 主要方法

void clear( )
// 从此映射中移除所有映射关系（可选操作）。
boolean containsKey(Object k)
// 如果此映射包含指定键的映射关系，则返回 true。
3boolean containsValue(Object v)
// 如果此映射将一个或多个键映射到指定值，则返回 true。
Set entrySet( )
// 返回此映射中包含的映射关系的 Set 视图。
boolean equals(Object obj)
// 比较指定的对象与此映射是否相等。
Object get(Object k)
// 返回指定键所映射的值；如果此映射不包含该键的映射关系，则返回 null。
int hashCode( )
// 返回此映射的哈希码值。
boolean isEmpty( )
// 如果此映射未包含键-值映射关系，则返回 true。
Set keySet( )
// 返回此映射中包含的键的 Set 视图。
Object put(Object k, Object v)
// 将指定的值与此映射中的指定键关联（可选操作）。
void putAll(Map m)
// 从指定映射中将所有映射关系复制到此映射中（可选操作）。
Object remove(Object k)
// 如果存在一个键的映射关系，则将其从此映射中移除（可选操作）。
int size( )
// 返回此映射中的键-值映射关系数。
Collection values( )
// 返回此映射中包含的值的 Collection 视图。
```
```java
// Map主要子接口对象
Hashtable
// 任何非空（non-null）的对象。同步的
HashMap
// 可空的对象。不同步的 ，但是效率高，较常用。 注：迭代子操作时间开销和HashMap的容量成比例。因此，如果迭代操作的性能相当重要的话，不要将HashMap的初始化容量设得过高，或者load factor过低。
    WeakHashMap
    // 改进的HashMap，它对key实行“弱引用”，如果一个key不再被外部所引用，那么该key可以被GC回收。
SortMap
    TreeMap
```