**Prototype** is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.
The Prototype pattern delegates the cloning process to the actual objects that are being cloned.
The pattern declares a common interface for all objects that support cloning.
An object that supports cloning is called a _prototype_.
**Solves**
- How can objects be created so that which objects to create can be specified at run-time?
- How can dynamically loaded classes be instantiated?
**Applicability**
- Use the Prototype pattern when your code shouldn’t depend on the concrete classes of objects that you need to copy.
**Pros**
- You can clone objects without coupling to their concrete classes.
**Cons**
- Cloning complex objects that have circular references might be very tricky.
# References

> [!info] Prototype  
> Prototype is a creational design pattern that lets you copy existing objects without making your code dependent on their classes.  
> [https://refactoring.guru/design-patterns/prototype](https://refactoring.guru/design-patterns/prototype)  

> [!info] Wikiwand - Prototype pattern  
> The prototype pattern is a creational design pattern in software development.  
> [https://www.wikiwand.com/en/Prototype_pattern](https://www.wikiwand.com/en/Prototype_pattern)