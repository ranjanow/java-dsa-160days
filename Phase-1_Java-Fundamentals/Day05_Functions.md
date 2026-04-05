# 📄 Day 05 — Functions & Methods

<div align="center">

![Day](https://img.shields.io/badge/Day-05_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Functions_%26_Methods-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 04](./Day04_Loops_Patterns.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 06 →](./Day06_Arrays.md)

</div>

---

## 🎯 Day 5 Goal
> Master functions and methods in Java — define, call, pass arguments, return values, understand scope, and use method overloading. Functions are the **building blocks of clean code** and the structure every DSA solution depends on.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | What is a method, syntax, types, calling | 60 min |
| Block 2 | Parameters, arguments, return types, void | 50 min |
| Block 3 | Scope, pass-by-value, method overloading | 30 min |
| Block 4 | Revision — Days 3 & 4 flash review | 15 min |
| Block 5 | Practice Set — 6 Problems | 110 min |
| Block 6 | Mini Challenge + Revision Tasks | 35 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. Why Functions Exist

Without functions — repeated logic must be written every time:

```java
// Without function — repeated, hard to maintain
int sum1 = 0;
for (int i = 1; i <= 5; i++) sum1 += i;
System.out.println(sum1);   // 15

int sum2 = 0;
for (int i = 1; i <= 10; i++) sum2 += i;
System.out.println(sum2);   // 55

// With function — write once, reuse forever
static int sumUpTo(int n) {
    int sum = 0;
    for (int i = 1; i <= n; i++) sum += i;
    return sum;
}
System.out.println(sumUpTo(5));    // 15
System.out.println(sumUpTo(10));   // 55
System.out.println(sumUpTo(100));  // 5050
```

> 💡 **Functions give you:** Reusability, readability, testability, and the ability to break any big problem into small solvable pieces — the **core philosophy of all DSA problem solving**.

---

### 🔷 2. Method Anatomy — Every Part Explained

```java
//    ①      ②        ③           ④
  static  int  addTwoNumbers (int a, int b) {
      int result = a + b;   // ⑤ body
      return result;        // ⑥ return statement
  }
```

| # | Part | Name | Meaning |
|---|------|------|---------|
| ① | `static` | Modifier | Can be called without creating an object |
| ② | `int` | Return Type | Sends back an `int`; use `void` if nothing returned |
| ③ | `addTwoNumbers` | Method Name | camelCase verb describing what it does |
| ④ | `int a, int b` | Parameters | Input slots — filled with arguments when called |
| ⑤ | Body | Logic block | What the method actually does |
| ⑥ | `return result` | Return Statement | Sends value back to the caller, exits method |

---

### 🔷 3. Four Types of Methods

**A. void — does something, returns nothing**
```java
static void greet(String name) {
    System.out.println("Hello, " + name + "!");
}
greet("Rahul");   // Output: Hello, Rahul!
```

**B. Return-type — computes and sends back a value**
```java
static int square(int n) {
    return n * n;
}
int result = square(5);          // result = 25
System.out.println(square(7));   // 49 — use directly
```

**C. No-parameter method**
```java
static void printLine() {
    System.out.println("─────────────────");
}
printLine();   // prints the separator
```

**D. Multiple parameters**
```java
static double average(int a, int b, int c) {
    return (a + b + c) / 3.0;
}
System.out.println(average(10, 20, 30));   // 20.0
```

---

### 🔷 4. How Method Calls Work — The Call Stack

```java
public static void main(String[] args) {
    System.out.println("Start");
    int result = multiply(4, 5);         // jumps to multiply()
    System.out.println("Result: " + result);
    System.out.println("End");
}

static int multiply(int a, int b) {
    return a * b;                        // returns 20, jumps back to main
}
```

```
Execution flow:
main() starts → prints "Start"
  → calls multiply(4, 5)
      → multiply executes: 4 × 5 = 20
      → returns 20
  ← back in main: result = 20
  → prints "Result: 20"
  → prints "End"
main() ends
```

> 💡 Java uses a **Call Stack** — each method call pushes a frame onto the stack, each `return` pops it. This is why infinite recursion causes `StackOverflowError` — the stack overflows.

---

### 🔷 5. Parameters vs Arguments

```java
// PARAMETERS — variable names in the method DEFINITION (placeholders)
static int add(int x, int y) {     // x and y are PARAMETERS
    return x + y;
}

// ARGUMENTS — actual values PASSED when CALLING (real values)
int result = add(10, 20);          // 10 and 20 are ARGUMENTS
```

> 📌 **Memory trick:** Parameters = Placeholder. Arguments = Actual values.

---

### 🔷 6. Pass-by-Value — Critical Interview Concept

Java **always** passes primitive types by value — a **copy** is made. The original variable is NEVER modified by the method.

```java
static void tryToChange(int x) {
    x = 999;    // changes the LOCAL COPY only
    System.out.println("Inside : " + x);   // 999
}

public static void main(String[] args) {
    int num = 42;
    tryToChange(num);
    System.out.println("Outside: " + num); // still 42!
}
```

```
Output:
Inside : 999
Outside: 42    ← original completely untouched!
```

**Why this happens:**
```
main()        tryToChange()
num = 42  →   x = 42  (a COPY is made)
              x = 999  (only the copy changes)
num = 42  ←   method ends, copy is discarded
```

> ⚠️ **Guaranteed interview question.** Primitives = passed by value (copy). Objects/arrays = passed by reference (the reference is copied — covered in OOP). Know this cold.

---

### 🔷 7. Variable Scope

Scope = **where a variable can be seen and used**.

```java
static int classLevel = 100;     // class-level — visible in ALL methods

static void methodA() {
    int x = 10;                  // local to methodA ONLY
    System.out.println(x);       // ✅ fine
    System.out.println(classLevel); // ✅ fine — class-level visible
}

static void methodB() {
    System.out.println(x);       // ❌ ERROR — x not visible here
    int y = 20;                  // local to methodB ONLY
}
```

**Block-level scope:**
```java
for (int i = 0; i < 5; i++) {
    int temp = i * 2;   // temp exists ONLY inside this for block
}
System.out.println(temp); // ❌ ERROR — temp out of scope
System.out.println(i);    // ❌ ERROR — i out of scope
```

> 💡 **Rule:** A variable lives and dies within the `{ }` block where it was declared. This is not just a Java rule — it applies to C, C++, Python, JavaScript too.

---

### 🔷 8. Method Overloading

**Same method name, different parameter signatures.** Java automatically picks the right version.

```java
static int add(int a, int b) {
    return a + b;
}
static double add(double a, double b) {
    return a + b;
}
static int add(int a, int b, int c) {
    return a + b + c;
}

// Java picks automatically based on arguments:
System.out.println(add(2, 3));        // → 5      (int version)
System.out.println(add(2.5, 3.7));    // → 6.2    (double version)
System.out.println(add(1, 2, 3));     // → 6      (3-param version)
```

**Valid overloading:**
- ✅ Different number of parameters
- ✅ Different types of parameters
- ✅ Different order of parameter types
- ❌ Only different return type → **NOT valid** — compiler error

---

### 🔷 9. Return Statement Rules

```java
// return exits immediately — code after it is unreachable
static int absValue(int n) {
    if (n < 0) return -n;   // exits here if n is negative
    return n;               // only reached if n >= 0
}

// Multiple return points — clean and valid
static String classify(int n) {
    if (n > 0) return "Positive";
    if (n < 0) return "Negative";
    return "Zero";
}

// void + early return — avoids deep nesting
static void printIfPositive(int n) {
    if (n <= 0) return;    // early exit
    System.out.println(n);
}
```

---

### 🔷 10. Method Best Practices

```
✅ One method = one responsibility
✅ Name = action verb: isPrime, calculateSum, printPattern, findMax
✅ Boolean methods: isXxx() or hasXxx()
✅ Keep short — > 20 lines? Break it up
✅ Meaningful parameter names — not (a, b) but (num1, num2)
❌ Don't repeat logic — extract it into a method
❌ Don't use global variables when parameters work
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Basic Method Practice `[Easy]`

**Task:** Write three separate methods and call them from `main()`.

```java
public class P1_BasicMethods {

    // Method 1: returns int
    static int square(int n) {
        return n * n;
    }

    // Method 2: returns boolean
    static boolean isEven(int n) {
        return n % 2 == 0;
    }

    // Method 3: void — prints, returns nothing
    static void printStars(int n) {
        for (int i = 0; i < n; i++) System.out.print("* ");
        System.out.println();
    }

    public static void main(String[] args) {
        System.out.println("Square of 6  : " + square(6));
        System.out.println("Square of 11 : " + square(11));

        System.out.println("isEven(4)    : " + isEven(4));
        System.out.println("isEven(7)    : " + isEven(7));

        printStars(5);
        printStars(3);
    }
}
```

```
Output:
Square of 6  : 36
Square of 11 : 121
isEven(4)    : true
isEven(7)    : false
* * * * *
* * *
```

> 🔑 **Key Learning:** Three methods — three different return types (`int`, `boolean`, `void`). Notice `isEven` returns the boolean expression directly: `n % 2 == 0` is already a boolean — no need for `if-else`. `printStars` does work but returns nothing — `void` is the right choice. This is clean method design.

---

### ✅ Problem 2 — Min and Max of Three `[Easy]`

**Task:** Write `max()` and `min()` methods for three integers.

```java
public class P2_MinMax {

    static int max(int a, int b, int c) {
        return Math.max(a, Math.max(b, c));
    }

    static int min(int a, int b, int c) {
        return Math.min(a, Math.min(b, c));
    }

    public static void main(String[] args) {
        System.out.println("Max(12,45,23) = " + max(12, 45, 23));  // 45
        System.out.println("Min(12,45,23) = " + min(12, 45, 23));  // 12

        System.out.println("Max(7,7,3)    = " + max(7, 7, 3));     // 7
        System.out.println("Min(7,7,3)    = " + min(7, 7, 3));     // 3

        System.out.println("Max(-5,-1,-10)= " + max(-5, -1, -10)); // -1
        System.out.println("Min(-5,-1,-10)= " + min(-5, -1, -10)); // -10
    }
}
```

```
Output:
Max(12,45,23) = 45
Min(12,45,23) = 12
Max(7,7,3)    = 7
Min(7,7,3)    = 3
Max(-5,-1,-10)= -1
Min(-5,-1,-10)= -10
```

> 🔑 **Key Learning:** `Math.max(a, Math.max(b, c))` — nesting built-in methods is perfectly valid. For three values: find max of last two, then max of that result with the first. Works correctly for negative numbers and equal values. Clean one-liner logic inside a method.

---

### ✅ Problem 3 — isPrime Method + printPrimes `[Easy-Medium]`

**Task:** Wrap prime logic in a reusable method. Use it to print all primes in a range.

```java
public class P3_PrimeMethods {

    // Reusable isPrime — same √n optimization from Day 3
    static boolean isPrime(int n) {
        if (n <= 1) return false;
        if (n == 2) return true;
        if (n % 2 == 0) return false;       // even numbers > 2 not prime
        for (int i = 3; i * i <= n; i += 2) { // check odd divisors only
            if (n % i == 0) return false;
        }
        return true;
    }

    // Uses isPrime — no prime logic duplicated here
    static void printPrimes(int start, int end) {
        System.out.print("Primes [" + start + "-" + end + "]: ");
        for (int i = start; i <= end; i++) {
            if (isPrime(i)) System.out.print(i + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        // Test isPrime individually
        System.out.println("isPrime(29) : " + isPrime(29));   // true
        System.out.println("isPrime(15) : " + isPrime(15));   // false
        System.out.println("isPrime(2)  : " + isPrime(2));    // true
        System.out.println("isPrime(1)  : " + isPrime(1));    // false

        System.out.println();

        // Test printPrimes using isPrime
        printPrimes(1, 50);
        printPrimes(50, 100);
    }
}
```

```
Output:
isPrime(29) : true
isPrime(15) : false
isPrime(2)  : true
isPrime(1)  : false

Primes [1-50]  : 2 3 5 7 11 13 17 19 23 29 31 37 41 43 47
Primes [50-100]: 53 59 61 67 71 73 79 83 89 97
```

> 🔑 **Key Learning:** `printPrimes` contains ZERO prime-checking logic — it delegates entirely to `isPrime`. This is the **Single Responsibility Principle** in action. If prime logic needs fixing, you fix ONE method and everything that calls it benefits automatically. This is how professional code is written and how DSA solutions handle helper functions.

---

### ✅ Problem 4 — Factorial + nCr `[Easy-Medium]`

**Task:** Build `factorial()` then use it to compute combinations `nCr`.

```java
public class P4_FactorialNCR {

    // long because factorials grow fast — int overflows at 13!
    static long factorial(int n) {
        if (n == 0 || n == 1) return 1;    // base cases
        long fact = 1;
        for (int i = 2; i <= n; i++) fact *= i;
        return fact;
    }

    // Uses factorial — no repetition of factorial logic
    static long nCr(int n, int r) {
        if (r > n) return 0;    // invalid — r can't exceed n
        if (r == 0 || r == n) return 1;   // edge cases
        return factorial(n) / (factorial(r) * factorial(n - r));
    }

    public static void main(String[] args) {
        // Test factorial
        System.out.println("0! = " + factorial(0));   // 1
        System.out.println("1! = " + factorial(1));   // 1
        System.out.println("5! = " + factorial(5));   // 120
        System.out.println("10!= " + factorial(10));  // 3628800

        System.out.println();

        // Test nCr
        System.out.println("nCr(5,2)  = " + nCr(5, 2));   // 10
        System.out.println("nCr(6,3)  = " + nCr(6, 3));   // 20
        System.out.println("nCr(10,4) = " + nCr(10, 4));  // 210
        System.out.println("nCr(5,0)  = " + nCr(5, 0));   // 1
        System.out.println("nCr(3,5)  = " + nCr(3, 5));   // 0 (invalid)
    }
}
```

```
Output:
0! = 1
1! = 1
5! = 120
10!= 3628800

nCr(5,2)  = 10
nCr(6,3)  = 20
nCr(10,4) = 210
nCr(5,0)  = 1
nCr(3,5)  = 0
```

**Why `long` not `int`?**
```
int max  ≈ 2.1 billion
13!      = 6,227,020,800  ← already overflows int!
long max ≈ 9.2 × 10^18   ← safe up to 20!
```

> 🔑 **Key Learning:** Method calling method — `nCr` calls `factorial` three times. The formula `n! / (r! × (n-r)!)` maps directly to three method calls. Edge cases handled first (r>n, r==0, r==n) — this is defensive programming. `long` prevents silent overflow bugs. nCr appears in DP (combinations) and probability problems in DSA.

---

### ✅ Problem 5 — Method Overloading `[Medium]`

**Task:** Overloaded `area()` method for three shapes.

```java
public class P5_AreaOverloading {

    // Circle — one double parameter
    static double area(double radius) {
        return Math.PI * radius * radius;
    }

    // Rectangle — two double parameters
    static double area(double length, double width) {
        return length * width;
    }

    // Triangle — two doubles + boolean to distinguish from rectangle
    static double area(double base, double height, boolean isTriangle) {
        return 0.5 * base * height;
    }

    public static void main(String[] args) {
        // Java picks the right version automatically
        System.out.printf("Circle area    (r=7)      : %.2f%n", area(7.0));
        System.out.printf("Rectangle area (5×8)      : %.2f%n", area(5.0, 8.0));
        System.out.printf("Triangle area  (b=6,h=4)  : %.2f%n", area(6.0, 4.0, true));

        System.out.println();

        // Demonstrating Java's automatic resolution
        System.out.println("area(3.0)        → Circle    : " + (int)area(3.0) + " ≈ 28");
        System.out.println("area(3.0, 4.0)   → Rectangle : " + (int)area(3.0, 4.0));
        System.out.println("area(3.0,4.0,true)→ Triangle : " + area(3.0, 4.0, true));
    }
}
```

```
Output:
Circle area    (r=7)      : 153.94
Rectangle area (5×8)      : 40.00
Triangle area  (b=6,h=4)  : 12.00

area(3.0)         → Circle    : 28 ≈ 28
area(3.0, 4.0)    → Rectangle : 12
area(3.0,4.0,true)→ Triangle  : 6.0
```

**How Java resolves which version to call:**
```
area(7.0)           → 1 double param    → Circle version ✅
area(5.0, 8.0)      → 2 double params   → Rectangle version ✅
area(6.0, 4.0, true)→ 2 doubles+boolean → Triangle version ✅
```

> 🔑 **Key Learning:** Java matches method calls to definitions by counting parameters and checking types — this happens at **compile time**. The `boolean isTriangle` parameter exists to distinguish triangle from rectangle (both would otherwise have 2 double params — ambiguous!). Overloading creates a clean API where callers don't need to remember different method names.

---

### ✅ Problem 6 — Pass-by-Value Trap `[Medium-Hard]`

**Task:** Predict output, explain pass-by-value, then fix the swap.

**Part A — Predicted output (written BEFORE running):**
```
Prediction:
Inside  — x: 25, y: 10    (the swap happens inside the method)
Outside — a: 10, b: 25    (originals unchanged — pass by value)
```

```java
public class P6_PassByValue {

    static void mystery(int x, int y) {
        x = x + y;   // x = 10+25 = 35
        y = x - y;   // y = 35-25 = 10
        x = x - y;   // x = 35-10 = 25
        System.out.println("Inside  — x: " + x + ", y: " + y);
    }

    public static void main(String[] args) {
        int a = 10, b = 25;
        mystery(a, b);
        System.out.println("Outside — a: " + a + ", b: " + b);
    }
}
```

```
Actual Output:
Inside  — x: 25, y: 10
Outside — a: 10, b: 25    ← prediction was correct!
```

**Part B — Why `a` and `b` don't change (pass-by-value explained):**
```
When mystery(a, b) is called:
  Java creates COPIES: x = 10, y = 25  (separate memory locations)
  The swap happens on x and y (the copies)
  x becomes 25, y becomes 10  (inside the method)
  When method ends: x and y are DESTROYED
  a and b were never touched — they're in main()'s own memory frame

Memory diagram:
main frame:     mystery frame:
a = 10          x = 10 → 35 → 25
b = 25          y = 25 → 10
(never changes) (copies, discarded when method ends)
```

**Part C — Fix: return swapped values as array:**
```java
static int[] swapAndReturn(int a, int b) {
    // arithmetic swap on local copies
    a = a + b;
    b = a - b;
    a = a - b;
    return new int[]{a, b};   // return both values in an array
}

public static void main(String[] args) {
    int a = 10, b = 25;
    int[] swapped = swapAndReturn(a, b);
    a = swapped[0];   // 25
    b = swapped[1];   // 10
    System.out.println("After fix — a: " + a + ", b: " + b);
    // Output: After fix — a: 25, b: 10 ✅
}
```

> 🔑 **Key Learning:** This is one of Java's most important and misunderstood concepts. The swap WORKS inside the method — but on copies. The original variables are untouched. To "return" multiple values from a method, use an array or object. This understanding is critical for debugging — when you think a method "changed" a variable and it didn't, pass-by-value is why.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Number Properties Inspector using multiple focused methods.

```java
public class MiniChallenge_Day5 {

    // Method 1: Prime check
    static boolean isPrime(int n) {
        if (n <= 1) return false;
        if (n == 2) return true;
        if (n % 2 == 0) return false;
        for (int i = 3; i * i <= n; i += 2) {
            if (n % i == 0) return false;
        }
        return true;
    }

    // Method 2: Perfect number check
    // Perfect = sum of proper divisors equals the number itself
    static boolean isPerfect(int n) {
        if (n <= 1) return false;
        int sum = 1;   // 1 is always a proper divisor
        for (int i = 2; i * i <= n; i++) {
            if (n % i == 0) {
                sum += i;
                if (i != n / i) sum += n / i;  // add both divisors (avoid double-counting squares)
            }
        }
        return sum == n;
    }

    // Method 3: Armstrong number check (reused from Day 3)
    static boolean isArmstrong(int n) {
        int original = n, temp = n, digits = 0, armSum = 0;
        while (temp > 0) { temp /= 10; digits++; }
        temp = n;
        while (temp > 0) {
            int d = temp % 10;
            armSum += (int) Math.pow(d, digits);
            temp /= 10;
        }
        return armSum == original;
    }

    // Method 4: Inspector — calls all three, prints full report
    static void inspect(int n) {
        String prime     = isPrime(n)     ? "Prime ✅"     : "Not Prime";
        String perfect   = isPerfect(n)   ? "Perfect ✅"   : "Not Perfect";
        String armstrong = isArmstrong(n) ? "Armstrong ✅" : "Not Armstrong";
        System.out.printf("%-4d → %-12s | %-13s | %s%n",
                          n, prime, perfect, armstrong);
    }

    public static void main(String[] args) {
        System.out.println("Num  → Prime        | Perfect       | Armstrong");
        System.out.println("─".repeat(55));
        inspect(1);
        inspect(6);
        inspect(7);
        inspect(28);
        inspect(153);
        inspect(370);
        inspect(496);  // bonus — another perfect number
    }
}
```

```
Output:
Num  → Prime        | Perfect       | Armstrong
───────────────────────────────────────────────────────
1    → Not Prime    | Not Perfect   | Not Armstrong
6    → Not Prime    | Perfect ✅    | Not Armstrong
7    → Prime ✅     | Not Perfect   | Not Armstrong
28   → Not Prime    | Perfect ✅    | Not Armstrong
153  → Not Prime    | Not Perfect   | Armstrong ✅
370  → Not Prime    | Not Perfect   | Armstrong ✅
496  → Not Prime    | Perfect ✅    | Not Armstrong
```

**Perfect numbers verified:**
```
6:   1+2+3 = 6 ✅
28:  1+2+4+7+14 = 28 ✅
496: 1+2+4+8+16+31+62+124+248 = 496 ✅
```

> 🔑 **Key Learning:** This is your first taste of **clean code architecture**:
> - `isPrime`, `isPerfect`, `isArmstrong` — each does ONE thing
> - `inspect` is a **coordinator** — calls others, formats output, contains no math
> - Each method is independently testable
> - If `isPrime` logic changes, only that method changes — `inspect` needs no update
>
> This is exactly how DSA solutions are structured in interviews: helper methods + one main method that orchestrates. The `isPerfect` optimization using `i * i <= n` finds BOTH divisors `i` and `n/i` in one iteration — same √n trick from prime checking, applied to divisor finding.

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| Hollow rectangle from memory (rows=4, cols=6) | ✅ Border condition correct |
| Full pyramid — spaces + stars formula | ✅ `n-i` spaces, `2i-1` stars |
| Explained `n - i + 1` in inverted triangle | ✅ See note below |
| Floyd's Triangle from memory | ✅ Counter declared outside both loops |

**Revision Note — Why `n - i + 1` and not `n - i`?**
```
For n=5, row 1 should have 5 stars (full row):
n - i     = 5 - 1 = 4  ❌ (one short!)
n - i + 1 = 5 - 1 + 1 = 5  ✅

The +1 corrects the off-by-one error.
When i=n (last row): n - n + 1 = 1 ✅ (correctly gives 1 star)
```

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. Parameter vs argument — with example?</b></summary>

> **Parameter** — the variable name declared in the method signature (a placeholder).
> Example: `static int add(int x, int y)` — `x` and `y` are parameters.
>
> **Argument** — the actual value passed when calling the method.
> Example: `add(10, 20)` — `10` and `20` are arguments.
>
> Memory trick: **P**arameter = **P**laceholder. **A**rgument = **A**ctual value.

</details>

<details>
<summary><b>Q2. Java is pass-by-value for primitives — what exactly does this mean?</b></summary>

> When you call a method with a primitive variable, Java creates a **complete copy** of that variable's value and passes the copy to the method. The method works entirely on this copy. Any changes made inside the method to the parameter affect only the copy — the original variable in the calling code is never touched. When the method returns, the copy is discarded. This is why swapping two primitives inside a method doesn't affect the originals — you're swapping copies, not the originals themselves.

</details>

<details>
<summary><b>Q3. What is method overloading? Can two methods differ only in return type?</b></summary>

> **Method overloading** = multiple methods with the same name but different parameter signatures (different number, types, or order of parameters). Java picks the correct version at compile time based on what arguments you pass.
>
> **No — return type alone is NOT enough.** Java uses the method signature (name + parameter list) to distinguish methods. If two methods have the same name and same parameters but different return types, the compiler cannot determine which to call — it's a compile-time error.

</details>

<details>
<summary><b>Q4. What is variable scope? Where is `int x = 5` inside a for loop accessible?</b></summary>

> **Variable scope** = the region of code where a variable is visible and can be used. A variable's scope is limited to the `{ }` block in which it was declared.
>
> `int x = 5` declared inside a `for` loop is accessible ONLY within that loop's `{ }` block — including the loop body and any nested blocks inside it. It cannot be accessed before the loop starts, after the loop ends, or in any other method. Attempting to use it outside causes a compile-time error.

</details>

<details>
<summary><b>Q5. Why write isPrime() as a separate method instead of inline in main()? 3 reasons.</b></summary>

> **1. Reusability** — Once written, `isPrime(n)` can be called from `printPrimes()`, `inspect()`, or any future method without rewriting the logic.
>
> **2. Readability** — `if (isPrime(i))` reads like English. A full for-loop inline inside another loop makes the code dense and harder to understand.
>
> **3. Maintainability** — If the prime algorithm needs to change (e.g., optimized differently), you fix it in ONE place. All callers automatically benefit. With inline logic, you'd have to find and fix every copy.

</details>

---

## 📌 DAY 5 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 5 — SUMMARY                        ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 03, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Functions & Methods                             ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  Method anatomy — all 6 parts explained                      ║
║  ✅  4 types: void, return-type, no-param, multi-param           ║
║  ✅  Call stack — how method calls execute and return            ║
║  ✅  Parameters (placeholder) vs Arguments (actual values)       ║
║  ✅  Pass-by-value — primitives always copied, never modified    ║
║  ✅  Variable scope — lives and dies within its { } block        ║
║  ✅  Method overloading — same name, different signatures        ║
║  ✅  Return statement rules — early return, multiple returns     ║
║  ✅  Method best practices — SRP, naming, size                   ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  90%                              ║
║  STRONG AREA    :  Method design, overloading, clean structure   ║
║  WEAK AREA      :  Pass-by-value mental model (needs practice)  ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  One method = one responsibility — always                    ║
║  ▸  Primitives are COPIES in methods — originals untouched      ║
║  ▸  Overloading = same name, different params (not return type) ║
║  ▸  Scope = lives and dies within its { } block                 ║
║  ▸  Boolean methods → isXxx() naming convention                 ║
║  ▸  Method calling method = clean, testable, reusable code      ║
║  ▸  long over int for factorials — silent overflow is dangerous ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 04](./Day04_Loops_Patterns.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 06 →](./Day06_Arrays.md)

*Day 5 of 160 — Done ✅ | Streak: 5 Days 🔥🔥🔥🔥🔥*

</div>
