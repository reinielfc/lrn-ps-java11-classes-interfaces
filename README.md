# Learn With Pluralsight: [Working with Classes and Interfaces in Java 11][url.course]

1. **Course Overview** [[GITHUB][m01.gh]]
2. Understanding Java Classes and Objects [[NOTE][m02.note]]
3. Implementing Class Constructors and Initializers [[NOTE][m03.note]]
4. Using Static Members [[NOTE][m04.note]]
5. A Closer Look at Methods [[NOTE][m05.note]]
6. Class Inheritance [[NOTE][m06.note]]
7. More About Inheritance [[NOTE][m07.note]]
8. Working with Enums [[NOTE][m08.note]]
9. Creating Abstract Relationships with Interfaces
10. Nested Types and Anonymous Classes

## 2. Understanding Java Classes and Objects

### Basic Access Modifiers

| Modifier  | Visibility               | Classes | Members |
| --------- | ------------------------ | ------- | ------- |
|           | only w/i own package     | Y       | Y       |
| `public`  | everywhere               | Y       | Y       |
| `private` | only w/i declaring class | N*      | Y       |

\* available to nested classes

## 3. Implementing Class Constructors and Initializer s

### Default Initial State of Fields

| `byte`/`short`/`int`/`long` | `float`/`double` | `char`     | `boolean` | Reference types |
| --------------------------- | ---------------- | ---------- | --------- | --------------- |
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
| ---------- | --------------------------------------------------------------- |
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

## 7. More About Inheritance

### Special Reference: `super`

- similar to special reference `this`
- refers to current object
- key difference
    - treats object as an instance of the base class
    - provides access to overridden base class members

### Preventing Inheritance and Method Overriding

- default inheritance behavior
    - each class can be extended
    - derived class can override any method
- can change this behavior w/ `final` keyword
    - can prevent class extending
    - can prevent method overriding

### Requiring Inheritance and Method Overriding

- default class usage
    - each class can be directly instantiated
- default method overriding requirements
    - derived class option whether to override a method
- can change default behavior w/ `abstract`
    - can require inheritance to use class
    - can require derived class to override 1+ methods

### Inheritance and Constructors

> - constructors are **not** inherited
> - each class has its own constructor

## 8. Working with Enums

**Enumeration Types:** useful for defining a type w/ a finite list of valid values

- declare using the `enum` keyword
- provide a comma-separated value list
- all uppercase by convention

```java
public enum FlightCrewJob {
    FLIGHT_ATTENDANT,
    COPILOT,
    PILOT
}
```

- conditional logic
- enums support equality tests
- can use `==` and `!=` operators
- enums support switch statements

```java
class Main {
    public static void main(String[] args) {
        FLightCrewJob job1 = FlightCrewJob.PILOT;
        FLightCrewJob job2 = FlightCrewJob.FLIGHT_ATTENDANT;
        if (job1 == FlightCreqJob.PILOT) {
            System.out.println("job1 is PILOT");
        }
        if (job1 != job2)
            System.out.println("job1 and job2 are different");

        switch (job1) {
            case FLIGHT_ATTENDANT:
                //...
                break;
            case PILOT:
                //...
                break;
            default:
                //...
        }
    }
}
```

### Relative Comparisons and Common Methods

- values are ordered
- 1st value is lowest
- last value is highest
- use `compareTo` for relative comparison
    - returns -n, 0, +n
    - indicates current instance's ordering relative to another value

### Common Enum Methods

| Method    | Description                                                     |
| --------- | --------------------------------------------------------------- |
| `values`  | returns array containing all values                             |
| `valueOf` | returns the value that corresponds to a string (case sensitive) |

### Enum Types Are Classes

- implicit inherit from Java's `Enum` class
- enum types can have members: fields, methods, constructors
- enum values
    - each value is an instance of the enum type
    - declaring the value creates the instance
    - can leverage the enum type's constructor

```java
public enum FlightCrewJob {
    FLIGHT_ATTENDANT("Flight Attendant"), // it's these values themselves that are creating the instances
    COPILOT("First Officer"), // so when calling the constructor we do it through them
    PILOT("Captain"); // <- needs semicolon when adding members

    private String title;

    private FlightCrewJob(String title) {
        this.title = title;
    }

    public String getTitle() {
        return title;
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
[m07.note]: #7-more-about-inheritance
[m08.note]: #8-working-with-enums
[m09.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/09-CreatingAbstractRelationshipsWithInterfaces
[m10.gh]: https://github.com/reinielfc/lrn-ps-java11-classes-interfaces/tree/10-NestedTypesAndAnonymousClasses
