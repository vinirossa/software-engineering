# Design Patterns

## Creational Design Patterns

###  Factory Method

Creates an instance of several derived classes

**Uses:**
- Frameworks
- TDD

**Example:**

    function fabricatePerson(name, lastname) {
        let person = {}
        person.name = name
        person.lastname = lastname
            
        function fullName() {
    	    return `${person.name} ${person.lastname}`
        }

		person.fullName
        
        return person
    }

###  Abstract Factory

Provide an interface for creating families of related or dependent objects without specifying their concrete classes and compromising the final objects.

**Uses:**
- Kits
- Multiplatform Toolkits

**Examples:**

    class Animal {}

	class Bird extends Animal {}

	class Reptile extends Animal {}
	
	class Hawk extends Bird {}

	class Alligator extends Reptile {}

###  Builder

Separates object construction from its representation. 

A `Builder` abstract class or interface and one `ConcreteBuilder` for each variation of the product.

**Uses:**
- Text Editors
- Templates

###  Prototype

A fully initialized instance to be copied or cloned

###  Singleton

A class of which only a single instance can exist. 

All singletons have a private constructor (`createInstance`), a public access method (`getInstance`) and a static attribute (`instance`).

**Uses:**
- Database Configuration

**Example:**

    const Singleton = (() => {
        var instance
    
        function createInstance() {
            var object = new Object("I am the instance")
            return object
        }
    
        return {
            getInstance: () => {
                if (!instance) instance = createInstance()
                return instance
        	}
        }
    })()

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

-   **Rules Design Patterns**
-   **Dependency Injection**
-   **Intercepting filter**
-   **Lazy loading**
-   **Mock object**
-   **Method chaining**
-   **Inversion of control**
-   **Unit of Work**
-   **Object Pool:** avoid expensive acquisition and release of resources by recycling objects that are no longer in use
- **Null Object:** designed to act as a default value of an object

## S.O.L.I.D and Clean Code
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE0NjMyODg2ODEsNDYwNTU3NTgwXX0=
-->