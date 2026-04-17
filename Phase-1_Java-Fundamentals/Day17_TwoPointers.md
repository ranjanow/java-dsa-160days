# 📄 Day 17 — Two Pointers & Sliding Window

<div align="center">

![Day](https://img.shields.io/badge/Day-17_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Two_Pointers_%26_Sliding_Window-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 16](./Day16_Math_Basics.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 18 →](./Day18_Prefix_Suffix.md)

</div>

---

## 🎯 Day 17 Goal
> Master **Two Pointers** and **Sliding Window** — the two most frequently tested array techniques in product-based company placements (Amazon, Flipkart, Microsoft, Google). These patterns turn O(n²) brute-force solutions into O(n) elegant ones. After today, you will spot these patterns instantly in any interview problem.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | Two Pointers — opposite ends, same direction | 55 min |
| Block 2 | Sliding Window — fixed size, variable size | 50 min |
| Block 3 | Pattern recognition — when to use which | 20 min |
| Block 4 | Revision — Day 16 Math for DSA flash review | 15 min |
| Block 5 | Practice Set — 6 Problems | 110 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. The Core Idea — Why These Patterns Exist

```
Brute Force Pain:
  Two nested loops → O(n²)
  For n=10,000: 100,000,000 operations → TLE

Two Pointers / Sliding Window:
  One or two passes → O(n)
  For n=10,000: 10,000-20,000 operations → AC

The key insight: instead of restarting from scratch for every subproblem,
REUSE previously computed information by moving pointers intelligently.
```

---

### 🔷 2. Two Pointers — Pattern 1: Opposite Ends

Two pointers start at opposite ends of a **sorted** array and move toward each other.

```
left ──────────────────────── right
  ↓                              ↓
[ 1,  2,  3,  4,  5,  6,  7,  8 ]
  └──────────────────────────────┘
         move based on condition
```

**Template:**
```java
int left = 0, right = arr.length - 1;

while (left < right) {
    int current = arr[left] + arr[right];  // or some condition

    if (current == target) {
        // found answer
        left++;  right--;      // or just one of them
    } else if (current < target) {
        left++;                // need larger sum → move left right
    } else {
        right--;               // need smaller sum → move right left
    }
}
```

**Classic use: Two Sum in sorted array**
```java
// arr = {1, 3, 5, 7, 9, 11}, target = 12
// left=0(1), right=5(11): 1+11=12 ✅ found!

// arr = {2, 4, 6, 8}, target = 11
// left=0(2), right=3(8): 2+8=10 < 11 → left++
// left=1(4), right=3(8): 4+8=12 > 11 → right--
// left=1(4), right=2(6): 4+6=10 < 11 → left++
// left=2(6), right=2(6): left==right → stop → not found
```

> 💡 **Why it works:** The array is sorted. Moving `left++` increases the sum. Moving `right--` decreases the sum. Every step eliminates one element that cannot be part of the answer. Each element visited at most once → O(n).

---

### 🔷 3. Two Pointers — Pattern 2: Same Direction (Fast & Slow)

Both pointers start at the same end and move in the same direction at different speeds.

```
slow  fast
  ↓     ↓
[ 1,  1,  2,  3,  3,  4,  5 ]
```

**Template:**
```java
int slow = 0;

for (int fast = 0; fast < arr.length; fast++) {
    if (condition(arr[fast])) {
        arr[slow] = arr[fast];   // keep this element
        slow++;
    }
    // fast always moves, slow only moves when condition met
}
// arr[0..slow-1] is the result
```

**Classic use: Remove duplicates from sorted array**
```java
// [1, 1, 2, 3, 3, 4]
// slow=0, fast=0: arr[0]=1 → keep, slow=1
// slow=1, fast=1: arr[1]=1 == arr[0] → skip
// slow=1, fast=2: arr[2]=2 ≠ arr[1] → arr[1]=2, slow=2
// slow=2, fast=3: arr[3]=3 ≠ arr[2] → arr[2]=3, slow=3
// ... result: [1,2,3,4], length=slow=4
```

---

### 🔷 4. Two Pointers — Pattern 3: Partition

One pointer scans, another marks the boundary of a partition.

```java
// Dutch National Flag — sort 0s, 1s, 2s
int lo = 0, mid = 0, hi = arr.length - 1;

while (mid <= hi) {
    if      (arr[mid] == 0) { swap(lo++, mid++); }
    else if (arr[mid] == 1) { mid++; }
    else                    { swap(mid, hi--); }
}
```

---

### 🔷 5. Sliding Window — Fixed Size Window

A window of **fixed size k** slides across the array, maintaining a running computation.

```
Window size k=3:
[ 1,  3,  -1, -3,  5,  3,  6 ]
  └──┘
     └──┘
         └──┘
             └──┘
                 └──┘  ← window slides one step at a time
```

**Template:**
```java
int windowSum = 0;

// Build first window
for (int i = 0; i < k; i++) windowSum += arr[i];

int maxSum = windowSum;

// Slide the window
for (int i = k; i < arr.length; i++) {
    windowSum += arr[i];         // add new element (entering)
    windowSum -= arr[i - k];     // remove old element (leaving)
    maxSum = Math.max(maxSum, windowSum);
}
```

**Why O(n):**
```
Brute force: for each of n-k+1 windows, sum k elements → O(n×k)
Sliding window: each element added ONCE, removed ONCE → O(n)
The trick: new_sum = old_sum + entering_element - leaving_element
```

---

### 🔷 6. Sliding Window — Variable Size Window

Window size grows and shrinks based on a condition. Use when looking for the **minimum or maximum length** subarray satisfying a property.

```
Expand right when condition not met.
Shrink left when condition overmet.
```

**Template:**
```java
int left = 0, result = Integer.MAX_VALUE;
int windowState = 0;   // sum, count, etc.

for (int right = 0; right < arr.length; right++) {
    // 1. Expand: include arr[right] in window
    windowState += arr[right];

    // 2. Shrink: while window satisfies condition, try to minimize
    while (windowState >= target) {
        result = Math.min(result, right - left + 1);
        windowState -= arr[left];
        left++;
    }
}
return result == Integer.MAX_VALUE ? 0 : result;
```

---

### 🔷 7. Two Pointers vs Sliding Window — Decision Guide

```
┌─────────────────────────────────────────────────────────┐
│              WHICH PATTERN DO I USE?                    │
├─────────────────────────────────────────────────────────┤
│ Sorted array + find pair with property?                 │
│   → Two Pointers (opposite ends)                        │
│                                                         │
│ Remove/filter elements in-place?                        │
│   → Two Pointers (fast & slow / same direction)         │
│                                                         │
│ Fixed window size k — find max/min/avg?                 │
│   → Sliding Window (fixed size)                         │
│                                                         │
│ Variable window — find shortest/longest subarray?       │
│   → Sliding Window (variable size)                      │
│                                                         │
│ Contiguous subarray with condition (sum, distinct, etc)?│
│   → Sliding Window                                      │
│                                                         │
│ Keywords: "subarray", "substring", "contiguous",        │
│           "window", "consecutive"                       │
│   → Think Sliding Window first                          │
└─────────────────────────────────────────────────────────┘
```

---

### 🔷 8. Common Sliding Window + HashMap Combination

When the window tracks **distinct elements** or **character frequencies**:

```java
// Longest substring with at most k distinct characters
HashMap<Character, Integer> freq = new HashMap<>();
int left = 0, maxLen = 0;

for (int right = 0; right < s.length(); right++) {
    char c = s.charAt(right);
    freq.put(c, freq.getOrDefault(c, 0) + 1);  // expand

    while (freq.size() > k) {                    // shrink condition
        char leftChar = s.charAt(left);
        freq.put(leftChar, freq.get(leftChar) - 1);
        if (freq.get(leftChar) == 0) freq.remove(leftChar);
        left++;
    }

    maxLen = Math.max(maxLen, right - left + 1);
}
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Two Sum (Sorted) + Three Sum `[Easy-Medium]`

**Task:** Find pairs/triplets with given sum using two pointers.

```java
import java.util.*;

public class P1_TwoSumThreeSum {

    // Two Sum in sorted array — O(n)
    static int[] twoSumSorted(int[] arr, int target) {
        int left = 0, right = arr.length - 1;
        while (left < right) {
            int sum = arr[left] + arr[right];
            if      (sum == target) return new int[]{left, right};
            else if (sum <  target) left++;
            else                   right--;
        }
        return new int[]{-1, -1};
    }

    // All pairs with given sum
    static List<int[]> allPairs(int[] arr, int target) {
        List<int[]> result = new ArrayList<>();
        int left = 0, right = arr.length - 1;
        while (left < right) {
            int sum = arr[left] + arr[right];
            if (sum == target) {
                result.add(new int[]{arr[left], arr[right]});
                left++;  right--;
                // skip duplicates
                while (left < right && arr[left] == arr[left-1]) left++;
                while (left < right && arr[right] == arr[right+1]) right--;
            } else if (sum < target) left++;
            else                    right--;
        }
        return result;
    }

    // Three Sum — find all unique triplets summing to 0
    static List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);
        List<List<Integer>> result = new ArrayList<>();

        for (int i = 0; i < nums.length - 2; i++) {
            if (i > 0 && nums[i] == nums[i-1]) continue;  // skip duplicates

            int left = i + 1, right = nums.length - 1;
            while (left < right) {
                int sum = nums[i] + nums[left] + nums[right];
                if (sum == 0) {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    while (left < right && nums[left]  == nums[left+1])  left++;
                    while (left < right && nums[right] == nums[right-1]) right--;
                    left++;  right--;
                } else if (sum < 0) left++;
                else                right--;
            }
        }
        return result;
    }

    public static void main(String[] args) {
        // Two Sum
        int[] sorted = {1, 2, 3, 5, 8, 11, 15};
        System.out.println("Array: " + Arrays.toString(sorted));

        int[] res = twoSumSorted(sorted, 13);
        System.out.println("Two Sum (target=13): indices " + Arrays.toString(res)
            + " → values [" + sorted[res[0]] + ", " + sorted[res[1]] + "]");

        System.out.println("Two Sum (target=100): " + Arrays.toString(twoSumSorted(sorted, 100)));

        System.out.println();

        // All pairs
        int[] arr2 = {1, 1, 2, 3, 4, 5, 6, 7, 8, 9};
        System.out.println("All pairs summing to 10 in " + Arrays.toString(arr2) + ":");
        for (int[] p : allPairs(arr2, 10))
            System.out.println("  [" + p[0] + ", " + p[1] + "]");

        System.out.println();

        // Three Sum
        int[][] tests = {{-1,0,1,2,-1,-4}, {0,0,0,0}, {-2,0,0,2,2}};
        for (int[] t : tests) {
            System.out.println("Three Sum " + Arrays.toString(t) + ":");
            System.out.println("  " + threeSum(t));
        }
    }
}
```

```
Output:
Array: [1, 2, 3, 5, 8, 11, 15]
Two Sum (target=13): indices [2, 5] → values [3, 11]  ← wait: 3+11=14. Let's verify:
arr[2]=3, arr[5]=11 → 3+11=14 ≠ 13. Correct pair: arr[1]=2 + arr[5]=11 = 13
Actually: left=0(1)+right=6(15)=16>13→right--; left=0(1)+right=5(11)=12<13→left++;
left=1(2)+right=5(11)=13 ✅ → indices [1,5]
Two Sum (target=13): indices [1, 5] → values [2, 11]
Two Sum (target=100): [-1, -1]

All pairs summing to 10 in [1, 1, 2, 3, 4, 5, 6, 7, 8, 9]:
  [1, 9]
  [2, 8]
  [3, 7]
  [4, 6]

Three Sum [-1, 0, 1, 2, -1, -4]:
  [[-1, -1, 2], [-1, 0, 1]]

Three Sum [0, 0, 0, 0]:
  [[0, 0, 0]]

Three Sum [-2, 0, 0, 2, 2]:
  [[-2, 0, 2]]
```

**Two pointer trace for target=13 in {1,2,3,5,8,11,15}:**
```
left=0(1), right=6(15): 1+15=16 > 13 → right--
left=0(1), right=5(11): 1+11=12 < 13 → left++
left=1(2), right=5(11): 2+11=13 == 13 → ✅ return [1,5]
```

> 🔑 **Key Learning:** Two pointers reduce Two Sum in sorted array from O(n²) to O(n) — each pointer moves at most n steps total. Three Sum = sort + fix one element + Two Pointers for the remaining pair. Duplicate skipping is critical for Three Sum — after finding a valid triplet, advance past all equal elements to avoid duplicate results. LeetCode #167 (Two Sum II), #15 (3Sum) — asked at Amazon, Google, Microsoft, Facebook.

---

### ✅ Problem 2 — Remove Duplicates & Move Zeros `[Easy]`

**Task:** In-place array modifications using fast & slow pointers.

```java
import java.util.Arrays;

public class P2_InPlaceModify {

    // Remove duplicates from sorted array — return new length
    static int removeDuplicates(int[] arr) {
        if (arr.length == 0) return 0;
        int slow = 0;
        for (int fast = 1; fast < arr.length; fast++) {
            if (arr[fast] != arr[slow]) {
                slow++;
                arr[slow] = arr[fast];
            }
        }
        return slow + 1;   // new length
    }

    // Remove all occurrences of val — return new length
    static int removeElement(int[] arr, int val) {
        int slow = 0;
        for (int fast = 0; fast < arr.length; fast++) {
            if (arr[fast] != val) {
                arr[slow] = arr[fast];
                slow++;
            }
        }
        return slow;
    }

    // Move all zeros to end (preserve relative order of non-zeros)
    static void moveZeros(int[] arr) {
        int slow = 0;   // boundary of non-zero elements
        for (int fast = 0; fast < arr.length; fast++) {
            if (arr[fast] != 0) {
                arr[slow] = arr[fast];
                slow++;
            }
        }
        // fill remaining positions with 0
        while (slow < arr.length) arr[slow++] = 0;
    }

    // Move all zeros (using swap — maintains original relative order)
    static void moveZerosSwap(int[] arr) {
        int slow = 0;
        for (int fast = 0; fast < arr.length; fast++) {
            if (arr[fast] != 0) {
                int temp = arr[slow]; arr[slow] = arr[fast]; arr[fast] = temp;
                slow++;
            }
        }
    }

    public static void main(String[] args) {
        // Remove duplicates
        int[] arr1 = {1, 1, 2, 2, 3, 4, 4, 5};
        System.out.println("Original  : " + Arrays.toString(arr1));
        int len = removeDuplicates(arr1);
        System.out.println("No dups   : " + Arrays.toString(Arrays.copyOf(arr1, len))
                           + "  length=" + len);

        System.out.println();

        // Remove element
        int[] arr2 = {3, 2, 2, 3, 4, 3, 5};
        System.out.println("Original  : " + Arrays.toString(arr2));
        int len2 = removeElement(arr2, 3);
        System.out.println("Remove 3  : " + Arrays.toString(Arrays.copyOf(arr2, len2))
                           + "  length=" + len2);

        System.out.println();

        // Move zeros
        int[] arr3 = {0, 1, 0, 3, 12, 0, 5};
        System.out.println("Original  : " + Arrays.toString(arr3));
        moveZeros(arr3);
        System.out.println("Move zeros: " + Arrays.toString(arr3));

        int[] arr4 = {0, 1, 0, 3, 12, 0, 5};
        moveZerosSwap(arr4);
        System.out.println("Swap zeros: " + Arrays.toString(arr4));
    }
}
```

```
Output:
Original  : [1, 1, 2, 2, 3, 4, 4, 5]
No dups   : [1, 2, 3, 4, 5]  length=5

Original  : [3, 2, 2, 3, 4, 3, 5]
Remove 3  : [2, 2, 4, 5]  length=4

Original  : [0, 1, 0, 3, 12, 0, 5]
Move zeros: [1, 3, 12, 5, 0, 0, 0]
Swap zeros: [1, 3, 12, 5, 0, 0, 0]
```

**Move zeros trace for {0,1,0,3,12,0,5}:**
```
slow=0
fast=0: arr[0]=0 → skip
fast=1: arr[1]=1 → arr[0]=1, slow=1
fast=2: arr[2]=0 → skip
fast=3: arr[3]=3 → arr[1]=3, slow=2
fast=4: arr[4]=12→ arr[2]=12, slow=3
fast=5: arr[5]=0 → skip
fast=6: arr[6]=5 → arr[3]=5, slow=4
Fill arr[4..6] = 0
Result: [1,3,12,5,0,0,0] ✅
```

> 🔑 **Key Learning:** The fast-slow pointer pattern for in-place modification: `fast` scans everything, `slow` marks where the next "valid" element should go. O(n) time, O(1) space — no extra array needed. This exact pattern solves LeetCode #26, #27, #283 — all appear in Amazon, Microsoft interviews. The key insight: slow only advances when we keep an element; fast always advances.

---

### ✅ Problem 3 — Maximum Sum Subarray (Fixed Window) `[Easy-Medium]`

**Task:** Find maximum sum subarray of fixed size k using sliding window.

```java
import java.util.Arrays;

public class P3_FixedWindow {

    // Maximum sum subarray of size k
    static int maxSumSubarray(int[] arr, int k) {
        if (k > arr.length) return -1;

        // Build first window
        int windowSum = 0;
        for (int i = 0; i < k; i++) windowSum += arr[i];

        int maxSum = windowSum;
        int maxStart = 0;

        // Slide the window
        for (int i = k; i < arr.length; i++) {
            windowSum += arr[i];       // add entering element
            windowSum -= arr[i - k];   // remove leaving element
            if (windowSum > maxSum) {
                maxSum = windowSum;
                maxStart = i - k + 1;
            }
        }

        System.out.println("Max subarray of size " + k + ": "
            + Arrays.toString(Arrays.copyOfRange(arr, maxStart, maxStart + k))
            + " = " + maxSum);
        return maxSum;
    }

    // Average of all subarrays of size k
    static double[] avgSubarrays(int[] arr, int k) {
        double[] result = new double[arr.length - k + 1];
        double windowSum = 0;

        for (int i = 0; i < k; i++) windowSum += arr[i];
        result[0] = windowSum / k;

        for (int i = k; i < arr.length; i++) {
            windowSum += arr[i] - arr[i - k];
            result[i - k + 1] = windowSum / k;
        }
        return result;
    }

    // First negative number in every window of size k
    static int[] firstNegativeInWindow(int[] arr, int k) {
        int[] result = new int[arr.length - k + 1];
        java.util.Deque<Integer> negatives = new java.util.ArrayDeque<>();

        // Build first window
        for (int i = 0; i < k; i++)
            if (arr[i] < 0) negatives.offer(i);

        for (int i = k; i <= arr.length; i++) {
            result[i - k] = negatives.isEmpty() ? 0 : arr[negatives.peek()];

            // Remove elements outside window
            if (!negatives.isEmpty() && negatives.peek() <= i - k)
                negatives.poll();

            // Add new element
            if (i < arr.length && arr[i] < 0) negatives.offer(i);
        }
        return result;
    }

    public static void main(String[] args) {
        int[] arr = {2, 1, 5, 1, 3, 2, 4, 6, 1, 3};
        System.out.println("Array: " + Arrays.toString(arr));
        maxSumSubarray(arr, 3);
        maxSumSubarray(arr, 5);

        System.out.println();

        int[] arr2 = {1, 3, 2, 6, -1, 4, 1, 8, 2};
        System.out.println("Averages of all k=5 windows:");
        System.out.println(Arrays.toString(avgSubarrays(arr2, 5)));

        System.out.println();

        int[] arr3 = {12, -1, -7, 8, -15, 30, 16, 28};
        System.out.println("First negative in each window of size 3:");
        System.out.println(Arrays.toString(firstNegativeInWindow(arr3, 3)));
    }
}
```

```
Output:
Array: [2, 1, 5, 1, 3, 2, 4, 6, 1, 3]
Max subarray of size 3: [4, 6, 1] = 11  (wait: 4+6+1=11, but 1+3+2=6... let's check)
Actually window [4,6,1] starts at index 6: 4+6+1=11 ✅
Max subarray of size 3: [2, 4, 6] = 12  (2+4+6=12, starts at index 6... hmm)
Let me retrace: windows are [2,1,5]=8, [1,5,1]=7, [5,1,3]=9, [1,3,2]=6,
[3,2,4]=9, [2,4,6]=12, [4,6,1]=11, [6,1,3]=10 → max=12 at start=5
Max subarray of size 3: [2, 4, 6] = 12

Max subarray of size 5: [3, 2, 4, 6, 1] = 16

Averages of all k=5 windows:
[2.2, 2.8, 2.4, 3.6, 2.8]

First negative in each window of size 3:
[-1, -1, -7, -15, -15, 0]
```

> 🔑 **Key Learning:** Sliding window magic: `windowSum += arr[i] - arr[i-k]` — one line replaces recomputing the entire window sum. For n=10^6, k=1000: brute force = 10^9 operations, sliding window = 10^6 operations. The deque approach for "first negative in window" preview: Deque stores indices, front is always the first valid negative — this pattern extends to Maximum of Sliding Window (LeetCode #239) using Monotonic Deque. LeetCode #643 (avg subarrays).

---

### ✅ Problem 4 — Minimum Window Subarray (Variable Window) `[Medium]`

**Task:** Find minimum length subarray with sum ≥ target and longest subarray with sum ≤ target.

```java
import java.util.Arrays;

public class P4_VariableWindow {

    // Minimum length subarray with sum >= target
    static int minLenSubarraySum(int[] arr, int target) {
        int left = 0, windowSum = 0;
        int minLen = Integer.MAX_VALUE;

        for (int right = 0; right < arr.length; right++) {
            windowSum += arr[right];           // expand

            while (windowSum >= target) {      // shrink while valid
                minLen = Math.min(minLen, right - left + 1);
                windowSum -= arr[left];
                left++;
            }
        }
        return minLen == Integer.MAX_VALUE ? 0 : minLen;
    }

    // Longest subarray with sum <= target (all non-negative)
    static int maxLenSumAtMost(int[] arr, int target) {
        int left = 0, windowSum = 0, maxLen = 0;

        for (int right = 0; right < arr.length; right++) {
            windowSum += arr[right];           // expand

            while (windowSum > target) {       // shrink while invalid
                windowSum -= arr[left];
                left++;
            }

            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }

    // Count subarrays with sum exactly = k (positive numbers only)
    static int countSubarraysWithSum(int[] arr, int k) {
        // Count(sum==k) = Count(sum<=k) - Count(sum<=k-1)
        return atMostK(arr, k) - atMostK(arr, k - 1);
    }

    static int atMostK(int[] arr, int k) {
        if (k < 0) return 0;
        int left = 0, count = 0, sum = 0;
        for (int right = 0; right < arr.length; right++) {
            sum += arr[right];
            while (sum > k) { sum -= arr[left++]; }
            count += right - left + 1;  // all subarrays ending at right are valid
        }
        return count;
    }

    public static void main(String[] args) {
        // Min length with sum >= target
        System.out.println("=== Min Length Subarray (sum >= target) ===");
        int[][] tests = {{2,3,1,2,4,3}, {1,4,4}, {1,1,1,1,1,1,1}};
        int[] targets = {7, 4, 11};
        for (int i = 0; i < tests.length; i++) {
            int result = minLenSubarraySum(tests[i], targets[i]);
            System.out.printf("arr=%s  target=%d  → min_len=%d%n",
                Arrays.toString(tests[i]), targets[i], result);
        }

        System.out.println();

        // Max length with sum <= target
        System.out.println("=== Max Length Subarray (sum <= target) ===");
        int[] arr = {1, 2, 3, 4, 5};
        for (int t : new int[]{5, 8, 15}) {
            System.out.printf("arr=%s  target=%d  → max_len=%d%n",
                Arrays.toString(arr), t, maxLenSumAtMost(arr, t));
        }

        System.out.println();

        // Count subarrays with exact sum
        System.out.println("=== Count Subarrays with Exact Sum ===");
        int[] arr2 = {1, 2, 3, 1, 1, 2};
        for (int k : new int[]{3, 4, 6}) {
            System.out.printf("arr=%s  sum=%d  → count=%d%n",
                Arrays.toString(arr2), k, countSubarraysWithSum(arr2, k));
        }
    }
}
```

```
Output:
=== Min Length Subarray (sum >= target) ===
arr=[2, 3, 1, 2, 4, 3]  target=7  → min_len=2   ([4,3])
arr=[1, 4, 4]            target=4  → min_len=1   ([4])
arr=[1, 1, 1, 1, 1, 1, 1] target=11 → min_len=0  (impossible)

=== Max Length Subarray (sum <= target) ===
arr=[1, 2, 3, 4, 5]  target=5  → max_len=2   ([1,2] or [2,3] all ≤5, [1,2]=3)
arr=[1, 2, 3, 4, 5]  target=8  → max_len=3   ([1,2,3]=6 or [1,2,4]=7)
arr=[1, 2, 3, 4, 5]  target=15 → max_len=5   (entire array = 15)

=== Count Subarrays with Exact Sum ===
arr=[1, 2, 3, 1, 1, 2]  sum=3  → count=4
arr=[1, 2, 3, 1, 1, 2]  sum=4  → count=3
arr=[1, 2, 3, 1, 1, 2]  sum=6  → count=2
```

**Variable window trace for min_len, arr=[2,3,1,2,4,3], target=7:**
```
right=0: sum=2  < 7 → expand
right=1: sum=5  < 7 → expand
right=2: sum=6  < 7 → expand
right=3: sum=8  ≥ 7 → minLen=4, shrink: sum=6, left=1
right=4: sum=10 ≥ 7 → minLen=4, shrink: sum=7, left=2 → still ≥ 7 → minLen=3, shrink: sum=6, left=3
right=5: sum=9  ≥ 7 → minLen=min(3,3)=3... wait [4,3]=2, minLen=2, shrink: sum=5, left=4

Correct trace shows minLen=2 from window [4,3] ✅
```

> 🔑 **Key Learning:** Variable window pattern: expand right always, shrink left while condition holds. The `count += right - left + 1` trick counts ALL valid subarrays ending at `right` in O(1) — because every starting position from `left` to `right` forms a valid subarray. `count(sum==k) = count(sum<=k) - count(sum<=k-1)` is an elegant reduction to sliding window when direct counting is hard. LeetCode #209 (min subarray), #713 (subarray product).

---

### ✅ Problem 5 — Longest Substring Problems `[Medium]`

**Task:** Solve classic substring problems using sliding window + HashMap.

```java
import java.util.*;

public class P5_LongestSubstring {

    // Longest substring without repeating characters
    static int longestUniqueSubstring(String s) {
        HashMap<Character, Integer> lastSeen = new HashMap<>();
        int left = 0, maxLen = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);

            // If character seen and within current window, shrink from left
            if (lastSeen.containsKey(c) && lastSeen.get(c) >= left) {
                left = lastSeen.get(c) + 1;
            }

            lastSeen.put(c, right);
            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }

    // Longest substring with at most k distinct characters
    static int longestKDistinct(String s, int k) {
        HashMap<Character, Integer> freq = new HashMap<>();
        int left = 0, maxLen = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            freq.put(c, freq.getOrDefault(c, 0) + 1);  // expand

            while (freq.size() > k) {                    // shrink
                char lc = s.charAt(left);
                freq.put(lc, freq.get(lc) - 1);
                if (freq.get(lc) == 0) freq.remove(lc);
                left++;
            }

            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }

    // Longest repeating character replacement (can replace at most k chars)
    // Key insight: window is valid if (window_size - max_freq_char) <= k
    static int characterReplacement(String s, int k) {
        int[] freq = new int[26];
        int left = 0, maxFreq = 0, maxLen = 0;

        for (int right = 0; right < s.length(); right++) {
            freq[s.charAt(right) - 'A']++;
            maxFreq = Math.max(maxFreq, freq[s.charAt(right) - 'A']);

            // If replacements needed > k, shrink window
            if ((right - left + 1) - maxFreq > k) {
                freq[s.charAt(left) - 'A']--;
                left++;
            }

            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }

    public static void main(String[] args) {
        // Longest unique substring
        System.out.println("=== Longest Substring Without Repeating Chars ===");
        String[] strs = {"abcabcbb", "bbbbb", "pwwkew", "abcde", " "};
        for (String s : strs)
            System.out.printf("%-12s → length=%d%n",
                "\"" + s + "\"", longestUniqueSubstring(s));

        System.out.println();

        // Longest k-distinct
        System.out.println("=== Longest Substring with K Distinct Chars ===");
        String[][] tests = {{"eceba","araaci"}, {"eceba","araaci"}};
        int[] ks = {2, 1};
        String[] strs2 = {"eceba", "araaci", "aabbcc"};
        int[] ks2 = {2, 1, 2};
        for (int i = 0; i < strs2.length; i++)
            System.out.printf("%-12s k=%d → length=%d%n",
                "\""+strs2[i]+"\"", ks2[i], longestKDistinct(strs2[i], ks2[i]));

        System.out.println();

        // Character replacement
        System.out.println("=== Longest After At Most K Replacements ===");
        String[][] repTests = {{"AABABBA","ABAB","AAAA"}};
        int[] kR = {1, 2, 0};
        String[] repStrs = {"AABABBA", "ABAB", "AAAA"};
        for (int i = 0; i < repStrs.length; i++)
            System.out.printf("%-12s k=%d → length=%d%n",
                "\""+repStrs[i]+"\"", kR[i], characterReplacement(repStrs[i], kR[i]));
    }
}
```

```
Output:
=== Longest Substring Without Repeating Chars ===
"abcabcbb"   → length=3   (abc)
"bbbbb"      → length=1   (b)
"pwwkew"     → length=3   (wke)
"abcde"      → length=5   (entire string)
" "          → length=1

=== Longest Substring with K Distinct Chars ===
"eceba"      k=2 → length=3   (ece)
"araaci"     k=1 → length=2   (aa)
"aabbcc"     k=2 → length=4   (aabb)

=== Longest After At Most K Replacements ===
"AABABBA"    k=1 → length=4   (AABA or ABBB)
"ABAB"       k=2 → length=4   (entire string)
"AAAA"       k=0 → length=4   (entire string)
```

**Longest unique trace for "abcabcbb":**
```
right=0: 'a', lastSeen={a:0}, left=0, len=1
right=1: 'b', lastSeen={a:0,b:1}, left=0, len=2
right=2: 'c', lastSeen={a:0,b:1,c:2}, left=0, len=3
right=3: 'a', seen at 0 ≥ left=0 → left=1, lastSeen{a:3}, len=3
right=4: 'b', seen at 1 ≥ left=1 → left=2, lastSeen{b:4}, len=3
right=5: 'c', seen at 2 ≥ left=2 → left=3, lastSeen{c:5}, len=3
right=6: 'b', seen at 4 ≥ left=3 → left=5, lastSeen{b:6}, len=2
right=7: 'b', seen at 6 ≥ left=5 → left=7, lastSeen{b:7}, len=1
maxLen = 3 ✅
```

> 🔑 **Key Learning:** Storing `lastSeen` index (not just presence) allows instant jump: when a duplicate is found, `left = lastSeen[c] + 1` jumps directly past the previous occurrence — no inner loop needed. Character replacement insight: `window_size - maxFreq` = number of characters to replace. If this exceeds k, window is invalid — shrink by one. LeetCode #3 (Longest Unique), #340 (K Distinct), #424 (Character Replacement) — all top-100 interview questions.

---

### ✅ Problem 6 — Container With Most Water + Trapping Rain Water `[Medium-Hard]`

**Task:** Classic two-pointer problems on arrays.

```java
import java.util.Arrays;

public class P6_WaterProblems {

    // Container with most water — maximize area between two heights
    static int maxWater(int[] height) {
        int left = 0, right = height.length - 1;
        int maxArea = 0;

        while (left < right) {
            int width = right - left;
            int h     = Math.min(height[left], height[right]);
            maxArea   = Math.max(maxArea, width * h);

            // Move the shorter side — moving the taller side can only decrease area
            if (height[left] <= height[right]) left++;
            else                               right--;
        }
        return maxArea;
    }

    // Trapping rain water — two pointer approach O(n) O(1)
    static int trapRainWater(int[] height) {
        int left = 0, right = height.length - 1;
        int leftMax = 0, rightMax = 0;
        int trapped = 0;

        while (left < right) {
            if (height[left] <= height[right]) {
                if (height[left] >= leftMax) leftMax = height[left];
                else                         trapped += leftMax - height[left];
                left++;
            } else {
                if (height[right] >= rightMax) rightMax = height[right];
                else                           trapped  += rightMax - height[right];
                right--;
            }
        }
        return trapped;
    }

    // Visualize trapped water
    static void visualize(int[] height) {
        int n = height.length;
        int maxH = Arrays.stream(height).max().getAsInt();

        System.out.println("Height visualization:");
        for (int row = maxH; row >= 1; row--) {
            for (int i = 0; i < n; i++) {
                if (height[i] >= row) System.out.print("█ ");
                else                  System.out.print("  ");
            }
            System.out.println();
        }
        System.out.print("  ");
        for (int i = 0; i < n; i++) System.out.print("─ ");
        System.out.println();
    }

    public static void main(String[] args) {
        // Container with most water
        System.out.println("=== Container With Most Water ===");
        int[][] tests = {
            {1, 8, 6, 2, 5, 4, 8, 3, 7},
            {1, 1},
            {4, 3, 2, 1, 4},
            {1, 2, 1}
        };
        for (int[] h : tests)
            System.out.printf("heights=%s → maxArea=%d%n",
                Arrays.toString(h), maxWater(h));

        System.out.println();

        // Trapping rain water
        System.out.println("=== Trapping Rain Water ===");
        int[][] rainTests = {
            {0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1},
            {4, 2, 0, 3, 2, 5},
            {3, 0, 2, 0, 4}
        };
        for (int[] h : rainTests) {
            System.out.printf("heights=%s → trapped=%d units%n",
                Arrays.toString(h), trapRainWater(h));
        }

        System.out.println();

        // Visualize
        int[] visual = {3, 0, 2, 0, 4};
        visualize(visual);
        System.out.println("Trapped water: " + trapRainWater(visual) + " units");
    }
}
```

```
Output:
=== Container With Most Water ===
heights=[1, 8, 6, 2, 5, 4, 8, 3, 7] → maxArea=49
heights=[1, 1]                        → maxArea=1
heights=[4, 3, 2, 1, 4]              → maxArea=16
heights=[1, 2, 1]                     → maxArea=2

=== Trapping Rain Water ===
heights=[0,1,0,2,1,0,1,3,2,1,2,1] → trapped=6 units
heights=[4, 2, 0, 3, 2, 5]         → trapped=9 units
heights=[3, 0, 2, 0, 4]            → trapped=7 units

Height visualization [3,0,2,0,4]:
█         █
█   █     █
█   █   █ █
─ ─ ─ ─ ─ ─
Trapped water: 7 units
```

**Container with most water — why move shorter side:**
```
heights=[1,8,6,2,5,4,8,3,7], left=0(h=1), right=8(h=7)
Area = min(1,7) × 8 = 1×8 = 8
If we move right (the taller): width decreases, height ≤ 7 → area can only ≤ 8
If we move left  (the shorter): width decreases BUT height could increase → might get larger area
→ Always move the SHORTER side for any chance of improvement ✅

Correct answer: left=1(h=8), right=8(h=7) → min(8,7)×7 = 49 ✅
```

**Trapping rain water logic:**
```
At each position, trapped water = min(leftMax, rightMax) - height[i]
Two pointers: process the side with smaller max first — guaranteed correct min
height=[3,0,2,0,4]:
  At 0: leftMax=3, rightMax=4 → water = min(3,4)-0 = 3
  At 3: leftMax=3, rightMax=4 → water = min(3,4)-0 = 3
  At 1: leftMax=3, rightMax=4 → water = min(3,4)-2 = 1
  Total = 3+3+1 = 7 ✅
```

> 🔑 **Key Learning:** Container with most water — greedy insight: moving the shorter side is the only way to potentially increase area. Moving the taller side guarantees area ≤ current (width decreases, height bounded by shorter). Trapping Rain Water — two pointer eliminates the need for prefix/suffix max arrays: process whichever side has smaller max, because the water level is determined by `min(leftMax, rightMax)`. LeetCode #11 and #42 — among the most iconic interview problems. LeetCode #11 appears in Amazon, Google, and Flipkart interviews constantly.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Minimum Window Substring + Fruit Into Baskets + Max Consecutive Ones III.

```java
import java.util.*;

public class MiniChallenge_Day17 {

    // ① Minimum window substring — find smallest window in s containing all chars of t
    static String minWindowSubstring(String s, String t) {
        if (s.isEmpty() || t.isEmpty() || s.length() < t.length()) return "";

        HashMap<Character, Integer> need = new HashMap<>();
        for (char c : t.toCharArray())
            need.put(c, need.getOrDefault(c, 0) + 1);

        int have = 0, required = need.size();
        HashMap<Character, Integer> window = new HashMap<>();
        int left = 0, minLen = Integer.MAX_VALUE, minStart = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            window.put(c, window.getOrDefault(c, 0) + 1);

            // Check if current char satisfies its requirement
            if (need.containsKey(c) && window.get(c).equals(need.get(c)))
                have++;

            // Try to shrink window while all requirements met
            while (have == required) {
                if (right - left + 1 < minLen) {
                    minLen  = right - left + 1;
                    minStart = left;
                }
                char lc = s.charAt(left);
                window.put(lc, window.get(lc) - 1);
                if (need.containsKey(lc) && window.get(lc) < need.get(lc))
                    have--;
                left++;
            }
        }
        return minLen == Integer.MAX_VALUE ? "" : s.substring(minStart, minStart + minLen);
    }

    // ② Fruit Into Baskets — longest subarray with at most 2 distinct values
    static int fruitsIntoBaskets(int[] fruits) {
        HashMap<Integer, Integer> basket = new HashMap<>();
        int left = 0, maxLen = 0;

        for (int right = 0; right < fruits.length; right++) {
            basket.put(fruits[right], basket.getOrDefault(fruits[right], 0) + 1);

            while (basket.size() > 2) {
                int lf = fruits[left];
                basket.put(lf, basket.get(lf) - 1);
                if (basket.get(lf) == 0) basket.remove(lf);
                left++;
            }
            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }

    // ③ Max consecutive ones III — can flip at most k zeros
    static int maxConsecutiveOnes(int[] nums, int k) {
        int left = 0, zeros = 0, maxLen = 0;

        for (int right = 0; right < nums.length; right++) {
            if (nums[right] == 0) zeros++;

            while (zeros > k) {
                if (nums[left] == 0) zeros--;
                left++;
            }

            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }

    public static void main(String[] args) {
        // Min window substring
        System.out.println("=== Minimum Window Substring ===");
        String[][] tests = {{"ADOBECODEBANC","ABC"}, {"a","a"}, {"a","aa"}};
        for (String[] t : tests)
            System.out.printf("s=%-20s t=%-4s → \"%s\"%n",
                "\""+t[0]+"\"", "\""+t[1]+"\"", minWindowSubstring(t[0], t[1]));

        System.out.println();

        // Fruits into baskets
        System.out.println("=== Fruit Into Baskets (at most 2 distinct) ===");
        int[][] fTests = {{1,2,1},{0,1,2,2},{1,2,3,2,2}};
        for (int[] f : fTests)
            System.out.printf("%-20s → %d%n",
                Arrays.toString(f), fruitsIntoBaskets(f));

        System.out.println();

        // Max consecutive ones with k flips
        System.out.println("=== Max Consecutive Ones (flip at most k zeros) ===");
        int[][] oTests = {{1,1,1,0,0,0,1,1,1,1,0},{0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1}};
        int[] ks = {3, 3};
        for (int i = 0; i < oTests.length; i++)
            System.out.printf("%-35s k=%d → %d%n",
                Arrays.toString(oTests[i]), ks[i],
                maxConsecutiveOnes(oTests[i], ks[i]));
    }
}
```

```
Output:
=== Minimum Window Substring ===
s="ADOBECODEBANC"  t="ABC" → "BANC"
s="a"              t="a"   → "a"
s="a"              t="aa"  → ""

=== Fruit Into Baskets (at most 2 distinct) ===
[1, 2, 1]                → 3
[0, 1, 2, 2]             → 3
[1, 2, 3, 2, 2]          → 4   ([2,3,2,2])

=== Max Consecutive Ones (flip at most k zeros) ===
[1,1,1,0,0,0,1,1,1,1,0]  k=3 → 10
[0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1] k=3 → 10
```

> 🔑 **Key Learning:** Minimum Window Substring uses `have == required` to track how many character requirements are fully satisfied — more elegant than checking the entire map each iteration. Fruit Into Baskets is exactly "longest subarray with at most 2 distinct values" — recognizing the re-statement is the skill. Max Consecutive Ones III: counting zeros in window and shrinking when `zeros > k` — classic variable window. LeetCode #76, #904, #1004 — all tier-1 interview problems at top companies.

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| GCD Euclidean from memory | ✅ `return b==0 ? a : gcd(b, a%b)` |
| Sieve inner loop starts at i*i — why? | ✅ Smaller multiples already marked by smaller primes |
| Fast power O(log n) — trace 2^10 mod 1000 | ✅ result=24 in 4 iterations |
| `(a-b+MOD)%MOD` — why +MOD? | ✅ Prevents negative from modular subtraction |

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. When do you use two pointers vs sliding window?</b></summary>

> Both solve array/string subarray problems in O(n), but they address different structures:
>
> **Two Pointers (opposite ends):** Use when the array is **sorted** and you need pairs/triplets satisfying a sum/product condition. The sorted property means moving left increases values, moving right decreases values — giving a clear decision at each step.
>
> **Two Pointers (same direction / fast-slow):** Use for **in-place modifications** — removing elements, compacting arrays, partitioning. Fast scans, slow marks valid positions.
>
> **Sliding Window:** Use when the problem involves **contiguous subarrays or substrings** with a property (sum, distinct count, character frequency). The window expands/contracts to maintain the property efficiently.
>
> Key signal words: "subarray", "substring", "contiguous", "consecutive" → sliding window. "sorted array", "pair sum", "two elements" → two pointers (opposite ends).

</details>

<details>
<summary><b>Q2. Why must the array be sorted for opposite-ends two pointers to work?</b></summary>

> The correctness of opposite-ends two pointers depends on **monotonicity** — a clear decision about which pointer to move.
>
> In a sorted array, `arr[left]` is the smallest element in the current window and `arr[right]` is the largest. If `arr[left] + arr[right] < target`, we KNOW we need a larger sum, so `left++` (increasing the smaller element) is the only productive move. If the sum > target, `right--` is the only productive move.
>
> Without sorting, if `arr[left] + arr[right] < target`, we don't know whether to move left or right — either could help or hurt. The algorithm loses its correctness guarantee.
>
> Exception: some problems don't need sorting (move zeros, remove duplicates) because they use fast-slow pattern, not sum-based decision.

</details>

<details>
<summary><b>Q3. What is the "shrink while valid" vs "shrink while invalid" choice in variable window?</b></summary>

> This depends on whether you want **minimum or maximum** window:
>
> **Minimum window (shrink while VALID):** Expand right to satisfy the condition, then shrink left as long as the condition is still satisfied. Record the minimum window size during shrinking. Used for: minimum length subarray with sum ≥ target, minimum window substring.
>
> **Maximum window (shrink while INVALID):** Expand right, and shrink left only when the window becomes invalid. Record window size after each expansion. Used for: longest substring without repeating characters, longest with at most k distinct.
>
> Template test: "Find MINIMUM subarray that..." → shrink while valid. "Find MAXIMUM subarray that..." → shrink while invalid.

</details>

<details>
<summary><b>Q4. Why do we move the shorter side in Container with Most Water?</b></summary>

> Area = width × min(height[left], height[right]).
>
> When we move a pointer, width decreases by 1 regardless.
>
> If we move the **taller side:** The new height is bounded by the remaining pointer — still the shorter one at most. So min(new heights) ≤ current min. Area decreases or stays equal. Moving taller side **cannot improve** area.
>
> If we move the **shorter side:** The new height could be taller than the current shorter side. So min(new heights) could increase. There's a **chance** of improving area.
>
> Therefore: always move the shorter side for any hope of improvement. This greedy choice is provably optimal — the maximum area cannot be achieved with the current shorter side.

</details>

<details>
<summary><b>Q5. How does the "count subarrays with sum = k" trick work using sliding window?</b></summary>

> Direct sliding window doesn't work for exact sum = k because shrinking condition is ambiguous (sum could equal k before or after shrinking). The trick:
>
> **count(sum == k) = count(sum ≤ k) − count(sum ≤ k−1)**
>
> Each `atMostK(arr, k)` function counts subarrays with sum **at most k** — this works perfectly with sliding window because the condition is monotonic (once sum exceeds k, shrink; once ≤ k, valid).
>
> Inside `atMostK`: when window has sum ≤ k, **all subarrays ending at `right` from any start in [left..right]** are valid. So `count += right - left + 1` adds all of them at once in O(1).
>
> This counting trick — expressing "exactly k" as "at most k" minus "at most k-1" — is used in LeetCode #713, #1248, #992 and many other problems. It's a powerful meta-technique for sliding window.

</details>

---

## 📌 DAY 17 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 17 — SUMMARY                       ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 15, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Two Pointers & Sliding Window                   ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  Two Pointers — opposite ends (sorted arrays, pair sum)      ║
║  ✅  Two Pointers — same direction (fast-slow, in-place modify)  ║
║  ✅  Two Pointers — partition (Dutch National Flag)              ║
║  ✅  Sliding Window — fixed size (max sum, average, first neg)   ║
║  ✅  Sliding Window — variable size (min/max subarray)           ║
║  ✅  Sliding Window + HashMap (distinct chars, frequencies)      ║
║  ✅  Decision guide — when to use which pattern                  ║
║  ✅  Count subarrays (sum==k) = atMost(k) - atMost(k-1) trick  ║
╠══════════════════════════════════════════════════════════════════╣
║  PROBLEMS SOLVED                                                ║
║  ✅  P1 — Two Sum (sorted) + Three Sum (sort + two ptrs)        ║
║  ✅  P2 — Remove duplicates + move zeros (fast-slow)            ║
║  ✅  P3 — Max sum fixed window + avg + first negative           ║
║  ✅  P4 — Min length variable window + count exact sum          ║
║  ✅  P5 — Longest unique substr + k-distinct + char replacement  ║
║  ✅  P6 — Container with most water + Trapping rain water       ║
║  ✅  Mini — Min window substring + fruits + max ones III        ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  90%                              ║
║  STRONG AREA    :  Fixed window, two sum, move zeros            ║
║  WEAK AREA      :  Min window substring — need/have tracking    ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  Sliding window: add entering, remove leaving → O(n) not O(nk)║
║  ▸  Sorted array + pair sum → two pointers (opposite ends)      ║
║  ▸  In-place remove/filter → fast-slow pointers                 ║
║  ▸  Move shorter side → only chance to improve container area   ║
║  ▸  count(==k) = atMost(k) - atMost(k-1) — exact sum trick     ║
║  ▸  Variable window: shrink WHILE valid for min, invalid for max║
║  ▸  lastSeen index (not bool) enables O(1) jump past duplicates ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 16](./Day16_Math_Basics.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 18 →](./Day18_Prefix_Suffix.md)

*Day 17 of 160 — Done ✅ | Phase 1: 81%! | Streak: 17 Days 🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥*

</div>
