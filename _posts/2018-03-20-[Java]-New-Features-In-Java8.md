출처: http://www.baeldung.com/java-8-new-features

## 1. Overview

We have already covered:

- stream API: http://www.baeldung.com/java-8-streams-introduction
- lambda expressions and functional interfaces: http://www.baeldung.com/java-8-lambda-expressions-tips

Quick look at some oft the most interesting new features in Java 8:

- interface default and static methods
- method reference and Optional

## 2. Interface Default and Static Methods

Interfaces can have **static and default methods** that, despite being declared in an interface, have a defined behavior.

### 2.1. Static Method

It can't be overridden by an implementing class.

```java
static String producer() {
    return "N&F Vehicles";
}

String producer = Vehicle.producer();
```

### 2.2 Default Method

Using the new **default** keyword. These are accessible through the instance of the implementing class and can be overridden.

```java
default String getOverview() {
    return "ATV made by " + producer();
}

Vehicle vehicle = new VehicleImpl();
String overview = vehicle.getOverview();
```

## 3. Method References

### 3.1. Reference to a Static Method

- syntax: `ContainingClass::methodName`



Let's study stream API and lambda... first.

