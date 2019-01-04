---
layout: default
title: Object Oriented Programming
---

# Object Oriented Programming

<!-- TOC -->

- [Introduction](#introduction)
- [1. Types, Objects, Classes](#1-types-objects-classes)
    - [Object construction](#object-construction)
- [2. Designing classes](#2-designing-classes)
    - [Encapsulation and access modifiers](#encapsulation-and-access-modifiers)
    - [Immutable objects](#immutable-objects)
    - [Parameterised types](#parameterised-types)
- [3. References and memory](#3-references-and-memory)
    - [Memory](#memory)
- [4. Inheritance](#4-inheritance)
    - [Casting](#casting)
    - [Inheriting fields and methods](#inheriting-fields-and-methods)
    - [Abstract classes](#abstract-classes)
- [5. Polymorphism and multiple inheritance](#5-polymorphism-and-multiple-inheritance)
    - [Multiple inheritance](#multiple-inheritance)
- [6. Lifecycle of an object](#6-lifecycle-of-an-object)
    - [Object Destruction](#object-destruction)
    - [Garbage collection algorithms](#garbage-collection-algorithms)
- [7. Java Collections and Object Comparison](#7-java-collections-and-object-comparison)
    - [Set interface](#set-interface)
    - [List interface](#list-interface)
    - [Queue interface](#queue-interface)
    - [Map interface](#map-interface)
    - [Iterators](#iterators)
    - [Object comparison](#object-comparison)
- [8. Error handling](#8-error-handling)
    - [Creating exceptions](#creating-exceptions)
    - [Assertions](#assertions)
- [9. Copying Objects](#9-copying-objects)
    - [Variance of return types](#variance-of-return-types)
- [10. Language evolution](#10-language-evolution)
    - [Generics](#generics)
    - [Anonymous functions](#anonymous-functions)
    - [Streams](#streams)
- [11. Design patterns](#11-design-patterns)
    - [Singleton](#singleton)
    - [Decorator](#decorator)
    - [Composite](#composite)
    - [Observer](#observer)
    - [State](#state)
    - [Strategy](#strategy)
- [12. The JVM and Bytecode](#12-the-jvm-and-bytecode)
<!-- /TOC -->

*Some images herein have been borrowed from the Course Handout*

## Introduction 

- **Declarative languages** specify what to do, not how to do it. Functional languages like ML are a subset of declarative languages: the compiler may replace your implementation with something else (e.g tail optimisation).
- **Imperative languages** require you to specify exactly how a computation should be done. 
- **Procedural programming** is a paradigm within declarative programming in which statements are grouped into procedures that manipulate state.
- **Object oriented programming** is an extension to procedural programming which also groups certain aspects of the state with the procedures. 
- ML code consists of expressions which evaluate to values, which are **immutable**. Java uses statements and expressions with mutable state. 

## 1. Types, Objects, Classes 

Most languages will have some **primitive types** to signify what is in the memory. A **variable** is a name used to refer to a specific instance of the type. In Java:

- boolean (1 bit)
- char - 16 bits
- byte - 8 bits signed (-128 to 127)
- short - 16 bits signed 
- int - 32 bits signed
- long - 64 bits signed
- float - 32 bits floating point
- double - 64 bits floating point

It is safe to perform **widening conversions** between types, but **narrowing** will not compile. 

```java
int x = 200;
long y = x;  // safe
byte z = x;  // will raise Incompatible types error 
byte z = (byte) x;  // will compile but z = -56.
```

Java is **statically typed**, meaning that every variable name must be bound to a type when it is first declared.
  

### Object construction

**Classes** are used to represent concepts, grouping state and behaviour. To be precise, the state takes the form of **fields** and behaviour is described in **methods**.  

**Objects** are instances of classes, and can be made using a call to the **constructor** method. 

```java
public class Vector2d {
    float x; float y;  // Fields
    
    Vector2d(float xi, float yi) {
        this.x = xi;  // 'this' can be used to disambiguate
        this.y = yi;
    }
}
    
// Creating an object by calling the constructor
Vector2d v1 = new Vector2d(3.0, 4.0);
```
- The constructor has the same name as the class, and has no return type. 
- In general, we should minimise the work done by the constructor.
- If no constructor is defined, Java generates a blank **default constructor** with no arguments. The fields all get set to the 'zero' value for the type, e.g 0 for int, "" for string.

The **prototype** of a function refers to the function name, arguments, and return types. Functions can be be **overloaded** to support different arguments (possibly with a different return type) – this also applies to the constructor. 

When state is more logically associated with a class than an object, we can make a **static field** (same as class variable in python). These are most suited to class-related constants. 

We can also have **static methods**, which do not require any instance-specific state. These can be called without instantiating the class, e.g `Math.sqrt()`. Static methods tend to be easier to debug.

## 2. Designing classes

Classes should logically group state (nouns) and behaviour (verbs). We can visualise thus using a UML Class diagram

<center>
<img src="{{ site.imageurl }}note_img/uml1.png" style="width:80%;"/>
</center>

- Protected fields and methods are preceded by `#`
- Write `static` next to the return type if needed.
- Interfaces should have `<<interface>>` at the top
- To model "has-a" associations, we use open arrowheads and annotate the multiplicity. In real terms, "has-a" means that an object contains a reference to other object(s).

<center>
<img src="{{ site.imageurl }}note_img/uml2.png" style="width:80%;"/>
</center>

In addition to classes, for added modularity we can use **packages**, i.e directories containing multiple class files. This allows us to develop, test and update functionality independently.  

### Encapsulation and access modifiers

**Coupling** is how much one class depends on another (bad). **Cohesion** is how strongly related everything in a class is (good).

Encapsulation refers to hiding some parts of the internal state using **access modifiers**, in order to reduce coupling. In general, all state should be **private** unless there is a good reason to the contrary. 

<center>
<img src="{{ site.imageurl }}note_img/access.png" style="width:80%;"/>
</center>

We can interact with private fields from outside the class by using **getters** and **setters**. The advantage of a setter is that it can provide sanity checks for modification.

### Immutable objects

If we have private fields and remove any setters, they become **immutable**. The advantages of this are:

- Easier to construct, test and use. 
- Can be used for concurrency
- Allows lazy instantiation 

By default, arrays are *not* immutable, but strings *are*.

### Parameterised types

Although java does not have full type inference, we can define polymorphic functions using **generics**, which defines classes with a placeholder type. This is implemented under the hood using **type erasure**.

```java
public class Test<T> {
	private T t;
	
    public void set(T t){
        this.t = t;
    }
    public T get(){
        return this.t;
    }
}
```

There is then some limited type inference when instantiating a generic class: 

```java
LinkedList<Vector3D> ll = new LinkedList<>();
```

## 3. References and memory

All compilers map a variable name to a specific memory address – the type determines how many blocks the data tasks. Pointers are variables which contain memory addresses, which can then be manipulated. 

A **reference** is an *alias* for some data – it is either assigned (and therefore valid) or unassigned. References are implemented using pointers, just that the compiler ensures memory safety.

<center>
<img src="{{ site.imageurl }}note_img/pointervref.png" style="width:80%;"/>
</center>

In Java, *everything is either a reference or a primitive*.

### Memory 

Memory is represented by a grid of cells. 

<center>
<img src="{{ site.imageurl }}note_img/memory_diagram.png" style="width:80%;"/>
</center>

- When a method is called, it is allocated a portion of memory called a **stack frame**.
- Any local variables will go in that frame. New objects created go on the heap (*except strings*), but their reference will be stored in the stack. 
- If new methods are called, a new stack frame is created. This stack frame will contain the input parameters as well as the **return address**.
- When a method call terminates, it hands back control to the previous item on the stack via the return address.  

Only primitive types can be stored on the stack: everything else goes on the heap. 



## 4. Inheritance 

Inheritance is the mechanism by which new classes can take on the properties of existing classes – we can create a **base class** such that new classes **inherit** state, behaviour, and type. This reduces code duplication and allows us to represent conceptual hierarchies. 

Java is a **nominitive type language** - things must have the right name. By contrast, **structural typing** (e.g in Haskell) examines whether the internal structure is the same, so the below code would compile. **Duck typing** (e.g in python) is a subset of structural typing but only checks at runtime.  

```java
class Foo {
  public void method(String input) { }
}
class Bar {
  public void method(String input) { }
}
Foo f = new Bar(); // ERROR!!
```

In contrast to the "has-a" relationship, inheritance is described by empty arrowheads that describe the "is-a" relationship.

<center>
<img src="{{ site.imageurl }}note_img/inheritance_uml.png" style="width:100%;"/>
</center>


### Casting 

By definition, any instance of the superclass could be replaced by an instance of the subclass. This means that **widening** casts/conversions are acceptable.

```java
// Explicit 
Child c = new Child();
Parent p = (Parent) c;  // Redundant cast

// Implicit 
public void doStuff(Parent p) {...};
doStuff(new Child()); 
```

By contrast, **narrowing conversions** *may* fail. This is because the child may have some attributes that the parent doesn't, so the parent class may not be able to represent the child.

```java
Parent p = new Parent();
Child c = (Child) p; // Fails AT RUNTIME

Child c = new Child();
Parent p = (Parent) c; // Widening cast is fine
Child c2 = (Child) p; // Ok! 
```

When an object is created, Java stores the information at a specific memory location (returning a reference). Casting just creates a new reference with a different type. If we try to cast a parent to a child, there won't be the necessary information in memory, but this is *not caught by the compiler* and will lead to a **runtime error**.

### Inheriting fields and methods

All fields are inherited except for private fields (JLS 8.2). Subclass objects actually do contain the superclass's private fields, but do not have access to them. 

**Shadowing** is when a field in the subclass has the same name as a field in the parent class. We can specify which field we want to access using `this` and `super`.

When a subclass has a method of the same name as a parent method, we say that the method is **overridden**. It is best practice to use the `@Override` annotation. 

### Abstract classes

- An abstract class is a base class that provides access to some **abstract methods**, which provide no implementation and *must be overridden*. 
- Abstract classes cannot be instantiated, but can include non-abstract methods and fields which will be inherited.
- An abstract class does not have to contain any abstract methods – in this case `abstract` only serves to prevent instantiation. 
- Abstract classes/methods are represented on a UML diagram with italics. 


## 5. Polymorphism and multiple inheritance

When a subclass overrides a parent method and we refer to the object by the parent type, which method should we run?

- **Static polymorphism** is decided at compile time and chooses based on the static type (default in C++)
- **Dynamic polymorphism** (a.k.a **dynamic dispatch**) runs the child method because the compiler sees that the object is really from the subclass. 
    - java does this for methods by default: adds a performance cost at runtime 
    - does not apply to private, final or static methods.
    - does not apply to fields


### Multiple inheritance 

Multiple inheritance allows classes to inherit from multiple parents. Java *does not* support this because of the diamond problem: if the two parents of a child class have the same method (which they have overridden from their common parent), which method should be inherited by the child class? 

Java's alternative is **interfaces**, which are like special classes which:

- only support type inheritance
- have no state 
- only have abstract methods (public by default) which must be overridden
- (actually may have **default methods** which provide a default implementation)


They are represented on UML with a `<<interface>>` label, and are generally named as adjectives. 

<center>
<img src="{{ site.imageurl }}note_img/interfaces.png" style="width:50%;"/>
</center>

```java
interface Drivable {
    void turn();
    void brake();
}

interface Identifiable {
    void getIdentifier();
}

class Bicycle implements Drivable {
    public void turn() {...};
    public void brake() {...};
}

class Car implements Drivable, Identifiable {
    public void turn() {...};
    public void brake() {...};
    public void getIdentifier() {...};
}
```

## 6. Lifecycle of an object

When an object created, Java goes through the following process:

1. Load class (if not already loaded), allocate static fields, and run static initialisers.
2. Allocate memory for object
3. Run initialiser blocks 
4. Run constructor

<center>
<img src="{{ site.imageurl }}note_img/object_creation.png" style="width:80%;"/>
</center>

When inheritance is involved: 

- Parent and child classes loaded. 
- Memory allocated for child object, with the parent object inside. 
Parent fields initialised first, then parent constructor, then child fields, then child constructor. 
- All constructors *must* have a `super()` call – the compiler will add one if not provided.

### Object Destruction 

**Deterministic destruction** involves objects being deleted at predictable times, e.g manual deletion or automatic deletion when out of scope. Objected oriented languages sometimes have a **destructor**, which frees resources when an object is destroyed. This supports **RAII** (resource acquisition is initialisation), in which access to a resource follows an object's lifecycle. 

Java does not do this. Java instead uses **garbage collection** to automatically delete objects. Though there are **finalisers** (deprecated), these are not guaranteed to run and are very unreliable. As an alternative to RAII, Java offers **try-with-resources**:

<center>
<img src="{{ site.imageurl }}note_img/try-with-resources.png" style="width:100%;"/>
</center>


The garbage collector is a separate process that monitors the program. It will slow down a program but helps minimise memory leaks. The different philosophies are:

1. **Stop the world** – pause the operation of the program to delete objects 
2. **Incremental** – garbage collect in multiple phases letting the program run in the meanwhile 
3. **Concurrent** – no pause. Ideal but hard to implement.


### Garbage collection algorithms

The original Java garbage collector uses **reference counting**. It keeps track of how many references point to a given object; if there are none, the object is deleted. 

Problems with reference counting: 

- There is a performance cost because each object needs more memory and changes to references must be monitored. 
- If two otherwise unreachable objects point to each other (**circular references**), the garbage collector will not delete them.

An alternative is to use **mark and sweep tracing**:

1. List all immediate references
2. Follow each reference recursively, marking the object if visited.
3. Delete all unmarked objects. 

A potential improvement is the **generational garbage collector**, which classifies objects as short/long lived and collects the short ones more frequently. Objects can be promoted from short to long.


## 7. Java Collections and Object Comparison

Java is a platform that offers a large class library. The Collections Framework in particular offers the **Iterable** and **Collection** interfaces (`Iterable` is the parent). They define operations such as `add(Object o)`, `clear()`, `next()`, `isEmpty()` – if a particular class doesn't offer the operation, it will `throw UnsupportedOperationException`.

These operations only work on objects, not primitives. The workaround is **autoboxing**: for each primitive type, Java provides an object wrapper (e.g `Integer`, `Boolean`). The compiler autoboxes and unboxes, replacing the primitive with its object wrapper and vice versa as needed.

```java
List<Integer> li = new ArrayList<>();
for (int i = 1; i < 50; i++)
    li.add(i);  // int is autoboxed before insertion
```

Note on the LHS we used `List` rather than `ArrayList` – this is acceptable because `ArrayList` inherits from `List` (widening/upcasting).


### Set interface

Inheriting from `Collection`, represents a collection of unique elements

- `TreeSet` stores objects in order. All operations $O(\log n)$.
- `HashSet` stores objects unpredictably but with $O(1)$ access and addition

### List interface

A list is an ordered collection of elements that may contain duplicates. 

- `LinkedList` is a double linked list.
    - access and checking existence is $O(n)$ because you have to walk the linked list
    - addition is $O(1)$.
    - deletion is $O(1)$ for the head, $O(n)$ otherwise
- `ArrayList` uses an array:
    - main benefit is $O(1)$ access
    - addition is $O(1)$ **amortised**: occasionally a new array will have to be created.
    - deletion is $O(n)$ because items must be shifted

### Queue interface 

An ordered collection that supports removal of elements from the head (`poll`) and addition to the back (`offer`). `peek()` can be used to look at the head without removal. 

- `LinkedList` has $O(1)$ `offer` and `poll`
- `PriorityQueue` adds a notion of priority so more important elements move to the top.

### Map interface

The map interface provides dictionary functionality: mapping unique `key` objects to `value` objects. `m.keySet()` and `m.values()` get the keys and values respectively.

- `TreeMap` keeps the items in order with $O(\log n)$ operations
- `HashMap` is not ordered but has $O(1)$ operations.  

### Iterators 

Because `Collection` inherits from `Iterable`, we can iterate over a collection using a **foreach loop**:

```java
for (Integer i : list) {
    System.out.println(i);
} 

for (Map.Entry<String, String> entry : map.entrySet())
{
    System.out.println(entry.getKey() + "/" + entry.getValue());
}

hash.forEach((key, tab) -> /* do something with key and tab */);
```

When iterating over a collection, it is dangerous to change the structure (e.g by deleting). To do this we must use the `Iterator` class, which supports `hasNext()`, `next()` and `remove()`, but does not support `forEach`.

```java
Iterator<Integer> it = list.iterator();
while (it.hasNext()) {
    it.remove(); // Safe with Iterator
}
```

### Object comparison

Comparing primitives is straightforward. However, in many cases we will want to compare/sort objects. 

By default java tests **reference equality**, i.e whether the two references point to the same chunk of memory. If we want to test whether the values of the objects are equal, we must override the `equals()` method in object (default is reference equality).

```java
@Override
// Compare the current object to another object o 
public boolean equals(Object o) {
    return ((o.name == this.name) && (o.field == this.field))
}
```

Objects in Java have a `hashCode()` method which should return the same result if the objects are equal. Thus you should override `hashCode()` whenever you override `equals()`.

In order to compare objects, their class should implement the `Comparable<T>` interface, overriding the `int compareTo(T o)` method. 

- If the object is less than `o`, return negative
- If the object equals `o`, return zero
- If the object is more than `o`, return positive
- If `o` is null, throw `NullPointerException`.

Implementing `Comparable` defines a **natural ordering** for the class, which should be consistent with equals and define a **total order**:

- Antisymmetry: $a \leq b \text{ and } b \leq a \implies a = b$
- Transitivity: $a \leq b \text{ and } b \leq c \implies a \leq c$
- Connex: $a \leq b \text{ OR } b \leq a$

However, we may want to be able to sort the same class in different ways. For this we use a **Comparator**. This is a class which implements the `Comparator<T>` interface:

```java
class AgeComparator implements Comparator<Person> {
    public int compare (Person p1, Person p2) {
        return (p1.age - p2.age);
    }
}

Collections.sort(plist); // sort with compareTo
Collections.sort(plist, new AgeComparator()); // sort with comparator
```

Java does not support operator overloading.

## 8. Error handling

Errors fall broadly into three categories: syntactic, logical (bugs), external. Mostly we dicuss logical errors, which can be reduced by:

- unit testing: OOP allows for modular testing
- assertions to mark things that must be true if the algorithm is working
- defensive programming styles e.g code reuse
- pair programming

Traditional imperative languages deal with errors using **return codes**. But you have to constantly check what return values signify, and it means code can't conveniently just use the result of a method. 

Alternatively, we can use **deferred error handling**, in which an error sets some state, which can be later checked. 

However, the most common form of error handling is to use **exceptions**, which are *thrown* by a method then *caught* later. 

```java
double divide(double x, double y) throws DivideByZeroException {
    if (y==0.0) throw new DivideByZeroException();
    else return x/y;
}

try {
    double z = divide(x,y);
    // more code
} catch (DivideByZeroException | NumericalException d ) {
    // handle error here
    // 'more code' does not run if there is an error.
} catch (OtherException e) {
    // handle
} finally {
    // ALWAYS CALLED
}
```

### Creating exceptions 

To create an exception, we can extend `Exception` and provide a custom constructor. 
Java offers **exception chaining**: if one exception causes another, we can include the first one as data in the second one by having a `Throwable` argument in the constructor:

```java 
public class ComputationFailed extends Exception {
    public ComputationFailed(String errorMsg, Throwable t) {
        super(errorMsg, t);
    }
}

try {
    // some code that may divide by zero
} catch (DivideByZeroException d) {
    throw new ComputationFailed("Other IOException", d);
}
```

**Checked exceptions** must be declared then either handled or passed up. A function with a checked exception must be wrapped in `try-catch` when called. **Unchecked exceptions** extend from `RunTimeException` and are used for programming errors, such as `NullPointerException`. **Errors** typically arise from the JVM and should not be caught.


Important points about exceptions:

- Exceptions should never be used for control flow because the JVM has to unroll the stack and it is not readable. 
- Checked exceptions must be handled, otherwise you may forget to do so. If you really don't think it can happen, throw a `RunTimeException`.
- *Never* circumvent exception handling with `catch(Exception e) {}`.


Advantages of exceptions:

- Class name can be much more descriptive than error codes, and error can contain a lot of useful information. 
- Doesn't interrupt flow of code by internal checks 
- Strongly discourages ignoring errors. 
 
 Disadvantages of exceptions: 
 
 - Surprising control flow as they can be thrown anywhere
 - Not suited to multithreading. 
 - Doesn't unroll state change, only control flow. 

### Assertions 

Type of error checking designed for debugging. If the statement being asserted is `false`, the program ends. They *should not* be used in production and are disabled by default. They can be enabled/disabled with the `-ea` and `-da` flags when running a program. 

They are particularly useful for **postconditions**, i.e things that should be true at the end of your algorithm if it is working. They should only be used to test programmer error, and should not have side effects. 

## 9. Copying Objects

All classes in Java have the `Object` class as their ancestor. `Object` provides a `clone()` method, but we can only call this if we implement the `Cloneable` interface. `Cloneable` is a **marker interface** that doesn't have any code in it, but is used to label a class so that other functionality knows that it is cloneable. 

By default this is set to a shallow copy, so all reference types will still point to the same data. After implementing `Cloneable`, we should override `clone`. 

```java 
@Override
public Object clone() {
    MyClass cloned = (MyClass) super.clone(); // Copies primitives etc
    cloned.attr = deepCopy(this.attr); // deep copy reference types
    return cloned;  // this is of type MyClass... see below
}
```

The **principle of substitution** says that a reference to a superclass can always be replaced by references to its subclass, so rather than returning an `Object` we can return something more specific.

Although `clone()` is `protected` in `Object`, by overriding it we can weaken the access modifier.


An alternative to cloning is to use **copy constructors**, which accepts an object of the same type and manually copies data.

```java
public class MyClass {
    int x; int y;
    public MyClass(MyClass m) {
        this.x = m.x; 
        this.y = m.y;
    }
}
```

### Variance of return types 

Loosely speaking:

- **Covariance** refers to accepting subtypes in place of supertypes
- **Contravariance** refers to accepting supertypes in place of subtypes – this is *not supported by Java*, and any method attempting to do this will be overloaded.

Java only supports **covariant return types**, not **covariant parameter types**.

```java
public class A {
    Object work(Object o) {...}
}

public class B extends A {
    @Override 
    Person work(Object o) {...} // covariant return = ALLOWED
    
    @Override
    Object work(Person p) {...}  // covariant parameter = NOT ALLOWED
}
```
 
Java arrays are covariant: an array of type `T[]` can contains elements of type `T` or any subtype `S`. Additionally, `S[]` itself is a subtype of `T[]`.


## 10. Language evolution 

When implementing new features, a key issue is backward compatibility. For example, java initially included `Vector` as an expandable array. It has since been completely replaced by `ArrayList`, but only remains for backwards compatibility. 

### Generics

Initially, `Collections` dealt with objects only. But this requires constant casting and can cause accidental type mixing in the collection. 

In order to fix this while maintaining backwards-compatibility, Java uses **type erasure**, such that within a generic, the compiler deletes all instances of the generic type so that it is converted into the old code. 

<center>
<img src="{{ site.imageurl }}note_img/type_erasure.png" style="width:100%;"/>
</center>

This explains why primitives cannot be passed directly, and must first be boxed.

Because of this type erasure, generics must be **invariant**. Unlike arrays, `List<S>` is not a subtype of `List<T>` even if `S` is a subtype of `T`.

```java
ArrayList<String> strList = new ArrayList<>();
ArrayList<Object> superList = strList; // NOT OK
```

### Anonymous functions

Java 8 added support for syntax reminiscent of functional programming, which can reduce the boilerplate. In many cases we will only want to use a function or object once, so it is unnecessary to define a method/class just for that purpose.

For example, consider a simple GUI with a clickable button. We thus need to register an `ActionListener` which increments a counter whenever the button is clicked. The obvious way to structure this is:

```java
class ButtonCounter implements ActionListener {
    // Fields and constructor 
    
    @Override
    public void actionPerformed(ActionEvent e) {
        counter++;
        button.setText(String.valueOf(counter));
    }
}

public class Gui {
    // Fields
    
    Gui() {
        button = new JButton("mybutton");
        button.addActionListener(new ButtonCounter(button));
    }
    // Other GUI stuff
}
```

However, `ButtonCounter` is only needed as this specific part of the GUI. Thus we can improve using an **inner class**, in which we simply move `ButtonCounter` into `Gui` (its fields become Gui's fields). This can increase encapsulation, because inner classes can be given access modifiers.

However, because we only need to register this listener once, we have wasted effort in creating a reusable class. An **anonymous inner class** defines one on the spot. 

```java 
public class Gui {
    // Fields 
    
    Gui() {
        button = new JButton("mybutton");
        button.addActionListener(
                 new ActionListener() {
                     @Override
                     public void actionPerformed(ActionEvent e) {
                         button.setText(String.valueOf(counter++));
                     }
                 });
    }
    // Other GUI stuff 
}
```

But this whole class has been defined just to get the `actionPerformed` functionality! We can use a **lambda function** instead:


```java
public class Gui {
    // Fields 
    
    Gui() {
        button = new JButton("mybutton");
        button.addActionListener(
                a -> button.setText(String.valueOf(counter++))
        );
    }
    // Other GUI stuff
}
```

This is only possible because `addActionListener()` implements a **functional interface**. Such an interface only has one abstract method – a lambda function is an instance of a functional interface.

- Lambdas can work without parameters: `() -> something();`
- Lambdas can take multiple parameters: `(x, y) -> x+y;`
- Lambdas can replace comparators. 
- These functions can be composed using `andThen()`
- An **expression lambda** has some expression on the right hand side, whereas a **statement lambda** can have multiple statements (see below)
- Lambdas can sometimes be replaced with **method references**, if the only thing the lambda does is calling another method.


```java
List<String> list = new LinkedList<>();

// Expression lambda 
list.forEach(s -> System.out.println(s + s));

// Statement lambda 
list.forEach(s -> {s = s.toUpperCase();
                   System.out.println(s);});
                   
// Method reference
list.forEach(System.out::println);
```

Different functional interfaces allow functions to be treated as 'values', with different characteristics.

```java 
// No arguments, void return
Runnable r = () -> System.out.println("Some state change");
r.run()

// No arguments, non-void return 
Callable<Double> pi = () -> 3.141;
pi.call();

// One argument, non-void return
Function<String, Integer> f = s -> s.length();
f.apply("Some input string");
```
 
### Streams 

Collections can be made into **streams**, which we can then **filter** and **map** (though they get consumed in the process).

```java
// 1. Create stream from collection
// 2. Element-wise operate
// 3. Aggregate
List<Integer> list = new ArrayList<>(List.of(1,2,3,4,5));
list.stream().map(x -> x*x).collect(Collectors.toList());
```

## 11. Design patterns

A **design pattern** is a general reusable solution to a commonly occurring problem in software design. Many of them follow the **Open-Closed principle**, that classes should be open for extension but closed for modification. There are three classes of design pattern:

- **Creational patterns** - creating objects, e.g Singleton
- **Structural patterns** - composition of objects, e.g Decorator, Composite 
- **Behavioural Patterns** - object interaction, e.g Observer, State, Strategy.

### Singleton

How can we ensure that only one instance of an object is created by developers using the code? e.g database connections.

The Singleton pattern does this by having a private constructor and storing the only instance as a private static final field. 

```java 
public class Singleton {
    private static final Singleton instance;
    
    public static getInstance() {
        if (instance == null ) {
            instance = new Singleton();
        }
        return instance;
    } 
    private Singleton() {} // private constr blocks creation
}
```

We can then call it with `Singleton.getInstance()`. We can improve this to make it lazy by initially setting `instance = null` then creating if null in `getInstance()`.


### Decorator

How can we add state or methods at runtime? e.g supporting gift-wrapped books in an online store.

This pattern describes how we can **Decorate** implementations of some **Component** interface. 

- An abstract `Decorator` class inherits from `Component`, but also keeps a reference to a concrete component. 
- Subclasses of the decorator will call the component's methods via `super` but will also add their own state and functionality 

<center>
<img src="{{ site.imageurl }}note_img/decorator_pattern.png" style="width:100%;"/>
</center>

### Composite 

How can we treat a group of **Component** objects as a single **Composite** object?

- `Composite` has to inherit from the abstract `Component` class, but also maintains a list of `Component` subclass instances, which are called **Leafs**
- Functions within `Composite` can then be defined to affect all `Leaf` objects, e.g 

```java
for (Component c: children) c.operation();
```

<center>
<img src="{{ site.imageurl }}note_img/composite_pattern.png" style="width:80%;"/>
</center>
  

### Observer 

When an object changes state, how can other objects know? e.g action on click in a GUI.

This pattern refers to how an **observer** can monitor changes in the state of a **subject**. It is straightforward conceptually: 

- Subject maintains a list of observers and tells them to update when there is a state change. 
- Each observer maintains a subject instance. When told to update, it gets the state from the subject. 

<center>
<img src="{{ site.imageurl }}note_img/observer_pattern.png" style="width:80%;"/>
</center>


### State

How can we let a **Context** object alter its behaviour when its internal **state** changes? e.g pressing the play button does something different depending on whether the music is on or off. 

- `Context` keeps a private reference to an object of type `State` (which can refer to its subtypes as well). When we want to run a particular method, we call `state.action()`.
- `State` is an interface that contains the abstract methods which depend on state. 
    - We then implement concrete states (e.g `StartState` and `StopState`), which override `action()` with their own implementation. 
    - If these subclasses are to change the state, they need to be passed an instance of `Context` so we can `setState()`.

<center>
<img src="{{ site.imageurl }}note_img/state_pattern.png" style="width:80%;"/>
</center>

### Strategy 

How can we select an algorithm implementation at runtime? 

The Strategy pattern is designed in the same way as State (with `Context` being used as a 'switcher'), however its main purpose is to reduce code repetition during development:

- Strategies will not depend on internal state
- Strategies may not involve different functionality – they may just be different implementations.
- While State allows for different functionality at runtime, Strategy assumes that the implementation is selected at compile time. 


## 12. The JVM and Bytecode

In order to build a distributable platform, we can use an interpreter, but this is not as efficient as a compiled language. Java uses a hybrid model:

- Developer writes in Java, then runs the compiler.
- The compiler returns bytecode, which is platform-independent and can be distributed. 
- The JVM converts bytecode into machine code for a particular architecture.

The advantages of bytecode:

- Not easy to reverse engineer, so protects application logic. 
- The JVM contains many libraries, so bytecode will be relatively small. 
- Interpreting bytecode is relatively fast (though not as fast as running native code). 
