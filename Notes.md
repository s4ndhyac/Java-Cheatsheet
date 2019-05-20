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
- Identify the elementary operation
  - no other operation grows more frequently as the size of the input is increased
  - the time to execute a single elementary operation is constant
- WC -> max(T(n1), T(n2)...)
- AC -> P(n1)T(n1) + P(n2)T(n2) + .... (requires knowledge of how input is distributed P(n))
- For recursive functions: write the recurrence relation
  - for the asymptotic time complexities you can assume constants to be 1 in the recurrence relation
  - for a>=1 and b>1 and f(n) = n^d where d>=0 write recurrence in the form ->
    - T(n) = aT(n/b) + f(n)
    - a-> no. of Subproblems
    - b-> relative subproblem size
    - k -> the no. of subproblems is lower bound by k (i.e the no. of leaves)
    - calculate c = log a to the base b = log(# subproblems)/log(relative subproblem size)
    - Cases: Work to split/recombine:
      - Case 1: is dominated by no. of subproblems i.e leaf heavy problem
        - c > d
        - T(n) = O(n^c) where c = log a to the base b
      - Case 2: is comparable to no. of subproblems
        - d = c and f(n) = O(n^d logn^k)
          - if k > -1: T(n) = O(n^c logn^k+1) 
          - if k = -1: T(n) = O(n^c loglog n)
          - if k < -1: T(n) = O(n^c)
      - Case 3: dominates the no. of subproblems:
        - d > c and af(n/b)/f(n) < 1
        - T(n) = O(f(n))

- For recursive functions another way: draw the recursion tree
  - Complexity = length of tree from root node to leaf node * number of leaf nodes

- For complex algorithms - count the work performed for each piece of the data structure visited by the algorithms
- like for DFS input graph and vertices -> O(V + E)
