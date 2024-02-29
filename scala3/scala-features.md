# Scala Features

**Fun fact:** the name scala comes from "scalable"

There are three sections: 1. High-level Language Features, 2. Lower-level Language Features, and 3. Scala ecosystem

## 1. High-level Language Features

### A high-level language

- No low-level concepts like points, memory management (like Java!)
- Lambdas, high-order functions -> functional programming
  - Don't need imperative code

Lambda example:

```%scala
val oldNumbers = List(1, 2, 3)
val newNumbers = oldNumbers.map(_ * 2)
```

### Statically typed, but feels dynamic

```%scala
val s = "Hi"
val p = Person("Jane", "Doe")
val y = for i <- nums yield i * 2
```

Still get the benefits of static types:

- Most errors caught at compile-time
- IDE support: code completion, refactoring
- Method type declarations are more readable

### A functional programming language

- Functions can be passed like values
- Lambdas built in
- **Everything in Scala is an expression that returns a value**
- Standard library has immutable collection classes that return an updated copy of the data (_don't_ modify the collection)

### An object-oriented language

- Every value is an instance of a class, every operator is a method
- All types inherit from top-level class `Any`, whose children are `AnyVal` and `AnyRef`
  - No distinction between primitive and boxed types (unlike Java)

### Java Virtual Machine Client & Server

- Security
- Performance
- Memory management
- Platform independence
- Can use existing Java, JVM libraries

## 2. Lower-level Language Features

Scala 3 improvements over Scala 2

- Algebraic data types (ADTs) have more concise creation with enums
- More concise and readable syntax
- Clearer grammar

## Scala Ecosystem

- [Scaladex](https://index.scala-lang.org/): searchable index of Scala libraries
  - Web dev
  - HTTPS
  - JSON
  - Serialization
  - Data
  - AI, ML
  - Build Tools
