# String Manipulation

## Learning Goals

- Introduce the String Pool and new `String` objects
- Introduce `String` constructors

## Introduction

We saw in the previous lessons a few methods that belong to the `String` class,
like the constructor that takes a character array, and the `toCharArray()`
method used to convert a `String` into a `char[]`.

Object creation can be costly in terms of program performance and memory utilization.
As a result, Java  has a special way of creating and managing `String` objects.
In this lesson, we will explore when and how Java creates `String` objects during program execution.

## Constructing Strings

Recall that we can initialize a `String` like this:

```java
String cat = "Tom";
```

This code uses a **string literal** "Tom" to assign a value to a `String` data type.

But what happens when we assigned multiple variables to the same string literal,
for example:

```java
public class StringPoolExample {
    public static void main(String[] args) {
        String cat1 = "Tom";
        String cat2 = "Tom";
        String cat3 = "Garfield";
    }
}
```

When we assign a string literal to a `String`, the Java Virtual Machine (JVM) looks for any other
`String` that has the same value (i.e. sequence of characters) in a special place in memory called a
**string pool**. The **string pool** is a part of the heap used to store
objects that represent string literals. If the JVM does not find another `String`
object in the string pool with that value, then it will create a new object.
But if it _does_ find another `String` with that value, Java will return a reference to its memory
address and not create another object.

![String Pool with Literals](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-strings/string_heap_1.png)

The JVM uses the String pool to avoid creating unnecessary objects
since each `String` is immutable and it is usually ok to have multiple variables
referencing the same `String` object.

NOTE: IntelliJ's Java Visualizer displays strings as literal values stored in the variable,
as if they were primitives:

![intellij string literals](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-strings/intellij_string.png)

However, the browser-based Java Visualizer at [https://pythontutor.com/java.html](https://pythontutor.com/java.html) lets us choose whether to display strings as literals
or as objects on the heap.  The default is to display them inline (i.e. as string literals),
but we can change that:

![strings on heap](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-strings/pythontutor_select_heap.png)

Selecting "render all objects on the heap" will make it easier for us to see how
the JVM avoids creating unnecessary objects for variables that are
assigned to the same string literal.  Use  [this link](https://pythontutor.com/visualize.html#code=public%20class%20StringPool%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20String%20cat1%20%3D%20%22Tom%22%3B%0A%20%20%20%20%20%20%20%20String%20cat2%20%3D%20%22Tom%22%3B%0A%20%20%20%20%20%20%20%20String%20cat3%20%3D%20%22Garfield%22%3B%0A%20%20%20%20%7D%0A%7D&cumulative=false&heapPrimitives=true&mode=edit&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false)
to step through the sample program and observe what happens when a variable wants to reference
a string literal that already exists on the heap.

## String Constructor

Now what if we want to have two different `String` objects with the same sequence of characters?
Java's `String` class actually comes with several constructors, so we can use the `new`
operator followed by a constructor call such as `String()`
to help instantiate different `String` objects with the same character sequence!

Recall that a constructor is a special
method used to initialize the fields of an object. A class can have several
constructors as long as the number or type of the parameters differ.
While the `String` class has over a dozen constructors, we will look at four:


```text
String()
String(String value)
String(Char[] value)
String(char[] value, int offset, int count)
```


Consider the example code below, which results in the instantiation of
one `char` array and five different `String` objects.

```java
public class StringConstructorExample {
    public static void main(String[] args) {
        char[] charArray = {'T', 'o', 'm', '-', 'C', 'a', 't'};
        String name = "Tom-Cat";
        String cat1 = new String();
        String cat2 = new String(name);
        String cat3 = new String(charArray);
        String cat4 = new String(charArray, 0, 3);
    }
}
```

By using the constructors, we are instantiating the `String` variables and
forcing Java to create new objects and store them in the heap space instead of
the string pool.

![String Pool with Literals and Heap](https://curriculum-content.s3.amazonaws.com/6676/java-mod2-strings/string_constructors.png)


To better explain some of these constructors, let's look at each one:

1. `String cat1 = new String();` - The first constructor does not take any parameters - this will assign
   the  variable `cat1` to a new `String` object that contains an empty character sequence; which we can show with two
   double quotes with nothing inside them: `""`.
2. `String cat2 = new String(name);` - The second constructor takes in a `String` as a parameter. This will
   assign the variable `cat2` to a new `String` object that contains the same sequence of characters as the
   `String` object referenced by `name`, i.e. "Tom-Cat".
   - Notice that `name` and `cat` reference different objects in memory,
     even though the character sequence they store "Tom-Cat" is the same.
   - The JVM *always* creates a new object when the `new` operator is used.
3. `String cat3 = new String(charArray);` - The third constructor takes in a character array
   as an argument and will convert the contents of the character array to a `String`
   as we saw in the previous lesson. In this case, `cat3` will be assigned to a new `String` object
   that contains the character sequence "Tom-Cat" as well.
4. `String cat4 = new String(charArray, 0, 3);`- The fourth constructor
   takes in 3 arguments: a character array, an integer
   to represent the offset, and another integer to represent the count. What
   this will do is it will take the character array and convert only part of it
   to a `String` object. The part it will create is defined by the offset and
   the count. The offset is the index of the array we want to start creating the
   `String` object at and the count is how many characters we expect to be in
   the object. In this case, we start the offset at index 0, so 'T', and we are
   expecting there to be 3 characters in the object. So we take the first 3
   characters from the `charArray` starting at index 0 and convert those into a
   new `String` object. This will assign `cat4` to "Tom". If the offset had
   started at 4 instead of 0, then the value of `cat4` would store the character
   sequence  "Cat" instead of "Tom".

You can step through the program to see each constructor in action at
[this link](https://pythontutor.com/visualize.html#code=public%20class%20StringCatExample%20%7B%0A%20%20%20%20public%20static%20void%20main%28String%5B%5D%20args%29%20%7B%0A%20%20%20%20%20%20%20%20char%5B%5D%20charArray%20%3D%20%7B'T',%20'o',%20'm',%20'-',%20'C',%20'a',%20't'%7D%3B%0A%20%20%20%20%20%20%20%20String%20name%20%3D%20%22Tom-Cat%22%3B%0A%20%20%20%20%20%20%20%20String%20cat1%20%3D%20new%20String%28%29%3B%0A%20%20%20%20%20%20%20%20String%20cat2%20%3D%20new%20String%28name%29%3B%0A%20%20%20%20%20%20%20%20String%20cat3%20%3D%20new%20String%28charArray%29%3B%0A%20%20%20%20%20%20%20%20String%20cat4%20%3D%20new%20String%28charArray,%200,%203%29%3B%0A%20%20%20%20%7D%0A%7D&cumulative=false&curInstr=0&heapPrimitives=true&mode=display&origin=opt-frontend.js&py=java&rawInputLstJSON=%5B%5D&textReferences=false).



## Resources

- [Java 17 String](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/String.html)   
- [Java 17 String constructors](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/String.html#constructor-summary)

