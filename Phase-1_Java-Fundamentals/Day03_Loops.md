# 📄 Day 03 — Loops & Iteration

<div align="center">

![Day](https://img.shields.io/badge/Day-03_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Loops_%26_Iteration-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![W3S](https://img.shields.io/badge/W3Schools_Exercises-6_Sets_✅-04AA6D?style=for-the-badge&logo=w3schools&logoColor=white)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 02](./Day02_Conditionals.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 04 →](./Day04_Loops_Patterns.md)

</div>

---

## 🎯 Day 3 Goal

> Master all loop types in Java — `for`, `while`, `do-while`, and the enhanced `for-each` loop. Understand `break` and `continue` control statements, nested loops, and know exactly when to choose each loop type. Loops are the backbone of algorithms — every sort, search, and pattern problem you will face in DSA is built on them.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | `for` loop + `while` loop + `do-while` | 60 min |
| Block 2 | `for-each`, `break`, `continue`, nested loops | 45 min |
| Block 3 | Quick Revision — Day 2 Conditionals | 15 min |
| Block 4 | Practice Set — 6 Problems | 120 min |
| Block 5 | W3Schools Exercises + Real-Life Challenges | 40 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. The `for` Loop

The `for` loop is used when you know **exactly how many times** you want to repeat something. All three control parts — initialization, condition, update — are written in a single line.

```java
// Syntax
for (initialization; condition; update) {
    // code to repeat
}
```

```java
// Example — count from 1 to 5
for (int i = 1; i <= 5; i++) {
    System.out.println("Count: " + i);
}
// Output: Count: 1  Count: 2  Count: 3  Count: 4  Count: 5

// Count backwards
for (int i = 5; i >= 1; i--) {
    System.out.print(i + " ");
}
// Output: 5 4 3 2 1

// Step by 2
for (int i = 0; i <= 10; i += 2) {
    System.out.print(i + " ");
}
// Output: 0 2 4 6 8 10
```

**How the `for` loop executes step by step:**
```
Step 1: int i = 1          ← initialization (runs ONCE at start)
Step 2: i <= 5 ?           ← condition checked BEFORE each iteration
Step 3: execute body
Step 4: i++                ← update runs AFTER each iteration
Step 5: go back to Step 2
Step 6: i <= 5 is FALSE → loop ends
```

> ✅ **When to use:** When you know the exact number of iterations upfront — printing a multiplication table, iterating over array indices, counting up or down to a fixed number.

---

### 🔷 2. The `while` Loop

The `while` loop is used when you **don't know exactly** how many times to repeat — you keep going as long as a condition remains true.

```java
// Syntax
while (condition) {
    // code to repeat
    // MUST update something that eventually makes condition false!
}
```

```java
// Example — countdown
int count = 5;
while (count > 0) {
    System.out.println("Countdown: " + count);
    count--;    // update — moves toward stopping condition
}
// Output: Countdown: 5  4  3  2  1

// Digit extraction (classic problem)
int n = 12345;
while (n > 0) {
    int digit = n % 10;          // last digit
    System.out.print(digit + " ");
    n = n / 10;                  // remove last digit
}
// Output: 5 4 3 2 1
```

> ✅ **When to use:** When the number of iterations depends on a condition that changes during execution — reading user input until valid, processing digits, game loops, searching until found.

> ⚠️ **Critical:** Always update something inside the loop that moves toward making the condition `false`. Forgetting this = **infinite loop**.

---

### 🔷 3. The `do-while` Loop

The `do-while` loop is like the `while` loop — **but it always runs at least once**, because the condition is checked AFTER the body executes.

```java
// Syntax
do {
    // code to repeat
} while (condition);   // ← semicolon is mandatory here!
```

```java
// Example — menu that runs at least once
int choice;
do {
    System.out.println("1. Start Game");
    System.out.println("2. Settings");
    System.out.println("3. Quit");
    System.out.println("Enter choice: ");
    choice = 3;   // simulating input
} while (choice != 3);
// Menu always shows at least once even if choice is 3 immediately

// Sum of digits using do-while
int num = 12345, sum = 0;
do {
    sum += num % 10;
    num /= 10;
} while (num > 0);
System.out.println("Sum of digits: " + sum);   // 15
```

> ✅ **When to use:** When the body MUST execute at least once before checking whether to continue — menu systems, input validation loops, game rounds.

---

### 🔷 4. The Enhanced `for-each` Loop

The `for-each` loop is a cleaner, read-only way to iterate over **arrays and collections**. No index variable needed.

```java
// Syntax
for (dataType element : arrayOrCollection) {
    // use element
}
```

```java
// Example — iterate over an array
int[] marks = {85, 92, 78, 96, 88};

for (int mark : marks) {
    System.out.print(mark + " ");
}
// Output: 85 92 78 96 88

// Sum all marks
int total = 0;
for (int mark : marks) {
    total += mark;
}
System.out.println("Total: " + total);   // 439

// Iterate over String array
String[] fruits = {"Apple", "Banana", "Cherry"};
for (String fruit : fruits) {
    System.out.println("Fruit: " + fruit);
}
```

> ✅ **When to use:** When you need to read every element of an array or collection and don't need the index. Cleaner and less error-prone than a regular `for` loop for simple traversal.

> ⚠️ **Limitation:** Cannot modify the array elements, cannot iterate in reverse, cannot skip elements — use a regular `for` loop when you need those.

---

### 🔷 5. `break` Statement

`break` immediately **exits the current loop** when executed. The loop stops and execution continues at the statement after the loop.

```java
// Example — stop when target is found
int[] arr = {3, 7, 1, 9, 4, 6};
int target = 9;
int foundAt = -1;

for (int i = 0; i < arr.length; i++) {
    if (arr[i] == target) {
        foundAt = i;
        break;   // no need to continue — found!
    }
}
System.out.println("Found at index: " + foundAt);   // 3

// Find first even number
int[] nums = {3, 5, 7, 4, 9, 2};
for (int n : nums) {
    if (n % 2 == 0) {
        System.out.println("First even: " + n);   // 4
        break;
    }
}
```

---

### 🔷 6. `continue` Statement

`continue` **skips the rest of the current iteration** and jumps to the next one. The loop itself does NOT stop.

```java
// Skip even numbers — print only odds
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) continue;   // skip even → go to next i
    System.out.print(i + " ");
}
// Output: 1 3 5 7 9

// Skip negative numbers when computing sum
int[] values = {5, -3, 8, -1, 4, -7, 2};
int sum = 0;
for (int v : values) {
    if (v < 0) continue;    // skip negatives
    sum += v;
}
System.out.println("Sum of positives: " + sum);   // 19
```

> 💡 **`break` vs `continue`:**
> - `break` → EXIT the loop entirely
> - `continue` → SKIP this iteration, continue with next

---

### 🔷 7. Nested Loops

A loop inside another loop. The inner loop completes ALL its iterations for every single iteration of the outer loop.

```java
// Multiplication table
for (int i = 1; i <= 3; i++) {
    for (int j = 1; j <= 3; j++) {
        System.out.printf("%4d", i * j);
    }
    System.out.println();
}
// Output:
//    1   2   3
//    2   4   6
//    3   6   9

// Simple star pattern
for (int i = 1; i <= 4; i++) {
    for (int j = 1; j <= i; j++) {
        System.out.print("* ");
    }
    System.out.println();
}
// Output:
// *
// * *
// * * *
// * * * *
```

**How nested loops execute:**
```
Outer i=1: inner j runs 1,2,3 → 3 iterations
Outer i=2: inner j runs 1,2,3 → 3 iterations
Outer i=3: inner j runs 1,2,3 → 3 iterations
Total: 3 × 3 = 9 iterations
```

> ⚠️ Every extra nesting level multiplies the total iterations. A triple-nested loop on 100 elements = 100³ = 1,000,000 iterations. Be mindful of performance.

---

## 🔑 KEY DIFFERENCES — `for` vs `while` vs `do-while`

| Feature | `for` | `while` | `do-while` |
|---------|-------|---------|------------|
| When to use | Known iteration count | Unknown count, condition-driven | Must run at least once |
| Condition checked | Before each iteration | Before each iteration | **After** each iteration |
| Guaranteed runs | Only if condition true | Only if condition true | **Always at least 1** |
| Counter in header | ✅ Yes (init + update) | ❌ No (manual) | ❌ No (manual) |
| Infinite loop risk | Lower (counter visible) | Higher (easy to forget update) | Higher |
| Best for | Arrays, counting, patterns | Input validation, searching | Menus, game loops |

---

## 🌍 REAL-LIFE USE CASES

---

### 🔹 `for` Loop — Real Life

```java
// ATM: Print transaction history (fixed count)
int[] transactions = {500, -200, 1000, -150, 300};
System.out.println("=== Transaction History ===");
for (int i = 0; i < transactions.length; i++) {
    System.out.printf("TXN %d: %+d%n", i + 1, transactions[i]);
}

// School: Calculate class average
int[] scores = {72, 85, 90, 68, 95, 78};
int total = 0;
for (int score : scores) total += score;
System.out.printf("Class Average: %.1f%n", (double)total / scores.length);

// E-commerce: Apply discount to all prices
double[] prices = {1299.0, 599.0, 2499.0, 899.0};
System.out.println("Prices after 10% discount:");
for (double price : prices)
    System.out.printf("  ₹%.2f → ₹%.2f%n", price, price * 0.90);
```

---

### 🔹 `while` Loop — Real Life

```java
// ATM: Keep asking for PIN until correct (max 3 attempts)
int correctPin = 1234;
int attempts   = 0;
int enteredPin = 0;

while (enteredPin != correctPin && attempts < 3) {
    attempts++;
    enteredPin = 1234;   // simulating correct pin on attempt 1
    System.out.println("Attempt " + attempts + ": PIN entered");
}
System.out.println(enteredPin == correctPin ? "Access granted!" : "Card blocked.");

// Investment: Compound interest until target reached
double balance = 10000.0;
double rate    = 0.08;    // 8% annual
int    years   = 0;
double target  = 20000.0;

while (balance < target) {
    balance += balance * rate;
    years++;
}
System.out.printf("₹10,000 doubles in %d years at 8%% interest%n", years);
// Output: ₹10,000 doubles in 10 years at 8% interest
```

---

### 🔹 `do-while` Loop — Real Life

```java
// Restaurant menu — always show at least once
int userChoice;
do {
    System.out.println("\n=== Restaurant Menu ===");
    System.out.println("1. Biryani    - ₹250");
    System.out.println("2. Paneer     - ₹180");
    System.out.println("3. Dal Makhani- ₹150");
    System.out.println("4. Exit");
    userChoice = 4;   // simulated: user chose Exit
    System.out.println("You selected: " + userChoice);
} while (userChoice != 4);
System.out.println("Thank you! Visit again.");
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Number Tables `[Easy]`

**Task:** Print multiplication table for a given number using all three loop types.

```java
public class P1_MultiplicationTable {
    public static void main(String[] args) {
        int n = 7;

        // Using for loop
        System.out.println("Table of " + n + " (for loop):");
        for (int i = 1; i <= 10; i++)
            System.out.printf("%d × %2d = %d%n", n, i, n * i);

        // Using while loop
        System.out.println("\nTable of " + n + " (while loop):");
        int i = 1;
        while (i <= 10) {
            System.out.printf("%d × %2d = %d%n", n, i, n * i);
            i++;
        }

        // Using do-while
        System.out.println("\nTable of " + n + " (do-while):");
        int j = 1;
        do {
            System.out.printf("%d × %2d = %d%n", n, j, n * j);
            j++;
        } while (j <= 10);
    }
}
```

```
Output:
Table of 7 (for loop):
7 ×  1 = 7
7 ×  2 = 14
...
7 × 10 = 70
```

> 🔑 All three loops produce identical output — the difference is only in structure and when you know the count upfront.

---

### ✅ Problem 2 — Sum, Average, Max, Min `[Easy]`

**Task:** Compute sum, average, maximum, and minimum of an array using a for-each loop.

```java
public class P2_ArrayStats {
    public static void main(String[] args) {
        int[] marks = {85, 92, 78, 96, 88, 74, 95, 61, 89, 77};

        int sum  = 0;
        int max  = marks[0];   // initialize with first element!
        int min  = marks[0];

        for (int m : marks) {
            sum += m;
            if (m > max) max = m;
            if (m < min) min = m;
        }

        System.out.println("Marks    : " + java.util.Arrays.toString(marks));
        System.out.println("Sum      : " + sum);
        System.out.printf ("Average  : %.1f%n", (double) sum / marks.length);
        System.out.println("Maximum  : " + max);
        System.out.println("Minimum  : " + min);
    }
}
```

```
Output:
Marks    : [85, 92, 78, 96, 88, 74, 95, 61, 89, 77]
Sum      : 835
Average  : 83.5
Maximum  : 96
Minimum  : 61
```

> 🔑 Initialize `max = arr[0]` and `min = arr[0]` — never use 0 as the initial value. For an all-negative array, max=0 would be wrong. Always start with the first element.

---

### ✅ Problem 3 — Prime Number Checker `[Easy-Medium]`

**Task:** Check if a number is prime. Optimize with √n loop boundary.

```java
public class P3_PrimeChecker {

    static boolean isPrime(int n) {
        if (n <= 1) return false;
        if (n <= 3) return true;
        if (n % 2 == 0 || n % 3 == 0) return false;

        // Only check up to √n — any factor > √n has a partner < √n
        for (int i = 2; i * i <= n; i++) {
            if (n % i == 0) return false;
        }
        return true;
    }

    public static void main(String[] args) {
        // Check individual numbers
        int[] tests = {1, 2, 3, 4, 17, 25, 29, 97, 100};
        for (int n : tests)
            System.out.printf("%3d → %s%n", n, isPrime(n) ? "✅ Prime" : "❌ Not Prime");

        System.out.println();

        // Print all primes up to 50
        System.out.print("Primes up to 50: ");
        for (int i = 2; i <= 50; i++)
            if (isPrime(i)) System.out.print(i + " ");
        System.out.println();
    }
}
```

```
Output:
  1 → ❌ Not Prime
  2 → ✅ Prime
  3 → ✅ Prime
  4 → ❌ Not Prime
 17 → ✅ Prime
 25 → ❌ Not Prime
 29 → ✅ Prime
 97 → ✅ Prime
100 → ❌ Not Prime

Primes up to 50: 2 3 5 7 11 13 17 19 23 29 31 37 41 43 47
```

> 🔑 `i * i <= n` instead of `i <= Math.sqrt(n)` — avoids the float computation and is equally correct. For n=100, we only check i=2..10 (9 checks) instead of 2..99 (98 checks). This √n optimization is used in many DSA problems.

---

### ✅ Problem 4 — Digit Operations `[Medium]`

**Task:** Extract and process individual digits using while loop.

```java
public class P4_DigitOps {

    static int sumOfDigits(int n) {
        n = Math.abs(n);
        int sum = 0;
        while (n > 0) { sum += n % 10; n /= 10; }
        return sum;
    }

    static int reverseNumber(int n) {
        boolean negative = n < 0;
        n = Math.abs(n);
        int reversed = 0;
        while (n > 0) { reversed = reversed * 10 + n % 10; n /= 10; }
        return negative ? -reversed : reversed;
    }

    static int countDigits(int n) {
        if (n == 0) return 1;
        n = Math.abs(n);
        int count = 0;
        while (n > 0) { count++; n /= 10; }
        return count;
    }

    static boolean isArmstrong(int n) {
        int digits = countDigits(n);
        int temp = n, sum = 0;
        while (temp > 0) {
            int d = temp % 10;
            sum += (int) Math.pow(d, digits);
            temp /= 10;
        }
        return sum == n;
    }

    public static void main(String[] args) {
        int[] tests = {12345, 153, 370, 9474, 0, -456};
        for (int n : tests) {
            System.out.printf("n=%-6d  sum=%2d  reverse=%-7d  digits=%d  armstrong=%s%n",
                n, sumOfDigits(n), reverseNumber(n), countDigits(n),
                isArmstrong(Math.abs(n)) ? "✅" : "❌");
        }
    }
}
```

```
Output:
n=12345   sum=15  reverse=54321    digits=5  armstrong=❌
n=153     sum= 9  reverse=351      digits=3  armstrong=✅
n=370     sum=10  reverse=73       digits=3  armstrong=✅
n=9474    sum=24  reverse=4749     digits=4  armstrong=✅
n=0       sum= 0  reverse=0        digits=1  armstrong=✅
n=-456    sum=15  reverse=-654     digits=3  armstrong=❌
```

> 🔑 The `while (n > 0)` + `n % 10` + `n /= 10` pattern extracts digits one by one from the right. This is the single most used while-loop pattern in DSA — it appears in digit sum, digit reversal, Armstrong numbers, and palindrome number checks.

---

### ✅ Problem 5 — Pattern Printing with Nested Loops `[Medium]`

**Task:** Print various patterns using nested loops.

```java
public class P5_Patterns {
    public static void main(String[] args) {

        // Pattern 1: Right triangle
        System.out.println("Right Triangle:");
        for (int i = 1; i <= 5; i++) {
            for (int j = 1; j <= i; j++)
                System.out.print("* ");
            System.out.println();
        }

        // Pattern 2: Inverted triangle
        System.out.println("\nInverted Triangle:");
        for (int i = 5; i >= 1; i--) {
            for (int j = 1; j <= i; j++)
                System.out.print("* ");
            System.out.println();
        }

        // Pattern 3: Number triangle
        System.out.println("\nNumber Triangle:");
        for (int i = 1; i <= 5; i++) {
            for (int j = 1; j <= i; j++)
                System.out.print(j + " ");
            System.out.println();
        }

        // Pattern 4: Pyramid
        System.out.println("\nPyramid:");
        int n = 5;
        for (int i = 1; i <= n; i++) {
            for (int j = i; j < n; j++) System.out.print("  ");   // spaces
            for (int j = 1; j <= (2*i - 1); j++) System.out.print("* "); // stars
            System.out.println();
        }
    }
}
```

```
Output:
Right Triangle:
*
* *
* * *
* * * *
* * * * *

Inverted Triangle:
* * * * *
* * * *
* * *
* *
*

Number Triangle:
1
1 2
1 2 3
1 2 3 4
1 2 3 4 5

Pyramid:
        *
      * * *
    * * * * *
  * * * * * * *
* * * * * * * * *
```

> 🔑 Pattern problems train you to think about nested loop relationships — outer loop = rows, inner loop = what to print in each row. The pyramid formula: spaces = `n-i`, stars = `2*i-1`. This exact thinking transfers to triangle DP problems and grid-based algorithms.

---

### ✅ Problem 6 — Search with Break + Skip with Continue `[Medium]`

**Task:** Combine break and continue in real scenarios.

```java
public class P6_BreakContinue {
    public static void main(String[] args) {

        // Scenario 1: Linear search with break
        int[] inventory = {12, 0, 45, 0, 8, 0, 99, 33};
        System.out.println("=== Out of Stock Items (quantity=0) ===");
        for (int i = 0; i < inventory.length; i++) {
            if (inventory[i] == 0) {
                System.out.println("Item " + (i+1) + " is OUT OF STOCK");
                continue;   // skip, don't print "in stock" for this
            }
            System.out.println("Item " + (i+1) + ": " + inventory[i] + " units");
        }

        System.out.println();

        // Scenario 2: Find first prime > 50 using break
        System.out.println("=== First Prime > 50 ===");
        for (int n = 51; n <= 200; n++) {
            boolean prime = true;
            for (int d = 2; d * d <= n; d++) {
                if (n % d == 0) { prime = false; break; }  // inner break
            }
            if (prime) { System.out.println("First prime > 50: " + n); break; } // outer break
        }

        System.out.println();

        // Scenario 3: Sum of positive numbers only, stop at -1
        int[] data  = {10, 25, -3, 40, 15, -1, 100, 55};
        int   total = 0;
        System.out.println("=== Sum positive, stop at -1 ===");
        for (int d : data) {
            if (d == -1) { System.out.println("Sentinel -1 found. Stopping."); break; }
            if (d < 0)   { System.out.println("Skipping negative: " + d); continue; }
            total += d;
            System.out.println("Adding " + d + ", running total = " + total);
        }
        System.out.println("Final sum: " + total);
    }
}
```

```
Output:
=== Out of Stock Items (quantity=0) ===
Item 1: 12 units
Item 2 is OUT OF STOCK
Item 3: 45 units
Item 4 is OUT OF STOCK
Item 5: 8 units
Item 6 is OUT OF STOCK
Item 7: 99 units
Item 8: 33 units

=== First Prime > 50 ===
First prime > 50: 53

=== Sum positive, stop at -1 ===
Adding 10, running total = 10
Adding 25, running total = 35
Skipping negative: -3
Adding 40, running total = 75
Adding 15, running total = 90
Sentinel -1 found. Stopping.
Final sum: 90
```

> 🔑 `break` in the inner loop only exits the inner loop — the outer loop continues. To exit both loops, you need a flag variable or a labeled break (`break outerLabel`). The sentinel value pattern (`if (val == -1) break`) is used in real data processing — streaming data where a special value signals end of input.

---

## 📚 W3SCHOOLS EXTRA PRACTICE — ✅ All 6 Sets Completed

> Practiced via: [W3Schools Java Exercises](https://www.w3schools.com/java/default.asp)
> Code files: [Google Drive — Java Practice Files](https://drive.google.com/drive/folders/1JQT-p1PA6JQ9JxPvOf_McKCFx1P0M2Tu?usp=sharing)

---

### 📋 Exercise Sets Completed

| # | Exercise Set | Questions | Status | Key Concept Reinforced |
|---|-------------|-----------|--------|----------------------|
| 1 | [While Loop](https://www.w3schools.com/java/exercise.asp?x=xrcise_while_loop1) | 5 | ✅ | `while` syntax, condition, update |
| 2 | [Do While Loop](https://www.w3schools.com/java/exercise.asp?x=xrcise_while_loop_do1) | 5 | ✅ | `do-while` — runs at least once, semicolon |
| 3 | [For Loop](https://www.w3schools.com/java/exercise.asp?x=xrcise_for_loop1) | 6 | ✅ | `for` structure — init, condition, update |
| 4 | [Nested For Loop](https://www.w3schools.com/java/exercise.asp?x=xrcise_for_loop_nested1) | 5 | ✅ | Nested loops, outer × inner iterations |
| 5 | [For Each Loop](https://www.w3schools.com/java/exercise.asp?x=xrcise_foreach_loop1) | 5 | ✅ | Enhanced for, array traversal, read-only |
| 6 | [Break](https://www.w3schools.com/java/exercise.asp?x=xrcise_break1) | 5 | ✅ | `break` exits loop, `continue` skips |

**Total: 31 questions — all completed ✅**

---

### 🌍 Real-Life Challenges Completed

| Challenge | Link | Status |
|-----------|------|--------|
| While Loop Real Life | [w3schools.com](https://www.w3schools.com/java/java_while_loop_reallife.asp) | ✅ |
| While Loop Challenges | [w3schools.com](https://www.w3schools.com/java/java_challenges_while_loop.asp) | ✅ |
| For Loop Real Life | [w3schools.com](https://www.w3schools.com/java/java_for_loop_reallife.asp) | ✅ |
| For Loop Challenges | [w3schools.com](https://www.w3schools.com/java/java_challenges_for_loop.asp) | ✅ |

---

### 🔑 Key Concepts from W3Schools Practice

#### 📌 `while` Loop — Reinforced

```java
// Pattern: while loop with counter
int i = 1;
while (i <= 5) {
    System.out.println(i);
    i++;              // ← MUST update, or infinite loop!
}

// Real life: keep asking for valid age
int age = 0;
while (age <= 0 || age > 120) {
    age = 20;         // simulated user input
}
System.out.println("Valid age: " + age);
```

#### 📌 `do-while` Loop — Reinforced

```java
// The do-while ALWAYS executes body first
int x = 10;
do {
    System.out.println("Runs once even if condition is false: " + x);
    x++;
} while (x < 5);    // condition false — but it still ran once!
// Output: Runs once even if condition is false: 10

// Real life: enter password — must try at least once
String password = "";
do {
    password = "secret123";   // simulated input
} while (!password.equals("secret123"));
System.out.println("Logged in!");
```

#### 📌 `for` Loop — Reinforced

```java
// Three variations
for (int i = 0; i < 5; i++)  System.out.print(i + " ");   // 0 1 2 3 4
for (int i = 5; i > 0; i--)  System.out.print(i + " ");   // 5 4 3 2 1
for (int i = 0; i <= 20; i+=5) System.out.print(i + " "); // 0 5 10 15 20

// Infinite loop (use carefully)
// for (;;) { } ← runs forever — always have a break condition

// Nested for — total iterations = outer × inner
for (int i = 1; i <= 3; i++)
    for (int j = 1; j <= 3; j++)
        System.out.println(i + "," + j);   // 9 pairs total
```

#### 📌 `for-each` Loop — Reinforced

```java
// Clean array traversal — no index needed
String[] cities = {"Patna", "Delhi", "Mumbai", "Bangalore"};
for (String city : cities)
    System.out.println("City: " + city);

// Works with any array type
double[] prices = {9.99, 19.99, 4.99};
double total = 0;
for (double p : prices) total += p;
// Cannot modify: city = "Chennai" inside loop — doesn't change array!
```

#### 📌 `break` and `continue` — Reinforced

```java
// break — exits the loop
for (int i = 0; i < 10; i++) {
    if (i == 5) break;
    System.out.print(i + " ");   // 0 1 2 3 4
}

// continue — skips current iteration
for (int i = 0; i < 10; i++) {
    if (i == 5) continue;
    System.out.print(i + " ");   // 0 1 2 3 4 6 7 8 9
}

// break in nested loop — only exits INNER loop
outer:   // labeled break — exits outer loop too
for (int i = 0; i < 3; i++)
    for (int j = 0; j < 3; j++)
        if (i == 1 && j == 1) break outer;
```

---

### 🏆 W3Schools + Challenge Stats

```
╔═══════════════════════════════════════════════════════╗
║      W3SCHOOLS EXERCISE COMPLETION — DAY 03           ║
╠═══════════════════════════════════════════════════════╣
║  Exercise Sets         :  6                           ║
║  Exercise Questions    :  31                          ║
║  Real-Life Articles    :  2 (while + for)             ║
║  Real-Life Challenges  :  2 (while + for)             ║
║  Completed             :  ✅ ALL                       ║
║  Topics Covered        :  while, do-while, for,       ║
║                           nested for, for-each, break ║
╠═══════════════════════════════════════════════════════╣
║  Score     :  ████████████████████  100%              ║
║  Confidence:  ██████████████████░░   90%              ║
╚═══════════════════════════════════════════════════════╝
```

---

## 💡 WHAT I LEARNED TODAY

- Java has **four loop types** — `for`, `while`, `do-while`, `for-each` — each with a specific best use case.
- The `for` loop puts init, condition, and update in **one place** — ideal when count is known.
- The `while` loop is best when **the exit condition depends on runtime behavior**, not a fixed count.
- The `do-while` loop **always runs at least once** — the only loop that checks the condition after the body.
- The `for-each` loop is the cleanest way to read through an array — but you **cannot modify elements** through it.
- `break` **exits the entire loop**. `continue` **skips to the next iteration** — the loop keeps running.
- In nested loops, `break` only exits the **innermost loop** — use a labeled break to exit outer loops.
- The `while (n > 0)` + `n % 10` + `n /= 10` pattern is a fundamental building block for digit-based problems.
- The `√n` optimization (`i * i <= n`) dramatically reduces iterations for primality tests.
- Every extra nesting level **multiplies** the total iterations — O(n²) for double nesting, O(n³) for triple.

---

## ⚠️ COMMON MISTAKES

| Mistake | Wrong Code | Correct Code |
|---------|-----------|--------------|
| Off-by-one in `for` | `i < n` when you want to include n | `i <= n` |
| Forgetting update in `while` | `while (i < 10) { ... }` with no `i++` | Always increment/update inside body |
| Missing semicolon in `do-while` | `} while (x < 5)` | `} while (x < 5);` ← mandatory `;` |
| Modifying array via for-each | `for (int n : arr) n = 0;` | Use regular `for` with index: `arr[i] = 0` |
| `break` exits inner loop only | Thinking `break` exits all loops | Use labeled break or boolean flag for outer |
| Infinite loop | `while (true) { }` with no `break` | Always ensure condition becomes false |
| Wrong max initialization | `int max = 0` for all-negative array | `int max = arr[0]` — always use first element |
| `continue` in `do-while` | Loop body partially skipped | Condition still checked — can cause infinite loop |

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| Wrote multiplication table from memory | ✅ for, while, do-while versions all correct |
| Digit extraction pattern from memory | ✅ `while (n>0) { n%10; n/=10; }` |
| Explained break vs continue difference | ✅ Exit vs skip |
| Wrote nested star pattern from memory | ✅ Pyramid and right triangle |
| Completed all W3Schools loop exercises | ✅ 31 questions + real-life challenges |
| Practiced with Google Drive code files | ✅ |

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. When should you use for vs while vs do-while?</b></summary>

> **Use `for`** when the number of iterations is known before the loop starts — counting, array traversal, patterns. The init-condition-update structure makes it clear and self-contained.
>
> **Use `while`** when the number of iterations depends on something that changes at runtime — processing input until a sentinel, searching until found, game loops that run until a win/loss condition.
>
> **Use `do-while`** when the loop body MUST execute at least once before checking the condition — menus (show before asking to continue), input validation (ask for input once, then validate), game rounds (play at least once).

</details>

<details>
<summary><b>Q2. What is the difference between break and continue?</b></summary>

> **`break`** terminates the entire loop immediately. Execution jumps to the first statement after the closing brace of the loop. Use it when you've found what you need and no further iteration is necessary.
>
> **`continue`** terminates only the current iteration. The loop variable is updated and the condition is checked again — the loop continues running. Use it to skip specific elements (negatives, invalids) without stopping the whole process.
>
> Key: `break` = exit the loop. `continue` = skip this round, keep looping.

</details>

<details>
<summary><b>Q3. Why can't you modify array elements using a for-each loop?</b></summary>

> The for-each loop creates a **copy** of each element in the loop variable. When you write `for (int n : arr)`, Java copies `arr[0]` into `n`, then `arr[1]` into `n`, and so on. Modifying `n` inside the loop only changes that local copy — the original array is untouched.
>
> To modify array elements, you need the index: `for (int i = 0; i < arr.length; i++) arr[i] = 0;`
>
> The for-each loop is intentionally read-only for safety. It tells readers of your code: "I'm only reading this collection, not changing it."

</details>

<details>
<summary><b>Q4. What happens when break is used in a nested loop?</b></summary>

> `break` only exits the **innermost loop** it is in. The outer loop continues normally from its next iteration.
>
> To exit ALL nested loops at once, Java has **labeled breaks**: place a label before the outer loop (`outer:`) and use `break outer;` — this jumps out of the labeled loop entirely.
>
> Alternatively, a boolean flag variable can be set inside the inner loop and checked in the outer loop's condition.

</details>

<details>
<summary><b>Q5. What is the √n optimization in prime checking? Why does it work?</b></summary>

> If a number `n` has a factor greater than `√n`, then it must also have a corresponding factor less than `√n` (because factors come in pairs that multiply to n). So if no factor up to `√n` exists, no factor above `√n` can exist either.
>
> Example: n=36. Factors: 1×36, 2×18, 3×12, 4×9, 6×6. Once we pass √36=6, all new pairs are repeats of old ones reversed. So we only need to check up to 6.
>
> This reduces the loop from O(n) iterations to O(√n) — for n=1,000,000, that's 1M vs 1,000 iterations. A massive speedup used in many number theory problems.

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
║  ✅  for loop — init, condition, update in one line              ║
║  ✅  while loop — condition-driven, manual update                ║
║  ✅  do-while loop — runs at least once, semicolon required      ║
║  ✅  for-each loop — clean read-only array traversal             ║
║  ✅  break — exits loop entirely                                 ║
║  ✅  continue — skips current iteration                          ║
║  ✅  nested loops — total = outer × inner iterations             ║
║  ✅  Key differences table — for vs while vs do-while            ║
║  ✅  Real-life use cases — ATM, compound interest, menus         ║
╠══════════════════════════════════════════════════════════════════╣
║  Practice Problems : 6/6 ✅   Mini Challenge : ✅                 ║
║  W3Schools Sets    : 6/6 ✅   Total Questions: 31 ✅             ║
║  Real-Life Articles: 4/4 ✅   Revision Tasks  : 6/6 ✅           ║
║  Reflection        : 5/5 ✅                                       ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  ████████████████████  95%                    ║
║  STRONG AREA    :  for loop, digit patterns, break/continue      ║
║  WEAK AREA      :  Labeled break — syntax and when to use it    ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  Known count → for. Runtime condition → while. At-least-once→ do-while ║
║  ▸  for-each = read only — use regular for to modify array       ║
║  ▸  break = exit loop. continue = skip this round               ║
║  ▸  break only exits innermost loop — use labels for outer      ║
║  ▸  while(n>0)+n%10+n/=10 → universal digit extraction pattern  ║
║  ▸  i*i<=n not i<=sqrt(n) → √n optimization, no float needed    ║
║  ▸  Always initialize max/min with arr[0], never with 0         ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 02](./Day02_Conditionals.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 04 →](./Day04_Loops_Patterns.md)

*Day 3 of 160 — Done ✅ | W3Schools: 6 Sets · 31 Questions + 4 Real-Life Challenges 🏆 | Streak: 3 Days 🔥🔥🔥*

</div>
