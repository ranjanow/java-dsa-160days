# 📄 Day 02 — Conditionals & Decision Making

<div align="center">

![Day](https://img.shields.io/badge/Day-02_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Conditionals_%26_Decision_Making-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 01](./Day01_Variables_DataTypes.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 03 →](./Day03_Loops.md)

</div>

---

## 🎯 Day 2 Goal
> Master all forms of conditional statements in Java — `if`, `if-else`, `if-else-if` ladder, nested `if`, `switch-case`, and the ternary operator. By end of today, look at any real-world decision problem and immediately convert it into code logic.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | Concepts — if, if-else, if-else-if ladder | 60 min |
| Block 2 | Concepts — Nested if, switch-case, ternary | 45 min |
| Block 3 | Quick revision — Day 1 operators | 15 min |
| Block 4 | Practice Set — 6 Problems | 120 min |
| Block 5 | Mini Challenge + Revision Tasks | 40 min |
| Block 6 | Reflection Questions | 20 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. The `if` Statement

Executes a block **only if** condition is `true`. Completely skipped if false.

```java
int age = 20;
if (age >= 18) {
    System.out.println("You are an adult.");
}
// age=20 → "You are an adult."
// age=15 → nothing prints
```

> 💡 The condition inside `if()` must always evaluate to a **boolean**.

---

### 🔷 2. The `if-else` Statement

Two mutually exclusive paths. **Exactly one** block always runs.

```java
int marks = 45;
if (marks >= 50) {
    System.out.println("Pass");
} else {
    System.out.println("Fail");
}
// Output: Fail
```

> 💡 Both blocks can NEVER run simultaneously for the same input.

---

### 🔷 3. The `if-else-if` Ladder

For more than 2 outcomes. Java checks **top to bottom — first true condition wins**, rest skipped.

```java
int marks = 72;
if      (marks >= 90) { System.out.println("Grade: A"); }
else if (marks >= 75) { System.out.println("Grade: B"); }
else if (marks >= 60) { System.out.println("Grade: C"); }  // ← runs for 72
else if (marks >= 40) { System.out.println("Grade: D"); }
else                  { System.out.println("Grade: F"); }
// Output: Grade: C
```

> ⚠️ **Order matters critically!** Java stops at the FIRST true condition. Wrong order = wrong results.

---

### 🔷 4. Nested `if` Statements

An `if` inside another `if`. Inner condition only checked when outer is true.

```java
int age = 20;
boolean hasID = true;

if (age >= 18) {
    if (hasID) {
        System.out.println("Entry allowed.");
    } else {
        System.out.println("Bring your ID.");
    }
} else {
    System.out.println("You are underage.");
}
// age=20, hasID=true  → "Entry allowed."
// age=20, hasID=false → "Bring your ID."
// age=16              → "You are underage."
```

> ⚠️ Avoid nesting deeper than 2–3 levels — hard to read and debug.

---

### 🔷 5. The `switch-case` Statement

Best when **one variable** matches multiple **specific discrete values**.

```java
int day = 3;
switch (day) {
    case 1: System.out.println("Monday");    break;
    case 2: System.out.println("Tuesday");   break;
    case 3: System.out.println("Wednesday"); break;  // ← runs
    case 4: System.out.println("Thursday");  break;
    case 5: System.out.println("Friday");    break;
    default: System.out.println("Weekend");
}
// Output: Wednesday
```

**Fall-Through — What happens without `break`:**
```java
int x = 2;
switch (x) {
    case 2: System.out.println("Two");      // runs
    case 3: System.out.println("Three");    // also runs! (falls through)
    default: System.out.println("Default"); // also runs!
}
// Output: Two  Three  Default
```

**Intentional fall-through (useful grouping):**
```java
switch (day) {
    case 1: case 2: case 3: case 4: case 5:
        System.out.println("Weekday"); break;
    case 6: case 7:
        System.out.println("Weekend"); break;
}
```

**Valid switch types:** `int`, `byte`, `short`, `char`, `String` (Java 7+), `enum`
**Invalid:** `boolean`, `float`, `double`

---

### 🔷 6. Ternary Operator

One-line compact `if-else` that returns a value.

```java
// Syntax: variable = (condition) ? valueIfTrue : valueIfFalse;

int a = 10, b = 20;
int max = (a > b) ? a : b;
System.out.println("Max = " + max);   // Max = 20

String result = (marks >= 50) ? "Pass" : "Fail";
int sign = (x > 0) ? 1 : -1;
```

> ✅ Use for simple single-value decisions.
> ❌ Avoid chaining ternary inside ternary — becomes unreadable.

---

### 🔷 7. String Comparison — Critical Rule

```java
String s1 = "hello";
String s2 = new String("hello");

// ❌ WRONG — compares memory addresses
System.out.println(s1 == s2);              // false (different objects!)

// ✅ CORRECT — always use .equals()
System.out.println(s1.equals(s2));         // true

// Case-insensitive
System.out.println(s1.equalsIgnoreCase("HELLO")); // true
```

> ⚠️ **Classic interview trap.** `==` on Strings compares references, not content. **Always use `.equals()`**.

---

### 🔷 8. if-else-if vs switch — Decision Guide

| Situation | Best Choice |
|-----------|-------------|
| Range-based conditions (`marks >= 90`) | `if-else-if` |
| Complex boolean expressions (`&&`, `\|\|`) | `if-else-if` |
| One variable vs many exact values | `switch-case` |
| More than ~5 discrete values | `switch-case` (cleaner) |
| `char` or `String` exact matching | `switch-case` |

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Positive, Negative, or Zero `[Easy]`

**Task:** Print whether `n` is Positive, Negative, or Zero.

```java
public class P1_PositiveNegativeZero {
    public static void main(String[] args) {
        int n = -3;
        if      (n > 0) { System.out.println(n + " is Positive"); }
        else if (n < 0) { System.out.println(n + " is Negative"); }
        else            { System.out.println("It is Zero");        }
    }
}
```

```
n=7  → 7 is Positive
n=-3 → -3 is Negative
n=0  → It is Zero
```

> 🔑 Three mutually exclusive conditions need `if-else-if`. Zero is special — neither positive nor negative. `else` covers exactly one remaining case.

---

### ✅ Problem 2 — Largest of Three Numbers `[Easy-Medium]`

**Task:** Find the largest among `a = 12, b = 45, c = 23`.

```java
public class P2_LargestOfThree {
    public static void main(String[] args) {
        int a = 12, b = 45, c = 23;
        int largest;

        if      (a >= b && a >= c) { largest = a; }
        else if (b >= a && b >= c) { largest = b; }
        else                       { largest = c; }

        System.out.println("Largest = " + largest);
    }
}
```

```
Output: Largest = 45
Equal test: a=5, b=5, c=3 → Largest = 5 ✅
```

> 🔑 Use `>=` not `>` to handle equal values. The final `else` implicitly handles `c` — if neither `a` nor `b` is largest, `c` must be.

---

### ✅ Problem 3 — Grade Calculator `[Easy-Medium]`

**Task:** Print grade for marks 0–100. Handle invalid input first.

```java
public class P3_GradeCalculator {
    public static void main(String[] args) {
        int marks = 88;

        if      (marks < 0 || marks > 100) { System.out.println("Invalid marks!"); }
        else if (marks >= 90) { System.out.println("Grade: A+ — Outstanding");   }
        else if (marks >= 75) { System.out.println("Grade: A  — Excellent");     }
        else if (marks >= 60) { System.out.println("Grade: B  — Good");          }
        else if (marks >= 45) { System.out.println("Grade: C  — Average");       }
        else if (marks >= 33) { System.out.println("Grade: D  — Below Average"); }
        else                  { System.out.println("Grade: F  — Fail");          }
    }
}
```

```
marks=88  → Grade: A  — Excellent
marks=32  → Grade: F  — Fail
marks=105 → Invalid marks!
```

> 🔑 **Always validate input FIRST** — fail fast principle. Conditions go highest → lowest; reversing would break the logic entirely.

---

### ✅ Problem 4 — Calculator with switch `[Medium]`

**Task:** Perform arithmetic on `a = 20, b = 4` with `char` operator. Handle division by zero.

```java
public class P4_SimpleCalculatorSwitch {
    public static void main(String[] args) {
        int a = 20, b = 4;
        char op = '/';

        switch (op) {
            case '+': System.out.println(a + " + " + b + " = " + (a + b)); break;
            case '-': System.out.println(a + " - " + b + " = " + (a - b)); break;
            case '*': System.out.println(a + " * " + b + " = " + (a * b)); break;
            case '/':
                if (b == 0) System.out.println("Error: Division by zero!");
                else System.out.println(a + " / " + b + " = " + ((double)a / b));
                break;
            case '%':
                if (b == 0) System.out.println("Error: Modulo by zero!");
                else System.out.println(a + " % " + b + " = " + (a % b));
                break;
            default: System.out.println("Invalid operator: " + op);
        }
    }
}
```

```
op='/'  → 20 / 4 = 5.0
op='+'  → 20 + 4 = 24
b=0     → Error: Division by zero!
op='z'  → Invalid operator: z
```

> 🔑 `switch` works perfectly with `char`. Nested `if` inside a `case` for b==0 check is valid and clean.

---

### ✅ Problem 5 — Leap Year Checker `[Medium]`

**Task:** Determine if a year is a leap year using 3 rules.

```java
public class P5_LeapYear {
    public static void main(String[] args) {
        int year = 2024;
        boolean isLeap;

        // Step-by-step (readable version)
        if      (year % 400 == 0) { isLeap = true;  }  // div by 400 → LEAP
        else if (year % 100 == 0) { isLeap = false; }  // div by 100, not 400 → NOT leap
        else if (year % 4   == 0) { isLeap = true;  }  // div by 4, not 100 → LEAP
        else                      { isLeap = false; }  // none → NOT leap

        System.out.println(year + " → " + (isLeap ? "Leap Year ✅" : "Not a Leap Year ❌"));

        // One-liner using logical operators
        boolean quick = (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
        System.out.println("One-liner: " + quick);
    }
}
```

```
2024 → Leap Year ✅       (div by 4, not 100)
2000 → Leap Year ✅       (div by 400)
1900 → Not a Leap Year ❌ (div by 100, not 400)
2023 → Not a Leap Year ❌ (not div by 4)
```

**Decision flow:**
```
year % 400 == 0? → YES → Leap Year
       ↓ NO
year % 100 == 0? → YES → NOT Leap Year
       ↓ NO
year % 4   == 0? → YES → Leap Year
       ↓ NO
                         NOT Leap Year
```

> 🔑 **Order is critical** — `% 400` MUST come before `% 100`. Classic interview question.

---

### ✅ Problem 6 — Triangle Validity + Classifier `[Medium-Hard]`

**Task:** Check validity then classify (Equilateral / Isosceles / Scalene / Right-Angled).

```java
public class P6_TriangleClassifier {
    public static void main(String[] args) {
        int a = 3, b = 4, c = 5;

        // Step 1: Validate
        if (a + b <= c || a + c <= b || b + c <= a) {
            System.out.println("Invalid Triangle ❌");
            return;  // early exit
        }
        System.out.println("Valid Triangle ✅ — Sides: " + a + ", " + b + ", " + c);

        // Step 2: Find largest side for right-angle check
        int max = Math.max(a, Math.max(b, c));
        int s1 = (max == a) ? b : a;
        int s2 = (max == c) ? b : c;

        // Step 3: Check right-angled (Pythagorean theorem)
        if (s1 * s1 + s2 * s2 == max * max) {
            System.out.println("Type: Right-Angled 📐");
        }

        // Step 4: Classify by side equality
        if      (a == b && b == c)          { System.out.println("Classification: Equilateral 🔺"); }
        else if (a == b || b == c || a == c){ System.out.println("Classification: Isosceles");       }
        else                                 { System.out.println("Classification: Scalene");         }
    }
}
```

```
3,3,3  → Valid ✅ | Equilateral 🔺
5,5,8  → Valid ✅ | Isosceles
3,4,5  → Valid ✅ | Right-Angled 📐 | Scalene
1,2,10 → Invalid Triangle ❌
```

> 🔑 Break complex problems into clear steps: Validate → Setup → Special check → General check. A triangle CAN be both Scalene and Right-Angled (3-4-5). `return` after invalid = clean early exit.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Traffic Light System — case-insensitive switch.

```java
public class MiniChallenge_Day2 {
    public static void main(String[] args) {
        String color = "red";  // test: RED, yellow, Green, blue

        switch (color.toUpperCase()) {  // .toUpperCase() makes it case-insensitive
            case "RED":    System.out.println("🔴 STOP — Do not cross");          break;
            case "YELLOW": System.out.println("🟡 SLOW DOWN — Prepare to stop");  break;
            case "GREEN":  System.out.println("🟢 GO — Safe to cross");           break;
            default:       System.out.println("⚠️  INVALID SIGNAL");
        }
    }
}
```

```
"red"    → 🔴 STOP — Do not cross       (toUpperCase → "RED" matches!)
"YELLOW" → 🟡 SLOW DOWN — Prepare to stop
"Green"  → 🟢 GO — Safe to cross
"blue"   → ⚠️  INVALID SIGNAL
```

> 🔑 `.toUpperCase()` before `switch` = instant case-insensitive matching. Professional pattern used in real applications.

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| Wrote grade calculator from memory | ✅ All conditions correct |
| Wrote traffic light switch from memory | ✅ |
| Demonstrated intentional fall-through | ✅ Weekday/weekend grouping |
| Predicted `5+3+"Java"` vs `"Java"+5+3` | ✅ See discovery below |

**Revision Discovery — String + int left-to-right evaluation:**
```java
System.out.println(5 + 3 + "Java");   // → "8Java"
// 5+3 = 8 (both int), then 8+"Java" = "8Java"

System.out.println("Java" + 5 + 3);   // → "Java53"
// "Java"+5 = "Java5" (string concat), then "Java5"+3 = "Java53"
```

> 🔑 Once `+` encounters a String, ALL subsequent `+` become string concatenation. Very common interview trap!

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. if-else-if vs switch — when to use which?</b></summary>

> **Use `if-else-if`** when: conditions involve ranges (`marks >= 90`), complex boolean expressions with `&&`/`||`, or comparing different variables.
> **Use `switch-case`** when: checking ONE variable against multiple exact discrete values. Cleaner and more readable for that use case.
> Rule of thumb: if you're writing `== someValue` for the same variable repeatedly → use `switch`.

</details>

<details>
<summary><b>Q2. What is fall-through in switch?</b></summary>

> Without `break`, execution falls into the next `case` automatically regardless of whether that case matches.
> **Bug:** unintended extra cases run.
> **Feature:** grouping cases — `case 1: case 2: case 3:` sharing one block + one `break`.

</details>

<details>
<summary><b>Q3. Why not use == for Strings?</b></summary>

> `==` compares **memory addresses** (references). Two String objects with identical content can sit at different memory locations — `==` returns `false` even when values match.
> Always use `.equals()` for content comparison, `.equalsIgnoreCase()` for case-insensitive.

</details>

<details>
<summary><b>Q4. What is the ternary operator?</b></summary>

> Compact one-line `if-else`: `condition ? valueIfTrue : valueIfFalse`
> Rewrite of `if (x > 0) { sign=1; } else { sign=-1; }` → `int sign = (x > 0) ? 1 : -1;`

</details>

<details>
<summary><b>Q5. In if-else-if, if two conditions are both true — which runs?</b></summary>

> The **FIRST** matching condition runs. All remaining are skipped. This is exactly why condition ordering matters. If `marks=95` and you put `marks>=40` before `marks>=90`, it prints Grade D and never reaches Grade A.

</details>

---

## 📌 DAY 2 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 2 — SUMMARY                        ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  March 31, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Conditionals & Decision Making                  ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  if — single-path decision                                   ║
║  ✅  if-else — two-path mutually exclusive                       ║
║  ✅  if-else-if ladder — multi-path, first-match wins            ║
║  ✅  Nested if — decisions within decisions                      ║
║  ✅  switch-case — discrete value matching + fall-through        ║
║  ✅  Ternary operator — compact one-line if-else                 ║
║  ✅  String comparison — .equals() vs == (critical!)             ║
║  ✅  if-else-if vs switch — decision guide                       ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  90%                              ║
║  STRONG AREA    :  if-else-if logic, leap year reasoning         ║
║  WEAK AREA      :  Knowing when switch is cleaner than if-else   ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  Condition ORDER in if-else-if is everything                 ║
║  ▸  Always validate input FIRST — fail fast principle           ║
║  ▸  NEVER use == for Strings — always .equals()                 ║
║  ▸  switch + .toUpperCase() = case-insensitive matching          ║
║  ▸  "Java"+5+3 = "Java53" — left-to-right String concat trap    ║
║  ▸  Ternary: simple decisions only — never chain them           ║
╚══════════════════════════════════════════════════════════════════╝
```
---

<div align="center">

[← Day 01](./Day01_Variables_DataTypes.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 03 →](./Day03_Loops.md)

*Day 2 of 160 — Done ✅ | Streak: 2 Days 🔥*

</div>
