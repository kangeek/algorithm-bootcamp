# 循环双端队列

## 解题思路

用一个内置的数组保存队列中的数据。由于双端都可以插入和删除数据，因此需要两个游标：`front` 和 `rear`。

- `front` 指向双端队列最前面的数字，`rear` 指向双端队列最后边的数字的下一个数组位置。
- 在队列前端插入元素时，`front` 向前移动；在队列后端插入元素时，`rear` 向后移动。
- 当 `front` 位于数组第 0 位置时，向前移动一位时需要回到数组最后元素；
  类似的，当 `rear` 位于数据最后一个元素位置时，向后移动一位时需要回到数组第 0 位置。
- `front` 和 `rear` 指向一个数组位置时，表示没有元素，也就是队列为空。
- `rear` 下一位就是 `front` 时，表示队列已经满了，因此需要一个 `k + 1` 个元素的数组。

## 结题代码

```java
class MyCircularDeque {

    private int[] nums;
    private int front, rear;
    private int size;

    public MyCircularDeque(int k) {
        this.nums = new int[k + 1];
        this.front = this.rear = 0;
        this.size = k + 1;
    }
    
    public boolean insertFront(int value) {
        if (this.isFull())
            return false;
        front = (front + this.size - 1) % this.size;
        nums[front] = value;
        return true;
    }
    
    public boolean insertLast(int value) {
        if (this.isFull())
            return false;
        nums[rear] = value;
        rear = (rear + 1) % this.size;
        return true;
    }
    
    public boolean deleteFront() {
        if (this.isEmpty())
            return false;
        front = (front + 1) % this.size;
        return true;
    }
    
    public boolean deleteLast() {
        if (this.isEmpty())
            return false;
        rear = (rear + this.size - 1) % this.size;
        return true;
    }
    
    public int getFront() {
        return nums[front];
    }
    
    public int getRear() {
        return nums[(rear + this.size - 1) % this.size];
    }
    
    public boolean isEmpty() {
        return front == rear;
    }
    
    public boolean isFull() {
        return (rear + 1) % this.size == front;
    }
}

/**
 * Your MyCircularDeque object will be instantiated and called as such:
 * MyCircularDeque obj = new MyCircularDeque(k);
 * boolean param_1 = obj.insertFront(value);
 * boolean param_2 = obj.insertrear(value);
 * boolean param_3 = obj.deleteFront();
 * boolean param_4 = obj.deleterear();
 * int param_5 = obj.getFront();
 * int param_6 = obj.getRear();
 * boolean param_7 = obj.isEmpty();
 * boolean param_8 = obj.isFull();
 */
```

## 时空复杂度

时间复杂度：所有的操作都是 `O(1)`。
空间复杂度：需要个数为 `k + 1` 的数组，因此空间复杂度为 `O(k)`。
