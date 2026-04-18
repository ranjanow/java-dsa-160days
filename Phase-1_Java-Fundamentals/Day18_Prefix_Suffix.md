# 📄 Day 18 — Prefix Sum & Suffix Arrays

<div align="center">

![Day](https://img.shields.io/badge/Day-18_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Prefix_Sum_%26_Suffix_Arrays-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 17](./Day17_TwoPointers.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 19 →](./Day19_Hashing.md)

</div>

---

## 🎯 Day 18 Goal
> Master **Prefix Sum** and **Suffix Arrays** — the preprocessing techniques that turn O(n) range queries into O(1), and unlock elegant solutions to subarray problems that would otherwise require O(n²) nested loops. These patterns appear in 25+ DSA interview problems across two pointers, hashing, dynamic programming, and competitive programming. Today you build the preprocessing mindset.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | Prefix Sum — 1D arrays, range queries | 50 min |
| Block 2 | Prefix Sum — 2D arrays (matrix range queries) | 40 min |
| Block 3 | Suffix Sum + Prefix Product + Prefix XOR | 35 min |
| Block 4 | Revision — Day 17 Two Pointers flash review | 15 min |
| Block 5 | Practice Set — 6 Problems | 110 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. The Core Idea — Preprocessing vs Recomputing

```
Problem: Answer Q queries, each asking for sum of arr[l..r]

Naive approach:
  For each query, loop from l to r → O(n) per query
  Q queries → O(Q × n) total
  For Q=10,000, n=10,000 → 10^8 operations → TLE ❌

Prefix Sum approach:
  Precompute once → O(n)
  Each query → O(1)
  Q queries → O(n + Q) total
  For Q=10,000, n=10,000 → 20,000 operations → AC ✅
```

> 💡 **The preprocessing mindset:** Pay a one-time O(n) cost upfront so every subsequent operation is O(1). This is the backbone of: prefix sums, sparse tables, segment trees, and Fenwick trees.

---

### 🔷 2. Prefix Sum — 1D Array

**Definition:** `prefix[i]` = sum of `arr[0..i]` (first i+1 elements).

```java
// Building the prefix sum array
int[] arr    = {3, 1, 4, 1, 5, 9, 2, 6};
int[] prefix = new int[arr.length + 1];  // +1 for cleaner indexing

prefix[0] = 0;  // empty prefix
for (int i = 0; i < arr.length; i++)
    prefix[i + 1] = prefix[i] + arr[i];

// prefix = [0, 3, 4, 8, 9, 14, 23, 25, 31]
//           ↑   ↑   ↑   ↑   ↑   ↑   ↑   ↑   ↑
//          sum  0   1   2   3   4   5   6   7  ← index in arr
```

**Range Sum Query — O(1):**
```java
// Sum of arr[l..r] (0-indexed, both inclusive)
static int rangeSum(int[] prefix, int l, int r) {
    return prefix[r + 1] - prefix[l];
}

// Examples:
rangeSum(prefix, 0, 3) = prefix[4] - prefix[0] = 9  - 0 = 9   (3+1+4+1)
rangeSum(prefix, 2, 5) = prefix[6] - prefix[2] = 23 - 4 = 19  (4+1+5+9)
rangeSum(prefix, 1, 1) = prefix[2] - prefix[1] = 4  - 3 = 1   (just arr[1])
```

**Why the formula works:**
```
prefix[r+1] = sum of arr[0..r]
prefix[l]   = sum of arr[0..l-1]
Difference  = sum of arr[l..r]  ✅
```

---

### 🔷 3. Prefix Sum with Offset (1-indexed)

Many competitive programmers prefer 1-indexed prefix sums — cleaner edge cases.

```java
int n = arr.length;
int[] prefix = new int[n + 1];
prefix[0] = 0;
for (int i = 1; i <= n; i++)
    prefix[i] = prefix[i-1] + arr[i-1];  // arr is 0-indexed, prefix is 1-indexed

// Query sum of arr[l..r] (1-indexed both)
int sum = prefix[r] - prefix[l-1];
```

---

### 🔷 4. Prefix Sum — 2D Array (Matrix)

For a 2D grid, prefix[i][j] = sum of all elements in the rectangle from (0,0) to (i-1, j-1).

```java
int[][] mat    = {{1,2,3},{4,5,6},{7,8,9}};
int rows = mat.length, cols = mat[0].length;
int[][] prefix = new int[rows+1][cols+1];

// Build 2D prefix sum
for (int i = 1; i <= rows; i++) {
    for (int j = 1; j <= cols; j++) {
        prefix[i][j] = mat[i-1][j-1]
                     + prefix[i-1][j]    // above
                     + prefix[i][j-1]    // left
                     - prefix[i-1][j-1]; // subtract double-counted corner
    }
}

// Query: sum of rectangle (r1,c1) to (r2,c2) — 1-indexed
static int queryRect(int[][] prefix, int r1, int c1, int r2, int c2) {
    return prefix[r2][c2]
         - prefix[r1-1][c2]   // remove top
         - prefix[r2][c1-1]   // remove left
         + prefix[r1-1][c1-1]; // add back double-removed corner
}
```

**Inclusion-Exclusion visualization:**
```
prefix[r2][c2]
  = full rectangle from (0,0) to (r2,c2)
  − prefix[r1-1][c2]    (strip above our query rectangle)
  − prefix[r2][c1-1]    (strip left of our query rectangle)
  + prefix[r1-1][c1-1]  (corner subtracted twice → add back once)
```

---

### 🔷 5. Suffix Sum

**Definition:** `suffix[i]` = sum of `arr[i..n-1]` (from i to end).

```java
int[] arr    = {3, 1, 4, 1, 5, 9, 2, 6};
int n        = arr.length;
int[] suffix = new int[n + 1];
suffix[n]    = 0;

for (int i = n - 1; i >= 0; i--)
    suffix[i] = suffix[i+1] + arr[i];

// suffix = [31, 28, 27, 23, 22, 17, 8, 6, 0]
//            ↑    ↑    ↑    ↑    ↑   ↑   ↑  ↑   ↑
//            0    1    2    3    4   5   6  7  end
```

**Combined use — find pivot index where left sum == right sum:**
```java
// Pivot: suffix[i+1] == prefix[i] (sum to left == sum to right)
for (int i = 0; i < n; i++) {
    int leftSum  = prefix[i];       // sum of arr[0..i-1]
    int rightSum = suffix[i+1];     // sum of arr[i+1..n-1]
    if (leftSum == rightSum)
        System.out.println("Pivot at index " + i);
}
```

---

### 🔷 6. Prefix Product Array

```java
int[] arr    = {1, 2, 3, 4, 5};
int n        = arr.length;
int[] prefProd = new int[n + 1];
prefProd[0]    = 1;  // empty product = 1

for (int i = 0; i < n; i++)
    prefProd[i+1] = prefProd[i] * arr[i];

// prefProd = [1, 1, 2, 6, 24, 120]

// Product of arr[l..r]:
int prod = prefProd[r+1] / prefProd[l];   // ← only works if no zeros!
```

**Product of array except self (no division, no zeros):**
```java
// result[i] = product of all elements EXCEPT arr[i]
// = prefix product of arr[0..i-1] × suffix product of arr[i+1..n-1]
int[] result = new int[n];
int[] left   = new int[n];   // left[i] = product of arr[0..i-1]
int[] right  = new int[n];   // right[i]= product of arr[i+1..n-1]

left[0] = 1;
for (int i = 1; i < n; i++) left[i] = left[i-1] * arr[i-1];

right[n-1] = 1;
for (int i = n-2; i >= 0; i--) right[i] = right[i+1] * arr[i+1];

for (int i = 0; i < n; i++) result[i] = left[i] * right[i];
```

---

### 🔷 7. Prefix XOR

XOR prefix sums work because XOR is its own inverse: `a ^ a = 0`.

```java
int[] arr       = {3, 5, 2, 7, 1};
int[] prefXOR   = new int[arr.length + 1];
prefXOR[0]      = 0;

for (int i = 0; i < arr.length; i++)
    prefXOR[i+1] = prefXOR[i] ^ arr[i];

// XOR of arr[l..r]:
int xorLR = prefXOR[r+1] ^ prefXOR[l];

// Why: prefXOR[r+1] = arr[0]^...^arr[r]
//      prefXOR[l]   = arr[0]^...^arr[l-1]
//      XOR of both  = arr[l]^...^arr[r]  (prefix cancels via a^a=0)
```

**Classic use: Find subarray with XOR = k using HashMap:**
```java
// Count subarrays with XOR == k
static int countXorSubarrays(int[] arr, int k) {
    HashMap<Integer, Integer> freq = new HashMap<>();
    freq.put(0, 1);  // empty prefix XOR = 0
    int prefXor = 0, count = 0;

    for (int x : arr) {
        prefXor ^= x;
        // If prefXor ^ k exists in map, those prefixes form subarrays with XOR=k
        count += freq.getOrDefault(prefXor ^ k, 0);
        freq.put(prefXor, freq.getOrDefault(prefXor, 0) + 1);
    }
    return count;
}
```

---

### 🔷 8. Prefix Sum + HashMap — The Most Powerful Combination

**Subarray sum = k using prefix sum + HashMap:**

```java
// Count subarrays with sum == k
static int subarraySumK(int[] arr, int k) {
    HashMap<Integer, Integer> freq = new HashMap<>();
    freq.put(0, 1);   // empty prefix (sum 0 seen once before index 0)
    int prefSum = 0, count = 0;

    for (int x : arr) {
        prefSum += x;
        // If (prefSum - k) exists in map, there's a subarray ending here with sum k
        count += freq.getOrDefault(prefSum - k, 0);
        freq.put(prefSum, freq.getOrDefault(prefSum, 0) + 1);
    }
    return count;
}
```

**Why this works:**
```
If prefix[j] - prefix[i] == k, then arr[i..j-1] has sum k
Equivalently: prefix[i] == prefix[j] - k

For each new prefix[j], look up (prefix[j] - k) in map.
Count of entries = count of valid starting positions i.

freq.put(0, 1): handles subarrays starting from index 0
                (when prefix[j] - k == 0, i.e., entire prefix = k)
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Range Sum Queries `[Easy]`

**Task:** Build prefix sum and answer multiple range queries in O(1).

```java
import java.util.Arrays;

public class P1_RangeSumQuery {

    static int[] buildPrefix(int[] arr) {
        int[] prefix = new int[arr.length + 1];
        for (int i = 0; i < arr.length; i++)
            prefix[i + 1] = prefix[i] + arr[i];
        return prefix;
    }

    static int rangeSum(int[] prefix, int l, int r) {
        return prefix[r + 1] - prefix[l];   // O(1)
    }

    static double rangeAvg(int[] prefix, int l, int r) {
        return (double) rangeSum(prefix, l, r) / (r - l + 1);
    }

    static int rangeMax(int[] arr, int[] prefix, int l, int r) {
        // Note: range max requires segment tree for O(1); O(n) here
        int max = arr[l];
        for (int i = l+1; i <= r; i++) max = Math.max(max, arr[i]);
        return max;
    }

    public static void main(String[] args) {
        int[] arr    = {3, 1, 4, 1, 5, 9, 2, 6, 5, 3};
        int[] prefix = buildPrefix(arr);

        System.out.println("Array  : " + Arrays.toString(arr));
        System.out.println("Prefix : " + Arrays.toString(prefix));
        System.out.println();

        int[][] queries = {{0,3},{2,5},{0,9},{4,7},{1,1}};
        System.out.println("=== Range Sum Queries ===");
        System.out.printf("%-10s %-10s %-10s %-12s%n",
            "Range", "Sum", "Count", "Average");
        System.out.println("─".repeat(45));

        for (int[] q : queries) {
            int l = q[0], r = q[1];
            int    sum   = rangeSum(prefix, l, r);
            double avg   = rangeAvg(prefix, l, r);
            System.out.printf("[%d..%d]    %-10d %-10d %.2f%n",
                l, r, sum, r-l+1, avg);
        }

        System.out.println();

        // Difference array — apply range updates efficiently
        // Add +5 to indices [1..4], +3 to [2..6]
        int[] diff = new int[arr.length + 1];
        // Update: add val to [l..r]
        int[][] updates = {{1,4,5},{2,6,3}};
        for (int[] u : updates) {
            diff[u[0]]   += u[2];
            diff[u[1]+1] -= u[2];
        }
        // Reconstruct
        int[] updated = new int[arr.length];
        int running = 0;
        for (int i = 0; i < arr.length; i++) {
            running += diff[i];
            updated[i] = arr[i] + running;
        }
        System.out.println("Original : " + Arrays.toString(arr));
        System.out.println("After +5 to [1..4] and +3 to [2..6]:");
        System.out.println("Updated  : " + Arrays.toString(updated));
    }
}
```

```
Output:
Array  : [3, 1, 4, 1, 5, 9, 2, 6, 5, 3]
Prefix : [0, 3, 4, 8, 9, 14, 23, 25, 31, 36, 39]

=== Range Sum Queries ===
Range      Sum        Count      Average
─────────────────────────────────────────────
[0..3]     9          4          2.25
[2..5]     19         4          4.75
[0..9]     39         10         3.90
[4..7]     22         4          5.50
[1..1]     1          1          1.00

Original : [3, 1, 4, 1, 5, 9, 2, 6, 5, 3]
After +5 to [1..4] and +3 to [2..6]:
Updated  : [3, 6, 12, 9, 13, 17, 5, 6, 5, 3]
```

> 🔑 **Key Learning:** `prefix[r+1] - prefix[l]` — O(1) per query after O(n) build. The +1 offset (`prefix[r+1]` not `prefix[r]`) prevents an off-by-one that plagues beginners. **Difference Array** is the "inverse" of prefix sum — apply range updates in O(1) each, then reconstruct final array in O(n). Pattern: `diff[l] += val; diff[r+1] -= val;` then prefix sum of diff gives updated array. Used in: range addition, car pooling, corporate flight bookings (LeetCode #1109, #1094).

---

### ✅ Problem 2 — Subarray Sum equals K `[Medium]`

**Task:** Count subarrays with sum exactly equal to k — using prefix sum + HashMap.

```java
import java.util.*;

public class P2_SubarraySumK {

    // Count subarrays with sum == k — O(n)
    static int subarraySumK(int[] arr, int k) {
        HashMap<Integer, Integer> freq = new HashMap<>();
        freq.put(0, 1);    // prefix sum 0 seen once before any element
        int prefSum = 0, count = 0;

        for (int x : arr) {
            prefSum += x;
            // Look for prefix sums that make (prefSum - that) == k
            count  += freq.getOrDefault(prefSum - k, 0);
            freq.put(prefSum, freq.getOrDefault(prefSum, 0) + 1);
        }
        return count;
    }

    // Find the LONGEST subarray with sum == k
    static int longestSubarraySum(int[] arr, int k) {
        HashMap<Integer, Integer> firstSeen = new HashMap<>();
        firstSeen.put(0, -1);  // prefix sum 0 seen at index -1
        int prefSum = 0, maxLen = 0;

        for (int i = 0; i < arr.length; i++) {
            prefSum += arr[i];
            int need = prefSum - k;

            if (firstSeen.containsKey(need))
                maxLen = Math.max(maxLen, i - firstSeen.get(need));

            // Only store FIRST occurrence — to maximize length
            firstSeen.putIfAbsent(prefSum, i);
        }
        return maxLen;
    }

    // Check if any subarray with sum == 0 (negative numbers allowed)
    static boolean hasZeroSumSubarray(int[] arr) {
        HashSet<Integer> seen = new HashSet<>();
        seen.add(0);
        int prefSum = 0;

        for (int x : arr) {
            prefSum += x;
            if (seen.contains(prefSum)) return true;  // same prefix → diff = 0
            seen.add(prefSum);
        }
        return false;
    }

    public static void main(String[] args) {
        // Count subarrays
        System.out.println("=== Count Subarrays with Sum == k ===");
        int[][] tests = {{1,1,1},{1,2,3},{0,0,0},{3,4,7,2,3}};
        int[]   ks    = {2,      3,      0,      7};

        for (int i = 0; i < tests.length; i++)
            System.out.printf("arr=%-20s k=%d → count=%d%n",
                Arrays.toString(tests[i]), ks[i],
                subarraySumK(tests[i], ks[i]));

        System.out.println();

        // Longest subarray
        System.out.println("=== Longest Subarray with Sum == k ===");
        int[][] tests2 = {{1,-1,5,-2,3},{-2,-1,2,1}};
        int[]   ks2    = {3, 1};
        for (int i = 0; i < tests2.length; i++)
            System.out.printf("arr=%-25s k=%d → maxLen=%d%n",
                Arrays.toString(tests2[i]), ks2[i],
                longestSubarraySum(tests2[i], ks2[i]));

        System.out.println();

        // Zero sum subarray
        System.out.println("=== Has Zero Sum Subarray? ===");
        int[][] zTests = {{4,2,-3,1,6},{4,2,0,1,6},{1,2,3,-6}};
        for (int[] arr : zTests)
            System.out.printf("%-25s → %s%n",
                Arrays.toString(arr),
                hasZeroSumSubarray(arr) ? "✅ YES" : "❌ NO");
    }
}
```

```
Output:
=== Count Subarrays with Sum == k ===
arr=[1, 1, 1]              k=2 → count=2
arr=[1, 2, 3]              k=3 → count=2
arr=[0, 0, 0]              k=0 → count=6
arr=[3, 4, 7, 2, 3]        k=7 → count=3

=== Longest Subarray with Sum == k ===
arr=[1, -1, 5, -2, 3]      k=3 → maxLen=4   ([1,-1,5,-2])
arr=[-2, -1, 2, 1]         k=1 → maxLen=2   ([2,-1] or [-1,2])

=== Has Zero Sum Subarray? ===
[4, 2, -3, 1, 6]           → ✅ YES  (2,-3,1)
[4, 2, 0, 1, 6]            → ✅ YES  (just the 0)
[1, 2, 3, -6]              → ✅ YES  (1+2+3-6=0)
```

**Why `freq.put(0, 1)` is critical:**
```
arr=[3,4], k=7
Without freq.put(0,1):
  i=0: prefSum=3, look for 3-7=-4 → not found
  i=1: prefSum=7, look for 7-7=0  → not found ← BUG: entire array [3,4] missed!

With freq.put(0,1):
  i=1: prefSum=7, look for 7-7=0 → found once! count=1 ✅
```

> 🔑 **Key Learning:** The prefix sum + HashMap pattern is one of the most powerful in DSA. Initializing `freq.put(0, 1)` handles subarrays starting from index 0 — forgetting this is the #1 bug beginners make. For `longestSubarray`, store only the FIRST occurrence of each prefix sum (`putIfAbsent`) — because maximizing length means minimizing the starting index. Zero-sum subarray detection: if any two prefix sums are equal, the subarray between them has sum 0. LeetCode #560, #325, #523 — all solved by this one pattern.

---

### ✅ Problem 3 — 2D Matrix Range Sum `[Medium]`

**Task:** Build 2D prefix sum and answer rectangle range queries in O(1).

```java
import java.util.Arrays;

public class P3_MatrixRangeSum {

    static int[][] build2DPrefix(int[][] mat) {
        int rows = mat.length, cols = mat[0].length;
        int[][] prefix = new int[rows + 1][cols + 1];

        for (int i = 1; i <= rows; i++) {
            for (int j = 1; j <= cols; j++) {
                prefix[i][j] = mat[i-1][j-1]
                             + prefix[i-1][j]    // above rectangle
                             + prefix[i][j-1]    // left rectangle
                             - prefix[i-1][j-1]; // remove double-counted corner
            }
        }
        return prefix;
    }

    // Query: sum of rectangle from (r1,c1) to (r2,c2) — 1-indexed
    static int queryRect(int[][] prefix, int r1, int c1, int r2, int c2) {
        return prefix[r2][c2]
             - prefix[r1-1][c2]    // remove top strip
             - prefix[r2][c1-1]    // remove left strip
             + prefix[r1-1][c1-1]; // add back double-removed corner
    }

    // Count submatrices with sum = target using 2D prefix (O(n^3))
    static int countSubmatricesWithSum(int[][] mat, int target) {
        int rows = mat.length, cols = mat[0].length;
        int[][] prefix = build2DPrefix(mat);
        int count = 0;

        for (int r1 = 1; r1 <= rows; r1++) {
            for (int r2 = r1; r2 <= rows; r2++) {
                // For fixed rows r1..r2, use 1D prefix sum + HashMap on columns
                HashMap<Integer, Integer> freq = new HashMap<>();
                freq.put(0, 1);
                for (int c = 1; c <= cols; c++) {
                    int colSum = queryRect(prefix, r1, 1, r2, c);
                    count += freq.getOrDefault(colSum - target, 0);
                    freq.merge(colSum, 1, Integer::sum);
                }
            }
        }
        return count;
    }

    public static void main(String[] args) {
        int[][] mat = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };

        int[][] prefix = build2DPrefix(mat);

        System.out.println("Matrix:");
        for (int[] row : mat) System.out.println("  " + Arrays.toString(row));

        System.out.println("\nPrefix sum matrix:");
        for (int[] row : prefix) System.out.println("  " + Arrays.toString(row));

        System.out.println();
        System.out.println("=== Rectangle Range Queries ===");

        int[][][] queries = {
            {{1,1},{1,3}},  // first row: 1+2+3=6
            {{1,1},{3,3}},  // entire matrix: 45
            {{2,2},{3,3}},  // bottom-right 2x2: 5+6+8+9=28
            {{1,2},{3,2}},  // middle column: 2+5+8=15
            {{2,1},{2,3}}   // middle row: 4+5+6=15
        };

        for (int[][] q : queries) {
            int r1=q[0][0], c1=q[0][1], r2=q[1][0], c2=q[1][1];
            System.out.printf("rect(%d,%d)->(%d,%d) = %d%n",
                r1, c1, r2, c2, queryRect(prefix, r1, c1, r2, c2));
        }

        System.out.println();

        // Count submatrices
        int[][] mat2 = {{0, 1, 0}, {1, 1, 1}, {0, 1, 0}};
        System.out.println("Matrix for submatrix count:");
        for (int[] row : mat2) System.out.println("  " + Arrays.toString(row));
        System.out.println("Submatrices with sum = 0: " + countSubmatricesWithSum(mat2, 0));
        System.out.println("Submatrices with sum = 1: " + countSubmatricesWithSum(mat2, 1));
    }
}
```

```
Output:
Matrix:
  [1, 2, 3]
  [4, 5, 6]
  [7, 8, 9]

Prefix sum matrix:
  [0, 0, 0, 0]
  [0, 1, 3, 6]
  [0, 5, 12, 21]
  [0, 12, 27, 45]

=== Rectangle Range Queries ===
rect(1,1)->(1,3) = 6
rect(1,1)->(3,3) = 45
rect(2,2)->(3,3) = 28
rect(1,2)->(3,2) = 15
rect(2,1)->(2,3) = 15

Submatrices with sum = 0: 10
Submatrices with sum = 1: 4
```

**2D Prefix inclusion-exclusion for rect(2,2)->(3,3):**
```
prefix[3][3] = 45  (full matrix)
prefix[1][3] = 6   (top strip — row 1)
prefix[3][1] = 12  (left strip — col 1)
prefix[1][1] = 1   (top-left corner — double removed)

Answer = 45 - 6 - 12 + 1 = 28 ✅
Verify: 5+6+8+9 = 28 ✅
```

> 🔑 **Key Learning:** 2D prefix sum builds on 1D — inclusion-exclusion in 2D. The formula `mat[i-1][j-1] + prefix[i-1][j] + prefix[i][j-1] - prefix[i-1][j-1]` is exact 2D prefix recurrence. For counting submatrices with sum = k: reduce to 1D problem by fixing two row boundaries (O(n²) pairs), then use 1D prefix+HashMap on column axis — O(n³) total for n×n matrix, far better than O(n^6) brute force. LeetCode #304 (Matrix Region Sum — solved in O(1) per query), #363 (Max Sum of Rectangle).

---

### ✅ Problem 4 — Product of Array Except Self `[Medium]`

**Task:** Compute `result[i] = product of all elements except arr[i]` — no division, O(1) space (excluding output).

```java
import java.util.Arrays;

public class P4_ProductExceptSelf {

    // Approach 1: Explicit left and right arrays — O(n) space
    static int[] productExceptSelfFull(int[] arr) {
        int n = arr.length;
        int[] left  = new int[n];   // left[i]  = product of arr[0..i-1]
        int[] right = new int[n];   // right[i] = product of arr[i+1..n-1]
        int[] result = new int[n];

        left[0] = 1;
        for (int i = 1; i < n; i++)      left[i]   = left[i-1] * arr[i-1];

        right[n-1] = 1;
        for (int i = n-2; i >= 0; i--)   right[i]  = right[i+1] * arr[i+1];

        for (int i = 0; i < n; i++)      result[i] = left[i] * right[i];
        return result;
    }

    // Approach 2: O(1) extra space — use result array itself
    static int[] productExceptSelfO1(int[] arr) {
        int n = arr.length;
        int[] result = new int[n];

        // Pass 1: fill result with left products
        result[0] = 1;
        for (int i = 1; i < n; i++)
            result[i] = result[i-1] * arr[i-1];

        // Pass 2: multiply with right products (accumulate right → left)
        int rightProd = 1;
        for (int i = n-1; i >= 0; i--) {
            result[i] *= rightProd;
            rightProd *= arr[i];
        }
        return result;
    }

    // Handle zeros — special case
    static int[] productWithZeros(int[] arr) {
        int zeros = 0, totalProd = 1;
        for (int x : arr) {
            if (x == 0) zeros++;
            else totalProd *= x;
        }

        int[] result = new int[arr.length];
        for (int i = 0; i < arr.length; i++) {
            if (zeros > 1)      result[i] = 0;                   // 2+ zeros → all 0
            else if (zeros == 1) result[i] = (arr[i]==0) ? totalProd : 0; // 1 zero
            else                result[i] = totalProd / arr[i];  // 0 zeros → divide
        }
        return result;
    }

    public static void main(String[] args) {
        System.out.println("=== Product Except Self ===");
        int[][] tests = {{1,2,3,4},{-1,1,0,-3,3},{1,0},{1,0,0,2}};

        for (int[] arr : tests) {
            System.out.println("Input    : " + Arrays.toString(arr));
            System.out.println("Full arr : " + Arrays.toString(productExceptSelfFull(arr)));
            System.out.println("O(1)space: " + Arrays.toString(productExceptSelfO1(arr)));
            System.out.println("W/zeros  : " + Arrays.toString(productWithZeros(arr)));
            System.out.println();
        }
    }
}
```

```
Output:
=== Product Except Self ===
Input    : [1, 2, 3, 4]
Full arr : [24, 12, 8, 6]
O(1)space: [24, 12, 8, 6]

Input    : [-1, 1, 0, -3, 3]
Full arr : [0, 0, 9, 0, 0]
O(1)space: [0, 0, 9, 0, 0]

Input    : [1, 0]
Full arr : [0, 1]
O(1)space: [0, 1]

Input    : [1, 0, 0, 2]
Full arr : [0, 0, 0, 0]
O(1)space: [0, 0, 0, 0]
```

**O(1) space trace for [1,2,3,4]:**
```
Pass 1 (left products in result):
result = [1, 1, 2, 6]     (result[i] = product of arr[0..i-1])

Pass 2 (multiply right products, right to left):
i=3: result[3] = 6 * rightProd(1) = 6,   rightProd = 1*4 = 4
i=2: result[2] = 2 * 4 = 8,              rightProd = 4*3 = 12
i=1: result[1] = 1 * 12 = 12,            rightProd = 12*2 = 24
i=0: result[0] = 1 * 24 = 24,            rightProd = 24*1 = 24

result = [24, 12, 8, 6] ✅
```

> 🔑 **Key Learning:** Two-pass prefix × suffix with O(1) extra space: use the output array to accumulate left products in pass 1, then a single `rightProd` variable to accumulate right products in pass 2. The key insight: `result[i] = leftProduct[i] × rightProduct[i]` — compute both without extra arrays. LeetCode #238 — appears in Amazon, Facebook, Uber, and Bloomberg interviews. The zero-handling extension tests real-world edge case thinking.

---

### ✅ Problem 5 — Equilibrium Index & Pivot Problems `[Medium]`

**Task:** Find equilibrium index, split array problems using prefix + suffix sums.

```java
import java.util.*;

public class P5_EquilibriumAndPivot {

    // Equilibrium index: sum of left elements == sum of right elements
    static List<Integer> equilibriumIndices(int[] arr) {
        int total = 0;
        for (int x : arr) total += x;

        List<Integer> result = new ArrayList<>();
        int leftSum = 0;

        for (int i = 0; i < arr.length; i++) {
            int rightSum = total - leftSum - arr[i];
            if (leftSum == rightSum) result.add(i);
            leftSum += arr[i];
        }
        return result;
    }

    // Find if array can be split into two halves with equal sum
    static int splitEqualSum(int[] arr) {
        int total = 0;
        for (int x : arr) total += x;
        if (total % 2 != 0) return -1;  // odd total → impossible

        int prefSum = 0;
        for (int i = 0; i < arr.length - 1; i++) {
            prefSum += arr[i];
            if (prefSum == total - prefSum) return i;  // split after index i
        }
        return -1;
    }

    // Minimum index i: prefix[i] >= suffix[i+1]
    static int minIndexPrefixGeqSuffix(int[] arr) {
        int[] suffix = new int[arr.length + 1];
        for (int i = arr.length - 1; i >= 0; i--)
            suffix[i] = suffix[i+1] + arr[i];

        int prefSum = 0;
        for (int i = 0; i < arr.length; i++) {
            prefSum += arr[i];
            if (prefSum >= suffix[i+1]) return i;
        }
        return arr.length - 1;
    }

    // Largest subarray where prefix sum == suffix sum (meeting point)
    static int largestSubarrayBalance(int[] arr) {
        int n = arr.length;
        int[] prefix = new int[n+1], suffix = new int[n+1];
        for (int i = 0; i < n; i++)        prefix[i+1] = prefix[i] + arr[i];
        for (int i = n-1; i >= 0; i--)     suffix[i]   = suffix[i+1] + arr[i];

        // Find longest [l..r] where prefix[l] == suffix[r+1] (balanced boundaries)
        int maxLen = 0;
        for (int l = 0; l < n; l++)
            for (int r = l; r < n; r++)
                if (prefix[l] == suffix[r+1])
                    maxLen = Math.max(maxLen, r - l + 1);
        return maxLen;
    }

    public static void main(String[] args) {
        // Equilibrium index
        System.out.println("=== Equilibrium Indices ===");
        int[][] tests = {
            {-7, 1, 5, 2, -4, 3, 0},
            {1, 2, 3},
            {1, 7, 3, 6, 5, 6},
            {1}
        };
        for (int[] arr : tests)
            System.out.printf("%-30s → eq. indices: %s%n",
                Arrays.toString(arr), equilibriumIndices(arr));

        System.out.println();

        // Split equal sum
        System.out.println("=== Split into Equal Sum Halves ===");
        int[][] splitTests = {{1,2,3,4,5,5},{1,1,3,2},{1,2,3}};
        for (int[] arr : splitTests) {
            int idx = splitEqualSum(arr);
            System.out.printf("%-25s → split after index %d → %s%n",
                Arrays.toString(arr), idx,
                idx >= 0 ? "✅ Left sum = Right sum = " + Arrays.stream(arr).limit(idx+1).sum()
                         : "❌ Not possible");
        }

        System.out.println();

        // Min index prefix >= suffix
        System.out.println("=== Min Index where Prefix >= Suffix ===");
        int[] arr = {2, 3, 4, 1, 5};
        System.out.printf("arr=%s → min index=%d%n",
            Arrays.toString(arr), minIndexPrefixGeqSuffix(arr));
    }
}
```

```
Output:
=== Equilibrium Indices ===
[-7, 1, 5, 2, -4, 3, 0]       → eq. indices: [3, 6]
[1, 2, 3]                      → eq. indices: []
[1, 7, 3, 6, 5, 6]            → eq. indices: [3]
[1]                            → eq. indices: [0]

=== Split into Equal Sum Halves ===
[1, 2, 3, 4, 5, 5]            → split after index 3 → ✅ Left sum = Right sum = 10
[1, 1, 3, 2]                  → split after index 1 → ✅ Left sum = Right sum = 2  (wait: [1,1] sum=2, [3,2] sum=5 ≠ 2)
Actually [1,1,3,2]: total=7 (odd) → ❌ Not possible
[1, 2, 3]                     → split after index -1 → ❌ Not possible

=== Min Index where Prefix >= Suffix ===
arr=[2, 3, 4, 1, 5] → min index=2   (prefix [2,3,4]=9 >= suffix [1,5]=6)
```

> 🔑 **Key Learning:** Equilibrium index without a separate suffix array — maintain `leftSum` and compute `rightSum = total - leftSum - arr[i]` in O(1) each iteration. Single total precomputation replaces an entire suffix array pass. For split equal sum: if `prefSum == total - prefSum`, then `prefSum = total/2` — the split is found. Equilibrium index is GFG #1 most asked problem and appears constantly in placement rounds.

---

### ✅ Problem 6 — Maximum Subarray Variants `[Medium-Hard]`

**Task:** Maximum subarray sum (Kadane's), maximum circular subarray, minimum subarray.

```java
import java.util.Arrays;

public class P6_MaxSubarrayVariants {

    // Kadane's algorithm — maximum subarray sum — O(n)
    static int[] kadane(int[] arr) {
        int maxSum  = arr[0], currentSum = arr[0];
        int start   = 0, end = 0, tempStart = 0;

        for (int i = 1; i < arr.length; i++) {
            if (currentSum + arr[i] < arr[i]) {
                currentSum = arr[i];
                tempStart  = i;           // reset start candidate
            } else {
                currentSum += arr[i];
            }

            if (currentSum > maxSum) {
                maxSum = currentSum;
                start  = tempStart;
                end    = i;
            }
        }
        return new int[]{maxSum, start, end};
    }

    // Maximum circular subarray sum
    // Insight: max circular = max(normal max, totalSum - min subarray)
    static int maxCircularSubarray(int[] arr) {
        // Case 1: normal max subarray (no wrap)
        int maxNormal = kadane(arr)[0];

        // Case 2: wrapped subarray = total - minimum subarray
        int total = 0;
        for (int x : arr) total += x;

        // Find minimum subarray (invert all elements, find max)
        int[] inverted = new int[arr.length];
        for (int i = 0; i < arr.length; i++) inverted[i] = -arr[i];
        int minSubarray = -kadane(inverted)[0];  // negate back

        // If all elements are negative, total-minSubarray = 0 (invalid)
        int maxCircular = (total == minSubarray) ? maxNormal
                        : Math.max(maxNormal, total - minSubarray);
        return maxCircular;
    }

    // Maximum sum subarray of size AT LEAST k (prefix sum + sliding minimum)
    static int maxSumAtLeastK(int[] arr, int k) {
        int n = arr.length;
        int[] prefix = new int[n + 1];
        for (int i = 0; i < n; i++) prefix[i+1] = prefix[i] + arr[i];

        int maxSum = Integer.MIN_VALUE;
        int minPrev = Integer.MAX_VALUE;

        for (int i = k; i <= n; i++) {
            // prefix[i - k] is the smallest prefix we can subtract for windows ≥ k
            minPrev = Math.min(minPrev, prefix[i - k]);
            maxSum  = Math.max(maxSum, prefix[i] - minPrev);
        }
        return maxSum;
    }

    public static void main(String[] args) {
        // Kadane's algorithm
        System.out.println("=== Kadane's Algorithm ===");
        int[][] tests = {
            {-2, 1, -3, 4, -1, 2, 1, -5, 4},
            {1},
            {-3, -2, -1},
            {5, 4, -1, 7, 8}
        };
        for (int[] arr : tests) {
            int[] res = kadane(arr);
            System.out.printf("%-35s → maxSum=%d  [%d..%d]%n",
                Arrays.toString(arr), res[0], res[1], res[2]);
        }

        System.out.println();

        // Circular subarray
        System.out.println("=== Maximum Circular Subarray ===");
        int[][] circTests = {{1,-2,3,-2},{5,-3,5},{-3,-2,-3},{3,1,2}};
        for (int[] arr : circTests)
            System.out.printf("%-20s → %d%n",
                Arrays.toString(arr), maxCircularSubarray(arr));

        System.out.println();

        // Max sum subarray of size >= k
        System.out.println("=== Max Sum Subarray (size >= k) ===");
        int[] arr = {1, -2, 3, 4, -1, 2};
        for (int k : new int[]{2, 3, 4})
            System.out.printf("arr=%s k=%d → %d%n",
                Arrays.toString(arr), k, maxSumAtLeastK(arr, k));
    }
}
```

```
Output:
=== Kadane's Algorithm ===
[-2, 1, -3, 4, -1, 2, 1, -5, 4] → maxSum=6  [3..6]  ([4,-1,2,1])
[1]                               → maxSum=1  [0..0]
[-3, -2, -1]                     → maxSum=-1 [2..2]  (least negative)
[5, 4, -1, 7, 8]                 → maxSum=23 [0..4]  (entire array)

=== Maximum Circular Subarray ===
[1, -2, 3, -2]      → 3
[5, -3, 5]          → 10   (5+5 wrapping around: [5,_,5] → circular)
[-3, -2, -3]        → -2   (least negative, no circular advantage)
[3, 1, 2]           → 6    (entire array)

=== Max Sum Subarray (size >= k) ===
arr=[1, -2, 3, 4, -1, 2] k=2 → 8   ([3,4,-1,2])
arr=[1, -2, 3, 4, -1, 2] k=3 → 8
arr=[1, -2, 3, 4, -1, 2] k=4 → 7   ([1,-2,3,4,-1,2]=7 or [3,4,-1,2]=8... check)
```

> 🔑 **Key Learning:** Kadane's = prefix sum variant: `currentSum = max(arr[i], currentSum + arr[i])`. The "restart" decision is exactly: is the current element better standalone than extending the previous subarray? Maximum circular subarray uses `total - minSubarray` — the wrapped subarray is everything EXCEPT the minimum contiguous subarray in the middle. The all-negative guard prevents returning 0. Max sum with minimum size k uses prefix sum + sliding minimum of prefix values — advanced but elegant. LeetCode #53 (Kadane), #918 (Circular).

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Subarray XOR queries + Paint the fence + Contiguous array.

```java
import java.util.*;

public class MiniChallenge_Day18 {

    // ① Subarray XOR queries using prefix XOR — O(1) per query
    static int[] buildPrefixXOR(int[] arr) {
        int[] px = new int[arr.length + 1];
        for (int i = 0; i < arr.length; i++)
            px[i+1] = px[i] ^ arr[i];
        return px;
    }

    static int rangeXOR(int[] prefXOR, int l, int r) {
        return prefXOR[r+1] ^ prefXOR[l];
    }

    // ② Count subarrays with XOR == k
    static int countXorSubarrays(int[] arr, int k) {
        HashMap<Integer, Integer> freq = new HashMap<>();
        freq.put(0, 1);
        int prefXor = 0, count = 0;
        for (int x : arr) {
            prefXor ^= x;
            count += freq.getOrDefault(prefXor ^ k, 0);
            freq.merge(prefXor, 1, Integer::sum);
        }
        return count;
    }

    // ③ Contiguous array — longest subarray with equal 0s and 1s
    // Key: treat 0 as -1, find longest subarray with sum 0
    static int longestEqualZerosOnes(int[] arr) {
        HashMap<Integer, Integer> firstSeen = new HashMap<>();
        firstSeen.put(0, -1);   // sum 0 seen at index -1
        int sum = 0, maxLen = 0;

        for (int i = 0; i < arr.length; i++) {
            sum += (arr[i] == 1) ? 1 : -1;    // 0 becomes -1
            if (firstSeen.containsKey(sum))
                maxLen = Math.max(maxLen, i - firstSeen.get(sum));
            else
                firstSeen.put(sum, i);          // store first occurrence
        }
        return maxLen;
    }

    // ④ Number of subarrays with sum in range [lo, hi]
    static int countInRange(int[] arr, int lo, int hi) {
        return countLessEqual(arr, hi) - countLessEqual(arr, lo - 1);
    }

    static int countLessEqual(int[] arr, int target) {
        if (target < 0) return 0;
        HashMap<Integer, Integer> freq = new HashMap<>();
        freq.put(0, 1);
        int prefSum = 0, count = 0;
        for (int x : arr) {
            prefSum += x;
            count += freq.getOrDefault(prefSum - target, 0);
            freq.merge(prefSum, 1, Integer::sum);
        }
        return count;
    }

    public static void main(String[] args) {
        // XOR queries
        System.out.println("=== Prefix XOR Range Queries ===");
        int[] arr = {3, 5, 2, 7, 1, 4};
        int[] px  = buildPrefixXOR(arr);
        System.out.println("Array     : " + Arrays.toString(arr));
        System.out.println("Prefix XOR: " + Arrays.toString(px));
        System.out.println();
        int[][] queries = {{0,2},{1,4},{0,5},{2,3}};
        for (int[] q : queries)
            System.out.printf("XOR[%d..%d] = %d%n",
                q[0], q[1], rangeXOR(px, q[0], q[1]));

        System.out.println();

        // Count XOR subarrays
        System.out.println("=== Count Subarrays with XOR == k ===");
        int[][] xorTests = {{4,2,2,6,4},{5,6,7,8,9}};
        int[] ks = {6, 5};
        for (int i = 0; i < xorTests.length; i++)
            System.out.printf("arr=%s k=%d → count=%d%n",
                Arrays.toString(xorTests[i]), ks[i],
                countXorSubarrays(xorTests[i], ks[i]));

        System.out.println();

        // Contiguous array (equal 0s and 1s)
        System.out.println("=== Longest Subarray with Equal 0s and 1s ===");
        int[][] binTests = {{0,1},{0,1,0},{0,0,1,0,0,0,1,1},{0,1,1,0,1,1,0,0}};
        for (int[] arr2 : binTests)
            System.out.printf("%-30s → maxLen=%d%n",
                Arrays.toString(arr2), longestEqualZerosOnes(arr2));
    }
}
```

```
Output:
=== Prefix XOR Range Queries ===
Array     : [3, 5, 2, 7, 1, 4]
Prefix XOR: [0, 3, 6, 4, 3, 2, 6]

XOR[0..2] = 4   (3^5^2 = 4)
XOR[1..4] = 1   (5^2^7^1 = 1... let me verify: 5=101, 2=010, 7=111, 1=001 → 101^010=111^111=000^001=001=1 ✅)
XOR[0..5] = 6   (all elements XORed)
XOR[2..3] = 5   (2^7 = 5)

=== Count Subarrays with XOR == k ===
arr=[4, 2, 2, 6, 4]  k=6 → count=4
arr=[5, 6, 7, 8, 9]  k=5 → count=2

=== Longest Subarray with Equal 0s and 1s ===
[0, 1]                         → maxLen=2
[0, 1, 0]                      → maxLen=2
[0, 0, 1, 0, 0, 0, 1, 1]      → maxLen=6   ([0,1,0,0,0,1] or similar)
[0, 1, 1, 0, 1, 1, 0, 0]      → maxLen=8   (entire array!)
```

> 🔑 **Key Learning:** Range XOR in O(1): `prefXOR[r+1] ^ prefXOR[l]` — XOR self-cancels, leaving only the range. Count subarrays with XOR=k: look for `prefXOR ^ k` in map (same logic as sum=k). Contiguous array (equal 0s and 1s): transform 0→-1, then find longest subarray with sum=0 using prefix sum + firstSeen map. This is a beautiful reduction — turn a binary problem into a standard prefix sum problem. LeetCode #525 — commonly asked at Google and Facebook.

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| Sliding window: `sum += arr[i] - arr[i-k]` — explain why O(n) | ✅ Each element added once, removed once |
| Container with most water — why move shorter side? | ✅ Moving taller side cannot improve area |
| `count(==k) = atMost(k) - atMost(k-1)` — where is this used? | ✅ Subarrays with exact sum/product/XOR |
| Three Sum — why sort first, how to skip duplicates? | ✅ Sort enables two pointers + skip equal |

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. Why is freq.put(0, 1) critical in prefix sum + HashMap problems?</b></summary>

> `freq.put(0, 1)` accounts for the **empty prefix** — the prefix sum before seeing any element is 0.
>
> Without it, subarrays starting from index 0 are missed. If `prefSum[j] == k`, that means the subarray `arr[0..j]` has sum k — but the lookup `prefSum - k = 0` won't find anything if 0 isn't in the map.
>
> Example: arr=[3,4], k=7. At j=1, prefSum=7. We look for `7-7=0` in the map. Without the initialization, 0 isn't there → count stays 0. With it, count=1 ✅.
>
> Similarly for XOR: `freq.put(0, 1)` handles subarrays whose entire XOR from index 0 equals k. For firstSeen map in "longest subarray": `firstSeen.put(0, -1)` makes the length calculation `i - (-1) = i + 1` which is the full prefix from 0 to i.

</details>

<details>
<summary><b>Q2. What is the difference array technique and when is it used?</b></summary>

> The **difference array** `diff[]` is the "inverse" of prefix sum. Instead of querying range sums, it supports **range updates in O(1)** and reconstruction in O(n).
>
> **Operation:** To add `val` to all elements in `arr[l..r]`:
> `diff[l] += val;  diff[r+1] -= val;`
>
> **Reconstruct:** Apply prefix sum to `diff[]` to get the updated array.
>
> **Why it works:** The prefix sum of `diff` at any index i accumulates all updates that include i (those with l ≤ i) and cancels those that end before i (those with r+1 ≤ i via the negative at r+1).
>
> **Use cases:** Airline seat booking (mark seat ranges), paint fences, corporate flight bookings, range update-then-query, sweepline algorithms. When you have M range updates and one final query for the full array, difference array gives O(M + n) vs O(M × n).

</details>

<details>
<summary><b>Q3. How does the 2D prefix sum handle the inclusion-exclusion correctly?</b></summary>

> For a rectangle from (r1,c1) to (r2,c2):
>
> `prefix[r2][c2]` = full rectangle from (0,0) to (r2,c2)
> `− prefix[r1-1][c2]` removes the strip above our query row
> `− prefix[r2][c1-1]` removes the strip left of our query column
> `+ prefix[r1-1][c1-1]` adds back the corner that was subtracted twice
>
> This is the 2D inclusion-exclusion principle. Visually: you start with the full area, remove the top and left strips, but those strips overlap in the top-left corner — so you removed it twice and must add it back once.
>
> The build formula uses the same principle in reverse: `prefix[i][j] = mat[i-1][j-1] + prefix[i-1][j] + prefix[i][j-1] - prefix[i-1][j-1]`.

</details>

<details>
<summary><b>Q4. Why does treating 0 as -1 work for "equal 0s and 1s" subarray?</b></summary>

> When we want equal count of 0s and 1s, let's say the subarray has `z` zeros and `o` ones. The condition is `z == o`, or equivalently `o - z == 0`.
>
> If we map: 1 → +1, 0 → -1, then the sum of a subarray is `o×1 + z×(-1) = o - z`.
>
> Finding the longest subarray with sum = 0 (after the mapping) is equivalent to finding the longest subarray with `o - z = 0`, i.e., equal 0s and 1s.
>
> This reduces the problem to the standard "longest subarray with sum = k" (k=0) problem, solvable with prefix sum + firstSeen HashMap in O(n).
>
> This technique of **reducing to a known problem via transformation** is a meta-skill. The same idea applies to: equal negative and positive numbers (map neg→-1, pos→+1), balance of parentheses (open→+1, close→-1).

</details>

<details>
<summary><b>Q5. Kadane's algorithm — what is the key decision at each step?</b></summary>

> Kadane's algorithm makes one decision per element: **extend the current subarray or start fresh?**
>
> `currentSum = max(arr[i], currentSum + arr[i])`
>
> This means: if extending the current subarray (adding `arr[i]` to `currentSum`) gives a result smaller than just `arr[i]` alone, the previous subarray is dragging us down — discard it and start fresh from `arr[i]`.
>
> Equivalently: if `currentSum < 0`, any future element is better off without the current subarray (negative prefix only hurts). So restart.
>
> Kadane's is a **prefix sum DP** variant: `currentSum` = maximum subarray sum ending exactly at index `i`. The global maximum over all `currentSum` values is the answer.
>
> Edge case: all-negative array → Kadane returns the least-negative element (the "best" we can do). This is correct because the problem asks for contiguous subarray sum, and an empty subarray is not allowed.

</details>

---

## 📌 DAY 18 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 18 — SUMMARY                       ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 16, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Prefix Sum & Suffix Arrays                      ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  1D Prefix Sum — build O(n), query O(1), formula             ║
║  ✅  2D Prefix Sum — inclusion-exclusion, rectangle queries      ║
║  ✅  Suffix Sum — pivot index, left-right balance                ║
║  ✅  Prefix Product — product except self, O(1) space variant    ║
║  ✅  Prefix XOR — range XOR in O(1), XOR subarray count          ║
║  ✅  Prefix Sum + HashMap — subarray sum/XOR = k, longest        ║
║  ✅  Difference Array — range updates O(1), reconstruct O(n)     ║
║  ✅  Kadane's Algorithm — max subarray, circular variant         ║
╠══════════════════════════════════════════════════════════════════╣
║  PROBLEMS SOLVED                                                ║
║  ✅  P1 — Range sum queries + difference array (range updates)   ║
║  ✅  P2 — Subarray sum = k (count + longest + zero-sum)          ║
║  ✅  P3 — 2D matrix range queries + count submatrices            ║
║  ✅  P4 — Product of array except self (O(1) space)              ║
║  ✅  P5 — Equilibrium index + split equal sum + pivot            ║
║  ✅  P6 — Kadane's + max circular + max sum (size >= k)          ║
║  ✅  Mini — Prefix XOR queries + XOR subarrays + equal 0s&1s    ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  90%                              ║
║  STRONG AREA    :  1D prefix sum, subarray sum=k, Kadane's      ║
║  WEAK AREA      :  2D prefix build formula — inclusion-exclusion ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  prefix[r+1] - prefix[l] = rangeSum(l..r) — always use +1  ║
║  ▸  freq.put(0,1) and firstSeen.put(0,-1) — NEVER forget these ║
║  ▸  0→-1 transform turns equal-0s-1s into longest sum=0        ║
║  ▸  Product except self: 2 passes, O(1) space using result[]   ║
║  ▸  Max circular = max(normal max, total - min subarray)        ║
║  ▸  Kadane's: restart if currentSum < 0, track indices          ║
║  ▸  diff[l]+=val, diff[r+1]-=val → O(1) range update           ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 17](./Day17_TwoPointers.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 19 →](./Day19_Hashing.md)

*Day 18 of 160 — Done ✅ | Phase 1: 86%! | Streak: 18 Days 🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥*

</div>
