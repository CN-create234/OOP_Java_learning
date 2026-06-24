# 程序输出 · 选择题精练（综合）

> 覆盖方法重写、静态绑定、动态绑定、构造器调用顺序、异常、I/O 等综合考点。

---

### 第1题
```java
class Parent {
    public void show() {
        System.out.println("Parent show");
    }
}

class Child extends Parent {
    public void show() {
        System.out.println("Child show");
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();
        p.show();
    }
}
```
**输出是什么？**

A. `Parent show`  
B. `Child show`  
C. 编译错误  
D. 运行时异常

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `Parent p = new Child()` 向上转型
- `p.show()` 动态绑定 → 调用实际对象（Child）的 `show()`
- 输出 `Child show`

**考点**：向上转型 + 动态绑定
</details>

---

### 第2题
```java
class A {
    int x = 10;
    void print() {
        System.out.println(x);
    }
}

class B extends A {
    int x = 20;
    void print() {
        System.out.println(x);
    }
}

public class Test {
    public static void main(String[] args) {
        A a = new B();
        System.out.println(a.x);
        a.print();
    }
}
```
**输出是什么？**

A. `10` `20`  
B. `20` `20`  
C. `10` `10`  
D. `20` `10`

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
- `a.x`：成员变量没有多态，看声明类型 `A` → `10`
- `a.print()`：方法有多态，看实际对象 `B` → 输出 `20`
- 变量看类型，方法看对象

**考点**：成员变量 vs 方法的多态差异
</details>

---

### 第3题
```java
class Animal {
    static void type() {
        System.out.println("Animal");
    }
    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    static void type() {
        System.out.println("Dog");
    }
    void sound() {
        System.out.println("Bark");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.type();
        a.sound();
    }
}
```
**输出是什么？**

A. `Animal` `Bark`  
B. `Dog` `Bark`  
C. `Animal` `Animal sound`  
D. `Dog` `Animal sound`

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
- `static` 方法：属于类，没有动态绑定 → 调用声明类型 `Animal` 的 `type()` → `Animal`
- 实例方法：有动态绑定 → 调用实际对象 `Dog` 的 `sound()` → `Bark`
- 静态方法看类型，实例方法看对象

**考点**：静态方法 vs 实例方法的多态差异
</details>

---

### 第4题
```java
class Base {
    Base() {
        System.out.print("B");
    }
}

class Derived extends Base {
    Derived() {
        System.out.print("D");
    }
}

public class Test {
    public static void main(String[] args) {
        Derived d = new Derived();
    }
}
```
**输出是什么？**

A. `D`  
B. `B`  
C. `BD`  
D. `DB`

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- 创建子类对象时，先调用父类构造器，再调用子类构造器
- 先输出 `B`，再输出 `D`
- 输出 `BD`

**考点**：构造器调用顺序（父类先于子类）
</details>

---

### 第5题
```java
class Parent {
    Parent() {
        System.out.print("P");
    }
}

class Child extends Parent {
    Child() {
        System.out.print("C");
    }
    Child(int x) {
        System.out.print("C" + x);
    }
}

public class Test {
    public static void main(String[] args) {
        Child c = new Child(5);
    }
}
```
**输出是什么？**

A. `C5`  
B. `P C5`  
C. `P C`  
D. `C5 P`

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- 无论调用子类哪个构造器，都先调用父类无参构造器
- 输出 `P`
- 然后执行子类构造器 `Child(int x)` → 输出 `C5`
- 输出 `P C5`

**考点**：所有子类构造器都先调用父类构造器
</details>

---

### 第6题
```java
class Parent {
    Parent() {
        System.out.print("P");
    }
}

class Child extends Parent {
    Child() {
        this(10);
        System.out.print("C");
    }
    Child(int x) {
        System.out.print("C" + x);
    }
}

public class Test {
    public static void main(String[] args) {
        Child c = new Child();
    }
}
```
**输出是什么？**

A. `P C10 C`  
B. `P C10`  
C. `P C`  
D. `C10 P C`

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
1. `new Child()` → 先调用父类构造器 → 输出 `P`
2. 执行 `this(10)` → 调用 `Child(int x)`
3. `Child(int x)` 输出 `C10`
4. 返回 `Child()` 继续执行，输出 `C`
5. 最终输出 `P C10 C`

**考点**：`this()` 调用本类其他构造器
</details>

---

### 第7题
```java
class A {
    A() {
        System.out.print("A");
    }
    {
        System.out.print("a");
    }
}

public class Test {
    public static void main(String[] args) {
        A a = new A();
    }
}
```
**输出是什么？**

A. `A`  
B. `a`  
C. `a A`  
D. `A a`

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- 实例初始化块（`{}`）先于构造器执行
- 先输出 `a`，再输出 `A`
- 输出 `a A`

**考点**：实例初始化块 vs 构造器的执行顺序
</details>

---

### 第8题
```java
class A {
    static {
        System.out.print("sA");
    }
    {
        System.out.print("iA");
    }
    A() {
        System.out.print("A");
    }
}

public class Test {
    public static void main(String[] args) {
        A a1 = new A();
        A a2 = new A();
    }
}
```
**输出是什么？**

A. `sA iA A iA A`  
B. `sA iA A sA iA A`  
C. `iA A iA A`  
D. `sA sA iA A iA A`

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
- 静态块：类加载时执行一次 → `sA`
- 第一次创建对象：实例块 → `iA`，构造器 → `A`
- 第二次创建对象：实例块 → `iA`，构造器 → `A`
- 最终输出 `sA iA A iA A`

**考点**：静态块执行一次，实例块每次创建对象都执行
</details>

---

### 第9题
```java
class A {
    A() {
        print();
    }
    void print() {
        System.out.print("A");
    }
}

class B extends A {
    int x = 10;
    B() {
        print();
    }
    void print() {
        System.out.print(x);
    }
}

public class Test {
    public static void main(String[] args) {
        B b = new B();
    }
}
```
**输出是什么？**

A. `010`  
B. `1010`  
C. `A10`  
D. `10`

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
1. `new B()` → 先调用父类 `A()` 构造器
2. `A()` 中调用 `print()`，动态绑定到 `B.print()` → 输出 `x`，但 `x` 尚未初始化，为 `0`
3. 回到 `B()` 构造器，调用 `print()` → 此时 `x` 已初始化（`10`）→ 输出 `10`
4. 最终输出 `010`

**⚠️ 关键**：父类构造器中调用被子类重写的方法，子类成员变量尚未初始化！

**考点**：构造器中调用重写方法的危险
</details>

---

### 第10题
```java
public class Test {
    static {
        System.out.print("S");
    }
    {
        System.out.print("I");
    }
    Test() {
        System.out.print("T");
    }
    public static void main(String[] args) {
        System.out.print("M");
        new Test();
        new Test();
    }
}
```
**输出是什么？**

A. `S M I T I T`  
B. `S M I T T`  
C. `M S I T I T`  
D. `S M I T I T`

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
1. 类加载 → 静态块 `S`
2. `main` 方法执行 → `M`
3. 第一次 `new Test()` → 实例块 `I` → 构造器 `T`
4. 第二次 `new Test()` → 实例块 `I` → 构造器 `T`
5. 最终输出 `S M I T I T`

**考点**：静态块、main、实例块、构造器的综合顺序
</details>

---

### 第11题
```java
class A {
    void show() throws Exception {
        System.out.println("A");
    }
}

class B extends A {
    void show() {
        System.out.println("B");
    }
}

public class Test {
    public static void main(String[] args) {
        A a = new B();
        a.show();
    }
}
```
**输出是什么？**

A. `A`  
B. `B`  
C. 编译错误  
D. 运行时异常

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- 子类重写方法可以**不抛出**父类声明的异常（缩小异常范围）
- 所以 `B.show()` 没有 `throws` 是合法的
- `a.show()` 动态绑定 → 调用 `B.show()` → 输出 `B`

**考点**：重写方法的异常规则（子类可以不抛/缩小异常范围）
</details>

---

### 第12题
```java
class Parent {
    void method() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    void method() throws RuntimeException {
        System.out.println("Child");
    }
}
```
**这段代码会怎样？**

A. 正常编译，运行输出 `Child`  
B. 编译错误  
C. 运行时异常  
D. 正常运行但输出 `Parent`

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
- `RuntimeException` 是**非检查型异常**
- 重写方法可以抛出更小的异常，或者不抛出
- `RuntimeException` 不违反规则（可以抛出父类未声明的 RuntimeException）
- 正常编译运行

**考点**：重写方法可以抛出 RuntimeException（即使父类没声明）
</details>

---

### 第13题
```java
public class Test {
    public static void main(String[] args) {
        try {
            System.out.print("A");
            throw new Exception();
        } catch (Exception e) {
            System.out.print("B");
        } finally {
            System.out.print("C");
        }
        System.out.print("D");
    }
}
```
**输出是什么？**

A. `A B C D`  
B. `A B C`  
C. `A C D`  
D. `A B D`

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
- 先执行 `try` 块 → 输出 `A`
- 抛出异常 → 进入 `catch` → 输出 `B`
- `finally` 块**总是执行** → 输出 `C`
- 异常已处理，继续执行后面的代码 → 输出 `D`
- 最终 `A B C D`

**考点**：try-catch-finally 执行顺序
</details>

---

### 第14题
```java
public class Test {
    public static void main(String[] args) {
        try {
            System.out.print("A");
            System.exit(0);
        } finally {
            System.out.print("B");
        }
        System.out.print("C");
    }
}
```
**输出是什么？**

A. `A B C`  
B. `A B`  
C. `A`  
D. `A C`

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `System.exit(0)` 终止 JVM，**不执行 finally 块**
- 输出 `A`，然后程序终止
- `C` 不会输出（程序已终止）

**考点**：`System.exit()` 会跳过 finally 块
</details>

---

### 第15题
```java
try {
    int[] arr = new int[3];
    System.out.println(arr[5]);
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.print("A");
} catch (Exception e) {
    System.out.print("B");
} finally {
    System.out.print("C");
}
```
**输出是什么？**

A. `A C`  
B. `B C`  
C. `A B C`  
D. `C`

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
- `arr[5]` 抛出 `ArrayIndexOutOfBoundsException`
- 第一个 `catch` 精确匹配 → 输出 `A`
- 第二个 `catch` 不会执行
- `finally` 总是执行 → 输出 `C`
- 最终 `A C`

**考点**：多个 catch 块的精确匹配 + finally
</details>

---

### 第16题
```java
public class Test {
    public static void main(String[] args) {
        String s = null;
        try {
            System.out.println(s.length());
        } catch (NullPointerException e) {
            System.out.print("A");
        } catch (Exception e) {
            System.out.print("B");
        } finally {
            System.out.print("C");
        }
    }
}
```
**输出是什么？**

A. `A C`  
B. `B C`  
C. `A B C`  
D. `C`

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
- `s.length()` 抛出 `NullPointerException`
- 第一个 `catch` 精确匹配 → 输出 `A`
- finally 总是执行 → 输出 `C`
- 最终 `A C`

**考点**：精确匹配 catch + finally
</details>

---

### 第17题
```java
public class Test {
    public static void main(String[] args) {
        try {
            return;
        } finally {
            System.out.print("A");
        }
    }
}
```
**输出是什么？**

A. `A`  
B. 无输出  
C. 编译错误  
D. 运行时异常

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
- `return` 之前，`finally` 块**总是执行**
- 输出 `A`，然后方法返回
- 注意：即使 try 中有 return，finally 也会执行

**考点**：try 中有 return 时，finally 仍会执行
</details>

---

### 第18题
```java
public class Test {
    public static int test() {
        try {
            return 1;
        } finally {
            return 2;
        }
    }
    public static void main(String[] args) {
        System.out.println(test());
    }
}
```
**输出是什么？**

A. `1`  
B. `2`  
C. 编译错误  
D. 运行时异常

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- try 中 `return 1`，但 finally 中也有 `return 2`
- **finally 中的 return 会覆盖 try 中的 return**
- 输出 `2`

⚠️ **尽量避免在 finally 中使用 return**

**考点**：finally 中的 return 覆盖 try 中的 return
</details>

---

### 第19题
```java
public class Test {
    public static void main(String[] args) {
        String s1 = "Hello";
        String s2 = "Hello";
        String s3 = new String("Hello");
        
        System.out.print(s1 == s2);
        System.out.print(s1 == s3);
    }
}
```
**输出是什么？**

A. `true true`  
B. `true false`  
C. `false true`  
D. `false false`

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `"Hello"` 是字符串常量，存储在**字符串常量池**
- `s1` 和 `s2` 指向同一个常量 → `s1 == s2` → `true`
- `new String("Hello")` 在堆中创建新对象 → `s1 == s3` → `false`
- 输出 `true false`

**考点**：字符串常量池 vs new 对象
</details>

---

### 第20题
```java
public class Test {
    public static void main(String[] args) {
        String s1 = "Hello";
        String s2 = "He" + "llo";
        String s3 = "He";
        String s4 = s3 + "llo";
        
        System.out.print(s1 == s2);
        System.out.print(s1 == s4);
    }
}
```
**输出是什么？**

A. `true true`  
B. `true false`  
C. `false true`  
D. `false false`

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `"He" + "llo"`：编译期常量折叠，结果也是常量池中的 `"Hello"` → `s1 == s2` → `true`
- `s3 + "llo"`：涉及变量，运行期创建新字符串对象 → `s1 == s4` → `false`
- 输出 `true false`

**考点**：字符串常量折叠 vs 运行期拼接
</details>

---

## 答案速查表

| 题号 | 答案 | 核心考点 |
|------|------|----------|
| 1 | B | 向上转型 + 动态绑定 |
| 2 | A | 变量看类型，方法看对象 |
| 3 | A | 静态方法没有多态 |
| 4 | C | 构造器调用顺序（父→子） |
| 5 | B | 子类构造器先调用父类构造器 |
| 6 | A | this() 调用本类其他构造器 |
| 7 | C | 实例块先于构造器 |
| 8 | A | 静态块一次，实例块每次 |
| 9 | A | 父类构造器中调用重写方法的危险 |
| 10 | A | 静态块 → main → 实例块 → 构造器 |
| 11 | B | 重写方法可以不抛异常 |
| 12 | A | 重写可抛出 RuntimeException |
| 13 | A | try-catch-finally 顺序 |
| 14 | C | System.exit() 跳过 finally |
| 15 | A | 多个 catch 精确匹配 + finally |
| 16 | A | NullPointerException 匹配 |
| 17 | A | return 前 finally 仍执行 |
| 18 | B | finally 中的 return 覆盖 try |
| 19 | B | 字符串常量池 vs new |
| 20 | B | 常量折叠 vs 运行期拼接 |
