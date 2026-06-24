# Java OOP 期末复习 · 选择题精练

> 以下题目覆盖异常处理、I/O流、文件操作、装饰器模式等核心考点，每题只考察一个关键概念，无重复考点。

---

## 异常处理（Exception Handling）

### 第1题
```java
try {
    int[] arr = new int[3];
    System.out.println(arr[5]);
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("A");
} catch (Exception e) {
    System.out.println("B");
} finally {
    System.out.println("C");
}
```
**输出是什么？**

A. A  
B. B C  
C. A C  
D. A B C

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `arr[5]` 抛出 `ArrayIndexOutOfBoundsException`
- 第一个 `catch` 精确匹配，执行 `System.out.println("A")`
- `finally` 块**无论是否发生异常都会执行**，输出 `C`
- 第二个 `catch`（Exception）不会执行，因为异常已被前面的 catch 捕获
- 最终输出 A C

**考点**：`try-catch-finally` 执行顺序 + 多个 catch 块的匹配规则（精确匹配优先）
</details>

---

### 第2题
```java
public class Test {
    public static void main(String[] args) {
        try {
            method();
        } catch (Exception e) {
            System.out.println("Caught");
        }
    }
    
    static void method() throws Exception {
        throw new Exception();
    }
}
```
**关于 `throws` 和 `throw` 的描述，以下哪个是正确的？**

A. `throws` 用于实际抛出异常对象  
B. `throw` 用于方法声明中说明可能抛出的异常  
C. `throw` 用于实际抛出异常对象，`throws` 用于方法声明  
D. 两者可以互换使用

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `throw` 在方法体内部使用，**实际抛出**一个异常对象
- `throws` 在方法签名中使用，**声明**该方法可能抛出哪些异常
- 两者功能不同，不能互换

**考点**：`throw` 与 `throws` 的区别
</details>

---

### 第3题
```java
class MyException extends Exception {}
```
**这段代码定义了什么？**

A. 一个普通类，不是异常  
B. 一个运行时异常  
C. 一个检查型异常（Checked Exception）  
D. 一个错误（Error）

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- 继承 `Exception` 类定义的异常是**检查型异常（Checked Exception）**
- 编译器强制要求处理（try-catch 或 throws）
- 继承 `RuntimeException` 才是运行时异常（Unchecked Exception）
- 继承 `Error` 才是错误

**考点**：异常类继承体系（Exception vs RuntimeException vs Error）
</details>

---

## I/O 流基础（I/O Stream Basics）

### 第4题
**以下哪个是字节输出流的最顶层抽象类？**

A. `OutputStream`  
B. `FileOutputStream`  
C. `BufferedOutputStream`  
D. `DataOutputStream`

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
- `OutputStream` 是所有**字节输出流**的父类（抽象类）
- `FileOutputStream`、`BufferedOutputStream`、`DataOutputStream` 都是其子类
- 同理，`InputStream` 是所有字节输入流的父类

**考点**：I/O 流的继承层次（顶层抽象类）
</details>

---

### 第5题
**`InputStream` 的 `read()` 方法在读取到文件末尾时返回什么？**

A. `0`  
B. `null`  
C. `-1`  
D. 抛出异常

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `InputStream.read()` 读取一个字节，返回 `0~255` 的整数
- 读到文件末尾（EOF）时返回 `-1`
- `read(byte[] b)` 也返回实际读取的字节数，末尾返回 `-1`

**考点**：`read()` 方法的返回值约定
</details>

---

### 第6题
**以下哪个类用于从文件中读取字节数据？**

A. `FileReader`  
B. `FileInputStream`  
C. `BufferedReader`  
D. `FileWriter`

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `FileInputStream`：从文件读取**字节**（字节流）
- `FileReader`：从文件读取**字符**（字符流）
- `BufferedReader`：带缓冲的字符输入流（需要包装其他 Reader）
- `FileWriter`：向文件写入字符（字符流）

**考点**：字节流 vs 字符流 + 各类的功能定位
</details>

---

## 缓冲流与装饰器（Buffered Streams & Decorator）

### 第7题
**以下关于 `BufferedInputStream` 的描述，哪个是正确的？**

A. 它只能包装 `FileInputStream`  
B. 它通过继承 `InputStream` 的各个子类来实现缓冲  
C. 它通过包含一个 `InputStream` 对象来实现缓冲  
D. 它每次只读取一个字节，没有任何优化

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `BufferedInputStream` 内部包含一个 `InputStream` 类型的成员变量
- 通过**组合（has-a）** 方式，而不是继承多个子类来实现缓冲
- 这样可以包装任何 `InputStream` 子类（FileInputStream、ByteArrayInputStream 等）
- 它一次读取多个字节到内部缓冲区，提高效率

**考点**：装饰器模式的实现方式（组合优于继承）
</details>

---

### 第8题
```java
DataInputStream din = new DataInputStream(
    new BufferedInputStream(
        new FileInputStream("data.txt")
    )
);
```
**这段代码使用了什么设计模式？**

A. 工厂模式（Factory Pattern）  
B. 单例模式（Singleton Pattern）  
C. 装饰器模式（Decorator Pattern）  
D. 观察者模式（Observer Pattern）

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- 通过多层包装，为 `FileInputStream` 叠加缓冲功能（`BufferedInputStream`）和数据类型读取功能（`DataInputStream`）
- 可以在运行时动态组合功能
- 这是**装饰器模式（Decorator Pattern）** 的典型应用

**考点**：装饰器模式的识别（嵌套包装）
</details>

---

### 第9题
**以下哪个类不属于 `FilterInputStream` 的子类？**

A. `BufferedInputStream`  
B. `DataInputStream`  
C. `FileInputStream`  
D. 以上都是

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `FilterInputStream` 的子类包括：`BufferedInputStream`、`DataInputStream`、`PushbackInputStream` 等
- `FileInputStream` 直接继承自 `InputStream`，**不是** `FilterInputStream` 的子类
- `FilterInputStream` 是装饰器模式的抽象基类，而 `FileInputStream` 是具体组件（ConcreteComponent）

**考点**：I/O 类的继承层次（哪些是装饰器，哪些是具体组件）
</details>

---

## 文件操作（Path / Files）

### 第10题
**`Path` 接口在以下哪个包中？**

A. `java.io`  
B. `java.util`  
C. `java.nio.file`  
D. `java.net`

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `Path` 接口位于 `java.nio.file` 包（Java 7+）
- `java.io` 包包含旧的 `File` 类
- `Files` 类也位于 `java.nio.file` 包

**考点**：新 I/O API（NIO）的包名
</details>

---

### 第11题
**以下哪个方法可以判断文件是否存在？**

A. `Files.exists(path)`  
B. `Files.isExist(path)`  
C. `Files.checkExists(path)`  
D. `path.isExists()`

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
- 正确写法：`Files.exists(Path)` 返回 `boolean`
- `Files` 类包含大量静态方法用于文件操作
- `Path` 接口本身不包含 `isExists()` 方法

**考点**：`Files` 类的静态方法使用
</details>

---

### 第12题
**`Files.delete(path)` 在删除一个非空目录时会抛出什么异常？**

A. `NoSuchFileException`  
B. `DirectoryNotEmptyException`  
C. `IOException`  
D. `NullPointerException`

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- 删除非空目录 → `DirectoryNotEmptyException`
- 文件不存在 → `NoSuchFileException`
- 权限问题等其他 I/O 错误 → `IOException`

**考点**：`Files.delete()` 的异常类型
</details>

---

## 字符流（Reader / Writer）

### 第13题
**`Reader` 和 `InputStream` 的根本区别是什么？**

A. `Reader` 有缓冲，`InputStream` 没有  
B. `Reader` 读取字符，`InputStream` 读取字节  
C. `Reader` 只能读文件，`InputStream` 可以读任何数据源  
D. `Reader` 是接口，`InputStream` 是类

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `Reader` / `Writer` 处理**字符**（Unicode，每次 16 位或 32 位）
- `InputStream` / `OutputStream` 处理**字节**（每次 8 位）
- 两者都可以有缓冲（通过装饰器 `BufferedReader` / `BufferedInputStream`）
- 两者都可以处理不同的数据源

**考点**：字节流与字符流的本质区别
</details>

---

### 第14题
**`System.in` 的类型是什么？**

A. `BufferedReader`  
B. `InputStreamReader`  
C. `InputStream`  
D. `Scanner`

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `System.in` 是**字节输入流**（`InputStream`），对应标准输入（键盘）
- `System.out` 和 `System.err` 是 `PrintStream`（字节输出流）
- 要读取字符或行，需要包装：
  ```java
  BufferedReader stdin = new BufferedReader(new InputStreamReader(System.in));
  ```

**考点**：标准 I/O 流的类型
</details>

---

### 第15题
**以下哪个类不能直接包装 `FileInputStream`？**

A. `BufferedInputStream`  
B. `DataInputStream`  
C. `InputStreamReader`  
D. `PrintStream`

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `InputStreamReader` 是**字节流到字符流的转换器**，需要包装 `InputStream`
- 但它本身的构造函数是 `InputStreamReader(InputStream in)`，所以**可以**包装 `FileInputStream`
- **等一下，题目问的是不能直接包装的，让我重新想一下**
- 实际上：所有四个都可以包装 `FileInputStream`：
  - `BufferedInputStream`：字节 → 字节（缓冲）
  - `DataInputStream`：字节 → 字节（读基本类型）
  - `InputStreamReader`：字节 → 字符（转换）
  - `PrintStream`：字节 → 字节（格式化输出）

**更正：这道题设计有误，换一道**

---

### 第15题（替换）
**以下哪个装饰器可以同时提供缓冲和格式化输出功能？**

A. `BufferedOutputStream`  
B. `PrintStream`  
C. `DataOutputStream`  
D. `FileOutputStream`

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `PrintStream` 提供格式化输出方法：`print()`、`println()`、`printf()`
- `BufferedOutputStream` 只提供缓冲功能
- `DataOutputStream` 提供基本类型写入：`writeInt()`、`writeDouble()` 等
- `FileOutputStream` 是具体组件，不提供任何装饰功能
- 注意：`PrintStream` **内部自带缓冲**（与 `BufferedOutputStream` 类似），同时提供格式化输出

**考点**：各装饰器的功能区分
</details>

---

## 综合题

### 第16题
**关于 `BufferedReader` 的 `readLine()` 方法，以下哪个描述是错误的？**

A. 读取一行文本，返回字符串  
B. 读取到文件末尾返回 `null`  
C. 返回的字符串包含换行符  
D. 可能抛出 `IOException`

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `readLine()` 返回的字符串**不包含**行终止符（`\n` 或 `\r\n`）
- 读到末尾返回 `null`
- 需要处理 `IOException`
- 如果一行是空字符串（用户直接按回车），返回 `""`（不是 `null`）

**考点**：`readLine()` 的行为细节
</details>

---

### 第17题
```java
String s;
while ((s = stdin.readLine()) != null && s.length() != 0) {
    System.out.println(s);
}
```
**这个循环在什么情况下会退出？**

1. 用户输入 `null`  
2. 用户按 Ctrl+D（Linux）或 Ctrl+Z（Windows）  
3. 用户直接按回车（空行）  
4. 用户输入 `"exit"`

A. 只有 1  
B. 2 和 3  
C. 2、3 和 4  
D. 1、2 和 3

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- 条件1：用户不能输入 `null`，`null` 是 EOF 的标志，对应按 Ctrl+D/Z → 条件 `!= null` 为 false → **退出**（情况2）
- 条件2：按 Ctrl+D/Z → `readLine()` 返回 `null` → 条件1为 false → **退出**（短路，不执行条件2）
- 条件3：直接按回车 → 返回 `""` → 条件1为 true，条件2 `s.length() != 0` 为 false → **退出**
- 条件4：输入 `"exit"` → 条件1和2都为 true → **继续循环**，不会自动退出

**考点**：循环条件的逻辑判断 + `readLine()` 对不同输入的处理
</details>

---

### 第18题
**以下哪组装饰器的顺序是正确的（功能可以正常工作）？**

A. `new DataOutputStream(new FileOutputStream("file.txt"))`  
B. `new BufferedOutputStream(new DataOutputStream(new FileOutputStream("file.txt")))`  
C. `new DataOutputStream(new BufferedOutputStream(new FileOutputStream("file.txt")))`  
D. A 和 C 都正确

<details>
<summary>点击查看答案与解析</summary>

**答案：D**

**解析**：
- A：`DataOutputStream` 包装 `FileOutputStream` ✅（DataOutputStream 需要 OutputStream 参数）
- B：`BufferedOutputStream` 包装 `DataOutputStream` ❌（BufferedOutputStream 的构造函数接受 `OutputStream`，但 DataOutputStream 也是 OutputStream 子类，语法上可行，但 DataOutputStream 的方法可能丢失缓冲效果，顺序不合理）
- C：`DataOutputStream` 包装 `BufferedOutputStream` ✅（先加缓冲，再添加数据类型写入功能，推荐顺序）

**正确顺序**：基础流 → 缓冲流 → 功能流
```
FileOutputStream → BufferedOutputStream → DataOutputStream
     (基础)    →      (缓冲)        →     (读基本类型)
```

**考点**：装饰器的正确包装顺序（从内到外：基础 → 缓冲 → 功能）
</details>

---

## 答案速查表

| 题号 | 答案 | 考点 |
|------|------|------|
| 1 | C | try-catch-finally 执行顺序 |
| 2 | C | throw vs throws |
| 3 | C | 检查型异常 vs 运行时异常 |
| 4 | A | 字节流顶层抽象类 |
| 5 | C | read() 返回 -1 表示 EOF |
| 6 | B | 文件字节输入流 |
| 7 | C | 装饰器模式：组合（has-a）|
| 8 | C | 装饰器模式识别 |
| 9 | C | FilterInputStream 的子类 |
| 10 | C | Path 接口的包名 |
| 11 | A | Files.exists() 方法 |
| 12 | B | 删除非空目录的异常 |
| 13 | B | 字节流 vs 字符流 |
| 14 | C | System.in 的类型 |
| 15 | B | PrintStream 的功能 |
| 16 | C | readLine() 不包含换行符 |
| 17 | B | 循环退出条件 |
| 18 | D | 装饰器正确顺序 |

---

祝考试顺利！如果需要更多练习题或对某道题有疑问，随时告诉我。
