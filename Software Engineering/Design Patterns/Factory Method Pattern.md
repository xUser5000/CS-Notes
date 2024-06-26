# Explanation
- **Factory Method** is a creational design pattern that provides an interface for creating objects in a superclass, but allows subclasses to alter the type of objects that will be created.

## Problem
- Imagine that you’re creating a logistics management application. The first version of your app can only handle transportation by trucks, so the bulk of your code lives inside the `Truck` class.
- After a while, your app becomes pretty popular. Each day you receive dozens of requests from sea transportation companies to incorporate sea logistics into the app.
- Great news, right? But how about the code? At present, most of your code is coupled to the `Truck` class. Adding `Ships` into the app would require making changes to the entire codebase. Moreover, if later you decide to add another type of transportation to the app, you will probably need to make all of these changes again.
- As a result, you will end up with pretty nasty code, riddled with conditionals that switch the app’s behavior depending on the class of transportation objects.

## Solution
- The Factory Method pattern suggests that you replace direct object construction calls (using the `new` operator) with calls to a special _factory_ method.
	- The objects are still created via the `new` operator, but it’s being called from within the factory method.
	- Objects returned by a factory method are often referred to as _products._

## UML
![[Factory Method Pattern.png]]

## Applications
- Use the Factory Method when you don’t know beforehand the exact types and dependencies of the objects your code should work with.
	- The Factory Method separates product construction code from the code that actually uses the product. Therefore it’s easier to extend the product construction code independently from the rest of the code.
	- For example, to add a new product type to the app, you’ll only need to create a new creator subclass and override the factory method in it.
- Use the Factory Method when you want to provide users of your library or framework with a way to extend its internal components.
	- Inheritance is probably the easiest way to extend the default behavior of a library or framework. But how would the framework recognize that your subclass should be used instead of a standard component?
- Use the Factory Method when you want to save system resources by reusing existing objects instead of rebuilding them each time.
	- You often experience this need when dealing with large, resource-intensive objects such as database connections, file systems, and network resources.

## Advantages
- You avoid tight coupling between the creator and the concrete products.
- _Single Responsibility Principle_. You can move the product creation code into one place in the program, making the code easier to support.
- _Open/Closed Principle_. You can introduce new types of products into the program without breaking existing client code.

## Disadvantages
- The code may become more complicated since you need to introduce a lot of new subclasses to implement the pattern. The best case scenario is when you’re introducing the pattern into an existing hierarchy of creator classes.

# Sources
- [Refactoring Guru - Factory Method](https://refactoring.guru/design-patterns/factory-method)
