# 📄 Day 07 — 2D Arrays & Matrix Operations 

<div align="center">

![Day](https://img.shields.io/badge/Day-07_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-2D_Arrays_%26_Matrix_Operations-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 06](./Day06_Arrays.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 08 →](./Day08_Strings.md)

</div>

---

## 🎯 Day 7 Goal
> Master 2D arrays and matrix operations — declaration, traversal, diagonal access, transpose, rotation, spiral order, and set zeroes. 2D arrays are the bridge to **graphs, DP tables, and grid problems** — three of the heaviest DSA topics in placements.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | 2D array declaration, memory model, initialization | 55 min |
| Block 2 | Row/column traversal, diagonal access, border elements | 50 min |
| Block 3 | Revision — Day 6 array operations flash review | 15 min |
| Block 4 | Practice Set — 6 Problems | 120 min |
| Block 5 | Mini Challenge + Revision Tasks | 40 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. What is a 2D Array?

A **2D array is an array of arrays** — a table with rows and columns.

```java
int[][] matrix = {
    {1, 2, 3},    // row 0
    {4, 5, 6},    // row 1
    {7, 8, 9}     // row 2
};
//   col0 col1 col2

// Access: matrix[row][col]
// matrix[0][0]=1  matrix[0][1]=2  matrix[0][2]=3
// matrix[1][0]=4  matrix[1][1]=5  matrix[1][2]=6
// matrix[2][0]=7  matrix[2][1]=8  matrix[2][2]=9
```

**Memory model:**
```
Stack:              Heap:
matrix ──────────► [ref0][ref1][ref2]
                     │     │     │
                     ▼     ▼     ▼
                   [1,2,3][4,5,6][7,8,9]
```

> 💡 Each row is an **independent 1D array on the heap**. This is why Java supports jagged arrays (rows of different lengths). `matrix.length` = number of rows. `matrix[0].length` = number of columns in row 0.

---

### 🔷 2. Declaration & Initialization

```java
// Method 1 — Inline values
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Method 2 — Fixed size (all zeros)
int[][] matrix = new int[3][4];   // 3 rows, 4 cols
matrix[1][2] = 99;

// Method 3 — Using loops (most common)
int rows = 3, cols = 3;
int[][] matrix = new int[rows][cols];
int val = 1;
for (int i = 0; i < rows; i++)
    for (int j = 0; j < cols; j++)
        matrix[i][j] = val++;
// Result: [[1,2,3],[4,5,6],[7,8,9]]
```

**Key size properties:**
```java
matrix.length        // number of ROWS
matrix[0].length     // number of COLUMNS in row 0
```

---

### 🔷 3. Traversal Patterns

```java
// A. Row-by-row (most common)
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}

// B. Column-by-column (swap loop order)
for (int j = 0; j < matrix[0].length; j++) {
    for (int i = 0; i < matrix.length; i++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}

// C. Enhanced for-each (read-only, clean)
for (int[] row : matrix)
    for (int val : row)
        System.out.print(val + " ");
```

---

### 🔷 4. Diagonal Access

```java
int n = 3;
int[][] m = {{1,2,3},{4,5,6},{7,8,9}};

// Main diagonal (top-left → bottom-right): i == j
for (int i = 0; i < n; i++)
    System.out.print(m[i][i] + " ");         // 1 5 9

// Anti-diagonal (top-right → bottom-left): i + j == n-1
for (int i = 0; i < n; i++)
    System.out.print(m[i][n-1-i] + " ");     // 3 5 7

// Above main diagonal: j > i
// Below main diagonal: j < i
```

```
Visualization (n=3):
Position  (i,j)    Condition
[1]  [2]  [3]       j>i → above diagonal
[4]  [5]  [6]       i==j → main diagonal: 1,5,9
[7]  [8]  [9]       j<i → below diagonal
```

---

### 🔷 5. Border Elements

```java
// Border = first row OR last row OR first col OR last col
if (i == 0 || i == rows-1 || j == 0 || j == cols-1) {
    // border element
}
```

Same condition as hollow rectangle from Day 4 — patterns connect!

---

### 🔷 6. Essential Matrix Operations

```java
// ① Row sums
for (int i = 0; i < rows; i++) {
    int sum = 0;
    for (int j = 0; j < cols; j++) sum += matrix[i][j];
    System.out.println("Row " + i + " sum: " + sum);
}

// ② Column sums
for (int j = 0; j < cols; j++) {
    int sum = 0;
    for (int i = 0; i < rows; i++) sum += matrix[i][j];
    System.out.println("Col " + j + " sum: " + sum);
}

// ③ Transpose (square matrix, in-place)
// j starts at i+1 — avoid double-swapping back to original
for (int i = 0; i < n; i++)
    for (int j = i + 1; j < n; j++) {
        int temp    = matrix[i][j];
        matrix[i][j] = matrix[j][i];
        matrix[j][i] = temp;
    }

// ④ Search in 2D array
static boolean search(int[][] matrix, int target) {
    for (int[] row : matrix)
        for (int val : row)
            if (val == target) return true;
    return false;
}

// ⑤ Print matrix (formatted)
static void printMatrix(int[][] matrix) {
    for (int[] row : matrix) {
        for (int val : row)
            System.out.printf("%4d", val);
        System.out.println();
    }
}
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Matrix Input + Row/Column Sums `[Easy]`

**Task:** Create a 3×4 matrix and compute row sums, column sums, and diagonal sum.

```java
public class P1_MatrixSums {

    static void printMatrix(int[][] m) {
        for (int[] row : m) {
            for (int val : row) System.out.printf("%4d", val);
            System.out.println();
        }
    }

    static void rowSums(int[][] m) {
        System.out.println("Row sums:");
        for (int i = 0; i < m.length; i++) {
            int sum = 0;
            for (int j = 0; j < m[i].length; j++) sum += m[i][j];
            System.out.println("  Row " + i + ": " + sum);
        }
    }

    static void colSums(int[][] m) {
        System.out.println("Column sums:");
        for (int j = 0; j < m[0].length; j++) {
            int sum = 0;
            for (int i = 0; i < m.length; i++) sum += m[i][j];
            System.out.println("  Col " + j + ": " + sum);
        }
    }

    static void diagSum(int[][] m) {
        // Only for square matrices
        if (m.length != m[0].length) {
            System.out.println("Diagonal: N/A (not square)");
            return;
        }
        int sum = 0;
        for (int i = 0; i < m.length; i++) sum += m[i][i];
        System.out.println("Main diagonal sum: " + sum);
    }

    public static void main(String[] args) {
        int[][] matrix = {
            { 1,  2,  3,  4},
            { 5,  6,  7,  8},
            { 9, 10, 11, 12}
        };

        System.out.println("Matrix:");
        printMatrix(matrix);
        System.out.println();
        rowSums(matrix);
        System.out.println();
        colSums(matrix);
        System.out.println();
        diagSum(matrix);  // non-square
    }
}
```

```
Output:
Matrix:
   1   2   3   4
   5   6   7   8
   9  10  11  12

Row sums:
  Row 0: 10
  Row 1: 26
  Row 2: 42

Column sums:
  Col 0: 15
  Col 1: 18
  Col 2: 21
  Col 3: 24

Diagonal: N/A (not square)
```

> 🔑 **Key Learning:** Row sum → outer loop = rows, inner = cols. Column sum → **swap the loop order**: outer = cols, inner = rows. This swap is a fundamental 2D array technique — it appears in matrix transpose, column-wise operations, and cache optimization problems. The `diagSum` guard check (`m.length != m[0].length`) is defensive programming — always check preconditions before operating.

---

### ✅ Problem 2 — Search in 2D Array `[Easy]`

**Task:** Return `{row, col}` of target, or `{-1, -1}` if not found.

```java
public class P2_MatrixSearch {

    static void printMatrix(int[][] m) {
        for (int[] row : m) {
            for (int val : row) System.out.printf("%4d", val);
            System.out.println();
        }
    }

    static int[] search(int[][] matrix, int target) {
        for (int i = 0; i < matrix.length; i++)
            for (int j = 0; j < matrix[i].length; j++)
                if (matrix[i][j] == target)
                    return new int[]{i, j};    // early exit with position
        return new int[]{-1, -1};              // not found sentinel
    }

    static void searchAndReport(int[][] m, int target) {
        int[] result = search(m, target);
        if (result[0] == -1)
            System.out.println("Search " + target + ": NOT FOUND");
        else
            System.out.println("Search " + target +
                               ": found at [" + result[0] + "][" + result[1] + "]");
    }

    public static void main(String[] args) {
        int[][] matrix = {
            { 1,  2,  3},
            { 4,  5,  6},
            { 7,  8,  9},
            {10, 11, 12}
        };

        System.out.println("Matrix:");
        printMatrix(matrix);
        System.out.println();

        searchAndReport(matrix, 7);    // found
        searchAndReport(matrix, 10);   // found at last row
        searchAndReport(matrix, 1);    // found at [0][0]
        searchAndReport(matrix, 99);   // not found
    }
}
```

```
Output:
Matrix:
   1   2   3
   4   5   6
   7   8   9
  10  11  12

Search 7 : found at [2][0]
Search 10: found at [3][0]
Search 1 : found at [0][0]
Search 99: NOT FOUND
```

> 🔑 **Key Learning:** `return new int[]{i, j}` inside the nested loop — this double-breaks out of BOTH loops immediately, which `break` alone cannot do in a nested loop (it only exits one level). Returning from inside a nested loop is a clean early-exit strategy. The return type `int[]` carries two values elegantly. This pattern is used in 2D grid BFS/DFS in DSA — finding a starting position before beginning traversal.

---

### ✅ Problem 3 — Transpose of a Matrix `[Easy-Medium]`

**Task:** Return the transpose of both square and non-square matrices.

```java
public class P3_Transpose {

    static void printMatrix(int[][] m) {
        for (int[] row : m) {
            for (int val : row) System.out.printf("%4d", val);
            System.out.println();
        }
    }

    // Returns a NEW matrix (for non-square, size changes)
    static int[][] transpose(int[][] matrix) {
        int rows = matrix.length;
        int cols = matrix[0].length;
        // Transpose dimensions FLIP: rows×cols → cols×rows
        int[][] result = new int[cols][rows];

        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                result[j][i] = matrix[i][j];   // rule: result[j][i] = matrix[i][j]

        return result;
    }

    public static void main(String[] args) {
        // Square matrix (3×3)
        int[][] square = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        System.out.println("Square Original (3×3):"); printMatrix(square);
        System.out.println("Transposed (3×3):");      printMatrix(transpose(square));

        System.out.println();

        // Non-square matrix (2×3)
        int[][] rect = {
            {1, 2, 3},
            {4, 5, 6}
        };
        System.out.println("Rectangle Original (2×3):"); printMatrix(rect);
        System.out.println("Transposed (3×2):");          printMatrix(transpose(rect));
    }
}
```

```
Output:
Square Original (3×3):
   1   2   3
   4   5   6
   7   8   9
Transposed (3×3):
   1   4   7
   2   5   8
   3   6   9

Rectangle Original (2×3):
   1   2   3
   4   5   6
Transposed (3×2):
   1   4
   2   5
   3   6
```

**Dimension analysis:**
```
Original : 2 rows × 3 cols
Transpose: 3 rows × 2 cols  (dimensions FLIP)
Rule     : result[j][i] = matrix[i][j]
```

> 🔑 **Key Learning:** For non-square matrices, the **dimensions flip** — a 2×3 becomes a 3×2. The transpose rule `result[j][i] = matrix[i][j]` swaps row and column indices. Using a NEW result matrix is necessary for non-square (in-place only works for square matrices). This operation is fundamental in linear algebra, image processing, and appears in many DP state transition problems.

---

### ✅ Problem 4 — Rotate Matrix 90° Clockwise `[Medium]`

**Task:** Rotate a square matrix 90° clockwise **in-place**.

```java
public class P4_RotateMatrix {

    static void printMatrix(int[][] m) {
        for (int[] row : m) {
            for (int val : row) System.out.printf("%4d", val);
            System.out.println();
        }
    }

    // Step 1: Transpose in-place (j starts at i+1 to avoid double-swap)
    static void transpose(int[][] m) {
        int n = m.length;
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j < n; j++) {
                int temp = m[i][j];
                m[i][j]  = m[j][i];
                m[j][i]  = temp;
            }
    }

    // Step 2: Reverse each row
    static void reverseRows(int[][] m) {
        for (int[] row : m) {
            int left = 0, right = row.length - 1;
            while (left < right) {
                int temp  = row[left];
                row[left]  = row[right];
                row[right] = temp;
                left++; right--;
            }
        }
    }

    // Rotate 90° clockwise = Transpose + Reverse each row
    static void rotate90(int[][] m) {
        transpose(m);
        reverseRows(m);
    }

    public static void main(String[] args) {
        int[][] matrix = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };

        System.out.println("Original:");    printMatrix(matrix);
        rotate90(matrix);
        System.out.println("Rotated 90° CW:"); printMatrix(matrix);

        System.out.println();

        int[][] m2 = {
            {1,  2,  3,  4},
            {5,  6,  7,  8},
            {9, 10, 11, 12},
            {13,14, 15, 16}
        };
        System.out.println("Original 4×4:");    printMatrix(m2);
        rotate90(m2);
        System.out.println("Rotated 90° CW:"); printMatrix(m2);
    }
}
```

```
Output:
Original:
   1   2   3
   4   5   6
   7   8   9
Rotated 90° CW:
   7   4   1
   8   5   2
   9   6   3

Original 4×4:
   1   2   3   4
   5   6   7   8
   9  10  11  12
  13  14  15  16
Rotated 90° CW:
  13   9   5   1
  14  10   6   2
  15  11   7   3
  16  12   8   4
```

**WHY transpose + reverse-row = 90° CW rotation:**
```
Original:        After Transpose:   After Reverse Rows:
1  2  3          1  4  7            7  4  1  ← 90° CW ✅
4  5  6    →     2  5  8     →      8  5  2
7  8  9          3  6  9            9  6  3

Verification: column 0 of original (1,4,7) becomes row 0 reversed = (7,4,1) ✅
```

**Why `j = i + 1` in transpose:**
```
If j starts at 0: [i][j] and [j][i] get swapped TWICE → back to original!
i=0,j=1: swap [0][1]↔[1][0] ✅
i=1,j=0: swap [1][0]↔[0][1] ← undoes the previous swap! ❌
Fix: j starts at i+1 → only upper triangle is processed → each pair swapped ONCE ✅
```

> 🔑 **Key Learning:** This is one of the most asked matrix problems at FAANG companies (LeetCode #48). The two-step algorithm (transpose + reverse) is O(n²) time, O(1) space — in-place. Understanding WHY it works (column becomes row, reversed) is more important than memorizing the steps. The `j = i+1` insight prevents double-swapping and is used in all symmetric matrix operations.

---

### ✅ Problem 5 — Spiral Order Traversal `[Medium]`

**Task:** Print matrix elements in clockwise spiral order.

```java
public class P5_SpiralOrder {

    static void printMatrix(int[][] m) {
        for (int[] row : m) {
            for (int val : row) System.out.printf("%4d", val);
            System.out.println();
        }
    }

    static void spiralOrder(int[][] matrix) {
        int top    = 0;
        int bottom = matrix.length - 1;
        int left   = 0;
        int right  = matrix[0].length - 1;

        System.out.print("Spiral: ");

        while (top <= bottom && left <= right) {

            // → Left to right along top row
            for (int j = left; j <= right; j++)
                System.out.print(matrix[top][j] + " ");
            top++;

            // ↓ Top to bottom along right column
            for (int i = top; i <= bottom; i++)
                System.out.print(matrix[i][right] + " ");
            right--;

            // ← Right to left along bottom row (if still valid)
            if (top <= bottom) {
                for (int j = right; j >= left; j--)
                    System.out.print(matrix[bottom][j] + " ");
                bottom--;
            }

            // ↑ Bottom to top along left column (if still valid)
            if (left <= right) {
                for (int i = bottom; i >= top; i--)
                    System.out.print(matrix[i][left] + " ");
                left++;
            }
        }
        System.out.println();
    }

    public static void main(String[] args) {
        int[][] m1 = {
            { 1,  2,  3,  4},
            { 5,  6,  7,  8},
            { 9, 10, 11, 12},
            {13, 14, 15, 16}
        };
        System.out.println("Matrix 4×4:"); printMatrix(m1);
        spiralOrder(m1);

        System.out.println();

        int[][] m2 = {
            {1, 2, 3},
            {4, 5, 6},
            {7, 8, 9}
        };
        System.out.println("Matrix 3×3:"); printMatrix(m2);
        spiralOrder(m2);

        System.out.println();

        // Non-square test
        int[][] m3 = {
            {1, 2, 3, 4, 5},
            {6, 7, 8, 9, 10}
        };
        System.out.println("Matrix 2×5:"); printMatrix(m3);
        spiralOrder(m3);
    }
}
```

```
Output:
Matrix 4×4:
   1   2   3   4
   5   6   7   8
   9  10  11  12
  13  14  15  16
Spiral: 1 2 3 4 8 12 16 15 14 13 9 5 6 7 11 10

Matrix 3×3:
   1   2   3
   4   5   6
   7   8   9
Spiral: 1 2 3 6 9 8 7 4 5

Matrix 2×5:
   1   2   3   4   5
   6   7   8   9  10
Spiral: 1 2 3 4 5 10 9 8 7 6
```

**Boundary shrinking visualization for 4×4:**
```
Pass 1: top=0,bot=3,left=0,right=3
  → Row 0: 1 2 3 4          (top becomes 1)
  ↓ Col 3: 8 12 16          (right becomes 2)
  ← Row 3: 15 14 13         (bot becomes 2)
  ↑ Col 0: 9 5              (left becomes 1)

Pass 2: top=1,bot=2,left=1,right=2
  → Row 1: 6 7              (top becomes 2)
  ↓ Col 2: 11               (right becomes 1)
  ← Row 2: 10               (bot becomes 1)
  ↑ (left>right? No: Col 1: nothing left)

Done! Spiral: 1 2 3 4 8 12 16 15 14 13 9 5 6 7 11 10 ✅
```

> 🔑 **Key Learning:** Four boundaries (`top`, `bottom`, `left`, `right`) shrink inward after each directional pass. The guards `if (top <= bottom)` and `if (left <= right)` prevent double-printing the last row/column in odd-sized matrices. This boundary-shrinking pattern is used in: binary search on 2D arrays, layer-by-layer DP, and BFS level processing. LeetCode #54 — commonly asked at Amazon, Google, Microsoft.

---

### ✅ Problem 6 — Set Matrix Zeroes `[Medium-Hard]`

**Task:** If any element is 0, set its entire row and column to 0.

```java
public class P6_SetZeroes {

    static void printMatrix(int[][] m) {
        for (int[] row : m) {
            for (int val : row) System.out.printf("%4d", val);
            System.out.println();
        }
    }

    static void setZeroes(int[][] matrix) {
        int rows = matrix.length;
        int cols = matrix[0].length;

        // Step 1: Find ALL zero positions first (before modifying anything!)
        boolean[] zeroRows = new boolean[rows];
        boolean[] zeroCols = new boolean[cols];

        for (int i = 0; i < rows; i++)
            for (int j = 0; j < cols; j++)
                if (matrix[i][j] == 0) {
                    zeroRows[i] = true;   // mark this row
                    zeroCols[j] = true;   // mark this column
                }

        // Step 2: Set entire rows to 0
        for (int i = 0; i < rows; i++)
            if (zeroRows[i])
                for (int j = 0; j < cols; j++)
                    matrix[i][j] = 0;

        // Step 3: Set entire columns to 0
        for (int j = 0; j < cols; j++)
            if (zeroCols[j])
                for (int i = 0; i < rows; i++)
                    matrix[i][j] = 0;
    }

    public static void main(String[] args) {
        int[][] m1 = {
            {1, 1, 1},
            {1, 0, 1},
            {1, 1, 1}
        };
        System.out.println("Before:"); printMatrix(m1);
        setZeroes(m1);
        System.out.println("After:"); printMatrix(m1);

        System.out.println();

        int[][] m2 = {
            {0, 1, 2, 0},
            {3, 4, 5, 2},
            {1, 3, 1, 5}
        };
        System.out.println("Before:"); printMatrix(m2);
        setZeroes(m2);
        System.out.println("After:"); printMatrix(m2);
    }
}
```

```
Output:
Before:
   1   1   1
   1   0   1
   1   1   1
After:
   1   0   1
   0   0   0
   1   0   1

Before:
   0   1   2   0
   3   4   5   2
   1   3   1   5
After:
   0   0   0   0
   0   4   5   0
   0   3   1   0
```

**WHY the naive single-pass approach fails:**
```
Naive: if matrix[i][j]==0, immediately zero out row i and col j

Problem: New zeros created by zeroing spread to other rows/cols
Example:
  Before:        Naive after processing [1][1]=0:
  1  1  1        1  0  1    ← col 1 zeroed
  1  0  1   →    0  0  0    ← row 1 zeroed
  1  1  1        1  0  1    ← col 1 zeroed (OK)

  But now [0][1]=0 and [2][1]=0 are NEW zeros from col 1!
  If we continue scanning, we'll zero out rows 0 and 2 WRONGLY!
  The original matrix had ONLY ONE zero — but naive approach
  propagates it into false zeros → wrong result!
```

**Correct approach — Two-pass:**
```
Pass 1: Scan entire matrix → record zero positions (don't touch matrix!)
Pass 2: Use recorded positions to zero rows and columns
Result: Only ORIGINAL zeros trigger row/col zeroing ✅
```

> 🔑 **Key Learning:** The trap is deliberately designed for this problem. **Never modify a data structure while using it to make decisions.** This is a general programming principle: scan first, modify second. Two boolean arrays (`zeroRows`, `zeroCols`) act as "flags" — recording intent before execution. LeetCode #73 — asked at Facebook, Amazon, Apple. The O(1) space solution uses the first row/column as marker storage — a technique we'll revisit in Phase 3.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Complete Matrix Inspector.

```java
public class MiniChallenge_Day7 {

    static void printMatrix(int[][] m) {
        for (int[] row : m) {
            for (int val : row) System.out.printf("%4d", val);
            System.out.println();
        }
    }

    static int[] maxPosition(int[][] m) {
        int maxVal = m[0][0], r = 0, c = 0;
        for (int i = 0; i < m.length; i++)
            for (int j = 0; j < m[i].length; j++)
                if (m[i][j] > maxVal) { maxVal = m[i][j]; r = i; c = j; }
        return new int[]{maxVal, r, c};
    }

    static int[] minPosition(int[][] m) {
        int minVal = m[0][0], r = 0, c = 0;
        for (int i = 0; i < m.length; i++)
            for (int j = 0; j < m[i].length; j++)
                if (m[i][j] < minVal) { minVal = m[i][j]; r = i; c = j; }
        return new int[]{minVal, r, c};
    }

    static int[] rowSums(int[][] m) {
        int[] sums = new int[m.length];
        for (int i = 0; i < m.length; i++)
            for (int j = 0; j < m[i].length; j++)
                sums[i] += m[i][j];
        return sums;
    }

    static int[] colSums(int[][] m) {
        int[] sums = new int[m[0].length];
        for (int i = 0; i < m.length; i++)
            for (int j = 0; j < m[i].length; j++)
                sums[j] += m[i][j];
        return sums;
    }

    static void matrixInspector(int[][] m) {
        int rows = m.length, cols = m[0].length;
        String line = "═".repeat(40);

        System.out.println(line);
        System.out.printf("    MATRIX INSPECTOR (%d×%d)%n", rows, cols);
        System.out.println(line);

        System.out.println("Matrix:");
        printMatrix(m);

        int[] maxInfo = maxPosition(m);
        int[] minInfo = minPosition(m);
        System.out.printf("Max element : %d at [%d][%d]%n",
                          maxInfo[0], maxInfo[1], maxInfo[2]);
        System.out.printf("Min element : %d at [%d][%d]%n",
                          minInfo[0], minInfo[1], minInfo[2]);

        int[] rSums = rowSums(m);
        System.out.print("Row sums    : ");
        for (int s : rSums) System.out.print(s + "  "); System.out.println();

        int[] cSums = colSums(m);
        System.out.print("Col sums    : ");
        for (int s : cSums) System.out.print(s + "  "); System.out.println();

        // Main diagonal (square only)
        if (rows == cols) {
            int diagSum = 0;
            System.out.print("Main diag   : ");
            for (int i = 0; i < rows; i++) {
                System.out.print(m[i][i] + " ");
                diagSum += m[i][i];
            }
            System.out.println(" (sum = " + diagSum + ")");

            int antiSum = 0;
            System.out.print("Anti diag   : ");
            for (int i = 0; i < rows; i++) {
                System.out.print(m[i][rows-1-i] + " ");
                antiSum += m[i][rows-1-i];
            }
            System.out.println(" (sum = " + antiSum + ")");
            System.out.println("Trace       : " + diagSum);
        }

        System.out.println(line);
    }

    public static void main(String[] args) {
        int[][] matrix = {
            { 5,  3,  8,  1},
            { 2,  9,  4,  7},
            { 6,  1, 10,  3},
            { 4,  7,  2,  8}
        };
        matrixInspector(matrix);
    }
}
```

```
Output:
════════════════════════════════════════
    MATRIX INSPECTOR (4×4)  
════════════════════════════════════════
Matrix:
   5   3   8   1
   2   9   4   7
   6   1  10   3
   4   7   2   8
Max element : 10 at [2][2]
Min element : 1  at [0][3]
Row sums    : 17  22  20  21
Col sums    : 17  20  24  19
Main diag   : 5 9 10 8  (sum = 32)
Anti diag   : 1 4 1 4   (sum = 10)
Trace       : 32
════════════════════════════════════════
```

> 🔑 **Key Learning:** The **trace** of a matrix (sum of main diagonal) is a fundamental linear algebra concept — it appears in graph adjacency matrices and covariance matrices in ML. Each helper method returns data (not void) so the coordinator can use it. `rowSums` and `colSums` both return `int[]` arrays — returning arrays from methods is a powerful pattern for collecting multiple computed values.

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| Two-pointer reverse — traced for `{3,1,4,1,5}` | ✅ See trace below |
| `linearSearch` from memory | ✅ Returns index or -1 |
| `arr.clone()` — when needed + example | ✅ See note below |
| Why `max = arr[0]` beats `max = 0` | ✅ Counter-example shown |

**Revision Trace — reverse `{3,1,4,1,5}`:**
```
left=0, right=4: swap 3↔5 → [5,1,4,1,3], left=1, right=3
left=1, right=3: swap 1↔1 → [5,1,4,1,3], left=2, right=2
left=2, right=2: left < right? NO → STOP
Result: [5,1,4,1,3] ✅
```

**`arr.clone()` note:**
```java
int[] original = {1, 2, 3, 4, 5};
int[] copy = original.clone();   // safe shallow copy for primitives
reverse(copy);                   // reverses copy only
// original still {1,2,3,4,5} ✅
```

**Counter-example for `max = 0` failure:**
```java
int[] arr = {-5, -3, -8, -1};
// With max = 0: loop finds no element > 0 → max stays 0 → WRONG! 0 not in array
// With max = arr[0] = -5: loop finds -3 > -5, -1 > -3 → max = -1 → CORRECT ✅
```

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. How is a 2D array stored in memory? Why "array of arrays"?</b></summary>

> In Java, a 2D array is literally an **array of references**, where each reference points to a separate 1D array on the heap. The outer array (on heap) stores references to each row. Each row is independently allocated as a 1D array.
> This is why it's called "array of arrays" — you can think of `matrix[i]` as fetching the i-th 1D array, and `matrix[i][j]` as then indexing into it.
> Implication: rows can have different lengths (jagged arrays), and `matrix.length` gives the number of rows (size of the outer array), while `matrix[0].length` gives the columns (size of the first inner array).

</details>

<details>
<summary><b>Q2. What does matrix.length give? What gives columns?</b></summary>

> `matrix.length` → number of **rows** (size of the outer array of references).
> `matrix[0].length` → number of **columns** in row 0 (size of the first inner 1D array).
> There's no `matrix.colLength` because in Java, each row is independent — columns are properties of individual rows. For a rectangular matrix, all rows have the same length, so `matrix[0].length` works. For a jagged matrix, you'd check each row separately.

</details>

<details>
<summary><b>Q3. Why does the transpose inner loop start at j = i + 1?</b></summary>

> The transpose operation swaps `matrix[i][j]` with `matrix[j][i]`. If we start `j` at 0, every pair gets swapped twice — once when we process `(i,j)` and again when we process `(j,i)` — resulting in the matrix returning to its original state (no change).
> Starting `j` at `i+1` ensures we only process the **upper triangle** (above the main diagonal). Each pair is swapped exactly once. The main diagonal (`i==j`) is skipped automatically since `j` starts at `i+1`, not `i`.

</details>

<details>
<summary><b>Q4. What are the four spiral boundaries? How do they shrink?</b></summary>

> **Four boundaries:** `top` (topmost unprocessed row), `bottom` (bottommost), `left` (leftmost column), `right` (rightmost column).
> **Shrinking after each direction:**
> - After left→right pass: `top++` (top row consumed)
> - After top→bottom pass: `right--` (right column consumed)
> - After right→left pass: `bottom--` (bottom row consumed)
> - After bottom→top pass: `left++` (left column consumed)
> Loop continues while `top ≤ bottom AND left ≤ right`. Guards `if (top ≤ bottom)` and `if (left ≤ right)` prevent double-counting for odd-dimension matrices.

</details>

<details>
<summary><b>Q5. Why does the naive single-pass approach fail for Set Matrix Zeroes?</b></summary>

> **The trap:** If you zero out a row/column as soon as you find a zero, the newly created zeros spread to other rows and columns during the same scan. Elements that were originally non-zero but got zeroed (due to being in the same row/column as an original zero) then falsely trigger zeroing of MORE rows and columns.
> **The fix — Two-pass approach:** First scan the entire matrix and record ALL original zero positions (using boolean arrays for rows and columns). Don't modify the matrix yet. Then in a second pass, use the recorded positions to zero out rows and columns. This ensures only ORIGINAL zeros cause zeroing — not the propagated ones.
> General principle: **never make decisions based on data you're actively modifying.**

</details>

---

## 📌 DAY 7 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 7 — SUMMARY                        ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 05, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  2D Arrays & Matrix Operations                   ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  2D array as array of arrays — memory model                  ║
║  ✅  3 declaration methods + size properties                     ║
║  ✅  3 traversal patterns — row, column, for-each                ║
║  ✅  Diagonal access — main, anti, above, below                  ║
║  ✅  Border element condition                                    ║
║  ✅  6 essential operations — sums, transpose, search, print     ║
╠══════════════════════════════════════════════════════════════════╣
║  PROBLEMS SOLVED                                                ║
║  ✅  P1 — Matrix Input + Row/Col/Diag Sums                       ║
║  ✅  P2 — Search in 2D (return {row, col})                       ║
║  ✅  P3 — Transpose (square + non-square)                        ║
║  ✅  P4 — Rotate 90° CW (transpose + reverse rows)              ║
║  ✅  P5 — Spiral Order (4 boundaries shrinking)                  ║
║  ✅  P6 — Set Matrix Zeroes (two-pass, no false propagation)     ║
║  ✅  Mini — Matrix Inspector (coordinator + helpers)             ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  90%                              ║
║  STRONG AREA    :  Traversal, sums, transpose, search           ║
║  WEAK AREA      :  Spiral boundary guards for odd dimensions     ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  matrix.length=rows, matrix[0].length=cols — always          ║
║  ▸  Transpose inner j=i+1 — avoid double-swap                   ║
║  ▸  Rotate 90°CW = Transpose + Reverse each row                 ║
║  ▸  Spiral: 4 boundaries shrink after each direction            ║
║  ▸  Set Zeroes: scan first, modify second — never both together  ║
║  ▸  Return from nested loop = clean early exit (vs break)       ║
║  ▸  2D arrays are DP tables — this is the foundation of Phase 4 ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 06](./Day06_Arrays.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 08 →](./Day08_Strings.md)

*Day 7 of 160 — Done ✅ | Streak: 7 Days 🔥🔥🔥🔥🔥🔥🔥*

</div>
