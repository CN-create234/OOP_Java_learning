# **迭代器（Iterator）实现代码**。这是 `ArraySequence` 类内部的三个迭代器类，用于遍历集合元素。

---

## 整体结构

```
ArraySequence 类（外部类）
    ├── ForwardIterator 类   → 正向迭代器（从前往后）
    ├── ReverseIterator 类   → 反向迭代器（从后往前）
    └── BiIterator 类        → 双向迭代器（可前可后）
```

---

## 一、ForwardIterator（正向迭代器）

### 成员变量

```java
private class ForwardIterator implements SeqIterator {
```
**第1行**：定义一个私有内部类 `ForwardIterator`，实现 `SeqIterator` 接口。`private` 表示只能在外部类 `ArraySequence` 内部使用。

```java
    private int cursor;
```
**第2行**：`cursor`（游标/光标），指向**下一个要返回的元素的位置**。初始值为 `0`（默认）。

```java
    private int lastReturned = -1;
```
**第3行**：`lastReturned`，记录**最后一次返回的元素索引**。初始为 `-1` 表示"还没有返回过任何元素"，用于 `remove()` 方法判断是否可以删除。

---

### hasNext() 方法

```java
    @Override
    public boolean hasNext() {
```
**第5行**：重写接口方法，判断是否还有下一个元素。

```java
        return cursor < size;
```
**第6行**：`size` 是外部类 `ArraySequence` 的成员变量（元素总数）。如果 `cursor < size`，说明还有元素未遍历。

---

### next() 方法

```java
    @Override
    public SequenceItem next() {
```
**第9行**：重写接口方法，返回下一个元素。

```java
        if (!hasNext()) {
```
**第10行**：先检查是否还有元素。

```java
            throw new NoSuchElementException();
```
**第11行**：如果没有，抛出 `NoSuchElementException`（Java 标准异常）。

```java
        }
        lastReturned = cursor;
```
**第13行**：记录当前 `cursor` 位置，供 `remove()` 使用。

```java
        return elements[cursor++];
```
**第14行**：
- `elements` 是外部类的数组（存储元素的容器）
- `cursor++` 先返回 `elements[cursor]`，然后将 `cursor` 加 1
- 例如：`cursor=0` 时，返回 `elements[0]`，然后 `cursor` 变为 `1`

---

### remove() 方法

```java
    @Override
    public void remove() {
```
**第17行**：重写接口方法，删除**最后一次返回的元素**。

```java
        if (lastReturned < 0) {
```
**第18行**：检查 `lastReturned` 是否小于 0（即还没有调用过 `next()`，或已经删除过了）。

```java
            throw new IllegalStateException("No element to remove");
```
**第19行**：如果没有元素可删除，抛出 `IllegalStateException`。

```java
        }
        ArraySequence.this.removeAt(lastReturned);
```
**第21行**：调用外部类 `ArraySequence` 的 `removeAt()` 方法，删除索引为 `lastReturned` 的元素。
- 写法 `ArraySequence.this` 表示**外部类的 this 引用**（在内部类中访问外部类成员）

```java
        if (lastReturned < cursor) {
```
**第22行**：判断被删除的元素是否在 `cursor` 之前。
- 如果删除的元素在 `cursor` 之前，`cursor` 需要左移一位（因为数组元素前移了）

```
举例：数组 [A, B, C, D]，cursor=2（指向 C），删除索引 0（A）
删除后数组变成 [B, C, D]，cursor 应该变成 1（指向 C）
lastReturned(0) < cursor(2) → true → cursor--
```

```java
            cursor--;
```
**第23行**：`cursor` 减 1。

```java
        }
        lastReturned = -1;
```
**第25行**：重置 `lastReturned` 为 -1，防止重复删除同一个元素。

---

## 二、ReverseIterator（反向迭代器）

### 成员变量

```java
private class ReverseIterator implements SeqIterator {
```
**第30行**：定义反向迭代器类。

```java
    private int cursor = size - 1;
```
**第31行**：`cursor` 初始化为 `size - 1`，指向**最后一个元素**（从后往前遍历）。

```java
    private int lastReturned = -1;
```
**第32行**：同正向迭代器，记录最后返回的元素索引。

---

### hasNext() 方法

```java
    @Override
    public boolean hasNext() {
        return cursor >= 0;
```
**第35-36行**：判断 `cursor` 是否 ≥ 0。从后往前遍历，当 `cursor` 变为 -1 时表示遍历结束。

---

### next() 方法

```java
    @Override
    public SequenceItem next() {
        if (!hasNext()) {
            throw new NoSuchElementException();
        }
        lastReturned = cursor;
        return elements[cursor--];
```
**第39-45行**：
- 检查是否有元素
- 记录当前 `cursor` 位置
- 返回 `elements[cursor]`，然后将 `cursor` 减 1（从后往前移动）

---

### remove() 方法

```java
    @Override
    public void remove() {
        if (lastReturned < 0) {
            throw new IllegalStateException("No element to remove");
        }
        ArraySequence.this.removeAt(lastReturned);
        if (lastReturned <= cursor) {
            cursor--;
        }
        lastReturned = -1;
```
**第48-56行**：

**关键差异**：`if (lastReturned <= cursor)`

```
举例：数组 [A, B, C, D]，反向遍历中 cursor=1（指向 B），删除索引 2（C）
删除后数组变成 [A, B, D]，cursor 应该变成 1（指向 B，保持不变）
lastReturned(2) <= cursor(1) → false → cursor 不变

举例：数组 [A, B, C, D]，反向遍历中 cursor=2（指向 C），删除索引 1（B）
删除后数组变成 [A, C, D]，cursor 应该变成 1（指向 C）
lastReturned(1) <= cursor(2) → true → cursor--
```

---

## 三、BiIterator（双向迭代器）

### 成员变量

```java
private class BiIterator implements SeqBiIterator {
```
**第60行**：定义双向迭代器，实现 `SeqBiIterator` 接口（同时支持正向和反向遍历）。

```java
    private int cursor;
```
**第61行**：指向下一个要返回的元素位置（初始为 0）。

```java
    private int lastReturned = -1;
```
**第62行**：记录最后返回的元素。

---

### hasNext() 和 next()

```java
    @Override
    public boolean hasNext() {
        return cursor < size;
    }

    @Override
    public SequenceItem next() {
        if (!hasNext()) {
            throw new NoSuchElementException();
        }
        lastReturned = cursor;
        return elements[cursor++];
```
**第64-73行**：与 `ForwardIterator` 完全相同，用于正向遍历。

---

### hasPrevious() 和 previous()

```java
    @Override
    public boolean hasPrevious() {
        return cursor > 0;
```
**第76-77行**：判断是否有前一个元素。`cursor > 0` 说明前面还有元素。

```java
    @Override
    public SequenceItem previous() {
        if (!hasPrevious()) {
            throw new NoSuchElementException();
        }
        cursor--;
        lastReturned = cursor;
        return elements[cursor];
```
**第80-86行**：
1. 检查是否有前一个元素
2. `cursor--`：先往前移动一位
3. `lastReturned = cursor`：记录当前位置
4. 返回 `elements[cursor]`

```
正向遍历时：
   cursor=0 → next() → 返回 elements[0]，cursor=1
   cursor=1 → next() → 返回 elements[1]，cursor=2

反向遍历时：
   cursor=2 → previous() → cursor=1，返回 elements[1]
   cursor=1 → previous() → cursor=0，返回 elements[0]
```

---

### remove() 方法

```java
    @Override
    public void remove() {
        if (lastReturned < 0) {
            throw new IllegalStateException("No element to remove");
        }
        ArraySequence.this.removeAt(lastReturned);
        if (lastReturned < cursor) {
            cursor--;
        }
        lastReturned = -1;
```
**第89-97行**：与 `ForwardIterator` 完全相同。

---

## 四、三个迭代器的对比总结

| 特性 | ForwardIterator | ReverseIterator | BiIterator |
|------|----------------|-----------------|------------|
| **遍历方向** | 从前往后 | 从后往前 | 双向 |
| **cursor 初始值** | 0 | size - 1 | 0 |
| **hasNext() 条件** | cursor < size | cursor >= 0 | cursor < size |
| **next() 操作** | cursor++ | cursor-- | cursor++ |
| **hasPrevious()** | ❌ 无 | ❌ 无 | ✅ 有 |
| **previous()** | ❌ 无 | ❌ 无 | ✅ 有 |
| **remove()** | 同 | 略有不同 | 同 |

---

## 五、remove() 方法的两种逻辑对比

```
删除元素后 cursor 调整规则：

┌─────────────────────────────────────────────────────────────┐
│  ForwardIterator / BiIterator                              │
│  if (lastReturned < cursor) cursor--;                     │
│                                                             │
│  ReverseIterator                                           │
│  if (lastReturned <= cursor) cursor--;                    │
└─────────────────────────────────────────────────────────────┘
```

**为什么不同？**

| 迭代器 | cursor 含义 | 删除后 cursor 调整 |
|--------|------------|-------------------|
| Forward | 指向**下一个**要返回的元素 | 删除位置在 cursor 之前 → cursor-- |
| Reverse | 指向**当前**要返回的元素 | 删除位置在 cursor 之前或等于 → cursor-- |

---

## 六、完整执行示例

```java
ArraySequence seq = new ArraySequence();
seq.add("A");
seq.add("B");
seq.add("C");

// 正向遍历
ForwardIterator it = seq.new ForwardIterator();
while (it.hasNext()) {
    System.out.print(it.next() + " ");  // A B C
}

// 反向遍历
ReverseIterator rit = seq.new ReverseIterator();
while (rit.hasNext()) {
    System.out.print(rit.next() + " ");  // C B A
}

// 双向遍历
BiIterator bit = seq.new BiIterator();
while (bit.hasNext()) {
    System.out.print(bit.next() + " ");  // A B C
}
// 然后反向
while (bit.hasPrevious()) {
    System.out.print(bit.previous() + " ");  // C B A
}
```

---

如果有其他疑问，随时告诉我！
