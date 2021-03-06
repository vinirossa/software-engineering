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
    + [Strategy](#strategy)
    + [Chain of Responsibility](#chain-of-responsibility)
    + [Command](#command)
    + [Mediator](#mediator)
    + [Interpreter](#interpreter)
    + [Iterator](#iterator)
      - [Null Iterator](#null-iterator)
    + [State](#state)
    + [Observer](#observer)
    + [Memento](#memento)
    + [Template Method](#template-method)
    + [Visitor](#visitor)
  * [Other Patterns](#other-patterns)
    + [Null Object](#null-object)
    + [Honorable Mentions](#honorable-mentions)

## Creational Design Patterns

###  Factory Method

Creates an instance of several derived classes.

> *Abstract the instantiation of objects by wrapping them in a Factory method.*

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

A class of which only a single instance can exist and that provides a global access to itl. 

All singletons have a private constructor (`createInstance`), a public access method (`getInstance`) and a static attribute (`instance`).

> *If possible, avoid singletons at all. They can be very tricky to escalate and to work with TDD, as one man's constant is one man's variable.*

> *In a way, it's completely fine to have a single object within an application, but it isn't to make it impossible to create a second instance.*

**Uses:**
- Database Configuration

**UML:**

![singleton](https://user-images.githubusercontent.com/72560319/159556077-69285a0d-d32d-4b24-ad8f-b8a8a1f301b8.png)

**In C#:**
```cs
class Singleton
{
    private Singleton() { }

    private static Singleton? Instance;

    public static void GetInstance()
    {
        if (Instance == null)
            Instance = new Singleton();
    }
}
```

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

> *It's useful when you don't want your code to directly depend on third party code or legacy, uncoupling your code.*

> **Changes the interface** but **doesn't change the implementation**.

**Uses:**
- Frameworks
- External Libraries
- Legacy Codes

**UML:**

![adapter](https://user-images.githubusercontent.com/72560319/159727140-5ee62fd8-c556-47d4-b8e8-b2138c76c243.png)

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
	public Adapter(Adaptee a)
	{
		Adaptee = a;
	}
	
	public Adaptee Adaptee { get; set; }
	
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
        console.log('Email ?? v??lido (CLASS)')
      } else {
        console.log('Email ?? inv??lido (CLASS)')
      }
    }
    
    function validaEmailFn(
      emailValidator: EmailValidatorFnProtocol,
      email: string,
    ): void {
      if (emailValidator(email)) {
        console.log('Email ?? v??lido (FN)')
      } else {
        console.log('Email ?? inv??lido (FN)')
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

Separates an object???s interface / abstraction from its implementation, so that the both can vary and evolve independently.

> *Different from the adapter pattern, the bridge pattern is usually used before the project started, while the adapter is used after the project is already ongoing.*

**Uses:**
- Frameworks
- External Libraries
- Legacy Codes

**UML:**

![bridge-pattern](https://user-images.githubusercontent.com/72560319/159353103-75e0b64a-b72e-46bf-bb00-c9fbd4f7a080.png)

**In C#:**
```cs
abstract class View
{
	public View(IMediaResource mediaResource)
	{
		MediaResource = mediaResource;
	}
	
	public IMediaResource MediaResource;

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
	public ArtistResource(string bio)
    {
        Bio = bio;
    }
    
	public string Bio { get; set; }
}

class ArtistAdapter : IMediaResource
{
	public ArtistAdapter(ArtistResource artistResource)
	{
		ArtistResource = artistResource;
	}
	
	public ArtistResource ArtistResource { get; set; }
	public string Snippet => ArtistResource.Bio;
}

class BookResource
{
	public BookResource(string coverText)
    {
        CoverText = coverText;
    }
    
	public string CoverText { get; set; }
}

class BookAdapter : IMediaResource
{
	public BookAdapter(BookResource bookResource)
	{
		BookResource = bookResource;
	}
	
	public BookResource BookResource { get; set; }
	public string Snippet => BookResource.CoverText;
}
```

### Facade

A single class (`wrapper`) that represents an entire subsystem in higher level. A facade helps to simply and unify a program.

> *High-level level abstraction over low-level components, where the **interface is changed** and the **implementation is not**.*

**Uses:**
- Operational Systems
- Compilers
- Complex Programs

#### Law of Demeter (Principle of Least Knowledge)

An object should never know the internal details of other objects.

> By this, ```a().b().c()``` isn't correct, however ```a().b()``` is. 

**UML:**

![facade](https://user-images.githubusercontent.com/72560319/159728186-1c2659b3-5f89-43d5-beec-2c67ca9b71c0.png)

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

Provide a surrogate or placeholder for another object to control access to it, working as a middleware and adding a level of indirection.

> *There is remote proxies (different context), virtual proxies (expensive crafting resources) and protection proxies (access management).*

> **Changes the implementation** but **doesn't change the interface**.

**Uses:**
- Networking
- VPNs
- Credit card validations
- Cache
- Logging

**UML:**

![proxy](https://user-images.githubusercontent.com/72560319/159519195-b962a1b8-2da4-48d3-88ff-8d464bd113a7.png)

**In C#:**
```cs
interface IBookParser
{
    string Book { get; set; } 
    int GetNumPages();
}

class BookParser : IBookParser
{
    public BookParser(string book)
    {
        // Expensive parsing
        Book = book;
    }

    public string Book { get; set; }

    public int GetNumPages() => 0; // get the number of pages
}

class LazyBookParserProxy : IBookParser
{
    public LazyBookParserProxy(string book)
    {
        BookParser = null;
        Book = book;
    }

    private BookParser? BookParser;
    public string Book { get; set; }

    public int GetNumPages()
    {
        if (BookParser == null)
            return 0;

        BookParser = new BookParser(Book);
        return BookParser.GetNumPages();
    }
}
```

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

Add responsibilities and behaviours to objects dynamically at runtime wrapping them without the need to change them.

> *The decorator pattern is useful to add features to existent code.*

> **Changes the implementation** but **doesn't change the interface**.

**Uses:**
- Cache
- Logging

**UML:**

![decorator](https://user-images.githubusercontent.com/72560319/159546636-9b279668-9f9d-40db-a87a-888dd913b008.png)

**In C#:**
```cs
public class Program
{
    public static void Main()
    {
        var coffe = new CaramelDecorator(new ChocolateDecorator(new Expresso()));
    }
}

interface IBeverage
{
    decimal Cost();
}

class Decaf : IBeverage
{
    public decimal Cost() => 1.10m;
}

class Expresso : IBeverage
{
    public decimal Cost() => 1.00m;
}

interface IAdditionDecorator : IBeverage
{
    IBeverage Beverage { get; set; }
}

class CaramelDecorator : IAdditionDecorator
{
    public CaramelDecorator(IBeverage beverage)
    {
        Beverage = beverage;
    }

    public IBeverage Beverage { get; set; }

    public decimal Cost() => Beverage.Cost() + 0.50m;
}

class ChocolateDecorator : IAdditionDecorator
{
    public ChocolateDecorator(IBeverage beverage)
    {
        Beverage = beverage;
    }

    public IBeverage Beverage { get; set; }

    public decimal Cost() => Beverage.Cost() + 0.74m;
}
```

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

> *Composite pattern is useful with abstractions that can be hierarchically organized.*

**Uses:**
- DOM
- TodoLists
- Genealogy

**UML:**

![composite-pattern](https://user-images.githubusercontent.com/72560319/159482200-4908a097-ae9d-4f25-8f8d-a4ffc76b1395.png)

**In C#:**
```cs
interface ITodoList
{
	string getHtml();
}

class Todo : ITodoList
{
    public Todo(string text)
    {
        Text = text;
    }
    
    public string Text { get; set; }

    public string getHtml() 
    { 
        return "<li>" + Text + "</li>"; 
    }
}

class Project : ITodoList
{
    public Project(string title, List<ITodoList> todos)
    {
        Title = title;
        Todos = todos;
    }
    
    public string Title { get; set; }
    public List<ITodoList> Todos { get; set; }

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

### Strategy

Encapsulates an algorithm inside a class and allows that a class behavior or its algorithm can be changed at runtime.

> *The strategy pattern is about using composition and dependency injection instead of inheritance.*

> *This pattern is useful to encapsulate business rules, once it can remove the need of subclasses and transform abstract classes into normal classes.*

**Uses:**
- Ecommerces
- Finances
- Law

**UML:**

![strategy](https://user-images.githubusercontent.com/72560319/159779934-01d3c83e-a469-4b82-812f-d52cb7e443fb.png)

**In C#:**
```cs
public class Program
{
    public static void Main()
    {
        var cityDuck = new Duck(new SimpleQuacking(), new SimpleFlying(), new DisplayAsText());
        var wildDuck = new Duck(new SimpleQuacking(), new JetFlying(), new DisplayAsGraphics());
        var mountainDuck = new Duck(new SimpleQuacking(), new JetFlying(), new DisplayAsText());
        var rubberDuck = new Duck(new NoQuacking(), new SimpleFlying(), new DisplayAsGraphics());
    }
}

class Duck
{
    public Duck(IQuackBehaviour quackBehaviour, IFlyBehaviour flyBehaviour, IDisplayBehaviour displayBehaviour)
    {
        QuackBehaviour = quackBehaviour;
        FlyBehaviour = flyBehaviour;
        DisplayBehaviour = displayBehaviour;
    }

    public IQuackBehaviour QuackBehaviour { get; set; }
    public IFlyBehaviour FlyBehaviour { get; set; }
    public IDisplayBehaviour DisplayBehaviour { get; set; }

    public void Quack() => QuackBehaviour.Quack();
    public void Fly() => FlyBehaviour.Fly();
    public void Display() => DisplayBehaviour.Display();
}

interface IQuackBehaviour
{
    void Quack();
}

class SimpleQuacking : IQuackBehaviour
{
    public void Quack()
    {
        // Logic
    }
}

class NoQuacking : IQuackBehaviour
{
    public void Quack()
    {
        // Logic
    }
}

interface IFlyBehaviour
{
    void Fly();
}

class SimpleFlying : IFlyBehaviour
{
    public void Fly()
    {
        // Logic
    }
}

class JetFlying : IFlyBehaviour
{
    public void Fly()
    {
        // Logic
    }
}

interface IDisplayBehaviour
{
    void Display();
}

class DisplayAsText : IDisplayBehaviour
{
    public void Display()
    {
        // Logic
    }
}

class DisplayAsGraphics : IDisplayBehaviour
{
    public void Display()
    {
        // Logic
    }
}
```

### Chain of Responsibility

A way of passing a request between a chain of objects called handlers that pass the request forward until it is solved, similar to middlewares.

> *This pattern can be used when it's needed to execute several handlers in a particular order and handlers need to be specified dynamically.*

> *Chain of responsability pattern is useful to apply Single Responsability principle,  to structure your code with an business rule, to make it easy to add features and to alter these responsabilities without affecting the others.*

**Uses:**
- HTTP Requisitions
- Finances
- Validators
- Attendance Chain
- Rules Engine

**UML:**

![chain-of-responsability](https://user-images.githubusercontent.com/72560319/159990391-5a84726d-1cde-4838-a199-1db692df49ed.png)

**In C#:**
```cs
// Client
class Attendance
{
    public void Treat(Call call)
    {
        var handler = new SalesHandler();
        handler
            .SetNextHandler(new RefoundHandler())
            .SetNextHandler(new MarketingHandler())
            .SetNextHandler(new OtherHandler());

        handler.Handle(call);
    }
}


// Request
class Call
{
    public Call(uint option)
    {
        Option = option;
    }

    public uint Option { get; set; }
    public bool Solved { get; set; }
}

// Handlers
abstract class BaseHandler
{
    protected BaseHandler? _nextHandler { get; set; }

    public BaseHandler SetNextHandler(BaseHandler nextHandler)
    {
        _nextHandler = nextHandler;
        return _nextHandler;
    }

    public virtual Call Handle(Call call)
    {
        if (_nextHandler == null)
            return call;

        return _nextHandler.Handle(call);
    }
}

class SalesHandler : BaseHandler
{
    public override Call Handle(Call call)
    {
        if (call.Option == 1)
        {
            Console.WriteLine("Forwarding your call to Sales...");
            call.Solved = true;
            return call;
        }

        return base.Handle(call);
    }
}

class RefoundHandler : BaseHandler
{
    public override Call Handle(Call call)
    {
        if (call.Option == 2)
        {
            Console.WriteLine("Forwarding your call to Refound...");
            call.Solved = true;
            return call;
        }

        return base.Handle(call);
    }
}

class MarketingHandler : BaseHandler
{
    public override Call Handle(Call call)
    {
        if (call.Option == 3)
        {
            Console.WriteLine("Forwarding your call to Marketing...");
            call.Solved = true;
            return call;
        }

        return base.Handle(call);
    }
}

class OtherHandler : BaseHandler
{
    public override Call Handle(Call call)
    {
        Console.WriteLine("Forwarding your call to another sector...");
        call.Solved = true;
        return call;
    }
}
```

### Command

Encapsulates a command request as an object, supporting undoable operations.

> *With this pattern, instead of just perform some action, it's possible to wrap that action in a command and make sure it is undoable.*

**Uses:**
- Automation
- Request Queues
- Editors
- Do & Undo Operations
- Macro Commands

**UML:**

![command](https://user-images.githubusercontent.com/72560319/159564163-dc0e989b-49e8-4fda-9c7b-b0e0f7e3f1b9.png)

**In C#:**
```cs
public class Program
{
    public static void Main()
    {
        var receiver = new Light();
        var invoker = new RemoteControl(
                        new LightOnCommand(receiver),
                        new LightOnCommand(receiver),
                        new LightOffCommand(receiver),
                        new LightOffCommand(receiver)
                      );
    }
}

class RemoteControl // Invoker
{
    public RemoteControl(ICommand on, ICommand up, ICommand down, ICommand off)
    {
        On = on;
        Up = up;
        Down = down;
        Off = off;
    }

    private ICommand On { get; set; }
    private ICommand Up { get; set; }
    private ICommand Down { get; set; }
    private ICommand Off { get; set; }

    public void ClickOn() => On.Execute();
    public void ClickOff() => Off.Execute();
}

interface ICommand
{
    void Execute();
    void Unexecute();
}

class LightOnCommand : ICommand
{
    public LightOnCommand(Light light)
    {
        Light = light;
    }

    public Light Light { get; set; }

    public void Execute() => Light.On();
    public void Unexecute() => Light.Off();
}

class LightOffCommand : ICommand
{
    public LightOffCommand(Light light)
    {
        Light = light;
    }

    public Light Light { get; set; }

    public void Execute() => Light.Off();
    public void Unexecute() => Light.On();
}

class Light // Receiver
{
    public void On() { }
    public void Off() { }
}
```

### Mediator

Defines simplified communication between classes.

### Interpreter

A way to include language elements in a program.

### Iterator

Sequentially access the elements of a collection, without exposing its underlying representation, that is, not exposing all the items right away.

> *Some benefits of iterator pattern is that is possible to create infinite collections and to pause the iteration to continue it later in a much easier way than `foreach`s with an index.*

> *Generic types can be very powerful with iterator pattern.*

**Uses:**
- Custom Collections
- Games

**UML:**

![iterator](https://user-images.githubusercontent.com/72560319/159944504-30077262-791e-433d-80c7-f25f24322081.png)

**In C#:**
```cs
public class Program
{
    public static void Main()
    {
        var handHeldInventory = new HandHeldInventory(new FoodItem(1, "Apple", 20.00), new WeaponIem(2, "Sword", 60.00));
        var handItor = handHeldInventory.GetIterator();

        while (!handItor.IsDone()) // Abstraction
        {
            Console.WriteLine(handItor.Current());
            handItor.Next();
        }        
        
        var backpackInventory = new BackpackInventory(new FoodItem(1, "Apple", 20.00), new FoodItem(1, "Bread", 40.00), new WeaponIem(2, "Sword", 60.00));
        var backpackItor = backpackInventory.GetIterator();

        while (!backpackItor.IsDone()) // Same abstraction
        {
            Console.WriteLine(backpackItor.Current());
            backpackItor.Next();
        }
    }
}

// Items
interface IItem
{
    uint Id { get; set; }
    string Name { get; set; }
}

class FoodItem : IItem
{
    public FoodItem(uint id, string name, double regeneration)
    {
        Id = id;
        Name = name;
        Regeneration = regeneration;
    }

    public uint Id { get; set; }
    public string Name { get; set; }
    public double Regeneration { get; set; }
}

class WeaponIem : IItem
{
    public WeaponIem(uint id, string name, double damage)
    {
        Id = id;
        Name = name;
        Damage = damage;
    }

    public uint Id { get; set; }
    public string Name { get; set; }
    public double Damage { get; set; }
}

// Iterables
interface IInventory
{
    IInventoryIterator GetIterator();
}

class HandHeldInventory : IInventory
{
    public HandHeldInventory(IItem leftItem, IItem rightItem)
    {
        LeftItem = leftItem;
        RightItem = rightItem;
    }

    public IItem LeftItem { get; set; }
    public IItem RightItem { get; set; }

    public IInventoryIterator GetIterator() => new HandHeldInventoryIterator(this); // Factory Method
}

class BackpackInventory : IInventory
{
    public BackpackInventory(IItem item1, IItem item2, IItem item3)
    {
        Item1 = item1;
        Item2 = item2;
        Item3 = item3;
    }

    public IItem Item1 { get; set; }
    public IItem Item2 { get; set; }
    public IItem Item3 { get; set; }
    // ...

    public IInventoryIterator GetIterator() => new HandHeldInventoryIterator(this); // Factory Method
}

// Iterators
interface IInventoryIterator
{
    bool IsDone();
    void Next();
    IItem? Current();
}

class HandHeldInventoryIterator : IInventoryIterator
{
    public HandHeldInventoryIterator(HandHeldInventory inventory)
    {
        _inventory = inventory;
    }

    private HandHeldInventory _inventory { get; set; } // Iterable
    private uint _index = 0;

    public bool IsDone() => _index < 2;
    public void Next() => _index++;
    public IItem? Current()
    {
        switch (_index)
        {
            case 0:
                return _inventory.LeftItem;
            case 1:
                return _inventory.RightItem;
            default:
                return null;
        }
    }
}

class BackpackInventoryIterator : IInventoryIterator
{
    public BackpackInventoryIterator(BackpackInventory inventory)
    {
        _inventory = inventory;
    }

    private BackpackInventory _inventory { get; set; } // Iterable
    private uint _index = 0;

    public bool IsDone() => _index < 3;
    public void Next() => _index++;
    public IItem? Current()
    {
        switch (_index)
        {
            case 0:
                return _inventory.Item1;
            case 1:
                return _inventory.Item2;
            case 2:
                return _inventory.Item3;
            default:
                return null;
        }
    }
}
```
#### Null Iterator

> *For better undestanding, check the [Null Object Pattern](#null-object)*.

Defines an always null iterator.

**Uses:**
- Component Leaves (Composite Pattern)
- Null States (State Pattern)

**In C#:**
```cs
class NullIterator : IIterator
{
    public NullIterator(IIterable iterable)
    {
        _iterable = iterable;
    }

    private IIterable _iterable { get; set; }
    private uint _index = 0;

    public bool IsDone() => true;
    public void Next() { }
    public IItem? Current() => null;
}
```

### State

Alter an object's behavior at runtime when its internal state changes.

> *The state is not an enum or property, it is an object. Therefore, it replaces a conditional with polymorphism.*

**Uses:**
- Network Requests
- UI Components
- No-Memory Machines

![state-example](https://user-images.githubusercontent.com/72560319/159712767-c29a028c-22a5-4be6-9a2d-7390e533e2a8.png)

**UML:**

![state](https://user-images.githubusercontent.com/72560319/159712692-cf90884f-1bee-475d-94d7-e02a630e9bec.png)

**In C#:**
```cs
class Gate
{
    public Gate(IGateState initialState)
    {
        State = initialState;
    }

    public IGateState State { get; set; }

    public void Enter() => State = State.Enter(); 
    public void Pay() => State = State.Pay(); 
    public void PayOk() => State = State.PayOk(); 
    public void PayFailed() => State = State.PayFailed(); 
}

interface IGateState
{
    IGateState Enter();
    IGateState Pay();
    IGateState PayOk();
    IGateState PayFailed();
}

class ClosedGateState : IGateState
{
    public IGateState Enter() => new ClosedGateState();
    public IGateState Pay() => new ProcessingPaymentGateState();
    public IGateState PayOk() => new ClosedGateState();
    public IGateState PayFailed() => new ClosedGateState();
}

class ProcessingPaymentGateState : IGateState
{
    public IGateState Enter() => new ProcessingPaymentGateState();
    public IGateState Pay() => new ProcessingPaymentGateState();
    public IGateState PayOk() => new OpenGateState();
    public IGateState PayFailed() => new ClosedGateState();
}

class OpenGateState : IGateState
{
    public IGateState Enter() => new ClosedGateState();
    public IGateState Pay() => new OpenGateState();
    public IGateState PayOk() => new OpenGateState();
    public IGateState PayFailed() => new OpenGateState();
}
```

### Observer

Defines an one to many dependency between objects, so that when the observable changes state all of its dependecies are notified and updated automatically.

> *The observable is the object that changes and the observers are the objects interested in the observable changes.*

**Uses:**
- Chats
- Sensors
- Newsletters
- Notifications

**UML:**

![observer](https://user-images.githubusercontent.com/72560319/159771101-5af99680-3192-430e-ac3e-ff87cc54c350.png)

**In C#:**
```cs
public class Program
{
	public static void Main()
	{
		var station = new WeatherStation();
		station.Add(new PhoneDisplay(station));
		station.Add(new WindowDisplay(station));
	}
}

interface IObservable
{
    List<IObserver> Observers { get; }

    void Add(IObserver observer);
    void Remove(IObserver observer);
    void Notify();
}

class WeatherStation : IObservable
{
    public List<IObserver> Observers { get; private set; } = new List<IObserver> { };

    private decimal _temperature { get; set; }
    public decimal Temperature
    {
        get => _temperature;
        set 
        { 
            _temperature = value;
            Notify();
        }
    }

    private decimal _humidity { get; set; }
    public decimal Humidity
    {
        get => _humidity;
        set
        {
            _humidity = value;
            Notify();
        }
    }

    public void Add(IObserver observer) => Observers.Add(observer);
    public void Remove(IObserver observer) => Observers.Remove(observer);
    public void Notify() => Observers.ForEach(o => o.Update());
}

interface IObserver
{
    void Update();
}

class PhoneDisplay : IObserver
{
    public PhoneDisplay(WeatherStation station)
    {
        Station = station;
    }

    public WeatherStation Station { get; init; }
    public decimal Temperature { get; private set; } // Only this state matters for this object

    public void Update() 
    {
        if (Temperature != Station.Temperature)
        {
            Temperature = Station.Temperature;
            Display();
        }
    }

    public void Display() { } // Refreshes temperature on display
}

class WindowDisplay : IObserver
{
    public WindowDisplay(WeatherStation station)
    {
        Station = station;
    }

    public WeatherStation Station { get; init; }
    public decimal Humidity { get; private set; } // Only this state matters for this object

    public void Update()
    {
        if (Humidity != Station.Humidity)
        {
            Humidity = Station.Humidity;
            Display();
        }
    }

    public void Display() { } // Refreshes humidity on display
}
```

### Memento

Capture and restore an object's internal state.

> *This pattern is similitar to [Command Pattern](#command), but the main difference is that instead of requests or actions, it deals with object's state.*

> *Because Memento Pattern stores the objects' states in memory, it can consume a large amount of RAM. However, this problem can be solved with [Flyweight Pattern](#flyweight), for example.*

**Uses:**
- Editors
- Do & Undo Operations
- Crash Recovery

**UML:**

![memento](https://user-images.githubusercontent.com/72560319/159993929-ce206993-de13-4f51-bb59-6fcd6a15982e.png)

**In C#:**
```cs
interface IOriginator<T>
{
    T SaveMemento();
    void RestoreMemento(T memento);
}

interface ISubjectMemento
{
    public string Prop1 { get; set; } // States
}

class Subject : IOriginator<SubjectMemento>, ISubjectMemento // Originator
{
    public Subject(string prop1, int prop2, bool prop3)
    {
        Prop1 = prop1;
        Prop2 = prop2;
        Prop3 = prop3;
    }

    public string Prop1 { get; set; }
    public int Prop2 { get; set; }
    public bool Prop3 { get; set; }

    public void Method1() { }
    public void Method2() { }

    public SubjectMemento SaveMemento() => new SubjectMemento(Prop1);
    public void RestoreMemento(SubjectMemento memento) => Prop1 = memento.Prop1;
}

class SubjectMemento : ISubjectMemento
{
    public SubjectMemento(string prop1)
    {
        Prop1 = prop1;
    }

    public string Prop1 { get; set; }
}

class SubjectHistory // Caretaker
{
    private List<SubjectMemento> _mementos { get; set; } = new List<SubjectMemento> { };

    public void Add(SubjectMemento memento) => _mementos.Add(memento);
    public SubjectMemento? Get(uint index) => (index < _mementos.Count) ? _mementos[(int)index] : null;
    public SubjectMemento? GetLast() => (_mementos.Count != 0) ? _mementos[_mementos.Count - 1] : null;
}
```

### Template Method

Defer the exact steps of an algorithm to a subclass, working as a blueprint for methods.

> *Template method pattern works better in cases where strategy doesn't fit and there is fixed logic on the template. Anyway, it can be very dangerous given that it uses inheritance over composition.*

> *A template method class is composed by a defined method (template method) and the empty methods (hooks) that are called inside the template method.*

> *This pattern is similitar to inversion of dependency pattern, like the Hollywood principle: "Don't call us, we'll call you. Or not."* 

**Uses:**
- Frameworks
- Database Operations

**UML:**

![template-method](https://user-images.githubusercontent.com/72560319/159915346-6633ca6f-0198-43cb-b041-3090c50ebe83.png)

**In C#:**
```cs
abstract class Record
{
    public void Save()
    {
        Validate();
        BeforeSave();
        // Save to DB
        AfterSave();
    }

    // Hooks
    public virtual void Validate() { }
    public virtual void BeforeSave() { }
    public virtual void AfterSave() { }
}

class UserRecord : Record
{
    public override void Validate()
    {
        // Logic
    }

    public override void BeforeSave()
    {
        // Logic
    }
}

class PostRecord : Record
{
    public override void Validate()
    {
        // Logic
    }

    public override void AfterSave()
    {
        // Logic
    }
}
```

### Visitor

Defines a new operation to a class without change it.

## Other Patterns

### Null Object

Designed to act as a default value of an object, working as a null state.

> *This pattern make it possible to handle nullness in the same way that somethingness is handled.*

### Honorable Mentions

-   **Dependency Injection**
-   **Inversion of Control**
-   **Inversion of Dependency**
-   **Lazy Loading**
-   **Object Pool:** avoid expensive acquisition and release of resources by recycling objects that are no longer in use
- **Private Class Data:** restricts accessor/mutator access.
-   **Mock Object**
-   **Rules Design Patterns**
-   **Intercepting Filter**
-   **Method Chaining**
-   **Unit of Work**

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NjY3MjY4MzEsLTEzODM0MzEyOTQsLT
QwNzI5OTM3LC04Mjc1NTM4NywtMjA5MDM3NzAxMSwtNzI1ODc5
MTY4LC0xNTEwNjcwODIyLDM2OTA3MDg4OCwtMzU2ODgyOTksLT
M0OTI1NzU2OSwtMjAxNTUxODE3Niw0OTk0NDM5ODYsMTkwOTAw
MTk1MCwtNjI2ODE4MzYsMTAzMDQwMTA3Miw0MDM4Nzk1NDEsND
EyOTk3ODQ0LDM0NTI3ODc1NywtMzY2MDE4OTA2LDQ2MTIxMDkx
MV19
-->