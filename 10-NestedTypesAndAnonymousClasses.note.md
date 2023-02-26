## 10. Nested Types and Anonymous Classes

### Nested Types

- a type declared w/i another type
- member of the enclosing type
- can access private members of enclosing class
- support all access modifiers
    - no modifier (aka package private)
    - public (usable anywhere)
    - private (only usable by enclosing type)
    - protected (only usable by other types that inherit from its enclosing type)
- 2 categories
    - one provides only naming scope
    - the other links type instances
    - syntax is similar
    - behavior is different

### Nesting Types for Naming Scope

- type name scoped w/i enclosing type
- no relationship b/w nested type and enclosing type instances
- applies to the following nested types
    - _static_ classes nested w/i classes
    - all classes nested w/i interfaces
    - all nested interfaces

```java
public class Passenger implements Comparable<Passenger> {
    
    public static class RewardProgram {
        private int memberLevel;
        private int memberDays;
        // getters & setters
    }

    //...
    private RewardProgram rewardProgram = new RewardProgram();

    public Passenger(String name, int memberLevel, int memberDays) {
        //...
        rewardProgram.memberLevel = memberLevel;
        rewardProgram.memberDays = memberDays;
    }

    // getters & setters
}
```

### Accessing a Nested Type

```java
class Main {
    public static void main(String[] args) {
        Passenger steve = Passenger("Steve", 3, 180); // 3 is setting reward program member level for Steve
        Passenger.RewardProgram platinum = new Passenger.RewardProgram();
        platinum.setMemberLevel(3);
        if (steve.getRewardProgram().getMemberLevel() == platinum.getMemberLevel()) {
            System.out.println("Steve is platinum");
        }
    }
}
```

### Inner Classes

- type name scoped w/i enclosing type
- creates instance relationship
- each instance of nested class is associated w/ instance of enclosing class
- applies to **non-static** classes nested w/i classes
- inner class has multiple `this` references
    - `this` refers to inner class
    - _`enclosingClass`_`.this`, e.g. `Flight.this`

```java
public class Flight implements Comparable<Flight>, Iterable<Passenger> {
    private ArrayList<Passenger> passengerList = new ArrayList<Passenger>();

    public Iterator<Passenger> iterator() {
        return passengerList.iterator();
    }

    private class FlightIterable implements Iterable<Passenger> {
        @Override
        public Iterator<Passenger> iterator() {
            Passenger[] passengers = new Passenger[passengerList.size()]; // can reference members of enclosing class
            passengersList.toArray(passengers);
            Arrays.sort(passengers);
            return Arrays.asList(passengers).iterator();
        }
    }

    public Iterable<Passenger> getOrderedPassengers() {
        FlightIterable orderedPassengers = new FlightIterable();
        return orderedPassengers;
    }
}
```

### Anonymous Classes

- declared as part of their creation
- use as simple interface implementations
- use as simple class extensions
- anonymous classes are _inner classes_
- instance is associated w/ enclosing class instance
- use `new` keyword w/ base class or interface name
- place parenthesis after name
- add code (place w/i brackets)
- can implement methods
- can override methods

```java
import java.util.Iterator;

public class Flight implements Comparable<Flight>, Iterable<Passenger> {
    private ArrayList<Passenger> passengerList = new ArrayList<Passenger>();

    public Iterable<Passenger> getOrderedPassengers() {
        return new Iterable<Passenger>() {
            @Override
            public Iterator<Passenger> iterator() {
                Passenger[] passengers = new Passenger[passengerList.size()]; // can reference members of enclosing class
                passengersList.toArray(passengers);
                Arrays.sort(passengers);
                return Arrays.asList(passengers).iterator();
            }
        };
    }
}
```
