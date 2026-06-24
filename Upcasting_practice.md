# 向上转型 & 多态 · 选择题精练

> 覆盖向上转型、向下转型、多态、动态绑定等核心考点，每题只考察一个关键概念。

---

### 第1题
```java
class Animal {
    public void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    @Override
    public void sound() {
        System.out.println("Bark");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.sound();
    }
}
```
**输出是什么？**

A. `Animal sound`  
B. `Bark`  
C. 编译错误  
D. 运行时异常

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `Animal a = new Dog()` 是**向上转型**（子类对象赋给父类引用）
- 调用 `a.sound()` 时，虽然引用类型是 `Animal`，但实际对象是 `Dog`
- Java 采用**动态绑定**，调用的是实际对象的方法（`Dog` 的 `sound()`）
- 输出 `Bark`

**考点**：向上转型 + 动态绑定（多态）
</details>

---

### 第2题
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
**这段代码体现了什么概念？**

A. 封装  
B. 继承  
C. 多态  
D. 抽象

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- 父类引用 `p` 指向子类对象 `new Child()`
- 调用 `p.show()` 时，实际执行的是子类的 `show()`
- 同一个方法调用（`p.show()`）在不同情况下表现不同 → **多态**
- 具体是**多态中的动态绑定/运行时多态**

**考点**：多态的定义和识别
</details>

---

### 第3题
```java
class Animal { }

class Dog extends Animal { }

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog();
        Dog d = (Dog) a;
    }
}
```
**这段代码中 `(Dog) a` 是什么操作？**

A. 向上转型  
B. 向下转型  
C. 自动类型转换  
D. 类型推断

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `Animal a = new Dog()` 是向上转型（子类→父类）
- `(Dog) a` 是将父类引用**强制转换**回子类类型
- 这叫**向下转型（Downcasting）**
- 因为 `a` 实际指向的是 `Dog` 对象，所以转换安全

**考点**：向下转型的概念
</details>

---

### 第4题
```java
class A {
    void print() {
        System.out.println("A");
    }
}

class B extends A {
    void print() {
        System.out.println("B");
    }
}

public class Test {
    public static void main(String[] args) {
        A a = new A();
        A b = new B();
        a.print();
        b.print();
    }
}
```
**输出是什么？**

A. `A` `A`  
B. `A` `B`  
C. `B` `B`  
D. 编译错误

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `a` 指向 `A` 对象 → 调用 `A.print()` → 输出 `A`
- `b` 指向 `B` 对象（向上转型）→ 动态绑定 → 调用 `B.print()` → 输出 `B`

**考点**：向上转型后多态行为
</details>

---

### 第5题
```java
class Base {
    void method() {
        System.out.println("Base method");
    }
}

class Derived extends Base {
    void method() {
        System.out.println("Derived method");
    }
    void extra() {
        System.out.println("Extra");
    }
}

public class Test {
    public static void main(String[] args) {
        Base b = new Derived();
        b.method();
        b.extra();  // 这行会怎样？
    }
}
```

A. 输出 `Derived method` `Extra`  
B. 输出 `Base method` `Extra`  
C. 编译错误（`extra()` 不可见）  
D. 运行时异常

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `b` 的声明类型是 `Base`，编译时只检查 `Base` 类中是否有 `extra()`
- `Base` 类没有 `extra()` 方法 → **编译错误**
- 虽然 `b` 实际指向 `Derived` 对象，但编译器只看声明类型

**考点**：编译时类型（声明类型）vs 运行时类型（实际类型）
</details>

---

### 第6题
```java
class Animal {
    void eat() {
        System.out.println("Animal eat");
    }
}

class Dog extends Animal {
    void eat() {
        System.out.println("Dog eat");
    }
    void bark() {
        System.out.println("Bark");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog();
        a.bark();
    }
}
```
**结果是什么？**

A. 输出 `Bark`  
B. 输出 `Dog eat`  
C. 编译错误  
D. 运行时异常

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `a` 的声明类型是 `Animal`
- `Animal` 类中没有 `bark()` 方法
- 编译器检查声明类型，发现找不到 `bark()` → **编译错误**
- 这就是向上转型的代价：**只能调用父类中定义的方法**

**考点**：向上转型后方法的可见范围
</details>

---

### 第7题
```java
class Vehicle {
    void start() {
        System.out.println("Vehicle start");
    }
}

class Car extends Vehicle {
    void start() {
        System.out.println("Car start");
    }
    void honk() {
        System.out.println("Beep");
    }
}

public class Test {
    public static void main(String[] args) {
        Car c = new Car();
        Vehicle v = c;
        c.honk();
        v.honk();
    }
}
```
**关于这段代码，以下说法正确的是？**

A. `c.honk()` 编译错误  
B. `v.honk()` 编译错误  
C. 两者都输出 `Beep`  
D. 只有 `c.honk()` 输出 `Beep`

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `c.honk()`：`c` 是 `Car` 类型，有 `honk()` 方法 ✅
- `v.honk()`：`v` 是 `Vehicle` 类型，`Vehicle` 没有 `honk()` 方法 → ❌ 编译错误
- 即使 `v` 实际指向 `Car` 对象，编译器只看声明类型

**考点**：向上转型后只能使用父类方法
</details>

---

### 第8题
```java
class Parent {
    int x = 10;
    void show() {
        System.out.println("Parent show " + x);
    }
}

class Child extends Parent {
    int x = 20;
    void show() {
        System.out.println("Child show " + x);
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();
        System.out.println(p.x);
        p.show();
    }
}
```
**输出是什么？**

A. `10` `Child show 20`  
B. `20` `Child show 20`  
C. `10` `Parent show 10`  
D. `20` `Parent show 10`

<details>
<summary>点击查看答案与解析</summary>

**答案：A**

**解析**：
- **成员变量**：编译时决定，看声明类型 → `p.x` = `10`（`Parent` 的 x）
- **方法**：运行时决定，看实际对象 → `p.show()` 调用 `Child.show()` → 输出 `Child show 20`
- **关键**：变量没有多态，方法才有！

**考点**：成员变量 vs 方法的多态区别
</details>

---

### 第9题
```java
class A {
    void test() {
        System.out.println("A");
    }
}

class B extends A {
    void test() {
        System.out.println("B");
    }
    void test(int x) {
        System.out.println("B int");
    }
}

public class Test {
    public static void main(String[] args) {
        A a = new B();
        a.test(5);
    }
}
```
**结果是什么？**

A. 输出 `B int`  
B. 输出 `B`  
C. 编译错误  
D. 运行时异常

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `a` 的声明类型是 `A`
- `A` 类中只有 `test()`（无参），没有 `test(int)`
- `a.test(5)` 编译器在 `A` 中找不到匹配的方法 → **编译错误**
- 注意：`B` 中的 `test(int)` 不是重写（参数不同，是重载），且父类没有这个方法

**考点**：向上转型后只能调用父类中声明的方法（包括重载版本）
</details>

---

### 第10题
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
- `static` 方法：类级别的，**不属于对象**，没有动态绑定
- `a.type()` 调用的是 `Animal.type()`（声明类型决定）
- 非静态 `sound()`：有动态绑定，调用 `Dog.sound()` → `Bark`
- **关键**：静态方法没有多态！

**考点**：静态方法 vs 实例方法的多态差异
</details>

---

### 第11题
```java
interface Flyable {
    void fly();
}

class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("Bird flies");
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        System.out.println("Penguin swims");
    }
}

public class Test {
    public static void main(String[] args) {
        Flyable f = new Penguin();
        f.fly();
    }
}
```
**输出是什么？**

A. `Bird flies`  
B. `Penguin swims`  
C. 编译错误  
D. 运行时异常

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `Flyable f = new Penguin()` 是向上转型（实现类 → 接口）
- `f.fly()` 调用的是实际对象 `Penguin` 的 `fly()`
- 输出 `Penguin swims`
- 接口引用同样具有多态行为

**考点**：接口引用同样支持多态
</details>

---

### 第12题
```java
class Parent {
    void print() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    void print() {
        System.out.println("Child");
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p = new Parent();
        Child c = (Child) p;  // 这行会怎样？
    }
}
```
**结果是什么？**

A. 编译正常，运行正常  
B. 编译错误  
C. 编译正常，运行抛出 `ClassCastException`  
D. 编译正常，运行抛出 `NullPointerException`

<details>
<summary>点击查看答案与解析</summary>

**答案：C**

**解析**：
- `p` 实际指向的是 `Parent` 对象，不是 `Child`
- 强制向下转型 `(Child) p` 时，类型不匹配
- 抛出 `ClassCastException`
- **安全做法**：转型前用 `instanceof` 检查

```java
if (p instanceof Child) {
    Child c = (Child) p;  // 安全
}
```

**考点**：不安全的向下转型 → `ClassCastException`
</details>

---

### 第13题
```java
class Fruit {
    void taste() {
        System.out.println("Fruit taste");
    }
}

class Apple extends Fruit {
    void taste() {
        System.out.println("Apple taste");
    }
    void peel() {
        System.out.println("Peeling apple");
    }
}

public class Test {
    public static void main(String[] args) {
        Fruit f = new Apple();
        if (f instanceof Apple) {
            ((Apple) f).peel();
        }
        f.taste();
    }
}
```
**输出是什么？**

A. `Peeling apple` `Fruit taste`  
B. `Peeling apple` `Apple taste`  
C. 编译错误  
D. 运行时异常

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `f instanceof Apple` 为 `true`（`f` 指向 Apple 对象）
- 安全向下转型后调用 `peel()` → `Peeling apple`
- `f.taste()` 动态绑定 → 调用 `Apple.taste()` → `Apple taste`

**考点**：`instanceof` + 安全向下转型 + 多态
</details>

---

### 第14题
```java
class Animal {
    String name = "Animal";
    String getName() {
        return name;
    }
}

class Dog extends Animal {
    String name = "Dog";
    String getName() {
        return name;
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Dog();
        System.out.println(a.name);
        System.out.println(a.getName());
    }
}
```
**输出是什么？**

A. `Animal` `Animal`  
B. `Animal` `Dog`  
C. `Dog` `Animal`  
D. `Dog` `Dog`

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `a.name`：成员变量，没有多态，看声明类型 → `Animal` → `Animal`
- `a.getName()`：方法调用，有多态，看实际对象 → `Dog.getName()` → `Dog`
- 这再次验证：**变量看类型，方法看对象**

**考点**：变量 vs 方法的多态差异（强化）
</details>

---

### 第15题
```java
class Shape {
    void draw() {
        System.out.println("Shape");
    }
}

class Circle extends Shape {
    void draw() {
        System.out.println("Circle");
    }
}

class Square extends Shape {
    void draw() {
        System.out.println("Square");
    }
}

public class Test {
    public static void main(String[] args) {
        Shape[] shapes = {new Circle(), new Square()};
        for (Shape s : shapes) {
            s.draw();
        }
    }
}
```
**输出是什么？**

A. `Shape` `Shape`  
B. `Circle` `Square`  
C. 编译错误  
D. `Shape` `Square`

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- 数组声明为 `Shape[]`，但存放的是 `Circle` 和 `Square` 对象
- 循环中 `s.draw()` 根据**实际对象类型**调用对应方法
- `Circle` 输出 `Circle`，`Square` 输出 `Square`
- 这是多态的最经典应用：**同一操作在不同对象上有不同表现**

**考点**：多态的实际应用场景
</details>

---

### 第16题
```java
class Parent {
    void method() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    void method() {
        System.out.println("Child");
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();
        Child c = (Child) p;
        c.method();
        p.method();
    }
}
```
**输出是什么？**

A. `Child` `Parent`  
B. `Child` `Child`  
C. `Parent` `Parent`  
D. 运行时异常

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `p` 实际指向 `Child` 对象
- `c` 是向下转型后的引用，也指向同一个 `Child` 对象
- 两次调用 `method()` 都是同一个对象的方法 → 都输出 `Child`

**考点**：向下转型不改变实际对象，只是改变引用类型
</details>

---

### 第17题
```java
class A {
    void show() {
        System.out.println("A");
    }
}

class B extends A {
    void show() {
        System.out.println("B");
    }
    void display() {
        System.out.println("B display");
    }
}

public class Test {
    public static void main(String[] args) {
        B b = new B();
        A a = b;
        
        a.show();
        // a.display();  // 取消注释会发生什么？
    }
}
```
**若取消 `a.display()` 的注释，会发生什么？**

A. 输出 `B display`  
B. 编译错误  
C. 运行时异常  
D. 输出 `A display`

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `a` 的声明类型是 `A`
- `A` 类中没有 `display()` 方法
- 取消注释后 → **编译错误**
- 虽然 `a` 实际指向 `B` 对象，但编译器只看声明类型

**考点**：向上转型后无法调用子类独有方法（编译时检查）
</details>

---

### 第18题
```java
class Animal {
    void move() {
        System.out.println("Animal move");
    }
}

class Bird extends Animal {
    void move() {
        System.out.println("Fly");
    }
    void move(String direction) {
        System.out.println("Fly to " + direction);
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Bird();
        a.move("north");  // 这行会怎样？
    }
}
```
**结果是什么？**

A. 输出 `Fly to north`  
B. 编译错误  
C. 运行时异常  
D. 输出 `Animal move`

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `a` 的声明类型是 `Animal`
- `Animal` 中只有 `move()`（无参），没有 `move(String)`
- 编译器在 `Animal` 中找不到匹配的方法 → **编译错误**
- 注意：`Bird` 中的 `move(String)` 不是重写，是重载，且父类没有这个方法

**考点**：向上转型后只能调用父类中声明的方法（包括重载版本）
</details>

---

### 第19题
```java
class Parent {
    void method() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    void method() {
        System.out.println("Child");
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p = null;
        p = new Child();
        p.method();
    }
}
```
**输出是什么？**

A. `Parent`  
B. `Child`  
C. 编译错误  
D. 运行时异常（`NullPointerException`）

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `Parent p = null;` 只是声明引用，不指向任何对象
- `p = new Child();` 将 `p` 指向一个新的 `Child` 对象
- `p.method()` 调用时，`p` 已经不可能是 `null`
- 动态绑定 → 调用 `Child.method()` → 输出 `Child`
- **注意**：`null` 引用在赋值前存在，但调用方法发生在赋值之后

**考点**：引用重新赋值后的多态行为
</details>

---

### 第20题
```java
class Grandparent {
    void show() {
        System.out.println("Grandparent");
    }
}

class Parent extends Grandparent {
    void show() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    void show() {
        System.out.println("Child");
    }
}

public class Test {
    public static void main(String[] args) {
        Grandparent g = new Child();
        Parent p = (Parent) g;
        g.show();
        p.show();
    }
}
```
**输出是什么？**

A. `Grandparent` `Parent`  
B. `Child` `Child`  
C. `Grandparent` `Child`  
D. `Child` `Parent`

<details>
<summary>点击查看答案与解析</summary>

**答案：B**

**解析**：
- `g` 实际指向 `Child` 对象（向上转型到 `Grandparent`）
- `(Parent) g` 向下转型到 `Parent`，但实际对象仍是 `Child`
- `g.show()` 动态绑定 → `Child.show()` → `Child`
- `p.show()` 同样指向同一个 `Child` 对象 → `Child`

**考点**：多层继承中的向上转型和动态绑定
</details>

---

## 答案速查表

| 题号 | 答案 | 核心考点 |
|------|------|----------|
| 1 | B | 向上转型 + 动态绑定 |
| 2 | C | 多态的概念 |
| 3 | B | 向下转型 |
| 4 | B | 父类引用指向子类对象 |
| 5 | C | 向上转型后只能调用父类方法 |
| 6 | C | 向上转型后不可见子类独有方法 |
| 7 | B | 向上转型后方法可见范围 |
| 8 | A | 变量看声明类型，方法看实际对象 |
| 9 | C | 向上转型后调用重载方法 |
| 10 | A | 静态方法没有多态 |
| 11 | B | 接口引用的多态 |
| 12 | C | 不安全的向下转型 → ClassCastException |
| 13 | B | instanceof + 安全向下转型 |
| 14 | B | 变量 vs 方法的多态差异 |
| 15 | B | 多态的实际应用 |
| 16 | B | 向下转型不改变实际对象 |
| 17 | B | 向上转型后无法调用子类独有方法 |
| 18 | B | 向上转型后无法调用重载方法 |
| 19 | B | 引用重新赋值后的多态 |
| 20 | B | 多层继承中的动态绑定 |

---

## 核心知识点速记

```
┌─────────────────────────────────────────────────────────────┐
│  向上转型 & 多态 · 核心要点                                 │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. 向上转型：子类对象 → 父类引用（自动）                  │
│     Animal a = new Dog();   ✅ 安全                        │
│                                                             │
│  2. 向下转型：父类引用 → 子类引用（强制）                  │
│     Dog d = (Dog) a;        ⚠️ 可能 ClassCastException    │
│                                                             │
│  3. 动态绑定：方法调用看实际对象（运行时）                 │
│     a.method() → 调用 Dog 的 method()                      │
│                                                             │
│  4. 静态绑定：变量访问看声明类型（编译时）                 │
│     a.field   → 访问 Animal 的 field                      │
│                                                             │
│  5. 静态方法：没有多态，看声明类型                         │
│     a.staticMethod() → 调用 Animal 的                      │
│                                                             │
│  6. 向上转型后：只能调用父类中声明的方法                   │
│     不能调用子类独有的方法/字段                            │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```
