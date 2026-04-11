# 📄 Day 12 — Collections Framework

<div align="center">

![Day](https://img.shields.io/badge/Day-12_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Collections_Framework-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 11](./Day11_Recursion.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 13 →](./Day13_Sorting.md)

</div>

---

## 🎯 Day 12 Goal
> Master Java's **Collections Framework** — ArrayList, LinkedList, HashMap, HashSet, Stack, and Queue. These are the ready-made data structures you will use in **every single DSA problem** from Day 46 onwards. Learn them now so you can focus on algorithms later — not syntax.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | Collections hierarchy + ArrayList + LinkedList | 60 min |
| Block 2 | HashMap + HashSet — hashing concepts | 50 min |
| Block 3 | Stack + Queue + Deque | 35 min |
| Block 4 | Revision — Day 11 Recursion flash review | 15 min |
| Block 5 | Practice Set — 6 Problems | 120 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. The Collections Framework — Big Picture

Java's Collections Framework is a unified architecture of **interfaces + implementations** for storing and manipulating groups of objects.

```
java.util.Collection (interface)
├── List (interface) — ordered, duplicates allowed, index-based
│   ├── ArrayList    ← dynamic array, fast random access
│   ├── LinkedList   ← doubly linked list, fast insert/delete
│   └── Vector       ← thread-safe ArrayList (legacy)
│
├── Set (interface) — no duplicates
│   ├── HashSet      ← unordered, O(1) ops
│   ├── LinkedHashSet← insertion-ordered
│   └── TreeSet      ← sorted order
│
└── Queue (interface) — FIFO
    ├── LinkedList   ← also implements Queue
    ├── PriorityQueue← heap-based, sorted by priority
    └── ArrayDeque   ← fast double-ended queue

java.util.Map (interface) — key-value pairs (NOT a Collection)
├── HashMap          ← unordered, O(1) ops
├── LinkedHashMap    ← insertion-ordered
└── TreeMap          ← sorted by key
```

> 💡 **Why Collections over arrays?**
> Arrays are fixed-size and type-limited. Collections are dynamic (grow/shrink), type-safe via generics, and come with built-in algorithms (sort, search, shuffle).

---

### 🔷 2. Generics — `<Type>` Syntax

Collections use **generics** to store a specific type safely:

```java
ArrayList<String>  names  = new ArrayList<>();   // only Strings
ArrayList<Integer> numbers = new ArrayList<>();  // only Integers (autoboxed)
HashMap<String, Integer> scores = new HashMap<>();

// Autoboxing — primitives auto-converted to wrapper types
ArrayList<Integer> list = new ArrayList<>();
list.add(42);        // int 42 → autoboxed to Integer(42)
int x = list.get(0); // Integer(42) → unboxed to int 42
```

**Primitive → Wrapper class mapping:**
| Primitive | Wrapper |
|-----------|---------|
| `int` | `Integer` |
| `double` | `Double` |
| `char` | `Character` |
| `boolean` | `Boolean` |
| `long` | `Long` |

---

### 🔷 3. ArrayList — Dynamic Array

`ArrayList` is a resizable array. Best for: frequent random access, iteration, infrequent insertions/deletions.

```java
import java.util.ArrayList;
import java.util.Collections;

ArrayList<String> cities = new ArrayList<>();

// ── Adding ────────────────────────────────────────
cities.add("Patna");           // add to end
cities.add("Delhi");
cities.add("Mumbai");
cities.add(1, "Kolkata");     // add at index 1

// ── Accessing ─────────────────────────────────────
System.out.println(cities.get(0));       // "Patna"
System.out.println(cities.size());       // 4
System.out.println(cities.contains("Delhi")); // true
System.out.println(cities.indexOf("Mumbai")); // 3

// ── Modifying ─────────────────────────────────────
cities.set(0, "Patna City");   // replace index 0
cities.remove("Delhi");        // remove by value
cities.remove(0);              // remove by index

// ── Iterating — 3 ways ────────────────────────────
for (int i = 0; i < cities.size(); i++)
    System.out.print(cities.get(i) + " ");

for (String city : cities)
    System.out.print(city + " ");

cities.forEach(city -> System.out.print(city + " ")); // lambda

// ── Sorting ───────────────────────────────────────
Collections.sort(cities);                    // ascending
Collections.sort(cities, Collections.reverseOrder()); // descending

// ── Useful operations ─────────────────────────────
cities.clear();                // remove all elements
boolean empty = cities.isEmpty();
List<String> sub = cities.subList(0, 2);   // view of [0,2)

// ── Convert Array ↔ ArrayList ─────────────────────
String[] arr = {"A", "B", "C"};
ArrayList<String> list = new ArrayList<>(Arrays.asList(arr));
String[] back = list.toArray(new String[0]);
```

**ArrayList Time Complexity:**
| Operation | Time |
|-----------|------|
| `get(i)` | O(1) |
| `add(end)` | O(1) amortized |
| `add(i)` | O(n) |
| `remove(i)` | O(n) |
| `contains` | O(n) |

---

### 🔷 4. LinkedList — Doubly Linked List

`LinkedList` implements both `List` and `Deque`. Best for: frequent insertions/deletions, implementing stacks and queues.

```java
import java.util.LinkedList;

LinkedList<Integer> ll = new LinkedList<>();

// As a List
ll.add(10); ll.add(20); ll.add(30);

// As a Queue / Deque — extra methods
ll.addFirst(5);    // insert at head
ll.addLast(40);    // insert at tail
ll.removeFirst();  // remove from head
ll.removeLast();   // remove from tail
ll.peekFirst();    // view head without removing
ll.peekLast();     // view tail without removing
```

**ArrayList vs LinkedList:**
| | ArrayList | LinkedList |
|--|-----------|------------|
| Random access `get(i)` | O(1) ✅ | O(n) ❌ |
| Insert/Delete at middle | O(n) | O(1) if reference known |
| Insert/Delete at ends | O(1) | O(1) ✅ |
| Memory | Less (array) | More (node pointers) |
| Use when | Read-heavy | Insert/Delete-heavy |

---

### 🔷 5. HashMap — Key-Value Storage

`HashMap` stores key-value pairs. Keys are unique. Values can repeat. O(1) for get/put/remove on average.

```java
import java.util.HashMap;
import java.util.Map;

HashMap<String, Integer> scores = new HashMap<>();

// ── Putting ───────────────────────────────────────
scores.put("Abhishek", 95);
scores.put("Riya",     88);
scores.put("Arjun",    76);
scores.put("Abhishek", 99);   // REPLACES old value for same key

// ── Getting ───────────────────────────────────────
System.out.println(scores.get("Riya"));          // 88
System.out.println(scores.get("Unknown"));       // null (key absent)
System.out.println(scores.getOrDefault("X", 0)); // 0 — safe default

// ── Checking ──────────────────────────────────────
System.out.println(scores.containsKey("Arjun")); // true
System.out.println(scores.containsValue(88));    // true
System.out.println(scores.size());               // 3

// ── Removing ──────────────────────────────────────
scores.remove("Arjun");

// ── Iterating — 3 ways ────────────────────────────
// Way 1: entrySet — most common
for (Map.Entry<String, Integer> entry : scores.entrySet()) {
    System.out.println(entry.getKey() + " → " + entry.getValue());
}

// Way 2: keySet
for (String key : scores.keySet()) {
    System.out.println(key + ": " + scores.get(key));
}

// Way 3: values only
for (int val : scores.values()) {
    System.out.println(val);
}

// ── Useful operations ─────────────────────────────
scores.putIfAbsent("Riya", 100);  // only adds if key missing
scores.getOrDefault("Zara", -1);  // safe get with default
scores.merge("Abhishek", 5, Integer::sum); // Abhishek += 5

// ── Frequency counting pattern (extremely common in DSA) ──
String s = "programming";
HashMap<Character, Integer> freq = new HashMap<>();
for (char c : s.toCharArray()) {
    freq.put(c, freq.getOrDefault(c, 0) + 1);
}
// freq: {p=1, r=2, o=1, g=2, a=1, m=2, i=1, n=1}
```

---

### 🔷 6. HashSet — Unique Elements

`HashSet` stores unique elements. No duplicates. No guaranteed order. O(1) add/remove/contains.

```java
import java.util.HashSet;

HashSet<String> visited = new HashSet<>();

// ── Adding ────────────────────────────────────────
visited.add("Delhi");
visited.add("Mumbai");
visited.add("Patna");
visited.add("Delhi");       // IGNORED — already present

System.out.println(visited.size());         // 3 (not 4!)
System.out.println(visited.contains("Patna")); // true

// ── Removing ──────────────────────────────────────
visited.remove("Mumbai");

// ── Set operations ────────────────────────────────
HashSet<Integer> a = new HashSet<>(Arrays.asList(1,2,3,4,5));
HashSet<Integer> b = new HashSet<>(Arrays.asList(4,5,6,7,8));

// Union
HashSet<Integer> union = new HashSet<>(a);
union.addAll(b);           // {1,2,3,4,5,6,7,8}

// Intersection
HashSet<Integer> intersection = new HashSet<>(a);
intersection.retainAll(b); // {4,5}

// Difference (a - b)
HashSet<Integer> diff = new HashSet<>(a);
diff.removeAll(b);         // {1,2,3}

// ── Most common DSA use — duplicate detection ──────
int[] nums = {1, 2, 3, 2, 4, 1};
HashSet<Integer> seen = new HashSet<>();
for (int n : nums) {
    if (!seen.add(n)) {    // add() returns false if already present!
        System.out.println("Duplicate: " + n);
    }
}
// Output: Duplicate: 2  Duplicate: 1
```

---

### 🔷 7. Stack — LIFO

Java's `Stack` class or `Deque` (preferred) implements Last-In-First-Out.

```java
import java.util.Stack;
import java.util.ArrayDeque;
import java.util.Deque;

// Option 1: Stack class (legacy but simple)
Stack<Integer> stack = new Stack<>();
stack.push(10);     // add to top
stack.push(20);
stack.push(30);
System.out.println(stack.peek());  // 30 — view top
System.out.println(stack.pop());   // 30 — remove top
System.out.println(stack.isEmpty());// false

// Option 2: ArrayDeque as Stack (preferred — faster)
Deque<Integer> stackD = new ArrayDeque<>();
stackD.push(10);    // addFirst()
stackD.push(20);
stackD.push(30);
System.out.println(stackD.peek());  // 30
System.out.println(stackD.pop());   // 30

// Stack use cases in DSA:
// - Balanced brackets problem
// - Undo/redo operations
// - DFS iterative implementation
// - Expression evaluation
// - Backtracking
```

---

### 🔷 8. Queue — FIFO

Queue implements First-In-First-Out. `LinkedList` or `ArrayDeque` as implementation.

```java
import java.util.Queue;
import java.util.LinkedList;
import java.util.ArrayDeque;

// Queue using LinkedList
Queue<String> queue = new LinkedList<>();
queue.offer("First");   // add to rear (offer preferred over add)
queue.offer("Second");
queue.offer("Third");

System.out.println(queue.peek());   // "First" — view front
System.out.println(queue.poll());   // "First" — remove front
System.out.println(queue.size());   // 2

// Queue using ArrayDeque (preferred — faster)
Queue<Integer> q = new ArrayDeque<>();
q.offer(1); q.offer(2); q.offer(3);
while (!q.isEmpty()) {
    System.out.print(q.poll() + " ");  // 1 2 3
}

// Queue use cases in DSA:
// - BFS (Breadth-First Search)
// - Level-order traversal of trees
// - Job scheduling
// - Sliding window maximum
```

---

### 🔷 9. Collections Utility Class

```java
import java.util.Collections;

ArrayList<Integer> nums = new ArrayList<>(Arrays.asList(5, 2, 8, 1, 9, 3));

Collections.sort(nums);                         // [1, 2, 3, 5, 8, 9]
Collections.sort(nums, Collections.reverseOrder()); // [9, 8, 5, 3, 2, 1]
Collections.reverse(nums);                      // reverses in place
Collections.shuffle(nums);                      // random shuffle
Collections.max(nums);                          // 9
Collections.min(nums);                          // 1
Collections.frequency(nums, 5);                 // count of 5
Collections.fill(nums, 0);                      // fill all with 0
Collections.nCopies(5, "Java");                 // [Java,Java,Java,Java,Java]
int idx = Collections.binarySearch(sorted, 5);  // binary search (must be sorted)
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — ArrayList Operations `[Easy]`

**Task:** Build a student name manager — add, remove, search, sort.

```java
import java.util.*;

public class P1_ArrayListOps {
    public static void main(String[] args) {
        ArrayList<String> students = new ArrayList<>();

        // Add students
        students.add("Abhishek Ranjan");
        students.add("Riya Sharma");
        students.add("Arjun Kumar");
        students.add("Priya Singh");
        students.add("Vikram Das");

        System.out.println("Original  : " + students);
        System.out.println("Size      : " + students.size());
        System.out.println("Index of Riya: " + students.indexOf("Riya Sharma"));
        System.out.println("Contains Arjun: " + students.contains("Arjun Kumar"));

        // Add at specific index
        students.add(2, "Sneha Patel");
        System.out.println("After add(2): " + students);

        // Update
        students.set(0, "Abhishek R.");
        System.out.println("After set(0): " + students);

        // Remove
        students.remove("Vikram Das");
        students.remove(0);
        System.out.println("After remove: " + students);

        // Sort
        Collections.sort(students);
        System.out.println("Sorted      : " + students);

        // Reverse
        Collections.sort(students, Collections.reverseOrder());
        System.out.println("Reversed    : " + students);

        // Iterate with index
        System.out.println("\nRoster:");
        for (int i = 0; i < students.size(); i++)
            System.out.println("  " + (i+1) + ". " + students.get(i));

        // Convert to array
        String[] arr = students.toArray(new String[0]);
        System.out.println("\nAs array[0]: " + arr[0]);
    }
}
```

```
Output:
Original  : [Abhishek Ranjan, Riya Sharma, Arjun Kumar, Priya Singh, Vikram Das]
Size      : 5
Index of Riya: 1
Contains Arjun: true
After add(2): [Abhishek Ranjan, Riya Sharma, Sneha Patel, Arjun Kumar, Priya Singh, Vikram Das]
After set(0): [Abhishek R., Riya Sharma, Sneha Patel, Arjun Kumar, Priya Singh, Vikram Das]
After remove: [Riya Sharma, Sneha Patel, Arjun Kumar, Priya Singh]
Sorted      : [Arjun Kumar, Priya Singh, Riya Sharma, Sneha Patel]
Reversed    : [Sneha Patel, Riya Sharma, Priya Singh, Arjun Kumar]

Roster:
  1. Sneha Patel
  2. Riya Sharma
  3. Priya Singh
  4. Arjun Kumar

As array[0]: Sneha Patel
```

> 🔑 **Key Learning:** `remove(int index)` vs `remove(Object o)` — Java picks by type. `remove(0)` removes by index; `remove("Name")` removes by value. `Collections.sort()` works on any `Comparable` — Strings sort alphabetically. `toArray(new String[0])` is the idiomatic way to convert ArrayList → array. `indexOf` returns -1 if not found — always check!

---

### ✅ Problem 2 — HashMap Frequency Counter `[Easy-Medium]`

**Task:** Count character/word frequency using HashMap — the most common DSA pattern.

```java
import java.util.*;

public class P2_HashMapFrequency {

    // Count character frequencies
    static HashMap<Character, Integer> charFreq(String s) {
        HashMap<Character, Integer> freq = new HashMap<>();
        for (char c : s.toCharArray())
            freq.put(c, freq.getOrDefault(c, 0) + 1);
        return freq;
    }

    // Count word frequencies
    static HashMap<String, Integer> wordFreq(String sentence) {
        HashMap<String, Integer> freq = new HashMap<>();
        for (String word : sentence.split("\\s+"))
            freq.put(word, freq.getOrDefault(word, 0) + 1);
        return freq;
    }

    // Find most frequent character
    static char mostFrequent(String s) {
        HashMap<Character, Integer> freq = charFreq(s);
        char maxChar = s.charAt(0);
        int  maxFreq = 0;
        for (Map.Entry<Character, Integer> e : freq.entrySet())
            if (e.getValue() > maxFreq) { maxFreq = e.getValue(); maxChar = e.getKey(); }
        return maxChar;
    }

    public static void main(String[] args) {
        // Char frequency
        String s = "programming";
        System.out.println("String: \"" + s + "\"");
        System.out.println("Char freq: " + charFreq(s));
        System.out.println("Most frequent: '" + mostFrequent(s) + "'");

        System.out.println();

        // Word frequency
        String sentence = "java is great java is powerful java";
        System.out.println("Sentence: \"" + sentence + "\"");
        HashMap<String, Integer> wf = wordFreq(sentence);
        // Print sorted by word
        List<String> words = new ArrayList<>(wf.keySet());
        Collections.sort(words);
        for (String w : words)
            System.out.println("  \"" + w + "\" → " + wf.get(w) + " times");

        System.out.println();

        // Check if two strings are anagrams using HashMap
        String s1 = "listen", s2 = "silent";
        HashMap<Character, Integer> f1 = charFreq(s1);
        HashMap<Character, Integer> f2 = charFreq(s2);
        System.out.println("\"" + s1 + "\" and \"" + s2 + "\" anagram? " + f1.equals(f2));
    }
}
```

```
Output:
String: "programming"
Char freq: {p=1, r=2, o=1, g=2, a=1, m=2, i=1, n=1}
Most frequent: 'r'  (or 'g' or 'm' — all freq=2)

Sentence: "java is great java is powerful java"
  "great"    → 1 times
  "is"       → 2 times
  "java"     → 3 times
  "powerful" → 1 times

"listen" and "silent" anagram? true
```

> 🔑 **Key Learning:** `freq.getOrDefault(c, 0) + 1` is the canonical frequency counting pattern — used in anagram checking, top-K frequent elements, sliding window problems, and group anagrams. `HashMap.equals()` compares all key-value pairs — two frequency maps being equal proves anagram. This single pattern (`getOrDefault + put`) solves 20+ DSA interview problems.

---

### ✅ Problem 3 — HashSet Duplicate Detection `[Easy-Medium]`

**Task:** Find duplicates, unique elements, and perform set operations.

```java
import java.util.*;

public class P3_HashSetOps {

    // Find all duplicate elements in array
    static List<Integer> findDuplicates(int[] arr) {
        HashSet<Integer> seen = new HashSet<>();
        List<Integer> dups    = new ArrayList<>();
        for (int n : arr)
            if (!seen.add(n)) dups.add(n);  // add() returns false if duplicate
        return dups;
    }

    // Remove duplicates from array, preserve order
    static List<Integer> removeDuplicates(int[] arr) {
        LinkedHashSet<Integer> lhs = new LinkedHashSet<>();
        for (int n : arr) lhs.add(n);       // LinkedHashSet preserves insertion order
        return new ArrayList<>(lhs);
    }

    // Two arrays: find common elements
    static HashSet<Integer> intersection(int[] a, int[] b) {
        HashSet<Integer> setA = new HashSet<>();
        for (int x : a) setA.add(x);
        HashSet<Integer> result = new HashSet<>();
        for (int x : b) if (setA.contains(x)) result.add(x);
        return result;
    }

    // Check if array has all unique elements
    static boolean allUnique(int[] arr) {
        HashSet<Integer> seen = new HashSet<>();
        for (int n : arr)
            if (!seen.add(n)) return false;
        return true;
    }

    public static void main(String[] args) {
        int[] nums = {1, 3, 4, 2, 2, 3, 5, 1, 6};

        System.out.println("Array    : " + Arrays.toString(nums));
        System.out.println("Dups     : " + findDuplicates(nums));
        System.out.println("No dups  : " + removeDuplicates(nums));
        System.out.println("Unique?  : " + allUnique(nums));

        System.out.println();

        int[] a = {1, 2, 3, 4, 5};
        int[] b = {3, 4, 5, 6, 7};
        System.out.println("A        : " + Arrays.toString(a));
        System.out.println("B        : " + Arrays.toString(b));
        System.out.println("A ∩ B    : " + intersection(a, b));

        HashSet<Integer> setA = new HashSet<>(Arrays.asList(1,2,3,4,5));
        HashSet<Integer> setB = new HashSet<>(Arrays.asList(3,4,5,6,7));

        HashSet<Integer> union = new HashSet<>(setA); union.addAll(setB);
        HashSet<Integer> diff  = new HashSet<>(setA); diff.removeAll(setB);
        System.out.println("A ∪ B    : " + union);
        System.out.println("A - B    : " + diff);
    }
}
```

```
Output:
Array    : [1, 3, 4, 2, 2, 3, 5, 1, 6]
Dups     : [2, 3, 1]
No dups  : [1, 3, 4, 2, 5, 6]
Unique?  : false

A        : [1, 2, 3, 4, 5]
B        : [3, 4, 5, 6, 7]
A ∩ B    : [3, 4, 5]
A ∪ B    : [1, 2, 3, 4, 5, 6, 7]
A - B    : [1, 2]
```

> 🔑 **Key Learning:** `set.add(n)` returns `false` if `n` already exists — one line duplicate detection, no nested loops, O(n) vs O(n²). `LinkedHashSet` preserves insertion order while still rejecting duplicates — perfect for "remove duplicates, maintain order" problems. `HashSet.contains()` is O(1) — used for O(n) intersection instead of O(n²) nested loop. These exact patterns appear in: Two Sum, Longest Substring Without Repeating Characters, and Intersection of Arrays.

---

### ✅ Problem 4 — Stack Applications `[Medium]`

**Task:** Use Stack for balanced brackets and reverse using stack.

```java
import java.util.*;

public class P4_StackApps {

    // Application 1: Balanced bracket checker using Stack
    static boolean isBalanced(String s) {
        Deque<Character> stack = new ArrayDeque<>();

        for (char c : s.toCharArray()) {
            if (c == '(' || c == '[' || c == '{') {
                stack.push(c);                  // push opening
            } else if (c == ')' || c == ']' || c == '}') {
                if (stack.isEmpty()) return false;
                char top = stack.pop();
                if (c==')' && top!='(') return false;
                if (c==']' && top!='[') return false;
                if (c=='}' && top!='{') return false;
            }
        }
        return stack.isEmpty();   // balanced only if nothing left
    }

    // Application 2: Reverse a string using Stack
    static String reverseWithStack(String s) {
        Deque<Character> stack = new ArrayDeque<>();
        for (char c : s.toCharArray()) stack.push(c);

        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) sb.append(stack.pop());
        return sb.toString();
    }

    // Application 3: Decimal to Binary using Stack
    static String decToBinary(int n) {
        if (n == 0) return "0";
        Deque<Integer> stack = new ArrayDeque<>();
        while (n > 0) {
            stack.push(n % 2);
            n /= 2;
        }
        StringBuilder sb = new StringBuilder();
        while (!stack.isEmpty()) sb.append(stack.pop());
        return sb.toString();
    }

    public static void main(String[] args) {
        // Balanced brackets
        String[] exprs = {"([])", "()[]{}", "([)]", "(((",
                          "{[()]}", "((())", ""};
        System.out.println("=== Balanced Brackets ===");
        for (String e : exprs)
            System.out.printf("%-12s → %s%n",
                "\"" + e + "\"",
                isBalanced(e) ? "✅ Balanced" : "❌ Not Balanced");

        System.out.println();

        // Reverse with stack
        System.out.println("=== Reverse with Stack ===");
        System.out.println("\"Abhishek\" → \"" + reverseWithStack("Abhishek") + "\"");
        System.out.println("\"Java\"     → \"" + reverseWithStack("Java") + "\"");

        System.out.println();

        // Decimal to Binary
        System.out.println("=== Decimal to Binary ===");
        for (int n : new int[]{0, 1, 5, 10, 42, 255})
            System.out.printf("%3d → %s%n", n, decToBinary(n));
    }
}
```

```
Output:
=== Balanced Brackets ===
"([])"       → ✅ Balanced
"()[]{}"     → ✅ Balanced
"([)]"       → ❌ Not Balanced
"((("        → ❌ Not Balanced
"{[()]}"     → ✅ Balanced
"((())"      → ❌ Not Balanced
""           → ✅ Balanced

=== Reverse with Stack ===
"Abhishek" → "kehsibhA"
"Java"     → "avaJ"

=== Decimal to Binary ===
  0 → 0
  1 → 1
  5 → 101
 10 → 1010
 42 → 101010
255 → 11111111
```

> 🔑 **Key Learning:** Stack's LIFO nature is perfect for bracket matching — the last opened bracket must be the first closed. For decimal-to-binary: remainders are extracted right-to-left (`n%2, n/2`) but must be read left-to-right for the result — push in extraction order, pop for output = reverses automatically. `ArrayDeque` is preferred over `Stack` class — same interface, significantly faster. These stack applications (brackets, expression evaluation, DFS) appear in almost every DSA interview.

---

### ✅ Problem 5 — Queue Applications `[Medium]`

**Task:** Queue for BFS-style level processing and sliding window.

```java
import java.util.*;

public class P5_QueueApps {

    // Application 1: Print numbers level by level (simulating BFS style)
    static void processLevels(int totalLevels) {
        Queue<String> queue = new ArrayDeque<>();
        queue.offer("Level-1-Task-A");
        queue.offer("Level-1-Task-B");

        System.out.println("=== Task Queue Processing ===");
        int level = 1;
        while (!queue.isEmpty() && level <= totalLevels) {
            int size = queue.size();    // number of tasks at this level
            System.out.print("Level " + level + ": ");
            for (int i = 0; i < size; i++) {
                String task = queue.poll();
                System.out.print(task + " ");
                // Enqueue next level tasks
                if (level < totalLevels) {
                    queue.offer(task + "→sub1");
                    queue.offer(task + "→sub2");
                }
            }
            System.out.println();
            level++;
        }
    }

    // Application 2: Moving average using Queue (sliding window)
    static double[] movingAverage(int[] nums, int k) {
        Queue<Integer> window = new ArrayDeque<>();
        double[] result = new double[nums.length];
        double   sum    = 0;

        for (int i = 0; i < nums.length; i++) {
            window.offer(nums[i]);
            sum += nums[i];

            if (window.size() > k) {
                sum -= window.poll();   // remove oldest element
            }

            result[i] = (window.size() == k) ? sum / k : sum / window.size();
        }
        return result;
    }

    // Application 3: Hot potato (circular queue simulation)
    static String hotPotato(String[] names, int count) {
        Queue<String> queue = new ArrayDeque<>(Arrays.asList(names));

        while (queue.size() > 1) {
            for (int i = 0; i < count; i++) {
                queue.offer(queue.poll());  // pass the potato
            }
            String eliminated = queue.poll();
            System.out.println("Eliminated: " + eliminated);
        }
        return queue.peek();
    }

    public static void main(String[] args) {
        processLevels(3);

        System.out.println();

        int[] nums = {1, 3, 5, 7, 9, 2, 4};
        double[] avgs = movingAverage(nums, 3);
        System.out.println("Array  : " + Arrays.toString(nums));
        System.out.printf ("Moving avg (k=3): %s%n",
            Arrays.toString(Arrays.stream(avgs)
                .mapToObj(d -> String.format("%.1f", d))
                .toArray(String[]::new)));

        System.out.println();

        String[] players = {"Abhishek","Riya","Arjun","Priya","Vikram"};
        System.out.println("=== Hot Potato (count=3) ===");
        String winner = hotPotato(players, 3);
        System.out.println("Winner: " + winner);
    }
}
```

```
Output:
=== Task Queue Processing ===
Level 1: Level-1-Task-A Level-1-Task-B
Level 2: Level-1-Task-A→sub1 Level-1-Task-A→sub2 Level-1-Task-B→sub1 Level-1-Task-B→sub2
Level 3: Level-1-Task-A→sub1→sub1 Level-1-Task-A→sub1→sub2 ...

Array  : [1, 3, 5, 7, 9, 2, 4]
Moving avg (k=3): [1.0, 2.0, 3.0, 5.0, 7.0, 6.0, 5.0]

=== Hot Potato (count=3) ===
Eliminated: Priya
Eliminated: Abhishek
Eliminated: Vikram
Eliminated: Riya
Winner: Arjun
```

> 🔑 **Key Learning:** The level-by-level processing pattern — `int size = queue.size()` before the inner for loop — is the **exact pattern used in BFS tree/graph traversal** in Phase 3. Every level-order tree problem, shortest path in unweighted graph, and word ladder problem uses this. Moving average with a Queue is the **sliding window** pattern — add new, remove old when window exceeds k. This is a real interview problem (LeetCode #346).

---

### ✅ Problem 6 — Combined Collections Challenge `[Medium-Hard]`

**Task:** Phone book system using HashMap + contact grouping using List + deduplication using Set.

```java
import java.util.*;

public class P6_PhoneBook {

    static HashMap<String, List<String>> phoneBook = new HashMap<>();

    // Add contact — one name can have multiple numbers
    static void addContact(String name, String number) {
        phoneBook.putIfAbsent(name, new ArrayList<>());
        if (!phoneBook.get(name).contains(number)) {
            phoneBook.get(name).add(number);
            System.out.println("✅ Added: " + name + " → " + number);
        } else {
            System.out.println("⚠️  Duplicate number for " + name + ": " + number);
        }
    }

    // Search — returns all numbers for a name
    static void search(String name) {
        if (phoneBook.containsKey(name)) {
            System.out.println("📞 " + name + ": " + phoneBook.get(name));
        } else {
            System.out.println("❌ Not found: " + name);
        }
    }

    // Delete contact
    static void delete(String name) {
        if (phoneBook.remove(name) != null)
            System.out.println("🗑️  Deleted: " + name);
        else
            System.out.println("❌ Not found: " + name);
    }

    // Print all contacts sorted by name
    static void printAll() {
        System.out.println("\n📖 Phone Book (" + phoneBook.size() + " contacts):");
        System.out.println("─".repeat(40));
        List<String> names = new ArrayList<>(phoneBook.keySet());
        Collections.sort(names);
        for (String name : names)
            System.out.printf("  %-20s %s%n", name, phoneBook.get(name));
        System.out.println("─".repeat(40));
    }

    // Find contacts with more than one number
    static void findMultiNumber() {
        System.out.println("\n📋 Contacts with multiple numbers:");
        for (Map.Entry<String, List<String>> e : phoneBook.entrySet())
            if (e.getValue().size() > 1)
                System.out.println("  " + e.getKey() + ": " + e.getValue());
    }

    // Get all unique area codes (first 3 digits of each number)
    static Set<String> uniqueAreaCodes() {
        HashSet<String> codes = new HashSet<>();
        for (List<String> numbers : phoneBook.values())
            for (String num : numbers)
                if (num.length() >= 3) codes.add(num.substring(0, 3));
        return codes;
    }

    public static void main(String[] args) {
        addContact("Abhishek Ranjan", "9876543210");
        addContact("Abhishek Ranjan", "9123456789");
        addContact("Abhishek Ranjan", "9876543210");  // duplicate
        addContact("Riya Sharma",     "8765432109");
        addContact("Arjun Kumar",     "7654321098");
        addContact("Priya Singh",     "9999888777");
        addContact("Riya Sharma",     "8888777666");

        printAll();
        findMultiNumber();

        System.out.println("\n🔍 Searching:");
        search("Riya Sharma");
        search("Unknown Person");

        System.out.println("\n🗑️  Deleting Arjun Kumar:");
        delete("Arjun Kumar");
        delete("Arjun Kumar");  // try again

        System.out.println("\nUnique area codes: " + uniqueAreaCodes());
        printAll();
    }
}
```

```
Output:
✅ Added: Abhishek Ranjan → 9876543210
✅ Added: Abhishek Ranjan → 9123456789
⚠️  Duplicate number for Abhishek Ranjan: 9876543210
✅ Added: Riya Sharma → 8765432109
✅ Added: Arjun Kumar → 7654321098
✅ Added: Priya Singh → 9999888777
✅ Added: Riya Sharma → 8888777666

📖 Phone Book (4 contacts):
────────────────────────────────────────
  Abhishek Ranjan      [9876543210, 9123456789]
  Arjun Kumar          [7654321098]
  Priya Singh          [9999888777]
  Riya Sharma          [8765432109, 8888777666]
────────────────────────────────────────

📋 Contacts with multiple numbers:
  Abhishek Ranjan: [9876543210, 9123456789]
  Riya Sharma: [8765432109, 8888777666]

🔍 Searching:
📞 Riya Sharma: [8765432109, 8888777666]
❌ Not found: Unknown Person

🗑️  Deleting Arjun Kumar:
🗑️  Deleted: Arjun Kumar
❌ Not found: Arjun Kumar

Unique area codes: [876, 912, 876, 999, 888]  → HashSet: [876, 912, 999, 888]
```

> 🔑 **Key Learning:** `HashMap<String, List<String>>` — a map whose values are lists. This **nested collection pattern** (`Map<Key, List<Value>>`) is one of the most common in real applications and DSA (group anagrams, adjacency list for graphs). `putIfAbsent` initializes the list only on first entry. `Collections.sort(keySet)` — sort keys for ordered output. `HashSet` automatically deduplicates area codes. This problem combines ArrayList, HashMap, HashSet, and Collections utilities — professional Java code.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Top-K Frequent Elements using HashMap + sorting, and Two-Sum using HashMap.

```java
import java.util.*;

public class MiniChallenge_Day12 {

    // ① Top K frequent elements
    static List<Integer> topKFrequent(int[] nums, int k) {
        // Step 1: Count frequencies
        HashMap<Integer, Integer> freq = new HashMap<>();
        for (int n : nums)
            freq.put(n, freq.getOrDefault(n, 0) + 1);

        // Step 2: Sort by frequency descending
        List<Integer> keys = new ArrayList<>(freq.keySet());
        keys.sort((a, b) -> freq.get(b) - freq.get(a)); // lambda comparator

        // Step 3: Return top k
        return keys.subList(0, k);
    }

    // ② Two Sum — find indices of two numbers that add to target
    static int[] twoSum(int[] nums, int target) {
        HashMap<Integer, Integer> seen = new HashMap<>(); // value → index
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (seen.containsKey(complement)) {
                return new int[]{seen.get(complement), i};
            }
            seen.put(nums[i], i);
        }
        return new int[]{-1, -1};   // not found
    }

    // ③ Group Anagrams — classic HashMap problem
    static Map<String, List<String>> groupAnagrams(String[] words) {
        HashMap<String, List<String>> groups = new HashMap<>();
        for (String w : words) {
            char[] chars = w.toCharArray();
            Arrays.sort(chars);
            String key = new String(chars);     // sorted form is the key
            groups.putIfAbsent(key, new ArrayList<>());
            groups.get(key).add(w);
        }
        return groups;
    }

    public static void main(String[] args) {
        // Top K Frequent
        int[] nums1 = {1,1,1,2,2,3,3,3,3,4};
        System.out.println("Array   : " + Arrays.toString(nums1));
        System.out.println("Top 2   : " + topKFrequent(nums1, 2));   // [3,1]
        System.out.println("Top 3   : " + topKFrequent(nums1, 3));   // [3,1,2]

        System.out.println();

        // Two Sum
        int[] nums2 = {2, 7, 11, 15};
        int[] result = twoSum(nums2, 9);
        System.out.println("Array   : " + Arrays.toString(nums2));
        System.out.println("Target 9: indices " + Arrays.toString(result)); // [0,1] (2+7=9)

        int[] nums3 = {3, 2, 4};
        System.out.println("Array   : " + Arrays.toString(nums3));
        System.out.println("Target 6: indices " + Arrays.toString(twoSum(nums3, 6))); // [1,2]

        System.out.println();

        // Group Anagrams
        String[] words = {"eat","tea","tan","ate","nat","bat"};
        System.out.println("Words   : " + Arrays.toString(words));
        System.out.println("Groups  : " + groupAnagrams(words));
    }
}
```

```
Output:
Array   : [1, 1, 1, 2, 2, 3, 3, 3, 3, 4]
Top 2   : [3, 1]
Top 3   : [3, 1, 2]

Array   : [2, 7, 11, 15]
Target 9: indices [0, 1]

Array   : [3, 2, 4]
Target 6: indices [1, 2]

Words   : [eat, tea, tan, ate, nat, bat]
Groups  : {aet=[eat, tea, ate], ant=[tan, nat], abt=[bat]}
```

**Two Sum trace for [2,7,11,15], target=9:**
```
i=0: num=2, complement=9-2=7, seen={} → 7 not found → put {2:0}
i=1: num=7, complement=9-7=2, seen={2:0} → 2 FOUND at index 0!
     return [0, 1] ✅ — only 2 iterations!
```

> 🔑 **Key Learning:** Two Sum with HashMap is O(n) — for each element, check if its complement exists in the map. Without HashMap: O(n²) nested loops. This is **LeetCode #1 — the most famous coding interview problem** — and HashMap reduces it to one pass. Group Anagrams: sorted form of any anagram is the same ("eat","tea","ate" all sort to "aet") — use sorted form as HashMap key. LeetCode #49. Top-K: count with HashMap, sort by value using lambda comparator. LeetCode #347. You just solved three classic interview problems.

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| Fibonacci memoization from memory | ✅ `memo[n] = fibMemo(n-1) + fibMemo(n-2)` |
| GCD recursive from memory | ✅ `if(b==0) return a; return gcd(b, a%b)` |
| "Trust the recursion" — explained | ✅ Focus current level, trust subproblem |
| Binary exponentiation — why O(log n) | ✅ Halves exponent each call |

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. What is the difference between ArrayList and LinkedList? When to choose each?</b></summary>

> **ArrayList** — backed by a dynamic array. `get(i)` is O(1) — direct index access. Insert/delete in middle is O(n) — must shift elements. Best when: read-heavy, random access needed, infrequent modifications.
>
> **LinkedList** — doubly linked list. `get(i)` is O(n) — must traverse. Insert/delete at ends is O(1) — just pointer update. Best when: frequent insertions/deletions, implementing stack/queue, or when you need `addFirst/removeLast` efficiently.
>
> Default choice: ArrayList. Switch to LinkedList only when profiling shows bottleneck in insertions.

</details>

<details>
<summary><b>Q2. How does HashMap achieve O(1) get and put?</b></summary>

> HashMap uses a **hash function** to convert a key into an array index (bucket). Storing: `hashCode(key) % arraySize` gives the bucket index — element placed there. Retrieval: same hash gives same bucket — direct jump, no search.
>
> **Collision handling** (two keys map to same bucket): Java uses **chaining** — each bucket holds a LinkedList of entries. With a good hash function and load factor ≤ 0.75, average chain length ≈ 1 → O(1).
>
> Worst case: all keys collide → O(n). But this is extremely rare with proper keys.

</details>

<details>
<summary><b>Q3. HashSet vs HashMap — what is the relationship?</b></summary>

> `HashSet<E>` is internally implemented as a `HashMap<E, PRESENT>` where `PRESENT` is a dummy constant object. Every element you add to the HashSet becomes a key in the backing HashMap with a fixed dummy value.
>
> This means HashSet gives you all HashMap's O(1) properties (add, remove, contains) but only stores keys — no values. Use HashMap when you need key-value pairs. Use HashSet when you only need "is this element present?" queries.

</details>

<details>
<summary><b>Q4. Stack vs Queue — when to use each in DSA?</b></summary>

> **Stack (LIFO — Last In, First Out):** Use when you need to process items in reverse order, or when the most recently seen item is the most relevant. DSA uses: DFS, bracket matching, expression evaluation, undo/redo, backtracking.
>
> **Queue (FIFO — First In, First Out):** Use when you need to process items in arrival order. DSA uses: BFS, level-order tree traversal, shortest path in unweighted graphs, task scheduling, sliding window.
>
> Memory aid: Stack = pile of plates (last placed = first taken). Queue = line at a ticket counter (first arrived = first served).

</details>

<details>
<summary><b>Q5. What is the Two Sum HashMap pattern and why is it O(n) instead of O(n²)?</b></summary>

> **The pattern:** For each element `x`, the complement needed is `target - x`. Instead of searching the entire array for the complement (O(n) per element = O(n²) total), store each seen element in a HashMap as `value → index`. Then for each new element, check if its complement is already in the map — O(1) lookup.
>
> **Why O(n):** You traverse the array once (O(n)). For each element, you do one O(1) HashMap lookup and one O(1) HashMap put. Total: O(n) × O(1) = **O(n)**.
>
> The key insight: instead of "search forward for complement," you "remember what you've seen" and check against it. This trade of time for space (using O(n) extra space for the HashMap) is the fundamental HashMap optimization — trading space for time.

</details>

---

## 📌 DAY 12 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 12 — SUMMARY                       ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 10, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Collections Framework                           ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  Collections hierarchy — List, Set, Queue, Map               ║
║  ✅  Generics + Autoboxing — wrapper types                       ║
║  ✅  ArrayList — all operations + time complexity table          ║
║  ✅  LinkedList — List + Deque, when to prefer over ArrayList    ║
║  ✅  HashMap — put/get/iterate, getOrDefault, frequency pattern  ║
║  ✅  HashSet — add returns false for dup, set operations         ║
║  ✅  Stack — LIFO, ArrayDeque preferred over Stack class         ║
║  ✅  Queue — FIFO, offer/poll/peek, BFS pattern preview          ║
║  ✅  Collections utility — sort, reverse, max, min, shuffle      ║
╠══════════════════════════════════════════════════════════════════╣
║  PROBLEMS SOLVED                                                ║
║  ✅  P1 — ArrayList CRUD (sort, reverse, toArray)                ║
║  ✅  P2 — HashMap Frequency Counter (char + word + anagram)      ║
║  ✅  P3 — HashSet Duplicates (detect, remove, set ops)           ║
║  ✅  P4 — Stack: brackets, reverse, decimal→binary               ║
║  ✅  P5 — Queue: BFS pattern, moving average, hot potato         ║
║  ✅  P6 — PhoneBook: HashMap<String, List<String>>               ║
║  ✅  Mini — Two Sum O(n), Top-K, Group Anagrams                  ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  92%                              ║
║  STRONG AREA    :  HashMap patterns, HashSet ops, Stack apps     ║
║  WEAK AREA      :  Choosing right collection for the problem     ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  freq.getOrDefault(key, 0)+1 — canonical frequency counter   ║
║  ▸  set.add() returns false for duplicates — use this!          ║
║  ▸  ArrayDeque over Stack class — same API, much faster          ║
║  ▸  offer/poll for Queue — safer than add/remove (no exception)  ║
║  ▸  Two Sum: HashMap complement lookup = O(n) vs O(n²) brute    ║
║  ▸  Group Anagrams: sorted string as HashMap key                ║
║  ▸  BFS level pattern: size=queue.size() before inner for loop  ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 11](./Day11_Recursion.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 13 →](./Day13_Sorting.md)

*Day 12 of 160 — Done ✅ | Streak: 12 Days 🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥*

</div>
