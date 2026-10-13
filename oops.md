1. What is OOP?
OOP (Object-Oriented Programming) is a programming paradigm in which software is designed around objects that contain data and behavior.
For example, consider a banking application:
- Data: account number, balance, account holder.
- Behavior: deposit, withdraw, check balance.
Instead of writing all these operations as unrelated functions, we can create a BankAccount class that groups related data and behavior.

1.1  Why do we use OOP?(it helps us to write code that is reusable, modular, maintainable,secure and easy extend)- 
1. Code reusability: Reuse existing code through inheritance and composition.
2. Encapsulation: Protect an object's internal state.
3. Abstraction: Expose essential functionality while hiding implementation details.
4. Polymorphism: Allow a common interface to have different implementations.
5. Maintainability: Organize code into manageable units.
6. Extensibility: Add new functionality without rewriting the entire application.

##### Class and Object
2. Class and Object
These are the foundational concepts of OOP.

2.1 Class - Class is a reference type(blue print) that describe(defines) the data and behavior of its object can have.

2.2 Object - Object is a runtime instance of class that have data and behavior. 

###### Constructor
3. Constructors in Java
A constructor is a special member used to initialize a newly created object.

3.1 Properties of constructors
- The constructor name must match the class name.
- It has no return type, not even void.
- It runs when an object is created using new.
- Constructors can be overloaded.
- Constructors are not inherited and cannot be overridden.
- A constructor can use access modifiers such as public, protected, package-private, or private.

3.2 Default constructor vs parameterized constructor

Important distinction: If you do not declare any constructor, Java provides a default no-argument constructor. If you declare a constructor yourself, Java does not automatically provide that default constructor.

3.3 What is this?
The this keyword refers to the current object in an instance context.

#### Encapsulation
4. Encapsulation
Encapsulation = bundling data and methods together, while controlling access to the internal state using access modifier.

4.1 Access modifier comparison table
Access modifier	->   Same class ->	Same package ->	Subclass in another package	->  Unrelated class in another package
public	                 Yes	        Yes         	Yes	                            Yes
protected	             Yes	        Yes	            Yes                          through inheritance rules	No
Default                	 Yes	        Yes	            No	                                No
private	                 Yes	        No	            No                              	No

#### Inheritance
5. Inheritance - 
 In programming, inheritance is a mechanism where a new class—known as the child class (subclass or derived class)—acquires the properties and behaviors (methods and attributes) of an existing class—known as the parent class (superclass or base class).
It is achieved using extends keyword.

5.1 Types of inheritance:-
a.) Single inheritance-> One child class extends one parent class. e.g - Animal → Dog
b.) Multilevel inheritance-> A class extends a class that itself extends another class. eg- Animal → Dog → Puppy
c.) Hierarchical inheritance-> Multiple child classes extend the same parent. e.g- Animal → Dog and Animal → Cat
d.) Multiple inheritance -> Java classes cannot extend multiple classes, but a class can implement multiple interfaces. using interface 
Duck implements Flyable, Swimmable

5.2 The super keyword - super refers to the superclass portion of the current object.
It can be used to:
- Access a superclass field hidden by a subclass field.
- Call an overridden superclass method.
- Invoke a superclass constructor using super(...).

# Example- 
class Animal {
    String name = "Animal";

    Animal(String type) { System.out.println("Animal constructor: " + type);}

    void sound() {  System.out.println("Animal makes a sound");  }
}

class Dog extends Animal {
    String name = "Dog";
    Dog() {   super("Mammal"); }

    @Override
    void sound() { super.sound();
        System.out.println("Dog barks"); }

    void display() {
        System.out.println(super.name);
        System.out.println(this.name);
    }
}

When new Dog() is executed, the superclass constructor runs before the subclass constructor body.

Example for the above - 
class A {
    A() {  System.out.println("A constructor");  }
}

class B extends A {
    B() {   System.out.println("B constructor");  }
}

Creating new B() prints:
A constructor
B constructor

#### Ploymorphism
6. Polymorphism  means one thing can take many forms. It allows the same method name or interface to behave differently in different situations.
Example: An Animal makes a sound, but a Dog barks and a Cat meows.
In Java, polymorphism is achieved through:
1. Compile-time polymorphism — Method overloading.
2. Runtime polymorphism — Method overriding.
In simple words: Polymorphism allows the same method or reference to perform different actions depending on the situation.

6.1 Method overloading
Multiple methods in the same class can have the same name but different parameter lists.

6.2 Method overriding
A subclass provides its own implementation of an inherited instance method.

### Example - 
class Animal {
    void sound() {
        System.out.println("Generic animal sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Meow");
    }
}

Animal a1 = new Dog();
Animal a2 = new Cat();

a1.sound(); // Bark
a2.sound(); // Meow 

This is runtime polymorphism: the implementation invoked depends on the actual object's class, not merely the declared reference type.

Additional overriding rules:
- The overriding method cannot reduce access visibility.
- It cannot declare broader checked exceptions than the overridden method permits.
- final methods cannot be overridden.
- private methods are not overridden because they are not inherited.
- Static methods are hidden, not overridden.
- Constructors cannot be overridden.

6.3 Upcasting and downcasting
Animal a = new Dog(); // Upcasting

Upcasting treats a child object as a parent type. It is generally implicit and useful for polymorphism.

6.4 DownCasting
if (a instanceof Dog) {
    Dog d = (Dog) a; // Downcasting
    d.sound();
}

Downcasting converts a reference to a more specific type. It must be valid for the actual object or it can throw ClassCastException.

## Interview point: A reference's declared type determines which members can be accessed at compile time; the actual object's type determines which overridden instance implementation runs.

#### Abstraction 
7. Abstraction
Abstraction means exposing the essential operations of an object while hiding unnecessary implementation details.
Think about using an ATM:
- You enter a PIN and request a withdrawal.
- You do not need to understand the internal banking network, database transactions, or cash dispenser mechanism.

Java provides two major ways to implement abstraction:
1. Abstract classes.
2. Interfaces.

7.1 Abstract class
An abstract class is declared using the abstract keyword. It cannot be instantiated directly.
It can contain:
- Abstract methods without method bodies.
- Concrete methods with implementations.
- Constructors.
- Instance fields and static fields.
- Static methods and other class members.

### Example:
abstract class Payment {
    abstract void pay(double amount);

    void printReceipt() {
        System.out.println("Receipt generated");
    }
}

The method pay() defines behavior that subclasses must implement, while printReceipt() provides shared behavior.
class UpiPayment extends Payment {
    @Override
    void pay(double amount) {
        System.out.println("Paid ₹" + amount + " using UPI");
    }
}

class CardPayment extends Payment {
    @Override
    void pay(double amount) {
        System.out.println("Paid ₹" + amount + " using Card");
    }
}

Usage:
Payment payment = new UpiPayment();

payment.pay(500); /// Paid ₹500.0 using UPI
payment.printReceipt(); /// Receipt generated


The abstract class defines a common contract and can also provide shared implementation.

### Interface
7.2 Interface
An interface defines a contract that implementing classes agree to follow.
interface PaymentMethod {
    void pay(double amount);
}

class Upi implements PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("UPI payment: " + amount);
    }
}

class CreditCard implements PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Card payment: " + amount);
    }
}

Usage:
PaymentMethod method = new Upi();
method.pay(1000);

method = new CreditCard();
method.pay(2000);

A class can implement multiple interfaces:

7.3 Modern interface features
An interface can contain:
- Abstract instance methods.
- default methods with implementations.
- static methods.
- private helper methods (since Java 9).
- Constants, which are implicitly public static final.

### 8 Static keyword
The static keyword associates a member with the class rather than with each individual object.
A static method can be called without creating an instance.
Prefer accessing static members through the class name: Employee.company.

#### 9 final, finally, and finalize()
Keyword / Method	Meaning
final	Restricts modification, overriding, or inheritance depending on where it is used.
finally	A block that normally executes after try and catch, whether an exception occurs or not.
finalize()	An old method associated with garbage collection; it is deprecated and should not be used.

### 10 equals() and hashCode()
When overriding equals(), you should also override hashCode() consistently.
The contract requires that if a.equals(b) is true, then a.hashCode() and b.hashCode() must be equal

### 11 Shallow copy vs deep copy
- Shallow copy: Copies field values, so object references inside the copy may still point to the same nested objects.
- Deep copy: Copies the object and the relevant nested mutable objects so they are independently owned.

## 12 Coupling and cohesion
Coupling describes how strongly one component depends on another.
Cohesion describes how closely related the responsibilities within a component are.
Good software design generally aims for:
- Low coupling between independent modules.
- High cohesion within a class or module.
- Clear responsibilities and well-defined interfaces.
For example, a PaymentService should focus on payment-related responsibilities rather than also handling unrelated student registration and email template rendering.

#### 13 SOLID principles
SOLID is a set of five object-oriented design principles frequently discussed in interviews.
Principle	Meaning	Simple example
S — Single Responsibility	A class should have one coherent responsibility	Separate invoice calculation from invoice printing
O — Open/Closed	Open for extension, closed to unnecessary modification	Add a new payment implementation through an interface
L — Liskov Substitution	A subtype should be usable wherever its parent type is expected without breaking the parent's contract	A subtype must honor the behavior promised by its base type
I — Interface Segregation	Prefer small, focused interfaces over one large interface	Separate Printable and Scannable
D — Dependency Inversion	High-level logic should depend on abstractions rather than concrete implementation details	Inject a PaymentMethod instead of constructing a particular payment provider inside a service
These principles are guidelines for maintainable design, not rigid rules that must be applied identically in every class.