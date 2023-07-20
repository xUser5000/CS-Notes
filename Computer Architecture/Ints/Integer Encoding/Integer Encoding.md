# Explanation

## Types
- [[Unsigned]]
- [[Two's Complement]] (signed)

## Observations
- $|TMin| = TMax + 1$ (Asymmetric range)
- $UMax = 2 \cdot TMax + 1$
- Non-negative values have the same encoding in both schemes
- Every bit pattern represents a unique integer value and vice versa (i.e one-to-one mapping)

## Conversion
![[Conversion between Two's Complement and Unsigned.png]]

## Programming (C)
- The following constants are declared in `limits.h` to represent the extreme values of numeric types:
	- `ULONG_MAX`
	- `LONG_MAX`
	- `LONG_MIN`
	- Corresponding values for `int` ...
- Constants:
	- By default, considered two's complement (i.e signed) integers.
	- Unsigned if they have the "U" as a suffix (e.g `0U`)
- Casting:
	- Converts between the two types according to the conversion rules above (either implicit or explicit).
	- If there is a mix unsigned and signed integers in a single expression (including comparison operators), <font color="orangered">signed values implicitly cast to unsigned</font>.
	- Examples: ![[Two's Complement and unsigned casting examples.png]]

# FAQ
- When do we use unsigned integers?
	- When performing modular arithmetic
	- When using bits to represent sets

# Sources
- Extends [[Ints#Sources]]