# Design Patterns

## Creational Design Patterns

###  Abstract Factory

Provide an interface for creating families of related or dependent objects without specifying their concrete classes and compromising the final objects.

**Uses:**
- Kits
- Multiplatform Toolkits

**Examples:**

    abstract class Animal {}

	abstract class Bird extends Animal {}

	abstract class Reptile extends Animal {}
	
	class Hawk extends Bird {}

	class Alligator extends Reptile {}

###  Builder

Separates object construction from its representation. 

A `Builder` abstract class or interface and one `ConcreteBuilder` for each variation of the product.

**Uses:**
- Text Editors
- Templates

**Example:**

    var Task = (name, description, finished, dueDate) => {
    
        this.name = name
        this.description = description
        this.finished = finished
        this.dueDate = dueDate
    }
    
    var TaskBuilder = () => {
    
        let name
        let description
        let isFinished = false
        let dueDate
    
        return {
            setName: name => {
                this.name = name
                return this
            },
            setDescription: description => {
                this.description = description
                return this
            },
            setFinished: finished => {
                this.finished = finished
                return this
            },
            setDueDate: dueDate => {
                this.dueDate = dueDate
                return this
            },
            build: () => {
                return new Task(name, description, isFinished, dueDate)
            }
        }
    }
    
    let task = new TaskBuilder().setName('Task A').setDescription('finish book')
        .setDueDate(new Date(2019, 5, 12))

###  Factory Method

Creates an instance of several derived classes.

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
    
###  Prototype

A fully initialized instance to be copied or cloned and then modified.

**Uses:**

- JavaScript and TypeScript
- To avoid unnecessary subclasses

**Example:**

    interface Prototype {
        clone(): Prototype
    }
    
    class Person implements Prototype {
        constructor(public name: string, public age: number) {}
    	
        clone(): this {
            const newPerson = Object.create(this)
            return newPerson
        }
    }
    
    const person1 = new Person('Luiz', 30)
    const person2 = person1.clone()

###  Singleton

A class of which only a single instance can exist. 

All singletons have a private constructor (`createInstance`), a public access method (`getInstance`) and a static attribute (`instance`).

**Uses:**
- Database Configuration

**Example:**

    const database = (() => {
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

#### Monostate

## Structural Design Patterns

### Adapter

Match interfaces of different classes.

### Bridge

Separates an object’s interface from its implementation.

### Composite

A tree structure of simple and composite objects.

### Decorator

Add responsibilities to objects dynamically.

**Example:**

    function log(constructor: any) {
        console.log(`New ${constructor.name} created!`)
    }
    
    @log
    class Yogurt {
        
        public flavor: string;
    
        constructor(flavor: string) {
            this.flavor = flavor
        }
    }

### Facade

A single class that represents an entire subsystem.

### Flyweight

A fine-grained instance used for efficient sharing.

### Proxy

An object representing another object.

## Behavioral Patterns

### Chain of Responsibility

A way of passing a request between a chain of objects.

### Command

Encapsulate a command request as an object.

### Interpreter

A way to include language elements in a program.

### Iterator

Sequentially access the elements of a collection.

### Mediator

Defines simplified communication between classes.

### Memento

Capture and restore an object's internal state.

### Observer

A way of notifying change to a number of classes.

### State

Alter an object's behavior when its state changes.

### Strategy

Encapsulates an algorithm inside a class.

### Template Method

Defer the exact steps of an algorithm to a subclass.

### Visitor

Defines a new operation to a class without change.

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
- **Private Class Data:** restricts accessor/mutator access.

## S.O.L.I.D and Clean Code
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTYzMjMzNzc1Niw0NjA1NTc1ODBdfQ==
-->