# Learn With Pluralsight: [Working with Classes and Interfaces in Java 11][url.course]

1. **Course Overview** [[GITHUB][m01.gh]]
2. Understanding Java Classes and Objects [[NOTE][m02.note]]
3. Implementing Class Constructors and Initializers [[NOTE][m03.note]]
4. Using Static Members
5. A Closer Look at Methods
6. Class Inheritance
7. More About Inheritance
8. Working with Enums
9. Creating Abstract Relationships with Interfaces
10. Nested Types and Anonymous Classes

## 2. Understanding Java Classes and Objects

### Basic Access Modifiers

| Modifier  | Visibility               | Classes | Members |
|-----------|--------------------------|---------|---------|
|           | only w/i own package     | Y       | Y       |
| `public`  | everywhere               | Y       | Y       |
| `private` | only w/i declaring class | N*      | Y       |

\* available to nested classes

## 3. Implementing Class Constructors and Initializers

### Default Initial State of Fields

| `byte`/`short`/`int`/`long` | `float`/`double` | `char`     | `boolean` | Reference types |
|-----------------------------|------------------|------------|-----------|-----------------|
| `0`                         | `0.0`            | `'\u0000'` | `false`   | `null`          |

### Number of Constructors

- must have at least 1
- when no explicit constructor, Java provides a default one (empty)

> when overloading and calling another constructor, call to `this()` must be the first line of the constructor  

### Initialization Blocks

- share code across all constructors
- cannot receive parameters
- place code w/i brackets outside any method or constructor
- a class can have multiple (they will all always execute)
- execute in order starting at the top of the source file

```java
public class Flight {
    private int passenger, seats = 150;
    private int flightNumber;
    private char flightClass;
    private boolean[] isSeatAvailable = new boolean[seats]; // we need to make all values true

    { // initialization block (will run no matter what constructor is used)
        for (int i = 0; i < isSeatAvailable.length; i++)
            isSeatAvailable[i] = true;
    }
    
    //...
}
```

[url.course]: https://app.pluralsight.com/library/courses/working-classes-interfaces-java

[m01.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/main
[m02.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/02-UnderstandingJavaClassesAndObjects
[m03.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/03-ImplementingClassConstructorsAndInitializers
[m04.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/04-UsingStaticMembers
[m05.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/05-ACloserLookAtMethods
[m06.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/06-ClassInheritance
[m07.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/07-MoreAboutInheritance
[m08.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/08-WorkingWithEnums
[m09.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/09-CreatingAbstractRelationshipsWithInterfaces
[m10.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/10-NestedTypesAndAnonymousClasses
