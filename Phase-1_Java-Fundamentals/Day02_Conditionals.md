# 📄 Day 02 — Conditionals & Decision Making

<div align="center">

![Day](https://img.shields.io/badge/Day-02_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Conditionals_%26_Decision_Making-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![W3S](https://img.shields.io/badge/W3Schools_Exercises-5_Sets_✅-04AA6D?style=for-the-badge&logo=w3schools&logoColor=white)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 01](./Day01_Variables_DataTypes.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 03 →](./Day03_Loops.md)

</div>

---

## 🎯 Day 2 Goal

> Master every form of conditional statement in Java — `if`, `if-else`, `if-else-if` ladder, nested `if`, `switch`, and the ternary operator. By the end of today, you should be able to look at any real-world decision problem and immediately translate it into working Java code logic.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | `if`, `if-else`, `if-else-if` ladder | 60 min |
| Block 2 | Nested `if`, `switch-case`, Ternary | 45 min |
| Block 3 | Quick Revision — Day 1 Operators | 15 min |
| Block 4 | Practice Set — 6 Problems | 120 min |
| Block 5 | W3Schools Exercises + Mini Challenge | 40 min |
| Block 6 | Reflection Questions | 20 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. The `if` Statement

The most basic decision tool. Runs a block of code **only when** a condition is `true`. If the condition is `false`, the block is skipped entirely.

```java
// Syntax
if (condition) {
    // runs only when condition is true
}
```

```java
// Example
int temperature = 35;
if (temperature > 30) {
    System.out.println("It is hot outside!");
}
// Output: It is hot outside!
// If temperature = 20 → nothing prints
```

> 💡 The condition inside `if()` must evaluate to a **boolean** — either `true` or `false`.

---

### 🔷 2. The `if-else` Statement

Provides **two paths** — one for `true`, one for `false`. Exactly one block always runs.

```java
// Syntax
if (condition) {
    // runs when true
} else {
    // runs when false
}
```

```java
// Example
int marks = 45;
if (marks >= 50) {
    System.out.println("Result: Pass ✅");
} else {
    System.out.println("Result: Fail ❌");
}
// Output: Result: Fail ❌
```

> 💡 Both blocks can **never run at the same time**. They are mutually exclusive.

---

### 🔷 3. The `if-else-if` Ladder

Used when you have **more than two possible outcomes**. Java scans conditions from top to bottom and executes only the **first matching one**. All others are skipped.

```java
// Syntax
if (condition1) {
    // ...
} else if (condition2) {
    // ...
} else if (condition3) {
    // ...
} else {
    // default — runs if nothing above matched
}
```

```java
// Example — Grade Calculator
int marks = 72;

if      (marks >= 90) { System.out.println("Grade: A — Excellent");   }
else if (marks >= 75) { System.out.println("Grade: B — Good");        }
else if (marks >= 60) { System.out.println("Grade: C — Average");     }  // ← runs
else if (marks >= 40) { System.out.println("Grade: D — Below Avg");   }
else                  { System.out.println("Grade: F — Fail");        }

// Output: Grade: C — Average
```

> ⚠️ **Order is everything!** Java stops at the first `true` condition. If you put `marks >= 40` before `marks >= 90`, everyone above 40 gets Grade D — a logic bug.

---

### 🔷 4. Nested `if` Statements

An `if` block placed **inside** another `if` block. The inner condition is only checked if the outer condition passes.

```java
int age = 20;
boolean hasTicket = true;

if (age >= 18) {
    // outer is true — now check inner
    if (hasTicket) {
        System.out.println("Entry granted ✅");
    } else {
        System.out.println("No ticket — entry denied ❌");
    }
} else {
    System.out.println("You are underage ❌");
}

// age=20, hasTicket=true  → "Entry granted ✅"
// age=20, hasTicket=false → "No ticket — entry denied ❌"
// age=16                  → "You are underage ❌"
```

> ⚠️ Avoid nesting more than 2–3 levels deep. Deep nesting becomes hard to read and maintain — it is a sign that your logic might be restructured.

---

### 🔷 5. The `switch` Statement

The best tool when a **single variable** needs to be compared against several **fixed, specific values**. It is cleaner and more readable than a long `if-else-if` chain in this situation.

```java
// Syntax
switch (variable) {
    case value1:
        // code
        break;         // ← stops here, does not fall through
    case value2:
        // code
        break;
    default:
        // runs if no case matched
}
```

```java
// Example — Day of the week
int day = 3;
switch (day) {
    case 1:  System.out.println("Monday");    break;
    case 2:  System.out.println("Tuesday");   break;
    case 3:  System.out.println("Wednesday"); break;  // ← runs
    case 4:  System.out.println("Thursday");  break;
    case 5:  System.out.println("Friday");    break;
    default: System.out.println("Weekend");
}
// Output: Wednesday
```

**What happens without `break`? — Fall-through**

When `break` is missing, execution continues into the next `case` whether it matches or not.

```java
int x = 2;
switch (x) {
    case 2: System.out.println("Two");      // ← starts here
    case 3: System.out.println("Three");    // ← also runs (no break above!)
    default: System.out.println("Default"); // ← also runs
}
// Output:
// Two
// Three
// Default
```

**Intentional fall-through — grouping cases:**

You can deliberately leave out `break` to group multiple cases under one action.

```java
int day = 3;
switch (day) {
    case 1:
    case 2:
    case 3:
    case 4:
    case 5:
        System.out.println("Weekday");
        break;
    case 6:
    case 7:
        System.out.println("Weekend");
        break;
}
// day = 3 → "Weekday"
```

**Valid data types for `switch`:**

| ✅ Allowed | ❌ Not Allowed |
|------------|---------------|
| `int`, `byte`, `short`, `char` | `boolean` |
| `String` (Java 7+) | `float`, `double` |
| `enum` | Arrays, Objects |

---

### 🔷 6. Ternary Operator (Shorthand `if-else`)

A compact, single-line alternative to `if-else` that **returns a value**. Perfect for simple decisions.

```java
// Syntax
variable = (condition) ? valueIfTrue : valueIfFalse;
```

```java
// Examples
int a = 15, b = 30;

// Find maximum
int max = (a > b) ? a : b;
System.out.println("Max: " + max);         // Max: 30

// Pass or Fail
int score = 72;
String result = (score >= 50) ? "Pass" : "Fail";
System.out.println("Result: " + result);   // Result: Pass

// Positive or Negative
int num = -7;
String sign = (num >= 0) ? "Non-negative" : "Negative";
System.out.println(sign);                  // Negative

// Even or Odd
int n = 9;
System.out.println(n + " is " + (n % 2 == 0 ? "Even" : "Odd"));
// Output: 9 is Odd
```

> ✅ Use ternary for **simple, single-value** decisions.
> ❌ Avoid **chaining ternary inside ternary** — it becomes unreadable and confusing.

```java
// ❌ Avoid this — hard to read
String grade = (marks >= 90) ? "A" : (marks >= 75) ? "B" : (marks >= 60) ? "C" : "F";

// ✅ Use if-else-if instead for multi-level logic
```

---

### 🔷 7. String Comparison — A Critical Rule

When comparing Strings in conditions, never use `==`. Use `.equals()` instead.

```java
String city1 = "Patna";
String city2 = new String("Patna");

// ❌ WRONG — compares memory addresses, not content
System.out.println(city1 == city2);              // false (different objects in memory)

// ✅ CORRECT — compares actual string content
System.out.println(city1.equals(city2));         // true

// ✅ For case-insensitive comparison
System.out.println(city1.equalsIgnoreCase("PATNA")); // true
```

> ⚠️ This is a **classic Java interview trap**. `==` checks if two variables point to the same object in memory, not whether their contents are the same. Always use `.equals()` for Strings.

---

### 🔷 8. `if-else-if` vs `switch` — When to Use Which

| Situation | Recommended Choice |
|-----------|-------------------|
| Checking ranges (`age >= 18`, `marks >= 90`) | `if-else-if` |
| Complex conditions using `&&`, `\|\|` | `if-else-if` |
| Comparing one variable to many exact values | `switch` |
| Matching `char` or `String` exactly | `switch` |
| 5+ fixed discrete options | `switch` (cleaner) |

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Positive, Negative, or Zero `[Easy]`

**Task:** Given an integer, identify whether it is positive, negative, or zero.

```java
public class P1_PositiveNegativeZero {
    public static void main(String[] args) {
        int n = -3;

        if      (n > 0) { System.out.println(n + " → Positive ➕"); }
        else if (n < 0) { System.out.println(n + " → Negative ➖"); }
        else            { System.out.println(n + " → Zero 0️⃣");    }
    }
}
```

```
n=7   → Positive ➕
n=-3  → Negative ➖
n=0   → Zero 0️⃣
```

> 🔑 Three mutually exclusive outcomes = perfect `if-else-if` use case. Zero is a special case — neither positive nor negative. The final `else` acts as a catch-all for exactly one remaining case.

---

### ✅ Problem 2 — Largest of Three Numbers `[Easy-Medium]`

**Task:** Find the largest among three integers.

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
```

> 🔑 Using `>=` instead of `>` correctly handles equal values. The final `else` implicitly handles `c` — if neither `a` nor `b` is the largest, `c` must be.

---

### ✅ Problem 3 — Grade Calculator `[Easy-Medium]`

**Task:** Assign a letter grade based on marks (0–100). Handle invalid input first.

```java
public class P3_GradeCalculator {
    public static void main(String[] args) {
        int marks = 88;

        if      (marks < 0 || marks > 100) { System.out.println("Invalid marks entered!"); }
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
marks=105 → Invalid marks entered!
```

> 🔑 **Always validate input first** — this is the "fail fast" principle. If marks are invalid, there is no point running the rest of the logic.

---

### ✅ Problem 4 — Calculator with `switch` `[Medium]`

**Task:** Perform arithmetic on two numbers based on an operator character.

```java
public class P4_SwitchCalculator {
    public static void main(String[] args) {
        int a = 20, b = 4;
        char op = '/';

        switch (op) {
            case '+': System.out.println(a + " + " + b + " = " + (a + b)); break;
            case '-': System.out.println(a + " - " + b + " = " + (a - b)); break;
            case '*': System.out.println(a + " * " + b + " = " + (a * b)); break;
            case '/':
                if (b == 0) System.out.println("Cannot divide by zero!");
                else        System.out.println(a + " / " + b + " = " + ((double) a / b));
                break;
            case '%':
                if (b == 0) System.out.println("Cannot mod by zero!");
                else        System.out.println(a + " % " + b + " = " + (a % b));
                break;
            default: System.out.println("Unknown operator: " + op);
        }
    }
}
```

```
op='/'  → 20 / 4 = 5.0
op='+'  → 20 + 4 = 24
op='z'  → Unknown operator: z
b=0     → Cannot divide by zero!
```

> 🔑 `switch` works perfectly with `char`. An `if` nested inside a `case` is valid — this handles the divide-by-zero edge case cleanly.

---

### ✅ Problem 5 — Leap Year Checker `[Medium]`

**Task:** Determine if a year is a leap year using the 3-rule system.

```java
public class P5_LeapYear {
    public static void main(String[] args) {
        int year = 2024;
        boolean isLeap;

        if      (year % 400 == 0) { isLeap = true;  }  // div by 400 → always leap
        else if (year % 100 == 0) { isLeap = false; }  // div by 100, not 400 → NOT leap
        else if (year % 4   == 0) { isLeap = true;  }  // div by 4, not 100 → leap
        else                      { isLeap = false; }  // none → not leap

        System.out.println(year + " → " + (isLeap ? "Leap Year ✅" : "Not a Leap Year ❌"));

        // One-liner version using logical operators
        boolean quick = (year % 4 == 0 && year % 100 != 0) || (year % 400 == 0);
        System.out.println("One-liner check: " + quick);
    }
}
```

```
2024 → Leap Year ✅        (divisible by 4, not 100)
2000 → Leap Year ✅        (divisible by 400)
1900 → Not a Leap Year ❌  (divisible by 100, not 400)
2023 → Not a Leap Year ❌  (not divisible by 4)
```

**Decision Flow:**
```
year % 400 == 0?  →  YES  →  Leap Year
       ↓ NO
year % 100 == 0?  →  YES  →  NOT Leap Year
       ↓ NO
year % 4   == 0?  →  YES  →  Leap Year
       ↓ NO
                             NOT Leap Year
```

> 🔑 The order of checking is critical — `% 400` must come **before** `% 100`. This is a classic interview question.

---

### ✅ Problem 6 — Triangle Validity + Classifier `[Medium-Hard]`

**Task:** Check if sides form a valid triangle, then classify it.

```java
public class P6_TriangleClassifier {
    public static void main(String[] args) {
        int a = 3, b = 4, c = 5;

        // Step 1: Check validity (sum of any two sides must be > third)
        if (a + b <= c || a + c <= b || b + c <= a) {
            System.out.println("Invalid Triangle ❌");
            return;   // early exit — no need to continue
        }
        System.out.println("Valid Triangle ✅ — Sides: " + a + ", " + b + ", " + c);

        // Step 2: Find the longest side for right-angle check
        int max = Math.max(a, Math.max(b, c));
        int s1  = (max == a) ? b : a;
        int s2  = (max == c) ? b : c;

        // Step 3: Check right-angled using Pythagorean theorem
        if (s1 * s1 + s2 * s2 == max * max) {
            System.out.println("Type: Right-Angled Triangle 📐");
        }

        // Step 4: Classify by side lengths
        if      (a == b && b == c)           { System.out.println("Classification: Equilateral 🔺"); }
        else if (a == b || b == c || a == c) { System.out.println("Classification: Isosceles");      }
        else                                 { System.out.println("Classification: Scalene");         }
    }
}
```

```
3, 3, 3   → Valid ✅ | Equilateral 🔺
5, 5, 8   → Valid ✅ | Isosceles
3, 4, 5   → Valid ✅ | Right-Angled 📐 | Scalene
1, 2, 10  → Invalid Triangle ❌
```

> 🔑 Break complex problems into numbered steps — Validate → Setup → Special Check → General Check. Using `return` after invalid input is a clean "early exit" technique.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Traffic Light System — Case-Insensitive `switch`**

```java
public class MiniChallenge_Day2 {
    public static void main(String[] args) {
        String color = "red";   // test with: RED, yellow, Green, PURPLE

        // .toUpperCase() makes the switch case-insensitive
        switch (color.toUpperCase()) {
            case "RED":    System.out.println("🔴 STOP       — Do not cross");      break;
            case "YELLOW": System.out.println("🟡 SLOW DOWN  — Prepare to stop");   break;
            case "GREEN":  System.out.println("🟢 GO         — Safe to cross");      break;
            default:       System.out.println("⚠️  INVALID SIGNAL — Check the light");
        }
    }
}
```

```
"red"    → 🔴 STOP       — Do not cross      (toUpperCase → "RED" matches)
"YELLOW" → 🟡 SLOW DOWN  — Prepare to stop
"Green"  → 🟢 GO         — Safe to cross
"PURPLE" → ⚠️  INVALID SIGNAL — Check the light
```

> 🔑 Calling `.toUpperCase()` before `switch` is a simple trick that makes String matching case-insensitive. A clean, professional pattern used in real command-line applications and menu systems.

---

## 📚 W3SCHOOLS EXTRA PRACTICE — ✅ All 5 Sets Completed

> Practiced via: [W3Schools Java Exercises](https://www.w3schools.com/java/default.asp)
> Code files: [Google Drive — Java Practice Files](https://drive.google.com/drive/folders/13XLhNc1EOTbsxcr8jHnewYX30j-_fjeE?usp=sharing)

---

### 📋 Exercise Sets Completed

| # | Exercise Set | Questions | Status | Key Concept Reinforced |
|---|-------------|-----------|--------|----------------------|
| 1 | [if Conditions](https://www.w3schools.com/java/exercise.asp?x=xrcise_conditions1) | 6 | ✅ | Basic `if` syntax, boolean conditions |
| 2 | [else Conditions](https://www.w3schools.com/java/exercise.asp?x=xrcise_conditions_else1) | 5 | ✅ | `if-else` two-path logic |
| 3 | [else-if Conditions](https://www.w3schools.com/java/exercise.asp?x=xrcise_conditions_elseif1) | 5 | ✅ | Multi-path ladder, condition ordering |
| 4 | [Shorthand if (Ternary)](https://www.w3schools.com/java/exercise.asp?x=xrcise_conditions_shorthand1) | 5 | ✅ | Ternary `? :` syntax and usage |
| 5 | [Switch Statement](https://www.w3schools.com/java/exercise.asp?x=xrcise_switch1) | 7 | ✅ | `switch`, `case`, `break`, `default` |

**Total: 28 questions — all completed ✅**

---

### 🔑 Key Concepts Reinforced from W3Schools Practice

#### 📌 `if` Conditions — Reinforced

```java
// Condition must be a boolean expression
int x = 20;

if (x > 18) {
    System.out.println("x is greater than 18");
}

// Shorthand if (no curly braces for single line — use carefully)
if (x > 18) System.out.println("Above 18");

// Multiple conditions with &&
if (x > 10 && x < 30) {
    System.out.println("x is between 10 and 30");
}
```

#### 📌 `else` Conditions — Reinforced

```java
int time = 22;

if (time < 18) {
    System.out.println("Good day.");
} else {
    System.out.println("Good evening.");
}
// Output: Good evening.

// Real pattern — using else as a safety net
int speed = 130;
if (speed <= 120) {
    System.out.println("Speed is legal.");
} else {
    System.out.println("Over the speed limit!");
}
```

#### 📌 `else-if` Ladder — Reinforced

```java
// Order matters — always go from MOST specific to LEAST specific
int score = 85;

if      (score >= 90) { System.out.println("A");  }
else if (score >= 80) { System.out.println("B");  }   // ← runs for 85
else if (score >= 70) { System.out.println("C");  }
else if (score >= 60) { System.out.println("D");  }
else                  { System.out.println("F");  }

// Output: B

// ⚠️ If you reversed the order (>= 60 first), everyone ≥ 60 would get "D" — a logic bug!
```

#### 📌 Shorthand `if` (Ternary) — Reinforced

```java
// Pattern: result = (condition) ? trueValue : falseValue
int age = 20;
String status = (age >= 18) ? "Adult" : "Minor";
System.out.println(status);   // Adult

// Use directly in println — no variable needed
System.out.println((age >= 18) ? "You can vote" : "Too young to vote");

// Numeric ternary
int a = 5, b = 10;
int min = (a < b) ? a : b;
System.out.println("Min: " + min);   // Min: 5
```

#### 📌 `switch` Statement — Reinforced

```java
// switch with String (Java 7+)
String day = "Wednesday";

switch (day) {
    case "Monday":
    case "Tuesday":
    case "Wednesday":
    case "Thursday":
    case "Friday":
        System.out.println("Weekday");
        break;
    case "Saturday":
    case "Sunday":
        System.out.println("Weekend");
        break;
    default:
        System.out.println("Invalid day");
}
// Output: Weekday

// switch with char
char grade = 'B';
switch (grade) {
    case 'A': System.out.println("Outstanding"); break;
    case 'B': System.out.println("Good");        break;   // ← runs
    case 'C': System.out.println("Average");     break;
    default:  System.out.println("Below average");
}
// Output: Good
```

---

### 🏆 W3Schools Exercise Stats

```
╔═══════════════════════════════════════════════════════╗
║      W3SCHOOLS EXERCISE COMPLETION — DAY 02           ║
╠═══════════════════════════════════════════════════════╣
║  Total Exercise Sets   :  5                           ║
║  Total Questions       :  28                          ║
║  Completed             :  ✅ ALL                       ║
║  Topics Covered        :  if, else, else-if,          ║
║                           Ternary operator,           ║
║                           switch / case / break       ║
╠═══════════════════════════════════════════════════════╣
║  Score     :  ████████████████████  100%              ║
║  Confidence:  ██████████████████░░   90%              ║
╚═══════════════════════════════════════════════════════╝
```

---

## 💡 WHAT I LEARNED TODAY

- **Decision-making in Java** is done through `if`, `if-else`, `if-else-if`, nested `if`, `switch`, and the ternary operator.
- The **condition order** in an `if-else-if` ladder is critical — Java stops at the first match. Place the most specific condition first.
- **`switch`** is cleaner than a long `if-else-if` when comparing one variable to many exact values.
- The **ternary operator** is a compact shorthand for simple `if-else` assignments — not suitable for complex or nested logic.
- **Always use `.equals()`** to compare Strings in conditions — never `==`.
- **`break`** in switch prevents fall-through. Missing it causes unintended cases to run.
- **Validate input first** — check for invalid data before running the main logic (fail fast principle).
- The **ternary operator can be used inline** inside `println` for clean output.

---

## ⚠️ COMMON MISTAKES

| Mistake | Wrong | Right |
|---------|-------|-------|
| Using `=` instead of `==` in condition | `if (x = 5)` | `if (x == 5)` |
| Comparing Strings with `==` | `if (s1 == s2)` | `if (s1.equals(s2))` |
| Wrong order in ladder | `>= 40` before `>= 90` | Most specific first |
| Forgetting `break` in switch | Cases fall through | Add `break` after each case |
| Chaining ternary operators | `a ? b ? c : d : e` | Use `if-else-if` instead |
| Skipping input validation | Check marks range after grading | Validate **before** any logic |
| No `default` in switch | Unhandled inputs silently ignored | Always add `default` |
| Deep nesting (3+ levels) | Hard to read, error-prone | Restructure logic |

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| Wrote grade calculator from memory | ✅ All conditions correct |
| Wrote traffic light switch from memory | ✅ |
| Demonstrated intentional fall-through | ✅ Weekday/weekend grouping |
| Predicted `5+3+"Java"` vs `"Java"+5+3` | ✅ "8Java" vs "Java53" |

**Revision Discovery — String + int, left-to-right rule:**

```java
System.out.println(5 + 3 + "Java");   // → "8Java"
// 5 + 3 = 8 (both ints) → then 8 + "Java" = "8Java"

System.out.println("Java" + 5 + 3);   // → "Java53"
// "Java" + 5 = "Java5" (String concat) → then "Java5" + 3 = "Java53"
```

> 🔑 The moment `+` encounters a String, all subsequent `+` operations become String concatenation. Use parentheses to force arithmetic: `"Java" + (5 + 3)` → `"Java8"`.

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. When should you use if-else-if instead of switch?</b></summary>

> Use `if-else-if` when conditions involve **ranges** (`age >= 18`, `marks >= 90`), or require **complex boolean expressions** using `&&` or `||`. Use `switch` when you are comparing **one variable to multiple exact fixed values** (like a menu option or day name). Rule of thumb: if you keep writing `== someValue` for the same variable — switch is the better choice.

</details>

<details>
<summary><b>Q2. What is fall-through in switch? Is it always a bug?</b></summary>

> Fall-through happens when a `case` runs and there is no `break` — execution continues into the next case regardless of whether it matches. It is a **bug** when unintended. It is a **feature** when used deliberately to group multiple cases sharing the same action (e.g., `case 1: case 2: case 3:` all pointing to one block). Always comment intentional fall-through so other developers understand it.

</details>

<details>
<summary><b>Q3. Why should we never use == to compare Strings?</b></summary>

> `==` compares **memory references** — whether two variables point to the same object in memory. Two `String` objects with identical characters can exist at different memory addresses, making `==` return `false` even though their content is the same. `.equals()` compares the **actual character content**, which is what we almost always want. Use `.equalsIgnoreCase()` for case-insensitive comparison.

</details>

<details>
<summary><b>Q4. What is the ternary operator? When should you avoid it?</b></summary>

> The ternary operator is a one-line shorthand for `if-else`: `condition ? valueIfTrue : valueIfFalse`. It is ideal for simple assignments or inline output. Avoid it when: the logic is complex, when you need multiple statements per branch, or when you are tempted to chain ternaries inside each other — those cases are better handled by a clear `if-else-if` block.

</details>

<details>
<summary><b>Q5. If two conditions in an if-else-if are both true — which one runs?</b></summary>

> Java evaluates conditions **top to bottom** and executes the **first one that is true**, then skips all remaining branches. If `marks = 95` and you have `marks >= 60` before `marks >= 90`, then `>= 60` runs first and prints Grade C — the code never reaches Grade A. This is why placing more specific (higher) conditions first is essential for correct logic.

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
║  ✅  if — single-path conditional                                ║
║  ✅  if-else — two-path mutually exclusive decision              ║
║  ✅  if-else-if ladder — multi-path, first match wins            ║
║  ✅  Nested if — decisions inside decisions                      ║
║  ✅  switch-case — discrete value matching                       ║
║  ✅  Fall-through — both as bug and as feature                   ║
║  ✅  Ternary operator — compact one-line if-else                 ║
║  ✅  String comparison — .equals() vs == (critical)              ║
║  ✅  if-else-if vs switch — decision guide                       ║
╠══════════════════════════════════════════════════════════════════╣
║  Practice Problems : 6/6 ✅  |  Mini Challenge  : ✅             ║
║  W3Schools Sets    : 5/5 ✅  |  Total Questions : 28 ✅          ║
║  Revision Tasks    : 4/4 ✅  |  Reflection      : 5/5 ✅         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  ████████████████████  95%                    ║
║  STRONG AREA    :  if-else-if ordering, switch, ternary         ║
║  WEAK AREA      :  Knowing when switch is cleaner than if-else  ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  Condition ORDER in if-else-if is everything                 ║
║  ▸  Validate input FIRST — fail fast principle                  ║
║  ▸  NEVER use == on Strings — always .equals()                  ║
║  ▸  switch + .toUpperCase() = clean case-insensitive match      ║
║  ▸  "Java"+5+3 = "Java53" — String concat is left-to-right     ║
║  ▸  Ternary: simple decisions only — never chain them           ║
║  ▸  Always add default to switch — handle unexpected input      ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 01](./Day01_Variables_DataTypes.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 03 →](./Day03_Loops.md)

*Day 2 of 160 — Done ✅ | W3Schools: 5 Sets · 28 Questions Completed 🏆 | Streak: 2 Days 🔥🔥*

</div>
