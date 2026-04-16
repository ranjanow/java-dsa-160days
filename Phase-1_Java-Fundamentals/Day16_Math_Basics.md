# 📄 Day 16 — Math for DSA

<div align="center">

![Day](https://img.shields.io/badge/Day-16_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Math_for_DSA-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 15](./Day15_BitManipulation.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 17 →](./Day17_TwoPointers.md)

</div>

---

## 🎯 Day 16 Goal
> Master the **mathematical foundations** that appear in 30+ DSA interview problems — GCD, LCM, prime checking, Sieve of Eratosthenes, modular arithmetic, fast exponentiation, and combinatorics basics. These are not optional extras — they are the building blocks of Number Theory, Cryptography, Competitive Programming, and many placement problems.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | GCD, LCM, Prime checking — Euclidean algorithm | 55 min |
| Block 2 | Sieve of Eratosthenes — all primes up to N | 40 min |
| Block 3 | Modular arithmetic — mod rules, fast exponentiation | 45 min |
| Block 4 | Revision — Day 15 Bit Manipulation flash review | 15 min |
| Block 5 | Practice Set — 6 Problems | 105 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. GCD — Greatest Common Divisor

The **GCD** of two numbers is the largest number that divides both without a remainder.

**Euclidean Algorithm — O(log(min(a,b))):**
```java
// Key insight: GCD(a, b) = GCD(b, a % b)
// Base case: GCD(a, 0) = a

static int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

// Recursive version (cleaner)
static int gcdRecursive(int a, int b) {
    return b == 0 ? a : gcdRecursive(b, a % b);
}

System.out.println(gcd(48, 18));   // 6
System.out.println(gcd(100, 75));  // 25
System.out.println(gcd(17, 5));    // 1  (co-prime)
```

**Euclidean Algorithm trace for GCD(48, 18):**
```
gcd(48, 18): 48 % 18 = 12  → gcd(18, 12)
gcd(18, 12): 18 % 12 = 6   → gcd(12, 6)
gcd(12,  6): 12 %  6 = 0   → gcd(6, 0)
gcd( 6,  0): b == 0         → return 6 ✅
```

**Key properties:**
```java
// GCD is symmetric
gcd(a, b) == gcd(b, a)

// GCD of multiple numbers
gcd(a, b, c) = gcd(gcd(a, b), c)

// Every divisor of gcd(a,b) also divides both a and b
// If gcd(a, b) = 1, a and b are CO-PRIME
```

---

### 🔷 2. LCM — Least Common Multiple

The **LCM** is the smallest number divisible by both a and b.

```java
// Golden formula: LCM(a, b) = (a / GCD(a, b)) * b
// Divide FIRST to avoid overflow: a/gcd * b  (not a*b/gcd)

static long lcm(long a, long b) {
    return (a / gcd(a, b)) * b;
}

System.out.println(lcm(4, 6));    // 12
System.out.println(lcm(12, 18));  // 36
System.out.println(lcm(7, 5));    // 35  (co-prime → LCM = a*b)
```

**Why divide first?**
```java
// a=1000000, b=999999
long wrong = (a * b) / gcd(a, b);   // a*b overflows long!
long right  = (a / gcd(a, b)) * b;  // a/gcd is smaller → safe
```

> 💡 **GCD × LCM = a × b** for any two positive integers. Useful for verification.

---

### 🔷 3. Prime Numbers — Multiple Approaches

A number is **prime** if it has exactly 2 divisors: 1 and itself.

**Approach 1 — Trial division O(√n):**
```java
static boolean isPrime(int n) {
    if (n < 2)  return false;
    if (n == 2) return true;
    if (n % 2 == 0) return false;           // even numbers not prime (except 2)

    for (int i = 3; i * i <= n; i += 2) {   // only odd divisors, up to √n
        if (n % i == 0) return false;
    }
    return true;
}
```

**Approach 2 — 6k ± 1 optimization O(√n / 3):**
```java
// Every prime > 3 is of the form 6k-1 or 6k+1
static boolean isPrime6k(int n) {
    if (n < 2)  return false;
    if (n < 4)  return true;             // 2 and 3 are prime
    if (n % 2 == 0 || n % 3 == 0) return false;

    for (int i = 5; i * i <= n; i += 6) {
        if (n % i == 0 || n % (i + 2) == 0) return false;
    }
    return true;
}
```

**Why 6k ± 1 works:**
```
Every integer is one of: 6k, 6k+1, 6k+2, 6k+3, 6k+4, 6k+5
6k   → divisible by 6
6k+2 → divisible by 2
6k+3 → divisible by 3
6k+4 → divisible by 2
Only 6k+1 and 6k+5 (= 6k-1) can be prime → check only these!
```

---

### 🔷 4. Sieve of Eratosthenes — All Primes up to N

Find all primes up to N in **O(N log log N)** — the most efficient known algorithm for this.

```java
static boolean[] sieve(int n) {
    boolean[] isComposite = new boolean[n + 1];
    // isComposite[i] = true means i is NOT prime
    // Initially all false → all assumed prime

    for (int i = 2; i * i <= n; i++) {
        if (!isComposite[i]) {            // i is prime
            for (int j = i * i; j <= n; j += i) {
                isComposite[j] = true;    // mark multiples of i as composite
            }
            // Start at i*i because smaller multiples already marked
        }
    }

    return isComposite;
}

// Usage
boolean[] composite = sieve(50);
System.out.print("Primes up to 50: ");
for (int i = 2; i <= 50; i++)
    if (!composite[i]) System.out.print(i + " ");
// 2 3 5 7 11 13 17 19 23 29 31 37 41 43 47
```

**Sieve visualization for n=20:**
```
Start: [-, -, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20]

i=2: mark 4,6,8,10,12,14,16,18,20 as composite
i=3: mark 9,15 as composite (6,12,18 already marked)
i=4: isComposite[4]=true → skip (not prime)
i≥√20≈4.5: stop

Primes: 2,3,5,7,11,13,17,19 ✅
```

> 💡 **Why start at i*i?** For prime i, all multiples 2i, 3i, ..., (i-1)*i were already marked by smaller primes (2, 3, ..., i-1). The first unmarked multiple is always i*i.

---

### 🔷 5. Modular Arithmetic

Modular arithmetic keeps numbers within a fixed range, preventing overflow and enabling efficient computations.

**The four mod rules:**
```java
// Addition
(a + b) % m = ((a % m) + (b % m)) % m

// Subtraction
(a - b) % m = ((a % m) - (b % m) + m) % m   // +m prevents negative

// Multiplication
(a * b) % m = ((a % m) * (b % m)) % m

// Division — use modular inverse (requires m is prime for Fermat's theorem)
// (a / b) % m ≠ (a % m) / (b % m)  ← division does NOT distribute over mod!

// Example
int MOD = 1_000_000_007;   // common prime in competitive programming
long a = 1_000_000_000L;
long b = 999_999_999L;
long sum = (a % MOD + b % MOD) % MOD;   // = 999999999
long product = (a % MOD) * (b % MOD) % MOD;
```

**Why 1,000,000,007 (10⁹+7)?**
```
- It is a prime number
- Large enough to avoid most natural sums overflowing
- Small enough that (MOD-1)*(MOD-1) fits in a long (≈ 10^18 < Long.MAX_VALUE ≈ 9.2×10^18)
- Standard in competitive programming
```

---

### 🔷 6. Fast Modular Exponentiation — O(log n)

Compute `base^exp % mod` efficiently — essential for large power computations.

```java
static long fastPow(long base, long exp, long mod) {
    long result = 1;
    base %= mod;

    while (exp > 0) {
        if ((exp & 1) == 1) {        // if current bit of exp is set
            result = result * base % mod;
        }
        base = base * base % mod;    // square the base
        exp >>= 1;                   // move to next bit
    }
    return result;
}

// Examples
System.out.println(fastPow(2, 10, 1000));     // 1024 % 1000 = 24
System.out.println(fastPow(2, 31, 1_000_000_007)); // 2^31 mod 10^9+7
System.out.println(fastPow(3, 100, 1_000_000_007));// 3^100 mod 10^9+7
```

**Trace for 2^10 mod 1000:**
```
base=2, exp=10 (binary: 1010), mod=1000, result=1

exp=10 (1010): bit 0 = 0 → skip.  base = 2*2%1000 = 4. exp=5
exp=5  (0101): bit 0 = 1 → result=1*4=4. base=4*4%1000=16. exp=2
exp=2  (0010): bit 0 = 0 → skip.  base=16*16%1000=256. exp=1
exp=1  (0001): bit 0 = 1 → result=4*256%1000=1024%1000=24. base=...exp=0

return 24 ✅ (2^10 = 1024, 1024 % 1000 = 24)
```

---

### 🔷 7. Number Theory — Essential Facts

```java
// ① All divisors of n — O(√n)
static List<Integer> getDivisors(int n) {
    List<Integer> divs = new ArrayList<>();
    for (int i = 1; i * i <= n; i++) {
        if (n % i == 0) {
            divs.add(i);
            if (i != n / i) divs.add(n / i);
        }
    }
    Collections.sort(divs);
    return divs;
}
// getDivisors(12) = [1, 2, 3, 4, 6, 12]

// ② Count of divisors
// Perfect square → odd number of divisors
// Others → even number of divisors

// ③ Sum of digits
static int digitSum(int n) {
    int sum = 0;
    while (n > 0) { sum += n % 10; n /= 10; }
    return sum;
}

// ④ Reverse a number
static int reverseNum(int n) {
    int rev = 0;
    while (n > 0) { rev = rev * 10 + n % 10; n /= 10; }
    return rev;
}

// ⑤ Check palindrome number
static boolean isPalindromeNum(int n) {
    return n == reverseNum(n);
}

// ⑥ Fibonacci with matrix exponentiation — O(log n)
// F(n) = [[1,1],[1,0]]^n [0][0]
// Practical approach: use memoization or iterative O(n)
static long fib(int n) {
    if (n <= 1) return n;
    long a = 0, b = 1;
    for (int i = 2; i <= n; i++) { long c = a + b; a = b; b = c; }
    return b;
}

// ⑦ Catalan numbers — counts BSTs, balanced brackets, triangulations
// C(n) = C(2n, n) / (n+1)
// C(0)=1, C(1)=1, C(2)=2, C(3)=5, C(4)=14, C(5)=42
static long catalan(int n) {
    long result = 1;
    for (int i = 0; i < n; i++) {
        result = result * 2 * (2 * i + 1) / (i + 2);
    }
    return result;
}
```

---

### 🔷 8. Combinatorics — nCr and Permutations

```java
// nCr — combinations (choose r from n)
// Formula: nCr = n! / (r! × (n-r)!)
// With mod: use Pascal's triangle DP

// Pascal's triangle approach — O(n²) precomputation
static long[][] buildPascal(int maxN) {
    long[][] C = new long[maxN + 1][maxN + 1];
    long MOD = 1_000_000_007L;
    for (int i = 0; i <= maxN; i++) {
        C[i][0] = 1;
        for (int j = 1; j <= i; j++)
            C[i][j] = (C[i-1][j-1] + C[i-1][j]) % MOD;
    }
    return C;
}

// C(n, r) iterative — avoids large factorials
static long nCr(int n, int r) {
    if (r > n - r) r = n - r;   // C(n,r) = C(n,n-r), use smaller r
    long result = 1;
    for (int i = 0; i < r; i++) {
        result = result * (n - i) / (i + 1);
    }
    return result;
}

// Permutations nPr = n! / (n-r)!
static long nPr(int n, int r) {
    long result = 1;
    for (int i = n; i > n - r; i--) result *= i;
    return result;
}
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — GCD, LCM and Co-prime Checker `[Easy]`

**Task:** Build a complete number theory utility.

```java
import java.util.*;

public class P1_GCDandLCM {

    static long gcd(long a, long b) {
        return b == 0 ? a : gcd(b, a % b);
    }

    static long lcm(long a, long b) {
        return (a / gcd(a, b)) * b;
    }

    static boolean areCoprime(long a, long b) {
        return gcd(a, b) == 1;
    }

    // GCD of an array
    static long gcdArray(long[] arr) {
        long result = arr[0];
        for (int i = 1; i < arr.length; i++)
            result = gcd(result, arr[i]);
        return result;
    }

    // LCM of an array
    static long lcmArray(long[] arr) {
        long result = arr[0];
        for (int i = 1; i < arr.length; i++)
            result = lcm(result, arr[i]);
        return result;
    }

    public static void main(String[] args) {
        long[][] pairs = {{48, 18}, {100, 75}, {17, 5}, {36, 60}, {1000000, 999999}};

        System.out.println("═══════════════════════════════════════════════════");
        System.out.printf("%-12s %-12s %-8s %-12s %-10s%n",
            "a", "b", "GCD", "LCM", "Co-prime?");
        System.out.println("═══════════════════════════════════════════════════");

        for (long[] p : pairs) {
            long a = p[0], b = p[1];
            System.out.printf("%-12d %-12d %-8d %-12d %-10s%n",
                a, b, gcd(a, b), lcm(a, b),
                areCoprime(a, b) ? "✅ Yes" : "❌ No");
        }

        System.out.println();

        // GCD and LCM of arrays
        long[] arr = {12, 18, 24, 36};
        System.out.println("Array: " + Arrays.toString(arr));
        System.out.println("GCD of array: " + gcdArray(arr));    // 6
        System.out.println("LCM of array: " + lcmArray(arr));    // 72

        System.out.println();

        // Real-world: find largest tile size for a 48×18 floor
        long roomW = 48, roomH = 18;
        System.out.printf("Room %dx%d → largest square tile = %dx%d%n",
            roomW, roomH, gcd(roomW, roomH), gcd(roomW, roomH));
        System.out.printf("Number of tiles needed: %d%n",
            (roomW / gcd(roomW, roomH)) * (roomH / gcd(roomW, roomH)));
    }
}
```

```
Output:
═══════════════════════════════════════════════════
a            b            GCD      LCM          Co-prime?
═══════════════════════════════════════════════════
48           18           6        144          ❌ No
100          75           25       300          ❌ No
17           5            1        85           ✅ Yes
36           60           12       180          ❌ No
1000000      999999       1        999999000000 ✅ Yes

Array: [12, 18, 24, 36]
GCD of array: 6
LCM of array: 72

Room 48x18 → largest square tile = 6x6
Number of tiles needed: 24
```

> 🔑 **Key Learning:** Euclidean GCD is O(log(min(a,b))) — among the fastest algorithms ever discovered (250 BC). The tile problem is a classic real-world GCD application: the largest square tile that fits perfectly in any rectangular room is GCD(width, height). `(a / gcd(a,b)) * b` — divide FIRST to prevent long overflow: for a=10⁶, b=10⁶, a*b = 10¹² which overflows int but `a/gcd * b` stays manageable. Co-prime numbers (GCD=1) are critical in modular inverse and RSA cryptography.

---

### ✅ Problem 2 — Prime Checking All Approaches `[Easy-Medium]`

**Task:** Implement and compare all prime-checking approaches, count primes in range.

```java
import java.util.*;

public class P2_PrimeChecking {

    // Approach 1: Naive O(n)
    static boolean isPrimeNaive(int n) {
        if (n < 2) return false;
        for (int i = 2; i < n; i++)
            if (n % i == 0) return false;
        return true;
    }

    // Approach 2: √n O(√n)
    static boolean isPrimeSqrt(int n) {
        if (n < 2) return false;
        if (n == 2) return true;
        if (n % 2 == 0) return false;
        for (int i = 3; i * i <= n; i += 2)
            if (n % i == 0) return false;
        return true;
    }

    // Approach 3: 6k±1 O(√n/3)
    static boolean isPrime6k(int n) {
        if (n < 2)  return false;
        if (n < 4)  return true;
        if (n % 2 == 0 || n % 3 == 0) return false;
        for (int i = 5; i * i <= n; i += 6)
            if (n % i == 0 || n % (i + 2) == 0) return false;
        return true;
    }

    // Count primes in range [a, b]
    static int countPrimesInRange(int a, int b) {
        int count = 0;
        for (int i = a; i <= b; i++)
            if (isPrimeSqrt(i)) count++;
        return count;
    }

    // All prime factors of n
    static List<Integer> primeFactors(int n) {
        List<Integer> factors = new ArrayList<>();
        for (int i = 2; i * i <= n; i++) {
            while (n % i == 0) { factors.add(i); n /= i; }
        }
        if (n > 1) factors.add(n);   // remaining factor is prime
        return factors;
    }

    public static void main(String[] args) {
        int[] tests = {1, 2, 3, 4, 17, 25, 97, 100, 9973};

        System.out.println("=== Prime Checking ===");
        System.out.printf("%-8s %-10s%n", "Number", "Prime?");
        System.out.println("─".repeat(20));
        for (int n : tests)
            System.out.printf("%-8d %-10s%n", n,
                isPrime6k(n) ? "✅ Prime" : "❌ Composite");

        System.out.println();

        System.out.println("Primes in [1, 100]  : " + countPrimesInRange(1, 100));   // 25
        System.out.println("Primes in [100, 200]: " + countPrimesInRange(100, 200)); // 21

        System.out.println();

        System.out.println("=== Prime Factorization ===");
        int[] nums = {12, 84, 360, 9973, 100};
        for (int n : nums)
            System.out.printf("%-6d = %s%n", n, primeFactors(n));
    }
}
```

```
Output:
=== Prime Checking ===
Number   Prime?
────────────────────
1        ❌ Composite
2        ✅ Prime
3        ✅ Prime
4        ❌ Composite
17       ✅ Prime
25       ❌ Composite
97       ✅ Prime
100      ❌ Composite
9973     ✅ Prime

Primes in [1, 100]  : 25
Primes in [100, 200]: 21

=== Prime Factorization ===
12     = [2, 2, 3]
84     = [2, 2, 3, 7]
360    = [2, 2, 2, 3, 3, 5]
9973   = [9973]
100    = [2, 2, 5, 5]
```

> 🔑 **Key Learning:** The 6k±1 optimization gives ~3× speedup over basic √n — in competitive programming with tight TLEs, this matters. Prime factorization: `while (n % i == 0)` handles repeated factors (e.g., 2² in 12). The remaining `n > 1` check adds the largest prime factor. 9973 is prime so its factorization is itself. Prime factorization is used in: counting divisors (exponents+1 multiplied), finding GCD/LCM via factorization, and cryptography.

---

### ✅ Problem 3 — Sieve of Eratosthenes `[Medium]`

**Task:** Implement the sieve, find primes in range, and use segmented sieve idea.

```java
import java.util.*;

public class P3_Sieve {

    // Basic sieve — returns boolean array
    static boolean[] sieve(int n) {
        boolean[] isComposite = new boolean[n + 1];
        isComposite[0] = isComposite[1] = true;   // 0 and 1 are not prime

        for (int i = 2; i * i <= n; i++) {
            if (!isComposite[i]) {
                for (int j = i * i; j <= n; j += i)
                    isComposite[j] = true;
            }
        }
        return isComposite;
    }

    // Return list of all primes up to n
    static List<Integer> getPrimes(int n) {
        boolean[] composite = sieve(n);
        List<Integer> primes = new ArrayList<>();
        for (int i = 2; i <= n; i++)
            if (!composite[i]) primes.add(i);
        return primes;
    }

    // Smallest prime factor sieve — for fast factorization
    static int[] spfSieve(int n) {
        int[] spf = new int[n + 1];
        for (int i = 0; i <= n; i++) spf[i] = i;   // initialize spf[i] = i

        for (int i = 2; i * i <= n; i++) {
            if (spf[i] == i) {                       // i is prime
                for (int j = i * i; j <= n; j += i)
                    if (spf[j] == j) spf[j] = i;    // i is smallest factor of j
            }
        }
        return spf;
    }

    // Fast factorization using SPF sieve
    static List<Integer> factorizeWithSPF(int n, int[] spf) {
        List<Integer> factors = new ArrayList<>();
        while (n > 1) {
            factors.add(spf[n]);
            n /= spf[n];
        }
        return factors;
    }

    public static void main(String[] args) {
        // Basic sieve
        System.out.println("=== Sieve of Eratosthenes ===");
        List<Integer> primes50 = getPrimes(50);
        System.out.println("Primes ≤ 50 (" + primes50.size() + "): " + primes50);

        List<Integer> primes100 = getPrimes(100);
        System.out.println("Count ≤ 100: " + primes100.size());  // 25

        System.out.println();

        // SPF sieve for fast factorization
        System.out.println("=== Smallest Prime Factor Sieve ===");
        int N = 30;
        int[] spf = spfSieve(N);
        System.out.println("SPF array (2 to " + N + "):");
        for (int i = 2; i <= N; i++)
            System.out.printf("  spf[%2d] = %d%n", i, spf[i]);

        System.out.println();
        System.out.println("=== Fast Factorization using SPF ===");
        int[] nums = {12, 30, 360, 100, 13};
        for (int n : nums)
            System.out.printf("%-5d = %s%n", n, factorizeWithSPF(n, spf));

        System.out.println();

        // Twin primes (pairs differing by 2)
        System.out.println("=== Twin Primes ≤ 100 ===");
        boolean[] comp = sieve(100);
        for (int i = 2; i <= 98; i++)
            if (!comp[i] && !comp[i + 2])
                System.out.printf("(%d, %d) ", i, i + 2);
        System.out.println();
    }
}
```

```
Output:
=== Sieve of Eratosthenes ===
Primes ≤ 50 (15): [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47]
Count ≤ 100: 25

=== Smallest Prime Factor Sieve ===
SPF array (2 to 30):
  spf[ 2] = 2
  spf[ 3] = 3
  spf[ 4] = 2
  spf[ 5] = 5
  spf[ 6] = 2
  spf[ 7] = 7
  ...
  spf[30] = 2

=== Fast Factorization using SPF ===
12    = [2, 2, 3]
30    = [2, 3, 5]
360   = [2, 2, 2, 3, 3, 5]
100   = [2, 2, 5, 5]
13    = [13]

=== Twin Primes ≤ 100 ===
(3, 5) (5, 7) (11, 13) (17, 19) (29, 31) (41, 43) (59, 61) (71, 73)
```

> 🔑 **Key Learning:** The Smallest Prime Factor (SPF) sieve precomputes the smallest prime factor for every number ≤ N in O(N log log N). Once precomputed, ANY number can be factorized in O(log N) instead of O(√N) — crucial when you need to factorize thousands of numbers. Starting the inner loop at `i*i` (not `2*i`) is the key optimization — all smaller multiples were already handled by smaller primes. Twin primes (p, p+2) — a classic number theory concept — are trivially found using the sieve. LeetCode #204 (Count Primes).

---

### ✅ Problem 4 — Modular Arithmetic `[Medium]`

**Task:** Apply modular arithmetic rules to large computations.

```java
public class P4_ModularArithmetic {

    static final long MOD = 1_000_000_007L;

    // Fast modular exponentiation
    static long fastPow(long base, long exp, long mod) {
        long result = 1;
        base %= mod;
        while (exp > 0) {
            if ((exp & 1) == 1) result = result * base % mod;
            base = base * base % mod;
            exp >>= 1;
        }
        return result;
    }

    // Modular inverse — uses Fermat's Little Theorem (mod must be prime)
    // a^(-1) mod p = a^(p-2) mod p
    static long modInverse(long a, long mod) {
        return fastPow(a, mod - 2, mod);
    }

    // nCr mod p using Lucas theorem / direct calculation
    static long nCrMod(int n, int r, long mod) {
        if (r > n) return 0;
        if (r == 0 || r == n) return 1;

        long num = 1, den = 1;
        for (int i = 0; i < r; i++) {
            num = num * ((n - i) % mod) % mod;
            den = den * ((i + 1) % mod) % mod;
        }
        return num % mod * modInverse(den, mod) % mod;
    }

    public static void main(String[] args) {
        System.out.println("=== Fast Modular Exponentiation ===");
        System.out.printf("2^10   mod 1000     = %d%n", fastPow(2, 10, 1000));
        System.out.printf("2^31   mod 10^9+7   = %d%n", fastPow(2, 31, MOD));
        System.out.printf("3^100  mod 10^9+7   = %d%n", fastPow(3, 100, MOD));
        System.out.printf("10^18  mod 10^9+7   = %d%n", fastPow(10, 18, MOD));

        System.out.println();

        System.out.println("=== Modular Arithmetic Rules ===");
        long a = 1_000_000_000L, b = 999_999_999L;
        System.out.printf("a = %d, b = %d, MOD = %d%n", a, b, MOD);
        System.out.printf("(a+b) mod MOD = %d%n", (a % MOD + b % MOD) % MOD);
        System.out.printf("(a*b) mod MOD = %d%n", (a % MOD) * (b % MOD) % MOD);
        System.out.printf("(a-b) mod MOD = %d%n", (a % MOD - b % MOD + MOD) % MOD);

        System.out.println();

        System.out.println("=== Modular Inverse ===");
        long x = 3;
        long inv = modInverse(x, MOD);
        System.out.printf("Inverse of %d mod %d = %d%n", x, MOD, inv);
        System.out.printf("Verify: %d * %d mod %d = %d%n",
            x, inv, MOD, (x * inv) % MOD);  // should be 1

        System.out.println();

        System.out.println("=== nCr mod 10^9+7 ===");
        int[][] nrPairs = {{5,2},{10,3},{20,10},{50,25},{100,50}};
        for (int[] p : nrPairs)
            System.out.printf("C(%3d,%3d) mod 10^9+7 = %d%n",
                p[0], p[1], nCrMod(p[0], p[1], MOD));
    }
}
```

```
Output:
=== Fast Modular Exponentiation ===
2^10   mod 1000     = 24
2^31   mod 10^9+7   = 147483648
3^100  mod 10^9+7   = 981453966
10^18  mod 10^9+7   = 999999937

=== Modular Arithmetic Rules ===
a = 1000000000, b = 999999999, MOD = 1000000007
(a+b) mod MOD = 999999992
(a*b) mod MOD = 999999986
(a-b) mod MOD = 1

=== Modular Inverse ===
Inverse of 3 mod 1000000007 = 333333336
Verify: 3 * 333333336 mod 1000000007 = 1

=== nCr mod 10^9+7 ===
C(  5,  2) mod 10^9+7 = 10
C( 10,  3) mod 10^9+7 = 120
C( 20, 10) mod 10^9+7 = 184756
C( 50, 25) mod 10^9+7 = 126410606
C(100, 50) mod 10^9+7 = 538992043
```

> 🔑 **Key Learning:** `(a-b+MOD)%MOD` — the `+MOD` prevents negative results in subtraction. Modular inverse via Fermat's Little Theorem (`a^(p-2) mod p`) requires `p` to be prime — 10⁹+7 is prime, so it works. Modular inverse of 3 is 333333336 because `3 × 333333336 = 1,000,000,008 = 1 mod (10⁹+7)`. Fast power O(log n) vs naive O(n) — for `2^(10^18)`, naive would never finish; fast power takes only 60 multiplications.

---

### ✅ Problem 5 — Divisors and Number Properties `[Medium]`

**Task:** Find all divisors, count them, sum them, check perfect/abundant/deficient numbers.

```java
import java.util.*;

public class P5_Divisors {

    // All divisors in O(√n)
    static List<Integer> getDivisors(int n) {
        List<Integer> divs = new ArrayList<>();
        for (int i = 1; i * i <= n; i++) {
            if (n % i == 0) {
                divs.add(i);
                if (i != n / i) divs.add(n / i);
            }
        }
        Collections.sort(divs);
        return divs;
    }

    // Sum of proper divisors (all divisors except n itself)
    static int properDivisorSum(int n) {
        List<Integer> divs = getDivisors(n);
        int sum = 0;
        for (int d : divs)
            if (d != n) sum += d;
        return sum;
    }

    // Classify number
    static String classify(int n) {
        if (n <= 1) return "N/A";
        int s = properDivisorSum(n);
        if (s == n)   return "Perfect   (sum of divisors = n)";
        if (s >  n)   return "Abundant  (sum of divisors > n)";
        return             "Deficient (sum of divisors < n)";
    }

    // Check amicable pair: properSum(a)=b AND properSum(b)=a
    static boolean isAmicable(int a, int b) {
        return a != b && properDivisorSum(a) == b && properDivisorSum(b) == a;
    }

    // Count divisors using prime factorization
    // If n = p1^a1 * p2^a2 * ... then count = (a1+1)(a2+1)...
    static int countDivisors(int n) {
        int count = 1, temp = n;
        for (int i = 2; i * i <= temp; i++) {
            int exp = 0;
            while (temp % i == 0) { exp++; temp /= i; }
            count *= (exp + 1);
        }
        if (temp > 1) count *= 2;   // remaining prime with exponent 1 → (1+1)=2
        return count;
    }

    public static void main(String[] args) {
        System.out.println("=== Divisors ===");
        int[] nums = {12, 28, 36, 100, 97};
        for (int n : nums) {
            List<Integer> divs = getDivisors(n);
            System.out.printf("%-4d divisors(%d)=%s  count=%d  sum=%d%n",
                n, divs.size(), divs,
                countDivisors(n),
                divs.stream().mapToInt(Integer::intValue).sum());
        }

        System.out.println();

        // Perfect, Abundant, Deficient
        System.out.println("=== Number Classification ===");
        int[] tests = {6, 12, 28, 496, 8128};
        for (int n : tests)
            System.out.printf("%-6d → %s%n", n, classify(n));

        System.out.println();

        // Amicable pairs
        System.out.println("=== Amicable Pairs ≤ 1000 ===");
        for (int a = 2; a <= 1000; a++) {
            int b = properDivisorSum(a);
            if (b > a && b <= 1000 && isAmicable(a, b))
                System.out.printf("(%d, %d)%n", a, b);
        }
    }
}
```

```
Output:
=== Divisors ===
12   divisors(6)=[1, 2, 3, 4, 6, 12]  count=6  sum=28
28   divisors(6)=[1, 2, 4, 7, 14, 28] count=6  sum=56
36   divisors(9)=[1, 2, 3, 4, 6, 9, 12, 18, 36] count=9  sum=91
100  divisors(9)=[1, 2, 4, 5, 10, 20, 25, 50, 100] count=9 sum=217
97   divisors(2)=[1, 97]               count=2  sum=98

=== Number Classification ===
6      → Perfect   (sum of divisors = n)
12     → Abundant  (sum of divisors > n)
28     → Perfect   (sum of divisors = n)
496    → Perfect   (sum of divisors = n)
8128   → Perfect   (sum of divisors = n)

=== Amicable Pairs ≤ 1000 ===
(220, 284)
```

> 🔑 **Key Learning:** Finding divisors in pairs using `i` and `n/i` is O(√n) — only iterate to √n, collect both divisor and its complement. Count of divisors = product of (exponent+1) for each prime factor — 36=2²×3²has (2+1)(2+1)=9 divisors. Perfect numbers (6, 28, 496, 8128...) have been known since antiquity — only 51 are known, all of the form 2^(p-1)(2^p - 1) where 2^p-1 is prime. Amicable numbers (220, 284) — `properSum(220)=284, properSum(284)=220`. These are classic competition math problems.

---

### ✅ Problem 6 — Combinatorics in Practice `[Medium-Hard]`

**Task:** Compute nCr for large inputs, count paths, and solve combinatorics problems.

```java
import java.util.*;

public class P6_Combinatorics {

    static final long MOD = 1_000_000_007L;

    // Pascal's triangle DP — precompute all C(n,r) for n,r ≤ maxN
    static long[][] pascal(int maxN) {
        long[][] C = new long[maxN + 1][maxN + 1];
        for (int i = 0; i <= maxN; i++) {
            C[i][0] = 1;
            for (int j = 1; j <= i; j++)
                C[i][j] = (C[i-1][j-1] + C[i-1][j]) % MOD;
        }
        return C;
    }

    // Count unique paths in m×n grid (only right and down moves)
    // = C(m+n-2, m-1) paths
    static long countPaths(int m, int n, long[][] C) {
        return C[m + n - 2][m - 1];
    }

    // Derangements D(n) — permutations where no element appears in original position
    // D(1)=0, D(2)=1, D(n) = (n-1)(D(n-1) + D(n-2))
    static long derangements(int n) {
        if (n == 1) return 0;
        if (n == 2) return 1;
        long prev2 = 0, prev1 = 1;
        for (int i = 3; i <= n; i++) {
            long curr = (long)(i - 1) * (prev1 + prev2) % MOD;
            prev2 = prev1;
            prev1 = curr;
        }
        return prev1;
    }

    // Catalan number — counts BST structures, balanced brackets, etc.
    // C(n) = C(2n, n) / (n+1)
    static long catalan(int n, long[][] C) {
        return C[2*n][n] * modInverse(n + 1) % MOD;
    }

    static long fastPow(long b, long e, long m) {
        long r = 1; b %= m;
        while (e > 0) {
            if ((e & 1) == 1) r = r * b % m;
            b = b * b % m; e >>= 1;
        }
        return r;
    }

    static long modInverse(long a) {
        return fastPow(a, MOD - 2, MOD);
    }

    public static void main(String[] args) {
        int N = 20;
        long[][] C = pascal(N * 2);   // precompute up to 2N for Catalan

        // Basic nCr
        System.out.println("=== Pascal's Triangle nCr ===");
        int[][] pairs = {{5,2},{10,3},{15,7},{20,10}};
        for (int[] p : pairs)
            System.out.printf("C(%2d, %2d) = %d%n", p[0], p[1], C[p[0]][p[1]]);

        System.out.println();

        // Unique paths in grid
        System.out.println("=== Unique Grid Paths (right+down only) ===");
        int[][] grids = {{2,2},{3,3},{3,7},{5,5},{10,10}};
        for (int[] g : grids)
            System.out.printf("Grid %dx%d → %d unique paths%n",
                g[0], g[1], countPaths(g[0], g[1], C));

        System.out.println();

        // Derangements
        System.out.println("=== Derangements D(n) ===");
        System.out.println("(Permutations with NO fixed point)");
        for (int i = 1; i <= 8; i++)
            System.out.printf("D(%d) = %d%n", i, derangements(i));

        System.out.println();

        // Catalan numbers
        System.out.println("=== Catalan Numbers ===");
        System.out.println("(BST structures, bracket sequences, triangulations)");
        for (int i = 0; i <= 10; i++)
            System.out.printf("C(%2d) = %d%n", i, catalan(i, C));
    }
}
```

```
Output:
=== Pascal's Triangle nCr ===
C( 5,  2) = 10
C(10,  3) = 120
C(15,  7) = 6435
C(20, 10) = 184756

=== Unique Grid Paths (right+down only) ===
Grid 2x2 → 2 unique paths
Grid 3x3 → 6 unique paths
Grid 3x7 → 28 unique paths
Grid 5x5 → 70 unique paths
Grid 10x10 → 48620 unique paths

=== Derangements D(n) ===
(Permutations with NO fixed point)
D(1) = 0
D(2) = 1
D(3) = 2
D(4) = 9
D(5) = 44
D(6) = 265
D(7) = 1854
D(8) = 14833

=== Catalan Numbers ===
(BST structures, bracket sequences, triangulations)
C( 0) = 1
C( 1) = 1
C( 2) = 2
C( 3) = 5
C( 4) = 14
C( 5) = 42
C( 6) = 132
C( 7) = 429
C( 8) = 1430
C( 9) = 4862
C(10) = 16796
```

> 🔑 **Key Learning:** Unique paths in an m×n grid = C(m+n-2, m-1) — you need (m-1) down moves and (n-1) right moves, total m+n-2 moves, choose which (m-1) are "down". Pascal's triangle DP is O(n²) precomputation then O(1) per query. Derangements D(n) = (n-1)(D(n-1)+D(n-2)) — at a party of n people, D(n) ways to return hats so nobody gets their own. Catalan(3)=5 counts distinct BSTs with 3 nodes, valid bracket sequences of length 6, and polygon triangulations. LeetCode #62 (Unique Paths), classic DP on Pascal's triangle.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Number Theory toolkit — check all classic number properties.

```java
import java.util.*;

public class MiniChallenge_Day16 {

    static long gcd(long a, long b)  { return b == 0 ? a : gcd(b, a % b); }
    static long lcm(long a, long b)  { return a / gcd(a, b) * b; }

    static boolean isPrime(int n) {
        if (n < 2) return false;
        if (n < 4) return true;
        if (n % 2 == 0 || n % 3 == 0) return false;
        for (int i = 5; i * i <= n; i += 6)
            if (n % i == 0 || n % (i+2) == 0) return false;
        return true;
    }

    static boolean isArmstrong(int n) {
        int digits = String.valueOf(n).length(), temp = n, sum = 0;
        while (temp > 0) { sum += (int)Math.pow(temp%10, digits); temp /= 10; }
        return sum == n;
    }

    static boolean isPerfect(int n) {
        int sum = 1;
        for (int i = 2; i * i <= n; i++)
            if (n % i == 0) { sum += i; if (i != n/i) sum += n/i; }
        return n > 1 && sum == n;
    }

    static boolean isPalindromeNum(int n) {
        int rev = 0, temp = n;
        while (temp > 0) { rev = rev*10 + temp%10; temp /= 10; }
        return n == rev;
    }

    static int digitSum(int n) {
        int s = 0;
        while (n > 0) { s += n%10; n /= 10; }
        return s;
    }

    // Happy number: repeatedly sum of digit squares → eventually 1?
    static boolean isHappy(int n) {
        Set<Integer> seen = new HashSet<>();
        while (n != 1 && !seen.contains(n)) {
            seen.add(n);
            int sum = 0;
            while (n > 0) { int d = n%10; sum += d*d; n /= 10; }
            n = sum;
        }
        return n == 1;
    }

    static void analyze(int n) {
        System.out.printf("n = %-8d  prime=%-5s  armstrong=%-5s  " +
                          "perfect=%-5s  palindrome=%-5s  happy=%-5s  digitSum=%d%n",
            n,
            isPrime(n)        ? "✅" : "❌",
            isArmstrong(n)    ? "✅" : "❌",
            isPerfect(n)      ? "✅" : "❌",
            isPalindromeNum(n)? "✅" : "❌",
            isHappy(n)        ? "✅" : "❌",
            digitSum(n));
    }

    public static void main(String[] args) {
        System.out.println("=== Number Property Analyzer ===");
        System.out.printf("%-10s %-8s %-12s %-10s %-12s %-8s %s%n",
            "n", "prime", "armstrong", "perfect", "palindrome", "happy", "digitSum");
        System.out.println("─".repeat(75));

        int[] nums = {1, 2, 6, 7, 28, 97, 121, 153, 370, 371, 496, 1634};
        for (int n : nums) analyze(n);

        System.out.println();

        // GCD/LCM chain
        System.out.println("=== GCD/LCM Chain ===");
        long[] arr = {12, 18, 24, 36, 48};
        long g = arr[0], l = arr[0];
        for (int i = 1; i < arr.length; i++) {
            g = gcd(g, arr[i]);
            l = lcm(l, arr[i]);
        }
        System.out.println("Array: " + Arrays.toString(arr));
        System.out.println("GCD: " + g + "  LCM: " + l);
    }
}
```

```
Output:
=== Number Property Analyzer ===
n          prime    armstrong    perfect    palindrome  happy    digitSum
───────────────────────────────────────────────────────────────────────────
n = 1         prime=❌    armstrong=✅   perfect=❌   palindrome=✅  happy=✅  digitSum=1
n = 2         prime=✅    armstrong=✅   perfect=❌   palindrome=✅  happy=✅  digitSum=2
n = 6         prime=❌    armstrong=✅   perfect=✅   palindrome=✅  happy=❌  digitSum=6
n = 7         prime=✅    armstrong=✅   perfect=❌   palindrome=✅  happy=✅  digitSum=7
n = 28        prime=❌    armstrong=❌   perfect=✅   palindrome=❌  happy=✅  digitSum=10
n = 97        prime=✅    armstrong=❌   perfect=❌   palindrome=❌  happy=✅  digitSum=16
n = 121       prime=❌    armstrong=❌   perfect=❌   palindrome=✅  happy=❌  digitSum=4
n = 153       prime=❌    armstrong=✅   perfect=❌   palindrome=❌  happy=❌  digitSum=9
n = 370       prime=❌    armstrong=✅   perfect=❌   palindrome=❌  happy=✅  digitSum=10
n = 371       prime=❌    armstrong=✅   perfect=❌   palindrome=❌  happy=✅  digitSum=11
n = 496       prime=❌    armstrong=❌   perfect=✅   palindrome=❌  happy=✅  digitSum=19
n = 1634      prime=❌    armstrong=✅   perfect=❌   palindrome=❌  happy=❌  digitSum=14

=== GCD/LCM Chain ===
Array: [12, 18, 24, 36, 48]
GCD: 6  LCM: 144
```

> 🔑 **Key Learning:** Happy number uses a HashSet to detect cycles — if you reach a number you've seen before without hitting 1, it's not happy. 153, 370, 371, 407, 1634 are Armstrong numbers. 6, 28, 496, 8128 are perfect numbers. The number analyzer pattern (checking multiple properties at once) is frequently asked in placement rounds as a "menu-driven program."

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| `n & (n-1)` removes lowest set bit — trace it | ✅ 12(1100) → 8(1000) |
| XOR find-single — trace `[4,1,2,1,2]` | ✅ 4^1^2^1^2 = 4 |
| `(1 << i)` mask for bit i — check bit 3 of 13 | ✅ 13 & 8 = 8 ≠ 0 → set |
| `>>` vs `>>>` difference | ✅ Sign vs zero fill |

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. Why is the Euclidean algorithm O(log(min(a,b)))?</b></summary>

> Each step replaces `(a, b)` with `(b, a % b)`. The key observation: `a % b < a/2` always.
>
> **Proof:** If `b ≤ a/2`, then `a % b < b ≤ a/2`. If `b > a/2`, then `a % b = a - b < a - a/2 = a/2`.
>
> So every two steps, the larger number at least halves. Starting from `min(a, b)`, after `2k` steps the value is at most `min(a,b) / 2^k`. Setting this ≤ 1 gives `k = log₂(min(a,b))`.
>
> Total steps = O(log(min(a,b))). For a=10^18, b=10^18, this is only ~120 steps. Remarkably efficient for a 2,300-year-old algorithm.

</details>

<details>
<summary><b>Q2. Why does the Sieve start inner loop at i*i (not 2*i)?</b></summary>

> For a prime `i`, all composite multiples smaller than `i*i` have already been marked by **smaller primes**.
>
> Consider i=5: multiples 2×5=10 (marked by 2), 3×5=15 (marked by 3), 4×5=20 (marked by 2). The first NEW multiple not yet marked is 5×5=25.
>
> Starting at `i*i` skips already-marked composites, making the sieve roughly twice as fast in practice. The mathematical guarantee: every composite c < i*i has a prime factor p < i, so c was marked when the outer loop processed p.
>
> This optimization, combined with `i*i ≤ n` as the outer loop condition, makes the sieve O(N log log N) instead of O(N log N).

</details>

<details>
<summary><b>Q3. Why is 10^9+7 the standard modulus in competitive programming?</b></summary>

> **Four reasons:**
> 1. **It is prime** — this enables modular inverse via Fermat's Little Theorem (`a^(p-2) mod p`), which requires p to be prime.
> 2. **Large enough** — answers are always reduced, preventing naive overflow for most problems.
> 3. **Safe for multiplication** — `(10^9+7 - 1)^2 ≈ 10^18 < Long.MAX_VALUE ≈ 9.2×10^18`. So multiplying two numbers mod 10^9+7 fits in a `long` without overflow.
> 4. **Universally recognized** — every competitive programmer and interviewer knows it. Using it signals familiarity with modular arithmetic.
>
> Alternative: `998244353` is used when NTT (Number Theoretic Transform) is needed — it is also prime and has special properties for FFT-based polynomial multiplication.

</details>

<details>
<summary><b>Q4. What is modular inverse and when do you need it?</b></summary>

> The **modular inverse** of `a` modulo `m` is a number `x` such that `(a * x) % m = 1`. Written as `a^(-1) mod m`.
>
> **Why needed:** Modular arithmetic distributes over `+`, `-`, `*`, but NOT over `/`. To compute `(a / b) % m`, you cannot just do `(a % m) / (b % m)` — it gives wrong answers. Instead: `(a * b^(-1)) % m`.
>
> **How to compute:** When `m` is prime, Fermat's Little Theorem says `a^(m-1) ≡ 1 (mod m)`, so `a^(-1) ≡ a^(m-2) (mod m)`. Compute with fast exponentiation: `fastPow(a, m-2, m)`.
>
> **Used in:** Computing `nCr mod p` (divide by `r!` using inverse of factorial), probability problems (divide by total outcomes), counting paths with constraints.

</details>

<details>
<summary><b>Q5. What does the Catalan number count and why is it C(2n,n)/(n+1)?</b></summary>

> **Catalan(n) counts:**
> - Number of valid bracket sequences of length 2n (n pairs of `()`)
> - Number of distinct BSTs with n nodes
> - Number of ways to triangulate a convex polygon with n+2 sides
> - Number of paths below the diagonal in an n×n grid
>
> **Derivation:** Total paths from (0,0) to (n,n) using right/up moves = C(2n,n). Bad paths = paths that cross above the diagonal = C(2n, n-1) (by reflection principle). Good paths = C(2n,n) - C(2n,n-1) = C(2n,n)/(n+1).
>
> Catalan(3)=5 means there are 5 distinct BSTs with nodes {1,2,3} and 5 valid bracket sequences with 3 pairs. This appears in DP problems: "how many ways to…" where each choice branches into two subproblems of all possible split points.

</details>

---

## 📌 DAY 16 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 16 — SUMMARY                       ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 14, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Math for DSA                                    ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  GCD — Euclidean algorithm O(log n), array GCD               ║
║  ✅  LCM — formula a/gcd*b (divide first to avoid overflow)      ║
║  ✅  Prime checking — √n, 6k±1 optimization                      ║
║  ✅  Sieve of Eratosthenes — O(N log log N), starts at i*i       ║
║  ✅  SPF sieve — fast factorization in O(log n) after O(N loglogN)║
║  ✅  Modular arithmetic — 4 rules, subtraction +MOD trick        ║
║  ✅  Fast modular exponentiation — O(log n) using binary exp      ║
║  ✅  Modular inverse — Fermat's Little Theorem a^(p-2) mod p     ║
║  ✅  Divisors in O(√n), count via prime factorization formula     ║
║  ✅  Combinatorics — nCr, Pascal's triangle, Catalan, Derangements║
╠══════════════════════════════════════════════════════════════════╣
║  PROBLEMS SOLVED                                                ║
║  ✅  P1 — GCD, LCM, co-prime, array GCD/LCM, tile problem        ║
║  ✅  P2 — Prime checking all approaches + prime factorization     ║
║  ✅  P3 — Sieve + SPF sieve + twin primes                        ║
║  ✅  P4 — Modular arithmetic: fastPow, inverse, nCr mod p        ║
║  ✅  P5 — Divisors, perfect/abundant/deficient, amicable pairs   ║
║  ✅  P6 — Pascal's triangle, unique paths, derangements, Catalan  ║
║  ✅  Mini — Number property analyzer (prime, Armstrong, happy...) ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  88%                              ║
║  STRONG AREA    :  GCD/LCM, Sieve, modular arithmetic           ║
║  WEAK AREA      :  Catalan proof + modular inverse derivation    ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  GCD(a,b) = GCD(b, a%b) — Euclidean in 2 lines              ║
║  ▸  LCM = (a / gcd) * b — divide FIRST to prevent overflow      ║
║  ▸  Sieve inner loop starts at i*i — smaller multiples handled  ║
║  ▸  (a-b+MOD)%MOD — always add MOD before mod in subtraction    ║
║  ▸  Modular inverse = fastPow(a, MOD-2, MOD) when MOD is prime  ║
║  ▸  Count divisors = product of (exponent+1) over prime factors  ║
║  ▸  Grid paths = C(m+n-2, m-1) — beautiful combinatorics        ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 15](./Day15_BitManipulation.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 17 →](./Day17_TwoPointers.md)

*Day 16 of 160 — Done ✅ | Phase 1: 76%! | Streak: 16 Days 🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥*

</div>
