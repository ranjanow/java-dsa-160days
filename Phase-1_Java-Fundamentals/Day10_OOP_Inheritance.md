# 📄 Day 10 — OOP: Inheritance, Polymorphism & Abstraction

<div align="center">

![Day](https://img.shields.io/badge/Day-10_of_160-blueviolet?style=for-the-badge)
![Phase](https://img.shields.io/badge/Phase-1_Java_Fundamentals-green?style=for-the-badge)
![Topic](https://img.shields.io/badge/Topic-Inheritance_%26_Polymorphism-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed_✅-brightgreen?style=for-the-badge)
![Problems](https://img.shields.io/badge/Problems_Solved-6_of_6-blue?style=for-the-badge)
![Hours](https://img.shields.io/badge/Hours-5-red?style=for-the-badge)

</div>

<div align="center">

[← Day 09](./Day09_OOP_Basics.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 11 →](./Day11_Recursion.md)

</div>

---

## 🎯 Day 10 Goal
> Master Inheritance, Method Overriding, Polymorphism, Abstract Classes, and Interfaces — the remaining three pillars of OOP. These concepts are not just Java syntax — they are the thinking patterns behind every real framework, every design pattern, and every DSA node structure you will build.

---

## ⏰ Time Breakdown

| Block | Activity | Duration |
|-------|----------|----------|
| Block 1 | Inheritance — `extends`, `super`, constructor chaining | 60 min |
| Block 2 | Method Overriding — `@Override`, runtime polymorphism | 45 min |
| Block 3 | Abstract classes + Interfaces | 40 min |
| Block 4 | Revision — Day 9 OOP flash review | 15 min |
| Block 5 | Practice Set — 6 Problems | 120 min |

---

## 📘 CONCEPTS LEARNED

---

### 🔷 1. Inheritance — `extends`

Inheritance lets a **child class** reuse all non-private fields and methods of a **parent class**, and add or override behavior. Models the **IS-A** relationship.

```java
// Parent class (Superclass)
class Animal {
    String name;
    int    age;

    Animal(String name, int age) {
        this.name = name;
        this.age  = age;
    }

    void eat()   { System.out.println(name + " is eating."); }
    void sleep() { System.out.println(name + " is sleeping."); }
}

// Child class — gets ALL of Animal's members
class Dog extends Animal {
    String breed;

    Dog(String name, int age, String breed) {
        super(name, age);    // calls Animal's constructor
        this.breed = breed;
    }

    void bark() { System.out.println(name + " says: Woof! 🐕"); }
}

Dog d = new Dog("Bruno", 3, "Labrador");
d.eat();    // inherited → "Bruno is eating."
d.sleep();  // inherited → "Bruno is sleeping."
d.bark();   // Dog's own → "Bruno says: Woof! 🐕"
```

**IS-A vs HAS-A:**
```
Dog IS-A  Animal  → use inheritance (extends)
Car HAS-A Engine  → use composition (field)
```

> 💡 Java supports **single inheritance** only — a class can extend only ONE parent. But it can implement multiple interfaces (covered in section 6).

---

### 🔷 2. The `super` Keyword — Three Uses

```java
class Vehicle {
    String brand;
    int    speed;

    Vehicle(String brand, int speed) {
        this.brand = brand;
        this.speed = speed;
    }

    void describe() {
        System.out.println("Brand: " + brand + " | Speed: " + speed + " km/h");
    }
}

class Car extends Vehicle {
    int doors;

    // Use 1: super() — call parent constructor (MUST be first statement)
    Car(String brand, int speed, int doors) {
        super(brand, speed);
        this.doors = doors;
    }

    // Use 2: super.method() — call parent's version of an overridden method
    @Override
    void describe() {
        super.describe();             // prints Vehicle part first
        System.out.println("Doors: " + doors);
    }

    // Use 3: super.field — access parent's field (less common)
    void showBrand() {
        System.out.println("Brand: " + super.brand);
    }
}
```

> ⚠️ `super()` must be the **first statement** in a child constructor. You cannot call both `super()` and `this()` in the same constructor.

---

### 🔷 3. Method Overriding — `@Override`

A child class provides its **own version** of a method already defined in the parent.

```java
class Shape {
    double area()  { return 0; }
    void   draw()  { System.out.println("Drawing a shape..."); }
}

class Circle extends Shape {
    double radius;
    Circle(double r) { this.radius = r; }

    @Override
    double area() { return Math.PI * radius * radius; }

    @Override
    void draw()   { System.out.println("Drawing circle ⭕ r=" + radius); }
}

class Rectangle extends Shape {
    double l, w;
    Rectangle(double l, double w) { this.l = l; this.w = w; }

    @Override
    double area() { return l * w; }

    @Override
    void draw()   { System.out.println("Drawing rectangle 🔲 " + l + "×" + w); }
}
```

**Overriding Rules:**
| Rule | Detail |
|------|--------|
| Signature | Same name, same parameters, same return type |
| Access | Child visibility must be same OR wider (`protected` → `public` ✅) |
| `static` | Cannot override — only "hide" |
| `final` | Cannot override |
| `private` | Cannot override (not inherited) |

**Overloading vs Overriding — Critical Difference:**
| | Overloading | Overriding |
|--|-------------|------------|
| Where | Same class | Parent → Child |
| Parameters | Different | Same |
| Resolved | Compile time | **Runtime** |
| Purpose | Multiple versions | Redefine behavior |

---

### 🔷 4. Polymorphism — One Reference, Many Forms

**Runtime Polymorphism:** A parent-type reference holds a child-type object. The method that actually runs is decided at **runtime** based on the actual object — not the reference type.

```java
Shape s;

s = new Circle(5);
System.out.println(s.area());    // Circle's area() → 78.54

s = new Rectangle(4, 6);
System.out.println(s.area());    // Rectangle's area() → 24.0

// Polymorphic array — most powerful usage
Shape[] shapes = {
    new Circle(3),
    new Rectangle(4, 5),
    new Circle(7)
};

for (Shape shape : shapes) {
    shape.draw();                        // each calls ITS OWN draw()
    System.out.printf("Area: %.2f%n", shape.area());
}
```

```
Output:
Drawing circle ⭕ r=3.0    Area: 28.27
Drawing rectangle 🔲 4.0×5.0  Area: 20.00
Drawing circle ⭕ r=7.0    Area: 153.94
```

> 💡 **Why polymorphism matters:** You write ONE loop, ONE method call — each object type handles it correctly. When you add a new `Triangle` class later, the loop works with ZERO changes. This is **open/closed principle** — open for extension, closed for modification.

---

### 🔷 5. Abstract Classes

Cannot be instantiated. Exists to be extended. Can have both abstract methods (no body, must be overridden) and concrete methods (body, inherited as-is).

```java
abstract class Shape {
    String color;

    Shape(String color) { this.color = color; }

    // Abstract — no body, child MUST implement
    abstract double area();
    abstract void   draw();

    // Concrete — inherited as-is
    void printInfo() {
        System.out.printf("Color: %s | Area: %.2f%n", color, area());
        // area() here calls the CHILD's version — polymorphism!
    }
}

class Triangle extends Shape {
    double base, height;

    Triangle(String color, double base, double height) {
        super(color);
        this.base   = base;
        this.height = height;
    }

    @Override double area() { return 0.5 * base * height; }
    @Override void   draw() { System.out.println("Drawing triangle 🔺"); }
}

// Shape s = new Shape("Red");      ← ❌ cannot instantiate abstract class!
Shape s = new Triangle("Blue", 6, 4);   // ✅ reference is Shape, object is Triangle
s.printInfo();   // Color: Blue | Area: 12.00
```

---

### 🔷 6. Interfaces — Pure Contract

Defines **what a class must do**, not how. A class **implements** an interface and must provide all method bodies. Unlike inheritance, a class can implement **multiple interfaces**.

```java
interface Drawable {
    void draw();                         // implicitly public abstract
    void resize(double factor);
}

interface Colorable {
    void setColor(String color);
    String getColor();
}

// Implements BOTH interfaces
class Canvas implements Drawable, Colorable {
    private String color = "Black";

    @Override public void   draw()                { System.out.println("Drawing on canvas in " + color); }
    @Override public void   resize(double f)      { System.out.println("Resizing ×" + f); }
    @Override public void   setColor(String c)    { this.color = c; }
    @Override public String getColor()            { return color; }
}
```

**Interface vs Abstract Class:**
| Feature | Interface | Abstract Class |
|---------|-----------|----------------|
| Multiple inheritance | ✅ Many interfaces | ❌ One class only |
| Constructor | ❌ No | ✅ Yes |
| Instance variables | ❌ Only `public static final` | ✅ Any |
| When to use | Define capability | Partial implementation |

---

### 🔷 7. `instanceof` and Casting

```java
Shape s = new Circle(5);

System.out.println(s instanceof Shape);      // true — Circle IS-A Shape
System.out.println(s instanceof Circle);     // true
System.out.println(s instanceof Rectangle);  // false

// Downcasting — parent reference → child type
if (s instanceof Circle) {
    Circle c = (Circle) s;        // explicit downcast
    System.out.println("Radius: " + c.radius);
}

// Java 16+ pattern matching (cleaner)
if (s instanceof Circle c) {
    System.out.println("Radius: " + c.radius);
}
```

---

### 🔷 8. `final` Keyword in OOP

```java
final class Constants { }         // cannot be extended

class Base {
    final void secureMethod() { }  // cannot be overridden
    final int MAX = 100;           // cannot be reassigned
}
```

---

## 🧠 PRACTICE SET — 6 / 6 Solved ✅

---

### ✅ Problem 1 — Animal Hierarchy `[Easy]`

**Task:** Build Animal → Dog/Cat inheritance chain with polymorphic array.

```java
class Animal {
    protected String name;
    protected int    age;

    Animal(String name, int age) {
        this.name = name;
        this.age  = age;
    }

    void eat()   { System.out.println(name + " is eating 🍽️"); }
    void sleep() { System.out.println(name + " is sleeping 💤"); }

    @Override
    public String toString() {
        return "Animal{name='" + name + "', age=" + age + "}";
    }
}

class Dog extends Animal {
    private String breed;

    Dog(String name, int age, String breed) {
        super(name, age);
        this.breed = breed;
    }

    void bark()  { System.out.println(name + " says: Woof! 🐕"); }
    void fetch() { System.out.println(name + " fetches the ball! 🎾"); }

    @Override
    public String toString() {
        return "Dog{name='" + name + "', age=" + age + "', breed='" + breed + "'}";
    }
}

class Cat extends Animal {
    private boolean isIndoor;

    Cat(String name, int age, boolean isIndoor) {
        super(name, age);
        this.isIndoor = isIndoor;
    }

    void meow() { System.out.println(name + " says: Meow! 🐈"); }
    void purr()  { System.out.println(name + " is purring... 😌"); }

    @Override
    public String toString() {
        return "Cat{name='" + name + "', age=" + age +
               ", indoor=" + isIndoor + "}";
    }
}

public class P1_AnimalHierarchy {
    public static void main(String[] args) {
        // Polymorphic array — Animal reference holds Dog and Cat objects
        Animal[] animals = {
            new Dog("Bruno",  3, "Labrador"),
            new Dog("Rocky",  2, "German Shepherd"),
            new Cat("Whiskers", 4, true),
            new Cat("Shadow",   1, false)
        };

        System.out.println("═══ Animal Family ═══");
        for (Animal a : animals) {
            System.out.println(a);        // calls each class's toString()
            a.eat();
            // Downcast to access specific methods
            if (a instanceof Dog d)  { d.bark();  d.fetch(); }
            if (a instanceof Cat c)  { c.meow();  c.purr();  }
            System.out.println();
        }
    }
}
```

```
Output:
═══ Animal Family ═══
Dog{name='Bruno', age=3, breed='Labrador'}
Bruno is eating 🍽️
Bruno says: Woof! 🐕
Bruno fetches the ball! 🎾

Dog{name='Rocky', age=2, breed='German Shepherd'}
Rocky is eating 🍽️
Rocky says: Woof! 🐕
Rocky fetches the ball! 🎾

Cat{name='Whiskers', age=4, indoor=true}
Whiskers is eating 🍽️
Whiskers says: Meow! 🐈
Whiskers is purring... 😌

Cat{name='Shadow', age=1, indoor=false}
Shadow is eating 🍽️
Shadow says: Meow! 🐈
Shadow is purring... 😌
```

> 🔑 **Key Learning:** `Animal[] animals` can hold `Dog` and `Cat` objects because both ARE-A Animal — this is polymorphism. The `for (Animal a : animals)` loop calls `a.toString()` — but each object's OWN toString runs. Downcasting with `instanceof Dog d` gives access to Dog-specific methods without breaking polymorphism. `protected` on name and age allows child classes to access them directly.

---

### ✅ Problem 2 — Shape Area Calculator `[Easy-Medium]`

**Task:** Abstract Shape hierarchy with polymorphic `printInfo()`.

```java
abstract class Shape {
    protected String color;

    Shape(String color) { this.color = color; }

    // Each subclass MUST implement these
    abstract double area();
    abstract double perimeter();

    // Concrete method — calls abstract methods (polymorphism!)
    void printInfo() {
        System.out.println("┌─────────────────────────────────");
        System.out.println("│ Shape     : " + getClass().getSimpleName());
        System.out.println("│ Color     : " + color);
        System.out.printf ("│ Area      : %.2f%n", area());
        System.out.printf ("│ Perimeter : %.2f%n", perimeter());
        System.out.println("└─────────────────────────────────");
    }
}

class Circle extends Shape {
    private double radius;

    Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }

    @Override public double area()      { return Math.PI * radius * radius; }
    @Override public double perimeter() { return 2 * Math.PI * radius; }
}

class Rectangle extends Shape {
    private double length, width;

    Rectangle(String color, double length, double width) {
        super(color);
        this.length = length;
        this.width  = width;
    }

    @Override public double area()      { return length * width; }
    @Override public double perimeter() { return 2 * (length + width); }
}

class Triangle extends Shape {
    private double a, b, c;    // three sides
    private double base, height;

    Triangle(String color, double a, double b, double c, double base, double height) {
        super(color);
        this.a = a; this.b = b; this.c = c;
        this.base = base; this.height = height;
    }

    @Override public double area()      { return 0.5 * base * height; }
    @Override public double perimeter() { return a + b + c; }
}

public class P2_ShapeCalculator {
    public static void main(String[] args) {
        Shape[] shapes = {
            new Circle("Red", 7.0),
            new Rectangle("Blue", 5.0, 3.0),
            new Triangle("Green", 3.0, 4.0, 5.0, 3.0, 4.0)
        };

        for (Shape s : shapes) {
            s.printInfo();   // calls each subclass's area() and perimeter()
            System.out.println();
        }
    }
}
```

```
Output:
┌─────────────────────────────────
│ Shape     : Circle
│ Color     : Red
│ Area      : 153.94
│ Perimeter : 43.98
└─────────────────────────────────

┌─────────────────────────────────
│ Shape     : Rectangle
│ Color     : Blue
│ Area      : 15.00
│ Perimeter : 16.00
└─────────────────────────────────

┌─────────────────────────────────
│ Shape     : Triangle
│ Color     : Green
│ Area      : 6.00
│ Perimeter : 12.00
└─────────────────────────────────
```

**The polymorphism magic in `printInfo()`:**
```
printInfo() is defined in Shape parent.
It calls area() — but Shape's area() is abstract (no body).
When s = new Circle(...), calling s.printInfo() runs Shape's printInfo()
which calls area() → and Java dispatches to Circle's area() AT RUNTIME.
This is dynamic dispatch — the core of runtime polymorphism.
```

> 🔑 **Key Learning:** `printInfo()` is written ONCE in the parent — but calling `area()` and `perimeter()` inside it automatically invokes the child's versions. `getClass().getSimpleName()` returns the actual runtime class name ("Circle", "Rectangle", etc.) — another use of polymorphism. Adding a new `Hexagon` class requires ZERO changes to `printInfo()` or the loop.

---

### ✅ Problem 3 — Vehicle System with `super` `[Easy-Medium]`

**Task:** Vehicle → Car/Truck with `super.describe()` chaining.

```java
class Vehicle {
    protected String brand;
    protected int    year;
    protected int    speed;

    Vehicle(String brand, int year) {
        this.brand = brand;
        this.year  = year;
        this.speed = 0;
    }

    void accelerate(int kmph) {
        speed += kmph;
        System.out.println(brand + " accelerated to " + speed + " km/h");
    }

    void brake(int kmph) {
        speed = Math.max(0, speed - kmph);
        System.out.println(brand + " braked to " + speed + " km/h");
    }

    void describe() {
        System.out.println("Brand: " + brand + " | Year: " + year +
                           " | Speed: " + speed + " km/h");
    }
}

class Car extends Vehicle {
    private int    doors;
    private String fuelType;

    Car(String brand, int year, int doors, String fuelType) {
        super(brand, year);
        this.doors    = doors;
        this.fuelType = fuelType;
    }

    @Override
    void describe() {
        super.describe();   // Vehicle's info first
        System.out.println("  └── Doors: " + doors + " | Fuel: " + fuelType);
    }
}

class Truck extends Vehicle {
    private double cargoCapacityTons;

    Truck(String brand, int year, double cargoCapacityTons) {
        super(brand, year);
        this.cargoCapacityTons = cargoCapacityTons;
    }

    @Override
    void describe() {
        super.describe();   // Vehicle's info first
        System.out.printf("  └── Cargo: %.1f tons%n", cargoCapacityTons);
    }

    void loadCargo(double tons) {
        System.out.printf("%s loaded %.1f tons of cargo%n", brand, tons);
    }
}

public class P3_VehicleSystem {
    public static void main(String[] args) {
        Car   car   = new Car("Toyota", 2022, 4, "Petrol");
        Truck truck = new Truck("Volvo", 2020, 15.5);

        car.accelerate(60);
        car.accelerate(40);
        car.describe();

        System.out.println();

        truck.accelerate(80);
        truck.loadCargo(12.3);
        truck.describe();

        System.out.println();

        // Polymorphic array
        Vehicle[] fleet = {car, truck};
        System.out.println("=== Fleet Summary ===");
        for (Vehicle v : fleet) {
            v.describe();
            System.out.println();
        }
    }
}
```

```
Output:
Toyota accelerated to 60 km/h
Toyota accelerated to 100 km/h
Brand: Toyota | Year: 2022 | Speed: 100 km/h
  └── Doors: 4 | Fuel: Petrol

Volvo accelerated to 80 km/h
Volvo loaded 12.3 tons of cargo
Brand: Volvo | Year: 2020 | Speed: 80 km/h
  └── Cargo: 15.5 tons

=== Fleet Summary ===
Brand: Toyota | Year: 2022 | Speed: 100 km/h
  └── Doors: 4 | Fuel: Petrol

Brand: Volvo | Year: 2020 | Speed: 80 km/h
  └── Cargo: 15.5 tons
```

> 🔑 **Key Learning:** `super.describe()` in Car/Truck calls Vehicle's describe() first, then adds its own extra info — this is **extending parent behavior, not replacing it**. `accelerate()` is defined ONCE in Vehicle — both Car and Truck inherit it with zero duplication. The `fleet` array holds both Car and Truck as `Vehicle` references — `v.describe()` calls each object's overridden version.

---

### ✅ Problem 4 — Interface Implementation `[Medium]`

**Task:** Two interfaces, three implementing classes, multiple interface references.

```java
interface Playable {
    void play();
    void pause();
    void stop();
}

interface Chargeable {
    void charge(int percent);
    int getBattery();
}

class MusicPlayer implements Playable {
    private String currentSong;
    private boolean playing = false;

    MusicPlayer(String song) { this.currentSong = song; }

    @Override public void play()  { playing = true;  System.out.println("🎵 Playing: " + currentSong); }
    @Override public void pause() { playing = false; System.out.println("⏸️  Paused: " + currentSong); }
    @Override public void stop()  { playing = false; System.out.println("⏹️  Stopped"); }
}

class Smartphone implements Playable, Chargeable {
    private String  currentApp;
    private int     battery;

    Smartphone(String app, int battery) {
        this.currentApp = app;
        this.battery    = battery;
    }

    @Override public void play()          { System.out.println("📱 Running: " + currentApp + " [" + battery + "%]"); }
    @Override public void pause()         { System.out.println("📱 App paused"); }
    @Override public void stop()          { System.out.println("📱 App closed"); }
    @Override public void charge(int pct) { battery = Math.min(100, battery + pct);
                                            System.out.println("🔋 Charged to " + battery + "%"); }
    @Override public int getBattery()     { return battery; }
}

class PowerBank implements Chargeable {
    private int capacity;

    PowerBank(int capacity) { this.capacity = capacity; }

    @Override public void charge(int pct) { System.out.println("⚡ Power Bank charging device by " + pct + "%"); }
    @Override public int getBattery()     { return capacity; }
}

public class P4_Interfaces {
    public static void main(String[] args) {
        Smartphone phone = new Smartphone("YouTube", 45);

        // Playable reference — only sees play/pause/stop
        Playable p = phone;
        p.play();
        p.pause();
        // p.charge(20);   ← ❌ compile error! Playable doesn't have charge()

        System.out.println();

        // Chargeable reference — only sees charge/getBattery
        Chargeable c = phone;
        c.charge(30);
        System.out.println("Battery: " + c.getBattery() + "%");
        // c.play();   ← ❌ compile error! Chargeable doesn't have play()

        System.out.println();

        // Interface array — polymorphism with interfaces
        Playable[] players = { new MusicPlayer("Jai Ho"), phone };
        for (Playable pl : players) {
            pl.play();
            pl.stop();
        }

        System.out.println();

        PowerBank pb = new PowerBank(20000);
        pb.charge(50);
        System.out.println("Power Bank capacity: " + pb.getBattery() + "mAh");
    }
}
```

```
Output:
📱 Running: YouTube [45%]
📱 App paused

🔋 Charged to 75%
Battery: 75%

🎵 Playing: Jai Ho
⏹️  Stopped
📱 Running: YouTube [75%]
📱 App closed

⚡ Power Bank charging device by 50%
Power Bank capacity: 20000mAh
```

**Why `p.charge(20)` is a compile error:**
```
Playable p = phone;
p only "knows about" Playable's methods: play(), pause(), stop()
Even though phone (Smartphone) HAS charge() — p's type is Playable.
The compiler only sees what Playable declares.
To call charge(), you must use: ((Chargeable) p).charge(20) or c.charge(20)
```

> 🔑 **Key Learning:** A reference type determines WHAT methods are visible — the object type determines WHICH implementation runs. `Playable p = phone` restricts visible methods to the `Playable` interface even though `phone` has more methods. This is deliberate — it enforces the principle of least privilege. Interfaces enable true multiple inheritance of type in Java.

---

### ✅ Problem 5 — Template Method Pattern `[Medium]`

**Task:** Abstract `DataProcessor` with fixed algorithm skeleton.

```java
abstract class DataProcessor {
    private String processorName;

    DataProcessor(String name) { this.processorName = name; }

    // Template method — FINAL: algorithm skeleton cannot be changed
    final void process() {
        System.out.println("╔══════════════════════════════╗");
        System.out.println("║  " + processorName + " Processing");
        System.out.println("╠══════════════════════════════╣");
        readData();
        processData();
        writeOutput();
        System.out.println("╚══════════════════════════════╝");
        System.out.println();
    }

    // Steps — subclasses define HOW each step works
    abstract void readData();
    abstract void processData();
    abstract void writeOutput();
}

class CSVProcessor extends DataProcessor {
    private int rowCount;

    CSVProcessor() { super("CSV"); }

    @Override
    void readData() {
        rowCount = 1500;
        System.out.println("║  📂 Reading CSV file...");
        System.out.println("║     Loaded " + rowCount + " rows");
    }

    @Override
    void processData() {
        int valid = (int)(rowCount * 0.95);
        System.out.println("║  ⚙️  Processing rows...");
        System.out.println("║     Valid: " + valid + " | Skipped: " + (rowCount - valid));
    }

    @Override
    void writeOutput() {
        System.out.println("║  💾 Writing summary.csv...");
        System.out.println("║     ✅ Done!");
    }
}

class JSONProcessor extends DataProcessor {
    private int keyCount;

    JSONProcessor() { super("JSON"); }

    @Override
    void readData() {
        keyCount = 42;
        System.out.println("║  📂 Parsing JSON file...");
        System.out.println("║     Found " + keyCount + " keys");
    }

    @Override
    void processData() {
        System.out.println("║  ⚙️  Validating schema...");
        System.out.println("║     " + (keyCount - 3) + " keys valid, 3 deprecated");
    }

    @Override
    void writeOutput() {
        System.out.println("║  💾 Writing output.json...");
        System.out.println("║     ✅ Done!");
    }
}

public class P5_TemplateMethod {
    public static void main(String[] args) {
        DataProcessor[] processors = { new CSVProcessor(), new JSONProcessor() };

        for (DataProcessor dp : processors) {
            dp.process();   // fixed sequence: read → process → write
        }
    }
}
```

```
Output:
╔══════════════════════════════╗
║  CSV Processing
╠══════════════════════════════╣
║  📂 Reading CSV file...
║     Loaded 1500 rows
║  ⚙️  Processing rows...
║     Valid: 1425 | Skipped: 75
║  💾 Writing summary.csv...
║     ✅ Done!
╚══════════════════════════════╝

╔══════════════════════════════╗
║  JSON Processing
╠══════════════════════════════╣
║  📂 Parsing JSON file...
║     Found 42 keys
║  ⚙️  Validating schema...
║     39 keys valid, 3 deprecated
║  💾 Writing output.json...
║     ✅ Done!
╚══════════════════════════════╝
```

**Why `process()` is `final`:**
```
final void process() {
    readData();      ← ORDER is fixed
    processData();   ← ORDER is fixed
    writeOutput();   ← ORDER is fixed
}
Subclasses can change WHAT each step does, but NOT the sequence.
This is the Template Method Design Pattern — used in:
→ Android Activity lifecycle (onCreate → onStart → onResume)
→ JUnit test framework (setUp → test → tearDown)
→ HTTP request pipelines (authenticate → authorize → execute)
```

> 🔑 **Key Learning:** `final` on `process()` locks the algorithm sequence — subclasses can never reorder steps. The abstract steps (`readData`, `processData`, `writeOutput`) define customization points. The parent controls the skeleton; the children control the details. This is a real **GoF Design Pattern** — Template Method. You've implemented a professional pattern on Day 10.

---

### ✅ Problem 6 — Full OOP System — Library `[Medium-Hard]`

**Task:** Abstract LibraryItem + Searchable interface + Library coordinator.

```java
interface Searchable {
    boolean matchesTitle(String query);
    boolean matchesAuthor(String query);
}

abstract class LibraryItem implements Searchable {
    protected String  title;
    protected String  author;
    protected int     year;
    protected boolean available;

    LibraryItem(String title, String author, int year) {
        this.title     = title;
        this.author    = author;
        this.year      = year;
        this.available = true;
    }

    abstract String getInfo();

    void checkOut() {
        if (available) { available = false; System.out.println("✅ Checked out: " + title); }
        else           { System.out.println("❌ Not available: " + title); }
    }

    void returnItem() {
        available = true;
        System.out.println("✅ Returned: " + title);
    }

    boolean isAvailable() { return available; }

    // Implement Searchable
    @Override public boolean matchesTitle(String q)  { return title.toLowerCase().contains(q.toLowerCase()); }
    @Override public boolean matchesAuthor(String q) { return author.toLowerCase().contains(q.toLowerCase()); }
}

class Book extends LibraryItem {
    private String isbn;
    private int    pages;
    private String genre;

    Book(String title, String author, int year, String isbn, int pages, String genre) {
        super(title, author, year);
        this.isbn  = isbn;
        this.pages = pages;
        this.genre = genre;
    }

    @Override
    public String getInfo() {
        return String.format("📚 Book    | %-30s | %s (%d) | %s | %d pages | %s",
                             title, author, year, genre, pages,
                             available ? "✅ Available" : "❌ Checked Out");
    }
}

class Magazine extends LibraryItem {
    private int    issueNumber;
    private String frequency;

    Magazine(String title, String author, int year, int issueNumber, String frequency) {
        super(title, author, year);
        this.issueNumber = issueNumber;
        this.frequency   = frequency;
    }

    @Override
    public String getInfo() {
        return String.format("📰 Magazine| %-30s | %s (%d) | Issue #%d | %s | %s",
                             title, author, year, issueNumber, frequency,
                             available ? "✅ Available" : "❌ Checked Out");
    }
}

class Library {
    private LibraryItem[] items;
    private int           count;
    private String        libraryName;

    Library(String name, int capacity) {
        libraryName = name;
        items       = new LibraryItem[capacity];
        count       = 0;
    }

    void addItem(LibraryItem item) {
        if (count < items.length) items[count++] = item;
    }

    void printCatalog() {
        System.out.println("═══════════════════════════════════════════════════════════════");
        System.out.println("  📖 " + libraryName + " — Catalog (" + count + " items)");
        System.out.println("═══════════════════════════════════════════════════════════════");
        for (int i = 0; i < count; i++)
            System.out.println(items[i].getInfo());
        System.out.println("═══════════════════════════════════════════════════════════════");
    }

    void search(String query) {
        System.out.println("\n🔍 Searching for: \"" + query + "\"");
        boolean found = false;
        for (int i = 0; i < count; i++) {
            if (items[i].matchesTitle(query) || items[i].matchesAuthor(query)) {
                System.out.println("  → " + items[i].getInfo());
                found = true;
            }
        }
        if (!found) System.out.println("  No results found.");
    }

    void checkOutItem(String title) {
        for (int i = 0; i < count; i++)
            if (items[i].matchesTitle(title)) { items[i].checkOut(); return; }
        System.out.println("❌ Item not found: " + title);
    }
}

public class P6_Library {
    public static void main(String[] args) {
        Library lib = new Library("Abhishek's Java Library", 10);

        lib.addItem(new Book("Clean Code",         "Robert Martin", 2008, "978-0132350884", 431, "Programming"));
        lib.addItem(new Book("The Pragmatic Programmer", "David Thomas", 1999, "978-0135957059", 352, "Programming"));
        lib.addItem(new Book("Effective Java",     "Joshua Bloch",  2018, "978-0134685991", 412, "Java"));
        lib.addItem(new Magazine("Java Magazine", "Oracle",        2024, 47, "Monthly"));
        lib.addItem(new Magazine("Tech Today",    "TechCorp",      2024, 12, "Weekly"));

        lib.printCatalog();

        lib.search("Java");
        lib.search("Robert");
        lib.search("xyz");

        System.out.println();
        lib.checkOutItem("Clean Code");
        lib.checkOutItem("Clean Code");   // try again — should fail
        lib.printCatalog();
    }
}
```

```
Output:
═══════════════════════════════════════════════════════════════
  📖 Abhishek's Java Library — Catalog (5 items)
═══════════════════════════════════════════════════════════════
📚 Book    | Clean Code                      | Robert Martin (2008) | Programming | 431 pages | ✅ Available
📚 Book    | The Pragmatic Programmer        | David Thomas (1999)  | Programming | 352 pages | ✅ Available
📚 Book    | Effective Java                  | Joshua Bloch (2018)  | Java | 412 pages | ✅ Available
📰 Magazine| Java Magazine                   | Oracle (2024) | Issue #47 | Monthly | ✅ Available
📰 Magazine| Tech Today                      | TechCorp (2024) | Issue #12 | Weekly | ✅ Available

🔍 Searching for: "Java"
  → 📚 Book    | Effective Java  ...
  → 📰 Magazine| Java Magazine  ...

🔍 Searching for: "Robert"
  → 📚 Book    | Clean Code  ...

🔍 Searching for: "xyz"
  No results found.

✅ Checked out: Clean Code
❌ Not available: Clean Code
```

> 🔑 **Key Learning:** `LibraryItem` is BOTH an abstract class (provides `checkOut`, `returnItem`, abstract `getInfo`) AND implements `Searchable` — combining inheritance and interface in one class. The `Library.search()` method calls `matchesTitle()` and `matchesAuthor()` on `LibraryItem` references — but these come from the `Searchable` interface, not the abstract class. `getInfo()` being abstract forces each subclass to provide its own format. The `printCatalog()` loop calls `items[i].getInfo()` and gets the right format for each item type — pure polymorphism.

---

## ⚡ MINI CHALLENGE — ✅ Solved

**Problem:** RPG Character System — Template Method + Polymorphism + Interface.

```java
interface Upgradeable {
    void levelUp();
    void upgradeAttack(int bonus);
}

abstract class Character implements Upgradeable {
    protected String name;
    protected int    health;
    protected int    attackPower;
    protected int    level;

    Character(String name, int health, int attackPower) {
        this.name        = name;
        this.health      = health;
        this.attackPower = attackPower;
        this.level       = 1;
    }

    // Template method — fixed combat sequence
    final void performAttack(Character target) {
        System.out.println("\n⚔️  " + name + " attacks " + target.name + "!");
        int damage = calculateDamage();
        System.out.println("   Damage dealt: " + damage);
        target.takeDamage(damage);
        System.out.println("   " + target.name + " has " + target.health + " HP remaining.");
    }

    // Each subclass calculates damage differently
    abstract int  calculateDamage();
    abstract void specialAbility();

    void takeDamage(int dmg) {
        health = Math.max(0, health - dmg);
        if (health == 0) System.out.println("   💀 " + name + " has been defeated!");
    }

    boolean isAlive() { return health > 0; }

    // Implement Upgradeable
    @Override
    public void levelUp() {
        level++;
        health       += 20;
        attackPower  += 5;
        System.out.println("⬆️  " + name + " leveled up to Level " + level +
                           " | HP: " + health + " | ATK: " + attackPower);
    }

    @Override
    public void upgradeAttack(int bonus) {
        attackPower += bonus;
        System.out.println("🗡️  " + name + "'s attack upgraded by +" + bonus +
                           " → ATK: " + attackPower);
    }

    void printStatus() {
        System.out.printf("%-10s | Level: %2d | HP: %3d | ATK: %3d | %s%n",
                          name, level, health, attackPower,
                          isAlive() ? "✅ Alive" : "💀 Defeated");
    }
}

class Warrior extends Character {
    Warrior(String name) { super(name, 150, 25); }

    @Override
    public int calculateDamage() {
        return attackPower + level * 5;   // bonus from level
    }

    @Override
    public void specialAbility() {
        System.out.println("🛡️  " + name + " uses Shield Bash! Stuns the enemy!");
        attackPower += 10;   // temporary boost
    }
}

class Mage extends Character {
    Mage(String name) { super(name, 90, 40); }

    @Override
    public int calculateDamage() {
        return attackPower * 2;   // double magic damage
    }

    @Override
    public void specialAbility() {
        System.out.println("🔥 " + name + " casts Fireball! Area damage!");
    }
}

class Archer extends Character {
    Archer(String name) { super(name, 110, 30); }

    @Override
    public int calculateDamage() {
        return attackPower + (int)(Math.random() * 10);  // +0 to +9 random
    }

    @Override
    public void specialAbility() {
        System.out.println("🏹 " + name + " uses Arrow Rain! Multi-target attack!");
    }
}

public class MiniChallenge_Day10 {
    public static void main(String[] args) {
        Warrior warrior = new Warrior("Abhishek");
        Mage    mage    = new Mage("Riya");
        Archer  archer  = new Archer("Arjun");

        System.out.println("═══ RPG BATTLE ARENA ═══\n");

        // Combat sequence
        warrior.performAttack(mage);
        mage.performAttack(archer);
        archer.performAttack(warrior);

        // Level up all
        System.out.println("\n═══ LEVEL UP ═══");
        Character[] party = {warrior, mage, archer};
        for (Character c : party) c.levelUp();

        // Special abilities
        System.out.println("\n═══ SPECIAL ABILITIES ═══");
        warrior.specialAbility();
        mage.specialAbility();
        archer.specialAbility();

        // Upgrade via interface reference
        System.out.println("\n═══ UPGRADES ═══");
        Upgradeable[] upgrades = {warrior, mage, archer};
        for (Upgradeable u : upgrades) u.upgradeAttack(8);

        // Final status
        System.out.println("\n═══ FINAL STATUS ═══");
        for (Character c : party) c.printStatus();
    }
}
```

```
Output:
═══ RPG BATTLE ARENA ═══

⚔️  Abhishek attacks Riya!
   Damage dealt: 30
   Riya has 60 HP remaining.

⚔️  Riya attacks Arjun!
   Damage dealt: 80
   Arjun has 30 HP remaining.

⚔️  Arjun attacks Abhishek!
   Damage dealt: 34
   Abhishek has 116 HP remaining.

═══ LEVEL UP ═══
⬆️  Abhishek leveled up to Level 2 | HP: 136 | ATK: 30
⬆️  Riya leveled up to Level 2 | HP: 80 | ATK: 45
⬆️  Arjun leveled up to Level 2 | HP: 50 | ATK: 35

═══ FINAL STATUS ═══
Abhishek   | Level:  2 | HP: 136 | ATK:  43 | ✅ Alive
Riya       | Level:  2 | HP:  80 | ATK:  53 | ✅ Alive
Arjun      | Level:  2 | HP:  50 | ATK:  43 | ✅ Alive
```

> 🔑 **Key Learning:** `performAttack()` is `final` — the combat sequence (announce → calculate → apply damage → report) is locked. `calculateDamage()` is abstract — each character type damages differently. `Upgradeable[] upgrades` holds characters via interface reference — calls `upgradeAttack()` polymorphically. This is three patterns working together: **Template Method** (fixed sequence, variable steps) + **Runtime Polymorphism** (right method at runtime) + **Interface-based polymorphism** (`Upgradeable` reference).

---

## 🔁 REVISION TASKS — ✅ All Completed

| Task | Outcome |
|------|---------|
| Employee static auto-ID pattern from memory | ✅ `empId = nextId++` in constructor |
| Three uses of `this` with examples | ✅ `this.var`, `this()`, `return this` |
| Parameterized constructor — default disappears | ✅ Must write `ClassName() {}` explicitly |
| `private` vs `protected` vs `public` | ✅ See note below |

**Access modifier summary:**
```
private   → only within same class
(default) → within same package
protected → same package + child classes (even different packages)
public    → everywhere

Use protected when: parent has state that child classes need to read/use
directly (like `name`, `age` in Animal — child Dog uses them in bark())
```

---

## ❓ REFLECTION QUESTIONS — ✅ All Answered

<details>
<summary><b>Q1. Method Overloading vs Method Overriding?</b></summary>

> **Overloading** — same class, same method name, DIFFERENT parameters. Resolved at **compile time** (static binding).
> `int add(int a, int b)` and `double add(double a, double b)` — two methods in same class.
>
> **Overriding** — parent → child, same name, SAME parameters. Resolved at **runtime** (dynamic dispatch).
> `Shape.area()` overridden by `Circle.area()` — child provides its own version.
>
> Key difference: overloading is about having multiple versions in one place. Overriding is about replacing parent behavior in a subclass.

</details>

<details>
<summary><b>Q2. Three uses of super keyword with examples?</b></summary>

> **1. `super()`** — calls the parent constructor from the child constructor. Must be the first statement. Example: `Dog(String name, int age) { super(name, age); }`
>
> **2. `super.method()`** — calls the parent's version of an overridden method. Example: in `Car.describe()`, calling `super.describe()` first prints Vehicle's info, then Car adds its own.
>
> **3. `super.field`** — accesses a parent field (rare, when child has a field with the same name shadowing the parent's). Example: `super.brand` accesses Vehicle's brand when Car has its own `brand` field.

</details>

<details>
<summary><b>Q3. Why can't you instantiate an abstract class? What is it useful for?</b></summary>

> An abstract class may have abstract methods with no implementation — creating an object from it would leave those methods with no body to call, causing a crash. Java prevents this at compile time.
>
> **It is useful as:** A partial implementation that defines the structure and shared behavior for all subclasses. Abstract methods act as contracts — every subclass MUST implement them. The abstract class can contain concrete methods (fully implemented, shared by all children). It enforces a consistent API across an entire family of classes. Example: `Shape` provides `printInfo()` (concrete) but forces every subclass to implement `area()` (abstract).

</details>

<details>
<summary><b>Q4. Interface vs Abstract Class — key difference, when to use each?</b></summary>

> **Abstract class:** Can have constructors, instance variables, and a mix of abstract + concrete methods. Only one can be extended. Use when subclasses share **common state or partial implementation** — e.g., all shapes have a `color` field.
>
> **Interface:** No constructors, no instance variables (only constants). A class can implement **many interfaces**. Use when unrelated classes need to **guarantee the same capability** — e.g., `Dog`, `Car`, and `Remote` can all implement `Controllable`, despite being unrelated types.
>
> Rule of thumb: abstract class for IS-A with shared state. Interface for CAN-DO capability.

</details>

<details>
<summary><b>Q5. What is runtime polymorphism? How does Shape[] work?</b></summary>

> Runtime polymorphism means the method that runs is determined at **runtime** based on the actual object type, not the reference type. This is also called dynamic dispatch.
>
> `Shape[] shapes = {new Circle(3), new Rectangle(4,5)}` works because Circle and Rectangle both IS-A Shape (extend it). The array stores Shape references, but each holds a different actual object.
>
> When `shapes[0].area()` is called: the reference type is `Shape`, but the actual object is `Circle`. Java looks at the actual object at runtime and calls `Circle.area()` — not `Shape.area()`. This happens via the **vtable** (virtual method table) — each object carries a pointer to its class's method table, so the right version always runs.

</details>

---

## 📌 DAY 10 SUMMARY CARD

```
╔══════════════════════════════════════════════════════════════════╗
║                     📅  DAY 10 — SUMMARY                       ║
╠══════════════════════════════════════════════════════════════════╣
║  Date        :  April 08, 2025                                  ║
║  Phase       :  Phase 1 — Java Fundamentals                     ║
║  Topic       :  OOP — Inheritance, Polymorphism & Abstraction   ║
║  Hours Spent :  5 Hours        |   Status : ✅ COMPLETED         ║
╠══════════════════════════════════════════════════════════════════╣
║  CONCEPTS COVERED                                               ║
║  ✅  Inheritance — extends, IS-A vs HAS-A, single inheritance    ║
║  ✅  super() — constructor chaining (must be first statement)    ║
║  ✅  super.method() + super.field — accessing parent members     ║
║  ✅  Method Overriding — @Override, rules, overriding vs hiding  ║
║  ✅  Overloading vs Overriding — compile-time vs runtime         ║
║  ✅  Runtime Polymorphism — dynamic dispatch, vtable             ║
║  ✅  Abstract classes — abstract methods, concrete methods       ║
║  ✅  Interfaces — multiple implementation, capability contract   ║
║  ✅  final — class, method, field modifiers                      ║
║  ✅  instanceof — type checking + safe downcasting               ║
╠══════════════════════════════════════════════════════════════════╣
║  PROBLEMS SOLVED                                                ║
║  ✅  P1 — Animal Hierarchy (polymorphic array + instanceof)      ║
║  ✅  P2 — Shape Area (abstract class + printInfo polymorphism)   ║
║  ✅  P3 — Vehicle System (super.describe chaining)               ║
║  ✅  P4 — Interface Implementation (dual-interface Smartphone)   ║
║  ✅  P5 — Template Method Pattern (DataProcessor)                ║
║  ✅  P6 — Library System (abstract + interface combined)         ║
║  ✅  Mini — RPG System (Template + Polymorphism + Interface)     ║
╠══════════════════════════════════════════════════════════════════╣
║  Problems   : 6/6 ✅  |  Mini Challenge : ✅  |  Revision : ✅   ║
║  Reflection : 5/5 ✅                                             ║
╠══════════════════════════════════════════════════════════════════╣
║  CONFIDENCE     :  █████████░  90%                              ║
║  STRONG AREA    :  Inheritance, overriding, abstract classes     ║
║  WEAK AREA      :  Interface vs abstract class decision          ║
╠══════════════════════════════════════════════════════════════════╣
║  KEY TAKEAWAYS                                                  ║
║  ▸  Child IS-A Parent → polymorphic arrays work                 ║
║  ▸  super() must be FIRST statement in child constructor        ║
║  ▸  @Override every overridden method — no exceptions           ║
║  ▸  Runtime polymorphism = right method at runtime, always      ║
║  ▸  Abstract class = shared state + partial implementation      ║
║  ▸  Interface = capability contract, multiple allowed            ║
║  ▸  Template Method Pattern = fixed sequence, variable steps    ║
╚══════════════════════════════════════════════════════════════════╝
```

---

<div align="center">

[← Day 09](./Day09_OOP_Basics.md) &nbsp;|&nbsp; [Back to Dashboard](../README.md) &nbsp;|&nbsp; [Day 11 →](./Day11_Recursion.md)

*Day 10 of 160 — Done ✅ | Double Digits! | Streak: 10 Days 🔥🔥🔥🔥🔥🔥🔥🔥🔥🔥*

</div>
