# Explanation

## Optimizing compiler
- It's a compiler that tries to minimize or maximize some attributes of an executable computer program. [1]

###  Features
- Provide efficient mapping between source code and machine code
- Eliminate dead code
- Eliminate minor inefficiencies
- Source: [2]

### Limitations
- Don't usually improve asymptotic complexity
- Have difficulty overcoming [[Optimization blockers]]:
	- [[Memory aliasing]]
	- [[Procedure calling]]
- Analysis is based on static information
	- The compiler has difficulty anticipating runtime inputs
- When in doubt, the compiler will be conservative.
- Source: [2]

### Examples
- GCC

# Sources
1. [Wikipedia - Optimizing Compiler](https://en.wikipedia.org/wiki/Optimizing_compiler)
2. [CMU Introduction to Computer Systems - Lecture 10](https://scs.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=4b1da67c-2980-4b96-82e7-2f99139a2c0d)