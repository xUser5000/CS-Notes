# Explanation
- A way to implement [[Lock]]s.

## Mechanism
![[Code for Controlling Interrupts.png]]

## Advantages
- Simplicity

# Disadvantages
- Allows user programs to perform privileged instructions.
- Doesn't work on multi-processor systems.
- Turning off interrupts for extended periods of time can lead to interrupts becoming lost.
- Inefficient as code that masks or unmasks interrupts tend to be quite slow in modern systems.

# Sources
- Extends [[Lock#Sources]]