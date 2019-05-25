# Java-Cheatsheet
The complete Java cheatsheet

- Java: cross-platform memory model -> write once, run anywhere concurrent applications

## Data Structures and APIs

### Base Classes 
- Object
  - toString
  - clone()
    - creates and returns a shallow copy
  - finalize
    - called just before an object is garbage collected
    - called by Garbage Collector when it determines that there are no more references to it
    - override finalize to perform cleanup, dispose system resources and minimize memory leaks
    - Garbage Collector called by calling `System.gc()`
    - the finalize method is called only once by the gc even though the object is eligible for gc multiple times
  - getClass()
    - return the class object of this class
    - can also be used to get class metadata
    - After loading a .class file JVM creates an object of type java.lang.class in the heap area. We can use this class object to get class level information. This is widely used in reflection.
  - hashCode()
    - for every object JVM generates a unique integer
    - converts the internal address to an integer
    - the function is native in JAVA
    - overriding of hashCode() should return a unique int per object
    - used in hashing datastructures
  - obj.equals(obj)
    - compares the 'this' object to the object on which the method was called
    - recommended to override hashCode whenever equals is overriden because equal objects shouls have equal hashCodes
  - notify
    - wakes up a single thread waiting on this object's monitor
  - notifyAll
    - wakes up all threads that are waiting on this objects monitor
  - wait
    - causes the current thread to wait until either:
      - another thread invokes the notify or notifyAll method
      - a specified amount of time has elapsed

### Interfaces
- Collection
  - add(Element e)
  - addAll(Collection c)
  - clear()
    - remove all elements from this collection, collection will be empty after this method returns
  - contains(Object o)
  - containsAll(Collection c)
  - equals(Object o)
  - hashCode()
  - isEmpty()
  - iterator()
  - remove(Object o)
  - removeAll(Collection c)
  - retainAll(Collection c)
    - retain only the elements in the Collection c, remove all elements not in the collection c
  - size()
  - toArray()
  - toArray(T[] a)

- Iterable

- Comparable \
The type of objects that this object may be compared to

- Serializable

- Appendable

- CharSequence

- Queue
  - FIFO
  - is an interace and requires a concrete class for its declaration
  - most commonly used are - PriorityQueue and LinkedList
    - Both are not thread safe
  - PriorityBlockingQueue, ArrayBlockingQueue, any BlockingQueue is thread-safe
  - supports all methods of the Collection interface
  - Deque supports insertion and removal at both ends
  - Methods:
    - add(obj) - throws exception if empty else returns boolean
    - offer(obj) - returns boolean - null if failure
    - peek() - returns null if no element
    - element() - throws exception if empty  | similar to peek otherwise
    - remove() - throws exception if empty
    - poll() - returns null if no element
    - size() - returns no. of elements in the queue


### Functional Interface
- Comparator \
Interface used to order the objects of user-defined classes. 
  - compare(Object obj1, Object obj2);
  - equals(Object obj)
  - usage: Collections.sort(List list, ComparatorClass c) where ComparatorClass implements the Comparator interface and includes definition for the compare method as needed
  - comparing() - extracts sort key from template T, then compares based on it `Comparator<Person> lastName = Comparator.comparing(Person::lastName)` ||| \\\ly comparingInt, comparingDouble, comparingLong for different key types extracted from T 
  - naturalOrder() - returns a comparator that compares comparable objects in natural order
  - nullFirst() - Returns a null-friendly comparator that considers nulls to be less than non-nulls.
  - nullsLast() - Returns a null-friendly comparator that considers null to be greater than non-null. 
  - reversed() - returns a comparator that is in reverse order of this comparator
  - reverseOrder() - returns a comparator that is in reverse order of the natural order
  - thenComparing() - when `this` comparator results in equality, then `other` comparator is used - `Comparator<String> cmp = Comparator.comparingInt(String::length).thenComparing(String.CASE_INSENSITIVE_ORDER)` || \\\ly thenComparingInt, thenComparingDouble, thenComparingLong for different types of `other` comparator key

- Iterator

### Boxed [Wrapper classes]
- Character
  - Character.isDigit(a)
- String
- Integer
- Double
- Long
- StringBuilder
- StringBuffer -> `final class StringBuffer extends Object implements Serializable, Appendable, CharSequence`
  - Constructor -> 
    - new StringBuffer()
    - new StringBuffer(int size)
    - new StringBuffer(String s)
  - methods -> StringBuffer s = new StringBuffer()
    - s.length()
    - s.capacity()
    - s.append(String s)
    - s.insert(int index, String str/char ch)
    - s.reverse();
    - s.delete(int startIndex, int endIndex)
    - s.deleteCharAt(int index)
    - s.replace(int startIndex, int endIndex, String str)
    - s.ensureCapacity(int capacity) -> sets the new capacity to the input capacity or twice the current capacity whichever is larger
    - s.charAt(int index)
    - s.chars() -> returns stream of int zero-extending the char values from this sequence
    - s.getChars(int startIndex, int endIndex, char[] dest, int destStartIndex) -> copy chars b/w startIndex and endIndex from this sequence to the destination character array from the destination startIndex.
    - s.indexOf(String str) -> returns first occurence of startIndex of specified substring str within given sequence
    - s.lastIndexOf(String str) -> returns last occurence of startIndex of specified substring str within given sequence
    - s.setCharAt(int index, char ch)
    - s.setLength(int length)
    - s.subsequence(int startIndex, int endIndex) -> returns CharSequence
    - s.substring(int startIndex)
    - s.substring(int startIndex, int endIndex)
    - s.toString()
    - s.trimToSize() -> attempts to reduce the storage for the char sequence 

- Stack
- Deque
- PriorityQueue
- List
- Set
- Map
- Map.Entry
  - returns a collection view of this class - entrySet()
  - only way to obtain a reference to a map entry is from the iterator of this collection view - iterator of entrySet
  - equals - e1.getKey().equals(e2.getKey())
  - hashCode - e.hashCode()
  - getKey()
  - getValue()
  - setValue(value)
  - comparingByKey 
    - returns a comparator that compare entry in natural order on key
    - comparingByKey(Comparator c) - compares entry on key given by comparator
  - comparingByValue
    - returns a comparator that compares in natural order on value
    - comparingByValue(Comparator c) - compares entry on value given by comparator
  ```
  Map<Integer,Integer> m = new HashMap<>();
  Set<Map.Entry<Integer,Integer>> s = m.entrySet();
  for(Map.Entry<String, Integer> it: s)
  {
    System.out.println(it.getKey() + " " + it.getValue());
  }
  ```

- Pair<K, V> extends java.lang.Object
  - package -> javafx.util.Pair<K,V>
  - getKey()
  - getValue()
  - hashCode()
  - equals()
  - toString()
  - and all methods inherited from Object

- Iterator


### Unboxed [Primitives]


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

## Java Competitive Programming
- Check if the number is even or odd using bitwise &
```
System.out.println((a & 1) == 0 ?  "EVEN" : "ODD" ); 
```
- Multiplication by 2^n = a << n
- Division by 2^n = a >> n
- Swapping two numbers using bitwise XOR ^
```
a = a ^ b
b = b ^ a
a = a ^ b
```
- String is immutable
- StringBuilder (faster) single thread
- StringBuffer (thread-safe) synchronous (slower)
- Most significant digit:
```
K = Math.log10(N);
K = K - Math.floor(K);
X = (int)Math.pow(10, K);
return X
```
- No. of digits
```
return Math.floor(Math.log10(N)) + 1
```
- GCD
```
BigInteger b1 = BigInteger.valueOf(a); 
BigInteger b2 = BigInteger.valueOf(b); 
BigInteger gcd = b1.gcd(b2); 
return gcd.intValue(); ///ly gcd.longValue();
```
-  Check if no. is prime
```
return BigInteger.valueOf(1235).isProbablePrime(1);
```
- If no. is power of 2
```
return x!=0 && ((x&(x-1)) == 0);     
```
- Sorting Algorithm:
  - Arrays.sort() used to sort elements of a array.
  - Collections.sort() used to sort elements of a collection.
  - For primitives, Arrays.sort() uses dual pivot quicksort algorithms.

- Searching Algorithm:
  - Arrays.binarySearch() -> sorted array
  - Collections.binarySearch() -> collection based on comparators.

- Copy Algorithm: 
  - Arrays.copyOf() and copyOfRange() copy the specified array.
  - Collections.copy() copies specified collection.

- Rotation and Frequency 
  - Collections.rotate() to rotate a collection or an array by a specified distance. 
  - Collections.frequency() method to get frequency of specified element in a collection or an array.

- use Collections framework data structures

- use wrapper class for radix conversions:

- How to use array of lists

- How to use list of arrays
```
return Integer.toString(num, base);
```

- Avoid NullPointerException

## Head-First Design Patterns in Java [Notes]


## Java Interview Bootcamp [Notes]
- 

