# 📄 Day 06 — Arrays in Java

<div align="center">

![Day](https://img.shields.io/badge/Day-06_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Arrays_%26_Array_Operations-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 05](./Day05_Functions.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 07 →](./Day07_Arrays_2D.md)

</div>

---

## 🎯 Day 6 Goal
> Master 1D arrays in Java — declaration, initialization, traversal, searching, and manipulation. Arrays are the **most fundamental data structure in all of DSA**. Every topic from Day 46 onwards is built on this foundation.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | Array declaration, initialization, memory model | 60 min |
| Block 2 | Traversal, input, common operations | 45 min |
| Block 3 | Revision — Day 5 methods flash review | 15 min |
| Block 4 | Practice Set — 6 Problems | 120 min |
| Block 5 | Mini Challenge + Revision Tasks | 40 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. What is an Array?

A **fixed-size, contiguous block of memory** storing multiple values of the **same type** under one name, accessed by index.

```java
// Without array — 5 separate variables (unscalable)
int s1 = 85, s2 = 92, s3 = 78, s4 = 96, s5 = 88;

// With array — one name, indexed access (scalable)
int[] scores = {85, 92, 78, 96, 88};
//    index:     0   1   2   3   4
```

**Memory model:**
```
int[] arr = {10, 20, 30, 40, 50};

Stack:            Heap:
arr ──────────►  [10][20][30][40][50]
(reference)       [0] [1] [2] [3] [4]
                  ↑
                  base address (contiguous memory)
```

> 💡 Arrays live on the **Heap**. The variable `arr` on the Stack holds only a **reference** (address). This is why arrays behave differently from primitives when passed to methods.

---

### 🔷 2. Declaration & Initialization

```java
// Method 1 — with values (size inferred automatically)
int[] arr = {10, 20, 30, 40, 50};

// Method 2 — fixed size, fill later (default values applied)
int[] arr = new int[5];   // [0, 0, 0, 0, 0]
arr[0] = 10;
arr[1] = 20;

// Method 3 — declare first, initialize later
int[] arr;
arr = new int[]{1, 2, 3};
```

**Default values by type:**

| Type | Default |
|------|---------|
| `int`, `byte`, `short`, `long` | `0` |
| `float`, `double` | `0.0` |
| `boolean` | `false` |
| `char` | `'\u0000'` |
| `String`, Objects | `null` |

> ⚠️ **Indexing starts at 0.** Array of size `n` → valid indices `0` to `n-1`. Accessing index `n` → `ArrayIndexOutOfBoundsException`.

---

### 🔷 3. Accessing & Modifying Elements

```java
int[] arr = {10, 20, 30, 40, 50};

System.out.println(arr[0]);              // 10  — first element
System.out.println(arr[4]);              // 50  — last element
System.out.println(arr.length);          // 5   — size (.length, NO parentheses!)
System.out.println(arr[arr.length - 1]); // 50  — last element formula

arr[2] = 99;                             // modify element
System.out.println(arr[2]);              // 99
```

> ⚠️ `.length` for arrays (no parentheses). `.length()` for Strings (with parentheses). Classic mix-up.

---

### 🔷 4. Traversal — Three Ways

```java
int[] arr = {10, 20, 30, 40, 50};

// A. Classic for — when INDEX is needed
for (int i = 0; i < arr.length; i++) {
    System.out.print(arr[i] + " ");
}
// Output: 10 20 30 40 50

// B. Enhanced for-each — when index NOT needed (cleaner)
for (int num : arr) {
    System.out.print(num + " ");
}
// Output: 10 20 30 40 50

// C. Reverse traversal
for (int i = arr.length - 1; i >= 0; i--) {
    System.out.print(arr[i] + " ");
}
// Output: 50 40 30 20 10
```

**When to use which:**
| Loop Type | Use When |
|-----------|----------|
| Classic `for` | Need index, modifying elements, reverse |
| Enhanced `for-each` | Read-only traversal, clean code |
| `while` | Dynamic stop conditions (searching) |

---

### 🔷 5. Arrays are Passed by Reference

```java
static void doubleAll(int[] arr) {
    for (int i = 0; i < arr.length; i++) {
        arr[i] *= 2;    // modifies THE ORIGINAL array!
    }
}

public static void main(String[] args) {
    int[] nums = {1, 2, 3, 4, 5};
    doubleAll(nums);
    // nums is now {2, 4, 6, 8, 10} — CHANGED!
}
```

```
Memory explanation:
main()          doubleAll()
nums = 0x1A4 ──► arr = 0x1A4  ← SAME address!
                 arr[0] *= 2  ← modifies heap directly
nums[0] = 2  ✅  (original changed)
```

> 💡 Unlike primitives — arrays passed to methods share the **same heap memory**. Method changes affect the original. This enables efficient **in-place algorithms** in DSA.

---

### 🔷 6. Essential Array Operations

```java
// ① Find maximum — initialize with arr[0], NOT 0!
static int findMax(int[] arr) {
    int max = arr[0];
    for (int i = 1; i < arr.length; i++)
        if (arr[i] > max) max = arr[i];
    return max;
}

// ② Sum of all elements
static int sum(int[] arr) {
    int total = 0;
    for (int num : arr) total += num;
    return total;
}

// ③ Linear search — O(n)
static int linearSearch(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++)
        if (arr[i] == target) return i;
    return -1;    // not found
}

// ④ Reverse in-place — two pointer technique
static void reverse(int[] arr) {
    int left = 0, right = arr.length - 1;
    while (left < right) {
        int temp  = arr[left];
        arr[left] = arr[right];
        arr[right] = temp;
        left++;  right--;
    }
}

// ⑤ Print array — write once, use everywhere
static void print(int[] arr) {
    System.out.print("[");
    for (int i = 0; i < arr.length; i++) {
        System.out.print(arr[i]);
        if (i < arr.length - 1) System.out.print(", ");
    }
    System.out.println("]");
}
```

---

### 🔷 7. Common Array Pitfalls

```java
int[] arr = {1, 2, 3, 4, 5};

// ❌ Off-by-one — i <= arr.length crashes at boundary
for (int i = 0; i <= arr.length; i++) { }  // CRASH at i=5
// ✅ Use i < arr.length

// ❌ Wrong max initialization
int max = 0;    // fails if all elements are negative!
// ✅ Use max = arr[0]

// ❌ Array length with parentheses
arr.length()    // COMPILE ERROR
// ✅ arr.length  (no parentheses)

// ❌ Forgetting arrays are 0-indexed
arr[5]          // CRASH on size-5 array (valid: 0-4)
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Array Statistics `[Easy]`

**Task:** Find sum, average, max, min, and even count for `{15, 8, 23, 42, 4, 16, 8, 3}`.

```java
public class P1_ArrayStatistics {

    static void print(int[] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) System.out.print(", ");
        }
        System.out.println("]");
    }

    static int sum(int[] arr) {
        int total = 0;
        for (int num : arr) total += num;
        return total;
    }

    static double average(int[] arr) {
        return (double) sum(arr) / arr.length;
    }

    static int max(int[] arr) {
        int max = arr[0];
        for (int i = 1; i < arr.length; i++)
            if (arr[i] > max) max = arr[i];
        return max;
    }

    static int min(int[] arr) {
        int min = arr[0];
        for (int i = 1; i < arr.length; i++)
            if (arr[i] < min) min = arr[i];
        return min;
    }

    static int countEvens(int[] arr) {
        int count = 0;
        for (int num : arr) if (num % 2 == 0) count++;
        return count;
    }

    public static void main(String[] args) {
        int[] arr = {15, 8, 23, 42, 4, 16, 8, 3};
        print(arr);
        System.out.println("Sum     : " + sum(arr));
        System.out.printf ("Average : %.2f%n", average(arr));
        System.out.println("Maximum : " + max(arr));
        System.out.println("Minimum : " + min(arr));
        System.out.println("Evens   : " + countEvens(arr));
    }
}
```

```
Output:
[15, 8, 23, 42, 4, 16, 8, 3]
Sum     : 119
Average : 14.88
Maximum : 42
Minimum : 3
Evens   : 4
```

> 🔑 **Key Learning:** Initialize `max` and `min` with `arr[0]`, not `0` or `Integer.MAX_VALUE` — cleaner and safer. `average` calls `sum()` — method reuse. Cast to `double` before dividing to avoid integer division. `countEvens` uses for-each since index is irrelevant.

---

### ✅ Problem 2 — Linear Search `[Easy]`

**Task:** Find target's index in array, return `-1` if not found.

```java
public class P2_LinearSearch {

    static void print(int[] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) System.out.print(", ");
        }
        System.out.println("]");
    }

    static int linearSearch(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == target) return i;   // found — return index immediately
        }
        return -1;    // not found
    }

    static void searchAndReport(int[] arr, int target) {
        int index = linearSearch(arr, target);
        if (index != -1)
            System.out.println("Found " + target + " at index " + index);
        else
            System.out.println(target + " not found in array");
    }

    public static void main(String[] args) {
        int[] arr = {15, 8, 23, 42, 4, 16};
        print(arr);
        searchAndReport(arr, 42);   // found
        searchAndReport(arr, 99);   // not found
        searchAndReport(arr, 15);   // first element
        searchAndReport(arr, 16);   // last element
    }
}
```

```
Output:
[15, 8, 23, 42, 4, 16]
Found 42 at index 3
99 not found in array
Found 15 at index 0
Found 16 at index 5
```

**Trace for target=42:**
```
i=0: arr[0]=15, 15≠42 → continue
i=1: arr[1]=8,  8≠42  → continue
i=2: arr[2]=23, 23≠42 → continue
i=3: arr[3]=42, 42==42 → return 3 ✅
```

> 🔑 **Key Learning:** Linear search is O(n) — worst case checks every element. The `return i` inside the loop is an early exit — no need to check remaining elements once found. `-1` as sentinel value for "not found" is a universal convention in DSA. This same pattern appears in finding elements in linked lists, trees, and graphs.

---

### ✅ Problem 3 — Reverse an Array `[Easy-Medium]`

**Task:** Reverse array **in-place** using two-pointer technique.

```java
public class P3_ReverseArray {

    static void print(int[] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) System.out.print(", ");
        }
        System.out.println("]");
    }

    static void reverse(int[] arr) {
        int left  = 0;
        int right = arr.length - 1;

        while (left < right) {
            // Swap arr[left] and arr[right]
            int temp     = arr[left];
            arr[left]    = arr[right];
            arr[right]   = temp;
            left++;
            right--;
        }
    }

    public static void main(String[] args) {
        int[] arr1 = {1, 2, 3, 4, 5};
        System.out.print("Before: "); print(arr1);
        reverse(arr1);
        System.out.print("After : "); print(arr1);

        System.out.println();

        int[] arr2 = {10, 20, 30};
        System.out.print("Before: "); print(arr2);
        reverse(arr2);
        System.out.print("After : "); print(arr2);

        System.out.println();

        // Edge: single element
        int[] arr3 = {42};
        System.out.print("Before: "); print(arr3);
        reverse(arr3);
        System.out.print("After : "); print(arr3);
    }
}
```

```
Output:
Before: [1, 2, 3, 4, 5]
After : [5, 4, 3, 2, 1]

Before: [10, 20, 30]
After : [30, 20, 10]

Before: [42]
After : [42]
```

**Two-pointer trace for {1,2,3,4,5}:**
```
left=0, right=4: swap arr[0]↔arr[4] → [5,2,3,4,1], left=1, right=3
left=1, right=3: swap arr[1]↔arr[3] → [5,4,3,2,1], left=2, right=2
left=2, right=2: left < right? NO → STOP ✅
```

> 🔑 **Key Learning:** The **two-pointer technique** — start pointers at opposite ends, work inward. This is one of the most important DSA patterns. It appears in: two-sum problem, palindrome check, container with most water, merge sorted arrays. O(n/2) swaps = O(n) time, O(1) space. No extra array needed.

---

### ✅ Problem 4 — Second Largest Element `[Medium]`

**Task:** Find second largest in ONE pass. No sorting allowed.

```java
public class P4_SecondLargest {

    static void print(int[] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) System.out.print(", ");
        }
        System.out.println("]");
    }

    static int secondLargest(int[] arr) {
        if (arr.length < 2) return -1;

        int max        = Integer.MIN_VALUE;
        int secondMax  = Integer.MIN_VALUE;

        for (int num : arr) {
            if (num > max) {
                secondMax = max;    // old max becomes second
                max       = num;    // new max
            } else if (num > secondMax && num != max) {
                secondMax = num;    // update second only if different from max
            }
        }

        return (secondMax == Integer.MIN_VALUE) ? -1 : secondMax;
    }

    public static void main(String[] args) {
        int[] arr1 = {12, 35, 1, 10, 34, 1};
        int[] arr2 = {10, 10, 10};
        int[] arr3 = {5, 3};
        int[] arr4 = {-1, -5, -3, -2};

        System.out.print("arr1: "); print(arr1);
        System.out.println("Second Largest: " + secondLargest(arr1));  // 34

        System.out.print("arr2: "); print(arr2);
        System.out.println("Second Largest: " + secondLargest(arr2));  // -1

        System.out.print("arr3: "); print(arr3);
        System.out.println("Second Largest: " + secondLargest(arr3));  // 3

        System.out.print("arr4: "); print(arr4);
        System.out.println("Second Largest: " + secondLargest(arr4));  // -2
    }
}
```

```
Output:
arr1: [12, 35, 1, 10, 34, 1]
Second Largest: 34

arr2: [10, 10, 10]
Second Largest: -1

arr3: [5, 3]
Second Largest: 3

arr4: [-1, -5, -3, -2]
Second Largest: -2
```

**One-pass trace for {12, 35, 1, 10, 34, 1}:**
```
Start: max=MIN, secondMax=MIN
num=12: 12>MIN → secondMax=MIN, max=12
num=35: 35>12  → secondMax=12,  max=35
num=1:  1<35, 1<12 → no change
num=10: 10<35, 10<12 → no change (wait: 10>secondMax=12? No)
num=34: 34<35, 34>12 and 34≠35 → secondMax=34
num=1:  no change
Result: secondMax = 34 ✅
```

> 🔑 **Key Learning:** One pass, two variables — O(n) time, O(1) space. The `num != max` condition handles duplicates correctly — if max=35 appears again, it shouldn't update secondMax. Using `Integer.MIN_VALUE` as initial value handles negative arrays correctly (initializing with 0 would fail for all-negative arrays). This exact pattern — tracking top-2 values in one pass — appears in many interview problems.

---

### ✅ Problem 5 — Rotate Array Left by K `[Medium]`

**Task:** Rotate array left by `k` positions without extra array for rotation logic.

```java
public class P5_RotateArray {

    static void print(int[] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) System.out.print(", ");
        }
        System.out.println("]");
    }

    static void rotateLeft(int[] arr, int k) {
        int n = arr.length;
        k = k % n;      // handle k >= n (full rotations cancel out)
        if (k == 0) return;   // no rotation needed

        // Store first k elements in temp array
        int[] temp = new int[k];
        for (int i = 0; i < k; i++) temp[i] = arr[i];

        // Shift remaining elements left by k positions
        for (int i = k; i < n; i++) arr[i - k] = arr[i];

        // Place temp elements at the end
        for (int i = 0; i < k; i++) arr[n - k + i] = temp[i];
    }

    public static void main(String[] args) {
        int[] arr1 = {1, 2, 3, 4, 5};
        System.out.print("Original: "); print(arr1);
        rotateLeft(arr1, 2);
        System.out.print("Left k=2: "); print(arr1);

        System.out.println();

        int[] arr2 = {1, 2, 3, 4, 5};
        rotateLeft(arr2, 5);   // full rotation
        System.out.print("Left k=5: "); print(arr2);

        int[] arr3 = {1, 2, 3, 4, 5};
        rotateLeft(arr3, 7);   // 7 % 5 = 2, same as k=2
        System.out.print("Left k=7: "); print(arr3);
    }
}
```

```
Output:
Original: [1, 2, 3, 4, 5]
Left k=2: [3, 4, 5, 1, 2]

Left k=5: [1, 2, 3, 4, 5]
Left k=7: [3, 4, 5, 1, 2]
```

**Step-by-step trace for {1,2,3,4,5}, k=2:**
```
Step 1 - Store first 2: temp = [1, 2]
Step 2 - Shift left by 2:
  arr[0]=arr[2]=3, arr[1]=arr[3]=4, arr[2]=arr[4]=5
  arr = [3, 4, 5, 4, 5]  ← last 2 positions stale
Step 3 - Place temp at end:
  arr[3]=temp[0]=1, arr[4]=temp[1]=2
  arr = [3, 4, 5, 1, 2] ✅
```

> 🔑 **Key Learning:** `k = k % n` is critical — rotating by n is same as no rotation. This is a real interview edge case that trips up many candidates. The 3-step algorithm (store → shift → place) is the standard approach. Time: O(n), Space: O(k). Note: there's an even more elegant O(1) space solution using the **reversal algorithm** — we'll revisit this in Phase 3 (Two Pointers).

---

### ✅ Problem 6 — Find Duplicates `[Medium-Hard]`

**Task:** Print all elements appearing more than once. Print each duplicate only once.

```java
public class P6_FindDuplicates {

    static void print(int[] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) System.out.print(", ");
        }
        System.out.println("]");
    }

    static void findDuplicates(int[] arr) {
        boolean foundAny = false;
        System.out.print("Duplicates: ");

        for (int i = 0; i < arr.length; i++) {
            boolean isDuplicate = false;

            // Check if arr[i] appeared earlier (already reported)
            for (int k = 0; k < i; k++) {
                if (arr[k] == arr[i]) { isDuplicate = true; break; }
            }
            if (isDuplicate) continue;  // already reported this value

            // Check if arr[i] appears again later
            for (int j = i + 1; j < arr.length; j++) {
                if (arr[j] == arr[i]) {
                    System.out.print(arr[i] + " ");
                    foundAny = true;
                    break;
                }
            }
        }

        if (!foundAny) System.out.print("None");
        System.out.println();
    }

    public static void main(String[] args) {
        int[] arr1 = {1, 3, 4, 2, 2, 3, 5, 1};
        int[] arr2 = {1, 2, 3, 4, 5};
        int[] arr3 = {5, 5, 5, 5};

        System.out.print("arr1: "); print(arr1);
        findDuplicates(arr1);

        System.out.print("arr2: "); print(arr2);
        findDuplicates(arr2);

        System.out.print("arr3: "); print(arr3);
        findDuplicates(arr3);
    }
}
```

```
Output:
arr1: [1, 3, 4, 2, 2, 3, 5, 1]
Duplicates: 1 3 2

arr2: [1, 2, 3, 4, 5]
Duplicates: None

arr3: [5, 5, 5, 5]
Duplicates: 5
```

**Algorithm logic:**
```
For each element arr[i]:
  1. Check if arr[i] appeared in arr[0..i-1] → if yes, skip (already reported)
  2. Check if arr[i] appears in arr[i+1..n-1] → if yes, print it (first time seeing a dup)
```

> 🔑 **Key Learning:** O(n²) nested loop solution — acceptable for now. The **mentor question**: if array were sorted, adjacent duplicates would be easy to spot in O(n) — just compare `arr[i]` with `arr[i-1]`. In Phase 2, we use **HashMap** to do this in O(n) without sorting. This progression (brute force → sorting trick → hash trick) is exactly how DSA thinking evolves.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Complete Array Report Generator.

```java
public class MiniChallenge_Day6 {

    static void print(int[] arr) {
        System.out.print("[");
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i]);
            if (i < arr.length - 1) System.out.print(", ");
        }
        System.out.println("]");
    }

    static int sum(int[] arr) {
        int total = 0;
        for (int num : arr) total += num;
        return total;
    }

    static int maxIndex(int[] arr) {
        int idx = 0;
        for (int i = 1; i < arr.length; i++)
            if (arr[i] > arr[idx]) idx = i;
        return idx;
    }

    static int minIndex(int[] arr) {
        int idx = 0;
        for (int i = 1; i < arr.length; i++)
            if (arr[i] < arr[idx]) idx = i;
        return idx;
    }

    static int[] reversed(int[] arr) {
        int[] copy = arr.clone();   // clone — does NOT modify original
        int left = 0, right = copy.length - 1;
        while (left < right) {
            int temp = copy[left]; copy[left] = copy[right];
            copy[right] = temp; left++; right--;
        }
        return copy;
    }

    static void arrayReport(int[] arr) {
        String line = "═".repeat(45);
        System.out.println(line);
        System.out.println("           ARRAY REPORT");
        System.out.println(line);

        System.out.print("Array   : "); print(arr);
        System.out.println("Length  : " + arr.length);
        System.out.println("Sum     : " + sum(arr));
        System.out.printf ("Average : %.2f%n", (double) sum(arr) / arr.length);

        int maxIdx = maxIndex(arr);
        int minIdx = minIndex(arr);
        System.out.println("Maximum : " + arr[maxIdx] + " (at index " + maxIdx + ")");
        System.out.println("Minimum : " + arr[minIdx] + " (at index " + minIdx + ")");

        // Evens and Odds
        System.out.print("Evens   : ");
        int evenCount = 0, oddCount = 0;
        for (int num : arr) if (num % 2 == 0) { System.out.print(num + " "); evenCount++; }
        System.out.println(" (count: " + evenCount + ")");

        System.out.print("Odds    : ");
        for (int num : arr) if (num % 2 != 0) { System.out.print(num + " "); oddCount++; }
        System.out.println(" (count: " + oddCount + ")");

        System.out.print("Reversed: "); print(reversed(arr));
        System.out.print("Original: "); print(arr);   // prove original unchanged
        System.out.println(line);
    }

    public static void main(String[] args) {
        int[] arr = {64, 34, 25, 12, 22, 11, 90, 45, 33, 22};
        arrayReport(arr);
    }
}
```

```
Output:
═════════════════════════════════════════════
           ARRAY REPORT
═════════════════════════════════════════════
Array   : [64, 34, 25, 12, 22, 11, 90, 45, 33, 22]
Length  : 10
Sum     : 358
Average : 35.80
Maximum : 90 (at index 6)
Minimum : 11 (at index 5)
Evens   : 64 34 12 22 90 22  (count: 6)
Odds    : 25 11 45 33  (count: 4)
Reversed: [22, 33, 45, 90, 11, 22, 12, 25, 34, 64]
Original: [64, 34, 25, 12, 22, 11, 90, 45, 33, 22]
═════════════════════════════════════════════
```

> 🔑 **Key Learning:** `arr.clone()` creates a shallow copy — safe for primitive arrays. Reversing the copy leaves original untouched. `maxIndex()` and `minIndex()` return the INDEX (not value) — the value can then be retrieved as `arr[idx]`. The `arrayReport()` method is a **coordinator** — it calls helpers and formats output, containing zero algorithm logic itself. This is clean code architecture.

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| `isPrime(int n)` from memory with `√n` | ✅ `for(int i=2; i*i<=n; i++)` |
| `isPerfect(int n)` — starting value of sum | ✅ `sum = 1` (1 is always proper divisor) |
| Pass-by-value vs array pass-by-reference | ✅ See notes below |
| Output of `square(x)` on primitive x=5 | ✅ Prints 5 — original unchanged |

**Revision Notes:**

**Pass-by-value (primitives):**
> When a primitive is passed to a method, Java makes a COMPLETE COPY. The method works on the copy. Original variable is never touched. Changes in the method die with the method.

**Arrays pass-by-reference:**
> When an array is passed, Java copies the REFERENCE (address), not the data. Both the caller and the method point to the SAME heap memory. Changes made inside the method (via `arr[i] = ...`) permanently modify the original array. No copy of the actual data is made.

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. Arrays live on the Heap — what does that mean vs primitives on Stack?</b></summary>

> **Stack** — fast, auto-managed, stores primitive values and references. When a method ends, its stack frame is destroyed. Small, fixed size.
> **Heap** — larger, manually managed (Java GC handles it), stores objects including arrays. Data on heap persists as long as a reference points to it.
> Primitive `int x = 5` → the value `5` lives ON the stack in the current method frame.
> Array `int[] arr = {1,2,3}` → the variable `arr` is a reference on the stack, but the actual `{1,2,3}` data lives on the heap. This is why arrays survive being passed to methods — the heap data persists, only the reference changes hands.

</details>

<details>
<summary><b>Q2. Why does ArrayIndexOutOfBoundsException happen?</b></summary>

> Java strictly enforces array bounds at runtime. For an array of size `n`, valid indices are `0` through `n-1`. If you access index `n`, `-1`, or any index outside `[0, n-1]`, Java throws `ArrayIndexOutOfBoundsException`.
> **Exact condition:** `index < 0 || index >= arr.length`
> Most common cause: using `i <= arr.length` instead of `i < arr.length` in a for loop — the extra iteration accesses `arr[arr.length]` which doesn't exist.

</details>

<details>
<summary><b>Q3. arr.length vs arr.length() — which is correct?</b></summary>

> **`arr.length`** (no parentheses) — correct for arrays. `length` is a **field** (a stored property) of the array object, not a method.
> **`str.length()`** (with parentheses) — correct for Strings. `length()` is a **method** of the String class that computes and returns the length.
> Classic mistake: using `arr.length()` causes a compile error because arrays don't have a `length()` method. Using `str.length` on a String also fails. Know the difference cold.

</details>

<details>
<summary><b>Q4. Why are arrays passed by reference while primitives are by value?</b></summary>

> **Design reason:** Copying an entire array (potentially millions of elements) for every method call would be catastrophically slow. Java instead copies only the 4-byte reference (address).
> **Practical consequence:** Any modification to array elements inside a method (`arr[i] = x`) permanently changes the original. This enables in-place algorithms — sort, reverse, rotate — all modify the array directly without needing to return a new copy. If you need to preserve the original, manually clone it first: `int[] copy = arr.clone()`.

</details>

<details>
<summary><b>Q5. Why is O(n) second-largest better than sorting O(n log n)?</b></summary>

> Sorting the entire array just to find the second largest is wasteful — you're doing far more work than the problem requires. Sorting rearranges ALL n elements; you only need TWO pieces of information (max and second max).
> For n = 1,000,000: sorting needs ~20,000,000 operations (n log n). One-pass needs 1,000,000 operations (n). That's 20x faster.
> **The DSA mindset:** Always ask "what is the minimum information I need?" and find the algorithm that extracts exactly that. Don't reach for sorting unless the problem genuinely requires a sorted array. This thinking is what separates good DSA solutions from brute-force ones.

</details>

---

## 📌 DAY 6 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 6 — SUMMARY                        ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 04, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Arrays in Java                                  ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  What is an array — memory model (Stack ref → Heap data)     ║
║  ✅  3 declaration methods + default values by type              ║
║  ✅  Accessing, modifying, .length (no parentheses!)             ║
║  ✅  3 traversal types — for, for-each, reverse                  ║
║  ✅  Arrays passed by reference — methods modify original        ║
║  ✅  5 essential operations — max, min, sum, search, reverse     ║
║  ✅  Common pitfalls — off-by-one, wrong init, .length()         ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  90%                              ║
║  STRONG AREA    :  Traversal, statistics, two-pointer reverse   ║
║  WEAK AREA      :  Second largest one-pass logic (edge cases)   ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  Arrays are 0-indexed — always i < arr.length, not <=        ║
║  ▸  Initialize max/min with arr[0], never with 0                ║
║  ▸  Arrays passed by ref — methods change original!             ║
║  ▸  Two-pointer reverse is O(n) time, O(1) space — key pattern  ║
║  ▸  k % n before rotate — handles full rotation edge case       ║
║  ▸  arr.clone() for safe copy — preserve original               ║
║  ▸  O(n) one-pass > O(n log n) sort — always think efficiency   ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 05](./Day05_Functions.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 07 →](./Day07_Arrays_2D.md)

*Day 6 of 160 — Done ✅ | Streak: 6 Days 🔥🔥🔥🔥🔥🔥*

</div>
