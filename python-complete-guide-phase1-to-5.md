# Python Complete Refresh Guide — Phase 1 to Phase 5

---

# PHASE 1 — Core Refresh

---

## 1.1 Data Types, Variables, Type Casting

### Variables
A **variable** is a named label pointing to a value in memory. Python is **dynamically typed** — type is determined at runtime, not at declaration time.

```python
x = 10
name = "Alice"
pi = 3.14
is_valid = True
nothing = None
```

### Built-in Data Types

| Type | Example | Description |
|---|---|---|
| `int` | `10`, `-3` | Whole numbers |
| `float` | `3.14` | Decimal numbers |
| `complex` | `2+3j` | Real + imaginary |
| `str` | `"hello"` | Text / sequence of characters |
| `bool` | `True/False` | Boolean |
| `NoneType` | `None` | Absence of value |

```python
print(type(10))        # <class 'int'>
print(type(3.14))      # <class 'float'>
print(type("hello"))   # <class 'str'>
print(type(True))      # <class 'bool'>
print(type(None))      # <class 'NoneType'>
```

### Type Casting
Explicit conversion of one type to another.

```python
int("42")        # 42
float("3.14")    # 3.14
str(100)         # "100"
bool(0)          # False
bool("hello")    # True
list("abc")      # ['a', 'b', 'c']
```

### Falsy Values
These evaluate to `False` in boolean context:
`0`, `0.0`, `""`, `None`, `[]`, `{}`, `()`, `set()`

Everything else is **Truthy**.

---

## 1.2 Strings

### Definition
A **string** is an **immutable**, ordered sequence of characters. Immutable means once created, it cannot be modified — any operation returns a new string.

```python
s = "Hello, World"
print(s[0])    # H      — indexing
print(s[-1])   # d      — negative index (from end)
print(len(s))  # 12     — length
```

### Slicing
**Syntax:** `string[start:stop:step]`

```python
s = "Hello, World"
print(s[0:5])    # Hello
print(s[7:])     # World
print(s[:5])     # Hello
print(s[::2])    # Hlo ol  — every 2nd character
print(s[::-1])   # dlroW ,olleH  — reverse
```

### Common String Methods

```python
s = "  hello world  "

s.upper()                      # "  HELLO WORLD  "
s.lower()                      # "  hello world  "
s.strip()                      # "hello world"
s.lstrip()                     # "hello world  "
s.rstrip()                     # "  hello world"
s.replace("hello", "hi")       # "  hi world  "
s.split(" ")                   # list split by space
"hello".startswith("he")       # True
"hello".endswith("lo")         # True
"hello".find("ll")             # 2  — index, -1 if not found
"hello".count("l")             # 2
"hello".capitalize()           # "Hello"
"hello world".title()          # "Hello World"
" ".join(["a", "b", "c"])      # "a b c"
"hello".isdigit()              # False
"123".isdigit()                # True
"hello".isalpha()              # True
```

### String Formatting

```python
name = "Alice"
age = 30

# f-string (modern, preferred)
print(f"Name: {name}, Age: {age}")

# .format()
print("Name: {}, Age: {}".format(name, age))

# % formatting (old style)
print("Name: %s, Age: %d" % (name, age))
```

**f-string expressions:**
```python
x = 10
print(f"{x * 2}")         # 20
print(f"{x:.2f}")         # 10.00 — 2 decimal places
print(f"{'hello':>10}")   # right align in 10 chars
```

### String Immutability
```python
s = "hello"
s[0] = "H"           # TypeError — strings are immutable
s = "H" + s[1:]      # correct way — creates new string "Hello"
```

---

## 1.3 Lists, Tuples, Sets, Dicts

### Lists
**Definition:** An **ordered, mutable** collection. Allows duplicates. Items can be of any type.

```python
lst = [1, 2, 3, "hello", True]
```

**Indexing & Slicing:**
```python
lst = [10, 20, 30, 40, 50]
lst[0]       # 10
lst[-1]      # 50
lst[1:3]     # [20, 30]
lst[::-1]    # [50, 40, 30, 20, 10]
```

**Common Methods:**
```python
lst = [1, 2, 3]

lst.append(4)           # add to end
lst.insert(1, 99)       # insert at index
lst.extend([5, 6])      # add multiple items
lst.remove(99)          # remove first occurrence
lst.pop()               # remove & return last
lst.pop(0)              # remove & return at index
lst.index(2)            # find index of value
lst.count(1)            # count occurrences
lst.sort()              # sort ascending in-place
lst.sort(reverse=True)  # sort descending
lst.reverse()           # reverse in-place
lst.copy()              # shallow copy
lst.clear()             # empty the list
len(lst)                # length
```

---

### Tuples
**Definition:** An **ordered, immutable** collection. Once created, cannot be changed. Faster than lists.

```python
t = (1, 2, 3)
t = (1,)          # single element — comma is mandatory
t = 1, 2, 3       # parentheses optional
```

```python
t = (10, 20, 30, 20)
t[0]           # 10
t.count(20)    # 2
t.index(30)    # 2
len(t)         # 4
```

**Tuple Unpacking:**
```python
a, b, c = (1, 2, 3)
first, *rest = (1, 2, 3, 4)   # first=1, rest=[2,3,4]
```

**Why use tuples over lists?**
- Immutable = safe from accidental modification
- Faster iteration
- Can be used as dict keys (lists cannot)

---

### Sets
**Definition:** An **unordered, mutable** collection of **unique** elements. No duplicates, no indexing.

```python
s = {1, 2, 3, 3, 2}
print(s)   # {1, 2, 3} — duplicates removed

s = set()  # empty set — NOT {}, that's a dict
```

**Common Methods:**
```python
s = {1, 2, 3}

s.add(4)
s.remove(2)       # raises KeyError if not found
s.discard(99)     # no error if not found
s.pop()           # removes arbitrary element
s.clear()
len(s)
2 in s            # membership test — O(1)
```

**Set Operations:**
```python
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

a | b              # union         — {1,2,3,4,5,6}
a & b              # intersection  — {3, 4}
a - b              # difference    — {1, 2}
a ^ b              # symmetric diff — {1,2,5,6}
a.issubset(b)      # True if a ⊆ b
a.issuperset(b)    # True if a ⊇ b
```

---

### Dictionaries
**Definition:** An **ordered** (Python 3.7+), **mutable** collection of **key-value pairs**. Keys must be unique and immutable.

```python
d = {"name": "Alice", "age": 30}
d = dict(name="Alice", age=30)
```

**Accessing & Modifying:**
```python
d["name"]               # "Alice"
d.get("name")           # "Alice" — safe, returns None if missing
d.get("x", "N/A")       # "N/A" — default value
d["city"] = "NYC"       # add new key
d["age"] = 31           # update existing key
del d["city"]           # delete key
```

**Common Methods:**
```python
d.keys()                  # all keys
d.values()                # all values
d.items()                 # all key-value pairs
d.update({"d": 4})        # merge/update
d.pop("b")                # remove & return value
d.setdefault("x", 0)      # set only if key not exists
d.copy()                  # shallow copy
d.clear()                 # empty dict
"a" in d                  # key membership check
```

**Iterating:**
```python
for key in d:
    print(key)

for key, value in d.items():
    print(key, value)
```

---

## 1.4 Conditionals & Loops

### Conditionals
Control structures that execute code based on whether a condition is `True` or `False`.

```python
x = 10

if x > 0:
    print("positive")
elif x == 0:
    print("zero")
else:
    print("negative")
```

**Ternary (one-liner):**
```python
result = "even" if x % 2 == 0 else "odd"
```

**Logical Operators:**
```python
and   # both must be True
or    # at least one must be True
not   # negates
```

**Comparison Operators:**
```python
==    # equal
!=    # not equal
>     # greater than
<     # less than
>=    # greater or equal
<=    # less or equal
is    # identity — same object in memory
in    # membership
```

**`is` vs `==`:**
```python
a = [1, 2]
b = [1, 2]
a == b    # True  — same value
a is b    # False — different objects in memory
```

---

### `for` Loop
Iterates over any **iterable** (list, string, tuple, range, dict, etc.).

```python
for i in range(5):           # 0, 1, 2, 3, 4
    print(i)

for i in range(2, 10, 2):    # 2, 4, 6, 8
    print(i)

for char in "hello":
    print(char)

# enumerate — get index + value
for i, val in enumerate(["a", "b", "c"]):
    print(i, val)

# zip — iterate multiple iterables together
for a, b in zip([1,2,3], ["x","y","z"]):
    print(a, b)
```

---

### `while` Loop
Repeats a block **as long as** a condition is `True`.

```python
x = 0
while x < 5:
    print(x)
    x += 1

while True:
    user_input = input("Enter q to quit: ")
    if user_input == "q":
        break
```

---

### `break`, `continue`, `pass`

**`break`** — exits the loop entirely
```python
for i in range(10):
    if i == 5:
        break
    print(i)    # prints 0-4
```

**`continue`** — skips current iteration
```python
for i in range(10):
    if i % 2 == 0:
        continue
    print(i)    # prints 1,3,5,7,9
```

**`pass`** — placeholder, does nothing
```python
for i in range(5):
    pass
```

**`else` with loops** — runs when loop completes without `break`
```python
for i in range(5):
    if i == 10:
        break
else:
    print("loop completed normally")
```

---

## 1.5 Functions

### Definition
A **function** is a reusable block of code that performs a specific task. Defined with `def`, called by name.

```python
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

print(greet("Alice"))          # Hello, Alice!
print(greet("Bob", "Hi"))      # Hi, Bob!
```

### Parameters vs Arguments
- **Parameter** — variable in function definition
- **Argument** — actual value passed when calling

### `*args` — Variable Positional Arguments
Accepts any number of positional args, collected into a **tuple**.

```python
def add(*args):
    return sum(args)

add(1, 2)          # args = (1, 2)
add(1, 2, 3, 4)    # args = (1, 2, 3, 4)
```

### `**kwargs` — Variable Keyword Arguments
Accepts any number of keyword args, collected into a **dict**.

```python
def show_info(**kwargs):
    for key, val in kwargs.items():
        print(f"{key}: {val}")

show_info(name="Alice", age=30)
```

### Combining All Parameter Types
**Order must be:** `positional → *args → keyword-only → **kwargs`

```python
def func(a, b, *args, key="default", **kwargs):
    print(a, b, args, key, kwargs)

func(1, 2, 3, 4, key="custom", x=10, y=20)
```

### Unpacking when calling
```python
def add(a, b, c):
    return a + b + c

nums = [1, 2, 3]
add(*nums)            # unpacks list

data = {"a": 1, "b": 2, "c": 3}
add(**data)           # unpacks dict
```

### Return multiple values
```python
def min_max(lst):
    return min(lst), max(lst)    # returns a tuple

lo, hi = min_max([3, 1, 4, 1, 5])
```

---

## 1.6 Scope, `global`, `nonlocal`

### Definition
**Scope** is the region of code where a variable is accessible. Python uses the **LEGB rule**.

### LEGB Rule

| Level | Meaning |
|---|---|
| **L** | Local — inside the current function |
| **E** | Enclosing — inside outer function |
| **G** | Global — module level (top of file) |
| **B** | Built-in — Python's built-in names (`len`, `print`, etc.) |

```python
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
```

### `global`
Declares that a variable inside a function refers to the **module-level** variable.

```python
count = 0

def increment():
    global count
    count += 1

increment()
increment()
print(count)   # 2
```

### `nonlocal`
Used in nested functions to refer to a variable in the **enclosing function's scope**.

```python
def outer():
    count = 0
    
    def inner():
        nonlocal count
        count += 1
    
    inner()
    inner()
    print(count)   # 2

outer()
```

| | `global` | `nonlocal` |
|---|---|---|
| Refers to | Module-level variable | Enclosing function variable |
| Used in | Any function | Nested functions only |

---

---

# PHASE 2 — Intermediate

---

## 2.1 List / Dict / Set Comprehensions

### Definition
**Comprehensions** are concise, readable ways to create lists, dicts, or sets from iterables in a single line.

### List Comprehension
**Syntax:** `[expression for item in iterable if condition]`

```python
# Without comprehension
squares = []
for x in range(10):
    squares.append(x ** 2)

# With comprehension
squares = [x ** 2 for x in range(10)]

# With condition
evens = [x for x in range(20) if x % 2 == 0]

# Nested
matrix = [[i * j for j in range(1, 4)] for i in range(1, 4)]
```

### Dict Comprehension
**Syntax:** `{key: value for item in iterable if condition}`

```python
words = ["apple", "banana", "cherry"]
lengths = {word: len(word) for word in words}
# {'apple': 5, 'banana': 6, 'cherry': 6}

# Swap keys and values
d = {"a": 1, "b": 2}
swapped = {v: k for k, v in d.items()}
# {1: 'a', 2: 'b'}
```

### Set Comprehension
**Syntax:** `{expression for item in iterable}`

```python
nums = [1, 2, 2, 3, 3, 4]
unique_squares = {x ** 2 for x in nums}
# {1, 4, 9, 16}
```

---

## 2.2 Lambda, `map`, `filter`, `zip`

### Lambda
**Definition:** An **anonymous function** defined in a single expression. Used for short, throwaway functions.

**Syntax:** `lambda parameters: expression`

```python
square = lambda x: x ** 2
add = lambda x, y: x + y

print(square(5))     # 25
print(add(3, 4))     # 7
```

### `map`
**Definition:** Applies a function to **every item** of an iterable. Returns a map object (lazy iterator).

```python
nums = [1, 2, 3, 4, 5]

# Using lambda
squared = list(map(lambda x: x ** 2, nums))
# [1, 4, 9, 16, 25]

# Using named function
def double(x):
    return x * 2

doubled = list(map(double, nums))
# [2, 4, 6, 8, 10]
```

### `filter`
**Definition:** Filters items from an iterable based on a function that returns `True` or `False`.

```python
nums = [1, 2, 3, 4, 5, 6]

evens = list(filter(lambda x: x % 2 == 0, nums))
# [2, 4, 6]
```

### `zip`
**Definition:** Combines multiple iterables element-wise into tuples. Stops at the shortest iterable.

```python
names = ["Alice", "Bob", "Charlie"]
scores = [85, 90, 78]

paired = list(zip(names, scores))
# [('Alice', 85), ('Bob', 90), ('Charlie', 78)]

# Unzip
names, scores = zip(*paired)

# zip with dict
d = dict(zip(names, scores))
# {'Alice': 85, 'Bob': 90, 'Charlie': 78}
```

---

## 2.3 File I/O

### Definition
**File I/O** (Input/Output) is how Python reads from and writes to files on disk.

### Opening a File
Always use the `with` statement — it automatically closes the file even if an error occurs.

**Syntax:** `with open(filename, mode) as file:`

| Mode | Meaning |
|---|---|
| `'r'` | Read (default) |
| `'w'` | Write (creates or overwrites) |
| `'a'` | Append (adds to end) |
| `'x'` | Create (fails if file exists) |
| `'b'` | Binary mode (e.g., `'rb'`, `'wb'`) |
| `'+'` | Read + Write (e.g., `'r+'`) |

### Reading Files

```python
# Read entire file as string
with open("file.txt", "r") as f:
    content = f.read()

# Read line by line
with open("file.txt", "r") as f:
    for line in f:
        print(line.strip())

# Read all lines into list
with open("file.txt", "r") as f:
    lines = f.readlines()    # ['line1\n', 'line2\n']

# Read single line
with open("file.txt", "r") as f:
    first_line = f.readline()
```

### Writing Files

```python
# Write (creates or overwrites)
with open("file.txt", "w") as f:
    f.write("Hello, World!\n")
    f.write("Second line\n")

# Append
with open("file.txt", "a") as f:
    f.write("Third line\n")

# Write multiple lines
lines = ["line 1\n", "line 2\n", "line 3\n"]
with open("file.txt", "w") as f:
    f.writelines(lines)
```

### Working with JSON

```python
import json

data = {"name": "Alice", "age": 30}

# Write JSON
with open("data.json", "w") as f:
    json.dump(data, f, indent=4)

# Read JSON
with open("data.json", "r") as f:
    loaded = json.load(f)

# Convert to/from string
json_str = json.dumps(data)          # dict to JSON string
parsed = json.loads(json_str)        # JSON string to dict
```

---

## 2.4 Exception Handling

### Definition
**Exception handling** is a mechanism to catch and respond to runtime errors gracefully instead of crashing.

### Basic Structure

```python
try:
    # code that might raise an error
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
except (ValueError, TypeError) as e:
    print(f"Error: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
else:
    print("No error occurred")    # runs only if no exception
finally:
    print("Always runs")          # runs no matter what
```

### Common Built-in Exceptions

| Exception | Cause |
|---|---|
| `ValueError` | Wrong value type (e.g., `int("abc")`) |
| `TypeError` | Wrong type for operation |
| `KeyError` | Dict key not found |
| `IndexError` | List index out of range |
| `AttributeError` | Object has no such attribute |
| `FileNotFoundError` | File doesn't exist |
| `ZeroDivisionError` | Divide by zero |
| `ImportError` | Module not found |

### Raising Exceptions

```python
def divide(a, b):
    if b == 0:
        raise ValueError("b cannot be zero")
    return a / b
```

### Custom Exceptions

```python
class InsufficientFundsError(Exception):
    def __init__(self, amount, balance):
        self.amount = amount
        self.balance = balance
        super().__init__(f"Cannot withdraw {amount}, balance is {balance}")

def withdraw(balance, amount):
    if amount > balance:
        raise InsufficientFundsError(amount, balance)
    return balance - amount

try:
    withdraw(100, 200)
except InsufficientFundsError as e:
    print(e)
```

---

## 2.5 Modules & Packages

### Definition
- **Module** — a single `.py` file containing functions, classes, and variables
- **Package** — a directory of modules with an `__init__.py` file

### Importing

```python
import math                        # import full module
from math import sqrt, pi          # import specific names
from math import sqrt as sq        # import with alias
import numpy as np                 # common alias pattern

print(math.sqrt(16))    # 4.0
print(sqrt(16))         # 4.0 — directly usable
```

### Useful Standard Library Modules

```python
import os           # file system, paths, env vars
import sys          # Python interpreter info
import math         # math functions
import random       # random numbers
import datetime     # dates and times
import re           # regular expressions
import json         # JSON encoding/decoding
import collections  # advanced data structures
import itertools    # iterator utilities
import functools    # higher-order functions
```

```python
# os examples
os.getcwd()                     # current directory
os.listdir(".")                 # list files
os.path.exists("file.txt")      # check if file exists
os.path.join("folder", "file")  # safe path joining

# random examples
random.randint(1, 10)           # random int
random.choice([1, 2, 3])        # random element
random.shuffle(lst)             # shuffle in-place

# datetime examples
from datetime import datetime, date
now = datetime.now()
today = date.today()
print(now.strftime("%Y-%m-%d %H:%M"))
```

### Creating Your Own Module

**mymodule.py:**
```python
def greet(name):
    return f"Hello, {name}!"

PI = 3.14159
```

**main.py:**
```python
import mymodule
print(mymodule.greet("Alice"))
print(mymodule.PI)
```

### `__name__ == "__main__"`
```python
def main():
    print("Running as main script")

if __name__ == "__main__":
    main()    # only runs if file is executed directly, not imported
```

---

## 2.6 `*args` / `**kwargs` Deep Dive

### Forwarding arguments

```python
def logger(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"Done")
        return result
    return wrapper
```

### Keyword-only arguments
Parameters after `*` must be passed by keyword only.

```python
def func(a, b, *, verbose=False):
    if verbose:
        print(f"a={a}, b={b}")
    return a + b

func(1, 2, verbose=True)
func(1, 2)              # verbose defaults to False
func(1, 2, True)        # TypeError — verbose must be keyword
```

### Positional-only arguments (Python 3.8+)
Parameters before `/` must be passed positionally only.

```python
def func(a, b, /, c):
    return a + b + c

func(1, 2, c=3)         # valid
func(1, 2, 3)           # valid
func(a=1, b=2, c=3)     # TypeError — a,b must be positional
```

---

---

# PHASE 3 — Object-Oriented Programming (OOP)

---

## 3.1 Classes & Objects

### Definition
- **Class** — a blueprint/template for creating objects
- **Object** — an instance of a class
- **OOP** — a programming paradigm that organizes code around objects that have data (attributes) and behavior (methods)

```python
class Dog:
    species = "Canis lupus"      # class attribute — shared by all instances
    
    def __init__(self, name, age):   # constructor
        self.name = name             # instance attribute — unique per object
        self.age = age
    
    def bark(self):
        return f"{self.name} says Woof!"
    
    def __str__(self):
        return f"Dog({self.name}, {self.age})"

# Create instances
d1 = Dog("Rex", 3)
d2 = Dog("Buddy", 5)

print(d1.bark())       # Rex says Woof!
print(d2.name)         # Buddy
print(Dog.species)     # Canis lupus — via class
print(d1.species)      # Canis lupus — via instance
```

### `self`
`self` refers to the current instance. It must be the first parameter of every instance method. Python passes it automatically — you never pass it manually.

### Instance vs Class Attributes

```python
class Counter:
    count = 0    # class attribute — shared

    def __init__(self):
        Counter.count += 1
        self.id = Counter.count   # instance attribute — unique

c1 = Counter()
c2 = Counter()
print(Counter.count)   # 2
print(c1.id)           # 1
print(c2.id)           # 2
```

---

## 3.2 Inheritance

### Definition
**Inheritance** allows a class (child) to acquire the properties and methods of another class (parent). Promotes code reuse.

```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return "Some sound"
    
    def __str__(self):
        return f"{self.__class__.__name__}({self.name})"

class Dog(Animal):
    def speak(self):              # method overriding
        return f"{self.name} says Woof!"

class Cat(Animal):
    def speak(self):
        return f"{self.name} says Meow!"

d = Dog("Rex")
c = Cat("Whiskers")
print(d.speak())   # Rex says Woof!
print(c.speak())   # Whiskers says Meow!
```

### `super()`
**Definition:** Calls a method from the **parent class**. Used to extend (not fully override) parent behavior.

```python
class Animal:
    def __init__(self, name, age):
        self.name = name
        self.age = age

class Dog(Animal):
    def __init__(self, name, age, breed):
        super().__init__(name, age)    # call parent __init__
        self.breed = breed

d = Dog("Rex", 3, "Labrador")
print(d.name, d.age, d.breed)
```

### Multiple Inheritance

```python
class Flyable:
    def fly(self):
        return "I can fly"

class Swimmable:
    def swim(self):
        return "I can swim"

class Duck(Flyable, Swimmable):
    pass

d = Duck()
print(d.fly())    # I can fly
print(d.swim())   # I can swim
```

### MRO — Method Resolution Order
Python uses **C3 linearization** to determine the order in which methods are searched in multiple inheritance.

```python
print(Duck.__mro__)
# (<class 'Duck'>, <class 'Flyable'>, <class 'Swimmable'>, <class 'object'>)
```

---

## 3.3 Dunder (Magic) Methods

### Definition
**Dunder methods** (double underscore) are special methods Python calls automatically in certain situations. They define how objects behave with built-in operations.

```python
class Vector:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):               # str(obj), print(obj)
        return f"Vector({self.x}, {self.y})"
    
    def __repr__(self):              # repr(obj), debugging
        return f"Vector(x={self.x}, y={self.y})"
    
    def __len__(self):               # len(obj)
        return 2
    
    def __add__(self, other):        # obj + other
        return Vector(self.x + other.x, self.y + other.y)
    
    def __eq__(self, other):         # obj == other
        return self.x == other.x and self.y == other.y
    
    def __lt__(self, other):         # obj < other
        return (self.x**2 + self.y**2) < (other.x**2 + other.y**2)
    
    def __getitem__(self, index):    # obj[index]
        return (self.x, self.y)[index]
    
    def __contains__(self, item):   # item in obj
        return item in (self.x, self.y)
    
    def __call__(self, scale):       # obj()
        return Vector(self.x * scale, self.y * scale)

v1 = Vector(1, 2)
v2 = Vector(3, 4)
print(v1)              # Vector(1, 2)
print(v1 + v2)         # Vector(4, 6)
print(v1 == v2)        # False
print(len(v1))         # 2
print(v1[0])           # 1
print(1 in v1)         # True
print(v1(3))           # Vector(3, 6)
```

### Common Dunder Methods

| Method | Triggered by |
|---|---|
| `__init__` | `obj = Class()` |
| `__str__` | `str(obj)`, `print(obj)` |
| `__repr__` | `repr(obj)`, debugging |
| `__len__` | `len(obj)` |
| `__add__` | `obj + other` |
| `__sub__` | `obj - other` |
| `__mul__` | `obj * other` |
| `__eq__` | `obj == other` |
| `__lt__` | `obj < other` |
| `__getitem__` | `obj[key]` |
| `__setitem__` | `obj[key] = val` |
| `__contains__` | `item in obj` |
| `__iter__` | `for item in obj` |
| `__next__` | `next(obj)` |
| `__call__` | `obj()` |
| `__enter__` | `with obj` |
| `__exit__` | end of `with` block |

---

## 3.4 `@property`, `@staticmethod`, `@classmethod`

### `@property`
**Definition:** Allows a method to be accessed like an attribute. Used for computed properties and controlled access.

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius    # _ means "private by convention"
    
    @property
    def radius(self):
        return self._radius
    
    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("Radius cannot be negative")
        self._radius = value
    
    @property
    def area(self):
        import math
        return math.pi * self._radius ** 2

c = Circle(5)
print(c.radius)    # 5  — accessed like attribute
c.radius = 10      # calls setter
print(c.area)      # 314.15...
```

### `@staticmethod`
**Definition:** A method that belongs to the class but doesn't need access to the instance (`self`) or class (`cls`). Pure utility function.

```python
class MathUtils:
    @staticmethod
    def add(a, b):
        return a + b
    
    @staticmethod
    def is_even(n):
        return n % 2 == 0

MathUtils.add(3, 4)        # 7 — called on class
MathUtils.is_even(6)       # True
```

### `@classmethod`
**Definition:** A method that receives the **class** as first argument (`cls`) instead of the instance. Used for alternative constructors.

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    @classmethod
    def from_birth_year(cls, name, birth_year):
        age = 2024 - birth_year
        return cls(name, age)    # creates instance
    
    @classmethod
    def from_string(cls, data):
        name, age = data.split(",")
        return cls(name, int(age))

p1 = Person("Alice", 30)
p2 = Person.from_birth_year("Bob", 1994)
p3 = Person.from_string("Charlie,25")
```

| | `@property` | `@staticmethod` | `@classmethod` |
|---|---|---|---|
| First param | `self` | None | `cls` |
| Access instance | Yes | No | No |
| Access class | Via `self.__class__` | No | Yes |
| Common use | Computed attributes | Utility functions | Alternative constructors |

---

## 3.5 Dataclasses

### Definition
**Dataclasses** (Python 3.7+) auto-generate boilerplate like `__init__`, `__repr__`, `__eq__` for classes that primarily store data.

```python
from dataclasses import dataclass, field

@dataclass
class Point:
    x: float
    y: float
    z: float = 0.0    # default value

p1 = Point(1.0, 2.0)
p2 = Point(1.0, 2.0)
print(p1)             # Point(x=1.0, y=2.0, z=0.0)
print(p1 == p2)       # True — __eq__ auto-generated

@dataclass(order=True)   # enables <, >, <=, >=
class Student:
    name: str
    grade: float
    courses: list = field(default_factory=list)   # mutable default

s = Student("Alice", 95.5)
s.courses.append("Math")
```

---

---

# PHASE 4 — Advanced Python

---

## 4.1 Decorators

### Definition
A **decorator** is a function that takes another function as input, wraps it with additional behavior, and returns the modified function. Uses the `@` syntax.

### Basic Decorator

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before function call")
        result = func(*args, **kwargs)
        print("After function call")
        return result
    return wrapper

@my_decorator
def say_hello(name):
    print(f"Hello, {name}!")

say_hello("Alice")
# Before function call
# Hello, Alice!
# After function call
```

`@my_decorator` is equivalent to `say_hello = my_decorator(say_hello)`

### `functools.wraps`
Preserves the original function's metadata (name, docstring) when decorating.

```python
from functools import wraps

def my_decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper
```

### Decorator with Arguments

```python
def repeat(n):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(n):
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat(3)
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")   # prints 3 times
```

### Practical Decorators

```python
import time
from functools import wraps

# Timer decorator
def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        print(f"{func.__name__} took {time.time() - start:.4f}s")
        return result
    return wrapper

# Memoize / cache decorator
def memoize(func):
    cache = {}
    @wraps(func)
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
        return cache[args]
    return wrapper

@timer
@memoize
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

### Class as Decorator

```python
class Retry:
    def __init__(self, times=3):
        self.times = times
    
    def __call__(self, func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(self.times):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    print(f"Attempt {attempt+1} failed: {e}")
            raise Exception("All retries failed")
        return wrapper

@Retry(times=3)
def risky_operation():
    import random
    if random.random() < 0.7:
        raise ValueError("Random failure")
    return "Success"
```

---

## 4.2 Generators & `yield`

### Definition
A **generator** is a function that uses `yield` to produce a sequence of values **lazily** (one at a time), without storing them all in memory. It returns a **generator object**.

### `yield` vs `return`
- `return` — exits function, returns single value
- `yield` — pauses function, produces a value, resumes on next call

```python
def countdown(n):
    while n > 0:
        yield n
        n -= 1

gen = countdown(5)
print(next(gen))    # 5
print(next(gen))    # 4
print(next(gen))    # 3

for val in countdown(3):
    print(val)   # 3, 2, 1
```

### Why use generators?
- **Memory efficient** — values produced on demand, not stored
- **Infinite sequences** — can generate values forever
- **Pipeline** — chain generators together

```python
# Memory: list stores all million numbers
million_squares = [x**2 for x in range(1_000_000)]

# Memory: generator produces one at a time
million_squares = (x**2 for x in range(1_000_000))  # generator expression
```

### Generator with `send()`

```python
def accumulator():
    total = 0
    while True:
        value = yield total
        total += value

acc = accumulator()
next(acc)          # prime the generator
print(acc.send(10))  # 10
print(acc.send(20))  # 30
print(acc.send(5))   # 35
```

### `yield from`

```python
def chain(*iterables):
    for it in iterables:
        yield from it     # delegates to sub-generator

list(chain([1,2], [3,4], [5,6]))   # [1, 2, 3, 4, 5, 6]
```

---

## 4.3 Iterators & `itertools`

### Iterators vs Iterables

- **Iterable** — object you can loop over (`list`, `str`, `dict`, generator)
- **Iterator** — object with `__iter__()` and `__next__()`. Maintains state, can only move forward.

```python
lst = [1, 2, 3]        # iterable
it = iter(lst)         # create iterator from iterable
print(next(it))        # 1
print(next(it))        # 2
print(next(it))        # 3
print(next(it))        # StopIteration
```

### Custom Iterator

```python
class Range:
    def __init__(self, start, stop):
        self.current = start
        self.stop = stop
    
    def __iter__(self):
        return self
    
    def __next__(self):
        if self.current >= self.stop:
            raise StopIteration
        val = self.current
        self.current += 1
        return val

for x in Range(1, 5):
    print(x)   # 1, 2, 3, 4
```

### `itertools` Module

```python
import itertools

# count — infinite counter
for i in itertools.count(10, 2):   # 10, 12, 14, ...
    if i > 20: break
    print(i)

# cycle — repeat indefinitely
colors = itertools.cycle(["red", "green", "blue"])

# repeat — repeat value n times
list(itertools.repeat(5, 3))       # [5, 5, 5]

# chain — combine iterables
list(itertools.chain([1,2], [3,4]))  # [1, 2, 3, 4]

# combinations
list(itertools.combinations([1,2,3], 2))
# [(1,2), (1,3), (2,3)]

# permutations
list(itertools.permutations([1,2,3], 2))
# [(1,2), (1,3), (2,1), (2,3), (3,1), (3,2)]

# product — cartesian product
list(itertools.product([1,2], ["a","b"]))
# [(1,'a'), (1,'b'), (2,'a'), (2,'b')]

# groupby — group consecutive items
data = [("a", 1), ("a", 2), ("b", 3)]
for key, group in itertools.groupby(data, key=lambda x: x[0]):
    print(key, list(group))

# islice — slice an iterator
list(itertools.islice(range(100), 5, 15, 2))  # [5, 7, 9, 11, 13]
```

---

## 4.4 Context Managers

### Definition
A **context manager** defines setup and cleanup behavior using `with`. Guarantees cleanup runs even if an error occurs.

### `__enter__` and `__exit__`

```python
class ManagedFile:
    def __init__(self, name, mode):
        self.name = name
        self.mode = mode
    
    def __enter__(self):
        self.file = open(self.name, self.mode)
        return self.file       # bound to `as` variable
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.file.close()
        return False           # False = don't suppress exceptions

with ManagedFile("test.txt", "w") as f:
    f.write("Hello!")
```

`__exit__` receives exception info — return `True` to suppress exceptions, `False` to propagate.

### Using `contextlib`

```python
from contextlib import contextmanager

@contextmanager
def managed_file(name, mode):
    f = open(name, mode)
    try:
        yield f          # code in `with` block runs here
    finally:
        f.close()

with managed_file("test.txt", "w") as f:
    f.write("Hello!")
```

### Practical Example — Timer

```python
from contextlib import contextmanager
import time

@contextmanager
def timer():
    start = time.time()
    yield
    print(f"Elapsed: {time.time() - start:.4f}s")

with timer():
    sum(range(1_000_000))
```

---

## 4.5 `collections` Module

### Definition
The `collections` module provides specialized container types beyond `list`, `dict`, `set`, `tuple`.

### `namedtuple`
Tuple with named fields. Immutable, memory-efficient.

```python
from collections import namedtuple

Point = namedtuple("Point", ["x", "y"])
p = Point(1, 2)
print(p.x, p.y)    # 1 2
print(p[0])        # 1 — still indexable
```

### `defaultdict`
Dict that returns a default value for missing keys — no `KeyError`.

```python
from collections import defaultdict

d = defaultdict(int)     # default is 0
d["a"] += 1
d["b"] += 5
print(d["c"])            # 0 — no KeyError

# Grouping
words = ["apple", "ant", "banana", "avocado"]
by_letter = defaultdict(list)
for word in words:
    by_letter[word[0]].append(word)
# {'a': ['apple', 'ant', 'avocado'], 'b': ['banana']}
```

### `Counter`
Dict subclass for counting hashable items.

```python
from collections import Counter

c = Counter("mississippi")
# Counter({'i': 4, 's': 4, 'p': 2, 'm': 1})

c.most_common(2)    # [('i', 4), ('s', 4)]
c["i"]              # 4
c["z"]              # 0 — no KeyError

# Arithmetic
c1 = Counter(a=3, b=2)
c2 = Counter(a=1, b=4)
c1 + c2    # Counter({'b': 6, 'a': 4})
c1 - c2    # Counter({'a': 2})
```

### `deque`
Double-ended queue. O(1) append/pop from both ends (vs O(n) for list).

```python
from collections import deque

d = deque([1, 2, 3])
d.append(4)          # add right
d.appendleft(0)      # add left
d.pop()              # remove right
d.popleft()          # remove left
d.rotate(2)          # rotate right by 2

# Use as sliding window
d = deque(maxlen=3)
for i in range(6):
    d.append(i)
    print(list(d))
# [0] → [0,1] → [0,1,2] → [1,2,3] → [2,3,4] → [3,4,5]
```

### `OrderedDict`
Dict that remembers insertion order (mainly useful for Python < 3.7).

```python
from collections import OrderedDict
od = OrderedDict()
od["b"] = 2
od["a"] = 1
od.move_to_end("b")     # move key to end
```

---

## 4.6 Type Hints & `mypy`

### Definition
**Type hints** (Python 3.5+) annotate variables and function signatures with expected types. They don't enforce types at runtime but enable static analysis tools like `mypy`.

### Basic Type Hints

```python
name: str = "Alice"
age: int = 30
score: float = 95.5
active: bool = True
```

### Function Annotations

```python
def greet(name: str, times: int = 1) -> str:
    return f"Hello, {name}! " * times

def process(data: list) -> None:
    pass
```

### `typing` Module (Python 3.5-3.8)

```python
from typing import List, Dict, Tuple, Set, Optional, Union, Any, Callable

def func(names: List[str]) -> Dict[str, int]:
    return {name: len(name) for name in names}

# Optional — value can be the type or None
def find(name: str) -> Optional[str]:
    return name if name else None

# Union — multiple allowed types
def process(value: Union[int, str]) -> str:
    return str(value)
```

### Modern Type Hints (Python 3.10+)

```python
# Use built-in types directly (no need to import from typing)
def func(names: list[str]) -> dict[str, int]:
    return {name: len(name) for name in names}

# Union using | operator
def process(value: int | str) -> str:
    return str(value)

# Optional using | None
def find(name: str) -> str | None:
    return name if name else None
```

### Running `mypy`

```bash
pip install mypy
mypy your_file.py
```

---

---

# PHASE 5 — Ecosystem & Tooling

---

## 5.1 Virtual Environments (`venv`)

### Definition
A **virtual environment** is an isolated Python environment with its own packages. Prevents dependency conflicts between projects.

```bash
# Create virtual environment
python -m venv venv

# Activate
source venv/bin/activate          # Linux/Mac
venv\Scripts\activate             # Windows

# Deactivate
deactivate

# Install packages
pip install requests numpy pandas

# Save dependencies
pip freeze > requirements.txt

# Install from requirements
pip install -r requirements.txt
```

### `pyproject.toml` (modern standard)

```toml
[build-system]
requires = ["setuptools", "wheel"]
build-backend = "setuptools.backends.legacy:build"

[project]
name = "myproject"
version = "0.1.0"
dependencies = [
    "requests>=2.28.0",
    "numpy>=1.24.0",
]

[project.optional-dependencies]
dev = ["pytest", "mypy", "black"]
```

---

## 5.2 Testing with `pytest`

### Definition
**pytest** is the most popular Python testing framework. Tests are functions starting with `test_`.

```python
# math_utils.py
def add(a, b):
    return a + b

def divide(a, b):
    if b == 0:
        raise ZeroDivisionError("Cannot divide by zero")
    return a / b
```

```python
# test_math_utils.py
import pytest
from math_utils import add, divide

def test_add():
    assert add(1, 2) == 3
    assert add(-1, 1) == 0

def test_divide():
    assert divide(10, 2) == 5.0

def test_divide_by_zero():
    with pytest.raises(ZeroDivisionError):
        divide(10, 0)

def test_add_floats():
    assert add(0.1, 0.2) == pytest.approx(0.3)   # float comparison
```

```bash
pytest                    # run all tests
pytest -v                 # verbose
pytest test_file.py       # specific file
pytest -k "test_add"      # run matching tests
pytest --tb=short         # shorter traceback
```

### Fixtures
**Fixtures** provide reusable setup/teardown logic.

```python
import pytest

@pytest.fixture
def sample_list():
    return [1, 2, 3, 4, 5]

def test_sum(sample_list):
    assert sum(sample_list) == 15

def test_length(sample_list):
    assert len(sample_list) == 5

# Fixture with teardown
@pytest.fixture
def temp_file(tmp_path):
    file = tmp_path / "test.txt"
    file.write_text("hello")
    yield file
    # teardown code here (optional)
```

### Parametrize

```python
@pytest.mark.parametrize("a, b, expected", [
    (1, 2, 3),
    (-1, 1, 0),
    (0, 0, 0),
])
def test_add(a, b, expected):
    assert add(a, b) == expected
```

---

## 5.3 `asyncio` Basics

### Definition
**asyncio** is Python's built-in library for **asynchronous programming**. Allows running tasks concurrently without threads, using an event loop.

### Key Concepts

| Term | Meaning |
|---|---|
| `async def` | Defines a coroutine (async function) |
| `await` | Pause current coroutine, let others run |
| `asyncio.run()` | Runs the main coroutine |
| `asyncio.gather()` | Run multiple coroutines concurrently |
| `asyncio.sleep()` | Async sleep (non-blocking) |

### Basic Example

```python
import asyncio

async def fetch_data(name, delay):
    print(f"Start fetching {name}")
    await asyncio.sleep(delay)      # simulate async I/O
    print(f"Done fetching {name}")
    return f"{name} data"

async def main():
    # Sequential — takes 3 seconds
    result1 = await fetch_data("A", 2)
    result2 = await fetch_data("B", 1)

    # Concurrent — takes 2 seconds (max delay)
    results = await asyncio.gather(
        fetch_data("A", 2),
        fetch_data("B", 1),
        fetch_data("C", 3),
    )
    print(results)

asyncio.run(main())
```

### `asyncio.create_task()`

```python
async def main():
    task1 = asyncio.create_task(fetch_data("A", 2))
    task2 = asyncio.create_task(fetch_data("B", 1))
    
    result1 = await task1
    result2 = await task2
```

### When to use `asyncio`
- **I/O-bound** tasks: HTTP requests, file I/O, database queries
- **Not** for CPU-bound tasks (use `multiprocessing` instead)

---

## 5.4 Popular Libraries Overview

### `requests` — HTTP

```python
import requests

response = requests.get("https://api.example.com/data")
data = response.json()
print(response.status_code)   # 200

response = requests.post("https://api.example.com/users",
    json={"name": "Alice"},
    headers={"Authorization": "Bearer token"})
```

### `numpy` — Numerical Computing

```python
import numpy as np

arr = np.array([1, 2, 3, 4, 5])
matrix = np.zeros((3, 3))
arr * 2               # [2, 4, 6, 8, 10]
np.mean(arr)          # 3.0
np.dot(arr, arr)      # dot product
```

### `pandas` — Data Analysis

```python
import pandas as pd

df = pd.read_csv("data.csv")
df.head()
df.describe()
df["column"].mean()
df[df["age"] > 25]
df.groupby("city")["salary"].mean()
```

### `pathlib` — File Paths (modern)

```python
from pathlib import Path

p = Path("folder/file.txt")
p.exists()
p.read_text()
p.write_text("hello")
p.parent        # folder
p.stem          # file
p.suffix        # .txt
list(Path(".").glob("*.py"))
```

---

# Quick Reference Summary

## Phase 1 — Core
| Topic | Key Takeaway |
|---|---|
| Data Types | Python infers types; use `type()` to check |
| Strings | Immutable, rich methods, f-strings preferred |
| Lists | Ordered, mutable, allows duplicates |
| Tuples | Ordered, immutable, faster than lists |
| Sets | Unordered, unique elements, great for set math |
| Dicts | Key-value store, keys must be unique & immutable |
| Conditionals | `if/elif/else`, ternary, logical operators |
| Loops | `for` iterates iterables, `while` uses condition |
| Functions | `def`, return, `*args`, `**kwargs`, defaults |
| Scope | LEGB rule, `global` for module vars, `nonlocal` for enclosing |

## Phase 2 — Intermediate
| Topic | Key Takeaway |
|---|---|
| Comprehensions | Concise one-liners for list/dict/set creation |
| Lambda | Anonymous function for short throwaway use |
| map/filter | Apply function or filter over iterables |
| File I/O | Always use `with open()`, prefer JSON for structured data |
| Exceptions | `try/except/finally`, raise custom exceptions |
| Modules | `import`, standard library, `__name__ == "__main__"` |

## Phase 3 — OOP
| Topic | Key Takeaway |
|---|---|
| Classes | Blueprint with `__init__`, `self` is instance reference |
| Inheritance | `super()` extends parent, MRO resolves order |
| Dunder methods | Customize built-in behavior (`+`, `len`, `str`, etc.) |
| `@property` | Controlled attribute access with getter/setter |
| `@classmethod` | Alternative constructors, receives `cls` |
| `@staticmethod` | Utility methods, no `self` or `cls` |
| Dataclasses | Auto-generate boilerplate for data-holding classes |

## Phase 4 — Advanced
| Topic | Key Takeaway |
|---|---|
| Decorators | Wrap functions with extra behavior using `@` |
| Generators | `yield` for lazy, memory-efficient sequences |
| Iterators | `__iter__` + `__next__`, stateful, forward-only |
| Context Managers | `with` for safe setup/teardown (`__enter__`/`__exit__`) |
| `collections` | `Counter`, `defaultdict`, `deque`, `namedtuple` |
| Type Hints | Annotate code for readability and static analysis |

## Phase 5 — Ecosystem
| Topic | Key Takeaway |
|---|---|
| `venv` | Always use virtual environments per project |
| `pytest` | `test_` functions, fixtures, parametrize |
| `asyncio` | Concurrent I/O without threads, use `async/await` |
| Libraries | `requests`, `numpy`, `pandas`, `pathlib` |
