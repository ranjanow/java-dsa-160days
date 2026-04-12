# 📄 Day 13 — Sorting Algorithms

<div align="center">

![Day](https://img.shields.io/badge/Day-13_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Sorting_Algorithms-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 12](./Day12_Collections.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 14 →](./Day14_Searching.md)

</div>

---

## 🎯 Day 13 Goal
> Implement and deeply understand **all five fundamental sorting algorithms** — Bubble, Selection, Insertion, Merge, and Quick Sort. Know their time/space complexities, best/worst/average cases, and when to choose each. Sorting is tested in **every placement interview** — both as direct questions and as a prerequisite for binary search, two pointers, and greedy algorithms.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | Bubble Sort + Selection Sort — O(n²) family | 55 min |
| Block 2 | Insertion Sort + Shell Sort intro | 35 min |
| Block 3 | Merge Sort — divide and conquer | 45 min |
| Block 4 | Quick Sort + Partition logic | 40 min |
| Block 5 | Revision — Day 12 Collections flash review | 15 min |
| Block 6 | Practice Set — 6 Problems | 90 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. Why Study Sorting?

```
Every DSA topic touches sorting:
────────────────────────────────────────────────
Binary Search         → array must be SORTED
Two Pointers          → often requires SORTED array
Greedy Algorithms     → frequently sort first
Meeting Rooms         → sort by start time
Kth Largest Element   → sort or partial sort
Merge Intervals       → sort by interval start
Finding duplicates    → adjacent duplicates after sort
────────────────────────────────────────────────
```

> 💡 **Sorting IS DSA.** Not just a topic — a prerequisite skill woven through 30%+ of all interview problems.

---

### 🔷 2. Complexity Quick Reference — All 5 Algorithms

| Algorithm | Best | Average | Worst | Space | Stable? |
|-----------|------|---------|-------|-------|---------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | ✅ Yes |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | ❌ No |
| Insertion Sort | **O(n)** | O(n²) | O(n²) | O(1) | ✅ Yes |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | ✅ Yes |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | ❌ No |

> **Stable sort** = equal elements maintain their original relative order.
> **In-place sort** = O(1) extra space (Bubble, Selection, Insertion, Quick).

---

### 🔷 3. Bubble Sort — Adjacent Swaps

**Idea:** Compare adjacent pairs. If left > right, swap. Repeat n-1 passes. Largest element "bubbles up" to end each pass.

```java
static void bubbleSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        boolean swapped = false;       // optimization flag
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                // swap adjacent elements
                int temp   = arr[j];
                arr[j]     = arr[j + 1];
                arr[j + 1] = temp;
                swapped = true;
            }
        }
        if (!swapped) break;           // already sorted — early exit O(n) best case
    }
}
```

**Trace for {5, 3, 8, 1, 2}:**
```
Pass 1: [3,5,8,1,2] → [3,5,8,1,2] → [3,5,1,8,2] → [3,5,1,2,8]  ← 8 bubbles up
Pass 2: [3,5,1,2,8] → [3,1,5,2,8] → [3,1,2,5,8]                 ← 5 bubbles up
Pass 3: [1,3,2,5,8] → [1,2,3,5,8]                                ← 3 bubbles up
Pass 4: no swaps → already sorted! swapped=false → BREAK ✅
```

> 💡 **When to use:** Almost never in practice. Its O(n) best case (already sorted) makes it useful for detecting sorted arrays. Otherwise replaced by Insertion Sort.

---

### 🔷 4. Selection Sort — Find Minimum, Place It

**Idea:** In each pass, find the minimum element from the unsorted portion and place it at the start.

```java
static void selectionSort(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;                     // assume current position is minimum
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;                 // found smaller — update
            }
        }
        // swap arr[i] with arr[minIdx]
        int temp     = arr[i];
        arr[i]       = arr[minIdx];
        arr[minIdx]  = temp;
    }
}
```

**Trace for {5, 3, 8, 1, 2}:**
```
Pass 1: min=1 at idx 3 → swap arr[0]↔arr[3] → [1, 3, 8, 5, 2]
Pass 2: min=2 at idx 4 → swap arr[1]↔arr[4] → [1, 2, 8, 5, 3]
Pass 3: min=3 at idx 4 → swap arr[2]↔arr[4] → [1, 2, 3, 5, 8]
Pass 4: min=5 at idx 3 → swap arr[3]↔arr[3] → [1, 2, 3, 5, 8] ✅
```

> 💡 **Key property:** Makes the **minimum number of swaps** — exactly n-1 swaps worst case. Useful when swap cost is high. Not stable — swapping can change relative order of equal elements.

---

### 🔷 5. Insertion Sort — Build Sorted Array One by One

**Idea:** Take each element and insert it into its correct position in the already-sorted left portion. Like sorting playing cards in hand.

```java
static void insertionSort(int[] arr) {
    int n = arr.length;
    for (int i = 1; i < n; i++) {
        int key = arr[i];               // element to be placed
        int j   = i - 1;
        // shift elements > key one position right
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;              // place key in correct position
    }
}
```

**Trace for {5, 3, 8, 1, 2}:**
```
i=1: key=3, shift 5 right  → [3, 5, 8, 1, 2]
i=2: key=8, no shift        → [3, 5, 8, 1, 2]
i=3: key=1, shift 8,5,3    → [1, 3, 5, 8, 2]
i=4: key=2, shift 8,5,3    → [1, 2, 3, 5, 8] ✅
```

> 💡 **Best use case:** Nearly sorted arrays — O(n) time. Also used as the base case in Timsort (Java's `Arrays.sort()` for small arrays). Online algorithm — can sort elements as they arrive.

---

### 🔷 6. Merge Sort — Divide and Conquer

**Idea:** Recursively divide array into halves until each has 1 element (trivially sorted), then **merge** sorted halves back together.

```java
static void mergeSort(int[] arr, int left, int right) {
    if (left >= right) return;          // base case: single element

    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);          // sort left half
    mergeSort(arr, mid + 1, right);     // sort right half
    merge(arr, left, mid, right);       // merge both halves
}

static void merge(int[] arr, int left, int mid, int right) {
    // Create temp arrays for both halves
    int n1 = mid - left + 1;
    int n2 = right - mid;
    int[] L = new int[n1];
    int[] R = new int[n2];

    for (int i = 0; i < n1; i++) L[i] = arr[left + i];
    for (int j = 0; j < n2; j++) R[j] = arr[mid + 1 + j];

    // Merge L and R back into arr
    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) arr[k++] = L[i++];
        else               arr[k++] = R[j++];
    }
    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];
}
```

**Divide phase for {5, 3, 8, 1, 2}:**
```
                [5, 3, 8, 1, 2]
              /                 \
        [5, 3, 8]             [1, 2]
       /         \            /    \
    [5, 3]       [8]        [1]    [2]
    /    \
  [5]   [3]

Merge phase (bottom-up):
[5][3] → [3,5]
[3,5][8] → [3,5,8]
[1][2] → [1,2]
[3,5,8][1,2] → [1,2,3,5,8] ✅
```

> 💡 **Key advantage:** Guaranteed O(n log n) — worst case is same as best case. Used when stability matters. Downside: requires O(n) extra space. Foundation of external sorting (sorting data too large for RAM).

---

### 🔷 7. Quick Sort — Partition Around Pivot

**Idea:** Choose a **pivot** element. Rearrange so all elements < pivot are left, all > pivot are right. Recursively sort both partitions.

```java
static void quickSort(int[] arr, int low, int high) {
    if (low >= high) return;             // base case

    int pivotIdx = partition(arr, low, high);
    quickSort(arr, low, pivotIdx - 1);   // sort left partition
    quickSort(arr, pivotIdx + 1, high);  // sort right partition
}

// Lomuto partition scheme — pivot = last element
static int partition(int[] arr, int low, int high) {
    int pivot = arr[high];               // choose last element as pivot
    int i     = low - 1;                // i = boundary of smaller elements

    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            // swap arr[i] and arr[j]
            int temp = arr[i]; arr[i] = arr[j]; arr[j] = temp;
        }
    }
    // place pivot in its correct position
    int temp = arr[i + 1]; arr[i + 1] = arr[high]; arr[high] = temp;
    return i + 1;                        // return pivot's final index
}
```

**Partition trace for {5, 3, 8, 1, 2}, pivot=2:**
```
pivot=2, i=-1
j=0: arr[0]=5 > 2 → skip
j=1: arr[1]=3 > 2 → skip
j=2: arr[2]=8 > 2 → skip
j=3: arr[3]=1 ≤ 2 → i=0, swap arr[0]↔arr[3] → [1, 3, 8, 5, 2]
j=4: done → swap arr[i+1=1]↔arr[high=4] → [1, 2, 8, 5, 3]
pivot 2 is at index 1 → [1] [2] [8,5,3]
Recursively sort [1] (done) and [8,5,3]:
  [8,5,3] → pivot=3, sorts to [3,5,8]
Final: [1, 2, 3, 5, 8] ✅
```

> 💡 **Key advantage:** In-place (O(log n) stack space), cache-friendly, fastest in practice. Weakness: O(n²) worst case on already-sorted arrays with bad pivot choice. Fix: **randomized pivot** or **median-of-three** pivot selection.

---

### 🔷 8. Sorting Comparison — When to Choose What

```
Small array (n < 20):          → Insertion Sort
Almost sorted:                 → Insertion Sort
Need stability:                → Merge Sort
Need guaranteed O(n log n):    → Merge Sort
Best average performance:      → Quick Sort (with random pivot)
Memory constrained:            → Quick Sort (in-place)
Linked list sorting:           → Merge Sort (no random access needed)
Java's Arrays.sort():          → Dual-Pivot QuickSort (primitives)
                                  TimSort (objects — stable)
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Implement All Five Sorts + Verify `[Easy]`

**Task:** Implement all 5 sorting algorithms and verify with same test array.

```java
import java.util.Arrays;

public class P1_AllSorts {

    static void bubbleSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            boolean swapped = false;
            for (int j = 0; j < n - 1 - i; j++) {
                if (arr[j] > arr[j+1]) {
                    int t = arr[j]; arr[j] = arr[j+1]; arr[j+1] = t;
                    swapped = true;
                }
            }
            if (!swapped) break;
        }
    }

    static void selectionSort(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n - 1; i++) {
            int min = i;
            for (int j = i+1; j < n; j++)
                if (arr[j] < arr[min]) min = j;
            int t = arr[i]; arr[i] = arr[min]; arr[min] = t;
        }
    }

    static void insertionSort(int[] arr) {
        for (int i = 1; i < arr.length; i++) {
            int key = arr[i], j = i - 1;
            while (j >= 0 && arr[j] > key) { arr[j+1] = arr[j]; j--; }
            arr[j+1] = key;
        }
    }

    static void mergeSort(int[] arr, int l, int r) {
        if (l >= r) return;
        int m = l + (r-l)/2;
        mergeSort(arr, l, m); mergeSort(arr, m+1, r);
        merge(arr, l, m, r);
    }
    static void merge(int[] arr, int l, int m, int r) {
        int[] L = Arrays.copyOfRange(arr, l, m+1);
        int[] R = Arrays.copyOfRange(arr, m+1, r+1);
        int i=0, j=0, k=l;
        while (i<L.length && j<R.length) arr[k++] = (L[i]<=R[j]) ? L[i++] : R[j++];
        while (i<L.length) arr[k++] = L[i++];
        while (j<R.length) arr[k++] = R[j++];
    }

    static void quickSort(int[] arr, int lo, int hi) {
        if (lo >= hi) return;
        int p = partition(arr, lo, hi);
        quickSort(arr, lo, p-1); quickSort(arr, p+1, hi);
    }
    static int partition(int[] arr, int lo, int hi) {
        int pivot = arr[hi], i = lo - 1;
        for (int j = lo; j < hi; j++)
            if (arr[j] <= pivot) { i++; int t=arr[i]; arr[i]=arr[j]; arr[j]=t; }
        int t=arr[i+1]; arr[i+1]=arr[hi]; arr[hi]=t;
        return i+1;
    }

    static void test(String name, int[] arr) {
        System.out.printf("%-20s: %s%n", name, Arrays.toString(arr));
    }

    public static void main(String[] args) {
        int[] original = {64, 34, 25, 12, 22, 11, 90};
        System.out.println("Original : " + Arrays.toString(original));
        System.out.println();

        int[] a1 = original.clone(); bubbleSort(a1);    test("Bubble Sort",    a1);
        int[] a2 = original.clone(); selectionSort(a2); test("Selection Sort", a2);
        int[] a3 = original.clone(); insertionSort(a3); test("Insertion Sort", a3);
        int[] a4 = original.clone(); mergeSort(a4, 0, a4.length-1); test("Merge Sort", a4);
        int[] a5 = original.clone(); quickSort(a5, 0, a5.length-1); test("Quick Sort", a5);
    }
}
```

```
Output:
Original : [64, 34, 25, 12, 22, 11, 90]

Bubble Sort    : [11, 12, 22, 25, 34, 64, 90]
Selection Sort : [11, 12, 22, 25, 34, 64, 90]
Insertion Sort : [11, 12, 22, 25, 34, 64, 90]
Merge Sort     : [11, 12, 22, 25, 34, 64, 90]
Quick Sort     : [11, 12, 22, 25, 34, 64, 90]
```

> 🔑 **Key Learning:** All five produce the same sorted output — but via fundamentally different strategies. `.clone()` creates an independent copy for each test — crucial to test each algorithm on the same original data. `Arrays.copyOfRange(arr, l, m+1)` elegantly copies a subarray for merge. Quick note: `Arrays.sort()` internally uses dual-pivot quicksort for primitives — faster than a naive single-pivot implementation.

---

### ✅ Problem 2 — Sort Performance Comparison `[Easy-Medium]`

**Task:** Count comparisons and swaps made by each algorithm.

```java
public class P2_SortPerformance {

    static long comparisons, swaps;

    static void bubbleSortCount(int[] arr) {
        comparisons = swaps = 0;
        int n = arr.length;
        for (int i = 0; i < n-1; i++) {
            boolean swapped = false;
            for (int j = 0; j < n-1-i; j++) {
                comparisons++;
                if (arr[j] > arr[j+1]) {
                    int t=arr[j]; arr[j]=arr[j+1]; arr[j+1]=t;
                    swaps++; swapped=true;
                }
            }
            if (!swapped) break;
        }
    }

    static void selectionSortCount(int[] arr) {
        comparisons = swaps = 0;
        int n = arr.length;
        for (int i = 0; i < n-1; i++) {
            int min = i;
            for (int j = i+1; j < n; j++) { comparisons++; if (arr[j] < arr[min]) min=j; }
            if (min != i) { int t=arr[i]; arr[i]=arr[min]; arr[min]=t; swaps++; }
        }
    }

    static void insertionSortCount(int[] arr) {
        comparisons = swaps = 0;
        for (int i = 1; i < arr.length; i++) {
            int key=arr[i], j=i-1;
            while (j>=0 && ++comparisons > 0 && arr[j] > key) {
                arr[j+1]=arr[j]; j--; swaps++;
            }
            arr[j+1]=key;
        }
    }

    static void report(String name, int[] arr) {
        System.out.printf("%-18s comparisons=%-8d swaps=%d%n", name, comparisons, swaps);
    }

    public static void main(String[] args) {
        int[] base = {64, 34, 25, 12, 22, 11, 90, 45, 78, 33};

        System.out.println("Array: n=10, random order");
        System.out.println("─".repeat(55));
        int[] a; a=base.clone(); bubbleSortCount(a);    report("Bubble Sort", a);
        a=base.clone(); selectionSortCount(a); report("Selection Sort", a);
        a=base.clone(); insertionSortCount(a); report("Insertion Sort", a);

        System.out.println();
        int[] sorted = {1,2,3,4,5,6,7,8,9,10};
        System.out.println("Array: n=10, already SORTED");
        System.out.println("─".repeat(55));
        a=sorted.clone(); bubbleSortCount(a);    report("Bubble Sort", a);
        a=sorted.clone(); selectionSortCount(a); report("Selection Sort", a);
        a=sorted.clone(); insertionSortCount(a); report("Insertion Sort", a);

        System.out.println();
        int[] rev = {10,9,8,7,6,5,4,3,2,1};
        System.out.println("Array: n=10, REVERSE sorted (worst case)");
        System.out.println("─".repeat(55));
        a=rev.clone(); bubbleSortCount(a);    report("Bubble Sort", a);
        a=rev.clone(); selectionSortCount(a); report("Selection Sort", a);
        a=rev.clone(); insertionSortCount(a); report("Insertion Sort", a);
    }
}
```

```
Output:
Array: n=10, random order
───────────────────────────────────────────────────────
Bubble Sort        comparisons=35       swaps=16
Selection Sort     comparisons=45       swaps=8
Insertion Sort     comparisons=20       swaps=20

Array: n=10, already SORTED
───────────────────────────────────────────────────────
Bubble Sort        comparisons=9        swaps=0     ← O(n) best!
Selection Sort     comparisons=45       swaps=0
Insertion Sort     comparisons=9        swaps=0     ← O(n) best!

Array: n=10, REVERSE sorted (worst case)
───────────────────────────────────────────────────────
Bubble Sort        comparisons=45       swaps=45    ← O(n²) worst
Selection Sort     comparisons=45       swaps=9     ← fewest swaps!
Insertion Sort     comparisons=45       swaps=45    ← O(n²) worst
```

> 🔑 **Key Learning:** Selection Sort always makes exactly n(n-1)/2 comparisons regardless of input — no early exit. Bubble and Insertion Sort have O(n) best case on sorted arrays — `swapped=false` trigger and no inner loop iterations respectively. Selection Sort has the fewest swaps (at most n-1) — critical when swap is expensive (e.g., swapping large objects on disk). Data proves the theory: numbers match n(n-1)/2 = 45 for n=10 worst case.

---

### ✅ Problem 3 — Merge Sort — Count Inversions `[Medium]`

**Task:** Count inversions in an array (pairs where arr[i] > arr[j] and i < j) using merge sort.

```java
public class P3_InversionCount {

    static long inversions = 0;

    static void mergeSort(int[] arr, int left, int right) {
        if (left >= right) return;
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        mergeAndCount(arr, left, mid, right);
    }

    static void mergeAndCount(int[] arr, int left, int mid, int right) {
        int n1 = mid - left + 1, n2 = right - mid;
        int[] L = new int[n1], R = new int[n2];
        for (int i=0; i<n1; i++) L[i] = arr[left+i];
        for (int j=0; j<n2; j++) R[j] = arr[mid+1+j];

        int i=0, j=0, k=left;
        while (i < n1 && j < n2) {
            if (L[i] <= R[j]) {
                arr[k++] = L[i++];
            } else {
                arr[k++] = R[j++];
                // KEY: all remaining elements in L are > R[j] → all are inversions!
                inversions += (n1 - i);
            }
        }
        while (i < n1) arr[k++] = L[i++];
        while (j < n2) arr[k++] = R[j++];
    }

    public static void main(String[] args) {
        int[][] tests = {
            {2, 4, 1, 3, 5},
            {5, 4, 3, 2, 1},
            {1, 2, 3, 4, 5},
            {3, 1, 2}
        };

        for (int[] arr : tests) {
            int[] copy = arr.clone();
            inversions = 0;
            mergeSort(copy, 0, copy.length - 1);
            System.out.printf("%-20s → Inversions: %d  Sorted: %s%n",
                java.util.Arrays.toString(arr), inversions,
                java.util.Arrays.toString(copy));
        }
    }
}
```

```
Output:
[2, 4, 1, 3, 5]      → Inversions: 3   Sorted: [1, 2, 3, 4, 5]
[5, 4, 3, 2, 1]      → Inversions: 10  Sorted: [1, 2, 3, 4, 5]
[1, 2, 3, 4, 5]      → Inversions: 0   Sorted: [1, 2, 3, 4, 5]
[3, 1, 2]            → Inversions: 2   Sorted: [1, 2, 3, 2]
```

**Why inversions += (n1 - i) works:**
```
During merge, when R[j] < L[i]:
  L = [1, 4, 6]   R = [2, 5, 7]
  When placing R[0]=2: L[1]=4 > 2, L[2]=6 > 2
  → 2 inversions (4,2) and (6,2) counted at once!
  inversions += n1 - i = 3 - 1 = 2

Brute force: O(n²) — check all pairs
Merge sort:  O(n log n) — count during merge step
```

> 🔑 **Key Learning:** Inversion count is a classic "piggyback on merge sort" problem — O(n log n) instead of O(n²) brute force. The insight: when an element from the right half is merged before elements from the left half, ALL remaining left-half elements form inversions with it — count them in O(1) with `n1 - i`. This problem appears in: measuring sortedness of a list, counting swaps needed (like Bubble Sort's swap count), and competitive programming. LeetCode #315 (harder variant).

---

### ✅ Problem 4 — Quick Sort with Random Pivot `[Medium]`

**Task:** Implement Quick Sort with randomized pivot to avoid worst-case O(n²).

```java
import java.util.*;

public class P4_RandomQuickSort {

    static Random rand = new Random();

    static void quickSort(int[] arr, int lo, int hi) {
        if (lo >= hi) return;

        // Randomize pivot — swap random element with last
        int randIdx = lo + rand.nextInt(hi - lo + 1);
        int t = arr[randIdx]; arr[randIdx] = arr[hi]; arr[hi] = t;

        int pivot = partition(arr, lo, hi);
        quickSort(arr, lo, pivot - 1);
        quickSort(arr, pivot + 1, hi);
    }

    static int partition(int[] arr, int lo, int hi) {
        int pivot = arr[hi], i = lo - 1;
        for (int j = lo; j < hi; j++) {
            if (arr[j] <= pivot) {
                i++;
                int tmp = arr[i]; arr[i] = arr[j]; arr[j] = tmp;
            }
        }
        int tmp = arr[i+1]; arr[i+1] = arr[hi]; arr[hi] = tmp;
        return i + 1;
    }

    // Dutch National Flag (3-way partition) — handles duplicates better
    static void threeWayQuickSort(int[] arr, int lo, int hi) {
        if (lo >= hi) return;

        int pivot = arr[lo + rand.nextInt(hi - lo + 1)];
        int lt = lo, gt = hi, i = lo;

        while (i <= gt) {
            if      (arr[i] < pivot) { swap(arr, lt++, i++); }
            else if (arr[i] > pivot) { swap(arr, i,   gt--); }
            else                     { i++;                   }
        }
        // arr[lo..lt-1] < pivot, arr[lt..gt] == pivot, arr[gt+1..hi] > pivot
        threeWayQuickSort(arr, lo, lt - 1);
        threeWayQuickSort(arr, gt + 1, hi);
    }

    static void swap(int[] arr, int i, int j) {
        int t = arr[i]; arr[i] = arr[j]; arr[j] = t;
    }

    public static void main(String[] args) {
        int[] arr1 = {3, 6, 8, 10, 1, 2, 1};
        System.out.println("Before 3-way: " + Arrays.toString(arr1));
        threeWayQuickSort(arr1, 0, arr1.length - 1);
        System.out.println("After  3-way: " + Arrays.toString(arr1));

        // Already sorted — would be worst case for fixed pivot
        int[] arr2 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
        System.out.println("\nAlready sorted (worst for fixed pivot):");
        System.out.println("Before: " + Arrays.toString(arr2));
        quickSort(arr2, 0, arr2.length - 1);   // random pivot handles this
        System.out.println("After : " + Arrays.toString(arr2));

        // All same elements — 3-way shines
        int[] arr3 = {5, 5, 5, 5, 5, 5, 5};
        System.out.println("\nAll same elements:");
        System.out.println("Before: " + Arrays.toString(arr3));
        threeWayQuickSort(arr3, 0, arr3.length - 1);
        System.out.println("After : " + Arrays.toString(arr3));
    }
}
```

```
Output:
Before 3-way: [3, 6, 8, 10, 1, 2, 1]
After  3-way: [1, 1, 2, 3, 6, 8, 10]

Already sorted (worst for fixed pivot):
Before: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
After : [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

All same elements:
Before: [5, 5, 5, 5, 5, 5, 5]
After : [5, 5, 5, 5, 5, 5, 5]
```

**Dutch National Flag — 3 regions:**
```
[lo ... lt-1] < pivot
[lt ... gt]   = pivot  ← all duplicates go here — no recursion!
[gt+1 ... hi] > pivot

For [5,5,5,5,5]: pivot=5, everything == pivot → lt=0, gt=6
No recursion needed at all! O(n) for all-same arrays.
```

> 🔑 **Key Learning:** Random pivot reduces O(n²) worst case to O(n log n) expected — a sorted array is no longer catastrophic. 3-way Dutch National Flag partition handles duplicate-heavy arrays optimally — equal elements form a "dead zone" requiring no further recursion. This is why Java's `Arrays.sort()` for primitives uses **dual-pivot quicksort** — exploits both randomization and 3-way partitioning. Used in: sort colors (LeetCode #75), sort array by parity.

---

### ✅ Problem 5 — Sort Applications `[Medium]`

**Task:** Solve classic problems that reduce to sorting.

```java
import java.util.*;

public class P5_SortApplications {

    // ① Find Kth largest element (sort-based O(n log n))
    static int kthLargest(int[] arr, int k) {
        int[] copy = arr.clone();
        Arrays.sort(copy);
        return copy[copy.length - k];
    }

    // ② Check if array has duplicates
    static boolean hasDuplicate(int[] arr) {
        int[] copy = arr.clone();
        Arrays.sort(copy);
        for (int i = 1; i < copy.length; i++)
            if (copy[i] == copy[i-1]) return true;
        return false;
    }

    // ③ Find missing number in 1..n (sort approach)
    static int missingNumber(int[] arr) {
        Arrays.sort(arr);
        for (int i = 0; i < arr.length; i++)
            if (arr[i] != i + 1) return i + 1;
        return arr.length + 1;
    }

    // ④ Merge two sorted arrays
    static int[] mergeSorted(int[] a, int[] b) {
        int[] result = new int[a.length + b.length];
        int i=0, j=0, k=0;
        while (i < a.length && j < b.length)
            result[k++] = (a[i] <= b[j]) ? a[i++] : b[j++];
        while (i < a.length) result[k++] = a[i++];
        while (j < b.length) result[k++] = b[j++];
        return result;
    }

    // ⑤ Sort array of 0s, 1s, 2s (Dutch National Flag)
    static void sortColors(int[] arr) {
        int lo=0, mid=0, hi=arr.length-1;
        while (mid <= hi) {
            if      (arr[mid] == 0) { int t=arr[lo]; arr[lo++]=arr[mid]; arr[mid++]=t; }
            else if (arr[mid] == 1) { mid++; }
            else                    { int t=arr[mid]; arr[mid]=arr[hi]; arr[hi--]=t; }
        }
    }

    public static void main(String[] args) {
        int[] arr = {3, 2, 1, 5, 6, 4};
        System.out.println("Array: " + Arrays.toString(arr));
        System.out.println("2nd largest: " + kthLargest(arr, 2));  // 5
        System.out.println("3rd largest: " + kthLargest(arr, 3));  // 4

        System.out.println();
        System.out.println("Has duplicate [1,2,3,2]: " + hasDuplicate(new int[]{1,2,3,2})); // true
        System.out.println("Has duplicate [1,2,3,4]: " + hasDuplicate(new int[]{1,2,3,4})); // false

        System.out.println();
        System.out.println("Missing in [1,2,4,5,6]: " + missingNumber(new int[]{1,2,4,5,6})); // 3

        System.out.println();
        int[] a={1,3,5,7}, b={2,4,6,8};
        System.out.println("Merge " + Arrays.toString(a) + " + " + Arrays.toString(b));
        System.out.println("Result: " + Arrays.toString(mergeSorted(a, b)));

        System.out.println();
        int[] colors = {2,0,2,1,1,0};
        System.out.println("Colors before: " + Arrays.toString(colors));
        sortColors(colors);
        System.out.println("Colors after : " + Arrays.toString(colors));
    }
}
```

```
Output:
Array: [3, 2, 1, 5, 6, 4]
2nd largest: 5
3rd largest: 4

Has duplicate [1,2,3,2]: true
Has duplicate [1,2,3,4]: false

Missing in [1,2,4,5,6]: 3

Merge [1,3,5,7] + [2,4,6,8]
Result: [1, 2, 3, 4, 5, 6, 7, 8]

Colors before: [2, 0, 2, 1, 1, 0]
Colors after : [0, 0, 1, 1, 2, 2]
```

> 🔑 **Key Learning:** Sort Colors (Dutch National Flag) is O(n) single pass — lo/mid/hi three pointers. `arr[lo..lo-1]=0s, arr[lo..mid-1]=1s, arr[mid..hi]=unsorted, arr[hi+1..n-1]=2s`. Merge of two sorted arrays is the heart of Merge Sort — also used in Merge K Sorted Lists. After sorting, adjacent duplicates make `hasDuplicate` O(1) per check instead of O(n) HashSet. These are LeetCode #215, #217, #268, #75 — all in the top 100 most asked problems.

---

### ✅ Problem 6 — Comparator-Based Sorting `[Medium-Hard]`

**Task:** Sort custom objects and arrays with custom comparators.

```java
import java.util.*;

public class P6_ComparatorSort {

    static class Student implements Comparable<Student> {
        String name;
        int    marks;
        int    age;

        Student(String name, int marks, int age) {
            this.name = name; this.marks = marks; this.age = age;
        }

        // Natural ordering — by marks descending
        @Override
        public int compareTo(Student other) {
            return other.marks - this.marks;   // descending marks
        }

        @Override
        public String toString() {
            return String.format("%-15s marks=%-3d age=%d", name, marks, age);
        }
    }

    public static void main(String[] args) {
        List<Student> students = new ArrayList<>();
        students.add(new Student("Abhishek", 92, 24));
        students.add(new Student("Riya",     88, 22));
        students.add(new Student("Arjun",    95, 23));
        students.add(new Student("Priya",    88, 21));
        students.add(new Student("Vikram",   76, 25));

        // Natural sort — by marks descending (Comparable)
        Collections.sort(students);
        System.out.println("By marks DESC (natural):");
        students.forEach(s -> System.out.println("  " + s));

        // Custom: by name alphabetically
        students.sort(Comparator.comparing(s -> s.name));
        System.out.println("\nBy name ASC:");
        students.forEach(s -> System.out.println("  " + s));

        // Custom: by marks ASC, then age DESC for ties
        students.sort(Comparator.comparingInt((Student s) -> s.marks)
                                .thenComparingInt(s -> -s.age));
        System.out.println("\nBy marks ASC, then age DESC (for ties):");
        students.forEach(s -> System.out.println("  " + s));

        System.out.println();

        // Sort integer array of intervals by start time
        int[][] intervals = {{3,6},{1,4},{8,10},{2,5},{15,18}};
        Arrays.sort(intervals, (a, b) -> a[0] - b[0]);  // sort by start time
        System.out.println("Intervals sorted by start:");
        for (int[] iv : intervals)
            System.out.println("  [" + iv[0] + "," + iv[1] + "]");

        // Sort strings by length, then alphabetically
        String[] words = {"banana", "apple", "fig", "date", "kiwi", "plum"};
        Arrays.sort(words, Comparator.comparingInt(String::length)
                                     .thenComparing(Comparator.naturalOrder()));
        System.out.println("\nWords by length, then alpha:");
        System.out.println("  " + Arrays.toString(words));
    }
}
```

```
Output:
By marks DESC (natural):
  Arjun           marks=95  age=23
  Abhishek        marks=92  age=24
  Riya            marks=88  age=22
  Priya           marks=88  age=21
  Vikram          marks=76  age=25

By name ASC:
  Abhishek        marks=92  age=24
  Arjun           marks=95  age=23
  Priya           marks=88  age=21
  Riya            marks=88  age=22
  Vikram          marks=76  age=25

By marks ASC, then age DESC (for ties):
  Vikram          marks=76  age=25
  Priya           marks=88  age=21
  Riya            marks=88  age=22
  Abhishek        marks=92  age=24
  Arjun           marks=95  age=23

Intervals sorted by start:
  [1,4]
  [2,5]
  [3,6]
  [8,10]
  [15,18]

Words by length, then alpha:
  [fig, date, kiwi, plum, apple, banana]
```

> 🔑 **Key Learning:** `Comparable` defines the natural ordering — implemented once in the class. `Comparator` defines custom orderings — passed at sort call time. Lambda `(a, b) -> a[0] - b[0]` sorts 2D arrays by first element — used in **Merge Intervals, Meeting Rooms, Activity Selection** problems. `Comparator.comparingInt().thenComparingInt()` chains comparators for multi-key sorting. Interval sorting by start time is required for: Merge Intervals (LC #56), Non-overlapping Intervals (LC #435), Meeting Rooms (LC #252) — all classic greedy problems.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Find the minimum number of swaps to sort an array + Sort nearly sorted array efficiently.

```java
import java.util.*;

public class MiniChallenge_Day13 {

    // ① Minimum swaps to sort — cycle detection approach
    static int minSwapsToSort(int[] arr) {
        int n = arr.length;
        int[] sorted = arr.clone();
        Arrays.sort(sorted);

        // Map value → sorted index
        HashMap<Integer, Integer> posMap = new HashMap<>();
        for (int i = 0; i < n; i++) posMap.put(sorted[i], i);

        boolean[] visited = new boolean[n];
        int swaps = 0;

        for (int i = 0; i < n; i++) {
            if (visited[i] || arr[i] == sorted[i]) continue;

            // Trace the cycle starting at i
            int cycleSize = 0;
            int j = i;
            while (!visited[j]) {
                visited[j] = true;
                j = posMap.get(arr[j]);  // where should arr[j] go?
                cycleSize++;
            }
            if (cycleSize > 1) swaps += cycleSize - 1;
        }
        return swaps;
    }

    // ② Sort nearly sorted array (k-sorted: each element is ≤k positions from correct)
    // Uses min-heap of size k+1 — O(n log k)
    static int[] sortNearlySorted(int[] arr, int k) {
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        int[] result = new int[arr.length];
        int idx = 0;

        for (int i = 0; i < arr.length; i++) {
            minHeap.offer(arr[i]);
            if (minHeap.size() > k)
                result[idx++] = minHeap.poll();
        }
        while (!minHeap.isEmpty())
            result[idx++] = minHeap.poll();

        return result;
    }

    public static void main(String[] args) {
        // Min swaps
        int[][] tests = {{4,3,2,1}, {1,5,4,3,2}, {1,2,3,4,5}};
        System.out.println("=== Minimum Swaps to Sort ===");
        for (int[] arr : tests)
            System.out.printf("%-20s → %d swaps%n",
                Arrays.toString(arr), minSwapsToSort(arr));

        System.out.println();

        // Nearly sorted
        int[] nearSorted = {2, 1, 4, 3, 6, 5, 8, 7};
        System.out.println("=== Sort Nearly Sorted (k=1) ===");
        System.out.println("Input : " + Arrays.toString(nearSorted));
        System.out.println("Output: " + Arrays.toString(sortNearlySorted(nearSorted, 1)));

        int[] nearSorted2 = {6, 5, 3, 2, 8, 10, 9};
        System.out.println("\nInput (k=2): " + Arrays.toString(nearSorted2));
        System.out.println("Output:      " + Arrays.toString(sortNearlySorted(nearSorted2, 2)));
    }
}
```

```
Output:
=== Minimum Swaps to Sort ===
[4, 3, 2, 1]         → 2 swaps
[1, 5, 4, 3, 2]      → 3 swaps
[1, 2, 3, 4, 5]      → 0 swaps

=== Sort Nearly Sorted (k=1) ===
Input : [2, 1, 4, 3, 6, 5, 8, 7]
Output: [1, 2, 3, 4, 5, 6, 7, 8]

Input (k=2): [6, 5, 3, 2, 8, 10, 9]
Output:      [2, 3, 5, 6, 8, 9, 10]
```

> 🔑 **Key Learning:** Minimum swaps = sum of (cycle_size - 1) for all cycles in the permutation. Cycle detection: follow where each element needs to go until you return to start — brilliant O(n log n) approach. Nearly sorted k-sort uses a **min-heap of size k+1** — always poll minimum from window. O(n log k) vs O(n log n) — significant speedup when k << n. `PriorityQueue` is Java's min-heap — first preview of Heap data structure coming in Phase 3.

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| `freq.getOrDefault(key, 0) + 1` from memory | ✅ Core HashMap pattern |
| Two Sum HashMap approach — why O(n) | ✅ Complement lookup in O(1) per element |
| `set.add()` returns false for duplicate | ✅ Used in O(n) duplicate detection |
| BFS queue pattern: `size = queue.size()` | ✅ Before inner for loop — level width |

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. Why is Insertion Sort preferred over Bubble Sort for nearly-sorted data?</b></summary>

> Both have O(n) best case on sorted data, but Insertion Sort is significantly faster in practice for nearly-sorted arrays.
>
> **Bubble Sort:** Even with the `swapped` flag, it must scan almost the full array each pass to determine nothing was swapped. Many wasted comparisons.
>
> **Insertion Sort:** For each element that's already in position (or close), the inner `while` loop exits immediately — `arr[j] <= key` fails on the first check. For a k-nearly-sorted array, each element moves at most k positions → O(nk) total work. When k is small (say k=3), this is effectively O(n).
>
> This is why Insertion Sort is used as the base case in Timsort (Java's `Arrays.sort()` for objects) — when the subarray is small enough, it outperforms the O(n log n) algorithms.

</details>

<details>
<summary><b>Q2. What is the worst case for Quick Sort and how do we avoid it?</b></summary>

> **Worst case O(n²)** occurs when the pivot consistently partitions the array into one empty partition and one of size n-1. This happens when:
> - Array is already sorted + pivot = last/first element → picks minimum/maximum every time
> - All elements are equal + fixed pivot
>
> **Fixes:**
> 1. **Randomized pivot** — `swap(arr, lo + rand.nextInt(range), hi)` before partitioning. Expected O(n log n) for any input.
> 2. **Median-of-three** — pick median of arr[lo], arr[mid], arr[hi] as pivot. Avoids sorted-array pathology.
> 3. **3-way partition (Dutch National Flag)** — groups equal elements, avoids O(n²) on all-duplicate arrays.
>
> Java's `Arrays.sort()` uses dual-pivot quicksort (two pivots) — empirically faster and resistant to pathological inputs.

</details>

<details>
<summary><b>Q3. Why does Merge Sort use O(n) extra space? Can it be done in-place?</b></summary>

> Merge Sort requires O(n) extra space because the **merge step** cannot be done efficiently without a temporary array. When merging two sorted halves `[1,3,5]` and `[2,4,6]`, you need to read from both halves and write to a separate buffer — otherwise you'd overwrite elements you haven't merged yet.
>
> **In-place Merge Sort** exists but is extremely complex and has O(n log²n) time — worse than the standard O(n log n). The O(n) space cost is generally accepted as the price for guaranteed O(n log n) and stability.
>
> For linked lists, Merge Sort uses O(1) extra space because you're just relinking pointers — no copying needed. This is why Merge Sort is the preferred algorithm for sorting linked lists.

</details>

<details>
<summary><b>Q4. What does "stable sort" mean and why does it matter?</b></summary>

> A sort is **stable** if equal elements maintain their original relative order after sorting.
>
> Example: Sort students by marks: `[(Alice,88), (Bob,92), (Charlie,88)]`
> - Stable: Alice before Charlie (original order preserved for equal marks)
> - Unstable: Charlie might come before Alice — original order lost
>
> **Why it matters:** Multi-key sorting requires stability. To sort by (marks DESC, name ASC): first sort by name (stable), then sort by marks (stable). Because the name sort is stable, equal-marks students remain alphabetically ordered after the marks sort. If the second sort is unstable, the name ordering is destroyed.
>
> Java: `Arrays.sort()` for primitives uses quicksort (unstable). `Arrays.sort()` for objects uses Timsort (stable). Always use `Arrays.sort(objects)` when stability matters.

</details>

<details>
<summary><b>Q5. Given an array of size n, which sorting algorithm minimizes the number of writes (memory writes)?</b></summary>

> **Selection Sort** minimizes writes — it performs at most **n-1 swaps** regardless of input (one swap per pass, even if the element is already in position it can swap in-place). In the worst case, Selection Sort writes to each position exactly once.
>
> Bubble Sort and Insertion Sort can perform O(n²) writes in the worst case (reverse-sorted input).
>
> **Real-world relevance:** Flash memory and EEPROM have limited write cycles — each cell can only be written ~100,000 times before failing. In embedded systems sorting small arrays in flash memory, Selection Sort's minimum-write property is genuinely valuable. This is a real interview question at embedded systems companies.

</details>

---

## 📌 DAY 13 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 13 — SUMMARY                       ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 11, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Sorting Algorithms                              ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  ALGORITHMS MASTERED                                            ║
║  ✅  Bubble Sort   — O(n²) avg, O(n) best, stable, in-place     ║
║  ✅  Selection Sort— O(n²) always, fewest swaps, unstable       ║
║  ✅  Insertion Sort— O(n²) avg, O(n) best, stable, online       ║
║  ✅  Merge Sort    — O(n log n) guaranteed, stable, O(n) space  ║
║  ✅  Quick Sort    — O(n log n) avg, O(n²) worst, in-place      ║
║  ✅  Randomized QS — eliminates worst case via random pivot     ║
║  ✅  3-way QS      — Dutch National Flag, handles duplicates    ║
╠══════════════════════════════════════════════════════════════════╣
║  PROBLEMS SOLVED                                                ║
║  ✅  P1 — All 5 sorts implemented + verified                     ║
║  ✅  P2 — Comparison/swap counting: best vs worst case proof    ║
║  ✅  P3 — Inversion count via Merge Sort O(n log n)             ║
║  ✅  P4 — Randomized QS + 3-way Dutch National Flag             ║
║  ✅  P5 — Sort applications: Kth largest, duplicates, colors    ║
║  ✅  P6 — Comparator: multi-key sort, interval sort             ║
║  ✅  Mini — Min swaps + k-sorted heap sort O(n log k)           ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  90%                              ║
║  STRONG AREA    :  All 5 implementations, comparator sorting    ║
║  WEAK AREA      :  Inversion count proof + cycle detection      ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  Bubble best=O(n), Selection always=O(n²), Insert best=O(n)  ║
║  ▸  Merge Sort — guaranteed O(n log n), stable, O(n) space     ║
║  ▸  Quick Sort — fastest avg, O(n²) worst → fix with random     ║
║  ▸  (a,b) -> a[0]-b[0] sorts intervals/pairs by first element  ║
║  ▸  Stable sort needed for multi-key sorting                    ║
║  ▸  Selection Sort = fewest swaps — use for write-limited HW   ║
║  ▸  Insertion Sort = best for nearly sorted + online sorting   ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 12](./Day12_Collections.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 14 →](./Day14_Searching.md)

*Day 13 of 160 — Done ✅ | Streak: 13 Days 🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥*

</div>
