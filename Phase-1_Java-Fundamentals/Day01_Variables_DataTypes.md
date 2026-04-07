# 📄 Day 01 — Variables, Data Types, Type Casting & Operators

<div align="center">

![Day](https://img.shields.io/badge/Day-01_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Variables_%26_Operators-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Practice_Problems-6_of_6-blue?style=for-the-badge)
![W3S](https://img.shields.io/badge/W3Schools_Exercises-20_Sets_✅-04AA6D?style=for-the-badge&logo=w3schools&logoColor=white)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 02 →](./Day02_Conditionals.md)

</div>

---

## 🎯 Day 1 Goal
> Understand how Java works, set up the development environment, master variables, data types, type casting, and all operators — then **write code**, not just read it. Reinforce with W3Schools structured exercises.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | Java Intro + Setup + First Program | 60 min |
| Block 2 | Variables + Data Types + Type Casting | 60 min |
| Block 3 | Operators — All Types | 60 min |
| Block 4 | Practice Set — 6 Problems | 90 min |
| Block 5 | W3Schools Exercises + Revision + Reflection | 30 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. How Java Works

Java is a **compiled + interpreted** language — this makes it platform-independent.

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
| **Bytecode** | — | Platform-neutral `.class` file |

> 💡 **"Write Once, Run Anywhere"** — JVM is platform-specific, but bytecode is universal.

---

### 🔷 2. Java Syntax & First Program

Every Java program must have **a class** and a **main method** — this is where execution begins.

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, World!");   // prints with newline
        System.out.print("Hello ");            // prints without newline
        System.out.println("World!");          // continues same line
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
| `System.out.println` | Prints to console **WITH** new line |
| `System.out.print` | Prints to console **WITHOUT** new line |

**Output numbers directly:**
```java
System.out.println(42);       // integer
System.out.println(3.14);     // decimal
System.out.println(100 + 50); // arithmetic result: 150
```

---

### 🔷 3. Comments in Java

Comments are ignored by the compiler — used for notes and documentation.

```java
// This is a single-line comment

/* This is a
   multi-line comment */

/**
 * This is a Javadoc comment
 * Used for documentation generation
 */
public class Main {
    public static void main(String[] args) {
        // System.out.println("This line is commented out — won't run");
        System.out.println("This runs!"); // inline comment
    }
}
```

> 💡 Use comments to explain WHY, not WHAT. Code shows what — comments explain reasoning.

---

### 🔷 4. Variables & Data Types

A **variable** is a named memory container that stores a value.

```java
// Syntax: dataType variableName = value;
int     age    = 20;
double  price  = 99.99;
char    grade  = 'A';
boolean isPass = true;
String  name   = "Abhishek";   // Non-primitive (Object)
```

**Multiple variables — several ways:**
```java
// Declare separately
int x = 5;
int y = 10;
int z = 15;

// Declare same type on one line
int x = 5, y = 10, z = 15;

// Declare then assign
int x;
x = 5;

// Same value to multiple variables
int x = y = z = 50;
```

**Variable naming rules (identifiers):**
```java
// ✅ Valid names
int myAge = 25;
int _count = 0;
int $price = 100;
int myFavoriteNumber = 7;

// ❌ Invalid names
int 2cool = 5;       // cannot start with digit
int my-age = 5;      // no hyphens
int class = 5;       // reserved keyword

// ✅ Conventions (follow these!)
int camelCase = 5;         // variables — camelCase
final int MAX_SIZE = 100;  // constants — UPPER_SNAKE_CASE
```

---

### 🔷 5. Primitive Data Types — Complete Table

| Type | Size | Default | Range | Example |
|------|------|---------|-------|---------|
| `byte` | 1 byte | 0 | -128 to 127 | `byte b = 100;` |
| `short` | 2 bytes | 0 | -32,768 to 32,767 | `short s = 1000;` |
| `int` | 4 bytes | 0 | -2,147,483,648 to 2,147,483,647 | `int x = 500;` |
| `long` | 8 bytes | 0L | ±9.2 × 10¹⁸ | `long l = 99999L;` |
| `float` | 4 bytes | 0.0f | 6–7 decimal digits | `float f = 3.14f;` |
| `double` | 8 bytes | 0.0d | 15 decimal digits | `double d = 3.14159;` |
| `char` | 2 bytes | '\u0000' | Single Unicode char | `char c = 'A';` |
| `boolean` | 1 bit | false | true / false | `boolean b = true;` |

**Numbers — key rules:**
```java
// Integer types
byte  b = 127;         // max byte
short s = 32767;       // max short
int   i = 2147483647;  // max int
long  l = 15000000000L; // MUST add L suffix for long literals

// Floating-point
float  f = 3.14f;      // MUST add f suffix for float literals
double d = 3.14159265; // default decimal type — no suffix needed

// Underscores for readability (Java 7+)
int million = 1_000_000;
long bigNum = 9_999_999_999L;

// Scientific notation
double sci = 1.5e10;   // 1.5 × 10^10 = 15000000000.0
```

**Boolean type:**
```java
boolean isJavaFun = true;
boolean isFishTasty = false;

// Booleans from expressions — evaluated directly
boolean isAdult  = (18 >= 18);    // true
boolean isEqual  = (10 == 9);     // false
boolean isBig    = (5 > 3);       // true

System.out.println(isAdult);      // true
System.out.println(10 > 9);       // true — directly in println
System.out.println(10 == 15);     // false
```

**Characters — key rules:**
```java
char letter = 'A';         // single quotes — NOT double!
char digit  = '5';         // digit as char, NOT int
char space  = ' ';         // space is also a char

// ASCII / Unicode values
char a = 65;               // prints 'A' (ASCII 65)
char b = 'B';
System.out.println((int) b); // prints 66 — char to its ASCII value

// char arithmetic
char c = 'A';
c++;                       // now c = 'B'
System.out.println(c);     // B

// char can be used in expressions
System.out.println('A' + 1); // 66 (arithmetic on ASCII value)
```

**Primitive vs Non-Primitive:**

| Feature | Primitive | Non-Primitive |
|---------|-----------|---------------|
| Stores | Actual value | Memory reference |
| Memory | Stack | Heap |
| Examples | `int`, `char`, `boolean` | `String`, Arrays, Classes |
| Default | 0 / false | `null` |
| Methods | ❌ No methods | ✅ Has methods |

---

### 🔷 6. Strings in Java

```java
// Creating strings
String greeting = "Hello, World!";
String name = "Abhishek";

// String length
System.out.println(greeting.length());   // 13

// String concatenation — two ways
String firstName = "Abhishek";
String lastName  = "Ranjan";
String fullName  = firstName + " " + lastName;   // + operator
String fullName2 = firstName.concat(" " + lastName); // concat method

System.out.println(fullName);   // Abhishek Ranjan

// Concatenating strings with numbers
int age = 24;
String message = "I am " + age + " years old.";
System.out.println(message);    // I am 24 years old.

// ⚠️ Order matters with + and numbers
System.out.println("Sum: " + 5 + 3);    // Sum: 53  ← string concat!
System.out.println("Sum: " + (5 + 3));  // Sum: 8   ← arithmetic first!
System.out.println(5 + 3 + " is sum");  // 8 is sum ← arithmetic first!
```

**Special Characters (Escape Sequences):**
```java
// Inside strings, some characters need escaping with backslash \
System.out.println("He said \"Hello\"");  // He said "Hello"
System.out.println("It\'s fine");         // It's fine
System.out.println("C:\\Users\\Java");    // C:\Users\Java
System.out.println("Line1\nLine2");       // Line1 (newline) Line2
System.out.println("Col1\tCol2");         // Col1 (tab) Col2
System.out.println("Back\\slash");        // Back\slash
```

| Escape | Meaning | Example Output |
|--------|---------|----------------|
| `\"` | Double quote | `He said "Hello"` |
| `\'` | Single quote | `It's fine` |
| `\\` | Backslash | `C:\Users` |
| `\n` | New line | Line break |
| `\t` | Tab | Horizontal space |
| `\r` | Carriage return | Moves cursor to line start |

---

### 🔷 7. Type Casting

Converting a value from one data type to another.

**Widening — Automatic (small → big, NO data loss)**
```java
int    x = 100;
long   l = x;       // int → long   ✅ automatic
double d = x;       // int → double ✅ automatic

// Safe widening order:
// byte → short → int → long → float → double
```

**Narrowing — Manual (big → small, data may be lost)**
```java
double d = 9.99;
int    x = (int) d;    // x = 9  ← decimal TRUNCATED (NOT rounded!)

double pi = 3.99;
int    n  = (int) pi;  // n = 3  ← always truncates, never rounds
```

> ⚠️ **Critical Rule:** Narrowing TRUNCATES. `(int) 3.9 = 3`, not `4`.

```java
// Practical examples of casting
double myDouble = 9.78;
int    myInt    = (int) myDouble;   // 9

int    num      = 65;
char   letter   = (char) num;       // 'A'  (ASCII 65)

// Most Common Bug — Integer Division
int a = 7, b = 2;
double r1 = a / b;            // → 3.0  ❌ (int division FIRST, then stored)
double r2 = (double) a / b;   // → 3.5  ✅ (cast BEFORE dividing)
```

---

### 🔷 8. Operators

**A. Arithmetic Operators**
```java
int a = 10, b = 3;
System.out.println(a + b);   // 13 — Addition
System.out.println(a - b);   // 7  — Subtraction
System.out.println(a * b);   // 30 — Multiplication
System.out.println(a / b);   // 3  — Integer division (decimal dropped!)
System.out.println(a % b);   // 1  — Remainder (Modulo)
```

**B. Relational / Comparison Operators — always returns boolean**
```java
System.out.println(10 > 3);    // true
System.out.println(10 < 3);    // false
System.out.println(10 >= 10);  // true
System.out.println(10 <= 9);   // false
System.out.println(10 == 10);  // true  ← equality CHECK (double =)
System.out.println(10 != 5);   // true  ← not equal
```
> ⚠️ `=` is **assignment**. `==` is **comparison**. Never mix these up!

**C. Logical Operators**
```java
System.out.println(true && true);    // true  — AND: both must be true
System.out.println(true && false);   // false
System.out.println(true || false);   // true  — OR: at least one true
System.out.println(false || false);  // false
System.out.println(!true);           // false — NOT: reverses
System.out.println(!false);          // true
```

**D. Increment & Decrement**
```java
int x = 5;
System.out.println(x++);   // 5  → prints THEN increments → x=6
System.out.println(++x);   // 7  → increments THEN prints → x=7
System.out.println(x--);   // 7  → prints THEN decrements → x=6
System.out.println(--x);   // 5  → decrements THEN prints → x=5
```

**E. Assignment Operators**
```java
int x = 10;
x += 5;   // x = x + 5 = 15
x -= 3;   // x = x - 3 = 12
x *= 2;   // x = x * 2 = 24
x /= 4;   // x = x / 4 = 6
x %= 4;   // x = x % 4 = 2
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Hello Programmer `[Easy]`

**Task:** Store name and age in variables and print a formatted sentence.

```java
public class P1_Details {
    public static void main(String[] args) {
        String name = "Abhishek Ranjan";
        int age = 24;
        System.out.println("My name is " + name + " and I am " + age + " years old.");
    }
}
```

```
Output:
My name is Abhishek Ranjan and I am 24 years old.
```

> 🔑 `+` concatenates String with int — Java auto-converts `int` to String when joining.

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

> 🔑 `17/5 = 3` (integer division). Cast to `double` before dividing to get `3.4`.

---

### ✅ Problem 3 — Temperature Converter `[Easy-Medium]`

**Task:** Convert Celsius to Fahrenheit. Formula: `F = (C × 9/5) + 32`

```java
public class P3_TemperatureConverter {
    public static void main(String[] args) {
        double celsius = 37.0;
        double fahrenheit = (celsius * 9 / 5) + 32;
        System.out.println(celsius + "\u00b0C = " + fahrenheit + "\u00b0F");
    }
}
```

```
Output:
37.0°C = 98.6°F
```

> 🔑 Using `double celsius` ensures floating-point math — prevents `9/5` becoming `1`.

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
        double result1 = a / b;            // int division FIRST → 3 → stored as 3.0
        double result2 = (double) a / b;   // cast FIRST → 3.5

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

> 🔑 `a / b` evaluates as integers (= 3) THEN stores as 3.0. Casting before dividing changes it to floating-point math.

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

> 🔑 `n % 2 == 0` is itself a boolean — evaluates to `true` or `false` directly.

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
        System.out.println("Total: \u20b9" + totalCost + " | Change: \u20b9" + change);
    }
}
```

```
Output:
Total: ₹705 | Change: ₹295
```

---

## 📚 W3SCHOOLS EXTRA PRACTICE — ✅ All 20 Sets Completed

> Practiced via: [W3Schools Java Exercises](https://www.w3schools.com/java/exercise.asp?x=xrcise_syntax1)
> External code reference: [Google Drive — Java Practice Files](https://drive.google.com/drive/folders/13XLhNc1EOTbsxcr8jHnewYX30j-_fjeE?usp=sharing)

---

### 📋 Exercise Sets Completed

| # | Exercise Set | Questions | Status | Key Concept Reinforced |
|---|-------------|-----------|--------|----------------------|
| 1 | [Syntax](https://www.w3schools.com/java/exercise.asp?x=xrcise_syntax1) | 7 | ✅ | Class structure, main method, code block rules |
| 2 | [Output Text](https://www.w3schools.com/java/exercise.asp?x=xrcise_output1) | 5 | ✅ | `println` vs `print`, string output |
| 3 | [Output Numbers](https://www.w3schools.com/java/exercise.asp?x=xrcise_output_numbers1) | 7 | ✅ | Printing ints, floats, arithmetic in print |
| 4 | [Comments](https://www.w3schools.com/java/exercise.asp?x=xrcise_comments1) | 5 | ✅ | `//` single-line, `/* */` multi-line |
| 5 | [Variables](https://www.w3schools.com/java/exercise.asp?x=xrcise_variables1) | 6 | ✅ | Declaration, initialization, data types |
| 6 | [Print Variables](https://www.w3schools.com/java/exercise.asp?x=xrcise_variables_print1) | 8 | ✅ | Printing variable values, concatenation |
| 7 | [Multiple Variables](https://www.w3schools.com/java/exercise.asp?x=xrcise_variables_multiple1) | 4 | ✅ | Declaring multiple vars, same-type shorthand |
| 8 | [Variable Names](https://www.w3schools.com/java/exercise.asp?x=xrcise_variables_identifiers1) | 5 | ✅ | Naming rules, conventions, reserved words |
| 9 | [Data Types](https://www.w3schools.com/java/exercise.asp?x=xrcise_data_types1) | 6 | ✅ | Choosing correct type for values |
| 10 | [Numbers](https://www.w3schools.com/java/exercise.asp?x=xrcise_data_types_numbers1) | 7 | ✅ | `int` vs `long` vs `float` vs `double`, suffixes |
| 11 | [Boolean Types](https://www.w3schools.com/java/exercise.asp?x=xrcise_data_types_boolean1) | 5 | ✅ | `true`/`false`, boolean expressions |
| 12 | [Characters](https://www.w3schools.com/java/exercise.asp?x=xrcise_data_types_characters1) | 7 | ✅ | `char`, single quotes, ASCII values |
| 13 | [Type Casting](https://www.w3schools.com/java/exercise.asp?x=xrcise_type_casting1) | 7 | ✅ | Widening vs narrowing, cast syntax |
| 14 | [Operators](https://www.w3schools.com/java/exercise.asp?x=xrcise_operators1) | 7 | ✅ | Arithmetic, assignment operators |
| 15 | [Strings](https://www.w3schools.com/java/exercise.asp?x=xrcise_strings1) | 8 | ✅ | String creation, `length()`, `toUpperCase()` |
| 16 | [String Concatenation](https://www.w3schools.com/java/exercise.asp?x=xrcise_strings_concat1) | 5 | ✅ | `+` operator, `concat()`, mixing types |
| 17 | [Strings and Numbers](https://www.w3schools.com/java/exercise.asp?x=xrcise_strings_numbers1) | 5 | ✅ | String + int order issues, `+` left-to-right |
| 18 | [Special Characters](https://www.w3schools.com/java/exercise.asp?x=xrcise_strings_specchars1) | 4 | ✅ | `\"`, `\\`, `\n`, `\t` escape sequences |
| 19 | [Booleans](https://www.w3schools.com/java/exercise.asp?x=xrcise_booleans1) | 6 | ✅ | Boolean values, expressions in conditions |
| 20 | [Boolean Challenges](https://www.w3schools.com/java/java_challenges_booleans.asp) | Challenge | ✅ | Boolean logic problem solving |

**Total: 115 questions + challenges — all completed ✅**

---

### 🔑 Key Concepts from W3Schools Practice

#### 📌 Syntax Rules Reinforced
```java
// ✅ Every Java program needs a class
public class MyClass {
    // ✅ Every program needs a main method
    public static void main(String[] args) {
        // ✅ Each statement ends with semicolon
        System.out.println("Hello");
        // ✅ Code blocks use curly braces {}
    }
}
// ✅ File name MUST match class name: MyClass.java
```

#### 📌 Output Methods — Difference Mastered
```java
System.out.println("Hello");    // prints + moves to NEW line
System.out.print("Hello");      // prints + STAYS on same line
System.out.println(100 + 50);   // prints: 150 (arithmetic done first)
System.out.println("100" + 50); // prints: 10050 (string concat!)
```

#### 📌 Numbers Practice — Type & Suffix Rules
```java
int    myInt    = 100000;
long   myLong   = 15000000000L;  // L suffix required
float  myFloat  = 5.75f;         // f suffix required
double myDouble = 19.99;         // default — no suffix needed

// Scientific notation
double sci = 35e3;   // 35000.0
float  sci2 = 35E3f; // 35000.0 as float
```

#### 📌 Boolean Expressions Mastered
```java
// Boolean is the result of any comparison
int x = 10, y = 9;
System.out.println(x > y);      // true
System.out.println(x == y);     // false
System.out.println(x == 10);    // true

boolean isGreater = x > y;      // store result in variable
System.out.println(isGreater);  // true

// Find max using boolean expression
boolean xIsGreater = x > y;     // true → x is the max
```

#### 📌 Characters — ASCII Mapping Practiced
```java
char letter = 'A';
System.out.println(letter);          // A
System.out.println((int) letter);    // 65 (ASCII value)

// Incrementing chars
char c = 'A';
System.out.println(++c);             // B
System.out.println(c + 1);          // 67 (int arithmetic on ASCII)

// char from int
char fromInt = 66;
System.out.println(fromInt);         // B
```

#### 📌 String Concatenation — Order of + Mastered
```java
// The key rule: + works LEFT TO RIGHT
// Once a String is encountered, all subsequent + are string concat

String s = "Result: ";
System.out.println(s + 5 + 3);      // Result: 53  ← 5 and 3 not added!
System.out.println(s + (5 + 3));    // Result: 8   ← parentheses force arithmetic
System.out.println(5 + 3 + s);      // 8Result:    ← 5+3=8 first, then concat

// Practical gotcha tested in exercise:
System.out.println("Java " + 10 + 20); // Java 1020 (NOT Java 30!)
System.out.println(10 + 20 + " Java"); // 30 Java   (arithmetic first!)
```

#### 📌 Special Characters Practiced
```java
// All escape sequences tested:
System.out.println("He said \"Hello World\"");   // He said "Hello World"
System.out.println("It\'s easy to learn Java");  // It's easy to learn Java
System.out.println("The path is C:\\Users\\test"); // The path is C:\Users\test
System.out.println("First Line\nSecond Line");    // two lines
System.out.println("Name:\tAbhishek");            // tab spacing
```

#### 📌 Type Casting — W3Schools Problems
```java
// Widening (automatic)
int myInt = 9;
double myDouble = myInt;    // 9.0 — automatic, safe

// Narrowing (manual)
double myDouble2 = 9.78;
int myInt2 = (int) myDouble2;  // 9 — truncated, NOT rounded

// Practical: convert myDouble to int, float, long
double d = 9.99;
int    i = (int) d;     // 9
float  f = (float) d;   // 9.99  (some precision may differ)
long   l = (long) d;    // 9

System.out.println(i);  // 9
System.out.println(f);  // 9.99
System.out.println(l);  // 9
```

---

### 🏆 W3Schools Exercise Stats

```
╔═══════════════════════════════════════════════════════╗
║         W3SCHOOLS EXERCISE COMPLETION STATS          ║
╠═══════════════════════════════════════════════════════╣
║  Total Exercise Sets   :  20                          ║
║  Total Questions       :  115 + challenges            ║
║  Completed             :  ✅ ALL                       ║
║  Topics Covered        :  Syntax, Output, Comments,   ║
║                           Variables, Data Types,      ║
║                           Booleans, Characters,       ║
║                           Type Casting, Operators,    ║
║                           Strings, Special Chars,     ║
║                           Boolean Challenges          ║
╠═══════════════════════════════════════════════════════╣
║  Score     :  ████████████████████  100%              ║
║  Confidence:  ██████████████████░░  90%               ║
╚═══════════════════════════════════════════════════════╝
```

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Status |
|------|--------|
| Re-wrote Hello World from memory | ✅ |
| Listed all 8 primitive types with sizes from memory | ✅ |
| Explained type casting in own words without notes | ✅ |
| Re-coded all operator examples without looking | ✅ |
| Completed all 20 W3Schools exercise sets | ✅ |
| Practiced code on Google Drive Java file | ✅ |

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. Difference between float and double?</b></summary>

> `float` = 4 bytes, 6–7 decimal digits, needs `f` suffix (`3.14f`).
> `double` = 8 bytes, 15 decimal digits, default decimal type — no suffix needed.
> Use `double` for most calculations. Use `float` only when memory is very constrained.

</details>

<details>
<summary><b>Q2. Difference between = and == ?</b></summary>

> `=` is the **assignment operator** — stores a value. Example: `int x = 5;`
> `==` is the **equality comparison** — returns boolean. Example: `x == 5` → `true`
> Confusing them is one of the most common beginner bugs in Java.

</details>

<details>
<summary><b>Q3. Why does 10/3 give 3? How to fix it?</b></summary>

> Both operands are `int` → Java performs integer division — decimal is truncated.
> Fix: cast one operand to `double` before dividing:
> `(double)10 / 3` → `3.3333` ✅ &nbsp;&nbsp; `10.0 / 3` → `3.3333` ✅

</details>

<details>
<summary><b>Q4. What is Type Casting? Real-life analogy?</b></summary>

> Converting a value from one data type to another.
> **Widening (auto):** Pouring water from a small cup into a big bucket — safe, no loss.
> **Narrowing (manual):** Pouring from a bucket into a small cup — overflows, data lost.
> Analogy: Saving a 4K video as 360p — smaller file but detail permanently lost.

</details>

<details>
<summary><b>Q5. What is % useful for?</b></summary>

> `%` gives the **remainder** after division.
> Even/odd: `n % 2 == 0`, Divisibility: `n % 5 == 0`, Last digit: `n % 10`,
> Circular indexing in arrays: `index % size`.

</details>

<details>
<summary><b>Q6. Why does "Java " + 10 + 20 give "Java 1020" not "Java 30"?</b></summary>

> The `+` operator is evaluated **left to right**.
> `"Java " + 10` → `"Java 10"` (String + int = String concatenation)
> `"Java 10" + 20` → `"Java 1020"` (still String concatenation)
> Fix: `"Java " + (10 + 20)` → `"Java 30"` — parentheses force arithmetic first.

</details>

<details>
<summary><b>Q7. What is the difference between println and print?</b></summary>

> `System.out.println()` — prints the content and **moves to a new line**.
> `System.out.print()` — prints the content and **stays on the same line**.
> Use `print` when building output piece by piece on one line, `println` for completed lines.

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
| W3Schools account — exercises tracked online | ✅ |
| Google Drive code folder set up | ✅ |

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
║  ✅  Java syntax — class structure, main method, semicolons      ║
║  ✅  Output methods — println vs print, printing numbers         ║
║  ✅  Comments — single-line, multi-line, Javadoc                 ║
║  ✅  8 Primitive Data Types — byte,short,int,long,float,         ║
║      double,char,boolean + String (non-primitive)               ║
║  ✅  Variables — declaration, initialization, naming rules       ║
║  ✅  Multiple variable declarations + same-value assignment      ║
║  ✅  Numbers — suffixes (L for long, f for float)                ║
║  ✅  Booleans — true/false + expressions as booleans            ║
║  ✅  Characters — char literals, ASCII values, arithmetic        ║
║  ✅  Strings — creation, length, concat, String+Number order     ║
║  ✅  Special characters — \", \\, \n, \t escape sequences        ║
║  ✅  Widening (auto) vs Narrowing (manual) Type Casting          ║
║  ✅  All Operators — Arithmetic, Relational, Logical,            ║
║      Increment/Decrement, Assignment                            ║
╠══════════════════════════════════════════════════════════════════╣
║  Practice Set    : 6/6 ✅   Mini Challenge : ✅                   ║
║  Revision Tasks  : 6/6 ✅   Reflection     : 7/7 ✅              ║
║  W3Schools Sets  : 20/20 ✅ Total Questions: 115+ ✅             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  ████████████████████  95%                    ║
║  STRONG AREA    :  Operators, data types, casting, output        ║
║  WEAK AREA      :  Post vs Pre increment edge cases              ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  Integer division silently drops decimals — sneaky bug       ║
║  ▸  Always cast BEFORE dividing, not after                      ║
║  ▸  = (assign) vs == (compare) — never confuse                  ║
║  ▸  "Java " + 10 + 20 = "Java 1020" — + is left to right       ║
║  ▸  long needs L suffix, float needs f suffix                   ║
║  ▸  Narrowing TRUNCATES — (int)3.9 = 3, NOT 4                   ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 02 →](./Day02_Conditionals.md)

*Day 1 of 160 — Done ✅ | W3Schools: 20 Sets Completed 🏆*

</div>
