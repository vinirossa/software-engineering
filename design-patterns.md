# Design Patterns

## Creational Design Patterns

###  Abstract Factory

Provide an interface for creating families of related or dependent objects without specifying their concrete classes and compromising the final objects.

**Examples:**
- Kits
- Multiplatform Toolkits

###  Builder

Separates object construction from its representation

###  Factory Method

Creates an instance of several derived classes

**Examples:**
- Frameworks
- TDD

    function fabricatePerson(name, lastname) {
    	let person = {}
    	person.name = name
    	person.lastname = lastname
        
        function fullName() {
    		return `${person.name} ${person.lastname}`
    	}
    
    	return person
    }

###  Object Pool 

Avoid expensive acquisition and release of resources by recycling objects that are no longer in use

###  Prototype

A fully initialized instance to be copied or cloned

###  Singleton

A class of which only a single instance can exist

## Structural Design Patterns

### Adapter

Match interfaces of different classes

### Bridge

Separates an object’s interface from its implementation

### Composite

A tree structure of simple and composite objects

### Decorator

Add responsibilities to objects dynamically

### Facade

A single class that represents an entire subsystem

### Flyweight

A fine-grained instance used for efficient sharing

### Private Class Data

Restricts accessor/mutator access

### Proxy

An object representing another object

## Behavioral Patterns

### Chain of Responsibility

A way of passing a request between a chain of objects

### Command

Encapsulate a command request as an object

### Interpreter

A way to include language elements in a program

### Iterator

Sequentially access the elements of a collection

### Mediator

Defines simplified communication between classes

### Memento

Capture and restore an object's internal state

### Null Object

Designed to act as a default value of an object

### Observer

A way of notifying change to a number of classes

### State

Alter an object's behavior when its state changes

### Strategy

Encapsulates an algorithm inside a class

### Template Method

Defer the exact steps of an algorithm to a subclass

### Visitor

Defines a new operation to a class without change


## Other Patterns

-   Rules Design Patterns
-   Dependency Injection
-   Intercepting filter
-   Lazy loading
-   Mock object
-   Method chaining
-   Inversion of control
-   Unit of Work

## S.O.L.I.D and Clean Code
<!--stackedit_data:
eyJoaXN0b3J5IjpbMzE2OTAyNjg2LDQ2MDU1NzU4MF19
-->