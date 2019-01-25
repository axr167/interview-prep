
# Comparators, Comparable and Sorting

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
