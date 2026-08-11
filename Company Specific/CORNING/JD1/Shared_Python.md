# Shared — Python MCQs
### Applies to: All JDs mentioning Python
> 35 Questions | 🟢 Easy | 🟡 Medium | 🔴 Hard

---

**Q1.** 🟢 Which of the following is a mutable data type in Python?
- A) `tuple`
- B) `str`
- C) `list`
- D) `int`

**Answer: C** — Lists are mutable; tuples and strings are immutable.

---

**Q2.** 🟢 What is the output of `print(type({}))` in Python?
- A) `<class 'set'>`
- B) `<class 'dict'>`
- C) `<class 'list'>`
- D) `<class 'NoneType'>`

**Answer: B** — `{}` without elements creates a dict, not a set. Use `set()` for an empty set.

---

**Q3.** 🟢 What does `len("hello world")` return?
- A) `10`
- B) `11`
- C) `5`
- D) `Error`

**Answer: B** — Includes the space character.

---

**Q4.** 🟢 What is the output of the following?
```python
x = [1, 2, 3]
x.append([4, 5])
print(len(x))
```
- A) `5`
- B) `4`
- C) `3`
- D) `Error`

**Answer: B** — `.append()` adds the entire `[4,5]` as a single element.

---

**Q5.** 🟢 Which keyword is used to define a function in Python?
- A) `func`
- B) `function`
- C) `def`
- D) `lambda` only

**Answer: C**

---

**Q6.** 🟢 What does the `range(2, 10, 3)` produce?
- A) `[2, 5, 8, 11]`
- B) `[2, 5, 8]`
- C) `[2, 3, 4, 5, 6, 7, 8, 9]`
- D) `[3, 6, 9]`

**Answer: B** — Start=2, stop=10 (exclusive), step=3 → 2, 5, 8.

---

**Q7.** 🟢 What is the output of `"hello"[1:4]`?
- A) `"hel"`
- B) `"ell"`
- C) `"ello"`
- D) `"hell"`

**Answer: B**

---

**Q8.** 🟢 Which Python library is primarily used for numerical array computation?
- A) Pandas
- B) Matplotlib
- C) NumPy
- D) Requests

**Answer: C**

---

**Q9.** 🟡 What is the output of the following?
```python
x = [1, 2, 3, 4, 5]
print(x[-2:])
```
- A) `[3, 4]`
- B) `[4, 5]`
- C) `[3, 4, 5]`
- D) `[5]`

**Answer: B** — Negative indexing: `-2` is the second-to-last element.

---

**Q10.** 🟡 What will this code print?
```python
d = {'a': 1, 'b': 2, 'c': 3}
print([k for k, v in d.items() if v > 1])
```
- A) `['a']`
- B) `['b', 'c']`
- C) `[2, 3]`
- D) `{'b': 2, 'c': 3}`

**Answer: B**

---

**Q11.** 🟡 What is the difference between `==` and `is` in Python?
- A) They are identical
- B) `==` checks value equality; `is` checks identity (whether two variables refer to the same object in memory)
- C) `is` checks value; `==` checks type
- D) `is` is used only for strings

**Answer: B**

---

**Q12.** 🟡 What is the output of the following?
```python
def foo(x, lst=[]):
    lst.append(x)
    return lst

print(foo(1))
print(foo(2))
```
- A) `[1]` then `[2]`
- B) `[1]` then `[1, 2]`
- C) `[1, 2]` then `[1, 2]`
- D) `Error`

**Answer: B** — Mutable default arguments are created once and shared across calls.

---

**Q13.** 🟡 What does the `*args` parameter do in a Python function?
- A) Passes keyword arguments as a dictionary
- B) Allows the function to accept any number of positional arguments, collected into a tuple
- C) Makes all arguments optional
- D) Unpacks a list before passing to the function

**Answer: B**

---

**Q14.** 🟡 What is a Python "list comprehension"? What does `[x**2 for x in range(5)]` produce?
- A) A loop stored in memory; produces `[0, 2, 4, 6, 8]`
- B) A concise way to create lists using an expression and a loop; produces `[0, 1, 4, 9, 16]`
- C) A filter function; produces `[1, 4, 9, 16, 25]`
- D) A generator expression; produces a generator object

**Answer: B**

---

**Q15.** 🟡 What is the output of the following?
```python
try:
    print(10 / 0)
except ZeroDivisionError:
    print("Cannot divide by zero")
finally:
    print("Done")
```
- A) `Cannot divide by zero`
- B) `Cannot divide by zero` then `Done`
- C) `Error` then `Done`
- D) `Done` only

**Answer: B** — `finally` always executes.

---

**Q16.** 🟡 In Pandas, what does `df.dropna()` do?
- A) Drops all columns with NaN values
- B) Removes rows containing at least one NaN (missing) value by default
- C) Replaces NaN values with 0
- D) Drops duplicate rows

**Answer: B**

---

**Q17.** 🟡 What is the difference between `df.loc[]` and `df.iloc[]` in Pandas?
- A) `loc` is faster; `iloc` is more accurate
- B) `loc` selects by label/index name; `iloc` selects by integer position
- C) `iloc` works only on columns; `loc` works on rows
- D) They are identical

**Answer: B**

---

**Q18.** 🟡 What does `df['col'].value_counts()` return in Pandas?
- A) The number of columns in the dataframe
- B) A Series with unique values and their occurrence counts, sorted descending
- C) The sum of all values in the column
- D) A list of unique values only

**Answer: B**

---

**Q19.** 🟡 What is the purpose of `pd.merge()` in Pandas?
- A) To concatenate DataFrames vertically
- B) To combine two DataFrames based on a common key column, similar to a SQL JOIN
- C) To merge duplicate rows into one
- D) To merge a DataFrame with a NumPy array

**Answer: B**

---

**Q20.** 🟡 What does the `yield` keyword do in Python?
- A) Returns a value and terminates the function
- B) Pauses the function, returns a value to the caller, and resumes from that point on the next call — making the function a generator
- C) Imports a module lazily
- D) Raises a custom exception

**Answer: B**

---

**Q21.** 🟡 What is a Python decorator?
- A) A formatting tool for print statements
- B) A function that wraps another function to add behavior before/after it runs, using `@decorator` syntax
- C) A class that decorates UI elements
- D) A type of list comprehension

**Answer: B**

---

**Q22.** 🟡 What is the output of the following?
```python
a = [1, 2, 3]
b = a
b.append(4)
print(a)
```
- A) `[1, 2, 3]`
- B) `[1, 2, 3, 4]`
- C) `[4]`
- D) `Error`

**Answer: B** — `b = a` copies the reference, not the list. Both point to the same object.

---

**Q23.** 🟡 How do you create a shallow copy of a list in Python?
- A) `b = a`
- B) `b = a.copy()` or `b = a[:]`
- C) `b = deepcopy(a)`
- D) `b = list.clone(a)`

**Answer: B**

---

**Q24.** 🔴 What is the difference between `__str__` and `__repr__` in Python?
- A) They are identical dunder methods
- B) `__str__` is for readable, user-facing string output; `__repr__` is for unambiguous, developer-facing representation (ideally enough to recreate the object)
- C) `__repr__` is called by `print()`; `__str__` is called by `repr()`
- D) `__str__` is for classes only; `__repr__` is for functions

**Answer: B**

---

**Q25.** 🔴 What is the output of the following?
```python
def outer():
    x = 10
    def inner():
        nonlocal x
        x += 5
    inner()
    return x

print(outer())
```
- A) `10`
- B) `15`
- C) `Error`
- D) `5`

**Answer: B** — `nonlocal` allows `inner()` to modify `x` in `outer()`'s scope.

---

**Q26.** 🔴 What is a Python "context manager" and when would you use it?
- A) A tool for managing Python package versions
- B) An object that defines `__enter__` and `__exit__` methods, used with `with` statements to guarantee resource cleanup (e.g., closing files, database connections)
- C) A class for managing thread contexts
- D) A decorator for managing function execution order

**Answer: B**

---

**Q27.** 🔴 What is the Global Interpreter Lock (GIL) in Python?
- A) A security lock on Python scripts
- B) A mutex in CPython that allows only one thread to execute Python bytecode at a time, limiting true parallelism in CPU-bound multithreaded programs
- C) A garbage collection lock
- D) A lock on global variables in a module

**Answer: B**

---

**Q28.** 🔴 What is the output of the following?
```python
x = (i * 2 for i in range(4))
print(next(x))
print(next(x))
```
- A) `0` then `2`
- B) `0` then `2`
- C) `[0, 2, 4, 6]` then Error
- D) `0` then `0`

**Answer: B** (A and B are same — answer is: `0` then `2`) — Generator produces values lazily; `next()` fetches one at a time.

---

**Q29.** 🔴 What is the difference between `multiprocessing` and `threading` in Python?
- A) Threading uses multiple CPUs; multiprocessing uses one CPU with multiple threads
- B) Multiprocessing uses separate OS processes (bypassing GIL, true parallelism for CPU-bound tasks); threading uses OS threads within one process (better for I/O-bound tasks, limited by GIL for CPU-bound)
- C) They are identical in Python
- D) Threading is for network tasks; multiprocessing is for database tasks

**Answer: B**

---

**Q30.** 🔴 What does `@staticmethod` do in a Python class?
- A) Makes a method run only once at class definition
- B) Defines a method that belongs to the class but doesn't receive `self` or `cls` — it has no access to instance or class state and behaves like a regular function namespaced in the class
- C) Makes a method thread-safe
- D) Prevents the method from being overridden in subclasses

**Answer: B**

---

**Q31.** 🟡 In Python, what does `zip([1,2,3], ['a','b','c'])` produce?
- A) `[[1,'a'], [2,'b'], [3,'c']]`
- B) A zip object yielding tuples `(1,'a'), (2,'b'), (3,'c')` when iterated
- C) `{1:'a', 2:'b', 3:'c'}`
- D) `[(1,2,3),('a','b','c')]`

**Answer: B**

---

**Q32.** 🟡 What is the purpose of `if __name__ == "__main__":` in Python?
- A) To mark the entry point for all Python scripts globally
- B) To ensure the enclosed code runs only when the file is executed directly, not when imported as a module
- C) To check if the script is run as an admin
- D) To prevent the script from running in virtual environments

**Answer: B**

---

**Q33.** 🟡 What does `dict.setdefault(key, default)` do?
- A) Sets all dict values to the default
- B) Returns the value for `key` if it exists; otherwise inserts `key` with `default` value and returns `default`
- C) Resets the dict to an empty state
- D) Sets a default type for all future keys

**Answer: B**

---

**Q34.** 🔴 What is the time complexity of looking up a key in a Python dictionary?
- A) O(n)
- B) O(1) average case (uses hash table internally)
- C) O(log n)
- D) O(n²)

**Answer: B**

---

**Q35.** 🔴 What is "monkey patching" in Python?
- A) A debugging technique for finding bugs
- B) Dynamically modifying a class or module at runtime, replacing methods or attributes — useful in testing to mock behavior
- C) A code review strategy
- D) A Python package manager command

**Answer: B**

---
*35 Questions | Shared across all JDs | See JD-specific files for domain MCQs.*
