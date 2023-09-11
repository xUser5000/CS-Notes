# Explanation

## Format
$$b = \sum^{m}_{i=-n}2^i \times b_i$$
- $b_i$ is the $i$-th bit
- $m-1$ is the number of digits to the left of the binary point
- $n$ is the number of digits to the right of the binary point.

## Encoding
![[Encoding of Fractional Binary Numbers.png]]

## Properties
- Shifting one position to the left has the effect of multiplying by 2
- Shifting one position to the right has the effect of dividing by 2
- A bit pattern of (000...0) represents zero

## Limitations
- Can only exactly represent numbers in the form $x / 2^k$
- Limited range of numbers

# Sources
- Extends [[Floats#Sources]]