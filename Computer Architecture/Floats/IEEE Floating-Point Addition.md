# Explanation
- Floating-point addition has the following properties:
	- Closed under addition
	- Commutative
	- Not Associative because of overflow and inexactness of rounding
		- `(3.14+1e10)-1e10 = 0`
		- `3.14+(1e10-1e10) = 3.14`
	- $0$ is the additive identity
	- Every element has an additive inverse except for *NAN*s and infinities
	- Monotonic except for *NAN*s and infinities
		- $a \ge b \implies a+c \ge b+c$

# Sources
- Extends [[Floats#Sources]]