# Explanation

## Task
- Given a *w*-bit integer x
- Convert it to a *w+k*-bit integer with the same value

## Solution for signed integers (two's complement)
- Make *k* copies of the sign bit (AKA **Sign Extension**): ![[Sign Extension.png]]

## Solution for unsigned integers
- Append *k* zeros to *x*

# Sources
- Extends [[Ints#Sources]]
