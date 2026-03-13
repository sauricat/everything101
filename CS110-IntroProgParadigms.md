# CS110 Introduction to Programming Paradigms

Instructor: Charlotte Shao

Programming paradigms are the styles and structures of abstraction on top of the implementations of computer programs. They often manifest in the design of programming languages. In this course, you will navigate through different programming paradigms, forming a holistic view of the syntactic differences between paradigms with an awareness of their respective applicable scenarios.

The methodology of this course is to challenge the pre-existing, dogmatic view of the difference between so-called "objective-oriented" and "functional" programming paradigms, and to provide new perspectives to understand the categories behind them.

We will discuss the following topics:
1. States and Automata
   - Extended Reading: Michael Sipser, _Introduction to the Theory of Computation_, Part One.
   - Language: natural language, pseudocode, etc.
   - Project: Simulation of a mechanical vending machine with manual management of the state transitions using explicit variables.
2. Declarations and Statements
   - Extended Reading: Peter J. Landin, _The Next 700 Programming Languages_.
   - Language: SQL.
   - Project: A simple data-quering script. First, implement it imperatively (telling the computer how to iterate through data to find a result). Then, write it declaratively (describing what the desired result is), highlighting the difference in cognitive load.
3. Labels, Blocks, Gotos, and Branches
   - Extended Reading: Edsger W. Dijkstra, _Goto Statement Considered Harmful_.
   - Language: BASIC or Assembly.
   - Project: Implement a text-based "Choose Your Own Adventure" game using only `GOTO` and labels for control flow. The goal is to let the code become "spaghetti" and hard-to-read.
4. Control Flow and Structured Programming
   - Extended Reading:  O.-J. Dahl, E. W. Dijkstra, and C. A. R. Hoare, _Structured Programming_ (selected chapters).
   - Language: BASIC or C.
   - Project: Refactor the text adventure game from the previous module. Remove all `GOTO` statements and replace them entirely with strict structured control flow (`if/else`, `while` loops, and proper block scoping).
5. Functions, Closures, and Effects
   - Extended Reading: John Hughes, _Why Functional Programming Matters_.
   - Language: Haskell.
   - Project: A text-processing utility (like a word frequency counter) using pure functions, `map`, `filter`, and `reduce`. State must be passed explicitly, demonstrating how closures capture environment without relying on global variables.
6. Classes, Objects, Methods, and Messages
   - Extended Reading: Dan Ingalls, _Design Principles Behind Smalltalk_.
   - Language: Pharo.
   - Project: A simulation of a simple ecosystem (e.g., a predator-prey grid). Focus entirely on message-passing: objects should not manipulate each other's internal states directly, but rather send messages requesting actions.
7. Types, Traits, Patterns, and Prototypes
   - Extended Reading: Benjamin C. Pierce, _Types and Programming Languages_ (selected chapters). David Ungar and Randall B. Smith, _SELF: The Power of Simplicity_.
   - Language: Rust (first-half); Lua or ECMAscript (second-half).
   - Project: A two-part shape-drawing API. First, use Lua to build a hierarchy of shapes using prototype delegation. Second, use Rust to implement a similar API using strict traits and pattern matching to handle different shape behaviors.
8. Facts, Rules, and Relations
   - Extended Reading: Leon Sterling and Ehud Shapiro, _The Art of Prolog_.
   - Language: Prolog.
   - Project: A simple logic-puzzle solver. A classic application is feeding the system a set of familial relations (parent, sibling) and writing rules to let the language autonomously deduce complex relations (e.g., second cousin once removed).
9. Macros and Homoiconicity
   - Extended Reading: Paul Graham, _The Roots of Lisp_.
   - Language: Common Lisp or Clojure.
   - Project: A simple domain-specific language (DSL) that creates a new, custom control structure that doesn't natively exist in the language. 
10. Systems, Runtimes, and Oracles
   - Extended Reading: Joe Armstrong, _Making reliable distributed systems in the presence of software errors_.
   - Language: Erlang or Elixir.
   - Project: A simple VM with garbage collection and supervisor tree.
