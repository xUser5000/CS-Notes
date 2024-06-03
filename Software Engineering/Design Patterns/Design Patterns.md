# Explanation
- *Design patterns*: typical solutions to commonly occurring problems in software design.
- They are like pre-made blueprints that you can customize to solve a recurring design problem in your code.
- You can’t just find a pattern and copy it into your program, the way you can with off-the-shelf functions or libraries.
- The pattern is not a specific piece of code, but a general concept for solving a particular problem.
- You can follow the pattern details and implement a solution that suits the realities of your own program.

## Patterns vs Algorithms
- Patterns are often confused with algorithms, because both concepts describe typical solutions to some known problems.
- While an algorithm always defines a clear set of actions that can achieve some goal, a pattern is a more high-level description of a solution.
- The code of the same pattern applied to two different programs may be different.
- An analogy to an algorithm is a cooking recipe: both have clear steps to achieve a goal.
- On the other hand, a pattern is more like a blueprint: you can see what the result and its features are, but the exact order of implementation is up to you.

## What does the pattern consist of?
- Most patterns are described very formally so people can reproduce them in many contexts. Here are the sections that are usually present in a pattern description:
	- **Intent** of the pattern briefly describes both the problem and the solution.
	- **Motivation** further explains the problem and the solution the pattern makes possible.
	- **Structure** of classes shows each part of the pattern and how they are related.
	- **Code example** in one of the popular programming languages makes it easier to grasp the idea behind the pattern.
- Some pattern catalogs list other useful details, such as applicability of the pattern, implementation steps and relations with other patterns.

## Why should we learn patterns?
- Design patterns are a toolkit of **tried and tested solutions** to common problems in software design.
	- Even if you never encounter these problems, knowing patterns is still useful because it teaches you how to solve all sorts of problems using principles of object-oriented design.  
- Design patterns define a common language that you and your teammates can use to communicate more efficiently.
	- You can say, “Oh, just use a Singleton for that,” and everyone will understand the idea behind your suggestion.
	- No need to explain what a [[Singleton Pattern|singleton]] is if you know the pattern and its name.

## Criticism of patterns

### 1. Kludges for a weak programming language
- Usually the need for patterns arises when people choose a programming language or a technology that lacks the necessary level of abstraction.
- In this case, patterns become a kludge that gives the language much-needed super-abilities.
- For example, the [[Strategy Pattern|Strategy]] pattern can be implemented with a simple anonymous (lambda) function in most modern programming languages.

### 2. Inefficient solutions
- Patterns try to systematize approaches that are already widely used. This unification is viewed by many as a dogma, and they implement patterns “to the letter”, without adapting them to the context of their project.

### 3. Unjustified use
```
If all you have is a hammer, everything looks like a nail.
```

- This is the problem that haunts many novices who have just familiarized themselves with patterns: having learned about patterns, they try to apply them everywhere, even in situations where simpler code would do just fine.

## Classification of Patterns
- All patterns can be categorized by their _intent_, or purpose. There are three main groups of patterns:
	- **Creational patterns** provide object creation mechanisms that increase flexibility and reuse of existing code.
		- [[Factory Method Pattern]]
		- [[Abstract Factory Pattern]]
		- [[Singleton Pattern]]
	- **Structural patterns** explain how to assemble objects and classes into larger structures, while keeping these structures flexible and efficient.
	- **Behavioral patterns** take care of effective communication and the assignment of responsibilities between objects.

# Sources
- [Refactoring Guru - What's a design pattern](https://refactoring.guru/design-patterns/what-is-pattern)
- [Refactoring Guru - Why learn patterns](https://refactoring.guru/design-patterns/why-learn-patterns)
- [Refactoring Guru - Criticism of patterns](https://refactoring.guru/design-patterns/criticism)
- [Refactoring Guru - Classification of patterns](https://refactoring.guru/design-patterns/classification)
