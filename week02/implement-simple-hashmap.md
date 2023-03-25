# 实现一个简单的 HashMap

自己动手，用哈希表（开散列）实现一个无须映射（map）。

固定以 String 为 key，int 为 value。支持 `find`、`put`、`remove` 方法即可。

## 作业代码

利用 Java 实现。

```java
import java.util.*;

public class MyHashMap {
    // 使用“数组挂链表”的方式存放哈希桶数据
    private ArrayList<LinkedList<Entry>> buckets;
    private int size;

    /*
     * 存放数据元素的类
     */
    private static class Entry {
        private String key;
        private Integer value;

        public Entry(String key, Integer value) {
            this.key = key;
            this.value = value;
        }

        public String getKey() {
            return key;
        }

        public Integer getValue() {
            return value;
        }

        public void setValue(Integer value) {
            this.value = value;
        }
    }

    public MyHashMap() {
        buckets = new ArrayList<>();
        // 数组的长度为 99991
        for (int i = 0; i < 99991; i++) {
            buckets.add(new LinkedList<>());
        }
    }
    /*
     * 计算散列值，将字符串（只有小写字母）视为 27 进制的数（a=1, b=2, z=26），
     * 计算十进制下的数值，然后对 99991 取模。
     */
    private int calcHashCode(String key) {
        int hash = 0;
        for (int i = 0; i < key.length(); i++) {
            char c = key.charAt(i);
            if (c < 'a' || c > 'z') {
                throw new IllegalArgumentException("Key must contain only lowercase letters.");
            }
            hash = hash * 27 + (c - 'a' + 1);
        }
        return hash % 99991;
    }

    public Integer find(String key) {
        int index = calcHashCode(key);
        LinkedList<Entry> list = buckets.get(index);
        for (Entry entry : list) {
            if (entry.getKey().equals(key)) {
                return entry.getValue();
            }
        }
        return null;
    }

    public void put(String key, Integer value) {
        int index = calcHashCode(key);
        LinkedList<Entry> list = buckets.get(index);
        for (Entry entry : list) {
            if (entry.getKey().equals(key)) {
                entry.setValue(value);
                return;
            }
        }
        list.add(new Entry(key, value));
        size++;
    }

    public Integer remove(String key) {
        int index = calcHashCode(key);
        LinkedList<Entry> list = buckets.get(index);
        Iterator<Entry> iterator = list.iterator();
        while (iterator.hasNext()) {
            Entry entry = iterator.next();
            if (entry.getKey().equals(key)) {
                iterator.remove();
                size--;
                return entry.getValue();
            }
        }
        return null;
    }

    public int size() {
        return size;
    }
}
 
```
