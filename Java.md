# Java-Cheatsheet
The complete Java cheatsheet

- Java: cross-platform memory model -> write once, run anywhere concurrent applications

## Data Structures and APIs
### Boxed
- String
- StringBuilder
- StringBuffer
- Queue
- Stack
- Deque
- PriorityQueue
- List
- Iterator


### Unboxed


## Common Operations
### String Manipulation
- substring:
    - substring(int startIndex, int endIndex)
    - substring(int startIndex) [To end]
- split:


### On Arrays
- Array of Lists -> 
```
List<Integer>[] r = new ArrayList<>();
for(int i=0; i<10; i++)
{
  r[i] = new ArrayList<>();
}
```
- BinarySearch
int i = Arrays.binarySearch(arr, val);\\
returns index

- sort 
Arrays.sort(arr, (a,b)-> a - b);
Arrays.sort(arr, (a,b)-> b - a);

- deep copy
- Arrays.clone()


## Threading


## Design Patterns


## Effective Java [Notes]

- Fundamental Libraries - java.util, java.lang, java.io, java.util.concurrent
- Four types:
    - interfaces (including annotations) [Reference type]
    - classes (including enums) [Reference type]
    - arrays [Reference type]
    - primitives
- Class instances and arrays are objects
- A Class's members are:
    - fields
    - methods
    - member classes
    - member interfaces
- Method's signature = method name + types of formal parameters [Does not include method's return type]
- Access level when none is specified - defautl access [means package private]
- 


## Head-First Design Patterns in Java [Notes]


## Java Interview Bootcamp
- 

