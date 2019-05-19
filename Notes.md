- Think of using stacks to reverse
- queue - sliding window
- Two pointers problems can also be solved with a stack
- Permutations/Combinations -> recursion -> memoization
- Induction -> 1 to n-1 to n -> Dynamic Programming
- Subproblems 
  - n-1 to n -> Bottom up DP (Induction)
  - n to n-1 -> Top down DP (recursion + memoization)
- Time complexity:
  - recursive -> typically exponential (PC)
  - recursive + memoization -> think about how the size of cache grows with input
    - pigeonhole principe: once we have filled the cache with every possible combination of input, all calls will hit the cache than result in a new function call
- drawback of recursive functions: languages place limits on the maximal size of their call stacks
  - but easily rectified since every recursive function can be implemented as a iterative function (using while loop)
- If your function is deterministic and we are calling it multiple times with the same inputs then we can benefit from memoization
- Find and write the recurrence relation


### Time complexity
