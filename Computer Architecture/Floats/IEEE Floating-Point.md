# Explanation

## Format
$$v = (-1)^s \times M \times 2^E$$
- $s$ is the sign bit (1 means negative and 0 means positive)
- The *signiﬁcand* $M$ is a fractional binary number that ranges either between 1 and 2 − $\epsilon$ or between 0 and 1 − $\epsilon$
- The exponent $E$ weights the value by a (possibly negative) power of 2

## Encoding
![[Encoding of IEEE floating-point numbers.png]]
- The single sign bit $s$ directly encodes the sign $s$.
- The $k$-bit exponent ﬁeld *exp* encodes the exponent $E$.
- The $n$-bit fraction ﬁeld *frac* encodes the signiﬁcand $M$, but the value encoded also depends on whether or not the exponent ﬁeld equals 0.

## Decoding

### Case 1: Normalized Values
- Condition: *exp* $\neq 000 \ldots 0$ and *exp* $\neq 111\ldots1$
- $E = Exp - Bias$
	- $Exp$ is the unsigned value of the *exp* field 
	- $Bias = 2^{k-1} - 1$, where $k$ is the number of exponent bits
- Significand coded with implied leading $1$; $M = (1.xxx \ldots x)_2$
	- $xxx \ldots x$ is bits of the *frac* field
	- Minimum when *frac* $= 000 \ldots 0$ ($M = 1.0$)
	- Maximum when *frac* $= 111 \ldots 1$ ($M = 2.0 - \epsilon$)

### Case 2: Denormalized Values
- Condition: *exp* $= 000 \ldots 0$
- $E = 1 – Bias$ (instead of $E = 0 – Bias$)
- Significand coded with implied leading $0$; $M = (0.xxx \ldots x)_2$
	- $xxx \ldots x$ is bits of the *frac* field

#### Case 2.1
- Condition: *frac* $= 000 \ldots 0$
- Represents zero value
- Note: distinct values of +0 and –0

#### Case 2.2
- Condition: *frac* $\neq 000 \ldots 0$
- Numbers closest to 0.0
- Equispaced

### Case 3: Special Values
- Condition: *exp* = $111\ldots1$

#### Case 3.1
- Condition: *frac* $= 000 \ldots 0$
- Represents value $\infty$ (infinity)
- Operation that overflows
- Both positive and negative
- Examples:
	- $1.0/0.0 = −1.0/−0.0 = +\infty$
	- $1.0/−0.0 = −\infty$

#### Case 3.2
- Condition: *frac* $\neq 000 \ldots 0$
- Not-a-Number (NaN)
- Represents case when no numeric value can be determined
- Examples:
	- $\sqrt{–1}$
	- $\infty - \infty$
	- $\infty \times 0$

## Visualization
![[Visualization of IEEE floating-point encoding.png]]

## Properties
- A floating-point zero is encoded the same way as an integer zero
- Can (almost) use unsigned integer comparison
- Numbers with the same *exp* field are equispaced in the number line

## Operations
- [[IEEE Floating-Point Addition]]
- [[IEEE Floating-Point Multiplication]]

## Conversion/Casting
- Casting between int, float, and double changes bit representation
- double/float --> int:
	- Truncates fractional part
	- Like rounding toward zero
	- Not defined when out of range or NaN: Generally sets to TMin
- int --> double: exact conversion, as long as int has $\le 53$ bit word size
- int --> float: will round according to rounding mode

# Sources
- Extends [[Floats#Sources]]
