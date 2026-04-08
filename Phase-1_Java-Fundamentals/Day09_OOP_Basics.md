# 📄 Day 09 — OOP: Classes, Objects & Constructors

<div align="center">

![Day](https://img.shields.io/badge/Day-09_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-OOP_Classes_%26_Objects-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 08](./Day08_Strings.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 10 →](./Day10_OOP_Inheritance.md)

</div>

---

## 🎯 Day 9 Goal
> Master the fundamentals of Object-Oriented Programming — classes, objects, instance variables, methods, constructors, `this` keyword, getters/setters, and static vs instance members. OOP is not just a Java feature — it's a **way of thinking** that every real-world Java application depends on.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | What is OOP, Classes vs Objects, instance variables | 60 min |
| Block 2 | Methods inside classes, `this` keyword | 50 min |
| Block 3 | Constructors — default, parameterized, overloaded | 40 min |
| Block 4 | Revision — Day 8 Strings flash review | 15 min |
| Block 5 | Practice Set — 6 Problems | 115 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. The Big Picture — Why OOP?

OOP bundles **data** (variables) and **behavior** (methods) into a single unit called an **object**.

```
Real World → Code
──────────────────────────────────────
Car        → class Car
  color    →   String color
  speed    →   int speed
  accelerate() → void accelerate()

Student    → class Student
  name     →   String name
  marks[]  →   int[] marks
  getAverage() → double getAverage()
```

**Four pillars of OOP:**
| Pillar | Meaning | Day |
|--------|---------|-----|
| **Encapsulation** | Bundle data + methods, hide internals | Day 9 ✅ |
| **Inheritance** | Child class reuses parent class | Day 10 |
| **Polymorphism** | One interface, many behaviors | Day 10 |
| **Abstraction** | Hide complexity, show essentials | Day 11 |

---

### 🔷 2. Class — The Blueprint

A **class** is a template that defines structure. It holds no data itself — objects do.

```java
public class Student {

    // ① Instance variables — each object gets its own copy
    String name;
    int    age;
    double gpa;

    // ② Instance method — behavior each object can perform
    void introduce() {
        System.out.println("Hi, I'm " + name + ", age " + age);
    }

    void printGPA() {
        System.out.println(name + "'s GPA: " + gpa);
    }
}
```

---

### 🔷 3. Object — An Instance of the Class

An **object** is a concrete instance created from the blueprint using `new`.

```java
Student s1 = new Student();   // create object
Student s2 = new Student();   // separate object

s1.name = "Abhishek";  s1.age = 24;  s1.gpa = 9.2;
s2.name = "Riya";      s2.age = 22;  s2.gpa = 8.7;

s1.introduce();   // Hi, I'm Abhishek, age 24
s2.introduce();   // Hi, I'm Riya, age 22
```

**Memory model:**
```
Stack:         Heap:
s1 ──────►  [ name="Abhishek", age=24, gpa=9.2 ]
s2 ──────►  [ name="Riya",     age=22, gpa=8.7 ]
```

> 💡 s1 and s2 are completely independent — changing `s1.name` has zero effect on `s2.name`.

---

### 🔷 4. The `this` Keyword

`this` refers to the **current object** — the object on which a method is being called.

```java
class Student {
    String name;
    int age;

    // ❌ BAD — parameter shadows instance variable
    void setDetails(String name, int age) {
        name = name;   // assigns parameter to itself! instance var unchanged
        age  = age;    // same bug
    }

    // ✅ GOOD — this. disambiguates
    void setDetails(String name, int age) {
        this.name = name;   // this.name = instance variable
        this.age  = age;    // name = parameter value
    }
}
```

**Three uses of `this`:**
```java
class Student {
    String name;
    int age;

    // Use 1: this.var — disambiguate instance var from parameter
    Student(String name, int age) {
        this.name = name;
        this.age  = age;
    }

    // Use 2: this() — call another constructor
    Student() {
        this("Unknown", 0);   // delegates to parameterized constructor
    }

    // Use 3: return this — for method chaining
    Student setName(String name) {
        this.name = name;
        return this;   // returns current object for chaining
    }

    Student setAge(int age) {
        this.age = age;
        return this;
    }
}

// Method chaining in action:
Student s = new Student().setName("Abhishek").setAge(24);
```

---

### 🔷 5. Constructors — Initialize Objects

A **constructor** runs automatically when `new` is called. It initializes the object.

**Rules:** Same name as class, no return type, called automatically with `new`.

```java
class Car {
    String color;
    int    speed;
    String brand;

    // A. Default constructor (Java provides this if you write none)
    Car() {
        this.color = "White";
        this.speed = 0;
        this.brand = "Unknown";
    }

    // B. Parameterized constructor
    Car(String color, int speed, String brand) {
        this.color = color;
        this.speed = speed;
        this.brand = brand;
    }

    // C. Partial constructor using this()
    Car(String color) {
        this(color, 0, "Unknown");   // delegates to B
    }
}

Car c1 = new Car();                      // uses A
Car c2 = new Car("Red");                 // uses C → delegates to B
Car c3 = new Car("Blue", 200, "BMW");    // uses B
```

> ⚠️ **If you write ANY constructor, Java no longer provides the default one.** You must write it explicitly if needed.

---

### 🔷 6. Getters & Setters — Encapsulation

Make instance variables `private`, expose via controlled methods.

```java
class BankAccount {
    private double balance;      // ← private, hidden from outside

    public double getBalance() { // getter — read access
        return balance;
    }

    public void deposit(double amount) {  // setter with validation
        if (amount > 0) balance += amount;
        else System.out.println("Invalid amount!");
    }

    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) balance -= amount;
        else System.out.println("Insufficient funds!");
    }
}

BankAccount acc = new BankAccount();
// acc.balance = -5000;   ← ❌ compile error — private!
acc.deposit(5000);
System.out.println(acc.getBalance());   // 5000.0
```

---

### 🔷 7. Static vs Instance Members

```java
class Counter {
    int    count;        // instance — each object has its own
    static int total;    // static — SHARED across ALL objects

    void increment() {
        count++;    // this object's count
        total++;    // shared total
    }
}

Counter c1 = new Counter();
Counter c2 = new Counter();
c1.increment(); c1.increment();   // c1.count=2, total=2
c2.increment();                   // c2.count=1, total=3

System.out.println(Counter.total);  // 3 — access via CLASS name
```

| Feature | Instance | Static |
|---------|----------|--------|
| Belongs to | Each object | The class itself |
| Memory | Separate per object | One shared copy |
| Access | `object.variable` | `ClassName.variable` |
| Use case | Object-specific data | Shared counters, constants |

---

### 🔷 8. toString() Override

```java
class Student {
    String name; int age;

    Student(String name, int age) {
        this.name = name; this.age = age;
    }

    @Override
    public String toString() {
        return "Student{name='" + name + "', age=" + age + "}";
    }
}

Student s = new Student("Abhishek", 24);
System.out.println(s);   // Student{name='Abhishek', age=24}
// Without override: Student@1b6d3586 (useless!)
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Rectangle Class `[Easy]`

**Task:** Design a complete `Rectangle` class with area, perimeter, isSquare, toString.

```java
class Rectangle {
    double length;
    double width;

    // Parameterized constructor
    Rectangle(double length, double width) {
        this.length = length;
        this.width  = width;
    }

    double area() {
        return length * width;
    }

    double perimeter() {
        return 2 * (length + width);
    }

    boolean isSquare() {
        return length == width;
    }

    @Override
    public String toString() {
        return "Rectangle[length=" + length + ", width=" + width + "]";
    }
}

public class P1_Rectangle {
    public static void main(String[] args) {
        Rectangle r1 = new Rectangle(5.0, 3.0);
        Rectangle r2 = new Rectangle(4.0, 4.0);
        Rectangle r3 = new Rectangle(10.0, 2.5);

        System.out.println(r1);
        System.out.printf("  Area: %.1f | Perimeter: %.1f | Square: %b%n%n",
            r1.area(), r1.perimeter(), r1.isSquare());

        System.out.println(r2);
        System.out.printf("  Area: %.1f | Perimeter: %.1f | Square: %b%n%n",
            r2.area(), r2.perimeter(), r2.isSquare());

        System.out.println(r3);
        System.out.printf("  Area: %.1f | Perimeter: %.1f | Square: %b%n",
            r3.area(), r3.perimeter(), r3.isSquare());
    }
}
```

```
Output:
Rectangle[length=5.0, width=3.0]
  Area: 15.0 | Perimeter: 16.0 | Square: false

Rectangle[length=4.0, width=4.0]
  Area: 16.0 | Perimeter: 16.0 | Square: true

Rectangle[length=10.0, width=2.5]
  Area: 25.0 | Perimeter: 25.0 | Square: false
```

> 🔑 **Key Learning:** The class is defined FIRST as a blueprint — then `main` uses it. Every `Rectangle` object gets its own `length` and `width` on the heap. `isSquare()` returns `length == width` directly — a boolean expression as return value. `toString()` with `@Override` makes `System.out.println(r1)` print meaningfully instead of a hash code.

---

### ✅ Problem 2 — BankAccount Class `[Easy-Medium]`

**Task:** Encapsulated bank account with validation.

```java
class BankAccount {
    private String accountNumber;
    private String ownerName;
    private double balance;

    BankAccount(String accountNumber, String ownerName, double initialBalance) {
        this.accountNumber = accountNumber;
        this.ownerName     = ownerName;
        this.balance       = initialBalance;
    }

    public double getBalance() { return balance; }
    public String getOwner()   { return ownerName; }

    public void deposit(double amount) {
        if (amount <= 0) {
            System.out.println("❌ Invalid deposit amount!");
            return;
        }
        balance += amount;
        System.out.printf("✅ Deposited ₹%.1f | New Balance: ₹%.1f%n",
                          amount, balance);
    }

    public void withdraw(double amount) {
        if (amount <= 0) {
            System.out.println("❌ Invalid withdrawal amount!");
        } else if (amount > balance) {
            System.out.println("❌ Insufficient funds! Balance: ₹" + balance);
        } else {
            balance -= amount;
            System.out.printf("✅ Withdrawn ₹%.1f | New Balance: ₹%.1f%n",
                              amount, balance);
        }
    }

    public void printStatement() {
        System.out.println("══════════════════════════════════");
        System.out.println("Account : " + accountNumber);
        System.out.println("Owner   : " + ownerName);
        System.out.printf ("Balance : ₹%.1f%n", balance);
        System.out.println("══════════════════════════════════");
    }
}

public class P2_BankAccount {
    public static void main(String[] args) {
        BankAccount acc = new BankAccount("ACC-1001", "Abhishek Ranjan", 5000.0);
        acc.printStatement();
        acc.deposit(2000);
        acc.withdraw(1500);
        acc.withdraw(8000);   // insufficient funds
        acc.deposit(-500);    // invalid
        System.out.println();
        acc.printStatement();
    }
}
```

```
Output:
══════════════════════════════════
Account : ACC-1001
Owner   : Abhishek Ranjan
Balance : ₹5000.0
══════════════════════════════════
✅ Deposited ₹2000.0 | New Balance: ₹7000.0
✅ Withdrawn ₹1500.0 | New Balance: ₹5500.0
❌ Insufficient funds! Balance: ₹5500.0
❌ Invalid deposit amount!

══════════════════════════════════
Account : ACC-1001
Owner   : Abhishek Ranjan
Balance : ₹5500.0
══════════════════════════════════
```

> 🔑 **Key Learning:** `private` on `balance` means no code outside this class can do `acc.balance = -9999` — a critical security guardrail. All access goes through validated methods. This is **Encapsulation** — the first OOP pillar. The `return` inside `deposit` after printing the error is an early exit — avoids deep nesting. In real banking systems, this pattern (private balance + validated methods) is non-negotiable.

---

### ✅ Problem 3 — Student Grade System `[Easy-Medium]`

**Task:** Student class with marks array and full report card.

```java
class Student {
    private String name;
    private int    rollNo;
    private int[]  marks;   // array of 5 subject marks

    Student(String name, int rollNo, int[] marks) {
        this.name   = name;
        this.rollNo = rollNo;
        this.marks  = marks;
    }

    int totalMarks() {
        int total = 0;
        for (int m : marks) total += m;
        return total;
    }

    double percentage() {
        return (totalMarks() / 500.0) * 100;
    }

    String grade() {
        double pct = percentage();
        if      (pct >= 80) return "A — Distinction";
        else if (pct >= 65) return "B — First Class";
        else if (pct >= 50) return "C — Second Class";
        else if (pct >= 35) return "D — Pass";
        else                return "F — Fail";
    }

    void printReport() {
        String[] subjects = {"Maths", "Physics", "Chemistry", "CS", "English"};
        System.out.println("╔════════════════════════════════╗");
        System.out.println("║         REPORT CARD            ║");
        System.out.println("╠════════════════════════════════╣");
        System.out.println("║ Name   : " + name);
        System.out.println("║ RollNo : " + rollNo);
        System.out.println("╠════════════════════════════════╣");
        for (int i = 0; i < marks.length; i++)
            System.out.printf("║ %-10s : %d/100%n", subjects[i], marks[i]);
        System.out.println("╠════════════════════════════════╣");
        System.out.printf( "║ Total  : %d/500%n", totalMarks());
        System.out.printf( "║ Pct    : %.1f%%%n", percentage());
        System.out.println("║ Grade  : " + grade());
        System.out.println("╚════════════════════════════════╝");
    }

    @Override
    public String toString() {
        return "Student{" + rollNo + ", " + name + ", " +
               String.format("%.1f", percentage()) + "%}";
    }
}

public class P3_StudentGrades {
    public static void main(String[] args) {
        Student s1 = new Student("Abhishek Ranjan", 101,
                                 new int[]{88, 92, 78, 95, 85});
        Student s2 = new Student("Riya Sharma", 102,
                                 new int[]{55, 60, 48, 70, 65});
        Student s3 = new Student("Arjun Kumar", 103,
                                 new int[]{30, 25, 40, 35, 28});

        s1.printReport(); System.out.println();
        s2.printReport(); System.out.println();

        System.out.println(s1);
        System.out.println(s2);
        System.out.println(s3);
    }
}
```

```
Output:
╔════════════════════════════════╗
║         REPORT CARD            ║
╠════════════════════════════════╣
║ Name   : Abhishek Ranjan
║ RollNo : 101
╠════════════════════════════════╣
║ Maths      : 88/100
║ Physics    : 92/100
║ Chemistry  : 78/100
║ CS         : 95/100
║ English    : 85/100
╠════════════════════════════════╣
║ Total  : 438/500
║ Pct    : 87.6%
║ Grade  : A — Distinction
╚════════════════════════════════╝

Student{101, Abhishek Ranjan, 87.6%}
Student{102, Riya Sharma, 59.6%}
Student{103, Arjun Kumar, 31.6%}
```

> 🔑 **Key Learning:** The `marks` array is an **instance variable** — each Student object has its own array on the heap. Methods like `totalMarks()` loop through `this.marks` using enhanced for-each — array operations inside OOP work exactly like standalone array problems. `grade()` calls `percentage()` internally — methods calling other methods of the same object. The `subjects` array in `printReport()` is a local variable (not instance) — it exists only during the method call. This shows arrays + OOP working together for the first time.

---

### ✅ Problem 4 — Constructor Overloading with Circle `[Medium]`

**Task:** Three overloaded constructors + `isPointInside()`.

```java
class Circle {
    private double centerX;
    private double centerY;
    private double radius;

    // Constructor 1 — default: origin, radius=1
    Circle() {
        this(0.0, 0.0, 1.0);
    }

    // Constructor 2 — specified radius, origin center
    Circle(double radius) {
        this(0.0, 0.0, radius);
    }

    // Constructor 3 — fully specified (all delegate here)
    Circle(double x, double y, double radius) {
        this.centerX = x;
        this.centerY = y;
        this.radius  = radius;
    }

    double area() {
        return Math.PI * radius * radius;
    }

    double circumference() {
        return 2 * Math.PI * radius;
    }

    boolean isPointInside(double px, double py) {
        double distance = Math.sqrt(
            Math.pow(px - centerX, 2) + Math.pow(py - centerY, 2)
        );
        return distance <= radius;
    }

    @Override
    public String toString() {
        return String.format("Circle[center=(%.1f,%.1f), radius=%.1f]",
                             centerX, centerY, radius);
    }
}

public class P4_Circle {
    public static void main(String[] args) {
        Circle c1 = new Circle();              // default
        Circle c2 = new Circle(5.0);           // radius only
        Circle c3 = new Circle(3.0, 4.0, 2.0); // full

        System.out.println(c1);
        System.out.printf("  Area: %.2f | Circumference: %.2f%n%n",
                          c1.area(), c1.circumference());

        System.out.println(c3);
        System.out.println("  Is (3,5) inside? " + c3.isPointInside(3, 5));
        System.out.println("  Is (1,1) inside? " + c3.isPointInside(1, 1));
        System.out.println("  Is (3,4) inside? " + c3.isPointInside(3, 4));
    }
}
```

```
Output:
Circle[center=(0.0,0.0), radius=1.0]
  Area: 3.14 | Circumference: 6.28

Circle[center=(3.0,4.0), radius=2.0]
  Is (3,5) inside? true   (distance=1.0 ≤ 2.0)
  Is (1,1) inside? false  (distance=3.61 > 2.0)
  Is (3,4) inside? true   (distance=0.0 ≤ 2.0 — it's the center!)
```

**Constructor delegation chain:**
```
Circle()         → this(0,0,1)  → Constructor 3
Circle(5.0)      → this(0,0,5)  → Constructor 3
Circle(3,4,2)    → Constructor 3 directly
```

**isPointInside trace for (3,5) in circle at (3,4) radius 2:**
```
distance = √((3-3)² + (5-4)²) = √(0+1) = √1 = 1.0
1.0 ≤ 2.0 → true ✅
```

> 🔑 **Key Learning:** `this()` chains constructors — Constructor 1 and 2 both delegate to Constructor 3, so initialization logic lives in ONE place. No code duplication. `isPointInside` uses the **Euclidean distance formula** — your first time applying math inside OOP. `Math.pow(x,2)` squares a number, `Math.sqrt()` takes square root. This pattern (distance ≤ radius) appears in many geometry/game problems.

---

### ✅ Problem 5 — `this` Keyword Mastery with Method Chaining `[Medium]`

**Task:** Demonstrate all 3 uses of `this` including fluent method chaining.

```java
class Person {
    private String name;
    private int    age;
    private String city;
    private String occupation;

    // Use 2: this() — default delegates to parameterized
    Person() {
        this("Unknown", 0, "Unknown", "Unemployed");
    }

    // Use 1: this.var — disambiguate instance vs parameter
    Person(String name, int age, String city, String occupation) {
        this.name       = name;
        this.age        = age;
        this.city       = city;
        this.occupation = occupation;
    }

    // Use 3: return this — enable method chaining
    Person setName(String name) {
        this.name = name;
        return this;          // ← returns current object
    }

    Person setAge(int age) {
        this.age = age;
        return this;
    }

    Person setCity(String city) {
        this.city = city;
        return this;
    }

    Person setOccupation(String occupation) {
        this.occupation = occupation;
        return this;
    }

    void introduce() {
        System.out.println("Hi! I'm " + name + ", age " + age +
                           ", from " + city + ". I work as " + occupation + ".");
    }

    @Override
    public String toString() {
        return "Person{" + name + ", " + age + ", " + city + "}";
    }
}

public class P5_ThisKeyword {
    public static void main(String[] args) {
        // Standard construction
        Person p1 = new Person("Abhishek Ranjan", 24, "Patna", "Software Engineer");
        p1.introduce();

        System.out.println();

        // Default constructor
        Person p2 = new Person();
        p2.introduce();

        System.out.println();

        // Method chaining — elegant!
        Person p3 = new Person()
                        .setName("Riya Sharma")
                        .setAge(22)
                        .setCity("Delhi")
                        .setOccupation("Data Analyst");
        p3.introduce();

        System.out.println();

        // Chain and modify later
        Person p4 = new Person("Arjun", 25, "Mumbai", "Developer");
        p4.setCity("Bangalore").setOccupation("Senior Developer"); // chained update
        p4.introduce();
    }
}
```

```
Output:
Hi! I'm Abhishek Ranjan, age 24, from Patna. I work as Software Engineer.

Hi! I'm Unknown, age 0, from Unknown. I work as Unemployed.

Hi! I'm Riya Sharma, age 22, from Delhi. I work as Data Analyst.

Hi! I'm Arjun, age 25, from Bangalore. I work as Senior Developer.
```

**Why method chaining works:**
```
new Person()               → creates object (let's call it X)
.setName("Riya")           → sets X.name, returns X
.setAge(22)                → sets X.age, returns X
.setCity("Delhi")          → sets X.city, returns X
.setOccupation("Analyst")  → sets X.occupation, returns X

p3 = X  (the final returned object) ✅
```

> 🔑 **Key Learning:** Method chaining is elegant and readable — you've seen it in `StringBuilder.append().reverse().toString()`. The secret is `return this` in every setter. The `this()` constructor delegation ensures the default constructor doesn't duplicate initialization code. All three uses of `this` working together = clean, professional OOP. This pattern is used extensively in Java APIs, builder patterns, and test frameworks.

---

### ✅ Problem 6 — Static vs Instance — Employee System `[Medium-Hard]`

**Task:** Auto-generating employee IDs, static class-level tracking.

```java
class Employee {
    // Static — shared by ALL employees, belongs to class
    private static int    nextId         = 1001;
    private static int    employeeCount  = 0;
    private static String companyName    = "TechCorp";

    // Instance — unique per employee
    private int    empId;
    private String name;
    private double salary;

    Employee(String name, double salary) {
        this.empId  = nextId++;          // auto-assign then increment static
        this.name   = name;
        this.salary = salary;
        employeeCount++;                 // track total created
    }

    // Static methods — don't need any object
    public static int    getEmployeeCount() { return employeeCount; }
    public static String getCompanyName()   { return companyName; }

    // Instance methods — operate on this object's data
    public void raiseSalary(double percent) {
        double raise = salary * (percent / 100);
        salary += raise;
        System.out.printf("✅ %s raised by %.0f%% → New salary: ₹%.1f%n",
                          name, percent, salary);
    }

    public void printDetails() {
        System.out.printf("EmpID: %d | Name: %-12s | Salary: ₹%.1f%n",
                          empId, name, salary);
    }

    @Override
    public String toString() {
        return String.format("Employee{%d, %s, ₹%.1f}", empId, name, salary);
    }
}

public class P6_Employee {
    public static void main(String[] args) {
        System.out.println("Company: " + Employee.getCompanyName());
        System.out.println();

        Employee e1 = new Employee("Abhishek Ranjan", 50000);
        Employee e2 = new Employee("Riya Sharma",     55000);
        Employee e3 = new Employee("Arjun Kumar",     48000);

        System.out.println("Total Employees: " + Employee.getEmployeeCount());
        System.out.println("─".repeat(50));

        e1.printDetails();
        e2.printDetails();
        e3.printDetails();

        System.out.println("─".repeat(50));
        e1.raiseSalary(10);
        e3.raiseSalary(15);

        System.out.println("─".repeat(50));
        System.out.println("Updated details:");
        e1.printDetails();
        e3.printDetails();
    }
}
```

```
Output:
Company: TechCorp

Total Employees: 3
──────────────────────────────────────────────────
EmpID: 1001 | Name: Abhishek Ranjan | Salary: ₹50000.0
EmpID: 1002 | Name: Riya Sharma     | Salary: ₹55000.0
EmpID: 1003 | Name: Arjun Kumar     | Salary: ₹48000.0
──────────────────────────────────────────────────
✅ Abhishek Ranjan raised by 10% → New salary: ₹55000.0
✅ Arjun Kumar raised by 15% → New salary: ₹55200.0
──────────────────────────────────────────────────
Updated details:
EmpID: 1001 | Name: Abhishek Ranjan | Salary: ₹55000.0
EmpID: 1003 | Name: Arjun Kumar     | Salary: ₹55200.0
```

**Static counter auto-increment trace:**
```
Employee("Abhishek", 50000): empId = 1001, nextId becomes 1002, count=1
Employee("Riya",     55000): empId = 1002, nextId becomes 1003, count=2
Employee("Arjun",    48000): empId = 1003, nextId becomes 1004, count=3
```

> 🔑 **Key Learning:** `nextId++` in the constructor — assign the current value to `this.empId`, THEN increment the static counter. This is how real applications generate user IDs, order numbers, and ticket numbers. `getEmployeeCount()` is `static` because it doesn't need any specific object — it's information about the class as a whole. Calling it as `Employee.getEmployeeCount()` (class name, not object) signals this clearly. Static + instance working together is a common pattern in database-backed applications.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** Mini Banking System with two interacting classes.

```java
class Account {
    // Static — shared by all accounts
    private static int    nextId       = 1001;
    private static int    totalAccounts = 0;
    private static String bankName     = "JavaBank";

    // Instance — unique per account
    private int    accountId;
    private String ownerName;
    private double balance;
    private int    transactionCount;

    Account(String ownerName, double initialDeposit) {
        this.accountId        = nextId++;
        this.ownerName        = ownerName;
        this.balance          = (initialDeposit > 0) ? initialDeposit : 0;
        this.transactionCount = (initialDeposit > 0) ? 1 : 0;
        totalAccounts++;
    }

    static int    getTotalAccounts() { return totalAccounts; }
    static String getBankName()      { return bankName; }

    double getBalance() { return balance; }

    void deposit(double amount) {
        if (amount <= 0) { System.out.println("❌ Invalid deposit!"); return; }
        balance += amount;
        transactionCount++;
        System.out.printf("✅ Deposited ₹%.0f into %s's account%n",
                          amount, ownerName);
    }

    void withdraw(double amount) {
        if (amount <= 0)       { System.out.println("❌ Invalid amount!"); return; }
        if (amount > balance)  { System.out.println("❌ Insufficient funds for " + ownerName); return; }
        balance -= amount;
        transactionCount++;
        System.out.printf("✅ Withdrawn ₹%.0f from %s's account%n",
                          amount, ownerName);
    }

    void transfer(Account target, double amount) {
        System.out.printf("🔄 Transfer: %s → %s: ₹%.0f%n",
                          ownerName, target.ownerName, amount);
        if (amount > balance) {
            System.out.println("❌ Transfer failed! Insufficient funds.");
            return;
        }
        this.balance        -= amount;
        target.balance      += amount;
        this.transactionCount++;
        target.transactionCount++;
        System.out.println("✅ Transfer successful!");
    }

    void printMiniStatement() {
        System.out.println("══════════════════════════════════════");
        System.out.println("    " + bankName + " — Mini Statement");
        System.out.println("══════════════════════════════════════");
        System.out.println("Account ID  : ACC-" + accountId);
        System.out.println("Owner       : " + ownerName);
        System.out.printf ("Balance     : ₹%.1f%n", balance);
        System.out.println("Transactions: " + transactionCount);
        System.out.println("══════════════════════════════════════");
    }

    @Override
    public String toString() {
        return String.format("Account{ACC-%d, %s, ₹%.1f}",
                             accountId, ownerName, balance);
    }
}

public class MiniChallenge_Day9 {
    public static void main(String[] args) {
        System.out.println("🏦 " + Account.getBankName() + " System Started");
        System.out.println();

        Account a1 = new Account("Abhishek Ranjan", 10000);
        Account a2 = new Account("Riya Sharma",     5000);
        Account a3 = new Account("Arjun Kumar",     7500);

        System.out.println("Total accounts: " + Account.getTotalAccounts());
        System.out.println();

        // Transactions
        a1.deposit(3000);
        a2.deposit(2000);
        a1.withdraw(1500);
        a1.transfer(a2, 2000);   // objects interacting!
        a3.withdraw(10000);      // should fail

        System.out.println();

        // Mini statements
        a1.printMiniStatement();
        System.out.println();
        a2.printMiniStatement();
        System.out.println();
        a3.printMiniStatement();
    }
}
```

```
Output:
🏦 JavaBank System Started

Total accounts: 3

✅ Deposited ₹3000 into Abhishek Ranjan's account
✅ Deposited ₹2000 into Riya Sharma's account
✅ Withdrawn ₹1500 from Abhishek Ranjan's account
🔄 Transfer: Abhishek Ranjan → Riya Sharma: ₹2000
✅ Transfer successful!
❌ Insufficient funds for Arjun Kumar

══════════════════════════════════════
    JavaBank — Mini Statement
══════════════════════════════════════
Account ID  : ACC-1001
Owner       : Abhishek Ranjan
Balance     : ₹9500.0
Transactions: 4
══════════════════════════════════════

══════════════════════════════════════
    JavaBank — Mini Statement
══════════════════════════════════════
Account ID  : ACC-1002
Owner       : Riya Sharma
Balance     : ₹9000.0
Transactions: 3
══════════════════════════════════════
```

> 🔑 **Key Learning:** The `transfer(Account target, double amount)` method takes another `Account` object as a parameter — this is **objects interacting with objects**. `this.balance -= amount` and `target.balance += amount` operate on two different objects in the same method call. Notice `target.balance` is accessible even though `balance` is `private` — this works because `target` is inside the same class (`Account`). Private access is per-class, not per-object. This "objects as parameters" pattern is the backbone of linked lists, trees, and graphs in DSA.

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| `isPalindrome(String)` two-pointer from memory | ✅ `charAt(left) != charAt(right)` |
| `c - 'a'` for `c = 'n'` | ✅ 'n'=110, 'a'=97, 110-97 = **13** |
| StringBuilder vs String += complexity | ✅ O(n) vs O(n²) — buffer doubling |
| `"Sum: " + 3 + 4` vs `"Sum: " + (3+4)` | ✅ "Sum: 34" vs "Sum: 7" |

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. Class vs Object — real-world analogy?</b></summary>

> **Class** = Blueprint/Template. It defines the structure — what data and behaviors exist — but holds no actual data itself.
> **Object** = A concrete instance built from that blueprint, with its own real data.
>
> **Analogy:** A **class** is like an architectural blueprint for a house. The blueprint shows where rooms, doors, and windows go — but you can't live in a blueprint. An **object** is an actual house built from that blueprint. You can build 100 houses (objects) from one blueprint (class) — each house has its own address, color, and furniture (instance variables), but all follow the same structural design.

</details>

<details>
<summary><b>Q2. What if you don't write any constructor?</b></summary>

> Java automatically provides a **default no-argument constructor** `ClassName() {}` that does nothing. This allows you to create objects with `new ClassName()`.
>
> **Critical trap:** The moment you write ANY constructor yourself, Java stops providing the default. So if you write only `Student(String name, int age)`, then `new Student()` becomes a compile error — Java no longer supplies the no-arg version. You must explicitly write `Student() {}` yourself if you need it alongside your parameterized constructor.

</details>

<details>
<summary><b>Q3. Three uses of the `this` keyword?</b></summary>

> **1. `this.variable`** — Disambiguates instance variable from parameter when they share the same name:
> `this.name = name;` — left is instance var, right is parameter.
>
> **2. `this()`** — Calls another constructor from within a constructor (constructor chaining). Must be the FIRST statement: `this("Unknown", 0);` delegates to a parameterized constructor, avoiding code duplication.
>
> **3. `return this`** — Returns the current object from a method, enabling method chaining: `setName("A").setAge(24).setCity("B")` — each setter returns the same object.

</details>

<details>
<summary><b>Q4. Instance variable vs Static variable?</b></summary>

> **Instance variable** — declared without `static`. Each object gets its OWN separate copy. `s1.name` and `s2.name` are independent memory locations. Accessed via object: `s1.name`.
>
> **Static variable** — declared with `static`. ONE copy shared by ALL objects of the class. Changing it from any object changes it for all. Accessed via class name: `Employee.totalCount`.
>
> Instance belongs to the object. Static belongs to the class. Use static for counters, constants, shared state. Use instance for object-specific data.

</details>

<details>
<summary><b>Q5. Why private instead of public for instance variables?</b></summary>

> **With `public`:** Any code anywhere can do `account.balance = -99999` — no validation, no control. Data can be corrupted by any piece of code in the entire program.
>
> **With `private`:** Only methods inside the class can access the variable. To change the balance, you MUST go through `deposit()` or `withdraw()`, which enforce rules (positive amounts, sufficient funds). This is **Encapsulation** — hiding implementation details and controlling access.
>
> **Real consequence:** In a banking app with `public balance`, a bug in any class could corrupt every account. With `private` + validated methods, corruption is impossible without going through the guard. This is why all professional Java code uses private fields.

</details>

---

## 📌 DAY 9 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 9 — SUMMARY                        ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 07, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  OOP — Classes, Objects & Constructors           ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  OOP big picture — 4 pillars overview                        ║
║  ✅  Class = blueprint, Object = instance (heap memory)          ║
║  ✅  Instance variables — each object gets its own copy          ║
║  ✅  this.var, this(), return this — all 3 uses                  ║
║  ✅  Default, parameterized, overloaded constructors             ║
║  ✅  Constructor chaining with this()                            ║
║  ✅  Getters & Setters — encapsulation pattern                   ║
║  ✅  Static vs Instance — class-level vs object-level            ║
║  ✅  toString() override — meaningful object representation      ║
╠══════════════════════════════════════════════════════════════════╣
║  PROBLEMS SOLVED                                                ║
║  ✅  P1 — Rectangle (area, perimeter, isSquare, toString)        ║
║  ✅  P2 — BankAccount (private, getters, validation)             ║
║  ✅  P3 — Student (array inside class, grade system)             ║
║  ✅  P4 — Circle (constructor overloading, isPointInside)        ║
║  ✅  P5 — Person (all 3 this uses + method chaining)             ║
║  ✅  P6 — Employee (static counter, auto-ID, raiseSalary)        ║
║  ✅  Mini — Banking System (objects as parameters, transfer)     ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  90%                              ║
║  STRONG AREA    :  Encapsulation, constructors, static/instance  ║
║  WEAK AREA      :  this() chaining order (must be first stmt)   ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  Class = blueprint, Object = instance with its own data      ║
║  ▸  Always private fields + public methods — Encapsulation       ║
║  ▸  If you write ANY constructor, default is NO longer given     ║
║  ▸  this() must be the FIRST statement in a constructor          ║
║  ▸  return this enables method chaining — elegant pattern        ║
║  ▸  Static = class-level, Instance = object-level               ║
║  ▸  Objects as method parameters = objects interacting           ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 08](./Day08_Strings.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 10 →](./Day10_OOP_Inheritance.md)

*Day 9 of 160 — Done ✅ | Streak: 9 Days 🔥🔥🔥🔥🔥🔥🔥🔥🔥*

</div>
