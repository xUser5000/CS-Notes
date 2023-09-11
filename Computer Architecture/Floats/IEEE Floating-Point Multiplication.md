# Explanation
- Floating-point multiplication has the following properties:
	- Closed under multiplication, but may generate infinity or NaN
	- Commutative
	- NOT Associative because of overflow and inexactness of rounding
		- `(1e20 * 1e20) * 1e-20 = inf`
		- `1e20 * (1e20 * 1e-20) = 1e20`
	- $1$ is the multiplicative identity
	- Multiplication DOES NOT distribute over addition
	- Monotonic except for *NAN*s and infinities
		- $a \ge b \space \& \space c \ge 0 \implies a \times c \ge b \times c$

# Sources
- Extends [[Floats#Sources]]