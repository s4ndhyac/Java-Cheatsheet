# Java-Cheatsheet
The complete Java cheatsheet

- Java: cross-platform memory model -> write once, run anywhere concurrent applications

## Data Structures and APIs

### Base Classes 
- Object
  - clone
  - equals
  - finalize
  - getClass
  - hashCode
  - notify
  - notifyAll

### Interfaces
- Serializable
  -
- Appendable
  - 
- CharSequence
  - 
- Comparator

### Boxed [Wrapper classes]
- String
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
- Queue
- Stack
- Deque
- PriorityQueue
- List
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
```
return Integer.toString(num, base);
```

- Avoid NullPointerException

## Head-First Design Patterns in Java [Notes]


## Java Interview Bootcamp
- 

