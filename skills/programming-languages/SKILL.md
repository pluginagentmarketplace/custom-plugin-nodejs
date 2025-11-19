---
name: programming-languages
description: Master 10+ programming languages including JavaScript, Python, Go, Rust, Java. Learn syntax, paradigms, and best practices. Use when learning language fundamentals or choosing the right language for a problem.
---

# Programming Languages Skill

Master multiple programming languages and understand their unique characteristics, use cases, and paradigms.

## Quick Start

Learn language fundamentals:
1. **Syntax & Grammar** - How to write valid code
2. **Data Types & Structures** - Primitive and complex types
3. **Control Flow** - If/else, loops, conditionals
4. **Functions & Scope** - Function definition and scope rules
5. **Object-Oriented vs Functional** - Programming paradigms

## Popular Languages by Category

### Dynamic Languages (Flexible, Great for Beginners)
- **JavaScript/TypeScript** - Web development standard
- **Python** - Data science, automation, ML
- **Ruby** - Web development (Rails)

### Compiled Languages (Fast, Type-Safe)
- **Go** - Concurrent systems, DevOps
- **Rust** - Systems programming, memory safety
- **Java** - Enterprise, Android, large systems
- **C++** - Performance-critical applications
- **C#** - Windows development, game dev (Unity)

### Functional Languages
- **Lisp/Clojure** - Functional paradigm
- **Haskell** - Pure functional
- **Scala** - JVM with functional support

## Language Comparison Matrix

| Language | Ease | Performance | Use Case | Job Market |
|----------|------|-------------|----------|-----------|
| **JavaScript** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | Web (Frontend/Backend) | Excellent |
| **Python** | ⭐⭐⭐⭐⭐ | ⭐⭐ | Data Science, Automation | Excellent |
| **Go** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | Backend, DevOps, Concurrency | Very Good |
| **Rust** | ⭐⭐ | ⭐⭐⭐⭐⭐ | Systems, Performance | Growing |
| **Java** | ⭐⭐⭐ | ⭐⭐⭐⭐ | Enterprise, Large Systems | Excellent |
| **C++** | ⭐ | ⭐⭐⭐⭐⭐ | Game Engines, OS, Performance | Good |
| **Ruby** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | Web Development (Rails) | Good |
| **PHP** | ⭐⭐⭐⭐ | ⭐⭐⭐ | Web Development (WordPress) | Good |

## Learning Path by Language

### JavaScript/TypeScript (3-6 months)
1. **Basics** (2 weeks)
   - Variables, data types, operators
   - Control flow, loops
   - Functions and scope

2. **Intermediate** (4 weeks)
   - Objects and arrays
   - Callbacks, promises, async/await
   - ES6+ features

3. **Advanced** (8 weeks)
   - Prototypes and inheritance
   - Closures and higher-order functions
   - TypeScript type system

### Python (3-6 months)
1. **Basics** (2 weeks)
   - Syntax, variables, types
   - Lists, tuples, dictionaries
   - Control flow

2. **Intermediate** (4 weeks)
   - Functions and modules
   - Object-oriented programming
   - File I/O and regular expressions

3. **Advanced** (8 weeks)
   - Decorators and generators
   - Context managers
   - Performance optimization

### Go (2-4 months)
1. **Basics** (1 week)
   - Syntax, goroutines
   - Channels, concurrency
   - Basic types

2. **Intermediate** (2 weeks)
   - Interfaces and composition
   - Error handling
   - Testing

3. **Advanced** (4 weeks)
   - Advanced concurrency patterns
   - Performance optimization
   - Systems programming

## Core Concepts Across Languages

### Variables & Types
```javascript
// JavaScript - Dynamic typing
let age = 25;
let name = "Alice";

// TypeScript - Static typing
let age: number = 25;
let name: string = "Alice";

// Go - Explicit typing
var age int = 25
var name string = "Alice"

// Rust - Immutable by default
let age: u32 = 25;
let mut count = 0;
```

### Functions
```javascript
// JavaScript
function greet(name) { return `Hello, ${name}!`; }

// TypeScript
function greet(name: string): string { return `Hello, ${name}!`; }

// Go
func greet(name string) string { return "Hello, " + name + "!" }

// Python
def greet(name: str) -> str: return f"Hello, {name}!"
```

### Control Flow
```javascript
// All languages have similar patterns
if (age >= 18) {
    console.log("Adult");
} else if (age >= 13) {
    console.log("Teen");
} else {
    console.log("Child");
}

for (let i = 0; i < 10; i++) { }
while (condition) { }
```

### Object-Oriented Programming
```javascript
// JavaScript Classes
class Person {
    constructor(name) { this.name = name; }
    greet() { return `Hello, I'm ${this.name}`; }
}

// Python Classes
class Person:
    def __init__(self, name):
        self.name = name
    def greet(self):
        return f"Hello, I'm {self.name}"

// Go Interfaces
type Person interface {
    Greet() string
}
```

## Best Practices by Language

### JavaScript/TypeScript
- Use const by default, let for variables
- Prefer arrow functions for callbacks
- Use async/await instead of promises
- Enable strict mode
- Use TypeScript for large projects

### Python
- Follow PEP 8 style guide
- Use virtual environments
- Type hints for complex code
- Avoid mutable default arguments
- Use context managers (with statements)

### Go
- Keep functions small and focused
- Handle errors explicitly
- Use interfaces for abstraction
- Leverage goroutines for concurrency
- Follow naming conventions strictly

### Rust
- Understand ownership and borrowing
- Use pattern matching
- Leverage the type system
- Avoid unwrap in production code
- Use Result type for error handling

## Language Selection Guide

### Choose JavaScript/TypeScript if...
- Building web applications
- Want single language for frontend/backend
- Need rapid development
- Large ecosystem of libraries

### Choose Python if...
- Data science or machine learning
- System automation
- Rapid prototyping
- Beginner-friendly learning

### Choose Go if...
- Building distributed systems
- Need high concurrency
- DevOps or infrastructure tools
- Performance is critical

### Choose Rust if...
- Systems programming needed
- Memory safety is critical
- Performance and reliability required
- Learning low-level concepts

## Resources for Each Language

**JavaScript/TypeScript**
- MDN Web Docs
- JavaScript.info
- TypeScript Handbook

**Python**
- Real Python
- Python Official Docs
- Automate the Boring Stuff

**Go**
- Go Official Tour
- Go by Example
- Effective Go

**Rust**
- The Rust Book
- Rust by Example
- Rustlings exercises

## Common Mistakes to Avoid

- ❌ Learning too many languages at once
- ❌ Skipping fundamentals for frameworks
- ❌ Not practicing enough
- ❌ Ignoring language-specific idioms
- ❌ Copying code without understanding

## Practice Exercises

1. **FizzBuzz** - Control flow basics
2. **Fibonacci** - Recursion understanding
3. **Palindrome Check** - String manipulation
4. **Binary Search** - Algorithm implementation
5. **Grade Calculator** - Functions and conditionals
6. **Word Frequency** - Data structures
7. **File Parser** - File I/O and processing

## When You Know a Language Well

✅ Can write idiomatic code
✅ Understand language quirks
✅ Know performance characteristics
✅ Can teach others basics
✅ Comfortable with standard library
✅ Follow style conventions
✅ Write clean, readable code
✅ Debug effectively
