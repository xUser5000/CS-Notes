# Explanation
- It's the operation of shifting the bits of a bit vector a number of positions either to the left or to the right.

## Types

### Left Shift
- Syntax: `x << y`
- Properties:
	- Throw away extra bits on left
	- Fill with 0’s on right

### Right Shift
- Syntax: `x >> y`
- Properties:
	- Throw away extra bits on right
	- Types:
		- **Logical Shift**: Fill with 0’s on left
		- **Arithmetic Shift**: Replicate most significant bit on left (to preserve the sign)

# FAQ
- What happens if `y` (the shift amount) is less than 0 or greater than `w` (the word size)?
	- Generally, causes an undefined behavior.
	- On most machines, it would produce the same integer `x`.

# Sources
- Extends [[Ints#Sources]]