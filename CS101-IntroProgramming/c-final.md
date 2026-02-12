# CS101 Introduction to Programming in C

## Final Exam

Instructor: Charlotte Shao
Session: 2026 Winter

- Duration: 3 hrs. 
- Minimal points to pass: 50%.
- Aids: Closed book. No aids (either paper or electronic) allowed. 
- Assume standard C11/C17.
- Write answers in clear English.

### Part A: Basic Concepts (10%)
#### A-1. Multiple Choice (5%)
For Questions 1–5, select **only one** option each.

1. Regarding the behaviour of `realloc(ptr, new_size)`, which of the following is TRUE when a "moving" allocation occurs? (1%)
- A) The original pointer `ptr` remains valid and points to the same data, but the capacity is extended.
- B) The function creates a new block but the developer is responsible for calling `free(ptr)` on the old block.
- C) The function may return a new address and automatically deallocate the memory at `ptr`, making all other pointers to the old block dangling.
- D) `realloc` never moves data; it only fails if the memory cannot be expanded in its current place.

2. Why is `memset` often considered dangerous or inappropriate for initializing an array of `int` to the value `1`? (1%)
- A) `memset` only works on memory allocated via `calloc`, not `malloc` or stack arrays.
- B) It operates on a byte-by-byte basis, filling each individual byte of the integer with `0x01`, which results in a value much larger than `1`.
- C) It requires a third argument that is the number of elements, but developers often pass the byte size.
- D) Standard C17 forbids using `memset` on any non-character types.

3. In the context of `qsort`, when sorting an array of strings (`char *array[]`), what is the nature of the two `void *` arguments passed to the comparison function? (1%)
- A) They are the actual character data of the strings being compared.
- B) They are the values of the pointers (the addresses where the strings start).
- C) They are pointers to the array elements, meaning they are of type "pointer to a string pointer" (`char **`).
- D) They are indices of the array elements being compared.

4. The `restrict` keyword is a contract between the programmer and the compiler. What is the specific guarantee given to the compiler, and what is the consequence of violating it? (1%)
- A) It guarantees that `p` and `q` are never NULL; violating it causes a segmentation fault.
- B) It guarantees that the object pointed to by `p` is only accessed through `p` (or pointers derived from `p`) within that scope; violating it results in Undefined Behaviour.
- C) It makes the pointer `p` read-only (immutable) similar to `const`, but enforces it at the hardware level.
- D) It guarantees that `p` points to a distinct memory page, preventing cache misses.

5. Regarding the keyword `_Noreturn`, which of the following statements is true? (1%)
- A) `_Noreturn` means the function never returns control to the caller (e.g., it calls `exit()`, `abort()`, or loops forever), allowing the compiler to remove dead code following the call.
- B) `_Noreturn` is just a deprecated alias for `void` kept for backward compatibility.
- C) `_Noreturn` forces the function to be inlined to prevent stack overflow.
- D) `void` functions can return a value if casted, while `_Noreturn` strictly forbids any `return` statement.

#### A-2. Multiple Choice (5%)
For Questions 6–10, select **all** correct answers. Each question may have 1–4 correct options. 

6. Which statement(s) are correct regarding the `sizeof` operator in the C17 standard? (1%)
- A) `sizeof` is a runtime operator that calculates the exact memory usage of an expression during program execution.
- B) `sizeof` is evaluated at compile-time for ALL data types.
- C) If the expression inside `sizeof` has side effects (e.g., `sizeof(i++)`), the compiler ensures the expression is executed before calculating the size.
- D) `sizeof` can be applied to built-in data type names (like `int`), variable names or complex expressions.
- E) None of above.
    
 7. Which statement(s) correctly describe the `&` (address-of) operator and `void*` pointers? (1%)
- A) An array name (e.g., `arr`) and the address of that array (e.g., `&arr`) are identical in numerical value.
- B) A `void*` pointer can be directly dereferenced (e.g., `*ptr`) to retrieve data without an explicit type cast.
- C) `&arr + 1` skips the size of the entire array named `arr`.
- D) `void*` pointers are allowed to perform pointer arithmetic (like `+` or `-`) by defaulting the step size to 0.
- E) None of above.

8. Regarding memory alignment and the `aligned_alloc` function, which of the following statement(s) are true? (1%)
- A) Standard `malloc` is guaranteed to return a pointer that is suitably aligned for any built-in standard type (e.g., `long double`, `max_align_t`).
- B) `aligned_alloc(alignment, size)` requires that the requested `size` be an exact integral multiple of the `alignment`; otherwise, the call may fail or invoke undefined behavior.
- C) `aligned_alloc` is strictly used for performance optimization (e.g., SIMD instructions) and has no impact on program correctness; unaligned access on all modern hardware is safe but slow.
- D) The `_Alignas` specifier can be applied to `struct` definitions to force a stricter alignment requirement than the sum of its members would imply.
- E) None of above.
    
9. Which of the following statement(s) correctly describe the behavior of the `_Generic` keyword? (1%)
- A) It performs runtime type checking (RTTI), allowing the program to choose a function execution path based on the dynamic type of a variable.
- B) It is a compile-time mechanism where the compiler selects one expression based on the static type of the controlling expression and discards the others.
- C) The expressions in the non-selected associations of a `_Generic` selection are unevaluated operands, meaning any side effects within them (like function calls or increments) will not occur.
- D) `_Generic` treats `typedef` names as distinct types; for example, it can distinguish between `int` and `int32_t` even if `int32_t` is defined as `int` on that platform.
- E) None of above.
    
10. Regarding`_Atomic` and `volatile`, which of the following statement(s) are true? (1%)
- A) The C11 standard formally defines a "Data Race" (two threads accessing the same memory location concurrently where at least one is a writer) as Undefined Behavior.
- B) The `volatile` keyword, when applied to a global variable, is sufficient to guarantee thread safety because it forces the compiler to generate load/store instructions to memory every time the variable is accessed.
- C) Operations on `_Atomic` types are guaranteed to be indivisible and free from data races, but they may impose performance penalties due to the generation of memory barriers or lock instructions.
- D) The `_Atomic` type qualifier is a built-in keyword that can be used without including any headers, whereas `atomic_int` is a convenience typedef macro found in `<stdatomic.h>`.

### Part B: Code Analysis (25%)
#### B-1. Declaration (5%)
Consider the following snippet:
```C
struct Point { int x, y; };

struct Point a;
struct Point b = {0};
struct Point c();
static struct Point d;
struct Point e = {.y = 5};
```
11. For each line, state (1) whether it declares a variable or a function, and (2) the initial value of the entity (if a variable). (5%)
#### B-2. Data Types (10%)
Consider the following snippet. Assume a 64-bit architecture where `char` = 1 byte, `short` = 2 bytes, `int` = 4 bytes, `long` = 8 bytes, `double` = 8 bytes, and pointers = 8 bytes. Alignment rules follow standard natural alignment.
```C
char s1[] = "Hello";
char *s2 = "World";

void func(char p[100]) {
    int r1 = sizeof(p); 
    int r2 = sizeof("Hello");
    int r3 = sizeof(s1);
    int r4 = sizeof(s2);
    int r5 = sizeof(s1 + 1);
    int r6 = sizeof(*s2);
}
```
12. Write the values for `r1` to `r6` in the following snippet. (6%)

Consider the following snippet. Assume the variable `arr` starts at memory address `0x1000`. 
```C
long arr[5] = {10, 20, 30, 40, 50}; 
long *p = arr;
long (*q)[5] = &arr;
```
13. Calculate the resulting memory address (in hex) for the following expressions: (1) `p + 1`; (2) `q + 1`; (3) `(char*)p + 1`; (4) `&3[arr]`. (4%)
#### B-3. Memory Management (5%)
Assume `sizeof(int)` is 4, `sizeof(long)` is 8, `sizeof(char*)`is 8. Consider the following snippet:
```C
struct Blob {
    int id;
    long flags;
    char *buffer;
};

void process_blob() {
    struct Blob b1;
    b1.id = 1;
    b1.buffer = malloc(128);
    
    struct Blob b2 = b1;
    
    free(b1.buffer);
    free(b2.buffer);
}
```

14. Determine the exact value of `sizeof(struct Blob)` on a 64-bit system. Explain why it might differ from the sum of its members' sizes. Justify your answer with proper use of concepts. (1%)
15. Explain the runtime behaviour of the function `process_blob()`. Justify your answer with proper use of concepts. (1%)

Consider the following snippet:
```C
int* f1() {
    int x = 10;
    return &x;
}

void f2() {
    char *src = "Beta";
    char dest[4]; 
    strcpy(dest, src);
}

void f3() {
    int *p = malloc(sizeof(int) * 10);
    free(p);
    *p = 5; 
}
```
16. For each function: (i) compiles? (ii) safe? (iii) why? (3%, 1% for each function)
#### B-4. Optimization (2%)
For Questions 17–18, select **only one** option each.

Consider the following snippet.
```C
void process_buffer(int data[static 1024]);
```
 17. What does the keyword `static` mean in the following function prototype?
- A) The array `data` is allocated in static memory (global scope), not on the stack.
- B) The function `process_buffer` maintains the state of the `data` array across multiple calls (like a static local variable).
- C) It is a promise to the compiler that the pointer `data` is not NULL and points to at least 1024 valid integers for prefetching.
- D) It restricts the visibility of the `data` argument to the file where the function is defined.

Consider the following struct declaration. 
```C
struct Packet {
    int id;
    int length;
    char payload[];
};
```
18. Which statement is true?
- A) This declaration leads to compile error.
- B) `char payload[]` contributes zero bytes to the size of the structure as calculated by `sizeof`. 
- C) When a new instance of `struct Packet` is declared, the compiler allocates a single contiguous block of memory large enough to hold both the structure and the array elements automatically.
- D) The positions of the three members of this struct in the declaration are interchangeable.
#### B-5: Comprehensive Analysis (3%)
Consider the following snippet.
```C
#include <stdio.h>
void greetings(int flag) {
    char hello[] = "hello";
    char *world = "world";
    char *p = flag ? hello : world; 
    
    p[0] -= 32;
    printf("%s", p);
}

int main() {
    greetings(1); // Case 1
    greetings(0); // Case 2
}
```
19. What is the behaviour and/or output of `main()`? Why? (1%)

Consider the following snippet.
```C
#include <stdio.h>
#define SQUARE(x) x * x
#define MAX(a, b) ((a) > (b) ? (a) : (b))

int main() {
    int a = 3;
    int b = SQUARE(a + 1); 
    
    int i = 5, j = 10;
    int k = MAX(i++, j++); 
    
    printf("b = %d, k = %d, j = %d\n", b, k, j);
    return 0;
}
```
20. What is the behaviour and/or output of `main()`? Why? (1%)

Consider the following snippet.
```C
#include <stdio.h>

void print_int(int x) { printf("INT: %d\n", x); }
void print_float(float x) { printf("FLOAT: %.1f\n", x); }
void print_default(void) { printf("UNKNOWN\n"); }

#define PRINT(x) _Generic((x), \
    int: print_int, \
    float: print_float, \
    default: print_default \
)(x)

int main() {
    PRINT(10);
    PRINT(3.14);
    PRINT('a');
    return 0;
}
```
21. What is the behaviour and/or output of `main()`? Why? (1%)

### Part C: Code Completion (15%)
22. The output of the following code is: `Test 'x == 100' failed!` Complete the macro definition of `TEST_CASE` and `ASSERT_TRUE`. (5%)
```C
#define TEST_CASE(name) ____________________
#define ASSERT_TRUE(cond) ____________________

TEST_CASE(math_check) {
    int x = 5;
    ASSERT_TRUE(x > 0);
    ASSERT_TRUE(x == 100);
}

int main() {
    math_check();
    return 0;
}
```

23. Complete the code. It should compile the same way as `enum Color { RED, GREEN, BLUE }; char *ColorNames[] = { "RED", "GREEN", "BLUE" };`. (5%)
```C
#define COLOR_TABLE \
    COLOR(RED)      \
    COLOR(GREEN)    \
    COLOR(BLUE)

#____________________
enum Color {
    COLOR_TABLE
};
#____________________

#____________________
char *ColorNames[] = {
    COLOR_TABLE
};
#____________________

```

24. Complete the code. It should print "DEBUG: This is a log." when and only when being compiled with `-DDEBUG` option (i.e., `DEBUG` is defined). (5%)
```C
#include <stdio.h>

#ifdef DEBUG
___________________
___________________
___________________
#endif

int main() {
    LOG("This is a log.");
    return 0;
}
```

### Part D: Participation and Reflection (50%)
25. Briefly describe what you have learned or improved through this course.
