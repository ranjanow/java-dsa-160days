# 📄 Day 03 — Loops & Iteration

<div align="center">

![Day](https://img.shields.io/badge/Day-03_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Loops_%26_Iteration-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 02](./Day02_Conditionals.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 04 →](./Day04_Loops_Patterns.md)

</div>

---

## 🎯 Day 3 Goal
> Master all three loop types — `for`, `while`, `do-while`. Control them with `break` and `continue`. Build the **loop-based problem solving instinct** that every single DSA algorithm depends on.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | `for` loop — anatomy, tracing, nested loops | 55 min |
| Block 2 | `while`, `do-while`, `break`, `continue` | 45 min |
| Block 3 | Revision — Day 1 + Day 2 flash review | 15 min |
| Block 4 | Practice Set — 6 Problems | 120 min |
| Block 5 | Mini Challenge + Revision Tasks | 35 min |
| Block 6 | Reflection Questions | 10 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. Why Loops Exist

Without loops — printing 1 to 100 needs 100 lines.
With loops — 2 lines do the same job.

```java
// Without loop — repetitive and unscalable
System.out.println(1);
System.out.println(2);
// ... 98 more lines

// With loop — clean, scalable, powerful
for (int i = 1; i <= 100; i++) {
    System.out.println(i);
}
```

> 💡 A loop = **repeat a block of code** without rewriting it. This is the foundation of all algorithms.

---

### 🔷 2. The `for` Loop — When Count is Known

Best used when you know **exactly how many times** to repeat.

```java
// Syntax — 3 parts inside ()
for (initialization ; condition ; update) {
    // body — runs while condition is true
}
```

| Part | When It Runs | Purpose |
|------|-------------|---------|
| `initialization` | Once at the very start | Set up loop variable |
| `condition` | Before EVERY iteration | If false → loop ends |
| `update` | After EVERY iteration body | Move toward exit |

```java
// Print 1 to 5
for (int i = 1; i <= 5; i++) {
    System.out.print(i + " ");
}
// Output: 1 2 3 4 5
```

**Manual trace — always do this on paper:**
```
i=1 → 1<=5 ✅ → print 1 → i becomes 2
i=2 → 2<=5 ✅ → print 2 → i becomes 3
i=3 → 3<=5 ✅ → print 3 → i becomes 4
i=4 → 4<=5 ✅ → print 4 → i becomes 5
i=5 → 5<=5 ✅ → print 5 → i becomes 6
i=6 → 6<=5 ❌ → LOOP ENDS
```

**Variations:**
```java
// Count down
for (int i = 5; i >= 1; i--) {
    System.out.print(i + " ");
}
// Output: 5 4 3 2 1

// Custom step
for (int i = 0; i <= 20; i += 5) {
    System.out.print(i + " ");
}
// Output: 0 5 10 15 20

// Even numbers only
for (int i = 2; i <= 10; i += 2) {
    System.out.print(i + " ");
}
// Output: 2 4 6 8 10
```

---

### 🔷 3. Nested `for` Loops

A loop **inside** another loop. The inner loop runs **completely** for every single outer loop iteration.

```java
for (int i = 1; i <= 3; i++) {           // outer — controls rows
    for (int j = 1; j <= 3; j++) {       // inner — controls columns
        System.out.print("(" + i + "," + j + ") ");
    }
    System.out.println();                 // new line after each row
}
```

```
Output:
(1,1) (1,2) (1,3)
(2,1) (2,2) (2,3)
(3,1) (3,2) (3,3)
```

**Execution count:**
```
Outer i=1 → inner runs 3 times (j=1,2,3)
Outer i=2 → inner runs 3 times (j=1,2,3)
Outer i=3 → inner runs 3 times (j=1,2,3)
Total     = 3 × 3 = 9 iterations
```

> 💡 **First O(n²) Encounter:** If both loops run `n` times → `n × n = n²` total iterations. This is why nested loops are O(n²) — the foundation of Bubble Sort, Selection Sort, and matrix operations in DSA.

---

### 🔷 4. The `while` Loop — When Count is Unknown

Use when you **don't know in advance** how many iterations are needed.

```java
// Syntax
while (condition) {
    // body
    // MUST have something that eventually makes condition false!
}
```

```java
int i = 1;
while (i <= 5) {
    System.out.print(i + " ");
    i++;      // ← CRITICAL — without this → infinite loop!
}
// Output: 1 2 3 4 5
```

**Real power — count digits (unknown iterations):**
```java
int n = 12345, count = 0;
while (n > 0) {
    n = n / 10;    // 12345→1234→123→12→1→0
    count++;
}
System.out.println("Digits: " + count);
// Output: Digits: 5
```

**Trace:**
```
n=12345, count=0 → n=1234,  count=1
n=1234,  count=1 → n=123,   count=2
n=123,   count=2 → n=12,    count=3
n=12,    count=3 → n=1,     count=4
n=1,     count=4 → n=0,     count=5
n=0 → 0>0 ❌ → LOOP ENDS → count=5 ✅
```

> ⚠️ **Infinite Loop:** If condition never becomes `false` → program runs forever. Always ensure something **inside the loop** changes the condition variable toward `false`.

---

### 🔷 5. The `do-while` Loop — Runs At Least Once

Condition is checked **AFTER** the body — body always executes at **minimum once**.

```java
// Syntax
do {
    // body — runs AT LEAST ONCE guaranteed
} while (condition);    // ← semicolon is mandatory!
```

```java
int i = 1;
do {
    System.out.print(i + " ");
    i++;
} while (i <= 5);
// Output: 1 2 3 4 5
```

**The defining difference — condition false from start:**
```java
int x = 10;

// while — body NEVER runs (condition false immediately)
while (x < 5) {
    System.out.println("while runs");     // never prints
}

// do-while — body runs ONCE before checking condition
do {
    System.out.println("do-while runs");  // prints once: "do-while runs"
} while (x < 5);
```

> 💡 **Best use cases:** Menu systems (show at least once), input validation (ask at least once).

---

### 🔷 6. `break` — Exit Loop Immediately

Terminates the loop instantly, jumps to first line **after** the loop.

```java
// Find first multiple of 7 between 1 and 100
for (int i = 1; i <= 100; i++) {
    if (i % 7 == 0) {
        System.out.println("First multiple of 7: " + i);
        break;    // found it — stop searching
    }
}
// Output: First multiple of 7: 7
```

> ⚠️ **In nested loops:** `break` exits only the **innermost** loop. The outer loop continues normally.

---

### 🔷 7. `continue` — Skip Current Iteration

Skips the rest of the **current iteration**, jumps to the **next** one. Does NOT exit the loop.

```java
// Print 1 to 10, skip multiples of 3
for (int i = 1; i <= 10; i++) {
    if (i % 3 == 0) continue;    // skip 3, 6, 9
    System.out.print(i + " ");
}
// Output: 1 2 4 5 7 8 10
```

**`break` vs `continue` — the clear distinction:**

| | `break` | `continue` |
|--|---------|------------|
| What it does | **Exits** loop entirely | **Skips** current iteration only |
| Loop after | Loop is DONE | Loop CONTINUES with next iteration |
| Use case | Found what you needed | Skip specific unwanted values |

---

### 🔷 8. Loop Comparison Table

| Feature | `for` | `while` | `do-while` |
|---------|-------|---------|------------|
| Use when | Count **known** | Count **unknown** | Must run **≥ 1 time** |
| Condition check | Before each iteration | Before each iteration | **After** each iteration |
| Min executions | **0** | **0** | **1** |
| Counter location | Inside `()` | Outside, before loop | Outside, before loop |
| Best for | Arrays, fixed ranges | Searching, digit ops | Menus, validation |

---

### 🔷 9. Essential Loop Patterns — Know These Cold

```java
// ① Sum of 1 to N
int sum = 0;
for (int i = 1; i <= n; i++) sum += i;

// ② Factorial of N
int fact = 1;
for (int i = 1; i <= n; i++) fact *= i;

// ③ Reverse a Number (pure arithmetic — NO String!)
int rev = 0, temp = n;
while (temp > 0) {
    rev  = rev * 10 + temp % 10;   // extract last digit, build rev
    temp = temp / 10;              // remove last digit
}

// ④ Count Digits
int count = 0, temp = n;
while (temp > 0) { temp /= 10; count++; }

// ⑤ Sum of Digits
int digitSum = 0, temp = n;
while (temp > 0) { digitSum += temp % 10; temp /= 10; }

// ⑥ Print divisors in range
for (int i = 1; i <= n; i++) {
    if (i % k == 0) System.out.print(i + " ");
}
```

> 🧠 These 6 patterns appear in almost every DSA number problem. Know them without thinking.

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Multiplication Table `[Easy]`

**Task:** Print multiplication table of `n = 7`, from 1 to 10.

```java
public class P1_MultiplicationTable {
    public static void main(String[] args) {
        int n = 7;
        for (int i = 1; i <= 10; i++) {
            System.out.printf("%d x %2d = %3d%n", n, i, n * i);
        }
    }
}
```

```
Output:
7 x  1 =   7
7 x  2 =  14
7 x  3 =  21
7 x  4 =  28
7 x  5 =  35
7 x  6 =  42
7 x  7 =  49
7 x  8 =  56
7 x  9 =  63
7 x 10 =  70
```

> 🔑 `printf` with `%2d` and `%3d` pads numbers for neat column alignment. `for` is perfect here — exactly 10 iterations, count is known in advance.

---

### ✅ Problem 2 — Sum of Digits `[Easy-Medium]`

**Task:** Find the sum of digits of `n = 9876`.

```java
public class P2_SumOfDigits {
    public static void main(String[] args) {
        int n = 9876;
        int original = n;
        int sum = 0;

        while (n > 0) {
            sum += n % 10;   // extract last digit and add
            n   /= 10;       // remove last digit
        }

        System.out.println("Sum of digits of " + original + " = " + sum);
    }
}
```

```
Trace:
n=9876 → digit=6, sum=6,  n=987
n=987  → digit=7, sum=13, n=98
n=98   → digit=8, sum=21, n=9
n=9    → digit=9, sum=30, n=0
n=0    → loop ends ✅

Output: Sum of digits of 9876 = 30
```

> 🔑 `n % 10` extracts the **last digit**. `n /= 10` removes it. `while` is chosen because we don't know digit count in advance — loop runs until `n` becomes 0.

---

### ✅ Problem 3 — Reverse a Number `[Easy-Medium]`

**Task:** Reverse `n = 12345` to get `54321`. Pure arithmetic only.

```java
public class P3_ReverseNumber {
    public static void main(String[] args) {
        int n = 12345;
        int original = n;
        int rev = 0;

        while (n > 0) {
            int lastDigit = n % 10;           // extract last digit
            rev = rev * 10 + lastDigit;       // shift rev left, append digit
            n   = n / 10;                     // remove last digit
        }

        System.out.println("Reverse of " + original + " = " + rev);
    }
}
```

```
Trace:
n=12345, rev=0    → digit=5, rev=5,     n=1234
n=1234,  rev=5    → digit=4, rev=54,    n=123
n=123,   rev=54   → digit=3, rev=543,   n=12
n=12,    rev=543  → digit=2, rev=5432,  n=1
n=1,     rev=5432 → digit=1, rev=54321, n=0
n=0 → loop ends ✅

Output: Reverse of 12345 = 54321
```

> 🔑 `rev = rev * 10 + digit` — multiplying by 10 **shifts existing digits left** (makes room), then adding the new digit **appends on the right**. Standard mathematical reverse technique used in many interview problems.

---

### ✅ Problem 4 — Prime Number Check `[Medium]`

**Task:** Check if `n = 29` is prime. Optimized with `√n` upper bound.

```java
public class P4_PrimeCheck {
    public static void main(String[] args) {
        int n = 29;

        if (n <= 1) {
            System.out.println(n + " is NOT prime");
            return;
        }

        boolean isPrime = true;

        // Only check divisors up to √n — key optimization!
        for (int i = 2; i * i <= n; i++) {
            if (n % i == 0) {
                isPrime = false;
                break;   // found a factor — stop immediately
            }
        }

        System.out.println(n + (isPrime ? " is Prime ✅" : " is NOT Prime ❌"));
    }
}
```

```
n=29  → Prime ✅
n=15  → NOT Prime ❌  (15 % 3 == 0)
n=1   → NOT prime
n=2   → Prime ✅
```

**Why `i * i <= n` instead of `i < n`?**
```
Factors come in PAIRS: if a × b = n, one of them is ≤ √n.
Example: 36 = 2×18 = 3×12 = 4×9 = 6×6
         √36 = 6 → checking up to 6 finds ALL factors!

For n=29: √29 ≈ 5.38 → only check i=2,3,4,5
  29%2≠0, 29%3≠0, 29%4≠0, 29%5≠0 → Prime!

Brute force n=1,000,000: checks ~1,000,000 numbers
√n optimized: checks only ~1,000 numbers
```

> 🔑 **First real optimization mindset.** Brute force vs optimized — this thinking is the core of DSA. Reducing from O(n) to O(√n) is a massive improvement.

---

### ✅ Problem 5 — Fibonacci Series `[Medium]`

**Task:** Print first `n = 10` terms of Fibonacci series.

```java
public class P5_Fibonacci {
    public static void main(String[] args) {
        int n  = 10;
        int a  = 0, b = 1;    // first two terms

        System.out.print("Fibonacci (" + n + " terms): ");

        for (int i = 1; i <= n; i++) {
            System.out.print(a);
            if (i < n) System.out.print(", ");

            int next = a + b;   // calculate next term
            a = b;              // shift forward
            b = next;           // shift forward
        }
        System.out.println();
    }
}
```

```
Output:
Fibonacci (10 terms): 0, 1, 1, 2, 3, 5, 8, 13, 21, 34
```

**Variable trace — first 5 iterations:**
```
i=1: print 0, next=0+1=1,  a=1,  b=1
i=2: print 1, next=1+1=2,  a=1,  b=2
i=3: print 1, next=1+2=3,  a=2,  b=3
i=4: print 2, next=2+3=5,  a=3,  b=5
i=5: print 3, next=3+5=8,  a=5,  b=8
```

> 🔑 Only **3 variables** needed. The `a = b` BEFORE `b = next` order is critical — reverse it and everything breaks. This sliding window of two values reappears directly in Dynamic Programming (DP) optimizations later in the journey.

---

### ✅ Problem 6 — Number Pattern `[Medium-Hard]`

**Task:** Print ascending and descending number triangles for `n = 5`.

```java
public class P6_NumberPattern {
    public static void main(String[] args) {
        int n = 5;

        // Pattern 1 — Ascending triangle
        System.out.println("Pattern 1:");
        for (int i = 1; i <= n; i++) {         // row goes 1 to n
            for (int j = 1; j <= i; j++) {     // column goes 1 to current row
                System.out.print(j + " ");
            }
            System.out.println();
        }

        System.out.println();

        // Pattern 2 — Descending triangle
        System.out.println("Pattern 2:");
        for (int i = n; i >= 1; i--) {         // row counts DOWN from n to 1
            for (int j = 1; j <= i; j++) {     // column still goes 1 to i
                System.out.print(j + " ");
            }
            System.out.println();
        }
    }
}
```

```
Pattern 1:
1
1 2
1 2 3
1 2 3 4
1 2 3 4 5

Pattern 2:
1 2 3 4 5
1 2 3 4
1 2 3
1 2
1
```

**Row-Column Relationship:**
```
Pattern 1:
Row 1 (i=1) → inner j: 1 to 1 → prints 1 number
Row 2 (i=2) → inner j: 1 to 2 → prints 2 numbers
Row 3 (i=3) → inner j: 1 to 3 → prints 3 numbers
RULE: Row i → inner runs from 1 to i ✅

Pattern 2:
Row 1 (i=5) → inner j: 1 to 5 → prints 5 numbers
Row 2 (i=4) → inner j: 1 to 4 → prints 4 numbers
RULE: Outer counts DOWN (i=n to 1), inner still 1 to i ✅
```

> 🔑 The golden rule of patterns: **find the math relationship between row `i` and column count**. Once you see it, the inner loop writes itself. This exact thinking is used for 2D arrays, Pascal's Triangle, and matrix diagonal traversal in DSA.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Number Inspector — Digit count + Armstrong + Palindrome for `n = 153`.

```java
public class MiniChallenge_Day3 {
    public static void main(String[] args) {
        int n = 153;
        int original = n;

        // Step 1: Count digits
        int digits = 0, temp = n;
        while (temp > 0) { temp /= 10; digits++; }

        // Step 2: Reverse the number
        int rev = 0; temp = n;
        while (temp > 0) {
            rev  = rev * 10 + temp % 10;
            temp = temp / 10;
        }

        // Step 3: Armstrong check
        int armSum = 0; temp = n;
        while (temp > 0) {
            int d = temp % 10;
            armSum += (int) Math.pow(d, digits);   // d^digits, cast double→int
            temp   /= 10;
        }

        // Step 4: Palindrome check
        boolean isArmstrong = (armSum  == original);
        boolean isPalindrome = (rev    == original);

        // Output
        System.out.println("Number    : " + original);
        System.out.println("Digits    : " + digits);
        System.out.println("Reversed  : " + rev);
        System.out.println("Armstrong : " + (isArmstrong  ? "✅ YES" : "❌ NO"));
        System.out.println("Palindrome: " + (isPalindrome ? "✅ YES" : "❌ NO"));
    }
}
```

```
── n = 153 ──────────────────────────────
Number    : 153
Digits    : 3
Reversed  : 351
Armstrong : ✅ YES  (1³+5³+3³ = 1+125+27 = 153)
Palindrome: ❌ NO   (153 ≠ 351)

── n = 121 ──────────────────────────────
Number    : 121
Digits    : 3
Reversed  : 121
Armstrong : ❌ NO   (1³+2³+1³ = 10 ≠ 121)
Palindrome: ✅ YES  (121 = 121)

── n = 370 ──────────────────────────────
Number    : 370
Digits    : 3
Reversed  : 73
Armstrong : ✅ YES  (3³+7³+0³ = 27+343+0 = 370)
Palindrome: ❌ NO   (370 ≠ 73)
```

> 🔑 Complex problem = small independent reusable steps. Each step reuses `% 10` + `/ 10`. `Math.pow()` returns `double` — cast to `int` for integer comparison. **This divide-and-conquer breakdown is exactly the mindset DSA demands.**

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| Grade calculator if-else-if from memory | ✅ All 6 grade bands correct |
| Leap year one-liner using `&&` and `\|\|` | ✅ `(y%4==0 && y%100!=0)\|\|(y%400==0)` |
| Intentional fall-through — weekday/weekend | ✅ Grouped cases correctly |
| `"Result: "+2+3` vs `"Result: "+(2+3)` | ✅ See discovery below |

**Revision Discovery:**
```java
System.out.println("Result: " + 2 + 3);      // → "Result: 23"
// "Result:" + 2 = "Result: 2" (string concat, left to right)
// "Result: 2" + 3 = "Result: 23" (still string concat)

System.out.println("Result: " + (2 + 3));    // → "Result: 5"
// (2+3) = 5 first — parentheses force arithmetic before concat
// "Result: " + 5 = "Result: 5"
```

> 🔑 Parentheses force arithmetic BEFORE string concatenation. Always use `()` when mixing `+` with Strings and numbers.

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. What are the 3 parts of a for loop? When does each run?</b></summary>

> **Initialization** — runs ONCE before the first iteration. Sets up the counter.
> **Condition** — checked BEFORE every iteration. If `false` → loop exits. If `true` → body runs.
> **Update** — runs AFTER every iteration body. Moves counter toward the exit.
> If condition is `false` on the very first check — the body never runs (0 executions possible).

</details>

<details>
<summary><b>Q2. Key difference between while and do-while? Real-life scenario?</b></summary>

> **`while`** — checks condition BEFORE body. Body may run 0 times if condition is initially false.
> **`do-while`** — checks condition AFTER body. Body ALWAYS runs at least once, guaranteed.
> **Real-life:** ATM machine. You always show the menu at least once. After user selects an option, you ask "Do another transaction?" — only then decide to repeat. The menu showing is guaranteed minimum once — perfect `do-while`.

</details>

<details>
<summary><b>Q3. break vs continue? Which loop does break exit in nested loops?</b></summary>

> **`break`** — exits the loop ENTIRELY. Jumps to first line after the closing `}` of the loop.
> **`continue`** — skips the CURRENT iteration only. Loop continues with the next iteration normally.
> **In nested loops:** `break` exits only the **innermost** loop it directly belongs to. Outer loop keeps running. To break an outer loop, use a boolean flag variable or a labeled break.

</details>

<details>
<summary><b>Q4. Why is nested loop complexity O(n²)?</b></summary>

> If outer runs `n` times and inner also runs `n` times → inner body executes `n × n = n²` times total.
> For `n=5`: 5 outer × 5 inner = **25 total iterations**.
> As `n` doubles, work quadruples — quadratic growth = O(n²).
> Bubble Sort, Selection Sort, and matrix problems are all O(n²) because of this exact structure.

</details>

<details>
<summary><b>Q5. For sum of digits — which loop is natural and why?</b></summary>

> **`while`** is most natural. You don't know the digit count in advance — it varies per number. The loop runs as long as `n > 0` (digits remain). A `for` loop would require a pre-count step. `while` handles unknown iteration counts elegantly — that is precisely its strength.

</details>

---

## 📌 DAY 3 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 3 — SUMMARY                        ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 01, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Loops & Iteration                               ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  for loop — anatomy, 3-part structure, tracing, variations   ║
║  ✅  Nested for loops — O(n²) first encounter                    ║
║  ✅  while loop — unknown count, digit operations                ║
║  ✅  do-while — guaranteed min 1 execution, menus               ║
║  ✅  break — exits loop immediately                              ║
║  ✅  continue — skips current iteration only                     ║
║  ✅  break vs continue — clear distinction table                 ║
║  ✅  6 essential loop patterns — sum, factorial, reverse, etc.   ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  90%                              ║
║  STRONG AREA    :  for loop tracing, digit patterns, prime opt   ║
║  WEAK AREA      :  Nested loop pattern row-column relationship   ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  n%10 extracts last digit, n/10 removes it — core pattern    ║
║  ▸  Prime check up to √n beats O(n) — first DSA optimization    ║
║  ▸  Nested loops → O(n²) — critical for complexity analysis     ║
║  ▸  do-while runs at least once — for menus and validation      ║
║  ▸  break exits; continue skips — fundamentally different!      ║
║  ▸  Complex problems = small independent reusable steps          ║
╚══════════════════════════════════════════════════════════════════╝
```
---

<div align="center">

[← Day 02](./Day02_Conditionals.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 04 →](./Day04_Loops_Patterns.md)

*Day 3 of 160 — Done ✅ | Streak: 3 Days 🔥🔥🔥*

</div>
