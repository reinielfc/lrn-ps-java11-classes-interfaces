# Learn With Pluralsight: [Working with Classes and Interfaces in Java 11][url.course]

1. **Course Overview** [[GITHUB][m01.gh]]
2. Understanding Java Classes and Objects [[NOTE][m02.note]]
3. Implementing Class Constructors and Initializers [[NOTE][m03.note]]
4. Using Static Members [[NOTE][m04.note]]
5. A Closer Look at Methods [[NOTE][m05.note]]
6. Class Inheritance [[NOTE][m06.note]]
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

## 4. Using Static Members

- static members are shared class-wide
- not associated w/ individual instances
- declare using `static` keyword

### Static Members

- **static field**
  - value not associated w/ specific instance
  - all instances access the same value
- **static method**
  - performs action not tied to a specific instance
  - has access to static members only
- **static import**
  - used w/ static methods
  - allows method name to be used w/o being class qualified
    - `import static com.pluralsight.flightapp.Flight.getAllPassengers();` 
    - `Flight.getAllPassengers();` -> `getAllPassengers()`

### Static Initialization Blocks

- perform one-time type initialization
- executed before type's first use
- has access to static members only

```java
public class Flight {
    private int passengers, seats = 150;
    private static int allPassengers, maxPassengersPerFlight;
    
    static {
        AdminService admin = new AdminService();
        admin.connect();
        maxPassengersPerFlight = admin.isRestricted()
                ? admin.getMaxFlightPassengers() // value is fetched only once
                : Integer.MAX_VALUE;
        admin.close();
    }
}
```

## 5. A Closer Look at Methods

- there's automatic type conversion when calling methods, e.g. the following method can accept a `short` type
  - `void methodName(int num) { ... }`
- variable number of parameters
  - `public void addPassengers(Passenger... list) {}`
  - `flight.addPassengers(luisa, john, alice)`

## 6. Class Inheritance

### References to Derived Class Instances

- can be assigned to base class references
- affects available features
  - dictated by type of reference being used to access the instance
  - `Flight f = new CargoFlight();

### Field Hiding

- if your derived class declares a field that has the same name as a field in the base class

```java
public class Main {
  public static void main(String[] args) {
    Flight f1 = new Flight();
    System.out.println(f1.seats);   // 150
    
    CargoFlight cf = new Flight();  // (extends Flight)
    System.out.println(cf.seats);   // 12
    
    Flight f2 = new CargoFlight();
    System.out.println(f2.seats);   // 150
  }
} 
```

### Method Overloading

- key difference b/w methods and fields
  - for field hiding, it's the type of the _reference_ that determines which version of the field you use
  - for method overriding, it's the type of the _instance_ that determines which version of the method you use

### Object Class

- root of the Java class hierarchy
- every class has characteristics of Object
- Object references can reference any array or class instance
  - in Java, arrays are a type of class

### Object References 

```java
class Main {
  public static void main(String[] args) {
    Object o = new CargoFlight();
    // ...
    if (o instanceof CargoFlight) {
        CargoFlight cf = (CargoFlight) o;
        cf.add1Package(1.0f, 2.5f, 3.0f);
    }
  }
}
```

### Object Class Methods

| Method     | Description                                                     |
|------------|-----------------------------------------------------------------|
| `clone`    | create new object instance that duplicates the current instance |
| `hashCode` | get a hash code for current instance                            |
| `getClass` | return type information for the current instance                |
| `finalize` | handle special resource cleanup scenarios                       |
| `toString` | return a string value representing the current instance         |
| `equals`   | compare another object to the current instance for equality     |

### Equality

- `==` compares primitive types and references
- `obj1 == obj2` compares if they refer to the same object
- `Object.equals()` does the same
  - needs to be overridden to make a different comparison

```java
class Flight {
    // ...
    
    @Override
    public boolean equals(Object obj) {
          if (obj instanceof Flight) {
              Flight that = (Flight) obj;
              return this.flightNumber == that.flightNumber;
          }
          return false;
    }
}
```

##

[url.course]: https://app.pluralsight.com/library/courses/working-classes-interfaces-java

[m01.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/main
[m02.note]: #2-understanding-java-classes-and-objects
[m03.note]: #3-implementing-class-constructors-and-initializers
[m04.note]: #4-using-static-members
[m05.note]: #5-a-closer-look-at-methods
[m06.note]: #6-class-inheritance
[m06.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/06-ClassInheritance
[m07.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/07-MoreAboutInheritance
[m08.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/08-WorkingWithEnums
[m09.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/09-CreatingAbstractRelationshipsWithInterfaces
[m10.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/10-NestedTypesAndAnonymousClasses
