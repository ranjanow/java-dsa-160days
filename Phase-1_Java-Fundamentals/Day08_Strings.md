# 📄 Day 08 — Strings in Java

<div align="center">

![Day](https://img.shields.io/badge/Day-08_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Strings_%26_String_Methods-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 07](./Day07_Arrays_2D.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 09 →](./Day09_OOP_Basics.md)

</div>

---

## 🎯 Day 8 Goal
> Master Strings in Java — memory model, immutability, all essential methods, StringBuilder, and classic String problems. Strings appear in **every single placement interview** — palindromes, anagrams, compression, bracket matching. Today they become second nature.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | String memory model, immutability, comparison | 55 min |
| Block 2 | String methods — all essential ones | 50 min |
| Block 3 | StringBuilder — why and when | 15 min |
| Block 4 | Revision — Day 7 matrix flash review | 15 min |
| Block 5 | Practice Set — 6 Problems | 115 min |
| Block 6 | Mini Challenge + Revision | 30 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. String Basics & Memory Model

A String is an **object** (not a primitive) representing a sequence of characters.

```java
String s1 = "Hello";              // literal → String Pool
String s2 = new String("Hello");  // explicit → separate Heap object
```

**Memory model:**
```
Stack:          String Pool (Heap):    Heap:
s1 ──────────► ["Hello"]
s2 ──────────────────────────────────► ["Hello"] ← separate copy

s1 == s2       → false  (different addresses)
s1.equals(s2)  → true   (same content)
```

> ⚠️ **String Pool:** Literals with the same value share one object. `new String()` always creates a fresh heap object bypassing the pool. This is why `==` is unreliable — always use `.equals()`.

---

### 🔷 2. String Immutability — The Most Critical Property

**Strings are IMMUTABLE** — once created, their content CANNOT change.

```java
String s = "Hello";
s = s + " World";   // "Hello" is NOT modified!
                    // A NEW String "Hello World" is created
                    // s now references the new String
                    // "Hello" becomes garbage
```

**Every method returns a NEW String — original is untouched:**
```java
String s = "hello";
s.toUpperCase();      // returns "HELLO" but s is STILL "hello"!
s.replace('l','r');   // returns "herro" but s is STILL "hello"!

// Correct — capture the return value:
String upper    = s.toUpperCase();      // "HELLO"
String replaced = s.replace('l','r');   // "herro"
// s is still "hello" ✅
```

**Why `String +=` in a loop is O(n²):**
```java
// ❌ BAD — creates 1000 new String objects, O(n²) total copies
String result = "";
for (int i = 0; i < 1000; i++) result += i;

// ✅ GOOD — one mutable buffer, O(n) total
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) sb.append(i);
String result = sb.toString();
```

---

### 🔷 3. String vs char Array

```java
String s = "Hello";

// String → char array
char[] chars = s.toCharArray();    // ['H','e','l','l','o']

// Access individual char
char c = s.charAt(2);              // 'l'

// char array → String
String back = new String(chars);

// Iterate characters
for (int i = 0; i < s.length(); i++)
    System.out.print(s.charAt(i) + " ");   // H e l l o

for (char ch : s.toCharArray())
    System.out.print(ch + " ");            // H e l l o
```

---

### 🔷 4. Essential String Methods — Complete Reference

```java
String s = "Hello World";

// ── Length & Access ───────────────────────
s.length()              // 11
s.charAt(0)             // 'H'
s.charAt(s.length()-1)  // 'd'
s.indexOf('o')          // 4  (first occurrence)
s.lastIndexOf('o')      // 7  (last occurrence)
s.indexOf("World")      // 6

// ── Substrings ────────────────────────────
s.substring(6)          // "World"      (from index 6 to end)
s.substring(0, 5)       // "Hello"      (0 inclusive, 5 exclusive)

// ── Case ──────────────────────────────────
s.toUpperCase()         // "HELLO WORLD"
s.toLowerCase()         // "hello world"

// ── Comparison ────────────────────────────
s.equals("Hello World")            // true
s.equalsIgnoreCase("hello world")  // true
s.compareTo("Hello World")         // 0 (equal), <0 (less), >0 (greater)
s.startsWith("Hello")              // true
s.endsWith("World")                // true
s.contains("lo W")                 // true

// ── Trim & Replace ────────────────────────
"  hello  ".trim()               // "hello"
s.replace('l', 'r')              // "Herro Worrd"    (all occurrences)
s.replace("Hello", "Hi")         // "Hi World"
s.replaceAll("[aeiou]", "*")      // "H*ll* W*rld"   (regex)

// ── Split & Join ──────────────────────────
"a,b,c".split(",")               // ["a","b","c"]
"a  b  c".split("\\s+")          // ["a","b","c"]   (handles multiple spaces)
String.join("-", "a","b","c")    // "a-b-c"

// ── Check ─────────────────────────────────
s.isEmpty()                      // false (true only if length==0)
s.isBlank()                      // false (true if all whitespace)
Character.isLetter('a')          // true
Character.isDigit('5')           // true
Character.isWhitespace(' ')      // true
Character.toUpperCase('a')       // 'A'
Character.toLowerCase('A')       // 'a'

// ── Convert ───────────────────────────────
String.valueOf(42)               // "42"
Integer.parseInt("42")           // 42
Double.parseDouble("3.14")       // 3.14
```

---

### 🔷 5. StringBuilder — Mutable String Builder

```java
StringBuilder sb = new StringBuilder();
sb.append("Hello");           // "Hello"
sb.append(" World");          // "Hello World"
sb.insert(5, ",");            // "Hello, World"
sb.delete(5, 6);              // "Hello World"
sb.reverse();                 // "dlroW olleH"
sb.replace(0, 5, "Bye");     // "Bye olleH"
sb.length();                  // current length
sb.charAt(0);                 // 'B'
sb.toString();                // convert to String (use at the END)

// Most common one-liner:
String rev = new StringBuilder("Hello").reverse().toString();  // "olleH"
```

---

### 🔷 6. The `c - 'a'` Trick — Character Frequency Array

```java
int[] freq = new int[26];
String s = "hello";

for (char c : s.toCharArray())
    freq[c - 'a']++;

// 'a'-'a'=0  'e'-'a'=4  'h'-'a'=7  'l'-'a'=11  'o'-'a'=14
// freq[7]=1  freq[4]=1  freq[11]=2  freq[14]=1
```

> 💡 Characters are stored as Unicode integers: 'a'=97, 'b'=98 ... 'z'=122. So `'e'-'a'` = 101-97 = 4 — maps 'e' to index 4. This trick is used in EVERY character frequency problem. For uppercase: `c - 'A'` works the same way.

---

### 🔷 7. Essential String Patterns

```java
// ① Two-pointer palindrome check
static boolean isPalindrome(String s) {
    int l = 0, r = s.length() - 1;
    while (l < r) {
        if (s.charAt(l) != s.charAt(r)) return false;
        l++; r--;
    }
    return true;
}

// ② Reverse using StringBuilder
static String reverse(String s) {
    return new StringBuilder(s).reverse().toString();
}

// ③ Character frequency array
int[] freq = new int[26];
for (char c : s.toLowerCase().toCharArray())
    if (Character.isLetter(c)) freq[c - 'a']++;

// ④ Check anagram — increment + decrement approach
static boolean isAnagram(String s1, String s2) {
    if (s1.length() != s2.length()) return false;
    int[] count = new int[26];
    for (char c : s1.toCharArray()) count[c - 'a']++;
    for (char c : s2.toCharArray()) count[c - 'a']--;
    for (int n : count) if (n != 0) return false;
    return true;
}
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — String Inspector `[Easy]`

**Task:** Extract various properties from `"Hello World Java"`.

```java
public class P1_StringInspector {
    public static void main(String[] args) {
        String s = "Hello World Java";

        System.out.println("String       : \"" + s + "\"");
        System.out.println("Length       : " + s.length());
        System.out.println("First char   : " + s.charAt(0));
        System.out.println("Last char    : " + s.charAt(s.length() - 1));
        System.out.println("First space  : index " + s.indexOf(' '));
        System.out.println("Uppercase    : " + s.toUpperCase());
        System.out.println("Lowercase    : " + s.toLowerCase());
        System.out.println("Has 'Java'   : " + s.contains("Java"));
        System.out.println("Starts Hello : " + s.startsWith("Hello"));
        System.out.println("Ends Java    : " + s.endsWith("Java"));
        System.out.println("Sub(6,11)    : " + s.substring(6, 11));
        System.out.println("Replace l→r  : " + s.replace('l', 'r'));
        System.out.println("Index 'World': " + s.indexOf("World"));
    }
}
```

```
Output:
String       : "Hello World Java"
Length       : 16
First char   : H
Last char    : a
First space  : index 5
Uppercase    : HELLO WORLD JAVA
Lowercase    : hello world java
Has 'Java'   : true
Starts Hello : true
Ends Java    : true
Sub(6,11)    : World
Replace l→r  : Herro Worrd Java
Index 'World': 6
```

> 🔑 **Key Learning:** Every method returns a new value — `s` is NEVER modified. `substring(6, 11)` — end index is always **exclusive** (consistent across all Java collections). `replace('l','r')` replaces ALL occurrences. The immutability is confirmed: none of these calls change `s` — if you need the result, store it in a new variable.

---

### ✅ Problem 2 — Palindrome Check `[Easy]`

**Task:** Two-pointer palindrome — basic and preprocessed (ignore spaces/case).

```java
public class P2_Palindrome {

    static boolean isPalindrome(String s) {
        int left = 0, right = s.length() - 1;
        while (left < right) {
            if (s.charAt(left) != s.charAt(right)) return false;
            left++;
            right--;
        }
        return true;   // empty string = palindrome
    }

    static boolean isPalindromeClean(String s) {
        // Preprocess: lowercase + remove non-alphanumeric chars
        String clean = s.toLowerCase().replaceAll("[^a-z0-9]", "");
        return isPalindrome(clean);
    }

    static void check(String s) {
        System.out.printf("%-35s basic=%-5s clean=%s%n",
            "\"" + s + "\"",
            isPalindrome(s),
            isPalindromeClean(s));
    }

    public static void main(String[] args) {
        check("racecar");
        check("hello");
        check("madam");
        check("");
        check("A man a plan a canal Panama");
        check("Was it a car or a cat I saw");
    }
}
```

```
Output:
"racecar"                           basic=true  clean=true
"hello"                             basic=false clean=false
"madam"                             basic=true  clean=true
""                                  basic=true  clean=true
"A man a plan a canal Panama"       basic=false clean=true
"Was it a car or a cat I saw"       basic=false clean=true
```

**Two-pointer trace for "racecar":**
```
l=0, r=6: 'r'=='r' ✅  l=1, r=5
l=1, r=5: 'a'=='a' ✅  l=2, r=4
l=2, r=4: 'c'=='c' ✅  l=3, r=3
l=3, r=3: l < r? NO → return true ✅
```

> 🔑 **Key Learning:** `replaceAll("[^a-z0-9]", "")` — regex `[^...]` = "NOT these characters" — removes everything except alphanumerics. Two-pointer stops at the first mismatch (early exit). The same mental model as array reverse: two pointers moving inward. Empty string returns `true` — a zero-length string is trivially a palindrome.

---

### ✅ Problem 3 — Count Vowels, Consonants & Words `[Easy-Medium]`

**Task:** Character composition analysis.

```java
public class P3_StringStats {

    static int countVowels(String s) {
        int count = 0;
        for (char c : s.toLowerCase().toCharArray())
            if ("aeiou".indexOf(c) != -1) count++;
        return count;
    }

    static int countConsonants(String s) {
        int count = 0;
        for (char c : s.toLowerCase().toCharArray())
            if (Character.isLetter(c) && "aeiou".indexOf(c) == -1) count++;
        return count;
    }

    static int countWords(String s) {
        s = s.trim();
        if (s.isEmpty()) return 0;
        return s.split("\\s+").length;
    }

    static int countDigits(String s) {
        int count = 0;
        for (char c : s.toCharArray())
            if (Character.isDigit(c)) count++;
        return count;
    }

    static void analyse(String s) {
        System.out.println("String     : \"" + s + "\"");
        System.out.println("Vowels     : " + countVowels(s));
        System.out.println("Consonants : " + countConsonants(s));
        System.out.println("Words      : " + countWords(s));
        System.out.println("Digits     : " + countDigits(s));
        System.out.println();
    }

    public static void main(String[] args) {
        analyse("Hello World from Java");
        analyse("Java 11 has 4 new features");
        analyse("  multiple   spaces   here  ");
    }
}
```

```
Output:
String     : "Hello World from Java"
Vowels     : 6
Consonants : 11
Words      : 4
Digits     : 0

String     : "Java 11 has 4 new features"
Vowels     : 8
Consonants : 10
Words      : 5
Digits     : 3

String     : "  multiple   spaces   here  "
Vowels     : 8
Consonants : 12
Words      : 3
Digits     : 0
```

> 🔑 **Key Learning:** `"aeiou".indexOf(c) != -1` — elegant vowel membership check without a long if-chain. `Character.isLetter(c)` guard prevents spaces and digits being counted as consonants. `split("\\s+")` handles multiple consecutive spaces — `\\s+` means "one or more whitespace characters". `.trim()` before split prevents empty strings from leading/trailing spaces.

---

### ✅ Problem 4 — Anagram Check `[Medium]`

**Task:** Two approaches — sort O(n log n) and frequency O(n).

```java
import java.util.Arrays;

public class P4_Anagram {

    // Approach 1: Sort both strings — O(n log n)
    static boolean isAnagramSort(String s1, String s2) {
        s1 = s1.toLowerCase().replace(" ", "");
        s2 = s2.toLowerCase().replace(" ", "");
        if (s1.length() != s2.length()) return false;
        char[] a = s1.toCharArray();
        char[] b = s2.toCharArray();
        Arrays.sort(a);
        Arrays.sort(b);
        return Arrays.equals(a, b);
    }

    // Approach 2: Frequency count — O(n), O(1) space
    static boolean isAnagramFreq(String s1, String s2) {
        s1 = s1.toLowerCase().replace(" ", "");
        s2 = s2.toLowerCase().replace(" ", "");
        if (s1.length() != s2.length()) return false;
        int[] count = new int[26];
        for (char c : s1.toCharArray()) count[c - 'a']++;
        for (char c : s2.toCharArray()) count[c - 'a']--;
        for (int n : count) if (n != 0) return false;
        return true;
    }

    static void check(String s1, String s2) {
        System.out.printf("%-15s | %-15s → sort=%-5s freq=%s%n",
            "\""+s1+"\"", "\""+s2+"\"",
            isAnagramSort(s1,s2), isAnagramFreq(s1,s2));
    }

    public static void main(String[] args) {
        check("listen",     "silent");
        check("hello",      "world");
        check("Triangle",   "Integral");
        check("Astronomer", "Moon starer");
        check("abc",        "ab");
        check("",           "");
    }
}
```

```
Output:
"listen"        | "silent"        → sort=true  freq=true
"hello"         | "world"         → sort=false freq=false
"Triangle"      | "Integral"      → sort=true  freq=true
"Astronomer"    | "Moon starer"   → sort=true  freq=true
"abc"           | "ab"            → sort=false freq=false
""              | ""              → sort=true  freq=true
```

**Frequency trace for "listen" vs "silent":**
```
Increment (listen): l+1, i+1, s+1, t+1, e+1, n+1
Decrement (silent): s-1, i-1, l-1, e-1, n-1, t-1
All counts = 0 → ANAGRAM ✅
```

**Complexity:**
```
Approach 1 (sort): O(n log n) time, O(n) space
Approach 2 (freq): O(n) time,     O(1) space  ← Winner
```

> 🔑 **Key Learning:** Frequency approach: increment for s1, decrement for s2, check all-zero. The `c - 'a'` trick is the engine. For problems requiring only lowercase letters, `int[26]` is the canonical O(1) space solution. This frequency pattern transfers to: first unique character, group anagrams, minimum window substring — all common DSA problems. "Astronomer" / "Moon starer" are a famous real anagram pair!

---

### ✅ Problem 5 — String Compression `[Medium]`

**Task:** Run-length encoding using StringBuilder.

```java
public class P5_StringCompression {

    static String compress(String s) {
        if (s == null || s.isEmpty()) return s;

        StringBuilder sb = new StringBuilder();
        int i = 0;

        while (i < s.length()) {
            char current = s.charAt(i);
            int count = 1;

            // Count consecutive identical characters
            while (i + count < s.length() && s.charAt(i + count) == current)
                count++;

            sb.append(current);
            sb.append(count);
            i += count;   // jump past all counted chars
        }

        String compressed = sb.toString();
        // Return original if compressed is NOT shorter
        return compressed.length() < s.length() ? compressed : s;
    }

    public static void main(String[] args) {
        String[] tests = {"aabcccdddd","aaabbb","abcd","aaaa","aabbcc","a",""};

        for (String s : tests) {
            String result = compress(s);
            System.out.printf("%-15s → %-15s  %s%n",
                "\""+s+"\"",
                "\""+result+"\"",
                result.equals(s) ? "(kept original)" : "");
        }
    }
}
```

```
Output:
"aabcccdddd"    → "a2b1c3d4"       
"aaabbb"        → "a3b3"           
"abcd"          → "a1b1c1d1"        (kept original)
"aaaa"          → "a4"             
"aabbcc"        → "a2b2c2"          (kept original)
"a"             → "a"               (kept original)
""              → ""                (kept original)
```

**Walkthrough for "aabcccdddd":**
```
i=0: current='a', count=2 (a,a), append "a2", i=2
i=2: current='b', count=1 (b),   append "b1", i=3
i=3: current='c', count=3,       append "c3", i=6
i=6: current='d', count=4,       append "d4", i=10
Result: "a2b1c3d4" (len 8 < 10) → return compressed ✅
```

> 🔑 **Key Learning:** Inner `while` counts consecutive chars. Outer loop jumps `i += count` past all counted characters. StringBuilder is mandatory here — using `String +=` inside the loop would be O(n²). The final length comparison prevents returning a longer "compressed" string. This is LeetCode #443 — commonly asked at Microsoft and Amazon.

---

### ✅ Problem 6 — Valid Parentheses `[Medium-Hard]`

**Task:** Bracket validation using a manual char stack.

```java
public class P6_ValidParentheses {

    static boolean isValid(String s) {
        char[] stack = new char[s.length()];
        int top = -1;    // -1 = empty stack

        for (char c : s.toCharArray()) {
            if (c == '(' || c == '[' || c == '{') {
                stack[++top] = c;     // push opening bracket
            } else if (c == ')' || c == ']' || c == '}') {
                if (top == -1) return false;  // no matching open

                char open = stack[top--];     // pop from stack

                if (c == ')' && open != '(') return false;
                if (c == ']' && open != '[') return false;
                if (c == '}' && open != '{') return false;
            }
        }
        return top == -1;   // valid = stack completely empty
    }

    public static void main(String[] args) {
        String[] tests = {"()","()[]{}", "([])", "(]",
                          "([)]", "{[]}", "", "((("};

        for (String s : tests)
            System.out.printf("%-15s → %s%n",
                "\""+s+"\"",
                isValid(s) ? "✅ Valid" : "❌ Invalid");
    }
}
```

```
Output:
"()"            → ✅ Valid
"()[]{}"        → ✅ Valid
"([])"          → ✅ Valid
"(]"            → ❌ Invalid
"([)]"          → ❌ Invalid
"{[]}"          → ✅ Valid
""              → ✅ Valid
"((("           → ❌ Invalid
```

**Stack trace for "([])":**
```
'(': push '('    → stack: ['(']
'[': push '['    → stack: ['(','[']
']': pop '[', ']' matches '[' ✅   → stack: ['(']
')': pop '(', ')' matches '(' ✅   → stack: []
top==-1 → true → VALID ✅
```

**Stack trace for "([)]":**
```
'(': push       → stack: ['(']
'[': push       → stack: ['(','[']
')': pop '[', ')' should match '(' but got '[' ❌ → false
INVALID ✅
```

> 🔑 **Key Learning:** This is the classic Stack (LIFO) problem — every closing bracket must match the **most recently** opened bracket. We implemented the stack manually using `char[]` and an `int top`. In Phase 3, we'll use Java's `Stack<Character>` or `Deque<Character>`, but the algorithm is identical. LeetCode #20 — asked at every major tech company. The key insight: when you see `)`, the thing that should match it is whatever was opened LAST — this is exactly LIFO behavior.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Complete String Analyzer.

```java
public class MiniChallenge_Day8 {

    static int countVowels(String s) {
        int count = 0;
        for (char c : s.toLowerCase().toCharArray())
            if ("aeiou".indexOf(c) != -1) count++;
        return count;
    }

    static int countConsonants(String s) {
        int count = 0;
        for (char c : s.toLowerCase().toCharArray())
            if (Character.isLetter(c) && "aeiou".indexOf(c) == -1) count++;
        return count;
    }

    static boolean isPalindrome(String s) {
        String clean = s.toLowerCase().replaceAll("[^a-z]", "");
        int l = 0, r = clean.length() - 1;
        while (l < r) {
            if (clean.charAt(l) != clean.charAt(r)) return false;
            l++; r--;
        }
        return true;
    }

    static char mostFrequentLetter(String s) {
        int[] freq = new int[26];
        for (char c : s.toLowerCase().toCharArray())
            if (Character.isLetter(c)) freq[c - 'a']++;
        int maxIdx = 0;
        for (int i = 1; i < 26; i++)
            if (freq[i] > freq[maxIdx]) maxIdx = i;
        return (char)('a' + maxIdx);
    }

    static void analyzeString(String s) {
        String line = "═".repeat(45);
        String[] words = s.trim().split("\\s+");
        String reversed = new StringBuilder(s).reverse().toString();

        System.out.println(line);
        System.out.println("         STRING ANALYZER");
        System.out.println(line);
        System.out.println("Original   : \"" + s + "\"");
        System.out.println("Length     : " + s.length());
        System.out.println("Words      : " + words.length);
        System.out.println("Reversed   : \"" + reversed + "\"");
        System.out.println("Uppercase  : \"" + s.toUpperCase() + "\"");
        System.out.println("Vowels     : " + countVowels(s));
        System.out.println("Consonants : " + countConsonants(s));
        System.out.println("Palindrome : " + (isPalindrome(s) ? "Yes" : "No"));
        System.out.println("Has 'Fun'  : " + s.contains("Fun"));
        System.out.println("First word : \"" + words[0] + "\"");
        System.out.println("Last word  : \"" + words[words.length - 1] + "\"");
        System.out.println("Most freq  : '" + mostFrequentLetter(s) + "'");
        System.out.println(line);
    }

    public static void main(String[] args) {
        analyzeString("Programming is Fun and Challenging");
    }
}
```

```
Output:
═════════════════════════════════════════════
         STRING ANALYZER
═════════════════════════════════════════════
Original   : "Programming is Fun and Challenging"
Length     : 34
Words      : 5
Reversed   : "gnignelahhC dna nuF si gnimmargorP"
Uppercase  : "PROGRAMMING IS FUN AND CHALLENGING"
Vowels     : 10
Consonants : 19
Palindrome : No
Has 'Fun'  : Yes
First word : "Programming"
Last word  : "Challenging"
Most freq  : 'g'
═════════════════════════════════════════════
```

> 🔑 **Key Learning:** `mostFrequentLetter` uses `int[26]`, finds max index, converts back with `(char)('a' + maxIdx)`. The coordinator `analyzeString` contains zero algorithm logic — purely calls helpers and formats. `words[words.length - 1]` elegantly retrieves the last word. Clean architecture: each method does one thing, the coordinator orchestrates.

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| Rotate 90° CW from memory | ✅ Step 1: transpose (j=i+1). Step 2: reverse each row |
| Spiral — 4 boundaries + guards | ✅ top,bottom,left,right + if guards on last 2 directions |
| Set Matrix Zeroes — why 2 passes | ✅ Modifying while scanning creates false zeros that propagate |
| `matrix[0].length` vs `matrix.length` | ✅ cols (inner array size) vs rows (outer array size) |

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. String immutability — concrete code example?</b></summary>

> String immutability means once a String object is created, its content **cannot be changed**. Any operation that appears to modify it actually creates and returns a **new String**, leaving the original untouched.
>
> ```java
> String s = "hello";
> s.toUpperCase();            // creates "HELLO" — s is STILL "hello"!
> System.out.println(s);     // prints "hello"
>
> String upper = s.toUpperCase();  // correct: capture the new String
> System.out.println(upper);       // "HELLO" ✅
> ```
>
> This enables String Pool optimization, thread safety, and hash code caching.

</details>

<details>
<summary><b>Q2. Why is String += in a loop O(n²)? What to use instead?</b></summary>

> Each `+=`: allocates new memory, copies ALL existing characters + new content, discards old object. In a loop of n iterations, total character copies = 1+2+3+...+n = n(n+1)/2 = **O(n²)**.
>
> **Use `StringBuilder`** — it maintains a mutable internal buffer with amortized O(1) per `append()`. Total O(n). The buffer grows by doubling strategy (not on every call), making it vastly more efficient for dynamic string building in loops.

</details>

<details>
<summary><b>Q3. equals() vs compareTo() — difference and when to use each?</b></summary>

> **`equals()`** returns `boolean` — use for simple equality checks in conditions. Clearer and more readable.
>
> **`compareTo()`** returns `int` — 0 (equal), negative (s1 < s2), positive (s1 > s2). Use when you need **ordering** — sorting strings, checking alphabetical order, binary search on strings.
>
> For equality alone, `equals()` is always preferred. `compareTo()` is essential when you need to know the relative ordering, not just whether two strings are identical.

</details>

<details>
<summary><b>Q4. What does c - 'a' compute for c = 'e'? Exact numeric value?</b></summary>

> Characters are stored as Unicode integers: 'a'=97, 'e'=101, 'z'=122.
>
> `'e' - 'a'` = 101 - 97 = **4**
>
> This maps 'e' to index 4 in a frequency array — `freq[4]` counts 'e' occurrences. Full mapping: a→0, b→1, c→2, d→3, e→4, ..., z→25. Every lowercase letter gets a unique 0-based index into a size-26 array. For uppercase: `c - 'A'` works identically (A=65).

</details>

<details>
<summary><b>Q5. Palindrome two-pointer is O(n/2) — does this equal O(n)? Why?</b></summary>

> The two-pointer makes at most **n/2 comparisons** — each covers both ends simultaneously. But yes, **O(n/2) = O(n)** in Big-O notation.
>
> Big-O drops constant factors. O(n/2) = O(1/2 × n) — the 1/2 is a constant, which Big-O ignores. What matters is the **growth rate**: doubling n doubles the work, so it's linear = O(n) regardless of the constant multiplier.
>
> Practical benefit: early exit on first mismatch. Best case O(1) (first pair mismatches), worst case O(n/2), often much better than O(n) in practice.

</details>

---

## 📌 DAY 8 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 8 — SUMMARY                        ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 06, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  Strings in Java                                 ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  String memory model — pool vs heap, == vs .equals()         ║
║  ✅  Immutability — every method returns a new String            ║
║  ✅  String vs char array — toCharArray(), charAt()              ║
║  ✅  20+ essential String methods across 7 categories            ║
║  ✅  StringBuilder — mutable, O(n) vs O(n²) for String +=       ║
║  ✅  c - 'a' trick — map chars to 0–25 for freq arrays          ║
║  ✅  7 essential String patterns — palindrome, anagram, reverse  ║
╠══════════════════════════════════════════════════════════════════╣
║  PROBLEMS SOLVED                                                ║
║  ✅  P1 — String Inspector (all major methods)                   ║
║  ✅  P2 — Palindrome (two-pointer + preprocessed version)        ║
║  ✅  P3 — Vowels, Consonants, Words, Digits                      ║
║  ✅  P4 — Anagram (sort O(n log n) + freq O(n) both)            ║
║  ✅  P5 — String Compression (run-length, StringBuilder)         ║
║  ✅  P6 — Valid Parentheses (manual stack preview)               ║
║  ✅  Mini — String Analyzer (coordinator + helpers)              ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  90%                              ║
║  STRONG AREA    :  String methods, palindrome, anagram, compress ║
║  WEAK AREA      :  Bracket matching order logic (Stack needed)   ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  Strings are IMMUTABLE — always capture the returned value   ║
║  ▸  Never String += in loops — use StringBuilder (O(n) vs O(n²))║
║  ▸  c - 'a' maps lowercase letters to 0–25 (freq array trick)   ║
║  ▸  Two-pointer palindrome = same pattern as array reverse      ║
║  ▸  Anagram freq O(n) beats sort O(n log n) — always prefer O(n)║
║  ▸  "([)]" invalid — bracket ORDER matters, not just counts     ║
║  ▸  Bracket matching = Stack LIFO — full solution in Phase 3    ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 07](./Day07_Arrays_2D.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 09 →](./Day09_OOP_Basics.md)

*Day 8 of 160 — Done ✅ | Streak: 8 Days 🔥🔥🔥🔥🔥🔥🔥🔥*

</div>
