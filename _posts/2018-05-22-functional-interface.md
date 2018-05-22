---
title: Java 8 Functional Interfaces
date: 2018-05-22 00:00:00 Z
tags:
- jdk 8
layout: post
---

Java 8 onwards, the concept of <u>Functional Interface</u> has been introduced. There are well defined functional interfaces, available out of the box. **Agenda** for this blog is to understand, how can we design better java code, while using these out - of - the - box functional interfaces. 

To start, let's review the definition of a functional interface

- A functional interface is one that has excatly one abstract method

Further documentation about the functional interfaces is available here  https://docs.oracle.com/javase/8/docs/api/java/util/function/package-summary.html.

Following table summarizes significant functional interfaces from this package. 

| Interface Name    | Argument | Return  | Use Case                                                   |
| ----------------- | -------- | ------- | ---------------------------------------------------------- |
| Predicate<T>      | T        | boolean | Was the album published on 1978?                           |
| Consumer<T>       | T        | void    | Print name of the albums                                   |
| Function<T,R>     | T        | R       | Get the name of the first album by the band, Rolling Stone |
| Supplier<T>       | None     | T       | Generate a random number                                   |
| UnaryOperator<T>  | T        | T       | Convert *name* of an album to upper case                   |
| BinaryOperator<T> | (T, T)   | T       | Multiply two numbers                                       |

**Data Set**

To facilitate discussion, we have assumed following data source. It is a collection of  musical albums  compiled by Rolling Stone mangazine. Each entry has an associated year of publication, name, artist, genre and subgenre. 

| Year | Album                | Artist         | Genre | Subgenre                   |
| ---- | -------------------- | -------------- | ----- | -------------------------- |
| 1966 | Pet Sounds           | The Beach Boys | Rock  | Pop Rock, Psychedelic Rock |
| 1966 | Revolver             | The Beatles    | Rock  | Psychedelic Rock, Pop Rock |
| 1965 | Highway 61 Revisited | Bob Dylan      | Rock  | Folk Rock, Blues Rock      |

Code for the corresponding domain model is following
```java
public class RollingStoneAlbum {

private String year;
private String album;
private String artist;
private String genre;
private String subgenre;

// with getters and setters

}
```
With ground work done, lets deep dive.

**What is a *Predicate* interface and how can I use it?**

Predicate<T> interface often represents a condition that can evaluate either into a true or false. The generic type T represents the input type to the predicate. Depending on type of input, you may decide to choose a LongPredicate, DoublePredicate or IntPredicate where the first part of the predicate indicates the type of input. There is an interesting variation of Predicate<T>, being known as BiPredicate<T,U>. A BiPredicate represents what predicate represents, but can take two parameters instead of one.

Lets think of the use case, that we want to print the name of the album only if it is published before year 1970. A predicate expression for the same will be 

```java
Predicate<RollingStoneAlbum> isBefore1970 = (x) -> {return Year.of(Integer.parseInt(x.getYear())).isBefore(Year.of(1970));};
```

> explanation
>
> RollingStoneAlbum domain represents the input type for the predicate. Within the lambda expression, we are verifying if the publication year of a given RollingStoneAlbum *is before* the year 1970

Now if we want to create a print method that will print an entry if the predicate returns true, it might look like following

```java
public static void print(List<RollingStoneAlbum> list, Predicate<RollingStoneAlbum> predicate) {
		for (RollingStoneAlbum album : list) {
			if (predicate.test(album)) {
				System.out.println(album);
			}
		}
	}
```

As you can see, the if block, invokes the test method of the given predicate to assert if the function should print the name of album.

As a matter of fact, predicates can be combined using **AND** and **OR** constructs.

For an example, if we decide, at a later point in time to print ONLY those albums whose publication year is after 1965 and before 1970, we can very well write a second predicate

```java
Predicate<RollingStoneAlbum> isAfter1965 = (x) -> {return Year.of(Integer.parseInt(x.getYear())).isAfter(Year.of(1965));};
```

And combine them as

```java
isAfter1965.and(isBefore1970)
```

In summary, Predicates are used to design a lamda expression, that returns a boolean as outcome.



**What is a *Consumer* interface and how can I use it?**

Consumer<T> interface represents operations which *accepts* a single input argument and returns *no result*. Consumer operations operate via *side effect*. Side effect, in simple term is the change that occures to the state of the program, once the operation completes. 

For our case, lets design a consumer which will change name of the artist of a given album into upper case.

```java
Consumer<RollingStoneAlbum> consumer = (x) -> {x.setArtist(x.getArtist().toUpperCase());};
```

Observe, that the expression, *sets* or *mutates* an entry's name into upper case (side effect).

A print function for the same might look like following

```java
public static void print(List<RollingStoneAlbum> list, Consumer<RollingStoneAlbum> consumer) {
		for (RollingStoneAlbum album : list) {
			consumer.accept(album);
			System.out.println(album.getArtist());
		}
	}
```

As you can see, the print function accepts a Consumer, and once it applies the Consumer function, name of the Artist, is updated into upper case.

Like Predicate, there are Typed Consumer for Int, Double, Long input types. Consumers can be chained together to form a composite consumer. There is another variety of Consumer, that takes two arguments instead one, being called as BiConsumer.

In summary, Consmers are used to design lambda expression, that takes an argument, does not return anything, and creates impact via side effect.



**What is a *Function* interface and how can I use it?**

Function<T,R> represents a lambda expression that accepts one argument T, and produes a result R. For an exmaple if we want to design a funtion that returns a Map<String,Integer> (where the key, a String type, represents the name of the album and corresponding value, an Integer type, represents the number of letters in the name) by working out the list of RollingStoneAlbums it would look like following

```java
public static void functionExample(List<RollingStoneAlbum> list) {
		Function<List<RollingStoneAlbum>, Map<String,Integer>> function = (x) -> {
			Map<String,Integer> names = new HashMap<>();
			for (RollingStoneAlbum entry : x) {
				names.put(entry.getAlbum(), entry.getAlbum().length());
			}
			return names;
		};
		Map<String,Integer> names = function.apply(list);
		

}
```

I understand, it will take a bit of time to appreciate the power Function interface. Lets walk through the code, to understand it thoroughly

1. A typical function interface will have following signature Function<T,R>, where T is input and R is the output. In our case, Function<List<RollingStoneAlbum>, Map<String,Integer>> indicates that we are designing a function which will take a List<RollingStoneAlbum> as input and would return a Map<String,Integer>. 
2. Inside the lambda expression, we have initiatlized the Map that we have to return.
3. Further, we iterated the input type (the list of RollingStoneAlbums) and populated the Map with required data
4. Finally we returned the map

Function has few variations. It is worth making a deepdive to underatand the variation of this interface.

*Function* interface has default *compose* method that allows to combine several functions into one and execute them sequentially. Also, the interface have *andThen* method which serves opposite purpose of *compose* function. However, these two utility methods are the secret sauce that forms a *Higher Order Function*.

**What is a higher order function? How does it benefit me? How can I create one?** 

In simple term, a higher order function is one that is composed of more than one smaller individual functions being chained together. Let's assume we have function a and b.

- a.**andThen**(b) will create a higher order function, whereby a will execute first and then b will execute
- a.**compose**(b) will create a higher order function, whereby b will execute first and then a will execute

This *pipe* - ing of functions demand that output of one function is consumable by the next function.

- For a.**andThen**(b), higher order function, out put of a must be consumable by b

Let us assume that we want to create two functions, first one of which will extract name of the artist, given an album, find the number of letters in the name and store the outcome in a map of <u>artist</u> vs. <u>number of letters in the name of the artist</u>. Second function, will consume this map, and find the average number of letters in the name of an artist.

**First function**

```java
Function<List<RollingStoneAlbum>, Map<String, Integer>> buildArtistsVsNameCollection = (List<RollingStoneAlbum> x) -> {
			Map<String, Integer> names = new HashMap<>();
			for (RollingStoneAlbum entry : x) {
				if (null == names.get(entry.getArtist())) {
					names.put(entry.getArtist(), entry.getArtist().length());
				}
			}
			return names;
		};
```

**Second function**

```
Function<Map<String, Integer>, Double> findAverageLength = (x) -> {
			double numArtists = x.keySet().size();
			double totalLength = 0;
			for (Entry<String,Integer> entry : x.entrySet()) {
				totalLength = totalLength + entry.getValue();
			}
			return totalLength/numArtists;
		};
```

**Combining these together using compose()**

```java
Double averageLength = findAverageLength.compose(buildArtistsVsNameCollection).apply(list);
```

As you see, by joing the two functions we achive a goal which is bigger than individual capability of the functions.

**What is Supplier interface? What is it's purpose? How can I use it?**

Supplier interface helps us write code that executes *lazily*. It's get() method returns type T. Usually supplier interface wraps around a lambda expression, and executes it only when needed.



Apart from the functional interfaces above, there are unary and binary operators which serves the purpose of desining computation involving one and two inputs consecutively. In next part of this blog series we will explore the infamous stream abstraction and understand it in depth. 

Bye for now. 

Code: https://github.com/soumyakbhattacharyya/java8, feel free to check it out