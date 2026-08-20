# C++ OOP & Memory Management — Interview Notes

This is a **tech-interview focused set of notes** covering the concepts most commonly asked in C++ OOP, memory, constructors/destructors, inheritance, polymorphism, smart pointers, RAII, virtual functions, copy/move semantics, and common interview traps.

---

# 1. C++ OOP Fundamentals

The four core pillars of OOP are:

1. **Encapsulation**
2. **Abstraction**
3. **Inheritance**
4. **Polymorphism**

Additional concepts you should know:

- Classes and objects
- Constructors/destructors
- `this` pointer
- Static members
- `const`
- Virtual functions
- Pure virtual functions
- Abstract classes
- Object slicing
- Copy constructor
- Copy assignment
- Move constructor
- Move assignment
- Rule of 3/5/0
- Composition vs inheritance

---

# 2. Class vs Object

A **class** is a blueprint.

An **object** is an instance of a class.

```cpp
class Car {
public:
    string brand;
    int speed;

    void drive() {
        cout << "Driving\n";
    }
};

int main() {
    Car c1;
    c1.brand = "BMW";
    c1.speed = 100;

    c1.drive();
}
```

Here:

- `Car` → class
- `c1` → object

### Memory

When you create:

```cpp
Car c1;
```

the object occupies memory.

Member functions generally exist as program code and are **not duplicated for every object**. Each object primarily contains its non-static data members.

---

# 3. Access Specifiers

C++ provides:

```cpp
public
private
protected
```

### `public`

Accessible from anywhere the object is accessible.

```cpp
class A {
public:
    int x;
};
```

### `private`

Accessible only inside the class and its friends.

```cpp
class A {
private:
    int x;
};
```

### `protected`

Accessible inside:

- the class
- derived classes
- friends

but generally not directly through an ordinary object from outside.

---

# 4. Encapsulation

**Encapsulation = bundling data and methods together + controlling access to data.**

Example:

```cpp
class BankAccount {
private:
    double balance;

public:
    void deposit(double amount) {
        if (amount > 0)
            balance += amount;
    }

    double getBalance() const {
        return balance;
    }
};
```

The caller cannot directly do:

```cpp
account.balance = -100000;
```

because `balance` is private.

### Interview answer

> Encapsulation protects an object's internal state by restricting direct access and exposing controlled interfaces.

---

# 5. Abstraction

Abstraction means exposing **what an object does** while hiding **how it does it**.

Example:

```cpp
class CoffeeMachine {
public:
    void makeCoffee() {
        heatWater();
        grindBeans();
        brew();
    }

private:
    void heatWater() {}
    void grindBeans() {}
    void brew() {}
};
```

User only needs:

```cpp
machine.makeCoffee();
```

The implementation is hidden.

### Encapsulation vs Abstraction

| Encapsulation                         | Abstraction                                        |
| ------------------------------------- | -------------------------------------------------- |
| Protects data                         | Hides implementation                               |
| Achieved using access control/classes | Achieved using interfaces/classes/abstract classes |
| Focus: "How do I protect it?"         | Focus: "What should the user see?"                 |

---

# 6. Inheritance

Inheritance allows one class to acquire properties/behavior of another.

```cpp
class Animal {
public:
    void eat() {
        cout << "Eating\n";
    }
};

class Dog : public Animal {
public:
    void bark() {
        cout << "Barking\n";
    }
};
```

Usage:

```cpp
Dog d;

d.eat();
d.bark();
```

`Dog` is the derived class.

`Animal` is the base class.

---

# 7. Types of Inheritance

### Single inheritance

```text
A
|
B
```

```cpp
class B : public A {};
```

### Multilevel inheritance

```text
A
|
B
|
C
```

### Multiple inheritance

```text
A     B
 \   /
   C
```

```cpp
class C : public A, public B {};
```

### Hierarchical inheritance

```text
      A
     / \
    B   C
```

### Hybrid inheritance

Combination of multiple inheritance types.

---

# 8. Public / Protected / Private Inheritance

Example:

```cpp
class Child : public Parent {};
```

For **public inheritance**:

| Parent member | Child                 |
| ------------- | --------------------- |
| public        | public                |
| protected     | protected             |
| private       | inaccessible directly |

For **protected inheritance**:

| Parent member | Child        |
| ------------- | ------------ |
| public        | protected    |
| protected     | protected    |
| private       | inaccessible |

For **private inheritance**:

| Parent member | Child        |
| ------------- | ------------ |
| public        | private      |
| protected     | private      |
| private       | inaccessible |

### Interview tip

Most real-world "is-a" relationships use:

```cpp
class Dog : public Animal
```

---

# 9. Composition vs Inheritance

Very important design question.

### Inheritance

Represents:

> **is-a**

```text
Dog is an Animal
```

```cpp
class Dog : public Animal {};
```

### Composition

Represents:

> **has-a**

```text
Car has an Engine
```

```cpp
class Engine {};

class Car {
    Engine engine;
};
```

In many designs, **composition is preferred over inheritance** because it reduces coupling.

---

# 10. Constructors

A constructor initializes an object.

```cpp
class Student {
public:
    string name;
    int age;

    Student(string n, int a) {
        name = n;
        age = a;
    }
};
```

Better:

```cpp
Student(string n, int a)
    : name(n), age(a) {}
```

Usage:

```cpp
Student s("Rahul", 22);
```

---

# 11. Constructor Initialization List

Prefer:

```cpp
class A {
    int x;
    string name;

public:
    A(int x, string name)
        : x(x), name(name) {}
};
```

rather than:

```cpp
A(int x, string name) {
    this->x = x;
    this->name = name;
}
```

Initialization lists are **required** for:

- `const` data members
- references
- members without default constructors

Example:

```cpp
class A {
    const int x;
    int& ref;

public:
    A(int value, int& r)
        : x(value), ref(r) {}
};
```

---

# 12. Types of Constructors

### Default constructor

```cpp
class A {
public:
    A() {}
};
```

### Parameterized constructor

```cpp
A(int x) {}
```

### Copy constructor

```cpp
A(const A& other) {}
```

### Move constructor

```cpp
A(A&& other) {}
```

### Constructor overloading

```cpp
class A {
public:
    A() {}
    A(int x) {}
    A(int x, int y) {}
};
```

---

# 13. Destructor

Destructor executes when an object is destroyed.

Syntax:

```cpp
~ClassName() {
}
```

Example:

```cpp
class A {
public:
    A() {
        cout << "Constructor\n";
    }

    ~A() {
        cout << "Destructor\n";
    }
};
```

For:

```cpp
A obj;
```

constructor executes when `obj` is created.

Destructor executes when `obj` goes out of scope.

---

# 14. Constructor/Destructor Order

Consider:

```cpp
class A {
public:
    A() { cout << "A constructor\n"; }
    ~A() { cout << "A destructor\n"; }
};

class B : public A {
public:
    B() { cout << "B constructor\n"; }
    ~B() { cout << "B destructor\n"; }
};
```

```cpp
B obj;
```

Output:

```text
A constructor
B constructor
B destructor
A destructor
```

### Rule

Construction:

```text
Base → Derived
```

Destruction:

```text
Derived → Base
```

---

# 15. `this` Pointer

Inside a non-static member function, `this` points to the current object.

```cpp
class Student {
    int age;

public:
    Student(int age) {
        this->age = age;
    }
};
```

Here:

```cpp
this->age
```

means the object's member.

---

# 16. Static Data Members

A static data member belongs to the **class**, not individual objects.

```cpp
class Employee {
public:
    static int count;

    Employee() {
        count++;
    }
};

int Employee::count = 0;
```

```cpp
Employee e1;
Employee e2;

cout << Employee::count;
```

Output:

```text
2
```

There is only one `count` shared by all objects.

---

# 17. Static Member Functions

```cpp
class A {
public:
    static void hello() {
        cout << "Hello";
    }
};
```

Call:

```cpp
A::hello();
```

A static member function does **not have a `this` pointer**.

Therefore it cannot directly access non-static members.

---

# 18. Polymorphism

Polymorphism means:

> One interface, multiple implementations.

Two major types:

### Compile-time polymorphism

- Function overloading
- Operator overloading
- Templates

### Runtime polymorphism

- Virtual functions
- Function overriding

---

# 19. Function Overloading

Same function name, different parameter list.

```cpp
class Calculator {
public:
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
};
```

This is resolved at compile time.

### Important

Return type alone cannot overload a function.

Invalid:

```cpp
int add(int a);
double add(int a); // Error
```

---

# 20. Function Overriding

A derived class provides its own implementation of a base-class virtual function.

```cpp
class Animal {
public:
    virtual void sound() {
        cout << "Animal sound\n";
    }
};

class Dog : public Animal {
public:
    void sound() override {
        cout << "Bark\n";
    }
};
```

---

# 21. Virtual Function

This is one of the **most important C++ interview topics**.

```cpp
class Animal {
public:
    virtual void sound() {
        cout << "Animal\n";
    }
};

class Dog : public Animal {
public:
    void sound() override {
        cout << "Dog\n";
    }
};
```

Now:

```cpp
Animal* ptr = new Dog();

ptr->sound();
```

Output:

```text
Dog
```

Without `virtual`:

```cpp
Animal* ptr = new Dog();
ptr->sound();
```

would call `Animal::sound()`.

---

# 22. Why Virtual Functions?

Because the actual object is `Dog`, even though the pointer type is `Animal*`.

This enables **runtime polymorphism**.

Conceptually:

```text
Animal*
   |
   v
  Dog object
   |
   v
Dog::sound()
```

---

# 23. Virtual Function Internals: vtable and vptr

A common interview question:

> How are virtual functions implemented?

Typically, implementations use:

- **vtable** — virtual function table
- **vptr** — hidden pointer inside objects of polymorphic classes

Conceptually:

```text
Dog object
+----------------+
| vptr ----------|----> Dog vtable
| data members   |       |
+----------------+       +--> Dog::sound()
```

The exact implementation is compiler-dependent, so say **"typically"**, not "C++ guarantees vtable."

---

# 24. Pure Virtual Function

Syntax:

```cpp
virtual void draw() = 0;
```

Example:

```cpp
class Shape {
public:
    virtual void draw() = 0;
};
```

This makes `Shape` an abstract class.

You cannot do:

```cpp
Shape s; // Error
```

Derived classes must implement the function.

```cpp
class Circle : public Shape {
public:
    void draw() override {
        cout << "Circle\n";
    }
};
```

---

# 25. Abstract Class

A class containing at least one pure virtual function is abstract.

```cpp
class Vehicle {
public:
    virtual void start() = 0;
};
```

You cannot instantiate it:

```cpp
Vehicle v; // Error
```

But:

```cpp
Vehicle* v = new Car();
```

can be valid if `Car` implements all required pure virtual functions.

---

# 26. Virtual Destructor — Extremely Important

Suppose:

```cpp
class Base {
public:
    ~Base() {}
};

class Derived : public Base {
public:
    ~Derived() {
        cout << "Derived destroyed\n";
    }
};
```

Now:

```cpp
Base* ptr = new Derived();
delete ptr;
```

This is dangerous because the base destructor isn't virtual.

Use:

```cpp
class Base {
public:
    virtual ~Base() {}
};
```

Now:

```cpp
Base* ptr = new Derived();
delete ptr;
```

correctly destroys:

```text
Derived
↓
Base
```

### Interview rule

> If a class is intended to be used polymorphically and deleted through a base pointer, its destructor should be virtual.

---

# 27. Object Slicing

Very common interview question.

```cpp
class Animal {
public:
    int x;
};

class Dog : public Animal {
public:
    int y;
};
```

Now:

```cpp
Dog d;

Animal a = d;
```

The derived-specific part is sliced away.

Conceptually:

```text
Dog
+------+
| x    |
| y    |
+------+

       ↓ copy as Animal

Animal
+------+
| x    |
+------+
```

Avoid slicing by using references/pointers:

```cpp
Animal& a = d;
```

or:

```cpp
Animal* a = &d;
```

---

# 28. Virtual vs Pure Virtual

### Virtual

Can have a default implementation.

```cpp
virtual void foo() {
    cout << "Base";
}
```

### Pure virtual

No required implementation for normal overriding purposes.

```cpp
virtual void foo() = 0;
```

Class becomes abstract if it has a pure virtual function.

---

# 29. `override`

Use:

```cpp
void sound() override;
```

instead of merely:

```cpp
void sound();
```

`override` tells the compiler:

> I intend to override a virtual function from the base class.

If you accidentally change the signature:

```cpp
class Base {
public:
    virtual void foo(int);
};

class Derived : public Base {
public:
    void foo(double) override; // compiler error
};
```

This catches bugs.

---

# 30. `final`

Prevent further overriding:

```cpp
class Base {
public:
    virtual void foo() final {}
};
```

Derived classes cannot override `foo()`.

You can also prevent class inheritance:

```cpp
class A final {};
```

Then:

```cpp
class B : public A {}; // Error
```

---

# 31. Friend Function

A friend function can access private/protected members.

```cpp
class Box {
private:
    int value = 10;

    friend void print(Box&);
};

void print(Box& b) {
    cout << b.value;
}
```

Friendship is:

- not inherited
- not automatically mutual
- not transitive

Use it carefully because it weakens encapsulation.

---

# 32. Operator Overloading

Allows operators to work with user-defined types.

```cpp
class Complex {
public:
    int real, imag;

    Complex(int r, int i)
        : real(r), imag(i) {}

    Complex operator+(const Complex& other) {
        return Complex(
            real + other.real,
            imag + other.imag
        );
    }
};
```

Usage:

```cpp
Complex a(2, 3);
Complex b(4, 5);

Complex c = a + b;
```

---

# 33. Important Operators That Cannot Be Overloaded

Common examples:

```text
::
.
.*
?:
sizeof
```

Some operators also have restrictions on how they can be overloaded.

---

# 34. Copy Constructor

A copy constructor creates an object from another object.

```cpp
class Student {
public:
    int age;

    Student(int a) : age(a) {}

    Student(const Student& other) {
        age = other.age;
    }
};
```

Usage:

```cpp
Student s1(20);
Student s2 = s1;
```

---

# 35. Copy Assignment

Different from copy construction.

```cpp
Student s1(20);
Student s2(30);

s2 = s1;
```

This invokes **copy assignment**, not copy construction.

### Difference

```cpp
Student s2 = s1;
```

→ copy constructor

```cpp
s2 = s1;
```

→ copy assignment operator

---

# 36. Shallow Copy

Consider:

```cpp
class A {
public:
    int* ptr;

    A(int x) {
        ptr = new int(x);
    }
};
```

Default copying can copy the pointer:

```text
A1              A2
 |               |
 v               v
+---+           +---+
| 10|           | 10|
+---+           +---+
 ^               ^
 |_______________|
    same memory
```

Both objects point to the same allocation.

This can lead to:

- double deletion
- dangling pointers
- unintended modification

---

# 37. Deep Copy

Deep copy allocates separate memory.

```cpp
class A {
    int* ptr;

public:
    A(int x) {
        ptr = new int(x);
    }

    A(const A& other) {
        ptr = new int(*other.ptr);
    }

    ~A() {
        delete ptr;
    }
};
```

Now:

```text
A1 → memory 1
A2 → memory 2
```

The values are copied, but memory is independent.

---

# 38. Rule of 3

If a class manually manages a resource and needs one of:

1. Destructor
2. Copy constructor
3. Copy assignment operator

it often needs all three.

Example:

```cpp
class Resource {
    int* data;

public:
    Resource(int value) {
        data = new int(value);
    }

    ~Resource() {
        delete data;
    }

    Resource(const Resource& other) {
        data = new int(*other.data);
    }

    Resource& operator=(const Resource& other) {
        if (this != &other) {
            delete data;
            data = new int(*other.data);
        }
        return *this;
    }
};
```

---

# 39. Move Semantics

Introduced in C++11.

Instead of copying a resource, we can **transfer ownership**.

Copy:

```text
Object A
  |
  v
Resource

        copy

Object B
  |
  v
New Resource
```

Move:

```text
Object A
  |
  v
Resource

        move

Object B
  |
  v
Resource
```

A moved-from object remains valid but its exact state is generally unspecified unless documented otherwise.

---

# 40. Move Constructor

```cpp
class Buffer {
    int* data;

public:
    Buffer(int value) {
        data = new int(value);
    }

    Buffer(Buffer&& other) noexcept {
        data = other.data;
        other.data = nullptr;
    }

    ~Buffer() {
        delete data;
    }
};
```

Instead of allocating/copying the resource, ownership is transferred.

---

# 41. Move Assignment

```cpp
Buffer& operator=(Buffer&& other) noexcept {
    if (this != &other) {
        delete data;

        data = other.data;
        other.data = nullptr;
    }

    return *this;
}
```

---

# 42. Rule of 5

If you manually manage a resource, the five special member functions are:

```cpp
Destructor
Copy constructor
Copy assignment
Move constructor
Move assignment
```

A typical resource-owning class may define all five.

---

# 43. Rule of 0

Modern C++ recommendation:

> Avoid manually managing resources whenever possible.

Use RAII types such as:

```cpp
std::vector
std::string
std::unique_ptr
std::shared_ptr
```

Then you often don't need custom:

```cpp
~Destructor()
Copy constructor
Copy assignment
Move constructor
Move assignment
```

This is the **Rule of 0**.

---

# 44. Stack vs Heap

### Stack

```cpp
int x = 10;
```

Usually automatic storage duration.

Destroyed automatically when its scope ends.

### Heap / dynamic storage

```cpp
int* p = new int(10);
```

Requires proper lifetime management:

```cpp
delete p;
```

Conceptually:

```text
Stack
+-------------+
| x           |
| pointer p --|------+
+-------------+      |
                     v
Heap             +-------+
                 |  10   |
                 +-------+
```

---

# 45. `new` and `delete`

```cpp
int* p = new int(10);

cout << *p;

delete p;
p = nullptr;
```

For arrays:

```cpp
int* arr = new int[10];

delete[] arr;
```

### Important

```cpp
new      → delete
new[]    → delete[]
```

Don't mix them.

---

# 46. Memory Leak

A memory leak occurs when allocated memory becomes unreachable without being released.

```cpp
void foo() {
    int* p = new int(10);
}
```

When `foo()` ends, `p` disappears but the allocated integer remains.

Result:

```text
Memory allocated
      ↓
Pointer lost
      ↓
Memory cannot be accessed
      ↓
Memory leak
```

---

# 47. Dangling Pointer

A dangling pointer points to memory that is no longer valid.

Example:

```cpp
int* p;

{
    int x = 10;
    p = &x;
}

cout << *p; // undefined behavior
```

`x` no longer exists.

Another example:

```cpp
int* p = new int(10);

delete p;

cout << *p; // Undefined behavior
```

Better:

```cpp
delete p;
p = nullptr;
```

---

# 48. Double Delete

Bad:

```cpp
int* p = new int(10);

delete p;
delete p; // Undefined behavior
```

Set pointer to `nullptr` after deletion:

```cpp
delete p;
p = nullptr;
```

Deleting `nullptr` is safe:

```cpp
delete nullptr;
```

---

# 49. Use-After-Free

```cpp
int* p = new int(10);

delete p;

*p = 20; // Undefined behavior
```

The memory has already been released.

---

# 50. Smart Pointers

Modern C++ strongly favors smart pointers over raw owning pointers.

Main types:

```cpp
std::unique_ptr
std::shared_ptr
std::weak_ptr
```

---

# 51. `unique_ptr`

Represents **exclusive ownership**.

```cpp
#include <memory>

std::unique_ptr<int> p =
    std::make_unique<int>(10);
```

You don't manually call:

```cpp
delete
```

When `p` goes out of scope, the resource is automatically released.

---

# 52. `unique_ptr` Cannot Be Copied

```cpp
auto p1 = std::make_unique<int>(10);

auto p2 = p1; // Error
```

Why?

Because two objects cannot have exclusive ownership.

Instead:

```cpp
auto p2 = std::move(p1);
```

Now:

```text
p1 → nullptr
p2 → resource
```

---

# 53. `shared_ptr`

Represents shared ownership.

```cpp
auto p1 = std::make_shared<int>(10);

auto p2 = p1;
```

Now both share ownership.

Conceptually:

```text
p1 ----\
        ---> resource
p2 ----/
         |
    reference count = 2
```

When the last `shared_ptr` is destroyed, the resource is destroyed.

---

# 54. Reference Counting

A `shared_ptr` typically maintains a control block containing ownership information, including a reference count.

Example:

```cpp
auto p1 = std::make_shared<int>(10);

cout << p1.use_count(); // 1

auto p2 = p1;

cout << p1.use_count(); // 2
```

When `p2` disappears:

```text
count = 1
```

When `p1` disappears:

```text
count = 0
resource destroyed
```

Implementation details are standard-library-dependent, but this is the usual model.

---

# 55. `weak_ptr`

`weak_ptr` observes an object managed by `shared_ptr` **without owning it**.

```cpp
auto shared = std::make_shared<int>(10);

std::weak_ptr<int> weak = shared;
```

`weak` doesn't keep the resource alive.

To access:

```cpp
if (auto p = weak.lock()) {
    cout << *p;
}
```

---

# 56. Why `weak_ptr` Exists

Most important reason:

> Prevent `shared_ptr` reference cycles.

Example:

```text
A shared_ptr → B
B shared_ptr → A
```

Reference counts never reach zero.

Memory leaks.

Use:

```text
A shared_ptr → B
B weak_ptr   → A
```

---

# 57. `make_unique` vs `make_shared`

Prefer:

```cpp
auto p = std::make_unique<MyClass>();
```

and:

```cpp
auto p = std::make_shared<MyClass>();
```

over manually writing:

```cpp
std::unique_ptr<MyClass>(
    new MyClass()
);
```

and:

```cpp
std::shared_ptr<MyClass>(
    new MyClass()
);
```

Benefits include safer construction and cleaner code.

`make_shared` commonly allows the object and its control block to be allocated efficiently together, though exact allocation details are implementation-dependent.

---

# 58. RAII

One of the most important C++ concepts.

**RAII = Resource Acquisition Is Initialization**

Resource lifetime is tied to object lifetime.

Example:

```cpp
class File {
public:
    File() {
        // open file
    }

    ~File() {
        // close file
    }
};
```

Usage:

```cpp
void process() {
    File file;

    // use file

} // destructor automatically runs
```

This works even if an exception causes the function to exit early.

RAII applies to:

- memory
- files
- mutex locks
- sockets
- database connections
- other resources

---

# 59. RAII + Smart Pointer

Instead of:

```cpp
void foo() {
    int* p = new int(10);

    // complicated code

    delete p;
}
```

use:

```cpp
void foo() {
    auto p = std::make_unique<int>(10);

    // complicated code
}
```

The resource is automatically released.

---

# 60. `const` Member Function

```cpp
class Student {
    int age;

public:
    int getAge() const {
        return age;
    }
};
```

The `const` after the function means:

> This function promises not to modify the object's ordinary non-mutable state.

You can call it on a const object:

```cpp
const Student s;

s.getAge();
```

---

# 61. `const` Pointer Variations

Very common interview question.

### Pointer to const

```cpp
const int* p;
```

You can change `p`:

```cpp
p = &b;
```

but cannot modify the value through `p`:

```cpp
*p = 10; // Error
```

### Const pointer

```cpp
int* const p = &a;
```

You can modify:

```cpp
*p = 10;
```

but cannot make `p` point somewhere else.

### Const pointer to const

```cpp
const int* const p = &a;
```

Neither the pointer nor the pointed-to value can be modified through `p`.

---

# 62. `const` Reference

```cpp
void print(const string& s) {
    cout << s;
}
```

Benefits:

- avoids copying
- cannot modify `s`
- can accept temporary objects

This is extremely common in C++ interview code.

---

# 63. Pass by Value vs Reference

### Pass by value

```cpp
void foo(vector<int> v)
```

Copies the vector.

### Pass by reference

```cpp
void foo(vector<int>& v)
```

No copy; can modify original.

### Const reference

```cpp
void foo(const vector<int>& v)
```

No copy and cannot modify original.

For large read-only objects, this is a common choice.

---

# 64. `nullptr`

Modern C++ uses:

```cpp
nullptr
```

instead of:

```cpp
NULL
```

or:

```cpp
0
```

Example:

```cpp
int* p = nullptr;
```

`nullptr` has a dedicated null-pointer type and avoids certain overload ambiguities associated with `0`/`NULL`.

---

# 65. Memory Layout of a Typical C++ Object

Conceptually, an object may contain:

```text
Object
+----------------------+
| data member 1        |
| data member 2        |
| ...                  |
| vptr (if applicable) |
+----------------------+
```

Important caveat:

The exact layout, padding, ABI details, and virtual dispatch representation are implementation-dependent.

---

# 66. Padding and Alignment

Consider:

```cpp
struct A {
    char c;
    int x;
};
```

You might expect:

```text
1 + 4 = 5 bytes
```

But due to alignment/padding, `sizeof(A)` can commonly be:

```text
8
```

Conceptually:

```text
+----+---------+---------+
| c  | padding |   x     |
+----+---------+---------+
```

Interview question:

> Why isn't `sizeof(struct)` always the sum of member sizes?

Answer:

> Because compilers insert padding to satisfy alignment requirements.

---

# 67. Virtual Inheritance and Diamond Problem

Consider:

```text
       A
      / \
     B   C
      \ /
       D
```

Without virtual inheritance, `D` can contain two copies of `A`.

```cpp
class A {};

class B : public A {};
class C : public A {};

class D : public B, public C {};
```

Now:

```cpp
D d;
```

There are two `A` subobjects.

---

# 68. Solving Diamond Problem

Use virtual inheritance:

```cpp
class A {};

class B : virtual public A {};
class C : virtual public A {};

class D : public B, public C {};
```

Now `D` has one shared `A` base subobject.

---

# 69. Multiple Inheritance Ambiguity

```cpp
class A {
public:
    void show() {}
};

class B {
public:
    void show() {}
};

class C : public A, public B {};
```

Now:

```cpp
C obj;

// obj.show(); // ambiguous
```

Resolve using:

```cpp
obj.A::show();
```

or:

```cpp
obj.B::show();
```

---

# 70. Virtual Function + Multiple Inheritance

A common interview discussion is that multiple inheritance can complicate:

- object layout
- pointer adjustments
- virtual dispatch
- ambiguity

Don't simply say "multiple inheritance is bad."

Better answer:

> Multiple inheritance is supported by C++, but it can increase complexity. It is useful in some designs, while composition or interfaces can often provide a simpler alternative.

---

# 71. Static vs Dynamic Binding

### Static binding

Function call determined at compile time.

Examples:

```cpp
function overloading
non-virtual functions
```

### Dynamic binding

Function selected at runtime.

Example:

```cpp
virtual functions
```

---

# 72. Early Binding vs Late Binding

Usually:

```text
Early binding = compile time
Late binding  = runtime
```

Runtime polymorphism relies on late/dynamic dispatch.

---

# 73. Constructor Cannot Be Virtual

You cannot write:

```cpp
virtual A();
```

Why?

The constructor is responsible for creating/initializing the object, and virtual dispatch requires an object whose dynamic type is already established.

### Destructor can be virtual

```cpp
virtual ~A() {}
```

because destruction through a base pointer is a legitimate polymorphic operation.

---

# 74. Can a Constructor Call a Virtual Function?

Technically yes, but it **does not dispatch to the most-derived override**.

Example:

```cpp
class Base {
public:
    Base() {
        foo();
    }

    virtual void foo() {
        cout << "Base";
    }
};

class Derived : public Base {
public:
    void foo() override {
        cout << "Derived";
    }
};
```

During `Base` construction:

```text
Base::foo()
```

is called, not `Derived::foo()`.

Similarly, during base destruction, virtual dispatch doesn't reach a more-derived object's override.

### Interview rule

> Avoid calling virtual functions from constructors/destructors when you expect derived-class behavior.

---

# 75. Pure Virtual Destructor

A destructor can be pure virtual:

```cpp
class Base {
public:
    virtual ~Base() = 0;
};
```

But it still needs a definition:

```cpp
Base::~Base() {}
```

This makes the class abstract.

---

# 76. `explicit` Constructor

Consider:

```cpp
class Number {
public:
    Number(int x) {}
};
```

Then:

```cpp
Number n = 10;
```

may perform implicit conversion.

Use:

```cpp
class Number {
public:
    explicit Number(int x) {}
};
```

Now:

```cpp
Number n = 10; // Error
```

but:

```cpp
Number n(10);
```

works.

### Interview answer

> `explicit` prevents unwanted implicit conversions through converting constructors.

---

# 77. Defaulted and Deleted Functions

You can explicitly request the compiler-generated version:

```cpp
class A {
public:
    A() = default;
};
```

Or disable operations:

```cpp
class A {
public:
    A(const A&) = delete;
};
```

Now copying is prohibited.

Useful for:

- mutex-like objects
- unique resources
- singleton-like restrictions
- non-copyable classes

---

# 78. Rule of 0/3/5 — Quick Revision

| Rule      | Meaning                                                                    |
| --------- | -------------------------------------------------------------------------- |
| Rule of 0 | Prefer RAII members; define none of special members                        |
| Rule of 3 | Resource ownership often requires destructor + copy ctor + copy assignment |
| Rule of 5 | Add move ctor + move assignment in modern C++                              |

---

# 79. Ownership Model

When reviewing code, ask:

### Who owns this resource?

For exclusive ownership:

```cpp
unique_ptr
```

For shared ownership:

```cpp
shared_ptr
```

For non-owning observation:

```cpp
weak_ptr
```

or raw/reference types when appropriate.

This mindset is extremely useful in interviews.

---

# 80. Raw Pointer vs Smart Pointer

A raw pointer:

```cpp
Node* p;
```

doesn't by itself tell you:

> Who owns `Node`?

A smart pointer communicates ownership.

```cpp
std::unique_ptr<Node> p;
```

means:

> This object owns the Node exclusively.

This makes code easier to reason about.

---

# 81. Smart Pointer Example

```cpp
#include <iostream>
#include <memory>

class Animal {
public:
    virtual void sound() {
        std::cout << "Animal\n";
    }

    virtual ~Animal() = default;
};

class Dog : public Animal {
public:
    void sound() override {
        std::cout << "Bark\n";
    }
};

int main() {
    std::unique_ptr<Animal> animal =
        std::make_unique<Dog>();

    animal->sound();
}
```

Output:

```text
Bark
```

This combines:

- inheritance
- runtime polymorphism
- virtual destructor
- RAII
- `unique_ptr`

---

# 82. `unique_ptr` with Polymorphism

Very common design:

```cpp
vector<unique_ptr<Animal>> animals;

animals.push_back(make_unique<Dog>());
animals.push_back(make_unique<Cat>());
```

Then:

```cpp
for (const auto& animal : animals) {
    animal->sound();
}
```

This gives runtime polymorphism without manual `delete`.

---

# 83. Why `unique_ptr` Is Usually Preferred

If you don't need shared ownership:

```cpp
unique_ptr
```

is generally preferable to:

```cpp
shared_ptr
```

because ownership is explicit and there is less overhead/complexity associated with shared ownership.

Interview answer:

> Use `shared_ptr` only when ownership genuinely needs to be shared.

---

# 84. `shared_ptr` Cycle Example

```cpp
class B;

class A {
public:
    shared_ptr<B> b;
};

class B {
public:
    shared_ptr<A> a;
};
```

If:

```text
A → B
B → A
```

both reference counts can remain non-zero.

Solution:

```cpp
weak_ptr<A> a;
```

on one side.

---

# 85. `new` vs `malloc`

### `new`

```cpp
A* p = new A();
```

- allocates memory
- calls constructor
- returns typed pointer

### `malloc`

```cpp
A* p = (A*)malloc(sizeof(A));
```

- allocates raw memory
- does not call constructor
- C-style memory allocation

For C++ objects, prefer C++ mechanisms.

---

# 86. `delete` vs `free`

Memory allocated by:

```cpp
new
```

must be released with:

```cpp
delete
```

Memory allocated by:

```cpp
new[]
```

with:

```cpp
delete[]
```

Memory allocated with:

```cpp
malloc
```

with:

```cpp
free
```

Do not mix them.

---

# 87. Memory Leak Detection

In real projects, tools can help identify memory problems.

Common tools/concepts:

- AddressSanitizer
- LeakSanitizer
- Valgrind
- compiler warnings
- static analysis

For example, AddressSanitizer can detect issues such as:

- use-after-free
- buffer overflow
- double-free

---

# 88. Exception Safety + RAII

Bad:

```cpp
void process() {
    Resource* r = new Resource();

    doSomething(); // may throw

    delete r;
}
```

If `doSomething()` throws:

```text
delete r
```

may never execute.

RAII:

```cpp
void process() {
    auto r = std::make_unique<Resource>();

    doSomething();
}
```

If an exception occurs, stack unwinding destroys `r`.

This is a major reason RAII is fundamental to modern C++.

---

# 89. Exception Stack Unwinding

Suppose:

```cpp
void foo() {
    A a;
    B b;

    throw runtime_error("error");
}
```

During stack unwinding:

```text
throw
 ↓
destroy b
 ↓
destroy a
 ↓
search for handler
```

Local objects are destroyed automatically.

---

# 90. `std::move`

`std::move` itself generally **does not move an object**.

It casts an expression to an rvalue reference, enabling move operations to be selected.

Example:

```cpp
string a = "hello";

string b = std::move(a);
```

This can allow `b` to acquire resources from `a`.

Afterward, `a` remains valid but its contents should not be assumed unless specified.

---

# 91. Lvalue vs Rvalue

### Lvalue

Generally identifies an object with a persistent identity.

```cpp
int x = 10;
```

`x` is an lvalue.

### Rvalue

Often represents a temporary/value.

```cpp
10
x + 5
```

are examples of expressions that can be rvalues.

---

# 92. Lvalue Reference

```cpp
int x = 10;

int& ref = x;
```

`ref` refers to `x`.

```cpp
ref = 20;

cout << x; // 20
```

---

# 93. Rvalue Reference

```cpp
int&& ref = 10;
```

Rvalue references enable move semantics and are useful in forwarding/perfect-forwarding patterns.

Example:

```cpp
class A {
public:
    A(A&& other) noexcept {
        // steal resources
    }
};
```

---

# 94. Perfect Forwarding

Advanced interview topic.

Templates can preserve whether an argument is an lvalue or rvalue using:

```cpp
std::forward<T>(arg)
```

Example:

```cpp
template <typename T>
void wrapper(T&& arg) {
    process(std::forward<T>(arg));
}
```

This is commonly used in generic library code.

---

# 95. `std::move` vs `std::forward`

### `std::move`

Expresses:

> Treat this object as movable/an rvalue.

### `std::forward`

Expresses:

> Preserve the value category originally passed to the function.

Interview-friendly answer:

> `std::move` is an unconditional cast to an rvalue reference; `std::forward` is used for conditional forwarding, especially with forwarding references.

---

# 96. Copy vs Move

Suppose a class owns a large buffer.

Copy:

```text
A
 ↓
[large data]

B
 ↓
[copy large data]
```

Move:

```text
A
 ↓
[large data]

        transfer ownership

B
 ↓
[large data]

A → empty/valid moved-from state
```

Move can avoid expensive resource duplication.

---

# 97. `noexcept` and Move Constructors

Move constructors are often declared:

```cpp
MyClass(MyClass&& other) noexcept;
```

This can be important because standard containers such as `std::vector` may prefer moving elements during reallocation when the move operation is known not to throw.

---

# 98. Virtual Function Interview Example

```cpp
class Animal {
public:
    virtual void speak() {
        cout << "Animal";
    }

    virtual ~Animal() = default;
};

class Dog : public Animal {
public:
    void speak() override {
        cout << "Dog";
    }
};

class Cat : public Animal {
public:
    void speak() override {
        cout << "Cat";
    }
};
```

Then:

```cpp
vector<unique_ptr<Animal>> animals;

animals.push_back(make_unique<Dog>());
animals.push_back(make_unique<Cat>());

for (auto& a : animals)
    a->speak();
```

Output:

```text
Dog
Cat
```

This is a textbook runtime polymorphism example.

---

# 99. Interface in C++

C++ doesn't have a Java-style `interface` keyword.

A common approach is an abstract class:

```cpp
class PaymentProcessor {
public:
    virtual void pay(double amount) = 0;
    virtual ~PaymentProcessor() = default;
};
```

Implementation:

```cpp
class CreditCardProcessor : public PaymentProcessor {
public:
    void pay(double amount) override {
        cout << "Paid using card\n";
    }
};
```

---

# 100. Dependency Inversion Example

Instead of:

```cpp
class Order {
    CreditCardProcessor processor;
};
```

depend on an abstraction:

```cpp
class Order {
    PaymentProcessor& processor;

public:
    Order(PaymentProcessor& p)
        : processor(p) {}
};
```

Now `Order` can work with:

```text
CreditCardProcessor
UPIProcessor
PayPalProcessor
MockProcessor
```

This is useful for system-design/OOP interviews.

---

# 101. Common Interview Design Question

### "Design a parking lot"

Possible classes:

```text
ParkingLot
 ├── ParkingFloor
 │    ├── ParkingSpot
 │    └── Vehicle
 │
 ├── Ticket
 └── Payment
```

Inheritance might be:

```text
Vehicle
 / | \
Car Bike Truck
```

Polymorphism:

```cpp
class Vehicle {
public:
    virtual double calculateFee() = 0;
    virtual ~Vehicle() = default;
};
```

But don't create inheritance just because classes have similar fields.

Ask:

> Is this genuinely an "is-a" relationship?

---

# 102. SOLID Principles

Very common for experienced C++ interviews.

### S — Single Responsibility Principle

A class should have one reason to change.

### O — Open/Closed Principle

Open for extension, closed for modification.

### L — Liskov Substitution Principle

Derived objects should be usable wherever the base object is expected without breaking correctness.

### I — Interface Segregation Principle

Prefer small, focused interfaces over huge interfaces.

### D — Dependency Inversion Principle

High-level modules should depend on abstractions rather than concrete implementations.

---

# 103. Liskov Substitution Example

Suppose:

```cpp
class Bird {
public:
    virtual void fly() = 0;
};
```

Then:

```cpp
class Penguin : public Bird {
public:
    void fly() override {
        // Penguin cannot fly
    }
};
```

This can indicate poor abstraction.

Better:

```text
Bird
 / \
FlyingBird  Penguin
```

The key question is whether the subtype truly satisfies the behavioral contract of the base type.

---

# 104. Composition Example

Instead of:

```cpp
class Car : public Engine {};
```

which says:

> Car is an Engine

use:

```cpp
class Car {
    Engine engine;
};
```

which says:

> Car has an Engine

This is a classic interview question.

---

# 105. Dependency Injection

Instead of creating dependencies internally:

```cpp
class OrderService {
    PaymentProcessor processor;
};
```

inject them:

```cpp
class OrderService {
    PaymentProcessor& processor;

public:
    OrderService(PaymentProcessor& p)
        : processor(p) {}
};
```

Benefits:

- easier testing
- lower coupling
- easier replacement of implementations

---

# 106. Common Tricky Question: `sizeof` Empty Class

```cpp
class Empty {};
```

Usually:

```cpp
sizeof(Empty)
```

is at least `1`.

Why?

Distinct objects must have distinct addresses, so an empty object still needs a non-zero size.

---

# 107. Common Tricky Question: `sizeof` Polymorphic Class

```cpp
class Base {
public:
    virtual void foo() {}
};
```

Its size is often larger than an equivalent non-polymorphic class because implementations typically need a hidden pointer such as a `vptr`.

But don't claim a specific size portably.

---

# 108. Common Tricky Question: Private Constructor

Can a constructor be private?

**Yes.**

Example:

```cpp
class Singleton {
private:
    Singleton() {}

public:
    static Singleton& getInstance() {
        static Singleton instance;
        return instance;
    }
};
```

Private constructors can control object creation.

---

# 109. Singleton — Interview Note

A classic implementation:

```cpp
class Singleton {
private:
    Singleton() {}

public:
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

    static Singleton& instance() {
        static Singleton obj;
        return obj;
    }
};
```

Modern C++ local static initialization is thread-safe.

But in interviews, also mention:

> Singleton introduces global/shared state and can make testing and dependency management harder, so it should not be used automatically.

---

# 110. `delete` on Base Pointer

Important:

```cpp
Base* p = new Derived();
delete p;
```

Safe polymorphic deletion requires:

```cpp
virtual ~Base() = default;
```

Without it, behavior is undefined if the delete expression does not invoke the correct destructor arrangement for the dynamic object.

---

# 111. Common Memory Bugs

Know these terms:

| Bug                  | Meaning                              |
| -------------------- | ------------------------------------ |
| Memory leak          | Allocated memory never released      |
| Dangling pointer     | Pointer refers to invalid object     |
| Double free          | Resource released more than once     |
| Use-after-free       | Access after resource release        |
| Buffer overflow      | Access outside allocated buffer      |
| Uninitialized memory | Reading before initialization        |
| Invalid free         | Releasing memory incorrectly         |
| Ownership bug        | Unclear/incorrect resource ownership |

---

# 112. Interview Coding Pattern: Linked List

A raw-pointer implementation:

```cpp
struct Node {
    int data;
    Node* next;

    Node(int value)
        : data(value), next(nullptr) {}
};
```

Insertion:

```cpp
Node* head = new Node(10);
head->next = new Node(20);
```

Cleanup:

```cpp
while (head) {
    Node* temp = head;
    head = head->next;
    delete temp;
}
```

In production C++, you might use ownership-aware designs instead of raw owning pointers.

---

# 113. Interview Coding Pattern: Polymorphic Factory

```cpp
class Animal {
public:
    virtual void sound() = 0;
    virtual ~Animal() = default;
};

class Dog : public Animal {
public:
    void sound() override {
        cout << "Bark\n";
    }
};

class Cat : public Animal {
public:
    void sound() override {
        cout << "Meow\n";
    }
};
```

Factory:

```cpp
unique_ptr<Animal> createAnimal(string type) {
    if (type == "dog")
        return make_unique<Dog>();

    if (type == "cat")
        return make_unique<Cat>();

    return nullptr;
}
```

Usage:

```cpp
auto animal = createAnimal("dog");

if (animal)
    animal->sound();
```

This demonstrates:

- abstraction
- polymorphism
- factory pattern
- smart pointers
- RAII

---

# 114. Most Important Interview Differences

### Class vs Object

```text
Class  = blueprint
Object = instance
```

### Overloading vs Overriding

```text
Overloading  = same name, different parameters
Overriding   = derived implementation of virtual base function
```

### Compile-time vs Runtime polymorphism

```text
Compile-time → overloading/templates
Runtime      → virtual functions
```

### Stack vs Heap

```text
Stack → automatic lifetime
Heap  → dynamic storage
```

### `unique_ptr` vs `shared_ptr`

```text
unique_ptr → exclusive ownership
shared_ptr → shared ownership
```

### `shared_ptr` vs `weak_ptr`

```text
shared_ptr → owns
weak_ptr   → observes without ownership
```

### Shallow vs Deep copy

```text
Shallow → copies resource handle/pointer
Deep    → duplicates owned resource
```

### Copy vs Move

```text
Copy → duplicate resource
Move → transfer resource
```

---

# 115. Top 30 Interview Questions

Make sure you can answer these without looking at notes:

1. What are the four pillars of OOP?
2. What is encapsulation?
3. What is abstraction?
4. Encapsulation vs abstraction?
5. What is inheritance?
6. What are different types of inheritance?
7. What is polymorphism?
8. Compile-time vs runtime polymorphism?
9. Overloading vs overriding?
10. Why do we need virtual functions?
11. What is a pure virtual function?
12. What is an abstract class?
13. Why should a base destructor be virtual?
14. What is a vtable?
15. What is a vptr?
16. What is object slicing?
17. What is the diamond problem?
18. How does virtual inheritance solve it?
19. What is a copy constructor?
20. Copy constructor vs copy assignment?
21. What is shallow copy?
22. What is deep copy?
23. Explain Rule of 3.
24. Explain Rule of 5.
25. What is Rule of 0?
26. What is RAII?
27. `unique_ptr` vs `shared_ptr`?
28. Why do we need `weak_ptr`?
29. What is a dangling pointer?
30. What is a memory leak?

---

# 116. Rapid-Fire Interview Answers

**Q: Why virtual destructor?**

> To ensure the derived destructor is called when deleting a derived object through a base pointer.

**Q: Why use `override`?**

> It lets the compiler verify that a derived function actually overrides a base virtual function.

**Q: Why use smart pointers?**

> To express ownership and automatically manage resource lifetime using RAII.

**Q: Why `unique_ptr` instead of `shared_ptr`?**

> Prefer `unique_ptr` when ownership is exclusive because it has simpler ownership semantics and avoids unnecessary shared-ownership machinery.

**Q: Why `weak_ptr`?**

> To observe a `shared_ptr`-managed object without increasing its strong reference count, especially to break ownership cycles.

**Q: What is RAII?**

> Resource lifetime is tied to object lifetime; acquisition happens during initialization and cleanup happens automatically in the destructor.

**Q: What is object slicing?**

> Copying a derived object into a base object by value removes the derived-specific portion.

**Q: What is a pure virtual function?**

> A virtual function declared with `= 0`; a class containing one is abstract.

**Q: Can constructors be virtual?**

> No.

**Q: Can destructors be virtual?**

> Yes, and polymorphic base classes generally need a virtual destructor when they may be deleted through a base pointer.

**Q: What is Rule of 5?**

> If a class needs custom resource-management semantics, consider destructor, copy constructor, copy assignment, move constructor, and move assignment.

---

# 117. One Complete Example

This combines most of the important concepts:

```cpp
#include <iostream>
#include <memory>
#include <vector>

using namespace std;

class Employee {
protected:
    string name;

public:
    Employee(string name)
        : name(std::move(name)) {}

    virtual void work() const = 0;

    virtual ~Employee() = default;
};

class Developer : public Employee {
public:
    Developer(string name)
        : Employee(std::move(name)) {}

    void work() const override {
        cout << name << " writes code\n";
    }
};

class Manager : public Employee {
public:
    Manager(string name)
        : Employee(std::move(name)) {}

    void work() const override {
        cout << name << " manages the team\n";
    }
};

int main() {

    vector<unique_ptr<Employee>> employees;

    employees.push_back(
        make_unique<Developer>("Alice")
    );

    employees.push_back(
        make_unique<Manager>("Bob")
    );

    for (const auto& employee : employees) {
        employee->work();
    }

    return 0;
}
```

Output:

```text
Alice writes code
Bob manages the team
```

### Concepts demonstrated

```text
Employee
   ↓
abstract class
   ↓
pure virtual function
   ↓
Developer / Manager
   ↓
inheritance
   ↓
overriding
   ↓
runtime polymorphism
   ↓
unique_ptr
   ↓
RAII
   ↓
virtual destructor
```

---

# 118. What to Prioritize for Interviews

If you're short on time, study in this order:

### Tier 1 — Must Know

```text
Classes & objects
Encapsulation
Inheritance
Polymorphism
Virtual functions
Pure virtual functions
Abstract classes
Constructors/destructors
Virtual destructor
Pointers/references
Stack vs heap
new/delete
Memory leaks
Dangling pointers
unique_ptr
shared_ptr
RAII
```

### Tier 2 — Very Important

```text
Copy constructor
Copy assignment
Deep vs shallow copy
Move constructor
Move assignment
Rule of 3
Rule of 5
Rule of 0
Object slicing
Composition vs inheritance
static
const
override
```

### Tier 3 — Advanced

```text
weak_ptr
Diamond problem
Virtual inheritance
vtable/vptr
Perfect forwarding
std::move/std::forward
Exception safety
SOLID
Dependency injection
Design patterns
```

---

# 119. The Mental Model to Remember

For almost every C++ OOP/memory question, think in these five layers:

```text
             C++ OBJECT
                 │
       ┌─────────┴─────────┐
       │                   │
    BEHAVIOR             DATA
       │                   │
 virtual functions     members
       │                   │
 polymorphism          encapsulation
       │
 inheritance
       │
       ▼
   OBJECT LIFETIME
       │
       ├── constructor
       ├── RAII
       ├── copy
       ├── move
       └── destructor
               │
               ▼
          OWNERSHIP
               │
       ┌───────┼────────┐
       │       │        │
   unique_ptr shared_ptr weak_ptr
       │       │        │
    unique    shared   observer
```

If you understand **object lifetime + ownership + polymorphism**, a large percentage of C++ interview questions become much easier.

### Best interview habit

When you see a pointer in C++, immediately ask:

> **Who owns this object, who destroys it, and what happens if an exception occurs?**

That single question leads naturally to **RAII, smart pointers, destructors, move semantics, and exception safety**—the core of modern C++ memory management.
