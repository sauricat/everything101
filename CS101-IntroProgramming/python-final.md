# CS101 Introduction to Programming in Python

## Final Exam

Instructor: Charlotte Shao
Session: 2026 Winter

- Duration: 3 hrs.
- Minimal points to pass: 50%.
- Aids: Closed book. No aids (either paper or electronic) allowed.
- Assume CPython 3.11+ implementation standards unless otherwise stated.


### Part A: Basic Knowledge (10%)

**Select all correct answers. Each question may have 1–4 correct options.**

1. Regarding CPython’s memory management and small integer caching:
- A) `x = 256; y = 256; x is y` evaluates to `True` because integers in the range $[-5, 256]$ are singleton instances loaded at startup.
- B) `x = 1000; y = 1000; x is y` is guaranteed to be `False` in all execution environments (REPL, script execution, PyPy).
- C) `sys.getrefcount(x)` returns the exact number of references to object `x` as seen by the user code.
- D) Integer objects are immutable; performing `x += 1` changes the memory address `x` points to.
- E) None of above.
        
2. Regarding Default Arguments and Function Definitions:
- A) Default argument values are evaluated every time the function is called.
- B) Default argument values are stored in the `__defaults__` tuple of the function object.
- C) The code `def f(a, L=[]):` creates a new list `L` for every call where `L` is not provided.
- D) Using `None` as a sentinel value (e.g., `if L is None: L = []`) is the PEP 8 recommended pattern to avoid mutable default argument traps.
- E) None of above.
        
3. Regarding Class and Instance Namespaces:
- A) `__slots__` is used primarily to protect private variables from being accessed outside the class.
- B) Defining `__slots__` prevents the creation of a `__dict__` for instances, significantly reducing memory usage for millions of small objects.
- C) If `class A` defines variable `x = 1`, and instance `a = A()` executes `a.x = 2`, the class variable `A.x` is updated to 2.
- D) Name mangling (e.g., `__var`) makes a variable strictly private and unaccessible from outside the class, even via `_ClassName__var`.
- E) None of above.
        
4. Regarding Strings and Optimization:
- A) Strings in Python are mutable byte-arrays; `s[0] = 'a'` is a valid operation.
- B) String concatenation using `+=` inside a loop (e.g., `s += substring`) has a time complexity of $O(n^2)$ in the worst case, though CPython may optimize specific patterns.
- C) Explicitly using `''.join(list_of_strings)` is generally more efficient than repeated concatenation.
- D) Two string literals with the same content (e.g., `a = "hello_world"; b = "hello_world"`) are guaranteed to point to the same object (implicit interning) only if they contain ASCII alphanumerics and underscores.
- E) None of above.

5. Regarding Comprehensions and Generators:
- A) `[x*2 for x in range(10)]` and `(x*2 for x in range(10))` produce objects of the same type.
- B) Generator expressions evaluate lazy; values are produced only when iterated over.
- C) A list comprehension leaks the loop variable into the enclosing scope in Python 3.x (e.g., `x` remains defined after `[x for x in ...]`).
- D) `sum([i for i in range(1000000)])` consumes more memory than `sum(i for i in range(1000000))`.
- E) None of above.

### Part B: Code Analysis (15%)

#### B-1. Variables (2%)
Consider the following code,

```python
import copy
a = [1, [2, 3], 4]
b = list(a)
c = copy.deepcopy(a)

a[1][0] = 99
a[0] = 88

print(b)
print(c)
```

6. What are the contents of list `b` and list `c`?
7. Does `list(a)` perform a shallow copy or deep copy?
    
#### B-2. Function (3%)
Consider the following code,

```python
def make_multipliers():
    return [lambda x: i * x for i in range(4)]

multipliers = make_multipliers()
print([m(2) for m in multipliers])
```

8. What is the output?
9. Explain _why_ this output occurs regarding variable binding/scope.
10. Rewrite the lambda expression to fix the issue using a default argument.
    

#### B-3. Class (2%)
Consider the following code,

```python
class A:
    def foo(self): print("A", end="")
class B(A):
    def foo(self): print("B", end=""); super().foo()
class C(A):
    def foo(self): print("C", end=""); super().foo()
class D(B, C):
    def foo(self): print("D", end=""); super().foo()

d = D()
d.foo()
```

11. What is the exact string printed to stdout?
12. Write the Method Resolution Order (MRO) list for class `D`.

#### B-4. Objects (2%)
Consider the following code,

```python
class Inventory:
    items = []  # Class variable

    def add(self, item):
        self.items.append(item)

user1 = Inventory()
user2 = Inventory()
user1.add("Sword")
user2.add("Shield")
user1.items = ["Potion"]  # Shadowing?
print(f"U1: {user1.items}, U2: {user2.items}, Class: {Inventory.items}")
```

13. Analyze the state of `user1.items`, `user2.items`, and `Inventory.items` at the end of execution.
14. Explain the difference between `self.items.append(...)` and `self.items = ...` in this context.

**B-5. List (2%)**

Consider the following two snippets:
- Snippet A: `res = [x**2 for x in data if x % 2 == 0]`
- Snippet B: `res = map(lambda x: x**2, filter(lambda x: x % 2 == 0, data))`

Assuming `data` is a list of 1 million integers:
15. Which snippet is generally considered more "Pythonic" (PEP 8 style preferred)?
16. Which snippet involves the overhead of function calls for every element (in pure Python, without C-extensions)? Why?
    
**B-6. Dictionary (2%)**

Python 3.6+ dictionaries are "ordered".

17. Is this order based on key value sorting or insertion order?
18. Briefly explain the "compact dict" optimization (how keys/values are stored) compared to the sparse hash table approach used in Python 2.7.

**B-7. Decorator (2%)**
Consider the following code,

```python
def dec1(func):
    print("Init 1")
    def wrapper(*args, **kwargs):
        print("Call 1")
        return func(*args, **kwargs)
    return wrapper

def dec2(func):
    print("Init 2")
    def wrapper(*args, **kwargs):
        print("Call 2")
        return func(*args, **kwargs)
    return wrapper

@dec1
@dec2
def worker():
    print("Work")

worker()
```

19. List the printed lines in the exact order they appear during the _entire_ execution of the script.

### Part C: Code Completion (25%)

#### C-1. Magic Methods (10%)

We want to implement a "chained" path builder DSL that works as follows:

```python
p = Path("/home")
new_path = p / "user" / "docs"
print(new_path)  # Output: /home/user/docs
```

20. Implement the `Path` class.
- Implement the `__init__` method.
- Implement the specific "magic method" required to support the `/` operator. Ensure it returns a **new** `Path` instance (immutability).
- Implement the method required to provide the string representation.
    

#### C-2. Context (10%)

Create a context manager class `Suppress` that swallows specified exceptions.

```python
with Suppress(ZeroDivisionError, ValueError):
    x = 1 / 0
print("Continued!") # Should execute
```

21. Implement `__enter__`.
22. Implement `__exit__`.
- The method signature must be correct (`exc_type`, `exc_value`, `traceback`).
- Return the correct boolean value to indicate whether the exception should be suppressed or propagated.

#### C-3. Refactoring (5%)

23. Refactor the following code to adhere to PEP 8 and use modern Python idioms (f-strings, `enumerate`, context managers).

```python
# Bad Code
f = open("data.txt", "r")
i = 0
lines = f.readlines()
for line in lines:
    print("Line " + str(i) + ": " + line.strip())
    i = i + 1
f.close()
```

### Part D: Participation and Reflection (50%)

24. Briefly describe what you have learned or improved through this course.
