

## Comparators, Comparable and Sorting

### Comparable:

Given a list of items we can use Collections.sort(list) to sort the list. This works for in built data types however if we have a custom object, java does not know how to sort it.

Collections.sort() accepts only objects that implement the comparable interface. 

It requires the **int compareTo(Object)** method

So if I have a custom object:

- Make the object implement Comparable
  - Add a method compareTo(object_2)
  - This compares current object (we refer to it using 'this' keyword) to another object object_2.
  - Here Object 1 is current object. Object 2 is object_2
  
Using this requires:

- Access to the class so we can make sure we implement comparable
- The compareTo method

### Comparator

If we do not have access to the class or if we do not want to modify it, we cannot use the comparable interface. To get around this Collections.sort() can accept 2 parameters: The collection and the logic for sorting as follows: 

    Collections.sort(list, logic);

The logic is a Comparator. The comparator is an interface so we cannot declare a new object. Because of this, we must define the class on each declaration (anonymous class). 

This anonymous class requires the **int compare(Object_1, Object_2)** method. This behaves the same way as the compareTo method.

Example:

        Comparator c = new Comparator<Interval>(){
            public int compare(Interval i1, Interval i2){
                if(i1.start > i2.start)
                    return 1;
                else if(i1.start < i2.start)
                    return -1;
                else
                    return 0;
            }
        };

### Comparing operations and Sorting:

  - If object_1 > object_2, return (+)
  - If object_1 < object_2, return (-)
  - If object_1 = object_2, return (0)
 
Then if we call Collections.sort it sorts by ascending order.


## Abstract class vs interface

    abstract class Customer {
      public void print() {
        System.out.println("Hi")
      }
    }

    interface iCustomer {
      void print();
    }

- Abstract classes can have implementation details (concrete methods) as well as abstract methods. Interfaces cannot
- Interface methods cannot have access modifiers. They are public by default.
- Abstract class can inherit from abstract classes or interface. Interface can only inherit from interface
- Class can implement multiple interfaces. But a class can extend only 1 class

### When to use abstract class:

If subclasses have a set of default functionalities that all do the same thing you can define concrete methods in abstract class. For common methods that may be different for child classes, use abstract methods.

### When to use interface:

When we dont know anything about implementation details, use interface. We can use multiple inheritance with interface so thats another application.
