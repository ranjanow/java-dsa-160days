# 📄 Day 11 — Recursion

<div align="center">

![Day](https://img.shields.io/badge/Day-11_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Recursion_%26_Recursive_Thinking-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 10](./Day10_OOP_Inheritance.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 12 →](./Day12_Collections.md)

</div>

---

## 🎯 Day 11 Goal
> Master **recursion** — what it is, how the call stack works, how to identify base cases and recursive cases, and how to solve classic problems using recursive thinking. Recursion is the gateway to Trees, Graphs, Backtracking, and Dynamic Programming. Today you build the mental model that unlocks 40% of DSA.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | What is recursion, call stack, base case, recursive case | 60 min |
| Block 2 | Tracing recursion on paper — the most important skill | 45 min |
| Block 3 | Types — linear, tail, tree recursion | 30 min |
| Block 4 | Revision — Day 10 OOP flash review | 15 min |
| Block 5 | Practice Set — 6 Problems | 120 min |
| Block 6 | Mini Challenge + Reflection | 30 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. What is Recursion?

A method that **calls itself** to solve a smaller version of the same problem until it reaches a condition where it can answer directly — the **base case**.

```java
// Iterative approach — counts down with a loop
void countDownIterative(int n) {
    while (n > 0) {
        System.out.println(n);
        n--;
    }
}

// Recursive approach — counts down by calling itself
void countDown(int n) {
    if (n <= 0) return;          // BASE CASE — stop here
    System.out.println(n);
    countDown(n - 1);            // RECURSIVE CASE — smaller problem
}

countDown(3);
// Output: 3  2  1
```

**Every recursive solution has exactly two parts:**

| Part | Purpose | What happens without it |
|------|---------|------------------------|
| **Base case** | Stops the recursion | Infinite recursion → StackOverflowError |
| **Recursive case** | Breaks problem into smaller piece | Never makes progress |

> ⚠️ **StackOverflowError** = you forgot the base case, or the recursive case doesn't actually move toward it. This is the #1 recursion bug.

---

### 🔷 2. The Call Stack — How Recursion Actually Works

Each recursive call pushes a **new stack frame** onto the call stack. When a base case is hit, frames unwind in reverse order (LIFO — Last In, First Out).

```java
int factorial(int n) {
    if (n == 0) return 1;        // base case
    return n * factorial(n - 1); // recursive case
}

factorial(4);
```

**Call stack trace:**
```
CALL PHASE (building up):              RETURN PHASE (unwinding):
factorial(4) waits for factorial(3) →  factorial(1) returns 1
factorial(3) waits for factorial(2) →  factorial(2) returns 2*1 = 2
factorial(2) waits for factorial(1) →  factorial(3) returns 3*2 = 6
factorial(1) → n==1, calls (n-1=0)  →  factorial(4) returns 4*6 = 24
factorial(0) → n==0, returns 1      →  DONE ✅
```

**Stack frames at deepest point:**
```
TOP → [ factorial(0) : n=0, returns 1       ]
      [ factorial(1) : n=1, waiting...      ]
      [ factorial(2) : n=2, waiting...      ]
      [ factorial(3) : n=3, waiting...      ]
BOTTOM→[ factorial(4) : n=4, waiting...    ]
       [ main()                             ]
```

> 💡 **Key insight:** Each stack frame has its own copy of all local variables. `n` in `factorial(3)` is completely separate from `n` in `factorial(2)`. This is why recursion works — each call is independent.

---

### 🔷 3. The Three Laws of Recursion

```
1. A recursive algorithm MUST have a BASE CASE
   (a condition where it stops and returns directly)

2. A recursive algorithm MUST CHANGE its state and
   MOVE TOWARD the base case
   (each call must make the problem smaller)

3. A recursive algorithm MUST CALL ITSELF
   (that's what makes it recursive)
```

---

### 🔷 4. Anatomy of a Recursive Solution

```java
returnType solve(parameters) {

    // ① BASE CASE — smallest possible input, answer known directly
    if (baseCondition) {
        return directAnswer;
    }

    // ② RECURSIVE CASE — break into smaller problem + combine result
    return combine(currentValue, solve(smallerProblem));
}
```

**Applied to sum of 1 to N:**
```java
int sumToN(int n) {
    if (n == 0) return 0;           // base: sum of 0 numbers = 0
    return n + sumToN(n - 1);       // n + sum(1 to n-1)
}

// Trace for n=4:
// sumToN(4) = 4 + sumToN(3)
// sumToN(3) = 3 + sumToN(2)
// sumToN(2) = 2 + sumToN(1)
// sumToN(1) = 1 + sumToN(0)
// sumToN(0) = 0  ← base case
// Returns up: 1→1, 2→3, 3→6, 4→10 ✅
```

---

### 🔷 5. Types of Recursion

**A. Linear Recursion** — one recursive call per method call
```java
int factorial(int n) {
    if (n == 0) return 1;
    return n * factorial(n - 1);   // ONE recursive call
}
```

**B. Tail Recursion** — recursive call is the LAST operation (no pending work)
```java
// NOT tail recursive — multiplication pending after return
int factorial(int n) {
    return n * factorial(n-1);   // n * (...) = pending work
}

// Tail recursive — accumulator carries the result
int factTail(int n, int acc) {
    if (n == 0) return acc;
    return factTail(n - 1, n * acc);   // LAST operation — no pending work
}
// factTail(5, 1) → factTail(4, 5) → factTail(3, 20) → ...→ 120
```

**C. Tree Recursion** — multiple recursive calls per call (branches like a tree)
```java
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n-1) + fibonacci(n-2);  // TWO recursive calls
}
// Creates a binary tree of calls — exponential calls O(2^n)!
```

```
fibonacci(4):
         fib(4)
        /      \
     fib(3)   fib(2)
    /    \    /    \
 fib(2) fib(1) fib(1) fib(0)
 /   \
fib(1) fib(0)
```

---

### 🔷 6. Recursion vs Iteration — When to Choose

| | Recursion | Iteration |
|--|-----------|-----------|
| **Best for** | Trees, graphs, divide-and-conquer, backtracking | Simple loops, counters, accumulators |
| **Space** | O(depth) — stack frames | O(1) — constant |
| **Readability** | Cleaner for hierarchical problems | Cleaner for linear problems |
| **Risk** | StackOverflow if depth too large | No stack risk |
| **Performance** | Function call overhead | Faster in practice |

> 💡 **Rule of thumb:** If the problem naturally divides into identical sub-problems → recursion. If it's just "do this N times" → iteration.

---

### 🔷 7. Essential Recursive Patterns

```java
// ① Print 1 to N (without loop!)
static void print1ToN(int n) {
    if (n == 0) return;
    print1ToN(n - 1);      // go to base first
    System.out.print(n + " ");  // print on the WAY BACK
}
// print1ToN(5) → 1 2 3 4 5

// ② Print N to 1
static void printNTo1(int n) {
    if (n == 0) return;
    System.out.print(n + " ");  // print on the WAY DOWN
    printNTo1(n - 1);
}
// printNTo1(5) → 5 4 3 2 1

// ③ Power function
static double power(double base, int exp) {
    if (exp == 0) return 1;
    return base * power(base, exp - 1);
}

// ④ Sum of digits
static int sumOfDigits(int n) {
    if (n == 0) return 0;
    return (n % 10) + sumOfDigits(n / 10);
}

// ⑤ Count digits
static int countDigits(int n) {
    if (n == 0) return 0;
    return 1 + countDigits(n / 10);
}

// ⑥ Check palindrome recursively
static boolean isPalin(String s, int left, int right) {
    if (left >= right) return true;
    if (s.charAt(left) != s.charAt(right)) return false;
    return isPalin(s, left + 1, right - 1);
}
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Factorial `[Easy]`

**Task:** Compute `n!` recursively. Handle edge cases `n=0` and `n=1`.

```java
public class P1_Factorial {

    static long factorial(int n) {
        if (n < 0)  throw new IllegalArgumentException("Negative input!");
        if (n == 0) return 1;    // base case: 0! = 1
        return (long) n * factorial(n - 1);
    }

    public static void main(String[] args) {
        for (int i = 0; i <= 10; i++) {
            System.out.printf("%2d! = %d%n", i, factorial(i));
        }
    }
}
```

```
Output:
 0! = 1
 1! = 1
 2! = 2
 3! = 6
 4! = 24
 5! = 120
 6! = 720
 7! = 5040
 8! = 40320
 9! = 362880
10! = 3628800
```

**Call stack trace for factorial(4):**
```
factorial(4)
  └─ 4 * factorial(3)
          └─ 3 * factorial(2)
                    └─ 2 * factorial(1)
                               └─ 1 * factorial(0)
                                          └─ returns 1   ← base case
                               returns 1 * 1 = 1
                    returns 2 * 1 = 2
          returns 3 * 2 = 6
  returns 4 * 6 = 24 ✅
```

> 🔑 **Key Learning:** `long` is used (not `int`) because factorials grow very fast — 13! overflows int. The base case `n==0` returns 1 because the empty product is 1 by mathematical convention. Without the base case, the method calls itself forever: `factorial(-1)` → `factorial(-2)` → ... → StackOverflowError. The `long` cast `(long) n * factorial(n-1)` prevents overflow during multiplication.

---

### ✅ Problem 2 — Fibonacci Series `[Easy]`

**Task:** Compute nth Fibonacci using recursion. Understand exponential complexity.

```java
public class P2_Fibonacci {

    // Simple recursive — O(2^n) time — exponential!
    static int fibRecursive(int n) {
        if (n <= 1) return n;   // fib(0)=0, fib(1)=1
        return fibRecursive(n - 1) + fibRecursive(n - 2);
    }

    // Optimized with memoization — O(n) time
    static int[] memo = new int[100];

    static int fibMemo(int n) {
        if (n <= 1) return n;
        if (memo[n] != 0) return memo[n];      // already computed!
        memo[n] = fibMemo(n-1) + fibMemo(n-2); // store result
        return memo[n];
    }

    public static void main(String[] args) {
        System.out.print("Fibonacci (recursive): ");
        for (int i = 0; i <= 10; i++)
            System.out.print(fibRecursive(i) + " ");

        System.out.println();

        System.out.print("Fibonacci (memoized) : ");
        for (int i = 0; i <= 15; i++)
            System.out.print(fibMemo(i) + " ");
        System.out.println();

        // Calls comparison
        System.out.println("\nfib(10) recursive calls: exponential (~2^10 = 1024)");
        System.out.println("fib(10) memoized calls : only 10 unique calls");
    }
}
```

```
Output:
Fibonacci (recursive): 0 1 1 2 3 5 8 13 21 34 55
Fibonacci (memoized) : 0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610

fib(10) recursive calls: exponential (~2^10 = 1024)
fib(10) memoized calls : only 10 unique calls
```

**Tree of calls for fib(5):**
```
              fib(5)
            /        \
        fib(4)       fib(3)
       /      \      /    \
    fib(3)  fib(2) fib(2) fib(1)
    / \      / \    / \
fib(2) fib(1) ...
```

> 🔑 **Key Learning:** Two base cases: `n==0` returns 0, `n==1` returns 1. Without memoization, fib(5) computes fib(3) TWICE, fib(2) THREE times — exponential waste. **Memoization** stores results in an array — if already computed, return immediately. This is your **first preview of Dynamic Programming** — the biggest DSA topic in Phase 4. The difference: O(2^n) vs O(n) time complexity.

---

### ✅ Problem 3 — Reverse a String Recursively `[Easy-Medium]`

**Task:** Reverse a String without loops, only recursion.

```java
public class P3_ReverseString {

    // Approach 1: build reversed string
    static String reverse(String s) {
        if (s.isEmpty()) return s;           // base: empty string → return as-is
        return reverse(s.substring(1)) + s.charAt(0);
        // reverse the tail + add first char at end
    }

    // Approach 2: two-pointer on char array (in-place)
    static void reverseInPlace(char[] arr, int left, int right) {
        if (left >= right) return;           // base: pointers met/crossed
        char temp   = arr[left];
        arr[left]   = arr[right];
        arr[right]  = temp;
        reverseInPlace(arr, left + 1, right - 1);  // move inward
    }

    public static void main(String[] args) {
        // Approach 1
        System.out.println(reverse("Abhishek"));  // kehsibhA
        System.out.println(reverse("Java"));      // avaJ
        System.out.println(reverse("racecar"));   // racecar (palindrome!)

        // Approach 2
        char[] arr = "Recursion".toCharArray();
        reverseInPlace(arr, 0, arr.length - 1);
        System.out.println(new String(arr));      // noisruceR
    }
}
```

```
Output:
kehsibhA
avaJ
racecar
noisruceR
```

**Trace of `reverse("Java")`:**
```
reverse("Java")
  = reverse("ava") + 'J'
  = (reverse("va") + 'a') + 'J'
  = ((reverse("a") + 'v') + 'a') + 'J'
  = (((reverse("") + 'a') + 'v') + 'a') + 'J'
  = ((("" + 'a') + 'v') + 'a') + 'J'
  = (("a" + 'v') + 'a') + 'J'
  = ("av" + 'a') + 'J'
  = "ava" + 'J'
  = "avaJ" ✅
```

**Trace of `reverseInPlace` on "Java":**
```
left=0, right=3: swap J↔a → "aavJ", recurse(1,2)
left=1, right=2: swap a↔v → "avvJ"... wait:
  arr='J','a','v','a' → swap [0]↔[3] = 'a','a','v','J'
  then swap [1]↔[2] = 'a','v','a','J'... hmm "avaJ" ✅
left=2, right=1: left >= right → base case → DONE
```

> 🔑 **Key Learning:** Approach 1 — peel the first character, reverse the rest, append the first at the end. The recursion builds the result **on the way back up** (during unwind). Approach 2 — recursive two-pointer works identically to the iterative version, but uses the call stack instead of a loop. This recursive two-pointer pattern is the basis of recursive binary search and recursive merge sort.

---

### ✅ Problem 4 — Power Function `[Easy-Medium]`

**Task:** Compute `base^exp` recursively. Then optimize with fast power.

```java
public class P4_Power {

    // Naive — O(n) calls
    static double powerNaive(double base, int exp) {
        if (exp == 0) return 1;                    // base case: x^0 = 1
        if (exp < 0)  return 1.0 / powerNaive(base, -exp);  // negative exp
        return base * powerNaive(base, exp - 1);
    }

    // Fast power (Binary Exponentiation) — O(log n) calls
    static double powerFast(double base, int exp) {
        if (exp == 0) return 1;
        if (exp < 0)  return 1.0 / powerFast(base, -exp);

        if (exp % 2 == 0) {
            double half = powerFast(base, exp / 2);
            return half * half;      // x^n = (x^(n/2))^2 — only ONE recursive call!
        } else {
            return base * powerFast(base, exp - 1);   // x^n = x * x^(n-1)
        }
    }

    public static void main(String[] args) {
        System.out.printf("2^10   = %.0f%n", powerNaive(2, 10));   // 1024
        System.out.printf("3^5    = %.0f%n", powerNaive(3, 5));    // 243
        System.out.printf("2^-3   = %.4f%n", powerNaive(2, -3));   // 0.125

        System.out.println();

        System.out.printf("Fast: 2^10  = %.0f%n", powerFast(2, 10));   // 1024
        System.out.printf("Fast: 2^20  = %.0f%n", powerFast(2, 20));   // 1048576

        System.out.println("\nNaive 2^10: 10 calls");
        System.out.println("Fast  2^10: ~4 calls (log2(10) ≈ 3.3)");
    }
}
```

```
Output:
2^10   = 1024
3^5    = 243
2^-3   = 0.1250

Fast: 2^10  = 1024
Fast: 2^20  = 1048576

Naive 2^10: 10 calls
Fast  2^10: ~4 calls (log2(10) ≈ 3.3)
```

**Fast power trace for 2^10:**
```
powerFast(2, 10):
  10 is even → half = powerFast(2, 5)
    5 is odd  → 2 * powerFast(2, 4)
      4 is even → half = powerFast(2, 2)
        2 is even → half = powerFast(2, 1)
          1 is odd  → 2 * powerFast(2, 0) = 2 * 1 = 2
        half=2 → 2*2 = 4
      half=4 → 4*4 = 16
    → 2 * 16 = 32
  half=32 → 32*32 = 1024 ✅  (only 4 recursive calls!)
```

> 🔑 **Key Learning:** Binary exponentiation halves the exponent each time — O(log n) instead of O(n). For `exp=1,000,000`: naive = 1M calls, fast = only 20 calls. This is **the most important optimization in competitive programming** for power computations. It appears in: modular exponentiation (cryptography), matrix power (Fibonacci in O(log n)), and graph problems. The key insight: `x^n = (x^(n/2))^2` for even n — one call computes both halves.

---

### ✅ Problem 5 — Check Palindrome Recursively `[Medium]`

**Task:** Check if a string is a palindrome using recursion (no loops, no StringBuilder).

```java
public class P5_PalindromeRecursive {

    // Core recursive check using two pointers
    static boolean isPalin(String s, int left, int right) {
        if (left >= right) return true;                    // base: pointers met
        if (s.charAt(left) != s.charAt(right)) return false; // mismatch found
        return isPalin(s, left + 1, right - 1);           // check inner substring
    }

    // Wrapper — clean API
    static boolean isPalindrome(String s) {
        String clean = s.toLowerCase().replaceAll("[^a-z0-9]", "");
        return isPalin(clean, 0, clean.length() - 1);
    }

    public static void main(String[] args) {
        String[] tests = {
            "racecar", "hello", "madam", "",
            "A man a plan a canal Panama",
            "Was it a car or a cat I saw",
            "Abhishek"
        };

        for (String s : tests) {
            System.out.printf("%-35s → %s%n",
                "\"" + s + "\"",
                isPalindrome(s) ? "✅ Palindrome" : "❌ Not Palindrome");
        }
    }
}
```

```
Output:
"racecar"                           → ✅ Palindrome
"hello"                             → ❌ Not Palindrome
"madam"                             → ✅ Palindrome
""                                  → ✅ Palindrome
"A man a plan a canal Panama"       → ✅ Palindrome
"Was it a car or a cat I saw"       → ✅ Palindrome
"Abhishek"                          → ❌ Not Palindrome
```

**Trace for `isPalin("racecar", 0, 6)`:**
```
isPalin("racecar", 0, 6): 'r'=='r' ✅ → isPalin(1, 5)
isPalin("racecar", 1, 5): 'a'=='a' ✅ → isPalin(2, 4)
isPalin("racecar", 2, 4): 'c'=='c' ✅ → isPalin(3, 3)
isPalin("racecar", 3, 3): left>=right → return true ← BASE CASE
All returns unwind → true ✅
```

**Trace for `isPalin("hello", 0, 4)`:**
```
isPalin("hello", 0, 4): 'h'!='o' ❌ → return false immediately
No further recursion needed — early exit ✅
```

> 🔑 **Key Learning:** The recursive palindrome check is the **exact same two-pointer logic** as the iterative version — but the "movement" happens through recursive calls instead of `left++; right--` in a loop. The recursive call `isPalin(s, left+1, right-1)` shrinks the problem by moving both pointers inward. Base case `left >= right` covers both even-length (pointers cross: left>right) and odd-length (center element: left==right). Early return `false` is an optimization — stops recursion immediately on first mismatch.

---

### ✅ Problem 6 — Tower of Hanoi `[Medium-Hard]`

**Task:** Solve Tower of Hanoi for n disks — move all from source to destination using auxiliary.

```java
public class P6_TowerOfHanoi {

    static int moves = 0;

    static void hanoi(int n, char source, char destination, char auxiliary) {
        if (n == 0) return;   // base case: no disk to move

        // Step 1: Move n-1 disks from source to auxiliary (using destination as temp)
        hanoi(n - 1, source, auxiliary, destination);

        // Step 2: Move the nth (largest remaining) disk from source to destination
        moves++;
        System.out.printf("Move disk %d: %c → %c%n", n, source, destination);

        // Step 3: Move n-1 disks from auxiliary to destination (using source as temp)
        hanoi(n - 1, auxiliary, destination, source);
    }

    public static void main(String[] args) {
        System.out.println("=== Tower of Hanoi — 3 Disks ===");
        hanoi(3, 'A', 'C', 'B');
        System.out.println("Total moves: " + moves + " (minimum possible: " + (int)(Math.pow(2,3)-1) + ")");

        System.out.println();
        moves = 0;

        System.out.println("=== Tower of Hanoi — 4 Disks ===");
        hanoi(4, 'A', 'C', 'B');
        System.out.println("Total moves: " + moves + " (minimum: " + (int)(Math.pow(2,4)-1) + ")");
    }
}
```

```
Output:
=== Tower of Hanoi — 3 Disks ===
Move disk 1: A → C
Move disk 2: A → B
Move disk 1: C → B
Move disk 3: A → C
Move disk 1: B → A
Move disk 2: B → C
Move disk 1: A → C
Total moves: 7 (minimum possible: 7)

=== Tower of Hanoi — 4 Disks ===
Move disk 1: A → B
Move disk 2: A → C
Move disk 1: B → C
Move disk 3: A → B
Move disk 1: C → A
Move disk 2: C → B
Move disk 1: A → B
Move disk 4: A → C
...
Total moves: 15 (minimum: 15)
```

**Recursive structure for n=3:**
```
hanoi(3, A→C):
  hanoi(2, A→B)   ← move top 2 to auxiliary
    hanoi(1, A→C) → "disk 1: A→C"
    "disk 2: A→B"
    hanoi(1, C→B) → "disk 1: C→B"
  "disk 3: A→C"   ← move largest to destination
  hanoi(2, B→C)   ← move top 2 to destination
    hanoi(1, B→A) → "disk 1: B→A"
    "disk 2: B→C"
    hanoi(1, A→C) → "disk 1: A→C"

Total: 7 moves. Formula: 2^n - 1
```

> 🔑 **Key Learning:** Tower of Hanoi is the perfect recursion problem because the solution is IMPOSSIBLE to see iteratively but OBVIOUS recursively: (1) move n-1 disks out of the way, (2) move the big disk, (3) move n-1 disks back on top. Total moves = 2^n - 1 — this is the mathematical proof that n disks require exactly this many moves — no fewer. The recursive call tree has depth n and is a perfect binary tree. This problem teaches the essential recursion mindset: **trust that the recursive call solves the sub-problem correctly, focus only on the current level.**

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** GCD (Euclidean Algorithm) + Sum of Array + Binary Search — all recursive.

```java
public class MiniChallenge_Day11 {

    // ① GCD using Euclidean algorithm (recursion shines here)
    static int gcd(int a, int b) {
        if (b == 0) return a;          // base: GCD(a, 0) = a
        return gcd(b, a % b);          // Euclid's insight: GCD(a,b) = GCD(b, a%b)
    }

    // ② Sum of array recursively
    static int arraySum(int[] arr, int index) {
        if (index == arr.length) return 0;       // base: past last element
        return arr[index] + arraySum(arr, index + 1);  // current + rest
    }

    // ③ Recursive Binary Search
    static int binarySearch(int[] arr, int target, int left, int right) {
        if (left > right) return -1;             // base: not found
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) return mid;      // base: found!
        if (arr[mid] < target)
            return binarySearch(arr, target, mid + 1, right);   // search right
        else
            return binarySearch(arr, target, left, mid - 1);    // search left
    }

    public static void main(String[] args) {
        // GCD
        System.out.println("GCD(48, 18) = " + gcd(48, 18));   // 6
        System.out.println("GCD(100, 75) = " + gcd(100, 75)); // 25
        System.out.println("GCD(17, 5)  = " + gcd(17, 5));    // 1 (coprime)

        System.out.println();

        // Array sum
        int[] arr = {10, 20, 30, 40, 50};
        System.out.println("Array: [10, 20, 30, 40, 50]");
        System.out.println("Sum = " + arraySum(arr, 0));       // 150

        System.out.println();

        // Binary search
        int[] sorted = {2, 5, 8, 12, 16, 23, 38, 45, 67, 90};
        System.out.println("Sorted: [2,5,8,12,16,23,38,45,67,90]");
        System.out.println("Search 23 → index: " + binarySearch(sorted, 23, 0, sorted.length-1)); // 5
        System.out.println("Search 45 → index: " + binarySearch(sorted, 45, 0, sorted.length-1)); // 7
        System.out.println("Search 99 → index: " + binarySearch(sorted, 99, 0, sorted.length-1)); // -1
    }
}
```

```
Output:
GCD(48, 18) = 6
GCD(100, 75) = 25
GCD(17, 5)  = 1

Array: [10, 20, 30, 40, 50]
Sum = 150

Sorted: [2,5,8,12,16,23,38,45,67,90]
Search 23 → index: 5
Search 45 → index: 7
Search 99 → index: -1
```

**GCD trace for gcd(48, 18):**
```
gcd(48, 18): b≠0 → gcd(18, 48%18) = gcd(18, 12)
gcd(18, 12): b≠0 → gcd(12, 18%12) = gcd(12, 6)
gcd(12,  6): b≠0 → gcd(6,  12%6)  = gcd(6, 0)
gcd(6,   0): b==0 → return 6 ✅
```

**Binary search trace for target=23 in sorted array:**
```
binarySearch(target=23, left=0, right=9):
  mid=4, arr[4]=16 < 23 → search right
binarySearch(target=23, left=5, right=9):
  mid=7, arr[7]=45 > 23 → search left
binarySearch(target=23, left=5, right=6):
  mid=5, arr[5]=23 == 23 → return 5 ✅  (3 calls only!)
```

> 🔑 **Key Learning:** GCD is the perfect example of recursion expressing a mathematical theorem directly in code — Euclid's algorithm in 2 lines. Binary search recursively halves the search space each call — O(log n). For n=1,000,000 elements, it finds any target in at most 20 calls. The `mid = left + (right-left)/2` formula (instead of `(left+right)/2`) prevents integer overflow for large arrays. These three recursive patterns — GCD, array traversal, binary search — appear constantly in competitive programming and DSA interviews.

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| Written `extends`, `super()` from memory — Animal→Dog | ✅ Correct |
| Overriding vs Overloading — one sentence each | ✅ Runtime vs compile-time |
| Abstract class vs Interface — key difference | ✅ State+partial vs contract+multiple |
| Template Method Pattern explained | ✅ final skeleton + abstract steps |

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. What are the two mandatory parts of every recursive solution?</b></summary>

> **1. Base case** — the condition where recursion stops and a direct answer is returned without any further calls. Without it, the method calls itself infinitely until the call stack overflows (StackOverflowError).
>
> **2. Recursive case** — the call to itself with a SMALLER version of the problem, making progress toward the base case. Without genuine progress, you get infinite recursion even with a base case.
>
> Both must be present. The base case terminates; the recursive case shrinks.

</details>

<details>
<summary><b>Q2. What is the call stack and how does recursion use it?</b></summary>

> The call stack is a LIFO (Last In, First Out) data structure that Java uses to track all active method calls. Each method call pushes a **stack frame** containing local variables, parameters, and the return address.
>
> In recursion: each call pushes a new frame. When the base case is reached, it returns — popping its frame. The popped result is used by the frame below it, which then returns — popping itself, and so on until the original call returns.
>
> For `factorial(4)`: 5 frames are pushed (factorial(4) through factorial(0)), then all 5 unwind returning 1→1→2→6→24. Each frame has its own `n` — completely independent.

</details>

<details>
<summary><b>Q3. What is the difference between tree recursion and linear recursion?</b></summary>

> **Linear recursion** — each method call makes exactly ONE recursive call. The call chain is a single line. Space: O(n), Time: O(n). Examples: factorial, reverse string, sum to N.
>
> **Tree recursion** — each call makes TWO or more recursive calls. The call structure branches like a tree. Space: O(n) stack depth, Time: O(2^n) for naive Fibonacci. The problem: overlapping subproblems — same inputs computed many times. Solution: memoization (cache results) → reduces to O(n). This is the bridge to Dynamic Programming.

</details>

<details>
<summary><b>Q4. Why is binary exponentiation O(log n) instead of O(n)?</b></summary>

> Naive power: compute `base * base * ... * base` n times → O(n) calls.
>
> Fast power uses the identity: `x^n = (x^(n/2))^2` for even n.
> This halves the exponent on every call → the number of calls = number of times you can halve n before reaching 0 = log₂(n).
>
> For n=1,000,000: naive = 1,000,000 calls. Fast = log₂(1,000,000) ≈ 20 calls. The same principle is used in binary search (halve the array), merge sort (halve the data), and all divide-and-conquer algorithms.

</details>

<details>
<summary><b>Q5. How does Tower of Hanoi demonstrate "trust the recursion"?</b></summary>

> Tower of Hanoi teaches the most important recursion mindset: **assume the recursive call correctly solves the subproblem, then focus only on the current level**.
>
> For `hanoi(n, A, C, B)`:
> - "I trust that `hanoi(n-1, A, B, C)` correctly moves n-1 disks to B" → don't trace it, just call it
> - "I move disk n from A to C" → my only responsibility
> - "I trust that `hanoi(n-1, B, C, A)` correctly moves n-1 disks to C" → trust again
>
> If you try to trace all calls for n=4 (15 moves) in your head, you'll get lost. If you trust the subproblem and focus on your level, it's trivial. **This mental model is what unlocks tree traversal, graph DFS, and backtracking in DSA.**

</details>

---

## 📌 DAY 11 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 11 — SUMMARY                       ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 09, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Recursion & Recursive Thinking                  ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  What is recursion — method calls itself                     ║
║  ✅  Base case + recursive case — both mandatory                 ║
║  ✅  Call stack — frames, LIFO, stack overflow                   ║
║  ✅  Three laws of recursion                                     ║
║  ✅  Anatomy — base case first, recursive case breaks problem    ║
║  ✅  Types — linear, tail, tree recursion                        ║
║  ✅  Recursion vs Iteration — when to use which                  ║
║  ✅  7 essential recursive patterns memorized                    ║
╠══════════════════════════════════════════════════════════════════╣
║  PROBLEMS SOLVED                                                ║
║  ✅  P1 — Factorial (call stack trace, long overflow)            ║
║  ✅  P2 — Fibonacci (naive O(2^n) + memoized O(n))              ║
║  ✅  P3 — Reverse String (peel + recurse + append)              ║
║  ✅  P4 — Power (naive O(n) + binary exp O(log n))              ║
║  ✅  P5 — Palindrome (two-pointer recursive)                     ║
║  ✅  P6 — Tower of Hanoi (trust the recursion)                  ║
║  ✅  Mini — GCD + Array Sum + Binary Search (all recursive)     ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  85%                              ║
║  STRONG AREA    :  Factorial, Fibonacci, Binary Search           ║
║  WEAK AREA      :  Tower of Hanoi — tracing deep recursion trees ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  Every recursion needs base case + move toward it            ║
║  ▸  Call stack = LIFO — results unwind bottom-up                ║
║  ▸  Tree recursion + memoization = Dynamic Programming preview  ║
║  ▸  Binary exp O(log n) beats naive O(n) — halve the problem    ║
║  ▸  "Trust the recursion" — focus on current level only         ║
║  ▸  Recursive binary search = same logic as iterative           ║
║  ▸  GCD: Euclidean algo in 2 lines — math expressed as code     ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 10](./Day10_OOP_Inheritance.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 12 →](./Day12_Collections.md)

*Day 11 of 160 — Done ✅ | Streak: 11 Days 🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥*

</div>
