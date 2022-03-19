# Design Patterns

## Creational Design Patterns

###  Abstract Factory

Provide an interface for creating families of related or dependent objects without specifying their concrete classes and compromising the final objects.

**Uses:**
- Kits
- Multiplatform Toolkits

**In TypeScript:**

```ts
    abstract class Animal {}
    
    abstract class Bird extends Animal {}
    
    abstract class Reptile extends Animal {}
    
    class Hawk extends Bird {}
    
    class Alligator extends Reptile {}
```

###  Builder

Separates object construction from its representation. 

A `Builder` abstract class or interface and one `ConcreteBuilder` for each variation of the product.

**Applicability:**
- Templates

**Known Uses:**
- Text Editors

**In TypeScript:**

```ts
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
```

###  Factory Method

Creates an instance of several derived classes.

**Applicability:**

**Known Uses:**
- Frameworks
- TDD

**In TypeScript:**

```ts
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
```
    
###  Prototype

A fully initialized instance to be copied or cloned and then modified.

**Applicability:**
- Avoid unnecessary subclasses

**Known Uses:**
- JavaScript and TypeScript

**In TypeScript:**

```ts
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
```

###  Singleton

A class of which only a single instance can exist. 

All singletons have a private constructor (`createInstance`), a public access method (`getInstance`) and a static attribute (`instance`).

**Applicability:**

**Known Uses:**
- Database Configuration

**In TypeScript:**

```ts
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
```

#### Monostate...

Respects the S.O.L.I.D principles.

## Structural Design Patterns

### Adapter

Match interfaces of different classes. It's useful when you don't want your code to directly depend on third party code or legacy, uncoupling your code.

**Applicability:**
- Uncouple code
- On Refactoring

**Known Uses:**
- Frameworks
- External Libraries
- Legacy Codes

**In TypeScript:**

```ts
> main.ts

    import {
      EmailValidatorProtocol,
      EmailValidatorFnProtocol,
    } from './validation/email-validator-protocol'
    import { EmailValidatorClassAdapter } from './validation/email-validator-class-adapter'
    import { emailValidatorFnAdapter } from './validation/email-validator-fn-adapter'
    
    function validaEmailClass(
      emailValidator: EmailValidatorProtocol,
      email: string,
    ): void {
      if (emailValidator.isEmail(email)) {
        console.log('Email é válido (CLASS)')
      } else {
        console.log('Email é inválido (CLASS)')
      }
    }
    
    function validaEmailFn(
      emailValidator: EmailValidatorFnProtocol,
      email: string,
    ): void {
      if (emailValidator(email)) {
        console.log('Email é válido (FN)')
      } else {
        console.log('Email é inválido (FN)')
      }
    }
    
    const email = 'luizomf@gmail.com'
    validaEmailClass(new EmailValidatorClassAdapter(), email)
    validaEmailFn(emailValidatorFnAdapter, email)

> email-validator-protocol.ts

    export interface EmailValidatorProtocol {
      isEmail: EmailValidatorFnProtocol
    }
    
    export interface EmailValidatorFnProtocol {
      (value: string): boolean
    }

> email-validator-class-adapter.ts

    import isEmail from 'validator/lib/isEmail'
    import { EmailValidatorProtocol } from './email-validator-protocol'
    
    export class EmailValidatorClassAdapter implements EmailValidatorProtocol {
      isEmail(value: string): boolean {
        return isEmail(value)
      }
    }

> email-validator-fn-adapter.ts

    import isEmail from 'validator/lib/isEmail'
    import { EmailValidatorFnProtocol } from './email-validator-protocol'
    
    export const emailValidatorFnAdapter: EmailValidatorFnProtocol = (
      value: string,
    ): boolean => {
      return isEmail(value)
    }
```

### Bridge

Separates an object’s interface / abstraction from its implementation, so that the both can vary and evolve independently.

**Applicability:**
- Uncouple code
- On Planning

**Known Uses:**
- Frameworks
- External Libraries
- Legacy Codes

### Composite

A tree structure of simple (leaf) and composite objects.

### Decorator

Add responsibilities to objects dynamically.

**Applicability:**
- Add features to existent code
- Logging

**In TypeScript:**

```ts
> Class decorators

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

> Method decorators

    import userModel from './user.model'
    import UserNotFoundException from './UserNotFoundException'
    
    function excludeProperties(propertiesToExclude: string[]) {
        return (target: any, propertyName: string, descriptor: PropertyDescriptor) =&gt {
            const originalFunction = descriptor.value
            
            descriptor.value = async function(...args: any[]) {
                const originalResult = await originalFunction.apply(this, args)
                propertiesToExclude.forEach(propertyName =&gt {
                delete originalResult[propertyName]
                })
                return originalResult
            }
        }
    }
        
    class UserService {
        private user = userModel
        
        @excludeProperties(['password'])
        private getUser = async (userId: string) => {
            const user = await this.user.findById(userId)
            if (user) {
                return user
            }
            throw new UserNotFoundException(userId)
        }
    }
```

### Facade

A single class (`wrapper`) that represents an entire subsystem.

**Applicability:**
- Simplify and Unify

**Known Uses:**
- Operational Systems

**In TypeScript:**

```ts
> customer-facade.ts

    import { CustomerClient } from '../models/customer-client.ts'
    import { CustomerAvatar } from '../models/customer-avatar.ts'
    import { CustomerDocuments } from '../models/customer-documents.ts'
    import { CustomerAccessHistory } from '../models/customer-access-history.ts'
    import { CustomerService } from '../models/customer-service.ts'
    import { CustomerEmail } from '../models/customer-email.ts'
     
    export module Facade { 
        export class CustomerFacade { 
            static removeAccount(customer: Customer) {
                const customerAvatar = new CustomerAvatar(customer)
                const customerDocumentos = new CustomerDocuments(customer)
                const customerHistoricoAcesso = new CustomerAccessHistory(customer)
                const customerService = new CustomerService(customer)
                const customerEmail = new CustomerEmail(customer)
    
                customerAvatar.remove()
                customerDocumentos.delete()
                customerHistoricoAcesso.remove()
                customerService.delete()
                customerEmail.sendRemoveAccount()
            }
        }
    }

> main.ts

    import { CustomerClient } from '../models/customer-client.ts'
    import { Facade } from './facade/customer-facade.ts'
    
    const John = new Customer(
        "John Holts",
        "johnholts",
        "johnholts67@hotmail.com"
    )
    
    const Alice = new Customer(
        "Alice Cooper",
        "alicecooper",
        "alicecooperw@gmail.com"
    )
    
    Facade.CustomerFacade.removeAccount(John)
    Facade.CustomerFacade.removeAccount(Alice)
```

### Flyweight

A fine-grained instance used for efficient sharing and memory saving, as if an flyweight object already exists, it will be returned and not created again.

**Applicability:**
- High memory cost softwares
- Optimization
- Memory saving

### Proxy

Provide a surrogate or placeholder for another object to control access to it, working as a middleware.

**Applicability:**
- Access control
- Logs
- Cache
- Lazy instanciation
- Lazy evaluation

**Known Uses:**
- Networking
- VPNs
- Credit card validations

**Example:**

    export interface Subject { 
        request(): void
    } 
    
    export class RealSubject implements Subject { 
        request(): void { 
            console.log('Something that the object do.')
        } 
    } 
    
    export class Proxy implements Subject { 
        constructor(private subject: Subject) {} 
    
        request(): void { 
            console.log('The proxy do something')
            this.subject.request()
            console.log('The proxy can do other thing')
        } 
    }

## Behavioral Patterns

### Chain of Responsibility

A way of passing a request between a chain of objects, similar to middlewares.

**Applicability:**
- Task delegation

**Known Uses:**
- HTTP requisitions

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

Encapsulates an algorithm inside a class and allows that a class behavior or its algorithm can be changed at run time.

**Applicability:**
- Many algorithms and need to change them at run time
- Isolate business rules
- Encapsulates a group of conditionals

**Known Uses:**
- Ecommerces

### Template Method

Defer the exact steps of an algorithm to a subclass.

### Visitor

Defines a new operation to a class without change.

## Other Patterns

-   **Rules Design Patterns**
-   **Dependency Injection**
-   **Inversion of control**
-   **Inversion of dependency **
-   **Intercepting filter**
-   **Lazy loading**
-   **Mock object**
-   **Method chaining**
-   **Unit of Work**
-   **Object Pool:** avoid expensive acquisition and release of resources by recycling objects that are no longer in use
- **Null Object:** designed to act as a default value of an object
- **Private Class Data:** restricts accessor/mutator access.

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ0NjA5MjQ0NywtMjk0ODY5OTM1LC0xOD
Y0ODY3OTc4LDMxNTM5MDcxOCw0NjA1NTc1ODBdfQ==
-->