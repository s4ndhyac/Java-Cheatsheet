# Java Concurrency in Practice

## Chapter - 1
- Threads allow multiple streams of program control flow to coexist within a process. 
- Threads -> 
  - share -> process-wide resources such as memory and file handles
  - has it's own -> program counter, stack, local variables
- provide a natural way of exploiting hardware parallelism on multiprocessor systems
- multiple threads within the same program can be scheduled simultaneously on multiple CPUs
- Threads -> lightweight processes
- modern OSes -> treat threads not processes as the basic unit of scheduling
- w/o coordination execute asynchronously - share memory address space and variables of owning process - causes problems
- threads turn asynchronous workflows into sequential ones
- threads allow close modelling of how humans work
- multi-threaded programs run better even on single core processors -> because then another thread can run while one is waiting on a blocking I/O
- legacy GUI applications - single-threaded
- Modern GUI frameworks - such as AWT/Swing replace the main event loop with an event dispatch thread (EDT)
  - on button click - application defined event handlers are called in the event thread
  - GUI application - still a single-threaded sub-system, main event loop runs in its own thread under the control of the GUI toolkit
  - event thread - short tasks | long running tasks in separate thread
- liveness failure -> an activity gets into a state such that it is permanently unable to make forward progresss
- context switches - when scheduled suspends one and activates another thread can be constly in applications with many threads

- Every Java application uses threads
- JVM starts - creates threads for housekeeping (garbage collection, finalization) and a main thread for running the main method
- AWT (Abstract Window Toolkit) & Swing -> threads for managing UI events
- Timer -> threads for executing deferred tasks
- Component frameworks such as servlets/RMI create pools of threads and invoke component methods in these threads

- TimerTasks are executed in a thread managed by the Timer not the application
- Easiest way to ensure thread-safety - objects accessed by the TimerTask are themselves thread-safe -> thus encapsulating the thread safety withim shared objects
- servlets, RMI -> called from multiple threads/requests -> thread-safety required

## Chapter - 2 Thread Safety
- means managing access to state (shared mutable state)
- stateless objects are always thread-safe
- ways to ensure thread-safety:
  - encapsulation
  - immutability
  - invariants
- race-condition types:
  - check-then-act (like lazy initialization): using a potentially stale observation to make a decision. observation could have become invalid b/w the time we observed and acted on it
  - read-modify-write (like increment):
- to avoid both check-then-act and read-modify-write race condition, operations should atomic
- check-then-act and read-modify-write sequences called compound actions (sequences of actions that must be executed atomically to be thread-safe)
- use java.util.concurrent.atomic package containing atomic variable classes for effecting atomic state transitions
- long -> atomicLong
- condition for thread-safety -> invariants maintained
- To preserve state consistency, the related state variables should be updated in a single atomic operation
- Intrinsic lock -> use synchronized keyword
- Reentrancy -> threads tries to acquire a lock it already holds it succeeds. Locks acquired a per-thread rather than per-invocation basis
- Thread = acquisition count + owning thread
  - When same thread acquired again count incremented
  - when synchronized block over -> count decreased
  - when count reaches 0, lock released
- access to each mutable state variable access should be performed with the same lock held
- every shared mutable variable should be guarded by exactly one lock
- Each variable participating in the invariant must be guarded by the same lock
- avoid premature optimization
- avoid holding locks during lengthy operations (such as network/console I/O)
- tradeoff b/w - simplicity and performance
- acquiring locks has overhead -> as few locks as possible 
- don't break it down too much but don't have too big blocks either 


## Chapter- 3 Sharing Objects
- possible to see stale values
- out-of-thin-air safety
- Locking is about mutual exclusion + memory visibility
- To avoid stale values synchronization must happen on a common lock
- volatile denotes that this variable is shared
- volatile variables are not reordered or cached or hidden from processors
- so read on a volatile variable always returns the most recent write by any thread
- volatile does not use locking so it does not cause a thread to block 
- volatile is a weaker lightweight form of synchronization than using the synchronized keyword
- use volatile only when they ensure visibility of their own state, the object they are referring to or indicating that an important lifecycle event has occurred such as initialization or shutdown
-  for example volatile does not guarantee atomicity in count++ (read-modify-write)
- Locking an guarantee both visibility and atomicity; volatile variables can only guarantee visibility
- volatile variables can be used when:
  - only a single thread ever updates the value
  - the variable does not participate in invariants with other state variables
  - Locking is not required for any other reason while the variable is being accessed
- Ways of publication:
  - Most blatant form of publication is to make it a public static field (visible to any class/thread)
  - returning a reference from a non-private method
  - publishing an object also publishes objects that have references contained inside the object
  - passing an object to an alien method is considered publishing the object
    - (alien method -> one whole behavior is not completely specified by the class (like methods in other classes as wells as overrideable methods (neither private nor final)))
  - publishing innerclass instance is considered publishing the enclosing instance because it contains a hidden reference to the enclosing instance
- Safe Construction practices:
  - this reference escaped during construction is considered improperly constructed
  - 