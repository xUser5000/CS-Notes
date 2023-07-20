# Explanation
- Mathematical Representation: $$B2T(X) = \sum_{i=0}^{w-2}x_i \cdot 2^i + -x_{w-1} \cdot 2^{w-1}$$, where $X$ is a bit vector and $W$ is the word size

- The bit number $w-1$ (i.e the leftmost bit) is the called the **sign bit**:
	- 0 for non-negative
	- 1 for negative
- $TMin = (-2^{w - 1})_{10} = (100...0)_{2}$
- $TMax = (2^{w-1} - 1)_{10} = (011...1)_{2}$
- $(-1)_{10} = (111...1)_2$

# Sources
- Extends [[Ints#Sources]]