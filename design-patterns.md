# Design Patterns

- [Design Patterns](#design-patterns)
  * [Creational Design Patterns](#creational-design-patterns)
    + [Factory Method](#factory-method)
    + [Abstract Factory](#abstract-factory)
    + [Builder](#builder)
    + [Prototype](#prototype)
    + [Singleton](#singleton)
      - [Monostate...](#monostate)
  * [Structural Design Patterns](#structural-design-patterns)
    + [Adapter](#adapter)
    + [Bridge](#bridge)
    + [Facade](#facade)
      - [Law of Demeter (Principle of Least Knowledge)](#law-of-demeter--principle-of-least-knowledge-)
    + [Proxy](#proxy)
    + [Decorator](#decorator)
    + [Composite](#composite)
    + [Flyweight](#flyweight)
  * [Behavioral Patterns](#behavioral-patterns)
    + [Chain of Responsibility](#chain-of-responsibility)
    + [Command](#command)
    + [Interpreter](#interpreter)
    + [Iterator](#iterator)
    + [Mediator](#mediator)
    + [Memento](#memento)
    + [Observer](#observer)
    + [State](#state)
    + [Strategy](#strategy)
    + [Template Method](#template-method)
    + [Visitor](#visitor)
  * [Other Patterns](#other-patterns)

## Creational Design Patterns

###  Factory Method

Creates an instance of several derived classes.

> Abstract the instantiation of objects by wrapping them in a Factory method.

> [Complete explanation](https://www.youtube.com/watch?v=EcFVTgRHJLM&list=PLrhzvIcii6GNjpARdnO4ueTUAVR9eMBpc&index=4&ab_channel=ChristopherOkhravi). 

**Uses:**
- Frameworks
- TDD

**UML:**

![factory-method](https://user-images.githubusercontent.com/72560319/159106269-6a0b440f-ad84-4f1b-bc60-9252db988268.png)

**In C#:**
```cs
public class Program
{
	public static void Main()
	{
		var animal1 = new BalancedAnimalFactory().CreateAnimal();
		var animal2 = new RandomAnimalFactory().CreateAnimal();
	}
}
	
public abstract class Animal { }

public class Dog : Animal { }
public class Cat : Animal { }
public class Duck : Animal { }

public interface IAnimalFactory 
{
	public Animal CreateAnimal();
}

public class BalancedAnimalFactory : IAnimalFactory
{
	public Animal CreateAnimal()
	{
		// Logic
		return new Dog();
	}
}

public class RandomAnimalFactory : IAnimalFactory
{
	public Animal CreateAnimal()
	{
		// Logic
		return new Cat();
	}
}
```

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

###  Abstract Factory

Create an object without exposing the creation logic to the client and refer to newly created object using a common interface.

> [Complete explanation](https://www.youtube.com/watch?v=v-GiuMmsXj4&list=PLrhzvIcii6GNjpARdnO4ueTUAVR9eMBpc&index=5&ab_channel=ChristopherOkhravi). 

**Uses:**
- UI Controls
- Multiplatform Toolkits
- Product Families

**UML:** 

![abstract-factory](https://user-images.githubusercontent.com/72560319/159106388-4cc123cf-d58e-4857-8d55-921cf0a3bd78.png)

**In C#:**
```cs
public class Program
{
	public static void Main()
	{
		new NavigationBar(new Android());
		new DropdownMenu(new Android());
	}
}

public class Button
{
	public string Type { get; set; }
}

public interface IUIFactory
{
	public Button CreateButton();
}

public class Apple : IUIFactory
{
	public Button CreateButton()
	{
		return new Button { Type = "iOS Button".Dump() };
	}
}

public class Android : IUIFactory
{
	public Button CreateButton()
	{
		return new Button { Type = "Android Button".Dump() };
	}
}

public class NavigationBar
{
	public NavigationBar(IUIFactory factory) => factory.CreateButton();
}

public class DropdownMenu
{
	public DropdownMenu(IUIFactory factory) => factory.CreateButton();
}
```

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

**Uses:**
- Templates
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
    
###  Prototype

A fully initialized instance to be copied or cloned and then modified.

**Uses:**
- Avoid unnecessary subclasses
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

**Uses:**
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

Match interfaces of different classes, working as a wrapper. With an adapter, change the underline behaviour is not necessary.

> It's useful when you don't want your code to directly depend on third party code or legacy, uncoupling your code.

**Uses:**
- Frameworks
- External Libraries
- Legacy Codes

**In C#:**
```cs
public class Program
{
	public static void Main()
	{
		var target = new Adapter(new Adaptee());
		target.Request();
	}
}

class Adaptee
{
	public void SpecificRequest()
	{
		// Logic
	}
}

interface ITarget
{
	void Request();
} 

class Adapter : ITarget
{
	public Adaptee Adaptee { get; set; }
	
	public Adapter(Adaptee a)
	{
		Adaptee = a;
	}
	
	public void Request()
	{
		// Logic to adapt the method
		Adaptee.SpecficRequest();
	}
} 
```

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

> Different from the adapter pattern, the bridge pattern is usually used before the project started, while the adapter is used after the project is already ongoing.

**UML:**

![bridge-pattern](https://user-images.githubusercontent.com/72560319/159353103-75e0b64a-b72e-46bf-bb00-c9fbd4f7a080.png)

**Uses:**
- Frameworks
- External Libraries
- Legacy Codes

**In C#:**
```cs
abstract class View
{
	public IMediaResource MediaResource;

	public View(IMediaResource mediaResource)
	{
		MediaResource = mediaResource;
	}

	public virtual string Show() { return "html"; }
}

class ShortFormView : View
{
    public ShortFormView(IMediaResource mediaResource) : base(mediaResource)
    {
		MediaResource = mediaResource;
	}

    public override string Show()
	{
		// Logic
		// MediaResource.Snippet;
		return "html";
	}
}

class LongFormView : View
{
	public LongFormView(IMediaResource mediaResource) : base(mediaResource)
	{
		MediaResource = mediaResource;
	}

	public override string Show()
	{
		// Logic
		// MediaResource.Snippet;
		return "html";
	}
}

interface IMediaResource
{
	string Snippet { get; }
}

class ArtistResource
{
	public string Bio { get; set; }

	public ArtistResource(string bio)
    {
        Bio = bio;
    }
}

class ArtistAdapter : IMediaResource
{
	public ArtistResource ArtistResource { get; set; }
	public string Snippet => ArtistResource.Bio;

	public ArtistAdapter(ArtistResource artistResource)
	{
		ArtistResource = artistResource;
	}
}

class BookResource
{
	public string CoverText { get; set; }

	public BookResource(string coverText)
    {
        CoverText = coverText;
    }
}

class BookAdapter : IMediaResource
{
	public BookResource BookResource { get; set; }
	public string Snippet => BookResource.CoverText;

	public BookAdapter(BookResource bookResource)
	{
		BookResource = bookResource;
	}
}
```

### Facade

A single class (`wrapper`) that represents an entire subsystem in higher level. A facade helps to simply and unify a program.

**Uses:**
- Operational Systems
- Compilers
- Complex Programs

#### Law of Demeter (Principle of Least Knowledge)

An object should never know the internal details of other objects.

> By this, ```a().b().c()``` isn't correct, however ```a().b()``` is. 

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


### Proxy

Provide a surrogate or placeholder for another object to control access to it, working as a middleware.

**Applicability:**
- Access control
- Logs
- Cache
- Lazy instanciation
- Lazy evaluation

**Uses:**
- Networking
- VPNs
- Credit card validations

**In TypeScript:**
```ts
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
```

### Decorator

Add responsibilities and behaviours to objects dynamically without the need to change it.

**Uses:**
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

### Composite

A tree structure of composite objects and leaf objects.

> Composite pattern is useful with abstractions that can be hierarchically organized.

**UML:**



**Uses:**
- DOM
- TodoLists
- Genealogy

**In C#:**
```cs
interface ITodoList
{
	string getHtml();
}

class Todo : ITodoList
{
    public string Text { get; set; }

    public Todo(string text)
    {
        Text = text;
    }

    public string getHtml() 
    { 
        return "<li>" + Text + "</li>"; 
    }
}

class Project : ITodoList
{
    public string Title { get; set; }
    public List<ITodoList> Todos { get; set; }

    public Project(string title, List<ITodoList> todos)
    {
        Title = title;
        Todos = todos;
    }

    public string getHtml() 
    {
        var html = "<h1>" + Title + "</h1>";

        html += "<ul>";
        Todos.ForEach(t => html += t.getHtml());
        html += "</ul>";

        return html;
    }
}
```

### Flyweight

A fine-grained instance used for efficient sharing and memory saving, as if an flyweight object already exists, it will be returned and not created again.

**Applicability:**
- High memory cost softwares
- Optimization
- Memory saving

## Behavioral Patterns

### Chain of Responsibility

A way of passing a request between a chain of objects, similar to middlewares.

**Applicability:**
- Task delegation

**Uses:**
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

**Uses:**
- Ecommerces

### Template Method

Defer the exact steps of an algorithm to a subclass.

### Visitor

Defines a new operation to a class without change.

## Other Patterns

-   **Rules Design Patterns**
-   **Dependency Injection**
-   **Inversion of Control**
-   **Inversion of Dependency**
-   **Intercepting Filter**
-   **Lazy Loading**
-   **Mock Object**
-   **Method Chaining**
-   **Unit of Work**
-   **Object Pool:** avoid expensive acquisition and release of resources by recycling objects that are no longer in use
- **Null Object:** designed to act as a default value of an object
- **Private Class Data:** restricts accessor/mutator access.

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTUxNTYzNzA5MSwtMTQ3NjM4ODM2NywtMT
Q1Nzk2MzEzMSwtMTYwMDQ2MzAxNiwyNDk4MzgyNDYsNzk5NTM5
MTMzLDgzNTA0NDYxNCwtNzEwMzc5NjA1LDczNTU3NjY2OSwxNj
cyNzA4OTQyLC02NTk1MTYwNjksMTkyMDMzMDI2Niw0NDc1ODE5
OTUsLTEyOTY2MTc3NDUsMTYxMzc4OTU4MCw3NDI1OTEwNjEsLT
ExODU0MTY3MjAsMTY1OTQzNTk0OCw0Mzc5NTkxMjAsLTI3ODkx
OTExN119
-->