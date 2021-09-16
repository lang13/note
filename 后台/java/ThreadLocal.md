### ThreadLocal使用与原理

#### ThreadLocal的特点

> 从字面意思上看，ThreadLocal可以解释成线程的局部变量，也就是说一个ThreadLocal的变量只有当前自身线程可以访问，别的线程都访问不了，运用ThreadLocal可以避免线程的竞争

#### ThreadLocal的基本实用

```java
private ThreadLocal<Integer> localInt = new ThreadLocal<>();
// 设置参数
localInt.set(8);
// 获取参数
localInt.get(); // 返回 8
```

> 由于ThreadLocal里设置的值，只有当前线程自己看得见，这意味着你不可能通过其他线程为它初始化值。为了弥补这一点，ThreadLocal提供了一个withInitial()方法统一初始化所有线程的ThreadLocal的值：

```java
// 为每一个线程中的ThreadLocal初始化一个 localInt，但这个localInt的值在每个线程中是相互独立互不影响的
private ThreadLocal<Integer> localInt = ThreadLocal.withInitial(() -> 6);
```

> 上述代码将ThreadLocal的初始值设置为6，这对全体线程都是可见的。

#### ThreadLocal的原理

**关键类：`ThreadLocalMap`、`ThreadLocal.Entry`、`ThreadLocal`**

```java
// Entry的数据接口
static class Entry extends WeakReference<ThreadLocal<?>> {
    /** The value associated with this ThreadLocal. */
    Object value; // 值
	
    // key 是继承了WeakReference中的 get()方法
    // Entry.get()返回的便是 Entry的key
    
    Entry(ThreadLocal<?> k, Object v) {
        super(k);
        value = v;
    }
}
```

ThreadLocal获取或者设置值的时候都需要先获取ThreadLocalMap

```java
// get() 和 set() 方法里面都有这段获取 ThreadLocalMap的代码
Thread t = Thread.currentThread();
ThreadLocalMap map = getMap(t);
```

##### 获取值（get方法）

```java
// ThreadLocal
public T get() {
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t); // 获取 map
    if (map != null) {
        ThreadLocalMap.Entry e = map.getEntry(this); // 获取 Entry
        if (e != null) {
            @SuppressWarnings("unchecked")
            T result = (T)e.value;
            return result;
        }
    }
    return setInitialValue();
}

// ThreadLocal
ThreadLocalMap getMap(Thread t) {
    return t.threadLocals;
}

// ThreadLocalMap
private Entry getEntry(ThreadLocal<?> key) {
    int i = key.threadLocalHashCode & (table.length - 1); // 求哈希值
    Entry e = table[i]; // Entry 数组
    if (e != null && e.get() == key)
        return e;
    else
        return getEntryAfterMiss(key, i, e); // 发生哈希冲突后
}

// 发生哈希冲突后
private Entry getEntryAfterMiss(ThreadLocal<?> key, int i, Entry e) {
    Entry[] tab = table;
    int len = tab.length;

    while (e != null) {
        ThreadLocal<?> k = e.get(); // 获取 Entry中的 key
        if (k == key)
            return e;
        if (k == null)
            expungeStaleEntry(i); // 清理Key已经没人引用的 Entry 中的value的内存	
        else
            i = nextIndex(i, len); // 哈希寻址
        e = tab[i];
    }
    return null;
}
```

##### 设置值（set方法）

```java
// ThreadLocal
public void set(T value) {
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t); // getMap()方法同 get中的getMap()
    if (map != null)
        map.set(this, value);
    else
        createMap(t, value);
}

// ThreadLocalMap
private void set(ThreadLocal<?> key, Object value) {

    // We don't use a fast path as with get() because it is at
    // least as common to use set() to create new entries as
    // it is to replace existing ones, in which case, a fast
    // path would fail more often than not.

    Entry[] tab = table;
    int len = tab.length;
    int i = key.threadLocalHashCode & (len-1);

    for (Entry e = tab[i];
         e != null;
         e = tab[i = nextIndex(i, len)]) {
        ThreadLocal<?> k = e.get();

        if (k == key) {
            e.value = value;
            return;
        }

        if (k == null) {
            replaceStaleEntry(key, value, i); // 清理key已经没人引用的Entry中的value的内存
            return;
        }
    }

    tab[i] = new Entry(key, value);
    int sz = ++size;
    if (!cleanSomeSlots(i, sz) && sz >= threshold)
        rehash();
}
```

##### 移除值（remove方法）

```java
// ThreadLocal
public void remove() {
    ThreadLocalMap m = getMap(Thread.currentThread());
    if (m != null)
        m.remove(this);
}

// ThreadLocalMap
private void remove(ThreadLocal<?> key) {
    Entry[] tab = table;
    int len = tab.length;
    int i = key.threadLocalHashCode & (len-1);
    for (Entry e = tab[i];
         e != null;
         e = tab[i = nextIndex(i, len)]) {
        if (e.get() == key) {
            e.clear();
            expungeStaleEntry(i); // 清理内存
            return;
        }
    }
}
```

