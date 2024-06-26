# Explanation
- **Proxy** is a structural design pattern that lets you provide a substitute or placeholder for another object. A proxy controls access to the original object, allowing you to perform something either before or after the request gets through to the original object.

## Problem
- Image that you have a massive object that consumes a vast amount of system resources. You need it from time to time, but not always.
- You could implement lazy initialization: create this object only when it’s actually needed. All of the object’s clients would need to execute some deferred initialization code. Unfortunately, this would probably cause a lot of code duplication.
- In an ideal world, we’d want to put this code directly into our object’s class, but that isn’t always possible. For instance, the class may be part of a closed 3rd-party library.

## Solution
- The Proxy pattern suggests that you create a new proxy class with the same interface as an original service object. Then you update your app so that it passes the proxy object to all of the original object’s clients. Upon receiving a request from a client, the proxy creates a real service object and delegates all the work to it.
- But what’s the benefit? If you need to execute something either before or after the primary logic of the class, the proxy lets you do this without changing that class. Since the proxy implements the same interface as the original class, it can be passed to any client that expects a real service object.

## UML
![[Proxy Pattern.png]]

## Applications
- **Lazy initialization (virtual proxy)**: This is when you have a heavyweight service object that wastes system resources by being always up, even though you only need it from time to time.
- **Access control (protection proxy)**: This is when you want only specific clients to be able to use the service object; for instance, when your objects are crucial parts of an operating system and clients are various launched applications (including malicious ones).
- **Local execution of a remote service (remote proxy)**: This is when the service object is located on a remote server.
- **Logging requests (logging proxy)**: This is when you want to keep a history of requests to the service object.
- **Caching request results (caching proxy)**: This is when you need to cache results of client requests and manage the life cycle of this cache, especially if results are quite large.
- **Smart reference**: This is when you need to be able to dismiss a heavyweight object once there are no clients that use it.

## Advantages
- You can control the service object without clients knowing about it.
- You can manage the lifecycle of the service object when clients don’t care about it.
- The proxy works even if the service object isn’t ready or is not available.
- _Open/Closed Principle_. You can introduce new proxies without changing the service or clients.

## Disadvantages
- The code may become more complicated since you need to introduce a lot of new classes.
- The response from the service might get delayed.

# Sources
- [Refactoring Guru - Proxy Pattern](https://refactoring.guru/design-patterns/proxy)