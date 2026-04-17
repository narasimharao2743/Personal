# Python + AI/ML Interview Preparation Guide
**Target:** Prowesssoft (API Integration, Kafka, Cloud) + General Python roles  
**Profile:** 1.5 years exp — Kafka, PyFlink, ClickHouse, Redis, Protobuf, Flask  
**Goal:** Brush up Python basics → advanced + AI/ML fundamentals

---

## STUDY PLAN (4 Weeks)

| Week | Focus |
|------|-------|
| Week 1 | Python Basics + OOP |
| Week 2 | Intermediate Python + APIs |
| Week 3 | Advanced Python + Concurrency |
| Week 4 | AI/ML Basics + Mock Interview |

---

# PART 1: PYTHON

---

## WEEK 1 — BASICS + OOP

### 1.1 Data Types

**Definition:**  
A data type defines what kind of value a variable holds and what operations can be performed on it. Python is dynamically typed — you don't declare types, Python figures it out at runtime.

```python
x = 10          # int   — whole numbers
y = 3.14        # float — decimal numbers
name = "Nara"   # str   — text
flag = True     # bool  — True or False
nothing = None  # NoneType — absence of value

# Type casting — converting one type to another
int("42")       # 42
float("3.14")   # 3.14
str(100)        # "100"
bool(0)         # False
bool("")        # False
bool([])        # False — empty containers are falsy
bool(1)         # True
bool("hello")   # True
```

**Falsy values in Python:** `0`, `0.0`, `""`, `[]`, `{}`, `set()`, `None`, `False`  
Everything else is truthy.

**Key interview points:**
- `is` checks identity (same object in memory), `==` checks value equality
- Strings are immutable — every modification creates a new string object
- `None` is a singleton — always compare with `is None`, not `== None`

---

### 1.2 Strings

**Definition:**  
A string is an immutable sequence of Unicode characters. Once created, you cannot change individual characters — you can only create new strings.

```python
s = "  Hello World  "

# Common methods
s.strip()            # "Hello World"         — removes whitespace from both ends
s.lstrip()           # "Hello World  "       — removes from left
s.rstrip()           # "  Hello World"       — removes from right
s.lower()            # "  hello world  "
s.upper()            # "  HELLO WORLD  "
s.replace("o", "0")  # "  Hell0 W0rld  "    — replaces all occurrences
s.split(" ")         # splits by space into list
s.startswith("  H")  # True
s.endswith("  ")     # True
s.find("World")      # 8  — returns index, -1 if not found
s.count("l")         # 3  — count occurrences
",".join(["a","b","c"])  # "a,b,c"

# Slicing  — string[start:stop:step]
s = "Hello"
s[0]      # "H"
s[-1]     # "o"   — last character
s[1:4]    # "ell"
s[::-1]   # "olleH"  — reverse

# f-strings (Python 3.6+) — best way to format strings
name = "Nara"
age = 25
f"Name: {name}, Age: {age}"   # "Name: Nara, Age: 25"
f"{3.14159:.2f}"               # "3.14"  — 2 decimal places
f"{1000000:,}"                 # "1,000,000" — thousands separator
```

---

### 1.3 Collections

**Definition:**  
Collections are data structures that store multiple values. Python has 4 built-in collection types.

#### List
**Definition:** An ordered, mutable sequence that allows duplicate elements. Most versatile collection in Python.

```python
fruits = ["apple", "banana", "apple"]
fruits.append("mango")       # add to end
fruits.insert(1, "cherry")   # insert at index
fruits.remove("apple")       # removes first occurrence
fruits.pop()                 # removes and returns last element
fruits.pop(0)                # removes and returns element at index 0
fruits[0]                    # access by index
fruits[-1]                   # last element
fruits[1:3]                  # slicing — returns new list
fruits.sort()                # in-place sort
fruits.sort(reverse=True)    # descending
sorted(fruits)               # returns new sorted list
fruits.reverse()             # in-place reverse
len(fruits)                  # length
"apple" in fruits            # membership check O(n)
fruits.index("banana")       # find index
fruits.count("apple")        # count occurrences
fruits.extend(["kiwi","pear"]) # add multiple items
fruits.copy()                # shallow copy
```

#### Tuple
**Definition:** An ordered, immutable sequence. Once created, cannot be modified. Faster than list, can be used as dictionary keys.

```python
point = (10, 20)
x, y = point          # unpacking
a, *rest = (1,2,3,4)  # a=1, rest=[2,3,4]
point[0]              # 10
len(point)            # 2
(10,) + (20,)         # (10, 20) — concatenation
10 in point           # True

# Single element tuple needs trailing comma
single = (42,)        # tuple
not_tuple = (42)      # just int 42
```

#### Set
**Definition:** An unordered collection of unique elements. Uses hash table internally — membership check is O(1). No indexing.

```python
s = {1, 2, 3, 2, 1}   # {1, 2, 3} — duplicates removed
s.add(4)
s.discard(1)           # remove if exists, no error
s.remove(1)            # remove, raises KeyError if not found
1 in s                 # O(1) membership check

s1 = {1, 2, 3}
s2 = {2, 3, 4}
s1 | s2    # {1,2,3,4}   — union
s1 & s2    # {2,3}       — intersection
s1 - s2    # {1}         — difference (in s1 but not s2)
s1 ^ s2    # {1,4}       — symmetric difference
```

#### Dictionary
**Definition:** An unordered collection of key-value pairs. Keys must be unique and hashable (immutable). Lookup, insert, delete are all O(1) average.

```python
person = {"name": "Nara", "age": 25}
person["name"]             # "Nara"
person.get("city")         # None  — safe, no KeyError
person.get("city", "N/A")  # "N/A" — with default
person["city"] = "Hyd"     # add or update
del person["age"]          # delete key
"name" in person           # True  — check key existence
person.keys()              # dict_keys
person.values()            # dict_values
person.items()             # dict_items — [(key,val),...]
person.update({"age": 26, "email": "x@y.com"})  # merge
person.pop("age", None)    # remove and return, None if missing
person.copy()              # shallow copy
```

#### Comprehensions
**Definition:** A concise, readable way to create collections using a single expression. Faster than a for loop.

```python
# List comprehension
squares  = [x**2 for x in range(10)]
even_sq  = [x**2 for x in range(10) if x % 2 == 0]
flat     = [x for sub in [[1,2],[3,4]] for x in sub]  # flatten

# Dict comprehension
sq_dict  = {x: x**2 for x in range(5)}

# Set comprehension
sq_set   = {x**2 for x in range(5)}

# Generator expression (lazy — not a comprehension but same syntax)
sq_gen   = (x**2 for x in range(5))   # doesn't compute until needed
```

**When to use which collection:**
| Structure | Use when |
|-----------|----------|
| List | Ordered data, duplicates needed, need indexing |
| Tuple | Fixed data, dict keys, function return multiple values |
| Set | Remove duplicates, fast membership test |
| Dict | Key-value lookup, counting, grouping |

---

### 1.4 Functions

**Definition:**  
A function is a reusable block of code that performs a specific task. In Python, functions are first-class objects — they can be passed as arguments, returned from other functions, and assigned to variables.

```python
# Basic function
def greet(name, greeting="Hello"):   # greeting has default value
    return f"{greeting}, {name}"

greet("Nara")           # "Hello, Nara"
greet("Nara", "Hi")     # "Hi, Nara"
greet(greeting="Hey", name="Nara")  # keyword arguments — order doesn't matter

# *args — collects extra positional arguments into a tuple
def add(*args):
    return sum(args)
add(1, 2, 3, 4)    # 10

# **kwargs — collects extra keyword arguments into a dict
def info(**kwargs):
    for k, v in kwargs.items():
        print(f"{k}: {v}")
info(name="Nara", age=25, city="Hyd")

# Combining all
def func(a, b, *args, keyword_only, **kwargs):
    pass

# Lambda — anonymous function, single expression only
square = lambda x: x**2
square(5)   # 25

add = lambda x, y: x + y
add(3, 4)   # 7

# Passing functions as arguments
nums = [3, 1, 4, 1, 5]
list(map(lambda x: x*2, nums))        # [6,2,8,2,10]  — apply to each
list(filter(lambda x: x > 2, nums))   # [3,4,5]       — keep if True
sorted(nums, key=lambda x: -x)        # [5,4,3,1,1]   — sort by custom key
```

---

### 1.5 OOP (Object Oriented Programming)

**Definition:**  
OOP is a programming paradigm that organizes code into objects — instances of classes that bundle data (attributes) and behavior (methods) together. It makes code more modular, reusable, and easier to maintain.

**4 Pillars:**

| Pillar | Definition |
|--------|------------|
| **Encapsulation** | Bundling data and methods together, hiding internal details. Controls what is accessible from outside. |
| **Inheritance** | A child class acquires properties and methods of a parent class. Promotes code reuse. |
| **Polymorphism** | Same method name behaves differently in different classes. "Many forms." |
| **Abstraction** | Hiding complex implementation, exposing only necessary interface. |

```python
class Animal:
    species = "Animal"          # class variable — shared by all instances

    def __init__(self, name, age):
        self.name = name        # instance variable — unique per object
        self.age = age

    def speak(self):
        return f"{self.name} makes a sound"

    # Dunder (magic) methods — called by Python internally
    def __str__(self):          # called by print() and str()
        return f"Animal({self.name}, {self.age})"

    def __repr__(self):         # called in REPL, should be unambiguous
        return f"Animal(name={self.name!r}, age={self.age!r})"

    def __eq__(self, other):    # called by ==
        return self.name == other.name and self.age == other.age

    def __len__(self):          # called by len()
        return self.age

    @classmethod
    def create(cls, name):      # receives class as first arg, not instance
        return cls(name, 0)     # can create instances

    @staticmethod
    def is_valid_age(age):      # no access to class or instance
        return age >= 0         # utility function, logically belongs here


# Inheritance
class Dog(Animal):
    def __init__(self, name, age, breed):
        super().__init__(name, age)   # call parent __init__
        self.breed = breed

    def speak(self):            # Overriding parent method — polymorphism
        return f"{self.name} barks"


# Multiple Inheritance
class A:
    def hello(self):
        return "A"

class B(A):
    def hello(self):
        return "B"

class C(A):
    def hello(self):
        return "C"

class D(B, C):    # MRO: D → B → C → A
    pass

D().hello()   # "B"  — follows Method Resolution Order (MRO)
D.__mro__     # shows the lookup order


# Encapsulation — access modifiers
class BankAccount:
    def __init__(self, balance):
        self._balance = balance    # _single  = convention for "protected"
        self.__secret = "hidden"   # __double = name mangling, truly private

    @property
    def balance(self):             # getter
        return self._balance

    @balance.setter
    def balance(self, value):      # setter with validation
        if value < 0:
            raise ValueError("Balance cannot be negative")
        self._balance = value

acc = BankAccount(1000)
acc.balance          # 1000   — uses getter
acc.balance = 2000   # uses setter
acc._BankAccount__secret   # "hidden" — can still access but shouldn't


# Abstraction — Abstract Base Class
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass              # subclass MUST implement this

    @abstractmethod
    def perimeter(self):
        pass

class Circle(Shape):
    def __init__(self, r):
        self.r = r
    def area(self):
        return 3.14 * self.r ** 2
    def perimeter(self):
        return 2 * 3.14 * self.r

# Shape()   # TypeError — cannot instantiate abstract class
Circle(5).area()   # 78.5
```

---

### 1.6 Error Handling

**Definition:**  
Error handling is the process of responding to and recovering from errors in a program. Python uses exceptions — objects that represent errors — and try/except blocks to catch them.

```python
# Basic structure
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")          # catches this specific error
except (TypeError, ValueError) as e:
    print(f"Type or Value error: {e}")   # catch multiple
except Exception as e:
    print(f"Any other error: {e}")  # catch-all (use sparingly)
else:
    print("No error occurred")    # runs ONLY if no exception was raised
finally:
    print("Always runs")          # cleanup code — always executes

# Raising exceptions manually
def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b

# Custom exception
class InsufficientFundsError(Exception):
    def __init__(self, amount, balance):
        self.amount = amount
        self.balance = balance
        super().__init__(f"Need ₹{amount} but only ₹{balance} available")

def withdraw(balance, amount):
    if amount > balance:
        raise InsufficientFundsError(amount, balance)
    return balance - amount

try:
    withdraw(1000, 5000)
except InsufficientFundsError as e:
    print(e)

# Common built-in exceptions
# TypeError       — wrong type: int + str
# ValueError      — right type, wrong value: int("abc")
# KeyError        — dict key doesn't exist
# IndexError      — list index out of range
# AttributeError  — object has no such attribute
# FileNotFoundError — file doesn't exist
# ImportError     — module not found
# ZeroDivisionError — divide by zero
```

---

## WEEK 2 — INTERMEDIATE + APIs

### 2.1 Decorators

**Definition:**  
A decorator is a function that takes another function as input, adds some functionality, and returns the modified function — without changing the original function's source code. It follows the wrapper pattern.

```python
import functools

# Step by step — what a decorator really is
def my_decorator(func):              # 1. takes a function
    @functools.wraps(func)           # preserves original function name/docstring
    def wrapper(*args, **kwargs):    # 2. defines wrapper
        print("Before calling")
        result = func(*args, **kwargs)  # 3. calls original
        print("After calling")
        return result
    return wrapper                   # 4. returns wrapper

# Using @ syntax (syntactic sugar)
@my_decorator
def say_hello(name):
    print(f"Hello {name}")

# Equivalent to: say_hello = my_decorator(say_hello)
say_hello("Nara")
# Before calling
# Hello Nara
# After calling


# Decorator with arguments
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
def hi():
    print("Hi")
hi()   # prints Hi 3 times


# Practical decorators you'll see in real code
import time

def timer(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        elapsed = time.time() - start
        print(f"{func.__name__} took {elapsed:.4f}s")
        return result
    return wrapper

def retry(max_attempts=3):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_attempts):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if attempt == max_attempts - 1:
                        raise
                    print(f"Attempt {attempt+1} failed: {e}, retrying...")
        return wrapper
    return decorator

def log(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__} with args={args}, kwargs={kwargs}")
        result = func(*args, **kwargs)
        print(f"{func.__name__} returned {result}")
        return result
    return wrapper
```

---

### 2.2 Generators & Iterators

**Definition — Iterator:**  
An iterator is any object that implements `__iter__()` and `__next__()`. It remembers state between calls and produces one value at a time. All iterators are iterables, but not all iterables are iterators.

**Definition — Generator:**  
A generator is a special function that uses `yield` instead of `return`. When called, it returns a generator object (which is an iterator). Execution pauses at each `yield` and resumes from there on next call. Memory efficient — computes values on demand.

```python
# Iterator manually
class CountUp:
    def __init__(self, limit):
        self.limit = limit
        self.current = 0

    def __iter__(self):       # makes it iterable
        return self           # returns itself as the iterator

    def __next__(self):       # produces next value
        if self.current >= self.limit:
            raise StopIteration   # signals end
        self.current += 1
        return self.current

for n in CountUp(3):
    print(n)    # 1  2  3


# Generator — same thing, much simpler
def count_up(limit):
    for i in range(1, limit + 1):
        yield i       # pauses here, sends value, resumes on next()

gen = count_up(3)
next(gen)    # 1
next(gen)    # 2
next(gen)    # 3
next(gen)    # StopIteration

# Generator with send()
def accumulator():
    total = 0
    while True:
        value = yield total    # yield sends total out, receives value in
        total += value

acc = accumulator()
next(acc)         # 0  — prime the generator
acc.send(10)      # 10
acc.send(20)      # 30

# Generator expression — lazy version of list comprehension
big_squares = (x**2 for x in range(1_000_000))  # uses ~100 bytes
next(big_squares)   # 0  — computed one at a time

# Real use case — reading large files without loading all in memory
def read_large_file(path):
    with open(path) as f:
        for line in f:
            yield line.strip()

for line in read_large_file("huge.log"):
    process(line)
```

**Generator vs List:**
| | List | Generator |
|--|------|-----------|
| Memory | All values in RAM at once | One value at a time |
| Speed | Faster if reusing values | Faster for single pass |
| Reusable | Yes | No — exhausted after one pass |
| Use when | Small data, need index | Large data, streaming |

---

### 2.3 Context Managers

**Definition:**  
A context manager is an object that defines setup and teardown behavior using `__enter__` and `__exit__` methods. The `with` statement calls these automatically, guaranteeing cleanup even if an exception occurs.

```python
# Why context managers exist
# BAD — file may not close if exception occurs
f = open("file.txt")
data = f.read()
f.close()

# GOOD — file always closes, even on exception
with open("file.txt") as f:
    data = f.read()

# Creating custom context manager — class-based
class DBConnection:
    def __enter__(self):
        print("Opening DB connection")
        self.conn = create_connection()
        return self.conn           # available as 'as' variable

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Closing DB connection")
        self.conn.close()
        return False               # False = don't suppress exceptions
                                   # True  = suppress exceptions

with DBConnection() as conn:
    conn.execute("SELECT 1")

# Creating custom context manager — function-based (simpler)
from contextlib import contextmanager

@contextmanager
def timer():
    import time
    start = time.time()
    yield                          # everything before yield = __enter__
    print(f"Elapsed: {time.time()-start:.4f}s")   # after yield = __exit__

with timer():
    sum(range(1_000_000))

# Multiple context managers in one line
with open("input.txt") as fin, open("output.txt", "w") as fout:
    fout.write(fin.read())
```

---

### 2.4 Collections Module

**Definition:**  
The `collections` module provides specialized container datatypes that are alternatives to Python's built-in dict, list, set, and tuple.

```python
from collections import defaultdict, Counter, deque, namedtuple, OrderedDict

# defaultdict — dict that never raises KeyError for missing keys
# Automatically creates a default value using the factory function

dd = defaultdict(list)
dd["fruits"].append("apple")    # works without initializing "fruits" key
dd["fruits"].append("mango")
# {"fruits": ["apple", "mango"]}

dd = defaultdict(int)
for char in "banana":
    dd[char] += 1   # word frequency counter
# {"b":1, "a":3, "n":2}


# Counter — count elements in a sequence
c = Counter("banana")              # Counter({'a':3,'n':2,'b':1})
c = Counter([1,1,2,3,1,2])        # Counter({1:3, 2:2, 3:1})
c.most_common(2)                   # [('a',3), ('n',2)]
c["a"]                             # 3
c["z"]                             # 0 — no KeyError
c1 = Counter("abc")
c2 = Counter("bcd")
c1 + c2                            # add counts
c1 - c2                            # subtract counts


# deque — double-ended queue, O(1) append/pop from both ends
# list.insert(0, x) is O(n), deque.appendleft(x) is O(1)

dq = deque([1, 2, 3])
dq.append(4)           # [1,2,3,4]
dq.appendleft(0)       # [0,1,2,3,4]
dq.pop()               # 4
dq.popleft()           # 0
dq.rotate(1)           # rotate right: [3,1,2]
dq.rotate(-1)          # rotate left
deque(maxlen=3)        # fixed-size — old items dropped automatically


# namedtuple — tuple with named fields, like a lightweight class
Point = namedtuple("Point", ["x", "y"])
p = Point(10, 20)
p.x        # 10   — attribute access
p[0]       # 10   — index access still works
p._asdict()  # {"x": 10, "y": 20}

Employee = namedtuple("Employee", ["name", "dept", "salary"])
emp = Employee("Nara", "IT", 50000)
```

---

### 2.5 REST APIs with Flask

**Definition — REST:**  
REST (Representational State Transfer) is an architectural style for designing web APIs. It uses HTTP methods (GET, POST, PUT, DELETE) to perform CRUD operations on resources identified by URLs.

**Definition — Flask:**  
Flask is a lightweight Python web framework for building web applications and REST APIs. It's minimal by design — you add only what you need.

```python
from flask import Flask, request, jsonify, abort

app = Flask(__name__)

users = {}    # in-memory store (use DB in production)

# GET all users
@app.route("/users", methods=["GET"])
def get_users():
    return jsonify(list(users.values())), 200

# GET single user
@app.route("/users/<int:user_id>", methods=["GET"])
def get_user(user_id):
    user = users.get(user_id)
    if not user:
        return jsonify({"error": "User not found"}), 404
    return jsonify(user), 200

# POST — create new user
@app.route("/users", methods=["POST"])
def create_user():
    data = request.get_json()
    if not data or "name" not in data:
        return jsonify({"error": "name is required"}), 400
    uid = len(users) + 1
    users[uid] = {"id": uid, "name": data["name"], "email": data.get("email")}
    return jsonify(users[uid]), 201

# PUT — update existing user
@app.route("/users/<int:user_id>", methods=["PUT"])
def update_user(user_id):
    if user_id not in users:
        return jsonify({"error": "User not found"}), 404
    data = request.get_json()
    users[user_id].update(data)
    return jsonify(users[user_id]), 200

# DELETE — remove user
@app.route("/users/<int:user_id>", methods=["DELETE"])
def delete_user(user_id):
    if user_id not in users:
        return jsonify({"error": "User not found"}), 404
    del users[user_id]
    return "", 204

# Query parameters
@app.route("/search", methods=["GET"])
def search():
    query = request.args.get("q", "")         # /search?q=nara
    page  = request.args.get("page", 1, type=int)
    return jsonify({"query": query, "page": page})

# Error handlers
@app.errorhandler(404)
def not_found(e):
    return jsonify({"error": "Not found"}), 404

@app.errorhandler(500)
def server_error(e):
    return jsonify({"error": "Internal server error"}), 500

if __name__ == "__main__":
    app.run(debug=True, port=5000)
```

**HTTP Methods:**
| Method | Purpose | Body | Idempotent |
|--------|---------|------|-----------|
| GET | Read resource | No | Yes |
| POST | Create resource | Yes | No |
| PUT | Full update | Yes | Yes |
| PATCH | Partial update | Yes | Yes |
| DELETE | Delete resource | No | Yes |

**HTTP Status Codes:**
| Code | Meaning |
|------|---------|
| 200 | OK |
| 201 | Created |
| 204 | No Content |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 409 | Conflict |
| 422 | Unprocessable Entity |
| 500 | Internal Server Error |

---

### 2.6 File Handling + JSON

**Definition:**  
File handling is reading from and writing to files on disk. Python provides built-in `open()` for this. JSON (JavaScript Object Notation) is a lightweight data format used heavily in APIs and config files.

```python
# File modes
# "r"  — read (default)
# "w"  — write (overwrites existing)
# "a"  — append
# "rb" — read binary
# "wb" — write binary

# Always use 'with' — auto-closes file
with open("output.txt", "w") as f:
    f.write("Hello\n")
    f.write("World\n")

with open("output.txt", "r") as f:
    content = f.read()        # entire file as string
    lines = f.readlines()     # list of lines
    for line in f:            # iterate line by line (memory efficient)
        print(line.strip())

# JSON
import json

data = {"name": "Nara", "skills": ["Python", "Kafka"], "age": 25}

# Write to file
with open("data.json", "w") as f:
    json.dump(data, f, indent=2)

# Read from file
with open("data.json") as f:
    loaded = json.load(f)

# Convert between dict and string
json_string = json.dumps(data)          # dict → JSON string
parsed_dict = json.loads(json_string)   # JSON string → dict

# CSV
import csv
with open("employees.csv", "w", newline="") as f:
    writer = csv.DictWriter(f, fieldnames=["name","age","dept"])
    writer.writeheader()
    writer.writerow({"name":"Nara","age":25,"dept":"IT"})

with open("employees.csv") as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row)
```

---

## WEEK 3 — ADVANCED PYTHON

### 3.1 Concurrency

**Definition — Threading:**  
Threading allows multiple threads (lightweight sub-processes) to run within the same process, sharing the same memory. Best for I/O-bound tasks (waiting for network, file, database).

**Definition — Multiprocessing:**  
Multiprocessing spawns multiple processes, each with its own memory and Python interpreter. Best for CPU-bound tasks (heavy computation, data processing).

**Definition — Asyncio:**  
Asyncio is Python's framework for asynchronous programming using a single thread with an event loop. It switches between tasks when one is waiting for I/O — very efficient for handling thousands of concurrent I/O operations.

```python
# Threading — I/O bound (API calls, file operations)
import threading
import time

def download(url):
    print(f"Downloading {url}")
    time.sleep(2)              # simulate network wait
    print(f"Done: {url}")

threads = []
for url in ["url1", "url2", "url3"]:
    t = threading.Thread(target=download, args=(url,))
    threads.append(t)
    t.start()

for t in threads:
    t.join()    # wait for all threads to finish
# All 3 run concurrently — total ~2s instead of 6s


# Multiprocessing — CPU bound (heavy calculations)
from multiprocessing import Pool

def heavy_calc(n):
    return sum(i**2 for i in range(n))

with Pool(processes=4) as pool:
    results = pool.map(heavy_calc, [100000, 200000, 300000, 400000])
# Each calc runs on a separate CPU core


# asyncio — best for many concurrent I/O operations
import asyncio

async def fetch_data(name, delay):
    print(f"Start fetching {name}")
    await asyncio.sleep(delay)    # non-blocking wait (like an API call)
    print(f"Done fetching {name}")
    return f"data from {name}"

async def main():
    # Run multiple coroutines concurrently
    results = await asyncio.gather(
        fetch_data("service1", 1),
        fetch_data("service2", 2),
        fetch_data("service3", 1),
    )
    print(results)

asyncio.run(main())
# Total time ~2s, not 4s (1+2+1)
```

**GIL (Global Interpreter Lock):**
- Python has a lock that allows only ONE thread to execute Python bytecode at a time
- This means threads don't run truly in parallel for CPU work
- But the GIL is released during I/O operations — so threading still helps for I/O
- To bypass GIL for CPU parallelism → use `multiprocessing`

| Use case | Best choice |
|----------|-------------|
| Many API calls | `asyncio` or `threading` |
| Reading many files | `threading` |
| Heavy math/data processing | `multiprocessing` |
| Simple concurrent I/O | `concurrent.futures.ThreadPoolExecutor` |

---

### 3.2 Shallow vs Deep Copy

**Definition:**  
- **Assignment (`=`):** Both variables point to the same object in memory. Modifying one affects the other.  
- **Shallow copy:** Creates a new outer object but the inner objects are still shared references.  
- **Deep copy:** Creates a completely independent copy — new outer AND new inner objects.

```python
import copy

original = [[1, 2], [3, 4]]

# Assignment — same object
ref = original
ref[0].append(99)
print(original)     # [[1, 2, 99], [3, 4]] — original changed!

# Shallow copy — new outer container, shared inner objects
shallow = copy.copy(original)
shallow.append([5, 6])      # outer is new — original NOT affected
shallow[0].append(99)       # inner is shared — original IS affected!
print(original)             # [[1, 2, 99], [3, 4]]

# Deep copy — completely independent
deep = copy.deepcopy(original)
deep[0].append(99)
print(original)             # [[1, 2], [3, 4]] — original unchanged
```

---

### 3.3 Closures

**Definition:**  
A closure is a function that remembers the variables from its enclosing scope even after that scope has finished executing. The inner function "closes over" the outer variable.

```python
def make_multiplier(n):
    def multiply(x):
        return x * n      # 'n' is captured from outer scope
    return multiply       # return inner function

double = make_multiplier(2)
triple = make_multiplier(3)

double(5)   # 10  — still remembers n=2
triple(5)   # 15  — still remembers n=3

# Practical use: counter factory
def make_counter(start=0):
    count = start
    def increment(by=1):
        nonlocal count       # 'nonlocal' allows modifying outer variable
        count += by
        return count
    return increment

counter = make_counter(10)
counter()     # 11
counter()     # 12
counter(5)    # 17

# LEGB Rule — Python's variable lookup order
x = "global"

def outer():
    x = "enclosing"
    def inner():
        x = "local"
        print(x)       # "local"    — L: local
    inner()
    print(x)           # "enclosing" — E: enclosing

outer()
print(x)               # "global"   — G: global
# B: built-in (len, print, etc.)
```

---

### 3.4 Type Hints

**Definition:**  
Type hints are annotations that indicate the expected types of function arguments and return values. They are not enforced at runtime but help with code readability, IDE autocompletion, and static analysis tools like `mypy`.

```python
from typing import List, Dict, Optional, Union, Tuple, Any, Callable

# Basic
def greet(name: str) -> str:
    return f"Hello {name}"

def add(a: int, b: int) -> int:
    return a + b

# Complex types
def process(items: List[int]) -> Dict[str, int]:
    return {"sum": sum(items), "count": len(items)}

# Optional — value can be the type OR None
def find_user(uid: int) -> Optional[str]:
    return None  # or return a string

# Union — value can be one of multiple types
def parse(value: Union[int, str]) -> str:
    return str(value)

# Python 3.10+ cleaner syntax
def newer(value: int | str) -> str:
    return str(value)

# Callable — function type hint
def apply(func: Callable[[int, int], int], a: int, b: int) -> int:
    return func(a, b)

# Tuple with specific types
def get_coords() -> Tuple[float, float]:
    return (10.5, 20.3)
```

---

### 3.5 Dataclasses

**Definition:**  
A dataclass is a class decorator that automatically generates boilerplate methods like `__init__`, `__repr__`, and `__eq__` based on the class attributes you define. Cleaner than writing these manually.

```python
from dataclasses import dataclass, field

@dataclass
class User:
    name: str
    age: int
    skills: list = field(default_factory=list)   # mutable defaults need field()
    is_active: bool = True

    def is_senior(self) -> bool:
        return self.age > 30

u1 = User("Nara", 25)
u2 = User("Nara", 25)
print(u1)         # User(name='Nara', age=25, skills=[], is_active=True)
print(u1 == u2)   # True — __eq__ auto-generated

# frozen=True makes it immutable (like tuple)
@dataclass(frozen=True)
class Point:
    x: float
    y: float

p = Point(1.0, 2.0)
# p.x = 5  # FrozenInstanceError
```

---

### 3.6 Regex

**Definition:**  
Regular expressions (regex) are patterns used to match, search, and manipulate text. Python's `re` module provides regex support.

```python
import re

text = "My email is nara@gmail.com and phone is +91-9876543210"

# re.search()  — finds first match anywhere in string
match = re.search(r'\w+@\w+\.\w+', text)
if match:
    print(match.group())        # "nara@gmail.com"
    print(match.start())        # start index
    print(match.end())          # end index

# re.findall() — returns all matches as list
emails = re.findall(r'\w+@\w+\.\w+', text)   # ["nara@gmail.com"]

# re.sub()     — replace matches
clean = re.sub(r'\d', 'X', text)              # replace every digit with X
clean = re.sub(r'\s+', ' ', text)             # collapse multiple spaces

# re.split()   — split by pattern
parts = re.split(r'[,\s]+', "one, two,  three")  # ["one","two","three"]

# re.compile() — compile pattern for reuse (faster in loops)
pattern = re.compile(r'\d+')
pattern.findall("I have 3 cats and 12 dogs")   # ["3", "12"]

# Pattern cheatsheet
r'\d'      # any digit 0-9
r'\D'      # non-digit
r'\w'      # word character: letter, digit, _
r'\W'      # non-word
r'\s'      # whitespace: space, tab, newline
r'\S'      # non-whitespace
r'.'       # any character except newline
r'^'       # start of string
r'$'       # end of string
r'+'       # 1 or more (greedy)
r'*'       # 0 or more (greedy)
r'?'       # 0 or 1 (optional)
r'{3}'     # exactly 3
r'{2,5}'   # 2 to 5
r'[a-z]'   # character class: lowercase letter
r'[^abc]'  # NOT a, b, or c
r'(abc)'   # capturing group
r'(?:abc)' # non-capturing group
r'a|b'     # a OR b
```

---

# PART 2: AI / ML BASICS

---

## WEEK 4 — AI/ML FUNDAMENTALS

### 4.1 What is AI, ML, DL?

**Artificial Intelligence (AI):**  
AI is the broad concept of machines simulating human intelligence — reasoning, learning, problem-solving, understanding language, recognizing images.

**Machine Learning (ML):**  
A subset of AI where machines learn patterns from data automatically, without being explicitly programmed with rules.

**Deep Learning (DL):**  
A subset of ML that uses neural networks with many layers (deep networks). It excels at image, speech, and text tasks. Requires large amounts of data.

```
AI → ML → Deep Learning → (Transformers, LLMs)
```

**Types of ML:**

| Type | Definition | Real Example |
|------|-----------|--------------|
| **Supervised** | Learns from labeled data (input + known output). Model maps inputs to outputs. | Spam detection, price prediction, disease diagnosis |
| **Unsupervised** | No labels. Finds hidden patterns or structures in data. | Customer segmentation, anomaly detection, topic modeling |
| **Reinforcement** | Agent learns by trial and error. Gets reward for good actions, penalty for bad. | Chess AI, game bots, robotics |

**ML Workflow:**
```
1. Define problem
2. Collect & explore data (EDA)
3. Clean data (handle nulls, outliers)
4. Feature engineering (create useful inputs)
5. Split data (train/test)
6. Choose & train model
7. Evaluate model
8. Tune hyperparameters
9. Deploy
```

---

### 4.2 Key Algorithms

#### Linear Regression
**Definition:** Predicts a continuous numerical output by finding the best-fit straight line through data points. Minimizes the sum of squared errors between predicted and actual values.

```python
from sklearn.linear_model import LinearRegression
import numpy as np

X = np.array([[1],[2],[3],[4],[5]])
y = np.array([2.1, 4.0, 6.2, 7.9, 10.1])   # y ≈ 2x

model = LinearRegression()
model.fit(X, y)

model.predict([[6]])    # ≈ [12.0]
model.coef_             # slope (≈2)
model.intercept_        # y-intercept (≈0)
```

#### Logistic Regression
**Definition:** Despite its name, used for CLASSIFICATION not regression. Predicts the probability that an input belongs to a class (0 or 1). Uses sigmoid function to output values between 0 and 1.

```
sigmoid(x) = 1 / (1 + e^(-x))
If probability > 0.5 → class 1, else class 0
```

```python
from sklearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fit(X_train, y_train)
model.predict_proba(X_test)   # probabilities for each class
```

#### Decision Tree
**Definition:** Splits data into branches based on feature conditions (if/else rules), forming a tree structure. Each leaf node represents a class or value prediction. Easy to understand and visualize.

- **Pro:** Interpretable, no scaling needed
- **Con:** Overfits easily — memorizes training data

#### Random Forest
**Definition:** An ensemble of many decision trees. Each tree is trained on a random subset of data and features. Final prediction = majority vote (classification) or average (regression). Reduces overfitting compared to a single tree.

- **Pro:** More accurate, handles missing values, feature importance
- **Con:** Slower, less interpretable

```python
from sklearn.ensemble import RandomForestClassifier
model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)
model.feature_importances_   # which features matter most
```

#### K-Nearest Neighbors (KNN)
**Definition:** Classifies a data point based on the majority class of its K nearest neighbors in the feature space. No training step — it memorizes all data and compares at prediction time.

- **Pro:** Simple, no training time
- **Con:** Slow prediction for large datasets, sensitive to scale (must normalize)

#### Support Vector Machine (SVM)
**Definition:** Finds the hyperplane (line in 2D, plane in 3D) that best separates two classes with the maximum margin. Data points closest to the hyperplane are called "support vectors."

- **Pro:** Effective for high-dimensional data
- **Con:** Slow for large datasets, hard to interpret

---

### 4.3 Data Preprocessing

**Definition:**  
Data preprocessing is the process of cleaning and transforming raw data into a format suitable for machine learning. Real-world data is always messy — missing values, wrong types, inconsistent formats, unbalanced classes.

```python
import pandas as pd
import numpy as np
from sklearn.preprocessing import StandardScaler, MinMaxScaler, LabelEncoder
from sklearn.model_selection import train_test_split

df = pd.read_csv("data.csv")

# Step 1: Explore
df.shape             # (rows, cols)
df.dtypes            # data types of each column
df.info()            # dtypes + null counts
df.describe()        # count, mean, std, min, max, quartiles
df.isnull().sum()    # null count per column
df.duplicated().sum()# duplicate rows

# Step 2: Handle missing values
df["age"].fillna(df["age"].mean(), inplace=True)      # fill with mean
df["city"].fillna("Unknown", inplace=True)            # fill with constant
df["salary"].fillna(df["salary"].median(), inplace=True)  # fill with median
df.dropna(subset=["target"], inplace=True)            # drop rows if target is null
df.dropna(thresh=5, inplace=True)                     # drop rows with < 5 non-null values

# Step 3: Encode categorical → numerical
# Label Encoding — for ordinal (ordered) categories
le = LabelEncoder()
df["size"] = le.fit_transform(df["size"])    # S→0, M→1, L→2

# One-Hot Encoding — for nominal (unordered) categories
df = pd.get_dummies(df, columns=["city"], drop_first=True)

# Step 4: Feature Scaling
# StandardScaler — mean=0, std=1 (use for SVM, logistic regression)
scaler = StandardScaler()
df[["age","salary"]] = scaler.fit_transform(df[["age","salary"]])

# MinMaxScaler — scales to [0,1] (use for neural networks, KNN)
mms = MinMaxScaler()
df[["age","salary"]] = mms.fit_transform(df[["age","salary"]])

# Step 5: Train-test split
X = df.drop("target", axis=1)
y = df["target"]
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y  # stratify keeps class ratio
)
```

---

### 4.4 Model Evaluation

**Definition:**  
Model evaluation measures how well a trained model performs on unseen data. Different metrics are used for classification vs regression problems.

```python
from sklearn.metrics import (
    accuracy_score, precision_score, recall_score, f1_score,
    confusion_matrix, classification_report,
    mean_squared_error, mean_absolute_error, r2_score
)

y_pred = model.predict(X_test)

# ─── CLASSIFICATION ───────────────────────────────────────

# Confusion Matrix
#            Predicted 0   Predicted 1
# Actual 0     TN             FP
# Actual 1     FN             TP

cm = confusion_matrix(y_test, y_pred)
# TN = correctly predicted negative
# TP = correctly predicted positive
# FP = predicted positive but actually negative (false alarm)
# FN = predicted negative but actually positive (missed)

accuracy_score(y_test, y_pred)
# = (TP + TN) / total
# How many total predictions were correct

precision_score(y_test, y_pred)
# = TP / (TP + FP)
# Of all predicted positives, how many were actually positive
# Important when: false positives are costly (spam filter — don't block real emails)

recall_score(y_test, y_pred)
# = TP / (TP + FN)
# Of all actual positives, how many did we catch
# Important when: false negatives are costly (disease detection — don't miss sick patients)

f1_score(y_test, y_pred)
# = 2 * (precision * recall) / (precision + recall)
# Harmonic mean of precision and recall
# Use when: imbalanced classes, need balance between precision and recall

print(classification_report(y_test, y_pred))  # all metrics in one table


# ─── REGRESSION ───────────────────────────────────────────

mean_squared_error(y_test, y_pred)
# Average of squared differences between predicted and actual
# Penalizes large errors heavily

mean_absolute_error(y_test, y_pred)
# Average of absolute differences — easier to interpret (same unit as target)

r2_score(y_test, y_pred)
# R² = 1.0 means model explains all variance (perfect)
# R² = 0.0 means model is as good as just predicting the mean
# R² < 0 means model is worse than predicting the mean


# Cross-validation — more reliable evaluation
from sklearn.model_selection import cross_val_score
scores = cross_val_score(model, X, y, cv=5, scoring="accuracy")
print(f"Mean: {scores.mean():.3f}, Std: {scores.std():.3f}")
```

**When to use which metric:**
| Metric | Use when |
|--------|----------|
| Accuracy | Balanced classes (equal 0s and 1s) |
| Precision | False positives are costly |
| Recall | False negatives are costly |
| F1 Score | Imbalanced classes |
| R² | Regression: overall fit quality |
| MAE | Regression: interpretable average error |
| MSE | Regression: when large errors must be penalized |

---

### 4.5 Overfitting vs Underfitting

**Overfitting:**  
The model learns the training data too well — including noise and random patterns. It performs very well on training data but poorly on new (test) data. Like a student who memorizes answers without understanding concepts.

**Underfitting:**  
The model is too simple to capture the underlying pattern in the data. Performs poorly on both training and test data.

**Bias-Variance Tradeoff:**
- **High Bias** = underfitting. Model makes strong assumptions, misses patterns.
- **High Variance** = overfitting. Model is too sensitive to training data.
- **Goal:** Find the sweet spot — low bias AND low variance.

| Problem | Training Accuracy | Test Accuracy | Fix |
|---------|------------------|---------------|-----|
| Overfitting | High | Low | More data, regularization, simpler model, dropout, cross-validation |
| Underfitting | Low | Low | More features, complex model, train longer |
| Good fit | High | High ≈ Training | — |

**Regularization:**
- **L1 (Lasso):** Adds absolute value of weights to loss. Can zero out unimportant features (feature selection).
- **L2 (Ridge):** Adds squared weights to loss. Shrinks all weights, doesn't zero out. Most common.
- **Dropout (Neural Networks):** Randomly turns off neurons during training. Prevents co-dependency.

---

### 4.6 Neural Networks Basics

**Definition:**  
A neural network is a model inspired by the human brain, made of layers of connected nodes (neurons). Each connection has a weight. The network learns by adjusting weights to minimize prediction error.

```
Input Layer → Hidden Layer(s) → Output Layer

Each neuron:  output = activation_function(sum(weights × inputs) + bias)
```

**Activation Functions:**
| Function | Formula | Range | Use |
|----------|---------|-------|-----|
| **ReLU** | max(0, x) | [0, ∞) | Hidden layers — most common |
| **Sigmoid** | 1/(1+e^-x) | (0, 1) | Binary output layer |
| **Softmax** | e^x / Σe^x | (0,1), sums to 1 | Multi-class output |
| **Tanh** | (e^x - e^-x)/(e^x + e^-x) | (-1, 1) | Hidden layers |

**Training process:**
1. **Forward pass** — input flows through network, prediction made
2. **Loss calculation** — compare prediction to actual (e.g., cross-entropy)
3. **Backpropagation** — compute gradient of loss with respect to each weight
4. **Gradient descent** — update weights in direction that reduces loss

```python
from sklearn.neural_network import MLPClassifier

model = MLPClassifier(
    hidden_layer_sizes=(100, 50),  # 2 hidden layers: 100 and 50 neurons
    activation='relu',
    solver='adam',                  # optimizer
    max_iter=300,
    random_state=42
)
model.fit(X_train, y_train)
```

---

### 4.7 Pandas Quick Reference

**Definition:**  
Pandas is Python's primary data manipulation library. It provides DataFrame (2D table) and Series (1D array) structures for loading, cleaning, transforming, and analyzing data.

```python
import pandas as pd

df = pd.DataFrame({
    "name":  ["Nara", "Ram", "Sita", "Ram"],
    "score": [85, 90, 78, 90],
    "dept":  ["IT", "IT", "HR", "HR"]
})

# ─── SELECTION ─────────────────────────────
df["name"]                          # single column → Series
df[["name", "score"]]               # multiple columns → DataFrame
df.loc[0]                           # row by label/index
df.iloc[0]                          # row by integer position
df.loc[0, "name"]                   # specific cell
df[df["score"] > 80]                # filter rows
df[(df["score"] > 80) & (df["dept"] == "IT")]  # multiple conditions
df[df["name"].isin(["Nara","Ram"])] # filter by list

# ─── OPERATIONS ────────────────────────────
df["score"].mean()           # 85.75
df["score"].sum()
df["score"].max()
df["score"].min()
df["score"].std()
df["score"].value_counts()   # frequency of each value

# ─── GROUPBY ───────────────────────────────
df.groupby("dept")["score"].mean()     # avg score per dept
df.groupby("dept").agg({"score": ["mean","max","count"]})

# ─── SORTING ───────────────────────────────
df.sort_values("score", ascending=False)
df.sort_values(["dept","score"], ascending=[True, False])

# ─── CLEANING ──────────────────────────────
df.drop_duplicates()
df.drop_duplicates(subset=["name"])     # deduplicate by column
df.dropna()
df.fillna(0)
df.rename(columns={"name": "Name", "score": "Score"})
df.drop(columns=["dept"])
df["score"] = df["score"].astype(float)

# ─── MERGING ───────────────────────────────
# Like SQL joins
merged = pd.merge(df1, df2, on="id", how="left")
# how = "left", "right", "inner", "outer"

# ─── PIVOT ─────────────────────────────────
df.pivot_table(values="score", index="dept", aggfunc="mean")

# ─── APPLY ─────────────────────────────────
df["grade"] = df["score"].apply(lambda x: "A" if x >= 85 else "B")
df["full"] = df.apply(lambda row: f"{row['name']}-{row['dept']}", axis=1)
```

---

# INTERVIEW QUESTIONS WITH FULL ANSWERS

---

## Python Questions

---

**Q1. What is the difference between `list`, `tuple`, `set`, `dict`?**

> A **list** is an ordered, mutable sequence that allows duplicates. Use when you need an ordered collection that can change.  
> A **tuple** is an ordered, immutable sequence. Use for fixed data, function returns, or as dictionary keys.  
> A **set** is an unordered collection of unique elements. Use when you need uniqueness or fast membership testing (O(1)).  
> A **dict** is an unordered collection of key-value pairs. Use for fast key-based lookups (O(1) average).  
>
> Key difference: list/tuple are indexed (ordered), set/dict are not (unordered). list/dict are mutable, tuple is immutable, set is mutable but elements must be immutable.

---

**Q2. What is the GIL and how does it affect threading?**

> The **Global Interpreter Lock (GIL)** is a mutex (lock) in CPython that allows only one thread to execute Python bytecode at a time, even on multi-core systems.  
>
> **Effect on threading:**
> - For **CPU-bound** tasks (heavy computation): threading doesn't give true parallelism because of GIL — only one thread runs Python at a time. Use `multiprocessing` instead.
> - For **I/O-bound** tasks (network, file, database): GIL is released while waiting for I/O. So multiple threads CAN run concurrently in practice — one thread waits for I/O while others execute.
>
> **Why does GIL exist?** It simplifies CPython's memory management (reference counting) by preventing race conditions.  
> **How to bypass for CPU tasks:** Use `multiprocessing` module (separate processes, each has its own GIL) or use libraries like NumPy that release GIL internally.

---

**Q3. Explain decorators with an example.**

> A **decorator** is a function that takes another function as input, wraps it with additional behavior, and returns the modified function — without changing the original function's code.  
>
> **Example:**
> ```python
> import functools
> def timer(func):
>     @functools.wraps(func)
>     def wrapper(*args, **kwargs):
>         import time
>         start = time.time()
>         result = func(*args, **kwargs)
>         print(f"{func.__name__} took {time.time()-start:.4f}s")
>         return result
>     return wrapper
>
> @timer
> def process_data(n):
>     return sum(range(n))
>
> process_data(1000000)  # prints: process_data took 0.0423s
> ```
> `@timer` is syntactic sugar for `process_data = timer(process_data)`.  
> `@functools.wraps(func)` preserves the original function's `__name__` and `__doc__`.

---

**Q4. What is the difference between `@staticmethod` and `@classmethod`?**

> **`@staticmethod`:** A method that belongs to the class but receives NO reference to the class or instance. It's just a regular function that lives inside a class for organizational purposes. Cannot access or modify class/instance state.
>
> **`@classmethod`:** A method that receives the **class itself** (`cls`) as the first argument instead of the instance (`self`). Can access and modify class-level state. Commonly used as alternative constructors.
>
> ```python
> class Date:
>     def __init__(self, year, month, day):
>         self.year = year
>
>     @classmethod
>     def from_string(cls, date_str):       # alternative constructor
>         year, month, day = map(int, date_str.split("-"))
>         return cls(year, month, day)      # creates instance
>
>     @staticmethod
>     def is_valid(date_str):               # utility — no class/instance needed
>         return len(date_str.split("-")) == 3
>
> d = Date.from_string("2026-04-17")       # classmethod
> Date.is_valid("2026-04-17")              # True — staticmethod
> ```

---

**Q5. What is a generator? How is it different from a list?**

> A **generator** is a function that uses `yield` to produce values one at a time, on demand. Instead of computing all values at once and storing in memory, it computes each value only when requested.
>
> ```python
> def squares(n):
>     for i in range(n):
>         yield i ** 2     # pauses here, resumes on next()
>
> gen = squares(5)
> next(gen)  # 0  — computes only this value
> next(gen)  # 1
> ```
>
> **Differences from list:**
>
> | | List | Generator |
> |-|------|-----------|
> | Memory | All values stored in RAM | One value at a time |
> | Creation | `[x**2 for x in range(n)]` | `(x**2 for x in range(n))` |
> | Reusable | Yes — iterate multiple times | No — exhausted after one pass |
> | Speed | Faster if all values needed | Faster for large/infinite sequences |
>
> **Use generators when:** processing large files, infinite sequences, streaming data pipelines (like your Kafka work).

---

**Q6. What is shallow copy vs deep copy?**

> - **Assignment (`=`):** Both variables point to the same object. Any change reflects in both.
> - **Shallow copy (`copy.copy()`):** Creates a new outer container but inner objects are still shared. Safe for flat structures, dangerous for nested.
> - **Deep copy (`copy.deepcopy()`):** Creates a fully independent copy — new outer AND new inner objects. Safe for all structures, but slower.
>
> ```python
> import copy
> original = [[1, 2], [3, 4]]
>
> ref = original
> ref[0].append(9)        # original is affected
>
> shallow = copy.copy(original)
> shallow[0].append(9)    # original IS affected (inner is shared)
>
> deep = copy.deepcopy(original)
> deep[0].append(9)       # original NOT affected
> ```

---

**Q7. Explain the LEGB scope rule.**

> LEGB is the order in which Python looks up variable names:
>
> - **L — Local:** Variables defined inside the current function
> - **E — Enclosing:** Variables in enclosing functions (for nested functions/closures)
> - **G — Global:** Variables defined at the module (file) level
> - **B — Built-in:** Python's built-in names like `len`, `print`, `range`
>
> ```python
> x = "global"
>
> def outer():
>     x = "enclosing"
>     def inner():
>         x = "local"
>         print(x)   # "local"  — found at L
>     inner()
>     print(x)       # "enclosing" — found at E
>
> outer()
> print(x)           # "global" — found at G
> ```
> Python searches L → E → G → B in that order. Stops at first match.

---

**Q8. What is `*args` and `**kwargs`?**

> **`*args`:** Allows a function to accept any number of **positional arguments**. They are collected into a **tuple** inside the function.
>
> **`**kwargs`:** Allows a function to accept any number of **keyword arguments**. They are collected into a **dict** inside the function.
>
> ```python
> def show(*args, **kwargs):
>     print(args)    # tuple of positional args
>     print(kwargs)  # dict of keyword args
>
> show(1, 2, 3, name="Nara", city="Hyd")
> # (1, 2, 3)
> # {"name": "Nara", "city": "Hyd"}
> ```
>
> Also used for unpacking:
> ```python
> def add(a, b, c): return a + b + c
> nums = [1, 2, 3]
> add(*nums)          # unpacks list as positional args → add(1, 2, 3)
>
> info = {"a":1, "b":2, "c":3}
> add(**info)         # unpacks dict as keyword args → add(a=1, b=2, c=3)
> ```

---

**Q9. What are dunder/magic methods?**

> **Dunder (double underscore) methods** — also called magic methods — are special methods with `__` prefix and suffix. Python calls them automatically in response to certain operations.
>
> | Method | When called |
> |--------|-------------|
> | `__init__` | Object creation: `obj = MyClass()` |
> | `__str__` | `print(obj)` or `str(obj)` |
> | `__repr__` | REPL display, `repr(obj)` |
> | `__len__` | `len(obj)` |
> | `__eq__` | `obj1 == obj2` |
> | `__lt__` | `obj1 < obj2` |
> | `__add__` | `obj1 + obj2` |
> | `__getitem__` | `obj[key]` |
> | `__contains__` | `x in obj` |
> | `__enter__`/`__exit__` | `with obj:` |
> | `__iter__`/`__next__` | `for x in obj:` |
>
> ```python
> class Vector:
>     def __init__(self, x, y):
>         self.x, self.y = x, y
>     def __add__(self, other):
>         return Vector(self.x + other.x, self.y + other.y)
>     def __str__(self):
>         return f"Vector({self.x}, {self.y})"
>
> v = Vector(1,2) + Vector(3,4)
> print(v)   # Vector(4, 6)
> ```

---

**Q10. How does Python memory management work?**

> Python manages memory using two main mechanisms:
>
> 1. **Reference Counting:** Every object has a counter tracking how many variables point to it. When the count reaches 0, memory is immediately freed.
>
> ```python
> a = [1, 2, 3]   # ref count = 1
> b = a            # ref count = 2
> del a            # ref count = 1
> del b            # ref count = 0 → freed
> ```
>
> 2. **Garbage Collector:** Handles **circular references** (objects that reference each other) which reference counting can't detect. Python's `gc` module runs periodically to find and free these.
>
> ```python
> import gc
> gc.collect()    # manually trigger garbage collection
> ```
>
> Python also has a **memory pool** (PyMalloc) — small objects (< 512 bytes) are allocated from pre-allocated pools for efficiency. Small integers (-5 to 256) and interned strings are cached and reused.

---

**Q11. What is the difference between `is` and `==`?**

> - **`==`** checks **value equality** — are the values the same?
> - **`is`** checks **identity** — are they the exact same object in memory (same id)?
>
> ```python
> a = [1, 2, 3]
> b = [1, 2, 3]
> c = a
>
> a == b   # True  — same values
> a is b   # False — different objects in memory
> a is c   # True  — same object (c = a, not a copy)
>
> # Small integers are cached (-5 to 256):
> x = 100
> y = 100
> x is y   # True  — CPython caches small ints
>
> x = 1000
> y = 1000
> x is y   # False — large ints are not cached
>
> # Always use 'is' for None check:
> if value is None:   # correct
> if value == None:   # works but discouraged
> ```

---

**Q12. What is a closure?**

> A **closure** is a function that retains access to variables from its enclosing scope even after that scope has finished executing.
>
> Three conditions for a closure:
> 1. There must be a nested function (function inside a function)
> 2. The nested function must refer to a variable in the enclosing scope
> 3. The enclosing function must return the nested function
>
> ```python
> def make_adder(n):
>     def add(x):
>         return x + n     # 'n' is captured from outer scope
>     return add
>
> add5  = make_adder(5)
> add10 = make_adder(10)
>
> add5(3)    # 8  — 'n' is still 5
> add10(3)   # 13 — 'n' is still 10
> ```
>
> Real use: factory functions, decorators, callbacks, maintaining state without a class.

---

**Q13. Explain `async/await`. When would you use it?**

> `async/await` is Python's syntax for **asynchronous programming**. It allows a function to pause and yield control back to the event loop while waiting for I/O, so other tasks can run in the meantime — without creating threads.
>
> - `async def` — declares an asynchronous coroutine function
> - `await` — pauses the current coroutine until the awaited task completes
> - `asyncio.run()` — runs the event loop
>
> ```python
> import asyncio
>
> async def fetch(url):
>     print(f"Starting {url}")
>     await asyncio.sleep(1)   # simulates a non-blocking API call
>     return f"Data from {url}"
>
> async def main():
>     results = await asyncio.gather(
>         fetch("api1"),
>         fetch("api2"),
>         fetch("api3"),
>     )
>     print(results)   # all 3 run concurrently — total ~1s not 3s
>
> asyncio.run(main())
> ```
>
> **Use when:** Making many concurrent HTTP requests, database queries, WebSocket connections, any I/O-heavy workloads. Don't use for CPU-heavy tasks — use multiprocessing for that.

---

**Q14. What is `__init__` vs `__new__`?**

> - **`__new__`** creates the object — allocates memory and returns a new instance. Called first.
> - **`__init__`** initializes the object — sets up attributes on the already-created instance. Called after `__new__`.
>
> In practice, you almost never override `__new__`. It's used for:
> - Singletons (ensure only one instance exists)
> - Immutable type subclasses (like subclassing `int`, `str`, `tuple`)
>
> ```python
> class Singleton:
>     _instance = None
>
>     def __new__(cls):
>         if cls._instance is None:
>             cls._instance = super().__new__(cls)
>         return cls._instance   # always return the same instance
>
>     def __init__(self):
>         self.value = 42
>
> a = Singleton()
> b = Singleton()
> a is b    # True — same object
> ```

---

**Q15. How is `dict` implemented internally in Python?**

> Python `dict` is implemented as a **hash table** (also called hash map).
>
> **How it works:**
> 1. When you do `d["key"] = value`, Python computes `hash("key")` — a fixed-size integer
> 2. Uses `hash % table_size` to find the bucket index
> 3. Stores the key-value pair in that bucket
> 4. On lookup `d["key"]`, computes hash again, goes directly to that bucket — O(1)
> 5. If two keys hash to the same bucket (**collision**), Python uses **open addressing** (probing nearby slots)
>
> This is why:
> - Lookup, insert, delete are O(1) average
> - Dict keys must be **hashable** (immutable) — lists can't be keys, tuples can
> - Dicts in Python 3.7+ maintain **insertion order** (guaranteed)
>
> ```python
> hash("name")   # some fixed integer
> hash(42)       # 42
> hash([1,2])    # TypeError: unhashable type: 'list'
> ```

---

## AI/ML Questions

---

**Q1. What is the difference between supervised and unsupervised learning?**

> **Supervised learning:** Training data has labels (known correct answers). The model learns to map inputs to outputs. Examples: spam detection (label = spam/not), house price prediction (label = price).
>
> **Unsupervised learning:** Training data has NO labels. The model finds hidden structure or patterns on its own. Examples: customer segmentation (cluster customers by behavior), anomaly detection (find unusual transactions).
>
> Key difference: supervised needs human-labeled data (expensive, time-consuming). Unsupervised finds patterns automatically but results are harder to interpret.

---

**Q2. What is overfitting? How do you prevent it?**

> **Overfitting** is when a model learns the training data too well — including its noise and random variations — resulting in poor performance on new, unseen data. The model has memorized rather than generalized.
>
> Signs: very high training accuracy, much lower test accuracy.
>
> **Prevention:**
> - **More training data** — harder to memorize with more examples
> - **Regularization (L1/L2)** — penalizes large weights in the model
> - **Cross-validation** — use k-fold to test on multiple splits
> - **Simpler model** — fewer parameters (shallower tree, fewer neurons)
> - **Dropout** (neural networks) — randomly disable neurons during training
> - **Early stopping** — stop training when validation loss starts increasing
> - **Data augmentation** — artificially increase training data variety

---

**Q3. Explain bias-variance tradeoff.**

> **Bias:** Error from wrong assumptions in the model. High bias = model too simple, underfits (misses patterns). Example: fitting a straight line to curved data.
>
> **Variance:** Error from sensitivity to small fluctuations in training data. High variance = model too complex, overfits (memorizes noise). Example: decision tree that perfectly fits every training point.
>
> **The tradeoff:** Reducing bias (more complex model) increases variance, and vice versa. The goal is to find the sweet spot — low bias AND low variance — that minimizes total error on unseen data.
>
> Total Error = Bias² + Variance + Irreducible Noise

---

**Q4. What is cross-validation?**

> Cross-validation is a technique to evaluate a model's performance more reliably by testing it on multiple different subsets of data.
>
> **K-Fold cross-validation (most common):**
> 1. Split data into K equal parts (folds) — typically K=5 or K=10
> 2. Train on K-1 folds, test on the remaining fold
> 3. Repeat K times, each fold gets a turn as the test set
> 4. Final score = average of all K test scores
>
> **Why use it?** A single train/test split might get lucky or unlucky. Cross-validation gives a much better estimate of how the model will perform on truly new data.
>
> ```python
> from sklearn.model_selection import cross_val_score
> scores = cross_val_score(model, X, y, cv=5)
> print(f"Accuracy: {scores.mean():.3f} ± {scores.std():.3f}")
> ```

---

**Q5. Difference between precision and recall?**

> Both measure classification performance, but focus on different types of errors:
>
> **Precision** = TP / (TP + FP)  
> "Of everything I predicted as positive, what fraction was actually positive?"  
> Use when false positives are costly. Example: spam filter — you don't want to mark legitimate emails as spam.
>
> **Recall** = TP / (TP + FN)  
> "Of all actual positives, what fraction did I correctly identify?"  
> Use when false negatives are costly. Example: cancer detection — you don't want to miss a sick patient.
>
> **F1 Score** = harmonic mean of both. Use when you need a balance between the two, especially with imbalanced data.
>
> You can't maximize both at once — increasing one usually decreases the other.

---

**Q6. When would you use Random Forest over Decision Tree?**

> Use **Random Forest** when:
> - You need better accuracy — RF almost always outperforms a single tree
> - Your data is complex with many features
> - Overfitting is a concern — RF reduces it through averaging
> - You need feature importance rankings
>
> Use **Decision Tree** when:
> - Interpretability is critical — you need to explain every decision to stakeholders
> - You have very limited computational resources
> - You're prototyping quickly
> - The dataset is small and simple
>
> In production, Random Forest is almost always preferred over a single Decision Tree unless explainability is the top priority.

---

**Q7. What is gradient descent?**

> **Gradient descent** is the optimization algorithm used to train machine learning models. It iteratively adjusts model parameters (weights) to minimize the loss (error) function.
>
> **How it works:**
> 1. Start with random weights
> 2. Compute the loss (how wrong the model is)
> 3. Compute the gradient (direction of steepest increase in loss)
> 4. Move weights in the opposite direction (downhill)
> 5. Repeat until loss stops decreasing
>
> `weight = weight - learning_rate × gradient`
>
> **Variants:**
> - **Batch GD:** Use entire dataset to compute gradient. Stable but slow.
> - **Stochastic GD (SGD):** Use one sample at a time. Fast but noisy.
> - **Mini-batch GD:** Use small batch (32-256 samples). Best of both. Most common.
>
> **Learning rate:** If too high → overshoots minimum. If too low → very slow convergence.

---

**Q8. What is feature engineering?**

> **Feature engineering** is the process of using domain knowledge to create, transform, or select input variables (features) that improve model performance. It's often the most impactful step in the ML pipeline.
>
> Examples:
> - **Creating new features:** From "date of birth" → create "age", "birth month", "is_weekend"
> - **Binning:** Convert continuous age (25, 30, 45) into categories (young, middle, senior)
> - **Log transform:** Reduce skew in salary distribution
> - **Interaction features:** `price × quantity = revenue`
> - **Encoding:** Convert city names to numbers
> - **Extracting from text:** Email domain from email address
>
> Good features can make a simple model outperform a complex model with raw features.

---

**Q9. What is normalization vs standardization?**

> Both are **feature scaling** techniques — they transform features to a similar range so no single feature dominates.
>
> **Normalization (Min-Max Scaling):**  
> Scales values to a fixed range, usually [0, 1].  
> `x_scaled = (x - min) / (max - min)`  
> Use when: you know the bounds, neural networks, KNN, image pixel values.
>
> **Standardization (Z-score Scaling):**  
> Centers data around mean 0 with standard deviation 1.  
> `x_scaled = (x - mean) / std`  
> Use when: data is normally distributed, SVM, logistic regression, PCA.
>
> **Key difference:** Normalization is bounded (always 0-1). Standardization is unbounded — outliers don't get clipped but their effect is reduced.

---

**Q10. What is a confusion matrix?**

> A confusion matrix is a table that summarizes the performance of a classification model by showing the counts of correct and incorrect predictions for each class.
>
> ```
>                  Predicted: NO    Predicted: YES
> Actual: NO         TN (correct)    FP (false alarm)
> Actual: YES        FN (missed)     TP (correct)
> ```
>
> - **TN (True Negative):** Correctly predicted as negative
> - **TP (True Positive):** Correctly predicted as positive
> - **FP (False Positive):** Predicted positive but actually negative — Type I error
> - **FN (False Negative):** Predicted negative but actually positive — Type II error
>
> From these 4 values you derive: Accuracy, Precision, Recall, F1 Score.  
> A perfect model has all values on the diagonal (TN and TP) with FP=0 and FN=0.

---

## Prowesssoft-Specific Questions

---

**Q1. How do you design a REST API in Flask?**

> Define resources as URLs, use HTTP methods for CRUD operations, return JSON responses with appropriate status codes.
>
> Key principles:
> - Use nouns for URLs (`/users`, not `/getUsers`)
> - Use HTTP methods semantically (GET=read, POST=create, PUT=update, DELETE=remove)
> - Always return appropriate status codes
> - Validate input and return meaningful error messages
> - Use `request.get_json()` for request body, `request.args` for query params
>
> Example structure for a `/users` resource: GET (list), POST (create), GET `/<id>` (fetch), PUT `/<id>` (update), DELETE `/<id>` (remove). See full code in section 2.5.

---

**Q2. What are HTTP methods and when to use each?**

> - **GET:** Retrieve a resource. No body. Safe and idempotent. Example: fetch user profile.
> - **POST:** Create a new resource. Has body. Not idempotent. Example: create new order.
> - **PUT:** Replace entire resource. Has body. Idempotent. Example: update full user record.
> - **PATCH:** Partially update resource. Has body. Example: change only email.
> - **DELETE:** Remove resource. Idempotent. Example: delete an account.
>
> **Idempotent** means calling it multiple times has the same effect as calling once. GET, PUT, DELETE are idempotent. POST is not — calling POST 3 times creates 3 resources.

---

**Q3. How do you handle authentication in APIs?**

> **API Keys:** Simple. Client sends a secret key in header (`X-API-Key: abc123`). Server validates against stored keys. Good for server-to-server.
>
> **JWT (JSON Web Token):** Client logs in → server returns a signed token → client sends token in every request header (`Authorization: Bearer <token>`). Server verifies signature without DB lookup. Stateless — good for scalability.
>
> ```python
> import jwt
>
> # Generate token on login
> token = jwt.encode({"user_id": 123, "exp": datetime.utcnow() + timedelta(hours=1)},
>                    SECRET_KEY, algorithm="HS256")
>
> # Verify token on each request
> payload = jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
> ```
>
> **OAuth2:** Delegated authorization (login with Google/GitHub). More complex but industry standard.

---

**Q4. What is Kafka? How does producer-consumer work?**

> **Apache Kafka** is a distributed event streaming platform. It acts as a high-throughput, fault-tolerant message broker between systems.
>
> **Core concepts:**
> - **Topic:** A named stream of messages (like a queue name)
> - **Partition:** Topics are split into partitions for parallelism and scalability
> - **Producer:** Application that publishes messages to a topic
> - **Consumer:** Application that reads messages from a topic
> - **Consumer Group:** Multiple consumers sharing the work — each partition is read by only one consumer in a group
> - **Offset:** The position of a message in a partition — consumers track their offset to know where they left off
> - **Broker:** A Kafka server that stores messages
>
> **How it works:**
> 1. Producer sends message to a topic → Kafka stores it in a partition
> 2. Consumer polls for messages → processes them → commits offset
> 3. If consumer crashes before committing, it re-reads from last committed offset
>
> This is exactly what you do at Spizen — consuming Pump.fun trade events from Kafka topics and writing to ClickHouse.

---

**Q5. What is the difference between a message queue and event streaming?**

> **Message Queue (RabbitMQ, SQS):**
> - Message is delivered to ONE consumer, then deleted
> - Point-to-point: one sender, one receiver
> - Once consumed, message is gone
> - Good for: task distribution, job queues, one-time processing
>
> **Event Streaming (Kafka):**
> - Message is retained on disk for a configurable period (hours, days, forever)
> - Multiple consumer groups can independently read the same messages
> - Consumers can replay historical events (seek to any offset)
> - Good for: audit logs, real-time analytics, event sourcing, multiple downstream systems

---

**Q6. How do you handle API rate limiting?**

> Rate limiting controls how many requests a client can make in a time window to prevent abuse and ensure fair usage.
>
> **Common strategies:**
> - **Fixed window:** Allow N requests per minute. Simple but allows burst at window boundary.
> - **Sliding window:** More accurate — tracks requests in last 60 seconds, not fixed intervals.
> - **Token bucket:** Each client has a bucket of tokens. Each request consumes one. Tokens refill at a fixed rate.
>
> **Implementation with Redis:**
> ```python
> import redis
> r = redis.Redis()
>
> def is_rate_limited(user_id, limit=100, window=60):
>     key = f"rate:{user_id}"
>     count = r.incr(key)
>     if count == 1:
>         r.expire(key, window)    # set TTL on first request
>     return count > limit
> ```
>
> Return `429 Too Many Requests` when limit exceeded.

---

**Q7. What is idempotency in APIs?**

> **Idempotency** means performing the same operation multiple times has the same effect as performing it once. The result doesn't change on repeated calls.
>
> - **Idempotent:** GET, PUT, DELETE, PATCH
> - **Not idempotent:** POST (creates new resource each time)
>
> **Why it matters:** In distributed systems, network failures can cause clients to retry requests. If your API isn't idempotent, retries can cause duplicate data (like charging a customer twice).
>
> **How to make POST idempotent:** Use an **idempotency key** — client generates a unique ID per request, sends it in the header. Server stores processed keys. If same key comes again, return the original response instead of processing again.
>
> ```python
> @app.route("/payments", methods=["POST"])
> def create_payment():
>     idempotency_key = request.headers.get("Idempotency-Key")
>     if idempotency_key and redis.get(idempotency_key):
>         return jsonify(redis.get(idempotency_key)), 200   # return cached result
>     # process payment...
>     redis.setex(idempotency_key, 86400, result)           # cache for 24h
> ```

---

## QUICK REVISION CHECKLIST

### Before the interview:
- [ ] OOP: class, inheritance, polymorphism, dunder methods
- [ ] Decorators: write one from scratch
- [ ] Generators: explain yield, difference from list
- [ ] `*args`, `**kwargs`: explain and use in a function
- [ ] List/dict comprehension
- [ ] `map`, `filter`, `zip`, `enumerate`
- [ ] Shallow vs deep copy
- [ ] try/except/finally
- [ ] GIL: what it is, how it affects threading
- [ ] `is` vs `==`
- [ ] Closure and LEGB rule
- [ ] REST API: write CRUD endpoints in Flask
- [ ] HTTP methods and status codes
- [ ] JWT authentication concept
- [ ] Kafka: topic, partition, consumer group, offset (you already know this well)
- [ ] ML: supervised vs unsupervised
- [ ] ML: overfitting, how to prevent
- [ ] ML: precision vs recall, when to use each
- [ ] ML: confusion matrix
- [ ] Pandas: groupby, merge, filter, apply

---

*Last updated: April 2026*
