# 📄 Day 01 — Variables, Data Types, Type Casting & Operators

<div align="center">

![Day](https://img.shields.io/badge/Day-01_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Variables_%26_Operators-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 02 →](./Day02_Conditionals.md)

</div>

---

## 🎯 Day 1 Goal
> Understand how Java works, set up the development environment, master variables, data types, type casting, and all operators — then **write code**, not just read it.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | Java Intro + Setup + First Program | 60 min |
| Block 2 | Variables + Data Types + Type Casting | 60 min |
| Block 3 | Operators — All Types | 60 min |
| Block 4 | Practice Set — 6 Problems | 90 min |
| Block 5 | Revision + Mini Challenge + Reflection | 30 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. How Java Works

Java is a **compiled + interpreted** language — this is what makes it platform-independent.

```
Your Code (.java)
      ↓  javac  (Java Compiler)
  Bytecode (.class)
      ↓  JVM  (Java Virtual Machine)
   Output on ANY Machine
```

| Term | Full Form | Role |
|------|-----------|------|
| **JDK** | Java Development Kit | Write and compile Java code |
| **JRE** | Java Runtime Environment | Run compiled Java programs |
| **JVM** | Java Virtual Machine | Converts bytecode → machine-specific code |
| **Bytecode** | — | Platform-neutral `.class` file (intermediate code) |

> 💡 **Key Insight:** "Write Once, Run Anywhere" — JVM is platform-specific, but bytecode is universal. The same `.class` file runs on Windows, Mac, and Linux.

---

### 🔷 2. First Java Program

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

**Every keyword broken down:**

| Keyword | Meaning |
|---------|---------|
| `public` | Access modifier — anyone can access this |
| `class Main` | Every Java program lives inside a class |
| `static` | Belongs to the class itself, not an object |
| `void` | This method returns no value |
| `main` | Entry point — JVM begins execution here |
| `String[] args` | Array for command-line arguments |
| `System.out.println` | Prints to console WITH a new line |
| `System.out.print` | Prints to console WITHOUT a new line |

---

### 🔷 3. Variables & Data Types

A **variable** is a named memory container that stores a value.

```java
// Syntax: dataType variableName = value;
int     age    = 20;
double  price  = 99.99;
char    grade  = 'A';
boolean isPass = true;
String  name   = "Rahul";   // Non-primitive (Object)
```

---

### 🔷 4. Primitive Data Types — Complete Table

| Type | Size | Default | Range | Example |
|------|------|---------|-------|---------|
| `byte` | 1 byte | 0 | -128 to 127 | `byte b = 100;` |
| `short` | 2 bytes | 0 | -32,768 to 32,767 | `short s = 1000;` |
| `int` | 4 bytes | 0 | -2,147,483,648 to 2,147,483,647 | `int x = 500;` |
| `long` | 8 bytes | 0L | Very large | `long l = 99999L;` |
| `float` | 4 bytes | 0.0f | 6–7 decimal digits | `float f = 3.14f;` |
| `double` | 8 bytes | 0.0d | 15 decimal digits | `double d = 3.14159;` |
| `char` | 2 bytes | '\u0000' | Single character | `char c = 'A';` |
| `boolean` | 1 bit | false | true / false | `boolean b = true;` |

**Primitive vs Non-Primitive:**

| Feature | Primitive | Non-Primitive |
|---------|-----------|---------------|
| Stores | Actual value | Memory reference (address) |
| Memory | Stack | Heap |
| Examples | `int`, `char`, `boolean` | `String`, Arrays, Classes |
| Default | 0 / false | `null` |

---

### 🔷 5. Type Casting

Converting a value from one data type to another.

**Widening — Automatic (small → big, NO data loss)**
```java
int    x = 100;
long   l = x;     // int → long   ✅ automatic
double d = x;     // int → double ✅ automatic

// Safe widening order:
// byte → short → int → long → float → double
```

**Narrowing — Manual (big → small, possible data loss)**
```java
double d = 9.99;
int    x = (int) d;   // x = 9  ← decimal TRUNCATED (NOT rounded!)

float  f = 3.99f;
int    i = (int) f;   // i = 3  ← always truncates, never rounds
```

> ⚠️ **Critical Rule:** Narrowing TRUNCATES. `(int) 3.9 = 3`, not `4`.

**Most Common Bug — Integer Division:**
```java
int a = 7, b = 2;
double result1 = a / b;            // → 3.0  ❌ (int division done FIRST, then stored)
double result2 = (double) a / b;   // → 3.5  ✅ (cast BEFORE dividing)
```

---

### 🔷 6. Operators

**A. Arithmetic Operators**
```java
int a = 10, b = 3;
System.out.println(a + b);   // 13  — Addition
System.out.println(a - b);   // 7   — Subtraction
System.out.println(a * b);   // 30  — Multiplication
System.out.println(a / b);   // 3   — Integer division (decimal dropped!)
System.out.println(a % b);   // 1   — Remainder (Modulo)
```

**B. Relational Operators — Always returns boolean**
```java
System.out.println(10 > 3);    // true
System.out.println(10 < 3);    // false
System.out.println(10 >= 10);  // true
System.out.println(10 <= 9);   // false
System.out.println(10 == 10);  // true
System.out.println(10 != 5);   // true
```
> ⚠️ `=` is **assignment**. `==` is **comparison**. Never confuse!

**C. Logical Operators**
```java
System.out.println(true && true);    // true  — AND: both must be true
System.out.println(true && false);   // false
System.out.println(true || false);   // true  — OR: at least one true
System.out.println(false || false);  // false
System.out.println(!true);           // false — NOT: reverses
```

**D. Increment & Decrement**
```java
int x = 5;
System.out.println(x++);   // prints 5, THEN x becomes 6  (post-increment)
System.out.println(++x);   // x becomes 7, THEN prints 7  (pre-increment)
System.out.println(x--);   // prints 7, THEN x becomes 6  (post-decrement)
System.out.println(--x);   // x becomes 5, THEN prints 5  (pre-decrement)
```

**E. Assignment Operators**
```java
int x = 10;
x += 5;   // x = 15
x -= 3;   // x = 12
x *= 2;   // x = 24
x /= 4;   // x = 6
x %= 4;   // x = 2
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Hello Programmer `[Easy]`

**Task:** Store name and age in variables and print a formatted sentence.

```java
public class P1_HelloProgrammer {
    public static void main(String[] args) {
        String name = "Rahul";
        int age = 20;
        System.out.println("My name is " + name + " and I am " + age + " years old.");
    }
}
```

```
Output:
My name is Rahul and I am 20 years old.
```

> 🔑 `+` concatenates String with int — Java auto-converts `int` to String when joining with `+`.

---

### ✅ Problem 2 — Simple Calculator `[Easy]`

**Task:** Perform all arithmetic operations on `a = 17`, `b = 5`.

```java
public class P2_SimpleCalculator {
    public static void main(String[] args) {
        int a = 17, b = 5;
        System.out.println("Addition       : " + (a + b));
        System.out.println("Subtraction    : " + (a - b));
        System.out.println("Multiplication : " + (a * b));
        System.out.println("Division       : " + (a / b));
        System.out.println("Remainder      : " + (a % b));
        System.out.println("Decimal Div    : " + ((double) a / b));
    }
}
```

```
Output:
Addition       : 22
Subtraction    : 12
Multiplication : 85
Division       : 3
Remainder      : 2
Decimal Div    : 3.4
```

> 🔑 `17/5 = 3` due to integer division. Cast one operand to `double` to get `3.4`.

---

### ✅ Problem 3 — Temperature Converter `[Easy-Medium]`

**Task:** Convert Celsius to Fahrenheit. Formula: `F = (C × 9/5) + 32`

```java
public class P3_TemperatureConverter {
    public static void main(String[] args) {
        double celsius = 37.0;
        double fahrenheit = (celsius * 9 / 5) + 32;
        System.out.println(celsius + "°C = " + fahrenheit + "°F");
    }
}
```

```
Output:
37.0°C = 98.6°F
```

> 🔑 Using `double celsius` ensures `celsius * 9` is floating-point — prevents `9/5` becoming `1`.

---

### ✅ Problem 4 — Swap Without Third Variable `[Medium]`

**Task:** Swap `a = 25` and `b = 40` using only arithmetic.

```java
public class P4_SwapNumbers {
    public static void main(String[] args) {
        int a = 25, b = 40;
        System.out.println("Before: a = " + a + ", b = " + b);
        a = a + b;   // a = 65
        b = a - b;   // b = 25
        a = a - b;   // a = 40
        System.out.println("After : a = " + a + ", b = " + b);
    }
}
```

```
Output:
Before: a = 25, b = 40
After : a = 40, b = 25
```

> 🔑 Store the sum in `a`, recover each original value by subtraction. Zero extra memory.

---

### ✅ Problem 5 — Type Casting Detective `[Medium]`

**Task:** Predict and explain the output difference.

```java
public class P5_TypeCastingDetective {
    public static void main(String[] args) {
        int a = 7, b = 2;
        double result1 = a / b;            // 3.0 — int division FIRST, then stored
        double result2 = (double) a / b;   // 3.5 — cast FIRST, then division

        System.out.println("result1 = " + result1);   // 3.0
        System.out.println("result2 = " + result2);   // 3.5
    }
}
```

```
Output:
result1 = 3.0
result2 = 3.5
```

> 🔑 `a / b` evaluates as integers (= 3) THEN stores as 3.0. Casting `(double) a` before dividing changes the operation to floating-point math.

---

### ✅ Problem 6 — Even or Odd Without if-else `[Medium]`

**Task:** Check even/odd using only modulo and a boolean.

```java
public class P6_EvenOdd {
    public static void main(String[] args) {
        int n = 29;
        boolean isEven = (n % 2 == 0);
        System.out.println(n + " is even: " + isEven);
    }
}
```

```
Output:
29 is even: false
```

> 🔑 `n % 2 == 0` is itself a boolean expression — evaluates directly to `true` or `false`. Modulo is the go-to tool for even/odd, divisibility, digit extraction, and circular indexing.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Shopkeeper sells apples at ₹15 each. Customer buys 47 apples, pays ₹1000.

```java
public class MiniChallenge_Day1 {
    public static void main(String[] args) {
        int pricePerApple = 15;
        int applesBought  = 47;
        int amountPaid    = 1000;

        int totalCost = pricePerApple * applesBought;   // 705
        int change    = amountPaid - totalCost;          // 295

        System.out.println("Total: ₹" + totalCost + " | Change: ₹" + change);
    }
}
```

```
Output:
Total: ₹705 | Change: ₹295
```

> 🔑 Real-world problems map directly to variables + operators. Break the problem into math steps first.

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Status |
|------|--------|
| Re-wrote Hello World from memory | ✅ |
| Listed all 8 primitive types with sizes from memory | ✅ |
| Explained type casting in own words without notes | ✅ |
| Re-coded all operator examples without looking | ✅ |

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. Difference between float and double?</b></summary>

> `float` = 4 bytes, 6–7 decimal digits, needs `f` suffix (`3.14f`).
> `double` = 8 bytes, 15 decimal digits, default decimal type in Java.
> Use `double` for most calculations. Use `float` only when memory is tightly constrained.

</details>

<details>
<summary><b>Q2. Difference between = and == ?</b></summary>

> `=` is the **assignment operator** — stores a value into a variable. Example: `int x = 5;`
> `==` is the **equality comparison operator** — compares two values, returns boolean. Example: `x == 5` → `true`
> Confusing them is one of the most common beginner bugs.

</details>

<details>
<summary><b>Q3. Why does 10/3 give 3? How to fix it?</b></summary>

> Both operands are `int` → Java performs integer division (decimal truncated, not rounded).
> Fix: cast one operand to `double` before dividing:
> `(double)10 / 3` → `3.3333` ✅ &nbsp;&nbsp; `10.0 / 3` → `3.3333` ✅

</details>

<details>
<summary><b>Q4. What is Type Casting? Real-life analogy?</b></summary>

> Converting a value from one data type to another.
> **Widening (auto):** Pouring water from a cup into a bucket — safe, no overflow.
> **Narrowing (manual):** Pouring from a bucket into a cup — overflows, loses data.
> Analogy: Saving a 4K video as 360p — smaller file but detail is permanently lost.

</details>

<details>
<summary><b>Q5. What is % useful for?</b></summary>

> `%` gives the **remainder** after division.
> Even/odd: `n % 2 == 0`, Divisibility: `n % 5 == 0`, Last digit: `n % 10`,
> Circular indexing in arrays: `index % size`.

</details>

---

## 🛠️ SETUP — ✅ Completed

| Task | Status |
|------|--------|
| JDK 17+ downloaded and installed | ✅ |
| IDE setup — IntelliJ IDEA Community Edition | ✅ |
| `java -version` verified in terminal | ✅ |
| `javac -version` verified in terminal | ✅ |
| First `.java` file compiled and run successfully | ✅ |

---

## 📌 DAY 1 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 1 — SUMMARY                        ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  March 30, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Variables, Data Types, Type Casting, Operators  ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  How Java works — JDK → Bytecode → JVM → Output             ║
║  ✅  Basic program structure — every keyword explained           ║
║  ✅  8 Primitive Data Types + String (non-primitive)             ║
║  ✅  Variables — declaration and initialization                  ║
║  ✅  Widening (auto) vs Narrowing (manual) Type Casting          ║
║  ✅  Arithmetic Operators  ( +  -  *  /  % )                    ║
║  ✅  Relational Operators  ( ==  !=  >  <  >=  <= )             ║
║  ✅  Logical Operators     ( &&  ||  ! )                        ║
║  ✅  Increment / Decrement ( ++  -- ) pre and post              ║
║  ✅  Assignment Operators  ( +=  -=  *=  /=  %= )              ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅  |  Setup          : ✅                     ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  ████████░░  80%                              ║
║  STRONG AREA    :  Operators, type casting logic                 ║
║  WEAK AREA      :  Post vs Pre increment in complex expressions  ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  Integer division silently drops decimals — a sneaky bug     ║
║  ▸  Always cast BEFORE dividing, not after                      ║
║  ▸  = (assign) vs == (compare) — never confuse these            ║
║  ▸  % (modulo) is one of the most used operators in DSA         ║
╚══════════════════════════════════════════════════════════════════╝
```
---

<div align="center">

[← Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 02 →](./Day02_Conditionals.md)

*Day 1 of 160 — Done ✅*

</div>
