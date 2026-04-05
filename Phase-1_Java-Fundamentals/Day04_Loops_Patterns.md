# 📄 Day 04 — Loop Patterns & Nested Loop Mastery

<div align="center">

![Day](https://img.shields.io/badge/Day-04_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Patterns_%26_Nested_Loops-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 03](./Day03_Loops.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 05 →](./Day05_Functions.md)

</div>

---

## 🎯 Day 4 Goal
> Master star, number, and character patterns using nested loops. Build the **row-column relationship instinct** that transfers directly to 2D arrays, matrix traversal, and boundary problems in DSA.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | Pattern thinking framework — 4 questions | 45 min |
| Block 2 | Star patterns — right triangle, inverted, pyramid | 30 min |
| Block 3 | Revision — Day 3 loop patterns flash review | 15 min |
| Block 4 | Practice Set — 6 Patterns | 130 min |
| Block 5 | Mini Challenge + Revision Tasks | 40 min |
| Block 6 | Reflection Questions | 20 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. The Universal Pattern Framework

Before writing a single line of code — answer these **4 questions**:

```
Q1. How many ROWS?          → Controls the OUTER loop
Q2. How many COLUMNS/ROW?   → Controls the INNER loop
                              Find the formula: row i → f(i) columns
Q3. What is PRINTED?        → Star? Number? Space? Conditional?
Q4. Are SPACES needed?      → For pyramids, diamonds, centered shapes
                              Needs a separate inner loop for spaces
```

> 🧠 This framework solves **95% of all pattern problems**. Every pattern = outer loop (rows) + inner loop(s) (spaces + content) + what to print at each cell.

---

### 🔷 2. Deriving the Row-Column Formula

Always build a **table** first. Never skip this step.

```
Pattern:              Row(i)   Stars    Formula
*                       1        1      = i
* *                     2        2      = i
* * *                   3        3      = i
* * * *                 4        4      = i
* * * * *               5        5      = i
→ Stars in row i = i   →   inner: j from 1 to i
```

```
Pattern:              Row(i)   Stars    Formula
* * * * *               1        5      = n - i + 1
* * * *                 2        4      = n - i + 1
* * *                   3        3      = n - i + 1
* *                     4        2      = n - i + 1
*                       5        1      = n - i + 1
→ Stars in row i = n - i + 1   →   inner: j from 1 to (n-i+1)
```

> 💡 Once you have the formula — the inner loop writes itself.

---

### 🔷 3. Spaces — The Hidden Dimension

For centered shapes, two inner loops are needed:
- **Loop 1** → print spaces (before content)
- **Loop 2** → print stars/numbers

```
Full Pyramid (n=5):
Row(i)   Spaces(n-i)   Stars(2i-1)
  1          4              1
  2          3              3
  3          2              5
  4          1              7
  5          0              9
```

```java
for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= n - i; j++)   System.out.print(" ");   // spaces
    for (int j = 1; j <= 2*i - 1; j++) System.out.print("*");   // stars
    System.out.println();
}
```

---

### 🔷 4. Conditional Printing Inside Loops

For hollow patterns — you don't always print the same thing. Use `if-else` inside the loop body:

```java
// Border condition for hollow shapes:
// Print * if: first row OR last row OR first col OR last col
// Print space otherwise

if (i == 1 || i == rows || j == 1 || j == cols) {
    System.out.print("* ");
} else {
    System.out.print("  ");
}
```

> 💡 This is the same logic used for **2D array boundary traversal** in DSA — marking borders, checking edge conditions in matrices.

---

### 🔷 5. Pattern Types Reference

| Pattern | Spaces | Content | Key Formula |
|---------|--------|---------|-------------|
| Right Triangle ▶ | None | `*` | `j <= i` |
| Inverted Triangle | None | `*` | `j <= n-i+1` |
| Number Triangle | None | `j` | `j <= i` |
| Row-Number Triangle | None | `i` | `j <= i` |
| Full Pyramid | `n-i` | `*` × `2i-1` | Two inner loops |
| Inverted Pyramid | `i-1` | `*` × `2(n-i)+1` | Two inner loops |
| Diamond | Pyramid + Inv. Pyramid | `*` | Combined |
| Hollow Rectangle | None | `*` or space | Border condition |
| Floyd's Triangle | None | counter++ | External counter |

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Right Triangle Star `[Easy]`

**Task:** Print right triangle for `n = 5`.

**Table first:**
```
Row(i)   Stars   Formula
  1        1      = i
  2        2      = i
  3        3      = i
  4        4      = i
  5        5      = i
→ Inner loop: j from 1 to i
```

```java
public class P1_RightTriangle {
    public static void main(String[] args) {
        int n = 5;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                System.out.print("* ");
            }
            System.out.println();
        }
    }
}
```

```
Output:
*
* *
* * *
* * * *
* * * * *
```

> 🔑 **Key Learning:** The simplest and most fundamental pattern. Row `i` prints exactly `i` stars — the inner loop upper bound IS the outer loop variable `i`. This direct relationship appears in Pascal's Triangle, lower triangular matrices, and many DP problems.

---

### ✅ Problem 2 — Inverted Right Triangle `[Easy]`

**Task:** Print inverted right triangle for `n = 5`.

**Table first:**
```
Row(i)   Stars      Formula
  1        5        = n - i + 1  →  5 - 1 + 1 = 5 ✅
  2        4        = n - i + 1  →  5 - 2 + 1 = 4 ✅
  3        3        = n - i + 1  →  5 - 3 + 1 = 3 ✅
  4        2        = n - i + 1  →  5 - 4 + 1 = 2 ✅
  5        1        = n - i + 1  →  5 - 5 + 1 = 1 ✅
→ Inner loop: j from 1 to (n - i + 1)
```

```java
public class P2_InvertedTriangle {
    public static void main(String[] args) {
        int n = 5;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n - i + 1; j++) {
                System.out.print("* ");
            }
            System.out.println();
        }
    }
}
```

```
Output:
* * * * *
* * * *
* * *
* *
*
```

> 🔑 **Key Learning:** `n - i + 1` is the most common formula in pattern problems. When you see a decreasing count, this formula gives you the column count. When `i=1` → full row. When `i=n` → single element. Memorize this.

---

### ✅ Problem 3 — Number Right Triangle `[Easy-Medium]`

**Task:** Print number triangle for `n = 5`.

**Analysis:**
```
Row(i)   What prints          Loop variable?
  1      1                    j = 1 to 1  → print j
  2      1 2                  j = 1 to 2  → print j
  3      1 2 3                j = 1 to 3  → print j
  4      1 2 3 4              j = 1 to 4  → print j
  5      1 2 3 4 5            j = 1 to 5  → print j
→ Same structure as P1, but print j instead of *
```

```java
public class P3_NumberTriangle {
    public static void main(String[] args) {
        int n = 5;
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                System.out.print(j + " ");
            }
            System.out.println();
        }
    }
}
```

```
Output:
1
1 2
1 2 3
1 2 3 4
1 2 3 4 5
```

**BONUS — Row number triangle (print `i` instead of `j`):**
```java
for (int i = 1; i <= n; i++) {
    for (int j = 1; j <= i; j++) {
        System.out.print(i + " ");   // print i, not j
    }
    System.out.println();
}
```
```
Output:
1
2 2
3 3 3
4 4 4 4
5 5 5 5 5
```

> 🔑 **Key Learning:** The ONLY difference between a star triangle and number triangle is **what you print** in the inner loop body. `j` → column number (resets each row). `i` → row number (same across entire row). This distinction is fundamental for understanding 2D array indexing.

---

### ✅ Problem 4 — Full Star Pyramid `[Medium]`

**Task:** Print centered pyramid for `n = 5`.

**Table first — TWO loops needed:**
```
Row(i)   Spaces(n-i)   Stars(2i-1)
  1          4              1
  2          3              3
  3          2              5
  4          1              7
  5          0              9

Spaces formula : n - i
Stars  formula : 2 * i - 1
```

```java
public class P4_FullPyramid {
    public static void main(String[] args) {
        int n = 5;
        for (int i = 1; i <= n; i++) {
            // Inner loop 1: print leading spaces
            for (int j = 1; j <= n - i; j++) {
                System.out.print(" ");
            }
            // Inner loop 2: print stars
            for (int j = 1; j <= 2 * i - 1; j++) {
                System.out.print("*");
            }
            System.out.println();
        }
    }
}
```

```
Output:
    *
   ***
  *****
 *******
*********
```

**Manual trace for i=3:**
```
Spaces: j from 1 to (5-3)=2  → prints "  " (2 spaces)
Stars : j from 1 to (2*3-1)=5 → prints "*****"
Result: "  *****" ✅
```

> 🔑 **Key Learning:** The pyramid needs **two inner loops** — one for spaces, one for stars. Spaces decrease as i increases (`n-i`). Stars increase as i increases (`2i-1`). The sum of first `n` odd numbers (1+3+5+...+(2n-1)) = n² — this explains why a pyramid of height `n` always has exactly `n²` stars total.

---

### ✅ Problem 5 — Floyd's Triangle `[Medium]`

**Task:** Print Floyd's Triangle for `n = 5`.

**Analysis:**
```
Row(i)   Numbers in row   Values
  1           1           1
  2           2           2 3
  3           3           4 5 6
  4           4           7 8 9 10
  5           5           11 12 13 14 15

Numbers don't RESET — they continue from where previous row ended.
Need an EXTERNAL counter starting at 1, increments every cell.
```

```java
public class P5_FloydsTriangle {
    public static void main(String[] args) {
        int n = 7;
        int counter = 1;    // external counter — lives OUTSIDE both loops

        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                System.out.print(counter + " ");
                counter++;   // increment after EVERY cell printed
            }
            System.out.println();
        }
    }
}
```

```
Output (n=5):
1
2 3
4 5 6
7 8 9 10
11 12 13 14 15
```

**Trace (first 3 rows):**
```
i=1: j=1 → print 1, counter=2          → row: "1"
i=2: j=1 → print 2, counter=3
     j=2 → print 3, counter=4          → row: "2 3"
i=3: j=1 → print 4, counter=5
     j=2 → print 5, counter=6
     j=3 → print 6, counter=7          → row: "4 5 6"
```

> 🔑 **Key Learning:** When values carry over between rows, the counter must live **outside** both loops. This pattern of an external state variable that persists across iterations is used in many DSA problems — running totals, cumulative sums, carry values in arithmetic. Floyd's Triangle is a classic interview pattern question.

---

### ✅ Problem 6 — Hollow Rectangle `[Medium-Hard]`

**Task:** Print hollow rectangle for `rows = 4`, `cols = 8`.

**Analysis — when to print `*` vs space:**
```
Print * when ANY of these is true:
  - First row    (i == 1)
  - Last row     (i == rows)
  - First column (j == 1)
  - Last column  (j == cols)
Otherwise: print space

Border condition: i==1 || i==rows || j==1 || j==cols
```

```java
public class P6_HollowRectangle {
    public static void main(String[] args) {
        int rows = 4, cols = 8;

        for (int i = 1; i <= rows; i++) {
            for (int j = 1; j <= cols; j++) {
                // Print * on border, space inside
                if (i == 1 || i == rows || j == 1 || j == cols) {
                    System.out.print("* ");
                } else {
                    System.out.print("  ");
                }
            }
            System.out.println();
        }
    }
}
```

```
Output:
* * * * * * * *
*             *
*             *
* * * * * * * *
```

**Trace row 2, cols 1–8:**
```
j=1: i≠1,i≠4 but j==1 → print *
j=2: not border        → print space
j=3: not border        → print space
j=4: not border        → print space
j=5: not border        → print space
j=6: not border        → print space
j=7: not border        → print space
j=8: j==cols(8)        → print *
Result: "*             *" ✅
```

> 🔑 **Key Learning:** Conditional printing inside loops — the `if` body is NOT always the same for every cell. The border condition `i==1 || i==rows || j==1 || j==cols` is exactly the same logic used to:
> - Detect boundary cells in a 2D array
> - Check if a matrix element is on the edge
> - Solve "set matrix borders" type problems in DSA
> This is pattern problem knowledge transferring directly to DSA.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Full Diamond Pattern for `n = 4`.

**Analysis — split into two halves:**
```
TOP HALF (n rows — full pyramid):
Row(i)   Spaces(n-i)   Stars(2i-1)
  1          3              1
  2          2              3
  3          1              5
  4          0              7

BOTTOM HALF (n-1 rows — inverted pyramid):
Row(i)   Spaces(i-1)   Stars(2(n-i)+1)  [i goes 1 to n-1]
  1          1              2*(4-1)+1 = 5? No...

Let i go from 1 to n-1, actual row from top of bottom = i
  i=1: spaces=1, stars=2*(4-1)-1=5? Let's re-derive:
  Bottom row 1 (below center): 7 stars, 0 spaces — wait n=4 so:

Correct bottom half (i from n-1 down to 1 using original n):
  i=n-1=3: spaces=1, stars=2*3-1=5
  i=n-2=2: spaces=2, stars=2*2-1=3
  i=n-3=1: spaces=3, stars=2*1-1=1
→ Bottom half: outer i from n-1 to 1
  Spaces = n - i,  Stars = 2*i - 1  (same formulas as top!)
```

```java
public class MiniChallenge_Day4 {
    public static void main(String[] args) {
        int n = 4;

        // Top half — full pyramid (i: 1 to n)
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n - i; j++) System.out.print(" ");
            for (int j = 1; j <= 2 * i - 1; j++) System.out.print("*");
            System.out.println();
        }

        // Bottom half — inverted pyramid (i: n-1 down to 1)
        for (int i = n - 1; i >= 1; i--) {
            for (int j = 1; j <= n - i; j++) System.out.print(" ");
            for (int j = 1; j <= 2 * i - 1; j++) System.out.print("*");
            System.out.println();
        }
    }
}
```

```
Output (n=4):
   *
  ***
 *****
*******
 *****
  ***
   *
```

**Key insight — both halves use the SAME formulas:**
```
Spaces = n - i
Stars  = 2 * i - 1
Top half:    i goes 1 → n     (ascending)
Bottom half: i goes n-1 → 1   (descending)
```

> 🔑 **Key Learning:** The diamond is NOT a new pattern — it is just a pyramid run twice with opposite loop directions. **Decompose complex patterns into known sub-patterns.** This decomposition skill — breaking a big problem into known smaller problems — is the core of DSA thinking. It's what makes recursion, divide-and-conquer, and DP work.

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| Digit-reversal algorithm traced for `n = 456` | ✅ See trace below |
| Prime check with `√n` from memory | ✅ `for(int i=2; i*i<=n; i++)` |
| Traced nested loop output | ✅ See output below |
| `break` in inner loop — which exits? | ✅ Only innermost |

**Revision Trace — Reverse of 456:**
```
n=456, rev=0
→ digit=6, rev=0*10+6=6,   n=45
→ digit=5, rev=6*10+5=65,  n=4
→ digit=4, rev=65*10+4=654, n=0
→ loop ends → rev = 654 ✅
```

**Revision Trace — Nested loop output:**
```java
for(int i=1; i<=3; i++) {
    for(int j=i; j<=3; j++) {   // j starts at i, not 1!
        System.out.print(j + " ");
    }
    System.out.println();
}
```
```
i=1: j=1,2,3 → "1 2 3"
i=2: j=2,3   → "2 3"
i=3: j=3     → "3"
Output:
1 2 3
2 3
3
```
> Key: inner loop starts at `j=i` — this creates an upper-right triangle, not lower-left.

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. What are the 4 questions before coding any pattern?</b></summary>

> 1. **How many ROWS?** → Controls outer loop bounds
> 2. **How many COLUMNS per row?** → Build table to find formula → controls inner loop bounds
> 3. **What is PRINTED at each position?** → Star? Number `i`? Number `j`? Space? Conditional?
> 4. **Are SPACES needed?** → For centered patterns, need extra inner loop for leading spaces
>
> These 4 questions transform any pattern from "how do I start?" to a mechanical process.

</details>

<details>
<summary><b>Q2. Why does a pyramid need TWO inner loops?</b></summary>

> One inner loop alone can only print one type of character continuously.
> A pyramid needs: spaces (decreasing) THEN stars (increasing) on each row.
> Loop 1 prints `n-i` spaces. Loop 2 prints `2i-1` stars.
> They run sequentially in the same outer iteration, producing the two-part row.
> If you try to combine them into one loop, the logic becomes messy — two separate loops is always cleaner.

</details>

<details>
<summary><b>Q3. Sum of stars in a pyramid with n rows?</b></summary>

> Stars per row: 1, 3, 5, 7, ... (2n-1) — these are the first `n` odd numbers.
> Sum of first `n` odd numbers = **n²** (famous formula).
> Proof: 1+3+5+7+9 = 25 = 5² for n=5. ✅
> This formula appears in number theory problems and complexity analysis.

</details>

<details>
<summary><b>Q4. Difference between printing i vs j in a number triangle?</b></summary>

> **Print `j`:** column counter resets to 1 each row → each row shows `1, 2, 3, ..., i`
> ```
> 1
> 1 2
> 1 2 3
> ```
> **Print `i`:** row number, same across entire row → each row repeats its row number
> ```
> 1
> 2 2
> 3 3 3
> ```
> This distinction is fundamental — `i` is the row index, `j` is the column index. In 2D arrays, `arr[i][j]` uses both independently — understanding what each controls is essential.

</details>

<details>
<summary><b>Q5. Why use one if-else inside the loop rather than separate loops for each row type?</b></summary>

> Separate loops would require checking "is this row a border row?" BEFORE the inner loop — meaning you'd need `if (i==1 || i==rows)` around one loop and `else` around another loop. That's MORE code, harder to read, and harder to modify.
> One `if-else` INSIDE the single nested loop means: one unified traversal logic, condition checked per cell. Cleaner, more scalable, and closer to how real 2D array manipulation works in DSA — you traverse all cells and decide per-cell what to do.

</details>

---

## 📌 DAY 4 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 4 — SUMMARY                         ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 02, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Loop Patterns & Nested Loop Mastery             ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                                ║
║  ✅  4-question pattern framework                                ║
║  ✅  Row-column formula derivation (build table first!)          ║
║  ✅  Spaces — second inner loop for centered shapes              ║
║  ✅  Conditional printing — if-else inside nested loops          ║
║  ✅  Pattern types reference — 8 types mastered                  ║
╠══════════════════════════════════════════════════════════════════╣
║  PATTERNS SOLVED                                                 ║
║  ✅  P1 — Right Triangle Star                                    ║
║  ✅  P2 — Inverted Right Triangle                                ║
║  ✅  P3 — Number Triangle (j and i variants)                     ║
║  ✅  P4 — Full Star Pyramid (two inner loops)                    ║
║  ✅  P5 — Floyd's Triangle (external counter)                    ║
║  ✅  P6 — Hollow Rectangle (border condition)                    ║
║  ✅  Mini Challenge — Full Diamond (top + bottom decomposed)     ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  90%                               ║
║  STRONG AREA    :  Triangle patterns, Floyd's, formula derivation║
║  WEAK AREA      :  Diamond decomposition — space formulas        ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                   ║
║  ▸  Build the row-column TABLE first — always, no exceptions     ║
║  ▸  n - i + 1 = most common decreasing formula in patterns       ║
║  ▸  2i - 1 = stars in pyramid row i; sum = n² total              ║
║  ▸  External counter for Floyd's → persists across rows          ║ 
║  ▸  Border condition i==1||i==n||j==1||j==n → hollow shapes      ║
║  ▸  Decompose complex patterns into known sub-patterns           ║
║  ▸  Conditional in loop → same skill as 2D array boundary DSA    ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 03](./Day03_Loops.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 05 →](./Day05_Functions.md)

*Day 4 of 160 — Done ✅ | Streak: 4 Days 🔥🔥🔥🔥*

</div>
