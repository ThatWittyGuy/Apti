# Python — Complete Study Notes (Basic → Advanced)
### For Corning OA Prep + Interviews | All JDs
> Covers: Syntax, data types, control flow, functions, OOP, modules, file I/O, exceptions, iterators, generators, decorators, comprehensions, dunder methods, standard library, NumPy, Pandas, algorithms, concurrency.
> Format: Explanation + Syntax + Code Example + Output

---

## 1. Python Fundamentals

### 1.1 Overview
- **Interpreted:** Code runs line-by-line; no compilation step
- **Dynamically typed:** Variable types determined at runtime, not declared
- **Strongly typed:** No implicit type coercion — `"5" + 5` → TypeError
- **Garbage collected:** Memory managed automatically (reference counting + cyclic GC)
- **Indentation-based:** Blocks defined by 4-space indentation (PEP 8), not braces
- **Multi-paradigm:** Procedural, object-oriented, and functional programming

### 1.2 Variables and Assignment
```python
x = 10              # int
name = "Alice"      # str
pi = 3.14           # float
is_active = True    # bool
nothing = None      # NoneType

# Multiple assignment
a, b, c = 1, 2, 3
a = b = c = 0       # all point to same object

# Augmented assignment
x += 5   # x = x + 5
x -= 2
x *= 3
x /= 2   # always returns float
x //= 2  # floor division
x **= 2  # exponentiation
x %= 3   # modulo
```

### 1.3 Data Types
```python
type(42)        # <class 'int'>
type(3.14)      # <class 'float'>
type("hello")   # <class 'str'>
type(True)      # <class 'bool'>
type(None)      # <class 'NoneType'>
type([1,2,3])   # <class 'list'>
type((1,2,3))   # <class 'tuple'>
type({1,2,3})   # <class 'set'>
type({"a":1})   # <class 'dict'>

isinstance(42, int)            # True
isinstance(42, (int, float))   # True — checks multiple types
```

### 1.4 Numbers
```python
# Integer operations
10 / 3    # 3.3333... (true division — always float)
10 // 3   # 3         (floor division — rounds toward -infinity)
10 % 3    # 1         (modulo)
2 ** 10   # 1024      (exponentiation)
abs(-5)   # 5

# Python ints are arbitrary precision — no overflow
2 ** 1000   # works!

# Float precision
0.1 + 0.2   # 0.30000000000000004
round(0.1 + 0.2, 2)   # 0.3

# Conversions
int(3.9)      # 3   (truncates, not rounds)
int("42")     # 42
float("3.14") # 3.14
float(5)      # 5.0

import math
math.sqrt(16)       # 4.0
math.ceil(3.2)      # 4
math.floor(3.9)     # 3
math.pi             # 3.141592653589793
math.factorial(5)   # 120
math.log(100, 10)   # 2.0
math.log(math.e)    # 1.0
```

### 1.5 Strings
```python
s = "Hello, World!"
len(s)              # 13
s.upper()           # "HELLO, WORLD!"
s.lower()           # "hello, world!"
s.title()           # "Hello, World!"
s.strip()           # remove leading/trailing whitespace
s.lstrip()          # remove leading only
s.rstrip()          # remove trailing only
s.replace("World", "Python")    # "Hello, Python!"
s.split(", ")       # ["Hello", "World!"]
", ".join(["a","b","c"])        # "a, b, c"
s.startswith("Hello")   # True
s.endswith("!")     # True
s.find("World")     # 7  (returns -1 if not found)
s.index("World")    # 7  (raises ValueError if not found)
s.count("l")        # 3
"hello".capitalize()  # "Hello"
s.center(20, "-")   # "---Hello, World!---"
s.zfill(20)         # zero-pad

# Indexing and slicing
s = "Python"
s[0]     # 'P'
s[-1]    # 'n'
s[1:4]   # 'yth'
s[:3]    # 'Pyt'
s[3:]    # 'hon'
s[::2]   # 'Pto'
s[::-1]  # 'nohtyP'  (reverse)

# f-strings (Python 3.6+) — preferred
name, age = "Alice", 30
f"Name: {name}, Age: {age}"     # "Name: Alice, Age: 30"
f"Pi is {3.14159:.2f}"          # "Pi is 3.14"
f"{'hello':>10}"                # "     hello" (right-align)
f"{'hello':<10}"                # "hello     " (left-align)
f"{'hello':^10}"                # "  hello   " (center)
f"{1000000:,}"                  # "1,000,000"
f"{0.1234:.2%}"                 # "12.34%"

# Other formatting
"{} is {}".format(name, age)
"%s is %d" % (name, age)        # legacy

# Escape sequences
"\t" # tab      "\n" # newline     "\\" # backslash     "\"" # quote
r"C:\new\path"  # raw string — backslashes literal

# Strings are IMMUTABLE
s = "hello"
# s[0] = "H"  # TypeError!
s = "H" + s[1:]  # create new string
```

### 1.6 Type Conversion
```python
int("42")    # 42         int(3.9)    # 3 (truncate)
float("3.14")  # 3.14    str(42)     # "42"
bool(0)      # False      bool("")    # False
bool([])     # False      bool(None)  # False
bool(1)      # True       bool("hi")  # True
bool([1])    # True

# Falsy values: 0, 0.0, "", [], {}, (), set(), None, False
# Everything else is truthy

list("hello")    # ['h', 'e', 'l', 'l', 'o']
tuple([1,2,3])   # (1, 2, 3)
set([1,1,2,3])   # {1, 2, 3}

ord("A")   # 65  (char → ASCII)
chr(65)    # "A" (ASCII → char)
bin(10)    # "0b1010"
hex(255)   # "0xff"
```

---

## 2. Control Flow

### 2.1 Conditionals
```python
x = 15
if x > 20:
    print("Greater than 20")
elif x > 10:
    print("Greater than 10")    # prints
else:
    print("10 or less")

# Ternary expression
status = "adult" if x >= 18 else "minor"   # "minor"

# Chained comparisons (Pythonic)
if 10 < x < 20:    # True — equivalent to x>10 and x<20
    print("between")

# Truthy/falsy checks
if not name:       # empty string is falsy
    print("empty")
if data:           # non-empty list is truthy
    print("has data")
```

### 2.2 For Loop
```python
for i in range(5):
    print(i, end=" ")   # 0 1 2 3 4

for i in range(2, 10, 2):   # start, stop, step
    print(i, end=" ")   # 2 4 6 8

# Iterate with index
fruits = ["apple", "banana", "cherry"]
for i, fruit in enumerate(fruits):
    print(f"{i}: {fruit}")
# 0: apple  1: banana  2: cherry

for i, fruit in enumerate(fruits, start=1):
    print(f"{i}: {fruit}")   # starts at 1

# Iterate dictionary
d = {"a":1, "b":2, "c":3}
for key in d:              # keys only
for key, val in d.items(): # key-value pairs
for val in d.values():     # values only

# zip — parallel iteration
names = ["Alice", "Bob"]
scores = [85, 92]
for name, score in zip(names, scores):
    print(f"{name}: {score}")

list(zip([1,2,3], ['a','b']))   # [(1,'a'), (2,'b')]  — stops at shortest
```

### 2.3 While Loop
```python
count = 0
while count < 5:
    print(count, end=" ")   # 0 1 2 3 4
    count += 1

# while-else: else runs if loop ended normally (no break)
n = 5
while n > 0:
    n -= 1
else:
    print("Done!")   # prints
```

### 2.4 Loop Control
```python
# break — exit loop immediately
for i in range(10):
    if i == 5: break
    print(i, end=" ")   # 0 1 2 3 4

# continue — skip rest of iteration
for i in range(10):
    if i % 2 == 0: continue
    print(i, end=" ")   # 1 3 5 7 9

# pass — placeholder (do nothing)
for i in range(5): pass
class Empty: pass
def todo(): pass

# for-else: else runs only if loop completed without break
for i in range(5):
    if i == 10: break
else:
    print("No break occurred")   # prints
```

---

## 3. Data Structures

### 3.1 Lists
```python
lst = [1, 2, 3, 4, 5]
lst2 = list(range(5))    # [0, 1, 2, 3, 4]
lst3 = [0] * 5           # [0, 0, 0, 0, 0]

# Access and slice
lst[0]     # 1    lst[-1]    # 5
lst[1:3]   # [2, 3]
lst[::-1]  # [5, 4, 3, 2, 1]

# Modify
lst[0] = 10
lst.append(6)          # add to end
lst.insert(2, 99)      # insert at index 2
lst.extend([7, 8])     # add multiple elements
lst += [7, 8]          # same as extend
lst.remove(99)         # remove first occurrence of value
lst.pop()              # remove and return last element
lst.pop(0)             # remove and return at index 0
del lst[0]             # delete (no return)
del lst[1:3]           # delete slice

# Search
lst.index(5)           # index of first occurrence
lst.count(1)           # count occurrences
1 in lst               # True

# Sort
lst.sort()                          # in-place ascending
lst.sort(reverse=True)              # in-place descending
sorted(lst)                         # new sorted list
sorted(lst, reverse=True)           # descending
sorted(words, key=len)              # by string length
data = [("Alice",85),("Bob",92)]
data.sort(key=lambda x: x[1])      # sort by score

# Other
lst.reverse()          # reverse in-place
lst.clear()            # empty list
lst.copy()             # shallow copy
len(lst); min(lst); max(lst); sum(lst)

# Nested
matrix = [[1,2,3],[4,5,6],[7,8,9]]
matrix[1][2]   # 6
```

### 3.2 Tuples
```python
t = (1, 2, 3)
t2 = 1, 2, 3           # parens optional
t3 = (42,)             # single element — comma required!
t4 = tuple([1,2,3])

# Access (same as list, no modification)
t[0]    # 1    t[-1]   # 3    t[1:3]  # (2, 3)

# Operations
len(t)          # 3
t.count(2)      # 1
t.index(2)      # 1
2 in t          # True
t + (4,5)       # (1,2,3,4,5)
t * 2           # (1,2,3,1,2,3)

# Unpacking
x, y, z = (1, 2, 3)
first, *rest = (1,2,3,4,5)         # first=1, rest=[2,3,4,5]
*start, last = (1,2,3,4,5)         # start=[1,2,3,4], last=5
a, *mid, b = (1,2,3,4,5)           # a=1, mid=[2,3,4], b=5

# Named tuple
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(3, 4)
p.x    # 3    p.y    # 4    p[0]   # 3 (still indexable)

# Why tuples: immutable (signal intent), hashable (dict keys, set members),
# slightly faster than lists, used for multiple return values
```

### 3.3 Dictionaries
```python
d = {"name": "Alice", "age": 30}
d2 = dict(name="Alice", age=30)
d3 = dict([("a",1), ("b",2)])
d4 = {}   # empty

# Access
d["name"]              # "Alice"
d.get("name")          # "Alice"
d.get("salary")        # None (no KeyError)
d.get("salary", 0)     # 0 (default)

# Modify
d["age"] = 31                    # update
d["email"] = "alice@ex.com"      # add new
del d["email"]                   # delete
d.pop("email")                   # remove and return
d.pop("email", None)             # remove if exists, else return None
d.update({"age":32,"city":"LA"}) # update multiple

# Check
"name" in d      # True
"salary" in d    # False

# Iterate
for key in d:
for key in d.keys():
for val in d.values():
for key, val in d.items():

# Methods
d.keys(); d.values(); d.items()
d.copy(); d.clear()
len(d)

# Merge (Python 3.9+)
d1 = {"a":1, "b":2}
d2 = {"b":3, "c":4}
d1 | d2          # {"a":1,"b":3,"c":4} (d2 wins)
d1 |= d2         # in-place
{**d1, **d2}     # dict unpacking (Python 3.5+)

# defaultdict
from collections import defaultdict
dd = defaultdict(int)     # default 0
dd["a"] += 1; dd["b"] += 5
print(dd)   # defaultdict(int, {'a':1,'b':5})

# Counter
from collections import Counter
words = ["apple","banana","apple","cherry","apple"]
freq = Counter(words)
freq.most_common(2)   # [('apple',3),('banana',1)]
freq["apple"]         # 3

# Nested dict
config = {"db": {"host":"localhost","port":5432}}
config["db"]["host"]   # "localhost"
```

### 3.4 Sets
```python
s = {1, 2, 3, 4}
s2 = set([1,2,2,3])   # {1,2,3} — duplicates removed
s3 = set("hello")     # {'h','e','l','o'}
empty = set()         # NOT {} (that's empty dict!)

# Modify
s.add(5)
s.update([6,7])
s.remove(5)           # KeyError if not found
s.discard(99)         # no error if not found
s.pop()               # remove arbitrary element
s.clear()

# Set operations
a = {1,2,3,4}; b = {3,4,5,6}
a | b            # {1,2,3,4,5,6}  union
a & b            # {3,4}          intersection
a - b            # {1,2}          difference (in a not b)
a ^ b            # {1,2,5,6}      symmetric difference

# Comparisons
{1,2} <= {1,2,3}     # True (subset)
{1,2} < {1,2,3}      # True (proper subset)
{1,2,3}.issuperset({1,2})   # True
a.isdisjoint(b)              # True if no common elements

5 in s          # O(1) — much faster than list!
10 in s         # False

# Frozenset — immutable, hashable
fs = frozenset([1,2,3])
# fs.add(4)  # AttributeError
```

### 3.5 Comprehensions

#### List Comprehension
```python
# [expression for item in iterable if condition]
squares = [x**2 for x in range(10)]
# [0,1,4,9,16,25,36,49,64,81]

evens = [x for x in range(20) if x % 2 == 0]

# Conditional expression in output
result = ["even" if x%2==0 else "odd" for x in range(5)]
# ['even','odd','even','odd','even']

# Nested comprehension
matrix = [[i*j for j in range(1,4)] for i in range(1,4)]
# [[1,2,3],[2,4,6],[3,6,9]]

# Flatten nested list
nested = [[1,2],[3,4],[5,6]]
flat = [x for row in nested for x in row]   # [1,2,3,4,5,6]
```

#### Dict and Set Comprehensions
```python
# Dict comprehension
squares = {x: x**2 for x in range(6)}
# {0:0,1:1,2:4,3:9,4:16,5:25}

inverted = {v:k for k,v in {"a":1,"b":2}.items()}
# {1:'a',2:'b'}

passing = {k:v for k,v in scores.items() if v >= 80}

# Set comprehension
unique_lengths = {len(w) for w in ["cat","dog","elephant"]}
# {3, 8}
```

#### Generator Expression
```python
# (expression for item in iterable) — lazy, O(1) memory
gen = (x**2 for x in range(1000000))
next(gen)    # 0    next(gen)    # 1

# Use in functions — efficient
total = sum(x**2 for x in range(1000))
any(x > 5 for x in [1,2,3,6])    # True
all(x > 0 for x in [1,2,3])      # True

import sys
lst = [x**2 for x in range(1000)]
gen = (x**2 for x in range(1000))
sys.getsizeof(lst)   # ~8856 bytes
sys.getsizeof(gen)   # ~112 bytes
```

---

## 4. Functions

### 4.1 Basic Functions
```python
def greet(name):
    """Docstring: describe the function."""
    return f"Hello, {name}!"

greet("Alice")   # "Hello, Alice!"

# No explicit return → returns None
def print_hi(name):
    print(f"Hi, {name}!")

x = print_hi("Bob")   # prints "Hi, Bob!"
print(x)              # None
```

### 4.2 Arguments
```python
def add(a, b, c):
    return a + b + c

add(1, 2, 3)           # positional
add(b=2, a=1, c=3)     # keyword
add(1, c=3, b=2)       # mix (positional first)

# Default parameters
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

greet("Alice")                   # "Hello, Alice!"
greet("Bob", "Hi")               # "Hi, Bob!"

# WARNING: mutable default argument bug
def bad(item, lst=[]):     # BAD — lst shared across calls
    lst.append(item)
    return lst

bad(1)   # [1]
bad(2)   # [1, 2]  ← BUG!

def good(item, lst=None):  # CORRECT
    if lst is None: lst = []
    lst.append(item)
    return lst
```

### 4.3 *args and **kwargs
```python
# *args — variable positional args → tuple
def sum_all(*args):
    print(type(args))   # <class 'tuple'>
    return sum(args)

sum_all(1, 2, 3, 4)   # 10

# **kwargs — variable keyword args → dict
def print_info(**kwargs):
    print(type(kwargs))  # <class 'dict'>
    for k, v in kwargs.items():
        print(f"{k}: {v}")

print_info(name="Alice", age=30)
# name: Alice
# age: 30

# Combined
def func(pos1, pos2, *args, kwonly, **kwargs):
    print(f"pos: {pos1},{pos2}")
    print(f"args: {args}")
    print(f"kwonly: {kwonly}")
    print(f"kwargs: {kwargs}")

func(1, 2, 3, 4, kwonly="kw", extra="val")
# pos: 1,2   args: (3,4)   kwonly: kw   kwargs: {'extra':'val'}

# Keyword-only (after bare *)
def func(a, b, *, c, d=10):   # c must be keyword
    return a + b + c + d

func(1, 2, c=3)        # 16
func(1, 2, c=3, d=5)   # 11
# func(1, 2, 3)  # TypeError!

# Positional-only (Python 3.8+, before /)
def func(a, b, /, c, d):   # a,b must be positional
    return a + b + c + d

func(1, 2, 3, 4)          # works
func(1, 2, c=3, d=4)      # works
# func(a=1, b=2, c=3, d=4)  # TypeError!

# Unpacking in calls
args = [1, 2, 3]
add(*args)               # unpack as positional

kwargs = {"a":1,"b":2,"c":3}
add(**kwargs)            # unpack as keyword
```

### 4.4 Lambda Functions
```python
# lambda arguments: expression  (single expression, implicit return)
double = lambda x: x * 2
double(5)   # 10

add = lambda x, y: x + y
add(3, 4)   # 7

# Most useful as arguments
nums = [5,2,8,1,9]
sorted(nums, key=lambda x: -x)        # [9,8,5,2,1]

data = [("Alice",85),("Bob",92),("Charlie",78)]
sorted(data, key=lambda x: x[1])      # sort by score

# Alternative — operator module
import operator
sorted(data, key=operator.itemgetter(1))   # same effect
```

### 4.5 Higher-Order Functions
```python
# map — apply function to each element (lazy)
nums = [1,2,3,4,5]
list(map(lambda x: x**2, nums))     # [1,4,9,16,25]
list(map(int, ["1","2","3"]))       # [1,2,3]
list(map(lambda x,y: x+y, [1,2,3],[4,5,6]))   # [5,7,9]

# filter — keep elements where func returns True (lazy)
list(filter(lambda x: x%2==0, nums))   # [2,4]
list(filter(None, [0,1,"",2,None,3]))  # [1,2,3] — removes falsy

# reduce — fold to single value
from functools import reduce
reduce(lambda x,y: x*y, [1,2,3,4,5])  # 120

# Note: prefer comprehensions over map/filter in most cases
squares = [x**2 for x in nums]           # clearer than map
evens = [x for x in nums if x%2==0]     # clearer than filter
```

### 4.6 Scope — LEGB Rule
```python
# LEGB: Local → Enclosing → Global → Built-in
x = "global"

def outer():
    x = "enclosing"
    def inner():
        x = "local"
        print(x)   # "local"
    inner()
    print(x)       # "enclosing"

outer()
print(x)           # "global"

# global keyword
count = 0
def increment():
    global count
    count += 1

increment(); increment()
print(count)   # 2

# nonlocal keyword
def counter():
    count = 0
    def inc():
        nonlocal count
        count += 1
        return count
    return inc

c = counter()
c()   # 1    c()   # 2    c()   # 3
```

### 4.7 Closures
```python
# Inner function captures variables from enclosing scope
def make_multiplier(n):
    def multiplier(x):
        return x * n   # 'n' captured from enclosing scope
    return multiplier

double = make_multiplier(2)
triple = make_multiplier(3)
double(5)   # 10    triple(5)   # 15

double.__closure__[0].cell_contents   # 2

# Common bug: closure in loop
funcs = [lambda: i for i in range(5)]
funcs[0]()   # 4 — all capture same i (=4 when loop ends)

# Fix: capture current value
funcs = [lambda i=i: i for i in range(5)]
funcs[0]()   # 0    funcs[4]()   # 4
```

### 4.8 Recursion
```python
def factorial(n):
    if n <= 1: return 1          # base case
    return n * factorial(n - 1)  # recursive case

factorial(5)   # 120

# Fibonacci (slow without memoization)
def fib(n):
    if n <= 1: return n
    return fib(n-1) + fib(n-2)

# With lru_cache (memoization)
from functools import lru_cache

@lru_cache(maxsize=None)
def fib_fast(n):
    if n <= 1: return n
    return fib_fast(n-1) + fib_fast(n-2)

fib_fast(100)   # instant!

import sys
sys.getrecursionlimit()      # 1000 (default)
sys.setrecursionlimit(5000)  # increase if needed
```

---

## 5. Iterators and Generators

### 5.1 Iterators
```python
# ITERABLE: anything you can iterate over (list, str, dict, etc.)
# ITERATOR: object with __iter__() and __next__()
# Every iterator is iterable; not every iterable is an iterator

lst = [1, 2, 3]
it = iter(lst)      # create iterator

next(it)    # 1
next(it)    # 2
next(it)    # 3
next(it)    # StopIteration!

next(it, None)   # return None instead of raising StopIteration

# Custom iterator class
class Countdown:
    def __init__(self, start):
        self.current = start
    
    def __iter__(self):     # makes it an iterable
        return self         # iterator is itself
    
    def __next__(self):     # defines next value
        if self.current <= 0:
            raise StopIteration
        value = self.current
        self.current -= 1
        return value

for n in Countdown(5):
    print(n, end=" ")   # 5 4 3 2 1

# itertools
from itertools import chain, cycle, repeat, islice, accumulate, combinations, permutations

list(chain([1,2],[3,4],[5,6]))        # [1,2,3,4,5,6]
list(islice(cycle([1,2,3]), 7))       # [1,2,3,1,2,3,1]
list(repeat("x", 3))                  # ['x','x','x']
list(accumulate([1,2,3,4,5]))         # [1,3,6,10,15] running sum
list(combinations([1,2,3], 2))        # [(1,2),(1,3),(2,3)]
list(permutations([1,2,3], 2))        # [(1,2),(1,3),(2,1),(2,3),(3,1),(3,2)]

from itertools import zip_longest, product, starmap
list(zip_longest([1,2,3],['a','b'], fillvalue=None))
# [(1,'a'),(2,'b'),(3,None)]
```

### 5.2 Generators
```python
# Generator function — uses yield to produce values lazily
def countdown(n):
    while n > 0:
        yield n          # suspend, return n
        n -= 1           # resume here on next call

gen = countdown(3)
type(gen)        # <class 'generator'>
next(gen)   # 3    next(gen)   # 2    next(gen)   # 1
next(gen)   # StopIteration

for n in countdown(5):
    print(n, end=" ")   # 5 4 3 2 1

# Infinite generator
def infinite_squares():
    n = 0
    while True:
        yield n * n
        n += 1

gen = infinite_squares()
[next(gen) for _ in range(6)]   # [0,1,4,9,16,25]

# Fibonacci generator
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

fib = fibonacci()
[next(fib) for _ in range(10)]
# [0,1,1,2,3,5,8,13,21,34]

# yield from — delegate to another iterable
def chain_gen(*iterables):
    for it in iterables:
        yield from it

list(chain_gen([1,2],[3,4]))   # [1,2,3,4]

# Generator vs List memory
import sys
lst = [x**2 for x in range(1000)]
gen = (x**2 for x in range(1000))
sys.getsizeof(lst)   # ~8856 bytes
sys.getsizeof(gen)   # ~112 bytes

# send() — send value INTO generator
def running_average():
    total = count = 0
    while True:
        value = yield total/count if count else 0
        total += value
        count += 1

avg = running_average()
next(avg)       # 0 — prime the generator
avg.send(10)    # 10.0
avg.send(20)    # 15.0
avg.send(30)    # 20.0
```

---

## 6. Decorators

### 6.1 Building Up to Decorators
```python
# Functions are objects — can be passed and returned
def greet(): return "Hello!"
func = greet    # assign to variable (no call)
func()          # "Hello!"

# A decorator wraps a function to add behavior
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before")
        result = func(*args, **kwargs)
        print("After")
        return result
    return wrapper

# @syntax — syntactic sugar
@my_decorator          # say_hi = my_decorator(say_hi)
def say_hi(name):
    return f"Hi, {name}!"

say_hi("Alice")
# Before
# After
# (returns "Hi, Alice!")
```

### 6.2 functools.wraps
```python
import functools

# Problem: without wraps, decorated function loses identity
def bad_decorator(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@bad_decorator
def my_func():
    """My docstring."""
    pass

my_func.__name__   # 'wrapper'  ← wrong!
my_func.__doc__    # None       ← lost!

# Solution: functools.wraps
def good_decorator(func):
    @functools.wraps(func)   # preserves __name__, __doc__, etc.
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper

@good_decorator
def my_func():
    """My docstring."""
    pass

my_func.__name__   # 'my_func' ✓
my_func.__doc__    # 'My docstring.' ✓
```

### 6.3 Practical Decorator Examples
```python
import functools, time

# Timer
def timer(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        print(f"{func.__name__} took {elapsed:.4f}s")
        return result
    return wrapper

@timer
def slow():
    time.sleep(1)
    return "done"

slow()   # slow took 1.0012s

# Memoization
def memoize(func):
    cache = {}
    @functools.wraps(func)
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
        return cache[args]
    return wrapper

# Built-in: lru_cache
from functools import lru_cache

@lru_cache(maxsize=128)
def fib(n):
    if n <= 1: return n
    return fib(n-1) + fib(n-2)

fib.cache_info()   # CacheInfo(hits=..., misses=..., maxsize=128, currsize=...)
fib.cache_clear()

@lru_cache(maxsize=None)  # unlimited (Python 3.9+: @cache)
def expensive(n): ...

# Retry decorator
def retry(max_attempts=3):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_attempts):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if attempt == max_attempts - 1: raise
                    print(f"Attempt {attempt+1} failed: {e}")
        return wrapper
    return decorator

@retry(max_attempts=3)
def flaky_function():
    import random
    if random.random() < 0.7: raise ConnectionError("fail")
    return "success"

# Logging decorator
def log_calls(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}{args}{kwargs}")
        result = func(*args, **kwargs)
        print(f"Returned: {result!r}")
        return result
    return wrapper
```

### 6.4 Decorator with Arguments
```python
import functools

# Extra level of nesting for parameterized decorators
def repeat(n):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(n):
                result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def say_hi():
    print("Hi!")

say_hi()   # prints "Hi!" three times

# Stacking decorators — applied bottom-up
@timer      # applied second (outermost)
@log_calls  # applied first (innermost)
def add(a, b):
    return a + b

# Equivalent: add = timer(log_calls(add))
# Execution: timer_before → log_before → add → log_after → timer_after

# Class-based decorator
import functools

class Retry:
    def __init__(self, max_attempts=3):
        self.max_attempts = max_attempts
    
    def __call__(self, func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for i in range(self.max_attempts):
                try:
                    return func(*args, **kwargs)
                except Exception:
                    if i == self.max_attempts - 1: raise
        return wrapper

@Retry(max_attempts=5)
def flaky(): ...
```

---

## 7. Object-Oriented Programming (OOP)

### 7.1 Classes and Objects
```python
class Dog:
    species = "Canis lupus familiaris"   # class attribute (shared)
    
    def __init__(self, name, age, breed):
        # instance attributes (unique per object)
        self.name = name
        self.age = age
        self.breed = breed
    
    def bark(self):
        return f"{self.name} says: Woof!"
    
    def __str__(self):    # print(dog) → "Dog(Rex, 3)"
        return f"Dog({self.name}, {self.age})"
    
    def __repr__(self):   # repr(dog) → developer string
        return f"Dog(name={self.name!r}, age={self.age!r})"

dog1 = Dog("Rex", 3, "Labrador")
dog2 = Dog("Bella", 5, "Poodle")

dog1.name         # "Rex"
dog1.bark()       # "Rex says: Woof!"
print(dog1)       # "Dog(Rex, 3)"
repr(dog1)        # "Dog(name='Rex', age=3)"

# Class vs instance attribute
Dog.species        # "Canis lupus familiaris"
dog1.species       # "Canis lupus familiaris" (found via class)
dog1.species = "override"  # creates INSTANCE attribute
dog1.species    # "override"
dog2.species    # "Canis lupus familiaris" (unchanged)
```

### 7.2 Class Methods and Static Methods
```python
class Employee:
    company = "Corning"
    employee_count = 0
    
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary
        Employee.employee_count += 1
    
    def get_info(self):                  # instance method
        return f"{self.name} at {Employee.company}"
    
    @classmethod
    def get_count(cls):                  # class method — receives cls
        return f"{cls.company}: {cls.employee_count}"
    
    @classmethod
    def from_string(cls, s):             # alternative constructor
        name, salary = s.split("-")
        return cls(name, int(salary))
    
    @staticmethod
    def is_valid_salary(salary):         # static — no self or cls
        return salary > 0

e1 = Employee("Alice", 80000)
e2 = Employee.from_string("Bob-90000")
Employee.get_count()           # "Corning: 2"
Employee.is_valid_salary(5000) # True
```

### 7.3 Inheritance
```python
class Animal:
    def __init__(self, name, sound):
        self.name = name
        self.sound = sound
    
    def speak(self):
        return f"{self.name} says {self.sound}!"

class Dog(Animal):
    def __init__(self, name, breed):
        super().__init__(name, "Woof")   # call parent __init__
        self.breed = breed
    
    def speak(self):                      # override
        return f"{self.name} barks: Woof woof!"
    
    def fetch(self):                      # new method
        return f"{self.name} fetches!"

class Cat(Animal):
    def speak(self):
        return f"{self.name} says Meow~"

dog = Dog("Rex", "Lab")
dog.speak()         # "Rex barks: Woof woof!"
dog.fetch()         # "Rex fetches!"

isinstance(dog, Dog)      # True
isinstance(dog, Animal)   # True
issubclass(Dog, Animal)   # True

Dog.__mro__    # (Dog, Animal, object)
```

### 7.4 Multiple Inheritance and MRO
```python
class A:
    def greet(self): return "from A"

class B(A):
    def greet(self): return "from B, then " + super().greet()

class C(A):
    def greet(self): return "from C, then " + super().greet()

class D(B, C):
    def greet(self): return "from D, then " + super().greet()

D().greet()    # "from D, then from B, then from C, then from A"
D.__mro__      # (D, B, C, A, object) — C3 linearization
```

### 7.5 Encapsulation and Properties
```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner          # public
        self._balance = balance     # protected (convention)
        self.__pin = "1234"         # private (name-mangled)
    
    def get_balance(self):
        return self._balance
    
    def withdraw(self, amount):
        if 0 < amount <= self._balance:
            self._balance -= amount
        else:
            raise ValueError("Insufficient funds")

# __pin is mangled to _BankAccount__pin
account = BankAccount("Alice", 1000)
account._balance          # accessible but convention says don't
# account.__pin           # AttributeError
account._BankAccount__pin # "1234" — bypass mangling

# Property — controlled attribute access
class Temperature:
    def __init__(self, celsius):
        self._celsius = celsius
    
    @property
    def celsius(self):              # getter — access as attribute
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):       # setter
        if value < -273.15:
            raise ValueError("Below absolute zero!")
        self._celsius = value
    
    @celsius.deleter
    def celsius(self):              # deleter
        del self._celsius
    
    @property
    def fahrenheit(self):           # computed (read-only)
        return self._celsius * 9/5 + 32

t = Temperature(25)
t.celsius           # 25 — calls getter
t.celsius = 30      # calls setter
t.fahrenheit        # 86.0
t.celsius = -300    # ValueError
del t.celsius       # calls deleter
```

### 7.6 Abstract Classes
```python
from abc import ABC, abstractmethod

class Shape(ABC):   # cannot be instantiated
    def __init__(self, color="white"):
        self.color = color
    
    @abstractmethod
    def area(self):      # MUST be implemented by subclasses
        pass
    
    @abstractmethod
    def perimeter(self):
        pass
    
    def describe(self):  # concrete method
        return f"A {self.color} shape, area={self.area():.2f}"

class Circle(Shape):
    def __init__(self, radius, color="white"):
        super().__init__(color)
        self.radius = radius
    
    def area(self):
        import math
        return math.pi * self.radius ** 2
    
    def perimeter(self):
        import math
        return 2 * math.pi * self.radius

c = Circle(5)
c.area()       # 78.54...
c.describe()   # "A white shape, area=78.54"
# Shape()      # TypeError: Can't instantiate abstract class
```

---

## 8. Dunder (Magic) Methods

### 8.1 String Representation
```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):    # str(p), print(p)
        return f"Point({self.x}, {self.y})"
    
    def __repr__(self):   # repr(p), shell, f"{p!r}"
        return f"Point(x={self.x}, y={self.y})"
    
    def __format__(self, spec):   # format(p, spec), f"{p:spec}"
        if spec == "polar":
            r = (self.x**2 + self.y**2)**0.5
            return f"r={r:.2f}"
        return str(self)

p = Point(3, 4)
str(p)      # "Point(3, 4)"
repr(p)     # "Point(x=3, y=4)"
f"{p}"      # "Point(3, 4)"
f"{p!r}"    # "Point(x=3, y=4)"
```

### 8.2 Arithmetic Operators
```python
class Vector:
    def __init__(self, x, y):
        self.x = x; self.y = y
    
    def __add__(self, other):       # v1 + v2
        return Vector(self.x+other.x, self.y+other.y)
    
    def __sub__(self, other):       # v1 - v2
        return Vector(self.x-other.x, self.y-other.y)
    
    def __mul__(self, scalar):      # v * 3
        return Vector(self.x*scalar, self.y*scalar)
    
    def __rmul__(self, scalar):     # 3 * v
        return self.__mul__(scalar)
    
    def __truediv__(self, scalar):  # v / 2
        return Vector(self.x/scalar, self.y/scalar)
    
    def __neg__(self):              # -v
        return Vector(-self.x, -self.y)
    
    def __abs__(self):              # abs(v)
        return (self.x**2 + self.y**2)**0.5
    
    def __iadd__(self, other):      # v += other
        self.x += other.x; self.y += other.y
        return self
    
    def __str__(self):
        return f"Vector({self.x}, {self.y})"

v1, v2 = Vector(1,2), Vector(3,4)
v1 + v2    # Vector(4, 6)
v1 * 3     # Vector(3, 6)
3 * v1     # Vector(3, 6)
abs(v1)    # 2.236...
-v1        # Vector(-1, -2)
```

### 8.3 Comparison Operators
```python
class Temperature:
    def __init__(self, degrees):
        self.degrees = degrees
    
    def __eq__(self, other):     # ==
        return self.degrees == other.degrees
    
    def __ne__(self, other):     # !=
        return not self.__eq__(other)
    
    def __lt__(self, other):     # <
        return self.degrees < other.degrees
    
    def __le__(self, other):     # <=
        return self.degrees <= other.degrees
    
    def __gt__(self, other):     # >
        return self.degrees > other.degrees
    
    def __ge__(self, other):     # >=
        return self.degrees >= other.degrees
    
    def __hash__(self):          # needed if __eq__ defined (for sets/dicts)
        return hash(self.degrees)

# functools.total_ordering — define __eq__ and ONE of lt/le/gt/ge,
# get the rest automatically
from functools import total_ordering

@total_ordering
class Student:
    def __init__(self, grade):
        self.grade = grade
    def __eq__(self, other):
        return self.grade == other.grade
    def __lt__(self, other):
        return self.grade < other.grade
    # __le__, __gt__, __ge__ automatically derived!
```

### 8.4 Container / Sequence Protocol
```python
class NumberList:
    def __init__(self, *args):
        self._data = list(args)
    
    def __len__(self):              # len(obj)
        return len(self._data)
    
    def __getitem__(self, index):   # obj[key], obj[1:3]
        return self._data[index]
    
    def __setitem__(self, index, value):  # obj[key] = val
        self._data[index] = value
    
    def __delitem__(self, index):   # del obj[key]
        del self._data[index]
    
    def __contains__(self, item):   # item in obj
        return item in self._data
    
    def __iter__(self):             # for x in obj, iter(obj)
        return iter(self._data)
    
    def __reversed__(self):         # reversed(obj)
        return reversed(self._data)
    
    def __bool__(self):             # bool(obj), if obj:
        return len(self._data) > 0

nl = NumberList(1, 2, 3, 4, 5)
len(nl)         # 5
nl[0]           # 1
nl[1:3]         # [2, 3]
nl[0] = 10
3 in nl         # True
list(nl)        # [10, 2, 3, 4, 5]
bool(nl)        # True
```

### 8.5 Context Manager Protocol
```python
class FileManager:
    def __init__(self, filename, mode):
        self.filename = filename
        self.mode = mode
        self.file = None
    
    def __enter__(self):             # called when 'with' begins
        self.file = open(self.filename, self.mode)
        return self.file
    
    def __exit__(self, exc_type, exc_val, traceback):   # called on exit
        self.file.close()
        return False   # False: don't suppress exceptions

with FileManager("file.txt", "r") as f:
    content = f.read()

# contextmanager decorator (simpler)
from contextlib import contextmanager

@contextmanager
def managed_connection(host):
    print(f"Connecting to {host}")
    conn = {"host": host}
    try:
        yield conn              # __enter__ — yield value
    finally:
        print("Disconnecting")  # __exit__ — always runs

with managed_connection("localhost") as conn:
    print(f"Connected to {conn['host']}")
```

### 8.6 Callable Protocol and Other Dunders
```python
class Adder:
    def __init__(self, n):
        self.n = n
    
    def __call__(self, x):    # obj(x) — make object callable
        return x + self.n

add5 = Adder(5)
add5(10)   # 15
callable(add5)   # True

# __slots__ — restrict attributes, save memory
class Efficient:
    __slots__ = ['x', 'y']   # only x and y allowed
    
    def __init__(self, x, y):
        self.x = x; self.y = y

e = Efficient(1, 2)
# e.z = 3   # AttributeError!
# No per-instance __dict__ — saves ~50-200 bytes/instance

# __missing__ — called on dict subclass when key not found
class DefaultDict(dict):
    def __missing__(self, key):
        return f"Key '{key}' not found"

d = DefaultDict({"a": 1})
d["a"]    # 1
d["b"]    # "Key 'b' not found" — no KeyError

# __class_getitem__ — enables generic syntax
class Stack:
    def __class_getitem__(cls, item):   # Stack[int] syntax
        return cls

# __init_subclass__ — called when subclassed
class Plugin:
    registry = []
    def __init_subclass__(cls, **kwargs):
        super().__init_subclass__(**kwargs)
        Plugin.registry.append(cls)

class PluginA(Plugin): pass
class PluginB(Plugin): pass
Plugin.registry   # [PluginA, PluginB]
```

### 8.7 Complete Dunder Reference Table

| Method | Triggered By |
|---|---|
| `__init__` | `obj = Class()` |
| `__del__` | object garbage collected |
| `__str__` | `str(obj)`, `print(obj)` |
| `__repr__` | `repr(obj)`, shell display |
| `__format__` | `format(obj)`, f-string |
| `__bool__` | `bool(obj)`, `if obj:` |
| `__len__` | `len(obj)` |
| `__getitem__` | `obj[key]` |
| `__setitem__` | `obj[key] = val` |
| `__delitem__` | `del obj[key]` |
| `__contains__` | `item in obj` |
| `__iter__` | `for x in obj`, `iter(obj)` |
| `__next__` | `next(obj)` |
| `__reversed__` | `reversed(obj)` |
| `__enter__` | `with obj as x:` |
| `__exit__` | end of `with` block |
| `__call__` | `obj()` |
| `__add__` | `obj + other` |
| `__sub__` | `obj - other` |
| `__mul__` | `obj * other` |
| `__truediv__` | `obj / other` |
| `__floordiv__` | `obj // other` |
| `__mod__` | `obj % other` |
| `__pow__` | `obj ** other` |
| `__neg__` | `-obj` |
| `__abs__` | `abs(obj)` |
| `__radd__` | `other + obj` (reflected) |
| `__iadd__` | `obj += other` (in-place) |
| `__eq__` | `obj == other` |
| `__ne__` | `obj != other` |
| `__lt__` | `obj < other` |
| `__le__` | `obj <= other` |
| `__gt__` | `obj > other` |
| `__ge__` | `obj >= other` |
| `__hash__` | `hash(obj)`, dict key, set elem |
| `__getattr__` | `obj.attr` (not found) |
| `__setattr__` | `obj.attr = val` |
| `__delattr__` | `del obj.attr` |
| `__get__` | descriptor accessed |
| `__set__` | descriptor assigned |
| `__delete__` | descriptor deleted |
| `__set_name__` | descriptor class created |
| `__slots__` | class definition (restrict attrs) |
| `__missing__` | `dict[missing_key]` |
| `__init_subclass__` | subclass created |
| `__class_getitem__` | `Class[type]` |
| `__sizeof__` | `sys.getsizeof(obj)` |
| `__doc__` | `obj.__doc__` (docstring) |
| `__name__` | `func.__name__` |
| `__module__` | `obj.__module__` |
| `__dict__` | `obj.__dict__` (instance attrs) |
| `__class__` | `obj.__class__` |
| `__bases__` | `Class.__bases__` |
| `__mro__` | `Class.__mro__` |

---

## 9. Exception Handling

### 9.1 Basic Exception Handling
```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")      # Error: division by zero
else:
    print(f"Result: {result}")  # runs only if NO exception
finally:
    print("Always runs")         # runs regardless

# Multiple except clauses
try:
    number = int(value)
    result = 100 / number
except ValueError:
    print("Cannot convert to int")
except ZeroDivisionError:
    print("Cannot divide by zero")
except (TypeError, AttributeError) as e:
    print(f"Type/Attr error: {e}")
except Exception as e:              # catch-all (be specific when possible)
    print(f"Unexpected: {type(e).__name__}: {e}")

# Raise exceptions
def validate_age(age):
    if not isinstance(age, int):
        raise TypeError(f"Age must be int, got {type(age).__name__}")
    if age < 0:
        raise ValueError(f"Age cannot be negative: {age}")

# Re-raise
try:
    validate_age(-5)
except ValueError as e:
    print(f"Caught: {e}")
    raise   # re-raises same exception, preserves traceback

# Exception chaining
try:
    int("bad")
except ValueError as e:
    raise RuntimeError("Processing failed") from e
    # Shows: "The above exception was the direct cause..."

# Exception hierarchy (partial)
# BaseException
# ├── SystemExit
# ├── KeyboardInterrupt
# └── Exception
#     ├── ArithmeticError: ZeroDivisionError, OverflowError
#     ├── LookupError: IndexError, KeyError
#     ├── TypeError
#     ├── ValueError
#     ├── AttributeError
#     ├── RuntimeError
#     ├── StopIteration
#     ├── NameError
#     └── OSError: FileNotFoundError, PermissionError
```

### 9.2 Custom Exceptions
```python
class AppError(Exception):
    """Base class for application errors."""
    pass

class ValidationError(AppError):
    def __init__(self, field, message, value=None):
        self.field = field
        self.message = message
        self.value = value
        super().__init__(f"Validation error on '{field}': {message}")

class DatabaseError(AppError):
    def __init__(self, operation, details):
        self.operation = operation
        super().__init__(f"DB error during {operation}: {details}")

try:
    raise ValidationError("email", "must contain @", "bad_email")
except ValidationError as e:
    print(e.field)    # "email"
    print(str(e))     # "Validation error on 'email': must contain @"
except AppError:
    print("Some app error")

# contextlib utilities
from contextlib import suppress, contextmanager

with suppress(FileNotFoundError):
    import os
    os.remove("nonexistent.txt")   # no error raised
```

---

## 10. Modules and Packages

### 10.1 Importing
```python
import math
math.sqrt(16)    # must use prefix

from math import sqrt, pi
sqrt(16)         # no prefix

import numpy as np           # alias
from datetime import datetime as dt

# Conditional import
try:
    import ujson as json     # faster library
except ImportError:
    import json              # standard fallback

# if __name__ == "__main__":
# Runs ONLY when script executed directly, NOT when imported
if __name__ == "__main__":
    print("Running directly")
```

### 10.2 Key Standard Library Modules
```python
import os
os.getcwd()
os.listdir(".")
os.path.join("dir","file")
os.path.exists("file.txt")
os.path.isfile("f.txt"); os.path.isdir("d")
os.path.basename("/path/file.txt")   # "file.txt"
os.path.dirname("/path/file.txt")    # "/path"
os.path.splitext("file.txt")         # ("file", ".txt")
os.makedirs("dir/sub", exist_ok=True)
os.environ.get("HOME")

import sys
sys.argv            # command-line arguments
sys.path            # module search paths
sys.version
sys.exit(0)         # exit with code 0

from datetime import datetime, date, timedelta
datetime.now()
date.today()
datetime.now().strftime("%Y-%m-%d %H:%M:%S")
datetime.strptime("2024-01-15", "%Y-%m-%d")
timedelta(days=7)
datetime.now() + timedelta(hours=2)

import json
json.dumps({"a":1})           # → JSON string
json.dumps(data, indent=2)    # pretty
json.loads('{"a":1}')         # → dict
with open("f.json","w") as f: json.dump(data, f)
with open("f.json") as f: data = json.load(f)

from pathlib import Path
p = Path(".")
(p / "sub" / "file.txt").read_text()
Path("f.txt").write_text("hello")

import re
re.match(r"\d+", "123abc")      # match at start
re.search(r"\d+", "abc123")     # search anywhere
re.findall(r"\d+", "a1b2c3")    # ['1','2','3']
re.sub(r"\d+", "NUM", "a1b2")   # "aNUMbNUM"
re.split(r"\s+", "a  b c")      # ['a','b','c']

pattern = re.compile(r"(\d{4})-(\d{2})-(\d{2})")
m = pattern.search("Date: 2024-01-15")
m.groups()   # ('2024','01','15')
```

---

## 11. File I/O

```python
# Reading
with open("file.txt", "r") as f:
    content = f.read()       # entire file as string
    
with open("file.txt") as f:
    lines = f.readlines()    # list of lines (with \n)

with open("file.txt") as f:
    for line in f:           # memory-efficient line-by-line
        print(line.strip())

# Writing
with open("out.txt", "w") as f:   # 'w' overwrites
    f.write("Hello\n")
    f.writelines(["a\n","b\n"])

with open("out.txt", "a") as f:   # 'a' appends
    f.write("more\n")

# Modes: r=read, w=write(truncate), a=append, x=create(error if exists)
#        b=binary (rb,wb), +=read+write (r+,w+)

# Binary
with open("image.png", "rb") as f: data = f.read()
with open("copy.png", "wb") as f: f.write(data)

# CSV
import csv
with open("data.csv", "w", newline="") as f:
    writer = csv.writer(f)
    writer.writerow(["Name","Age","Score"])
    writer.writerows([["Alice",30,85],["Bob",25,92]])

with open("data.csv") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["Name"], row["Score"])
```

---

## 12. Advanced Python Concepts

### 12.1 Context Managers
```python
# contextlib.contextmanager — generator-based
from contextlib import contextmanager

@contextmanager
def database_transaction(db):
    print("BEGIN TRANSACTION")
    try:
        yield db              # before yield = __enter__
        print("COMMIT")
    except Exception:
        print("ROLLBACK")
        raise
    finally:
        print("CONNECTION CLOSED")

with database_transaction(my_db) as db:
    db.execute("INSERT ...")

# Nested context managers
with open("in.txt") as fin, open("out.txt","w") as fout:
    fout.write(fin.read())

# contextlib utilities
from contextlib import suppress, redirect_stdout
import io

with suppress(FileNotFoundError):   # ignore specific exception
    os.remove("nonexistent.txt")

# Capture stdout
f = io.StringIO()
with redirect_stdout(f):
    print("Captured!")
output = f.getvalue()   # "Captured!\n"
```

### 12.2 Dataclasses (Python 3.7+)
```python
from dataclasses import dataclass, field, fields, asdict, astuple
from typing import List

@dataclass
class Employee:
    # Auto-generates: __init__, __repr__, __eq__
    name: str
    salary: float
    department: str = "Unknown"
    skills: List[str] = field(default_factory=list)
    
    def __post_init__(self):
        self.name = self.name.title()   # post-init processing
    
    def give_raise(self, pct):
        self.salary *= (1 + pct/100)

emp = Employee("alice", 80000, "Engineering", ["Python"])
print(emp)
# Employee(name='Alice', salary=80000, department='Engineering', skills=['Python'])

Employee("Alice",80000) == Employee("Alice",80000)   # True (auto __eq__)

# Frozen (immutable)
@dataclass(frozen=True)
class Point:
    x: float
    y: float

p = Point(3, 4)
# p.x = 5   # FrozenInstanceError!

# Field options
@dataclass
class Config:
    host: str
    port: int = field(default=8080)
    debug: bool = field(default=False, repr=False)  # exclude from repr
    _cache: dict = field(default_factory=dict, init=False, repr=False)

# Utility functions
asdict(emp)    # {'name':'Alice','salary':80000,...}
astuple(emp)   # ('Alice',80000,'Engineering',['Python'])
fields(emp)    # tuple of Field objects
```

### 12.3 Type Hints
```python
from typing import List, Dict, Tuple, Set, Optional, Union, Callable, Any, TypeVar

def greet(name: str) -> str:
    return f"Hello, {name}!"

def add(a: int, b: int) -> int:
    return a + b

# Optional — value or None
def find(id: int) -> Optional[str]:   # str | None
    ...

# Union — multiple types
def process(data: Union[str, bytes]) -> str: ...
def process(data: str | bytes) -> str: ...    # Python 3.10+

# Collections
def process(names: List[str]) -> Dict[str, int]: ...

# Python 3.9+ — built-ins directly as generics
def process(names: list[str]) -> dict[str, int]: ...

# Callable
def apply(func: Callable[[int, int], int], a: int, b: int) -> int:
    return func(a, b)

# TypeVar — generic functions
T = TypeVar('T')
def first(lst: list[T]) -> T:
    return lst[0]

# Literal — specific values
from typing import Literal
def direction(d: Literal["N","S","E","W"]) -> None: ...

# TypedDict
from typing import TypedDict
class User(TypedDict):
    name: str
    age: int

# Protocol — structural subtyping (duck typing)
from typing import Protocol
class Drawable(Protocol):
    def draw(self) -> None: ...

def render(obj: Drawable) -> None:
    obj.draw()   # any object with draw() method works
```

### 12.4 Enumerations
```python
from enum import Enum, auto, IntEnum, Flag

class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3

class Direction(Enum):
    NORTH = auto()   # auto-assigns 1,2,3,4
    SOUTH = auto()
    EAST = auto()
    WEST = auto()

Color.RED           # <Color.RED: 1>
Color.RED.name      # 'RED'
Color.RED.value     # 1
Color(1)            # <Color.RED: 1> — by value
Color["RED"]        # <Color.RED: 1> — by name
list(Color)         # [<Color.RED:1>, <Color.GREEN:2>, <Color.BLUE:3>]
Color.RED == Color.RED   # True
Color.RED is Color.RED   # True (singleton)

class Priority(IntEnum):    # comparable with ints
    LOW = 1; MEDIUM = 2; HIGH = 3

Priority.HIGH > Priority.LOW   # True
Priority.HIGH == 3             # True

class Permission(Flag):         # bitwise operations
    READ = auto()     # 1
    WRITE = auto()    # 2
    EXECUTE = auto()  # 4

perm = Permission.READ | Permission.WRITE
Permission.READ in perm    # True
```

### 12.5 Functional Programming Tools
```python
from functools import partial, reduce, singledispatch
from operator import itemgetter, attrgetter

# partial — fix some arguments
def power(base, exp): return base ** exp

square = partial(power, exp=2)
cube = partial(power, exp=3)
square(5)   # 25    cube(3)   # 27

# operator functions (faster than lambdas)
from operator import add, mul
reduce(add, [1,2,3,4,5])   # 15
reduce(mul, [1,2,3,4,5])   # 120

# itemgetter
get_age = itemgetter("age")
people = [{"name":"Alice","age":30},{"name":"Bob","age":25}]
sorted(people, key=itemgetter("age"))

# singledispatch — type-based overloading
@singledispatch
def process(data):
    raise NotImplementedError(f"Cannot process {type(data)}")

@process.register(str)
def _(data): return data.upper()

@process.register(int)
def _(data): return data * 2

@process.register(list)
def _(data): return [process(x) for x in data]

process("hello")   # "HELLO"
process(5)         # 10
process([1,"hi"]) # [2, "HI"]
```

---

## 13. Concurrency and Parallelism

### 13.1 Threading
```python
import threading, time

def worker(name, delay):
    print(f"Thread {name} starting")
    time.sleep(delay)
    print(f"Thread {name} done")

t1 = threading.Thread(target=worker, args=("A", 2))
t2 = threading.Thread(target=worker, args=("B", 1))
t1.start(); t2.start()
t1.join(); t2.join()   # wait for completion

# Thread safety with Lock
counter = 0
lock = threading.Lock()

def increment():
    global counter
    for _ in range(100000):
        with lock:   # thread-safe
            counter += 1

# Thread-safe queue
from queue import Queue
q = Queue()

# GIL (Global Interpreter Lock):
# - Only ONE thread runs Python bytecode at a time in CPython
# - Threading GOOD for: I/O-bound tasks (network, file I/O) — GIL released during I/O
# - Threading NOT GOOD for: CPU-bound tasks — use multiprocessing instead
```

### 13.2 Multiprocessing
```python
import multiprocessing as mp

def cpu_work(n):
    return sum(i*i for i in range(n))

# Process pool — true parallelism, bypasses GIL
with mp.Pool(processes=4) as pool:
    results = pool.map(cpu_work, [1000000, 2000000, 3000000, 4000000])

# For CPU-bound: multiprocessing
# For I/O-bound: threading or asyncio
```

### 13.3 Asyncio
```python
import asyncio

# async/await — cooperative concurrency (single thread)
# Best for: many concurrent I/O operations

async def count(name, delay):
    print(f"{name}: start")
    await asyncio.sleep(delay)   # non-blocking sleep
    print(f"{name}: end")

async def main():
    # concurrent — total time = max(delays)
    await asyncio.gather(
        count("A", 2),
        count("B", 1),
        count("C", 3)
    )
    # A start, B start, C start, B end, A end, C end (total ~3s not 6s)

asyncio.run(main())

# asyncio.create_task — schedule without waiting
async def main():
    task1 = asyncio.create_task(count("A", 2))
    task2 = asyncio.create_task(count("B", 1))
    await task1; await task2

# Async context manager and generator
async def async_range(n):
    for i in range(n):
        await asyncio.sleep(0.1)
        yield i

async def main():
    async for n in async_range(5):
        print(n)
```

---

## 14. NumPy Essentials

```python
import numpy as np

# Array creation
arr = np.array([1, 2, 3, 4, 5])
arr2d = np.array([[1,2,3],[4,5,6]])

np.zeros((3,4))          # 3x4 zeros
np.ones((2,3))           # 2x3 ones
np.eye(3)                # 3x3 identity
np.arange(0,10,2)        # [0,2,4,6,8]
np.linspace(0,1,5)       # [0, 0.25, 0.5, 0.75, 1.0]
np.random.rand(3,4)      # uniform [0,1)
np.random.randn(3,4)     # standard normal
np.random.randint(0,10,(3,4))

# Properties
arr.shape    # (5,)
arr.ndim     # 1
arr.size     # 5
arr.dtype    # dtype('int64')

# Indexing and slicing
arr[0]     # 1    arr[-1]   # 5    arr[1:3]   # [2,3]
arr[::2]   # [1,3,5]

arr2d[0,1]    # 2 (row 0, col 1)
arr2d[1,:]    # [4,5,6]
arr2d[:,2]    # [3,6]
arr2d[:2,1:]  # [[2,3],[5,6]]

# Boolean indexing
arr = np.array([1,2,3,4,5,6])
arr[arr > 3]   # [4,5,6]

# Vectorized math (element-wise)
a, b = np.array([1,2,3]), np.array([4,5,6])
a + b    # [5,7,9]
a * b    # [4,10,18]
a ** 2   # [1,4,9]
a + 10   # [11,12,13]

np.dot(a, b)   # 32  (dot product)
a @ b          # 32  (same with @)

# Aggregation
arr = np.array([[1,2,3],[4,5,6]])
arr.sum()         # 21
arr.sum(axis=0)   # [5,7,9] (along rows — down each column)
arr.sum(axis=1)   # [6,15]  (along cols — across each row)
arr.mean()        # 3.5
arr.std()         # std deviation
arr.min(); arr.max()
arr.argmin(); arr.argmax()   # index of min/max

# Reshaping
arr = np.arange(12)
arr.reshape(3,4)    # 3x4
arr.reshape(-1,3)   # auto-compute: 4x3
arr.flatten()       # 1D copy
arr.T               # transpose

# Stacking
a, b = np.array([1,2,3]), np.array([4,5,6])
np.vstack([a,b])   # [[1,2,3],[4,5,6]]
np.hstack([a,b])   # [1,2,3,4,5,6]

# Universal functions
np.sqrt(arr); np.exp(arr); np.log(arr); np.abs(arr)
np.where(arr > 3, "high", "low")   # conditional
np.maximum(a, b)                    # element-wise max
```

---

## 15. Pandas Essentials

```python
import pandas as pd
import numpy as np

# ---- SERIES ----
s = pd.Series([10,20,30], index=["a","b","c"])
s["a"]        # 10
s[s > 15]     # filter

# ---- DATAFRAME ----
df = pd.DataFrame({
    "name": ["Alice","Bob","Charlie","David"],
    "age": [30,25,35,28],
    "score": [85,92,78,95],
    "dept": ["Eng","Sales","Eng","HR"]
})

df = pd.read_csv("data.csv")
df.to_csv("output.csv", index=False)

# Exploration
df.shape       # (4,4)
df.dtypes      # column types
df.info()      # types + non-null counts + memory
df.describe()  # count,mean,std,min,quartiles,max
df.head(3); df.tail(2); df.sample(2)
df.columns; df.index
df.nunique()   # unique values per column

# ---- SELECTION ----
df["name"]            # Series
df[["name","score"]]  # DataFrame

# iloc — integer position
df.iloc[0]            # first row
df.iloc[0:2]          # rows 0-1
df.iloc[0, 1]         # row 0, col 1
df.iloc[[0,2],[1,2]]  # specific rows+cols

# loc — label
df.loc[0]             # row with label 0
df.loc[0:2]           # labels 0 to 2 INCLUSIVE
df.loc[0, "name"]     # row 0, col "name"

# Boolean indexing
df[df["score"] > 85]
df[df["dept"] == "Eng"]
df[(df["score"] > 80) & (df["age"] < 30)]   # AND
df[(df["dept"]=="Eng") | (df["dept"]=="HR")]  # OR
df[df["name"].isin(["Alice","Charlie"])]
df[~df["dept"].isin(["Sales"])]              # NOT

# ---- MANIPULATION ----
df["grade"] = ["B","A","C","A"]
df["score_sq"] = df["score"] ** 2
df["senior"] = df["age"].apply(lambda x: x > 30)
df["full"] = df["name"] + " - " + df["dept"]

# apply
df["name_len"] = df["name"].apply(len)
df["sg"] = df["score"].apply(
    lambda x: "A" if x>=90 else "B" if x>=80 else "C"
)
df.apply(lambda row: row["score"]/row["age"], axis=1)  # row-wise

# map — element-wise on Series
df["code"] = df["dept"].map({"Eng":"E","Sales":"S","HR":"H"})

# Replace
df["dept"].replace("Eng","Engineering")
df.replace({"Eng":"Engineering"})

# Rename
df.rename(columns={"name":"employee","score":"performance"})

# Drop
df.drop("grade", axis=1)
df.drop(columns=["grade","senior"])
df.drop([0,2], axis=0)

# Sort
df.sort_values("score", ascending=False)
df.sort_values(["dept","score"], ascending=[True,False])

# ---- MISSING DATA ----
df.isnull().sum()          # NaN count per column
df.dropna()                # drop rows with any NaN
df.dropna(subset=["score"])
df.fillna(0)
df.fillna({"age": df["age"].mean()})
df["score"].fillna(df["score"].median())
df.fillna(method="ffill")  # forward fill
df.fillna(method="bfill")  # backward fill
df.interpolate()            # interpolate numerics

# ---- GROUPBY ----
grouped = df.groupby("dept")
grouped["score"].mean()
grouped["score"].agg(["mean","std","min","max"])
grouped.agg({"score":"mean","age":"max"})

# Named aggregation
grouped.agg(
    avg_score=("score","mean"),
    max_age=("age","max"),
    count=("name","count")
)

# transform — same shape as original
df["dept_avg"] = df.groupby("dept")["score"].transform("mean")

# filter — keep/remove entire groups
df.groupby("dept").filter(lambda g: len(g) >= 2)

# apply — custom function per group
df.groupby("dept").apply(lambda g: g.nlargest(1,"score"))

# ---- MERGING ----
df1 = pd.DataFrame({"id":[1,2,3],"name":["A","B","C"]})
df2 = pd.DataFrame({"id":[1,2,4],"score":[85,92,78]})

pd.merge(df1, df2, on="id", how="inner")  # INNER JOIN
pd.merge(df1, df2, on="id", how="left")   # LEFT JOIN
pd.merge(df1, df2, on="id", how="outer")  # FULL OUTER JOIN
pd.merge(df1, df2, left_on="id", right_on="emp_id")

pd.concat([df1,df2], axis=0)              # stack vertically
pd.concat([df1,df2], axis=1)              # stack horizontally
pd.concat([df1,df2], ignore_index=True)   # reset index

# ---- PIVOT TABLES ----
df.pivot_table(values="score", index="dept", aggfunc="mean")
pd.pivot_table(df, values="score", index="dept",
               columns="senior", aggfunc=["mean","count"], fill_value=0)

# ---- STRING OPERATIONS ----
df["name"].str.upper(); df["name"].str.lower()
df["name"].str.len()
df["name"].str.contains("li")   # boolean mask
df["name"].str.startswith("A")
df["name"].str.replace("a","@")
df["name"].str.split(" ")

# ---- DATES ----
df["date"] = pd.to_datetime(df["date_str"])
df["year"] = df["date"].dt.year
df["month"] = df["date"].dt.month
df["dow"] = df["date"].dt.dayofweek   # 0=Monday

pd.date_range("2024-01-01", periods=12, freq="M")
pd.date_range("2024-01-01","2024-12-31", freq="D")

# ---- PERFORMANCE ----
# NEVER use iterrows for computation (very slow)
# BAD:
for i, row in df.iterrows():
    df.at[i,"new"] = row["score"] * 2

# GOOD (vectorized):
df["new"] = df["score"] * 2

# Memory optimization
df["dept"] = df["dept"].astype("category")
df["age"] = df["age"].astype("int32")
df["score"] = df["score"].astype("float32")
```

---

## 16. Important Built-in Functions

```python
# Math
abs(-5)           # 5
round(3.14159,2)  # 3.14
pow(2,10)         # 1024
divmod(17,5)      # (3, 2) — quotient and remainder
max(3,1,4); min(3,1,4)
sum([1,2,3,4])    # 10

# Type conversion
int("42"); float("3.14"); str(42); bool(0); list({1,2})

# Object inspection
type(42)                      # <class 'int'>
isinstance(42, int)           # True
issubclass(bool, int)         # True
id(obj)                       # memory address (unique identity)
dir(obj)                      # list attributes/methods
vars(obj)                     # object's __dict__
hasattr(obj,"attr")           # True if attribute exists
getattr(obj,"attr",default)   # get with optional default
setattr(obj,"attr",value)     # set attribute
delattr(obj,"attr")           # delete attribute
callable(obj)                 # True if obj()

# Sequences
len([1,2,3]); range(5)
enumerate([a,b,c])       # (0,a),(1,b),(2,c)
zip([1,2],[a,b])         # (1,a),(2,b)
sorted([3,1,2])          # [1,2,3]
reversed([1,2,3])        # iterator: 3,2,1
all([T,T,F])             # False
any([F,T,F])             # True
filter(func, iter)       # lazy filter
map(func, iter)          # lazy map

# Iteration
iter([1,2,3])            # iterator
next(iterator)           # next value
next(iterator, default)  # with fallback

# Introspection
help(obj)                # documentation
hash("hello")            # hash value

# Evaluation (use carefully!)
eval("2+2")              # 4 — evaluate expression
exec("x=5")             # execute statement

# Scope
locals()   # dict of local variables
globals()  # dict of global variables
```

---

## 17. Python Memory Model

### 17.1 Variables Are References
```python
# Variables are labels (references) to objects, not containers
x = [1, 2, 3]
y = x            # y and x reference SAME object
y.append(4)
print(x)         # [1,2,3,4] — affected!

y = x[:]         # y is now a COPY (new object)
y.append(5)
print(x)         # [1,2,3,4] — unchanged
```

### 17.2 Mutable vs. Immutable
```python
# IMMUTABLE: int, float, str, tuple, frozenset, bytes
# MUTABLE: list, dict, set, bytearray, most custom objects

s = "hello"
# s[0] = "H"    # TypeError — immutable!
s = "H" + s[1:] # new string

lst = [1, 2, 3]
lst[0] = 10     # OK — mutable

# Integer caching
a = 256; b = 256
a is b    # True (Python caches -5 to 256)

a = 1000; b = 1000
a is b    # False (separate objects)
a == b    # True (same value)
```

### 17.3 Shallow vs. Deep Copy
```python
import copy

original = [[1,2,3],[4,5,6]]

# Shallow copy — new outer, inner objects SHARED
shallow = original.copy()
shallow = original[:]
shallow = list(original)
shallow = copy.copy(original)

shallow[0][0] = 99
print(original[0][0])   # 99 — inner list is shared!

# Deep copy — recursively copies everything
deep = copy.deepcopy(original)
deep[0][0] = 100
print(original[0][0])   # 99 — unchanged
```

---

## 18. Common Algorithms

### 18.1 Sorting
```python
# Python's built-in: Timsort — O(n log n) — USE THIS
lst.sort()
sorted(lst)

# Merge Sort — O(n log n), stable, divide-and-conquer
def merge_sort(arr):
    if len(arr) <= 1: return arr
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    return merge(left, right)

def merge(left, right):
    result = []; i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i]); i += 1
        else:
            result.append(right[j]); j += 1
    result.extend(left[i:]); result.extend(right[j:])
    return result

# Quick Sort — O(n log n) avg, O(n²) worst
def quick_sort(arr):
    if len(arr) <= 1: return arr
    pivot = arr[len(arr)//2]
    left = [x for x in arr if x < pivot]
    mid = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quick_sort(left) + mid + quick_sort(right)
```

### 18.2 Searching
```python
# Binary Search — O(log n) — sorted array required
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if arr[mid] == target: return mid
        elif arr[mid] < target: lo = mid + 1
        else: hi = mid - 1
    return -1

import bisect
arr = [1,3,5,7,9,11]
bisect.bisect_left(arr, 7)   # 3
bisect.insort(arr, 6)         # insert keeping sorted
```

### 18.3 Common Patterns
```python
# Two pointers
def two_sum_sorted(arr, target):
    l, r = 0, len(arr)-1
    while l < r:
        s = arr[l] + arr[r]
        if s == target: return [l,r]
        elif s < target: l += 1
        else: r -= 1
    return []

# Sliding window
def max_sum_subarray(arr, k):
    window = sum(arr[:k]); best = window
    for i in range(len(arr)-k):
        window = window - arr[i] + arr[i+k]
        best = max(best, window)
    return best

# Stack for balanced parens
def is_balanced(s):
    stack = []
    pairs = {")":"(","]":"[","}":"{"}
    for ch in s:
        if ch in "([{": stack.append(ch)
        elif ch in pairs:
            if not stack or stack[-1] != pairs[ch]: return False
            stack.pop()
    return len(stack) == 0

# BFS
from collections import deque
def bfs(graph, start):
    visited = {start}
    queue = deque([start])
    while queue:
        node = queue.popleft()
        print(node)
        for nb in graph[node]:
            if nb not in visited:
                visited.add(nb); queue.append(nb)

# DFS
def dfs(graph, node, visited=None):
    if visited is None: visited = set()
    visited.add(node); print(node)
    for nb in graph[node]:
        if nb not in visited:
            dfs(graph, nb, visited)
```

---

## 19. Best Practices and Gotchas

```python
# ---- NAMING ----
my_variable = 42          # snake_case for variables/functions
class BankAccount: pass   # PascalCase for classes
MAX_CONNECTIONS = 100     # UPPER_SNAKE_CASE for constants
_internal = True          # _single: protected (convention)
self.__private = "x"      # __double: name-mangled (enforced)

# ---- GOTCHAS ----

# 1. is vs ==
x = [1,2,3]; y = [1,2,3]
x == y    # True (same value)
x is y    # False (different objects)
x is None # ALWAYS use 'is' for None

# 2. Mutable default argument
def bad(lst=[]):      # BAD
    lst.append(1); return lst
bad()   # [1]    bad()   # [1,1]  ← BUG!

def good(lst=None):   # CORRECT
    if lst is None: lst = []
    lst.append(1); return lst

# 3. Floor division
-7 // 2   # -4 (floors toward -inf, NOT truncates!)
7 / 2     # 3.5 (always float)
7 // 2    # 3

# 4. Single-element tuple
t = (1,)   # tuple (comma needed!)
t = (1)    # just int 1

# 5. Closure loop bug
funcs = [lambda: i for i in range(5)]
funcs[0]()   # 4 — all capture final i
funcs = [lambda i=i: i for i in range(5)]  # fix
funcs[0]()   # 0 ✓

# 6. next() with default
gen = iter([1,2])
next(gen, None)   # 1
next(gen, None)   # 2
next(gen, None)   # None (no StopIteration)

# 7. dict.update order
d = {"a":1,"b":2}; d.update({"b":3,"c":4})
# d = {"a":1,"b":3,"c":4} — update values win

# 8. String immutability
s = "hello"
result = ""
for ch in s:
    result += ch.upper()   # slow: creates new string each time
result = "".join(ch.upper() for ch in s)  # fast

# ---- PYTHONIC PATTERNS ----
# Swap
a, b = b, a

# Check empty
if items:    # non-empty list is truthy
if not data: # falsy check

# Membership in set (faster)
if x in {1,2,3,4,5}:  # O(1) vs list O(n)

# Enumerate
for i, item in enumerate(items):   # ✓
for i in range(len(items)):        # ✗

# Walrus operator := (Python 3.8+)
if (n := len(data)) > 10:
    print(f"Too long: {n}")

while chunk := file.read(8192):    # read until empty
    process(chunk)

# EAFP > LBYL
# LBYL (Look Before You Leap):
if key in d: val = d[key]

# EAFP (Easier to Ask Forgiveness — more Pythonic):
try: val = d[key]
except KeyError: val = default

# Simpler:
val = d.get(key, default)
```

---

## 20. Key Dunder Methods Summary

| Method | Triggered By | Purpose |
|---|---|---|
| `__init__` | `Class()` | Constructor |
| `__del__` | garbage collection | Destructor |
| `__str__` | `str(obj)`, `print(obj)` | Human string |
| `__repr__` | `repr(obj)`, shell | Dev string |
| `__bool__` | `bool(obj)`, `if obj:` | Truthiness |
| `__len__` | `len(obj)` | Length |
| `__getitem__` | `obj[key]` | Index access |
| `__setitem__` | `obj[key]=val` | Index set |
| `__delitem__` | `del obj[key]` | Index delete |
| `__contains__` | `item in obj` | Membership |
| `__iter__` | `for x in obj` | Iteration start |
| `__next__` | `next(obj)` | Next value |
| `__enter__` | `with obj:` | Context enter |
| `__exit__` | end of `with` | Context exit |
| `__call__` | `obj()` | Callable |
| `__add__` | `obj + other` | Addition |
| `__sub__` | `obj - other` | Subtraction |
| `__mul__` | `obj * other` | Multiplication |
| `__truediv__` | `obj / other` | Division |
| `__floordiv__` | `obj // other` | Floor div |
| `__mod__` | `obj % other` | Modulo |
| `__pow__` | `obj ** other` | Power |
| `__neg__` | `-obj` | Negation |
| `__abs__` | `abs(obj)` | Absolute value |
| `__radd__` | `other + obj` | Reflected add |
| `__iadd__` | `obj += other` | In-place add |
| `__eq__` | `obj == other` | Equality |
| `__ne__` | `obj != other` | Inequality |
| `__lt__` | `obj < other` | Less than |
| `__le__` | `obj <= other` | Less/equal |
| `__gt__` | `obj > other` | Greater than |
| `__ge__` | `obj >= other` | Greater/equal |
| `__hash__` | `hash(obj)` | Hash value |
| `__getattr__` | `obj.attr` (not found) | Missing attr |
| `__setattr__` | `obj.attr = val` | Attr set |
| `__delattr__` | `del obj.attr` | Attr delete |
| `__get__` | descriptor access | Descriptor |
| `__set__` | descriptor assign | Descriptor |
| `__slots__` | class definition | Restrict attrs |
| `__missing__` | `dict[missing]` | Dict fallback |
| `__init_subclass__` | subclassing | Subclass hook |
| `__class_getitem__` | `Class[T]` | Generic syntax |
| `__sizeof__` | `sys.getsizeof(obj)` | Size in bytes |

---
*End of Python Comprehensive Notes.*
*Read alongside: Shared_Python.md for MCQ practice.*
