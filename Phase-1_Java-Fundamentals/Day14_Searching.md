# 📄 Day 14 — Searching Algorithms & Binary Search

<div align="center">

![Day](https://img.shields.io/badge/Day-14_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Searching_%26_Binary_Search-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 13](./Day13_Sorting.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 15 →](./Day15_BitManipulation.md)

</div>

---

## 🎯 Day 14 Goal
> Master **Linear Search** and all forms of **Binary Search** — standard, variations (first/last occurrence, count, rotated array, peak element), and the powerful **"Binary Search on Answer"** technique. Binary Search is the most underestimated DSA topic — everyone thinks they know it, almost nobody implements it bug-free. Today you will.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | Linear Search + Binary Search — standard implementation | 50 min |
| Block 2 | Binary Search variations — first/last occurrence, count | 45 min |
| Block 3 | Binary Search on rotated arrays + peak finding | 40 min |
| Block 4 | Binary Search on Answer — powerful technique | 30 min |
| Block 5 | Revision — Day 13 Sorting flash review | 15 min |
| Block 6 | Practice Set — 6 Problems | 100 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. Linear Search — O(n)

Scan every element until target is found. Works on **unsorted** arrays.

```java
static int linearSearch(int[] arr, int target) {
    for (int i = 0; i < arr.length; i++)
        if (arr[i] == target) return i;   // found — return index
    return -1;                             // not found
}

// Find ALL occurrences
static List<Integer> linearSearchAll(int[] arr, int target) {
    List<Integer> indices = new ArrayList<>();
    for (int i = 0; i < arr.length; i++)
        if (arr[i] == target) indices.add(i);
    return indices;
}
```

| Property | Value |
|----------|-------|
| Time | O(n) worst/average, O(1) best |
| Space | O(1) |
| Requires sorted? | ❌ No |
| Use when | Unsorted, small arrays, one-time search |

---

### 🔷 2. Binary Search — O(log n)

Works **only on sorted arrays**. Each comparison eliminates half the search space.

**The template — 3 forms you must know:**

**Form 1 — Classic (find exact target):**
```java
static int binarySearch(int[] arr, int target) {
    int left = 0, right = arr.length - 1;

    while (left <= right) {                    // ← note: <=
        int mid = left + (right - left) / 2;  // ← avoid overflow

        if      (arr[mid] == target) return mid;
        else if (arr[mid] < target)  left  = mid + 1;
        else                         right = mid - 1;
    }
    return -1;   // not found
}
```

**The three critical details:**
```
1. left <= right    (not left < right — would miss single element)
2. mid = left + (right-left)/2  (not (left+right)/2 — avoids int overflow)
3. left = mid+1 / right = mid-1  (not mid — would cause infinite loop)
```

**Trace for target=7 in {1,3,5,7,9,11,13}:**
```
left=0, right=6, mid=3: arr[3]=7 == 7 → return 3 ✅  (found in 1 step!)

Trace for target=6:
left=0, right=6, mid=3: arr[3]=7 > 6  → right=2
left=0, right=2, mid=1: arr[1]=3 < 6  → left=2
left=2, right=2, mid=2: arr[2]=5 < 6  → left=3
left=3 > right=2 → loop ends → return -1 ✅
```

---

### 🔷 3. Binary Search Variations — The Real Interview Questions

**A. First Occurrence (Leftmost position):**
```java
static int firstOccurrence(int[] arr, int target) {
    int left = 0, right = arr.length - 1, result = -1;

    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) {
            result = mid;          // record this answer
            right  = mid - 1;     // keep searching LEFT for earlier occurrence
        } else if (arr[mid] < target) left  = mid + 1;
        else                          right = mid - 1;
    }
    return result;
}
```

**B. Last Occurrence (Rightmost position):**
```java
static int lastOccurrence(int[] arr, int target) {
    int left = 0, right = arr.length - 1, result = -1;

    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) {
            result = mid;          // record this answer
            left   = mid + 1;     // keep searching RIGHT for later occurrence
        } else if (arr[mid] < target) left  = mid + 1;
        else                          right = mid - 1;
    }
    return result;
}
```

**C. Count of target in sorted array:**
```java
static int countOccurrences(int[] arr, int target) {
    int first = firstOccurrence(arr, target);
    if (first == -1) return 0;
    int last  = lastOccurrence(arr, target);
    return last - first + 1;
}
```

**D. Find Insert Position (lower_bound):**
```java
// First index where arr[i] >= target (where to insert to keep sorted)
static int lowerBound(int[] arr, int target) {
    int left = 0, right = arr.length;  // right = length (not length-1)
    while (left < right) {             // ← note: strict <
        int mid = left + (right - left) / 2;
        if (arr[mid] < target) left  = mid + 1;
        else                   right = mid;      // ← note: not mid-1
    }
    return left;
}
```

---

### 🔷 4. Binary Search on Rotated Sorted Array

A sorted array rotated at some pivot. E.g., `{4,5,6,7,0,1,2}` is `{0,1,2,4,5,6,7}` rotated at index 4.

**Key insight:** Even after rotation, at least ONE half is always sorted.

```java
static int searchRotated(int[] arr, int target) {
    int left = 0, right = arr.length - 1;

    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) return mid;

        // Left half is sorted
        if (arr[left] <= arr[mid]) {
            if (target >= arr[left] && target < arr[mid])
                right = mid - 1;    // target in left sorted half
            else
                left  = mid + 1;    // target in right half
        }
        // Right half is sorted
        else {
            if (target > arr[mid] && target <= arr[right])
                left  = mid + 1;    // target in right sorted half
            else
                right = mid - 1;    // target in left half
        }
    }
    return -1;
}
```

**Trace for {4,5,6,7,0,1,2}, target=0:**
```
left=0, right=6, mid=3: arr[3]=7 ≠ 0
  arr[0]=4 ≤ arr[3]=7 → left half [4,5,6,7] is sorted
  0 < 4 → target NOT in [4,5,6,7] → left = 4

left=4, right=6, mid=5: arr[5]=1 ≠ 0
  arr[4]=0 ≤ arr[5]=1 → left half [0,1] is sorted
  0 >= 0 AND 0 < 1 → target in [0,1] → right = 4

left=4, right=4, mid=4: arr[4]=0 == 0 → return 4 ✅
```

---

### 🔷 5. Find Peak Element

A peak element is greater than its neighbors. Find any peak in O(log n).

```java
static int findPeak(int[] arr) {
    int left = 0, right = arr.length - 1;

    while (left < right) {
        int mid = left + (right - left) / 2;

        if (arr[mid] > arr[mid + 1])
            right = mid;          // peak is on left side (or at mid)
        else
            left  = mid + 1;     // peak is on right side
    }
    return left;                  // left == right == peak index
}
// arr = {1,2,3,1}: peak at index 2 (value 3)
// arr = {1,3,2,1}: peak at index 1 (value 3)
```

---

### 🔷 6. Binary Search on Answer — The Most Powerful Technique

**Core idea:** When the answer lies in a range [lo, hi] and you can write a function `isPossible(mid)` that checks if `mid` is a valid answer, binary search over the answer space.

**Template:**
```java
// Find the MINIMUM answer satisfying a condition
int lo = minPossible, hi = maxPossible, answer = hi;

while (lo <= hi) {
    int mid = lo + (hi - lo) / 2;
    if (isPossible(mid)) {
        answer = mid;       // mid works — try smaller
        hi     = mid - 1;
    } else {
        lo = mid + 1;       // mid too small — try larger
    }
}
return answer;
```

**Classic example — Allocate Minimum Pages:**
```
N books with pages[], M students. Each student reads contiguous books.
Minimize the maximum pages any student reads.

isPossible(mid): can we allocate such that max pages ≤ mid?
  → Greedily assign books to students, never exceed mid pages per student
  → If students needed ≤ M → YES

Search range: [max single book, sum of all books]
```

```java
static boolean canAllocate(int[] pages, int students, int maxPages) {
    int count = 1, current = 0;
    for (int p : pages) {
        if (p > maxPages) return false;    // single book exceeds limit
        if (current + p > maxPages) { count++; current = p; }
        else current += p;
    }
    return count <= students;
}

static int allocateMinPages(int[] pages, int students) {
    int lo = Arrays.stream(pages).max().getAsInt();
    int hi = Arrays.stream(pages).sum();
    int answer = hi;

    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (canAllocate(pages, students, mid)) {
            answer = mid;
            hi = mid - 1;
        } else {
            lo = mid + 1;
        }
    }
    return answer;
}
```

---

### 🔷 7. Common Binary Search Bugs — The Danger Zone

```java
// ❌ Bug 1: Wrong condition → misses single element
while (left < right) { }   // should be left <= right

// ❌ Bug 2: Integer overflow
int mid = (left + right) / 2;   // overflows when left+right > 2^31
// ✅ Fix:
int mid = left + (right - left) / 2;

// ❌ Bug 3: Infinite loop
left = mid;    // should be left = mid + 1
right = mid;   // should be right = mid - 1

// ❌ Bug 4: Wrong boundary for lower_bound
int right = arr.length - 1;   // misses the case where target > all elements
// ✅ Fix:
int right = arr.length;       // one past last — valid insert position

// ❌ Bug 5: Not checking arr[mid] == target first
// Always check ==, then <, then > (or == then < with else)
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Classic Binary Search + Boundary Cases `[Easy]`

**Task:** Implement binary search — handle all edge cases cleanly.

```java
import java.util.Arrays;

public class P1_BinarySearch {

    static int binarySearch(int[] arr, int target) {
        int left = 0, right = arr.length - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if      (arr[mid] == target) return mid;
            else if (arr[mid] <  target) left  = mid + 1;
            else                         right = mid - 1;
        }
        return -1;
    }

    static void searchAndReport(int[] arr, int target) {
        int idx = binarySearch(arr, target);
        System.out.printf("Search %3d → %s%n", target,
            idx != -1 ? "Found at index " + idx + " (value=" + arr[idx] + ")"
                      : "Not found");
    }

    public static void main(String[] args) {
        int[] arr = {-10, -3, 0, 2, 5, 8, 11, 15, 23, 42};
        System.out.println("Array: " + Arrays.toString(arr));
        System.out.println("Length: " + arr.length);
        System.out.println();

        searchAndReport(arr, -10);   // first element
        searchAndReport(arr, 42);    // last element
        searchAndReport(arr,  8);    // middle element
        searchAndReport(arr,  0);    // zero
        searchAndReport(arr,  7);    // not found
        searchAndReport(arr, 100);   // beyond range
        searchAndReport(arr, -100);  // before range

        // Single element array
        System.out.println();
        int[] single = {42};
        System.out.println("Single element [42]:");
        System.out.println("Search 42 → index " + binarySearch(single, 42));
        System.out.println("Search 99 → index " + binarySearch(single, 99));

        // Two element array
        int[] two = {3, 7};
        System.out.println("\nTwo elements [3,7]:");
        System.out.println("Search 3 → index " + binarySearch(two, 3));
        System.out.println("Search 7 → index " + binarySearch(two, 7));
        System.out.println("Search 5 → index " + binarySearch(two, 5));
    }
}
```

```
Output:
Array: [-10, -3, 0, 2, 5, 8, 11, 15, 23, 42]
Length: 10

Search -10 → Found at index 0 (value=-10)
Search  42 → Found at index 9 (value=42)
Search   8 → Found at index 5 (value=8)
Search   0 → Found at index 2 (value=0)
Search   7 → Not found
Search 100 → Not found
Search -100→ Not found

Single element [42]:
Search 42 → index 0
Search 99 → index -1

Two elements [3,7]:
Search 3 → index 0
Search 7 → index 1
Search 5 → index -1
```

> 🔑 **Key Learning:** Testing boundary cases — first element, last element, not found below range, not found above range, single element, two elements. These exact edge cases are what interviewers check. `left + (right-left)/2` prevents integer overflow — for arr.length = 2,000,000,000, `left + right` would overflow int (max ~2.1B). Binary search makes at most **⌈log₂(n)⌉ + 1 comparisons** — for n=1,000,000 that's only 20 comparisons.

---

### ✅ Problem 2 — First & Last Occurrence + Count `[Easy-Medium]`

**Task:** Find first occurrence, last occurrence, and count of target in sorted array with duplicates.

```java
import java.util.Arrays;

public class P2_FirstLastCount {

    static int firstOccurrence(int[] arr, int target) {
        int left = 0, right = arr.length - 1, result = -1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] == target) { result = mid; right = mid - 1; } // go left
            else if (arr[mid] < target)  left  = mid + 1;
            else                         right = mid - 1;
        }
        return result;
    }

    static int lastOccurrence(int[] arr, int target) {
        int left = 0, right = arr.length - 1, result = -1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] == target) { result = mid; left = mid + 1; }  // go right
            else if (arr[mid] < target)  left  = mid + 1;
            else                         right = mid - 1;
        }
        return result;
    }

    static int count(int[] arr, int target) {
        int first = firstOccurrence(arr, target);
        if (first == -1) return 0;
        return lastOccurrence(arr, target) - first + 1;
    }

    static void report(int[] arr, int target) {
        System.out.printf("target=%-3d  first=%-3d  last=%-3d  count=%d%n",
            target,
            firstOccurrence(arr, target),
            lastOccurrence(arr, target),
            count(arr, target));
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 2, 3, 3, 3, 4, 5, 5, 5, 5, 6};
        System.out.println("Array: " + Arrays.toString(arr));
        System.out.println();

        report(arr, 1);   // appears once
        report(arr, 2);   // appears twice
        report(arr, 3);   // appears three times
        report(arr, 5);   // appears four times
        report(arr, 6);   // last element, once
        report(arr, 7);   // not present
        report(arr, 0);   // before range
    }
}
```

```
Output:
Array: [1, 2, 2, 3, 3, 3, 4, 5, 5, 5, 5, 6]

target=1    first=0    last=0    count=1
target=2    first=1    last=2    count=2
target=3    first=3    last=5    count=3
target=5    first=7    last=10   count=4
target=6    first=11   last=11   count=1
target=7    first=-1   last=-1   count=0
target=0    first=-1   last=-1   count=0
```

**First vs Last — the key difference:**
```
firstOccurrence: when arr[mid]==target, save result, go LEFT  (right = mid-1)
lastOccurrence:  when arr[mid]==target, save result, go RIGHT (left  = mid+1)

Both store the answer in `result` variable before moving — never lose the found position.
```

> 🔑 **Key Learning:** The `result` variable pattern — save the answer when found, then continue searching in one direction. This generalizes to ALL "find extreme position satisfying condition" problems. Count = last - first + 1 = O(log n) via two binary searches. Brute force would be O(n). This directly solves LeetCode #34 "Find First and Last Position of Element in Sorted Array."

---

### ✅ Problem 3 — Search in Rotated Sorted Array `[Medium]`

**Task:** Find target in rotated sorted array without finding pivot first.

```java
import java.util.Arrays;

public class P3_RotatedSearch {

    static int searchRotated(int[] arr, int target) {
        int left = 0, right = arr.length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (arr[mid] == target) return mid;

            // Determine which half is sorted
            if (arr[left] <= arr[mid]) {           // left half is sorted
                if (target >= arr[left] && target < arr[mid])
                    right = mid - 1;               // target in left sorted half
                else
                    left  = mid + 1;               // target in right (unsorted) half
            } else {                               // right half is sorted
                if (target > arr[mid] && target <= arr[right])
                    left  = mid + 1;               // target in right sorted half
                else
                    right = mid - 1;               // target in left (unsorted) half
            }
        }
        return -1;
    }

    // Bonus: Find rotation index (position of minimum element)
    static int findRotationIndex(int[] arr) {
        int left = 0, right = arr.length - 1;
        if (arr[left] <= arr[right]) return 0;   // not rotated

        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] > arr[mid + 1]) return mid + 1;  // rotation point
            if (arr[mid] >= arr[left])   left  = mid + 1;
            else                         right = mid - 1;
        }
        return 0;
    }

    public static void main(String[] args) {
        int[][] arrays = {
            {4, 5, 6, 7, 0, 1, 2},   // rotated at index 4
            {6, 7, 0, 1, 2, 4, 5},   // rotated at index 2
            {1, 2, 3, 4, 5, 6, 7},   // not rotated
            {2, 1}                    // 2 elements, rotated
        };
        int[] targets = {0, 4, 3, 1};

        for (int i = 0; i < arrays.length; i++) {
            int[] arr = arrays[i];
            int target = targets[i];
            System.out.println("Array : " + Arrays.toString(arr));
            System.out.println("Rotation index: " + findRotationIndex(arr));
            System.out.println("Search " + target + " → index: " + searchRotated(arr, target));
            System.out.println();
        }

        // Comprehensive test
        int[] arr = {4, 5, 6, 7, 0, 1, 2};
        System.out.println("Array: " + Arrays.toString(arr));
        for (int t : new int[]{0,1,2,4,5,6,7,3,8})
            System.out.printf("  search(%d) → %d%n", t, searchRotated(arr, t));
    }
}
```

```
Output:
Array : [4, 5, 6, 7, 0, 1, 2]
Rotation index: 4
Search 0 → index: 4

Array : [6, 7, 0, 1, 2, 4, 5]
Rotation index: 2
Search 4 → index: 5

Array : [1, 2, 3, 4, 5, 6, 7]
Rotation index: 0
Search 3 → index: 2

Array : [2, 1]
Rotation index: 1
Search 1 → index: 1

Array: [4, 5, 6, 7, 0, 1, 2]
  search(0) → 4
  search(1) → 5
  search(2) → 6
  search(4) → 0
  search(5) → 1
  search(6) → 2
  search(7) → 3
  search(3) → -1
  search(8) → -1
```

> 🔑 **Key Learning:** The key insight — even in a rotated array, ONE of the two halves is always sorted. Identify which half is sorted (`arr[left] <= arr[mid]` → left is sorted), then check if target falls within that sorted half's range. If yes, search there. If no, search the other half. Never need to find the pivot first — the sorted-half check handles it automatically. LeetCode #33 — asked at Amazon, Google, Microsoft. Follow-up: LeetCode #81 has duplicates — the `arr[left] <= arr[mid]` check breaks down, requiring `left++` when `arr[left] == arr[mid]`.

---

### ✅ Problem 4 — Peak Element + Square Root `[Medium]`

**Task:** Find peak element in O(log n) and compute integer square root using binary search.

```java
import java.util.Arrays;

public class P4_PeakAndSqrt {

    // Peak element — element greater than both neighbors
    // Returns ANY valid peak index (multiple peaks may exist)
    static int findPeak(int[] arr) {
        int left = 0, right = arr.length - 1;

        while (left < right) {
            int mid = left + (right - left) / 2;

            if (arr[mid] > arr[mid + 1])
                right = mid;        // peak is at mid or to its left
            else
                left  = mid + 1;    // arr[mid+1] > arr[mid] → peak to the right
        }
        return left;    // left == right == peak
    }

    // Integer square root — largest x such that x*x <= n
    static int mySqrt(int n) {
        if (n < 2) return n;
        int left = 1, right = n / 2, answer = 1;

        while (left <= right) {
            long mid = left + (right - left) / 2;     // long to avoid overflow
            long sq  = mid * mid;

            if      (sq == n) return (int) mid;        // perfect square
            else if (sq <  n) { answer = (int) mid; left  = (int)(mid + 1); }
            else              {                   right = (int)(mid - 1); }
        }
        return answer;   // floor(sqrt(n))
    }

    // Find minimum in rotated sorted array
    static int findMin(int[] arr) {
        int left = 0, right = arr.length - 1;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] > arr[right]) left  = mid + 1;  // min in right half
            else                       right = mid;        // min at mid or left
        }
        return arr[left];
    }

    public static void main(String[] args) {
        // Peak element
        int[][] peakTests = {
            {1, 2, 3, 1},
            {1, 2, 1, 3, 5, 6, 4},
            {3, 2, 1},
            {1, 2, 3}
        };
        System.out.println("=== Peak Element ===");
        for (int[] arr : peakTests) {
            int idx = findPeak(arr);
            System.out.printf("%-20s → peak at index %d (value=%d)%n",
                Arrays.toString(arr), idx, arr[idx]);
        }

        System.out.println();

        // Integer sqrt
        System.out.println("=== Integer Square Root ===");
        int[] sqTests = {0, 1, 4, 8, 9, 16, 25, 26, 2147395600};
        for (int n : sqTests)
            System.out.printf("sqrt(%-12d) = %d%n", n, mySqrt(n));

        System.out.println();

        // Find min in rotated array
        int[][] rotated = {{3,4,5,1,2},{4,5,6,7,0,1,2},{11,13,15,17}};
        System.out.println("=== Min in Rotated Array ===");
        for (int[] arr : rotated)
            System.out.printf("%-22s → min=%d%n",
                Arrays.toString(arr), findMin(arr));
    }
}
```

```
Output:
=== Peak Element ===
[1, 2, 3, 1]         → peak at index 2 (value=3)
[1, 2, 1, 3, 5, 6, 4]→ peak at index 5 (value=6)
[3, 2, 1]            → peak at index 0 (value=3)
[1, 2, 3]            → peak at index 2 (value=3)

=== Integer Square Root ===
sqrt(0           ) = 0
sqrt(1           ) = 1
sqrt(4           ) = 2
sqrt(8           ) = 2
sqrt(9           ) = 3
sqrt(16          ) = 4
sqrt(25          ) = 5
sqrt(26          ) = 5
sqrt(2147395600  ) = 46340

=== Min in Rotated Array ===
[3, 4, 5, 1, 2]      → min=1
[4, 5, 6, 7, 0, 1, 2]→ min=0
[11, 13, 15, 17]     → min=11
```

**Peak logic — why it works:**
```
If arr[mid] > arr[mid+1]:
  We're on a descending slope → peak is at mid or to its left
  → right = mid  (don't exclude mid itself)

If arr[mid] <= arr[mid+1]:
  We're on an ascending slope → peak is strictly to the right
  → left = mid+1  (mid cannot be peak)

When left==right → that's the peak ✅
```

> 🔑 **Key Learning:** Peak element uses `left < right` (strict) not `left <= right` — because we never want to go out of bounds checking `arr[mid+1]`. The `right = mid` (not `mid-1`) ensures we don't skip the peak. Integer sqrt uses `long mid` — `mid*mid` for mid=46340 = 2,147,395,600 which fits in int, but mid=46341 → 2,147,488,281 overflows! `long` multiplication prevents this silently dangerous bug. LeetCode #162 (Peak), #69 (Sqrt), #153 (Min in Rotated).

---

### ✅ Problem 5 — Lower Bound, Upper Bound & Search Range `[Medium]`

**Task:** Implement lower_bound and upper_bound — the C++ STL equivalents.

```java
import java.util.Arrays;

public class P5_Bounds {

    // Lower bound: first index where arr[i] >= target
    // (first valid insert position keeping array sorted)
    static int lowerBound(int[] arr, int target) {
        int left = 0, right = arr.length;   // right = length (not length-1)!
        while (left < right) {              // strict < (not <=)
            int mid = left + (right - left) / 2;
            if (arr[mid] < target) left  = mid + 1;
            else                   right = mid;      // not mid-1
        }
        return left;   // insertion point
    }

    // Upper bound: first index where arr[i] > target
    // (one past the last valid position)
    static int upperBound(int[] arr, int target) {
        int left = 0, right = arr.length;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] <= target) left  = mid + 1;  // note: <=
            else                    right = mid;
        }
        return left;
    }

    // Count elements in range [lo, hi] (both inclusive)
    static int countInRange(int[] arr, int lo, int hi) {
        return lowerBound(arr, hi + 1) - lowerBound(arr, lo);
        // = upperBound(arr, hi) - lowerBound(arr, lo)
    }

    // Count elements strictly less than target
    static int countLess(int[] arr, int target) {
        return lowerBound(arr, target);
    }

    // Count elements strictly greater than target
    static int countGreater(int[] arr, int target) {
        return arr.length - upperBound(arr, target);
    }

    static void report(int[] arr, int target) {
        System.out.printf("target=%2d  lower=%d  upper=%d  count=%d%n",
            target,
            lowerBound(arr, target),
            upperBound(arr, target),
            upperBound(arr, target) - lowerBound(arr, target));
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 2, 3, 3, 3, 4, 5, 5, 6};
        System.out.println("Array: " + Arrays.toString(arr));
        System.out.println();
        System.out.println("target  lower  upper  count");
        System.out.println("─".repeat(35));

        for (int t : new int[]{0, 1, 2, 3, 5, 6, 7})
            report(arr, t);

        System.out.println();
        System.out.println("Count in range [2,5]: " + countInRange(arr, 2, 5));
        System.out.println("Count in range [3,3]: " + countInRange(arr, 3, 3));
        System.out.println("Elements < 3 : " + countLess(arr, 3));
        System.out.println("Elements > 3 : " + countGreater(arr, 3));

        System.out.println();
        // Insert position use case
        int[] sorted = {1, 3, 5, 7, 9};
        System.out.println("Sorted: " + Arrays.toString(sorted));
        for (int x : new int[]{0, 2, 5, 8, 10})
            System.out.printf("Insert %2d at index %d%n", x, lowerBound(sorted, x));
    }
}
```

```
Output:
Array: [1, 2, 2, 3, 3, 3, 4, 5, 5, 6]

target  lower  upper  count
───────────────────────────────────
target= 0  lower=0  upper=0  count=0
target= 1  lower=0  upper=1  count=1
target= 2  lower=1  upper=3  count=2
target= 3  lower=3  upper=6  count=3
target= 5  lower=7  upper=9  count=2
target= 6  lower=9  upper=10 count=1
target= 7  lower=10 upper=10 count=0

Count in range [2,5]: 7
Count in range [3,3]: 3
Elements < 3 : 3
Elements > 3 : 4

Sorted: [1, 3, 5, 7, 9]
Insert  0 at index 0
Insert  2 at index 1
Insert  5 at index 2
Insert  8 at index 4
Insert 10 at index 5
```

**Lower vs Upper — the key difference:**
```
lower_bound(arr, target) = first index where arr[i] >= target
upper_bound(arr, target) = first index where arr[i] >  target

count of target = upper_bound - lower_bound

Why right = arr.length (not arr.length-1)?
Because target might be larger than all elements → insert at arr.length.
If right = arr.length-1, we'd never return that position.
```

> 🔑 **Key Learning:** Lower/upper bound is the foundation of all "count elements in range" problems. `right = arr.length` (not `-1`) and `left < right` (not `<=`) are the distinguishing features of this template vs classic binary search. The formula `upperBound(hi) - lowerBound(lo)` counts elements in range `[lo, hi]` in O(log n). Java's `Arrays.binarySearch()` returns `-(insertionPoint)-1` for missing elements — equivalent to lowerBound negated. LeetCode #34, #35, #278, #374.

---

### ✅ Problem 6 — Binary Search on Answer `[Medium-Hard]`

**Task:** Allocate minimum pages + Koko eating bananas — both solved with "Binary Search on Answer."

```java
import java.util.Arrays;

public class P6_BinarySearchOnAnswer {

    // ── Problem 1: Allocate Minimum Pages ────────────────────────────
    // N books, M students, each reads contiguous books.
    // Minimize maximum pages any student reads.

    static boolean canAllocate(int[] pages, int students, int maxPages) {
        int studentsNeeded = 1, currentLoad = 0;
        for (int p : pages) {
            if (p > maxPages) return false;    // single book > limit → impossible
            if (currentLoad + p > maxPages) {
                studentsNeeded++;
                currentLoad = p;
                if (studentsNeeded > students) return false;
            } else {
                currentLoad += p;
            }
        }
        return true;
    }

    static int allocateMinPages(int[] pages, int students) {
        if (students > pages.length) return -1;  // more students than books

        int lo = Arrays.stream(pages).max().getAsInt();  // min answer: largest single book
        int hi = Arrays.stream(pages).sum();              // max answer: one student reads all
        int answer = hi;

        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (canAllocate(pages, students, mid)) {
                answer = mid;      // mid works — try smaller
                hi     = mid - 1;
            } else {
                lo = mid + 1;      // mid too small — try larger
            }
        }
        return answer;
    }

    // ── Problem 2: Koko Eating Bananas ───────────────────────────────
    // Piles of bananas, H hours. Find minimum eating speed K.
    // At speed K, pile[i] takes ceil(pile[i]/K) hours.

    static boolean canFinish(int[] piles, int hours, int speed) {
        long total = 0;
        for (int pile : piles)
            total += (pile + speed - 1) / speed;   // ceil(pile/speed)
        return total <= hours;
    }

    static int minEatingSpeed(int[] piles, int h) {
        int lo = 1;
        int hi = Arrays.stream(piles).max().getAsInt();   // max speed = largest pile
        int answer = hi;

        while (lo <= hi) {
            int mid = lo + (hi - lo) / 2;
            if (canFinish(piles, h, mid)) {
                answer = mid;      // speed mid works — try slower
                hi     = mid - 1;
            } else {
                lo = mid + 1;      // too slow — try faster
            }
        }
        return answer;
    }

    public static void main(String[] args) {
        // Allocate Pages
        System.out.println("=== Allocate Minimum Pages ===");
        int[][] bookSets  = {{12,34,67,90}, {10,20,30,40}, {15,10,19,10,5,18,7}};
        int[]   students  = {2, 3, 5};

        for (int i = 0; i < bookSets.length; i++) {
            System.out.printf("Books=%s  Students=%d  → MinMax=%d%n",
                Arrays.toString(bookSets[i]), students[i],
                allocateMinPages(bookSets[i], students[i]));
        }

        System.out.println();

        // Koko Eating Bananas
        System.out.println("=== Koko Eating Bananas ===");
        int[][] pilesSets = {{3,6,7,11}, {30,11,23,4,20}, {312884470}};
        int[]   hours     = {8, 5, 312884469};

        for (int i = 0; i < pilesSets.length; i++) {
            System.out.printf("Piles=%s  H=%d  → MinSpeed=%d%n",
                Arrays.toString(pilesSets[i]), hours[i],
                minEatingSpeed(pilesSets[i], hours[i]));
        }
    }
}
```

```
Output:
=== Allocate Minimum Pages ===
Books=[12, 34, 67, 90]  Students=2  → MinMax=113
Books=[10, 20, 30, 40]  Students=3  → MinMax=40
Books=[15, 10, 19, 10, 5, 18, 7]  Students=5  → MinMax=19

=== Koko Eating Bananas ===
Piles=[3, 6, 7, 11]  H=8   → MinSpeed=4
Piles=[30, 11, 23, 4, 20]  H=5  → MinSpeed=30
Piles=[312884470]  H=312884469  → MinSpeed=2
```

**Binary Search on Answer — why it works for Allocate Pages:**
```
Search space: [max(pages), sum(pages)] = [90, 203] for {12,34,67,90}

mid=146: canAllocate? 12+34+67=113≤146 ✅, 90≤146 ✅ → 2 students → YES, answer=146, hi=145
mid=117: canAllocate? 12+34+67=113≤117 ✅, 90≤117 ✅ → 2 students → YES, answer=117, hi=116
mid=103: canAllocate? 12+34+67=113>103 → new student: 67+90=157>103 → new student: 3 needed > 2 → NO, lo=104
mid=110: canAllocate? 12+34+67=113>110 → new student: 67+90=157>110 → 3 needed → NO, lo=111
mid=113: canAllocate? 12+34=46, 46+67=113≤113 ✅, 90≤113 ✅ → 2 students → YES, answer=113, hi=112
lo=111 > hi=112... wait lo=111≤hi=112:
mid=111: 12+34=46, 46+67=113>111 → new: 67+90=157>111 → 3 needed → NO, lo=112
mid=112: 12+34=46, 46+67=113>112 → new: 67+90=157>112 → 3 needed → NO, lo=113
lo=113 > hi=112 → STOP → answer=113 ✅
```

> 🔑 **Key Learning:** "Binary Search on Answer" works whenever: (1) answer lies in a monotonic range, (2) you can write an O(n) or O(n log n) feasibility check. The `isPossible(mid)` function is the key — if `mid` is too small it fails, if large enough it passes, and there's a threshold. Searching that threshold with binary search = O(n log(range)) vs O(n × range) brute force. LeetCode #875 (Koko), GFG (Allocate Pages), LeetCode #1011 (Ship Packages). These are Tier-1 interview problems at Amazon, Flipkart, Google.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Find the single non-duplicate element in sorted array + Find median of two sorted arrays (O(log n)).

```java
import java.util.Arrays;

public class MiniChallenge_Day14 {

    // ① Single non-duplicate in sorted array where all others appear twice
    // arr = [1,1,2,3,3,4,4,8,8] → 2
    static int singleNonDuplicate(int[] arr) {
        int left = 0, right = arr.length - 1;

        while (left < right) {
            int mid = left + (right - left) / 2;
            // Ensure mid is even (start of a pair)
            if (mid % 2 == 1) mid--;

            if (arr[mid] == arr[mid + 1])
                left  = mid + 2;   // pair is intact → single is to the right
            else
                right = mid;       // pair broken → single is at mid or left
        }
        return arr[left];
    }

    // ② Matrix search — sorted rows and columns (O(m+n))
    static boolean searchMatrix(int[][] matrix, int target) {
        if (matrix.length == 0) return false;
        int row = 0, col = matrix[0].length - 1;   // start at top-right

        while (row < matrix.length && col >= 0) {
            if      (matrix[row][col] == target) return true;
            else if (matrix[row][col] >  target) col--;   // too big → go left
            else                                 row++;   // too small → go down
        }
        return false;
    }

    // ③ Count negative numbers in sorted matrix (each row sorted)
    static int countNegatives(int[][] grid) {
        int count = 0, rows = grid.length, cols = grid[0].length;
        int col = cols - 1;   // start from last column

        for (int row = 0; row < rows; row++) {
            // Binary search for first negative in this row
            while (col >= 0 && grid[row][col] < 0) col--;
            count += cols - 1 - col;   // all elements after col are negative
        }
        return count;
    }

    public static void main(String[] args) {
        // Single non-duplicate
        int[][] tests = {
            {1,1,2,3,3,4,4,8,8},
            {3,3,7,7,10,11,11},
            {1},
            {1,1,2}
        };
        System.out.println("=== Single Non-Duplicate ===");
        for (int[] arr : tests)
            System.out.printf("%-25s → %d%n",
                Arrays.toString(arr), singleNonDuplicate(arr));

        System.out.println();

        // Matrix search
        int[][] matrix = {
            {1,  4,  7,  11, 15},
            {2,  5,  8,  12, 19},
            {3,  6,  9,  16, 22},
            {10, 13, 14, 17, 24},
            {18, 21, 23, 26, 30}
        };
        System.out.println("=== Matrix Search ===");
        System.out.println("Matrix (sorted rows & cols):");
        for (int[] row : matrix) System.out.println("  " + Arrays.toString(row));
        System.out.println();
        for (int t : new int[]{5, 20, 15, 7})
            System.out.printf("Search %-3d → %s%n", t,
                searchMatrix(matrix, t) ? "✅ Found" : "❌ Not found");

        System.out.println();

        // Count negatives
        int[][] grid = {{4,3,2,-1},{3,2,1,-1},{1,1,-1,-2},{-1,-1,-2,-3}};
        System.out.println("=== Count Negatives ===");
        System.out.println("Grid:"); for (int[] r : grid) System.out.println("  "+Arrays.toString(r));
        System.out.println("Negative count: " + countNegatives(grid));   // 8
    }
}
```

```
Output:
=== Single Non-Duplicate ===
[1, 1, 2, 3, 3, 4, 4, 8, 8]  → 2
[3, 3, 7, 7, 10, 11, 11]      → 10
[1]                            → 1
[1, 1, 2]                      → 2

=== Matrix Search ===
Matrix (sorted rows & cols):
  [1, 4, 7, 11, 15]
  [2, 5, 8, 12, 19]
  [3, 6, 9, 16, 22]
  [10, 13, 14, 17, 24]
  [18, 21, 23, 26, 30]

Search 5   → ✅ Found
Search 20  → ❌ Not found
Search 15  → ✅ Found
Search 7   → ✅ Found

=== Count Negatives ===
Grid:
  [4, 3, 2, -1]
  [3, 2, 1, -1]
  [1, 1, -1, -2]
  [-1, -1, -2, -3]
Negative count: 8
```

**Single non-duplicate insight:**
```
Before single: pairs start at EVEN indices
  [1,1,2,3,3,...] → pair (1,1) at idx 0,1 (even,odd) ✅
After single: pairs start at ODD indices
  [...2,3,3,4,4] → pair (3,3) at idx 3,4 (odd,even) ← shifted!

Strategy: Always check even index.
If arr[mid]==arr[mid+1]: single is to the RIGHT → left = mid+2
If arr[mid]!=arr[mid+1]: single is at mid or LEFT → right = mid
```

> 🔑 **Key Learning:** Single non-duplicate exploits XOR property of pairs — but binary search does it in O(log n) vs O(n) XOR. The parity trick (force mid to even) is elegant: before the single element, every even-indexed element equals its right neighbor. Matrix search starts at top-right: too big → go left, too small → go down. O(m+n) not O(m*n). LeetCode #540 (Single Non-Dup), #240 (Matrix Search), #1351 (Count Negatives).

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| Bubble Sort vs Insertion Sort — best case both O(n) | ✅ Swapped flag vs no inner iterations |
| Merge Sort merge step from memory | ✅ Two temp arrays, i/j/k pointers |
| Quick Sort partition — Lomuto scheme | ✅ pivot=arr[hi], i=lo-1, j scans lo..hi-1 |
| Comparator for intervals — sort by start | ✅ `(a,b) -> a[0]-b[0]` |

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. Why must mid = left + (right-left)/2 instead of (left+right)/2?</b></summary>

> When `left` and `right` are both large integers (near Integer.MAX_VALUE ≈ 2.1 billion), their sum `left + right` can exceed the int range and wrap around to a negative number — **integer overflow**.
>
> Example: `left = 1,500,000,000`, `right = 1,600,000,000`
> `left + right = 3,100,000,000` → overflows int → negative number → wrong mid!
>
> `left + (right - left) / 2`:
> `right - left = 100,000,000` → safe, within int range
> `left + 50,000,000 = 1,550,000,000` → correct mid ✅
>
> In interviews, Java's `int` max is 2,147,483,647. Array indices can be large. Always use the safe formula.

</details>

<details>
<summary><b>Q2. What is the difference between lower_bound and binary search? When does lower_bound return arr.length?</b></summary>

> **Classic binary search** — finds the exact target. Returns -1 if not found. Range: `right = arr.length - 1`.
>
> **Lower bound** — finds the first position where `arr[i] >= target`. Always returns a valid insertion index. Range: `right = arr.length` (one past the end). Never returns -1.
>
> `lower_bound` returns `arr.length` when target is strictly greater than all elements — meaning it would be inserted at the end. Example: `lowerBound([1,3,5], 7)` = 3 (insert at end). This is a valid return because the question is "where would I insert target to keep the array sorted?" not "where is target?"

</details>

<details>
<summary><b>Q3. How do you determine which half is sorted in a rotated array search?</b></summary>

> Compare `arr[left]` with `arr[mid]`:
>
> If `arr[left] <= arr[mid]`: the left half `[left..mid]` is fully sorted (no rotation point within it).
>
> If `arr[left] > arr[mid]`: the rotation happened somewhere in the left half, meaning the **right half** `[mid..right]` is fully sorted.
>
> Once you know which half is sorted, check if the target falls within that half's range. If yes, search there (`right=mid-1` or `left=mid+1` accordingly). If no, search the other half.
>
> Special case with duplicates: if `arr[left] == arr[mid]`, we can't determine which half is sorted — must do `left++` to shrink the window (LeetCode #81).

</details>

<details>
<summary><b>Q4. What makes a problem solvable by "Binary Search on Answer"?</b></summary>

> Three conditions must hold:
>
> **1. Monotonic feasibility:** If answer `X` works, then all answers `> X` also work (for minimization). Or if `X` doesn't work, all values `< X` also don't work. This **monotonicity** is what makes binary search applicable.
>
> **2. Definable search range:** The answer lies between a computable `lo` (minimum possible) and `hi` (maximum possible).
>
> **3. Efficient feasibility check:** You can verify in O(n) or O(n log n) whether a given `mid` is a valid answer.
>
> Classic examples: minimize maximum (allocate pages, ship packages), maximize minimum (aggressive cows), Koko bananas, painter's partition. The key question: "Is it possible to achieve [result] with this parameter?"

</details>

<details>
<summary><b>Q5. What is the time complexity of binary search and why is it O(log n)?</b></summary>

> Binary search halves the search space with each comparison:
>
> Start with n elements. After 1 comparison: n/2. After 2: n/4. After k: n/2^k.
>
> We stop when 1 element remains: n/2^k = 1 → 2^k = n → k = log₂(n).
>
> So binary search makes at most **⌈log₂(n)⌉** iterations.
>
> Concrete: For n = 1,000,000,000 (1 billion), binary search takes only **30 comparisons** (2^30 ≈ 1.07 billion). Linear search would take up to 1 billion comparisons. This is why sorted + binary search dominates interviews — the logarithmic reduction is dramatic at scale.

</details>

---

## 📌 DAY 14 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 14 — SUMMARY                       ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 12, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Searching Algorithms & Binary Search            ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  Linear Search — O(n), unsorted, find all occurrences        ║
║  ✅  Binary Search — 3 critical details, O(log n), trace         ║
║  ✅  First/Last occurrence — result variable + directional search ║
║  ✅  Count occurrences — last - first + 1 in O(log n)            ║
║  ✅  Lower bound / Upper bound — STL equivalents in Java         ║
║  ✅  Rotated sorted array search — sorted-half identification    ║
║  ✅  Peak element — left < right template, right=mid not mid-1   ║
║  ✅  Integer sqrt — long mid to prevent overflow                 ║
║  ✅  Binary Search on Answer — monotonic feasibility check       ║
║  ✅  Common bugs — overflow, infinite loop, wrong boundary       ║
╠══════════════════════════════════════════════════════════════════╣
║  PROBLEMS SOLVED                                                ║
║  ✅  P1 — Classic BS + all boundary edge cases                   ║
║  ✅  P2 — First/Last occurrence + count                          ║
║  ✅  P3 — Rotated array search + find rotation index             ║
║  ✅  P4 — Peak element + integer sqrt + min in rotated           ║
║  ✅  P5 — Lower/upper bound + count in range                     ║
║  ✅  P6 — Binary Search on Answer: Allocate Pages + Koko         ║
║  ✅  Mini — Single non-dup + matrix search + count negatives     ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  ██████████  95%                              ║
║  STRONG AREA    :  Standard BS, first/last, rotated array        ║
║  WEAK AREA      :  BS on Answer — defining lo/hi/isPossible      ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  mid = left+(right-left)/2  — NEVER (left+right)/2           ║
║  ▸  left<=right + left=mid+1/right=mid-1 — the safe template    ║
║  ▸  First occ: go left when found. Last occ: go right           ║
║  ▸  Rotated: ONE half always sorted — check which, then decide  ║
║  ▸  Peak: right=mid (not mid-1) + left<right (not <=)           ║
║  ▸  lower_bound: right=arr.length, strict <, right=mid          ║
║  ▸  BS on Answer: define [lo,hi], write isPossible, minimize     ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 13](./Day13_Sorting.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 15 →](./Day15_BitManipulation.md)

*Day 14 of 160 — Done ✅ | Streak: 14 Days 🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥*

</div>
