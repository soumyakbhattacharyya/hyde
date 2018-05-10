---
title: Java 8 Lambda Expression Basics
date: 2018-05-10 00:00:00 Z
tags:
- jdk 8
layout: post
---

This articles helps you in understanding and using Java 8's Lambda expression in effective manner. 

To avoid detailing out mere theoretical concepts which are available in plenty in the internet, I have approached this in a more cookbook style way. There are 9 questions with accompanying answers. 

The answers uses code snippet to prove a point. The code is available here on Github. Each question has an expected familiarity level. If as a reader, you are just beginning to understand Lambda expression, choose questions requiring familiarity level as *beginner*; in case you are using Lambda expression in your code today, use familiarity level *practitioner*; and when you are quite familiar with the basics, use *advanced* as the required level.  

To solidify your understanding of the subject each answer is followed by two exercises/further research ideas. 

- beginner
  - What is Lambda expression and why should care?
  - How to construct and use a Lambda expression ?
  - What is functional interface?
- practitioner
  - How to decide which variation of functional interface you should use for your case?
  - How to implement exception handling strategy?
  - What is method reference?
  - How to get a "closure" like behavior?
  - What is the significance of *this* reference?
- pro
  - How can you write simpler collection processing code using Lambda expression?



**What is Lambda expression and why should I care?**

While designing a solution following OOP approach, *nouns* are mapped to Classes. Classes have attributes and **behaviors (functions that the class can perform)**. Therefore functions are only accessible within the context of a Class that contains it. 

In Java either the class needs to be instantiated to create an object of it. Thereafter the **instance method / function** can be invoked on the object reference. If the **function** is static there is no need of object instantiation. It can be directly invoked using class reference. 

What if we are only concerned about the **functions**. In OO world, to be able to use a function/behavior we need to have an adjoining class. 

For an example, lets think of greeting in various acts, as a function. Following interface declares the same.

```java
public interface ActOfGreeting {
	
	void perform();

}
```

Without using an anonymous inner class or a concrete class it is not possible to use this behavior. 

So lets implement the interface, using a class called HumbleAcOfGreeting as shown in following snippet

```java
public class HumbleActOfGreeting implements ActOfGreeting {

	@Override
	public void perform() {
		System.out.println("hello world :-)");

	}

} 
```

Now we can always instantiate an object of HumbleActOfGreeting and **use the method implementation** and though we are only concerned about the function implementation we need to have a full blown class to act as function container. 

This is the problem Lambda tries to address. Lambda enables you to author a function that does not belong to any class. A lambda expression can be passed around as if it was an object and executed on demand. This is clearly the first step towards treating a function as first class citizen, the very basis of functional programming. 

In summary following are key benefits of writing code using Lambda expression

> 1. enables functional programming
> 2. helps in writing more readable and concise code
> 3. author better API and libraries
> 4. enables parallel processing

As you see, would not it be amazing if you can directly author function that contains business logic, and reuse; all without requiring to have a parent class? If your answer is yes, proceed to the next section.



**How to construct/use a lambda expression? **

Lets start where we left in the previous question. The class that uses *act of greeting* function have following code

```java
public class Greeter {

	public void greet(ActOfGreeting greeting) {
		greeting.perform();
	}

	public static void main(String[] args) {

		Greeter greeter = new Greeter();
		ActOfGreeting actOfGreeting = new HumbleActOfGreeting();
		greeter.greet(actOfGreeting);

	}
}
```

The function that we are concerned about is following

```java
public void perform() {
		System.out.println("hello world :-)");
}
```

Lets construct a Lambda expression from this. Lambda expression, by promise should be a standalone function which can be assigned to a variable. The variable can be passed away as argument and used as and when necessary.

With that, it should look like following (not a compliable Java code )

```java
SomeType myLambdaVariable = public void perform(){ SOP("hello world :-)");}
```

**step-1**

Access modifiers are only relevant within the context of a Class. As Lambda expression does not belong to a Class there is no point of having access modifier. After removing access modifier above expression will look like following

```java
SomeType myLambdaVariable = void perform(){ SOP("hello world :-)");}
```

**step-2**

Compiler is able to derive judging from method implementation that the return type is void, hence explicit return type is not required as well. With that change the expression become

```java
SomeType myLambdaVariable = perform(){ SOP("hello world :-)");}
```

**step-3**

As the function gets assigned to a variable (which is used thereafter) there is no point in having name. After removing it the expression becomes 

```
SomeType myLambdaVariable = (){ SOP("hello world :-)");}
```

 **step-4**

Finally, by placing -> operator, between () and {} above, RHS represents a perfectly valid Lambda expression.

```java
SomeType myLambdaVariable = () -> { SOP("hello world :-)");}
```

The LHS of the expression needs a bit more discussion about Functional Interface, which we will do next.

**In case there are input arguments**

In above scenario, perform method takes no argument. What if the method *takes a String argument* and have following signature 

```java
public interface ActOfGreeting {
  void perform(String message);
}
```

In this case, the Lambda expression will require to be written as following

```
SomeType myLambdaVariable = (String message) -> { SOP("hello world :-) : " + message);}
```

As you see, the input argument with fully qualified type is mentioned inside () brackets. 

Following are few more examples in similar line

```
myFunc = (int i) -> {return i + 2;};
myFunc = (int i) ->  i + 2;
myFunc = (int i, int j) ->  i + j;
myFunc = (int i, int j) -> {if (j==0) return 0 else return i/j;}
```

**What is a functional interface**

A functional interface is an interface that has **exactly one** abstract method. Following is an example for the same 

```java
public interface ActOfGreeting {
  void perform(String message);
}
```

When we construct a Lambda expression, there has to be a target functional interface. A Lambda expression, therefore can be assigned to the functional interface it implements, as shown in following updated code snippet

```java
ActOfGreeting greeting = () -> {SOP("hello world :-)");};
```

Here after, greeting variable can be passed to any other methods/API which takes ActOfGreeting as input.

**Note** in case an existing functional interface have more than one abstract method, it stops being a functional interface any more, and compilation fails. However the interface can have [default method](https://docs.oracle.com/javase/tutorial/java/IandI/defaultmethods.html). 

Annotating an interface as @FunctionalInterface would raise compile time error if the interface is attempted to be changed so as to contain more that one abstract method. 

Following are few example highlighting the concept

| Functional Interface                                        | Lambda Expression                                       |
| ----------------------------------------------------------- | :------------------------------------------------------ |
| public interface IncreaseByTwo{ int increaseByTwo(int i);}  | (int i) -> {return i + 2;};                             |
| public interface Add{ int add(int i, int j);}               | (i,j) -> {return i+j;}                                  |
| public interface SafeDivide {int safeDivide(int i, int j);} | (int i, int j) -> {if (j==0) return 0 else return i/j;} |

