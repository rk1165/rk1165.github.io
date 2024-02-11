+++
title = 'Few Tips and Techniques on problem solving'
date = 2024-02-11

+++

### Sieve of Eratosthenes

- The sieve of Eratosthenes is an algorithm that allows you to find all the prime number in the interval [1, n] for `O(n log log n)` operations
- The idea is simple - to write the series of numbers 1 … n, and we will strike out first all numbers divisible by 2, except for the numbers 2, and then dividing by 3, except for the numbers 3, then on 5, then 7, 11 and everything else upto n
- Various sieve optimisation of Eratosthenes :
  - The most obvious point is that in order to find all the primes before n, it is enough to perform the sieving only with ones, not surpassing the root of n.
  - Sieve only odd numbers : Since all even numbers, except for 2 are composite, it is possible not to process even numbers at all.

### Delete from a binary search tree

- It uses Hibbard delete algorithm
- Hibbard distinguish 3 cases in the deletion method:
  - The _deletion node_ has no subtree (i.e. a leaf node) : simply delete the node.
  - The _deletion node_ has only one subtree : short-circuit the BST at the deletion node.
  - The _deletion node_ has two subtrees. : Tough to handle.

### Reservoir Sampling

- So we are given a big array (or stream) of numbers (to simplify), and we need to write an efficient function to randomly select k numbers where 1 <= k <= n.
- Let the input array be stream[].
- Create an array reservoir[0..k-1] and copy first k items of stream[] to it.
- Now one by one consider all items from (k+1)th item to nth item
  - Generate a random number from 0 to i where i is index of current item in stream[]. Let the generated random number be j.
  - If j is in range 0 to k-1, replaces reservoir[j] with arr[i].

### Implement randM() using randN() when M > N:

- Use `randN()` to generate `randX()`, where x >= M.
- Use `randX()` to generate `randM()`
- N^b _ (randN() - 1) + N^(b-1) _ (randN()-1) + N^(b-2) _ (randN() - 1) + ... N^0 _ (randN() - 1) generates randX() - 1 , where X = N^(b+1)

### Bit Hacks

- x is even if `x & 1 = 0`.
- x is odd if `x & 1 = 1`
- `x ^ y` is negative if `x` and `y` have opposite signs.
- `-(~x)` will add 1 to an integer `x`
- Playing with kth bit
  - `x & ~(1 << (k-1))` turn **off** the k'th bit in a number
  - `x | (1 << (k-1))` turn **on** the k'th bit in a number
  - `x & (1 << (k-1)) != 0` check if k'th bit is set for number
  - `x ^ (1 << (k-1))` toggles the k'th bit
- `x & (x-1)` will turn off the rightmost set bit of a number
- x is a power of two exactly when `x & (x-1)` = 0.
- x is divisible by 2^k exactly when x & (2^k - 1) == 0
- x << k corresponds to multiplying x by 2^k and x >> k corresponds to dividing x by 2^k
- `x & -x` sets all the 1s to 0s except for the last 1.
- `x | (x-1)` inverts all the bits after the last one bit.
- `x & ~(x-1)` extracts the lowest set bit of x.
- convert uppercase to lowercase : `ch | ' '`
- convert lowercase to uppercase : `ch & '_'`
- invert alphabet's case : `ch ^ ' '`
- find a letter's position [1, 26] in the alphabet by taking its bitwise AND with ASCII 31. 'A' & 31 || 'c' & 31
- (n + (n >> 31)) ^ (n >> 31) : to compute absolute value
- `(x | y) - (x & y)` is equivalent to `x ^ y`

#### Finding Running Median from a Stream of Integers

- Keep two heaps maxHeap and minHeap
- For the first two elements add smaller one to the maxHeap on the left, and bigger one to the minHeap on the right. Then process stream data one by one
- Step 1: Add next item to one of the heaps
  - if next item is smaller than maxHeap root add it to maxHeap, else add it to minHeap
- Step 2: Balance the heaps (after this step heaps will be either balanced or one of them will contain 1 more item)
  - if number of elements in one of the heaps is greater than the other by more than 1, remove the root element from the one containing more elements and add to the other one.
- Then at any given time you can calculate median like this:
  - If the heaps contain equal amount of elements;
    - median = (root of maxHeap + root of minHeap) / 2
  - else
    - median = root of the heap with more elements

### Removing an element in O(1) time from an unordered array

- Replace the element you want to delete with the last element of the array. Then decrease the array size by 1.

```
[10,1,56,0,8,1]  # Replace "4" with the last element "1"
[10,1,56,0,8]    # Decrease array size by 1
```

### Min/Max heap for "K'th smallest/largest element"

- Build a max-heap from the first k elements of the array.
- For every remaining element, compare the element with the root of the max-heap. If it is less than the root, replace the root with the element and call heapify()
- When you're done with the second step, the root of the heap will be the k'th smallest element.
- Time complexity: O(k + (n-k)logk)
- Use **max-heap** for K'th smallest problems.
- Use **min-heap** for K'th largest problems.

### Use Index mapping

- Problem : Implement a min-heap data structure with O(1) search time.
- We can keep the index of each node in a hashmap and we need to update the indexes on this hashmap.

### Binary Prefix Divisible by 5

- For appending a digit d at the end of a binary number **old_number = a<sub>k</sub>a<sub>k-1</sub>...a<sub>0</sub>** we can just do **new_number = old_number \* 2 + d**
- This gives us the number with binary representation : a<sub>k</sub>a<sub>k-1</sub>...a<sub>0</sub>d
- A number is divisible by 5 iff the number is equivalent to 0 in the modular arithmetic of 5.
- (ab+c) % d = ((a % d) (b % d)) + (c % d)) % d

### Finding inorder successor of BST

- The key idea is that:
  - **if p node has a right subtree**, successor would be the smallest node in p's right subtree (in a BST, it is just the leftmost node)
  - **if p doesn't have a right subtree**, it is the last node whose left subtree it is under (this can be done through standard BST search and record its last right parent).

### Finding inorder predecessor of BST

- The key idea is that:
  - **if p node has a left subtree**, predecessor is the largest node found in the left subtree
  - **if p doesn't have a left subtree** it is the last node whose right subtree it is under

### Convert from postfix to expression tree

- Push operands on a stack (A, 2, B, etc. are operands) as leaf-nodes, not bound to any tree in any direction
- For operators, pop the necessary operands off the stack, create a node with the operator at the top, and the operands hanging below it, push the new node onto the stack

### Sliding Window problems

- _Basic template of such problems is basically 3 steps_
  - **Step1**: Have a counter or hash-map to count specific array input and keep on increasing the window toward right using outer loop.
  - **Step2**: Have a while loop inside to reduce the window side by sliding toward right. Movement will be based on constraints of problem. Go through few examples below
  - **Step3**: Store the current maximum window size or minimum window size or number of windows based on problem requirement.

### Interval

```python
def is_overlap(a, b):
  return a[0] < b[1] and b[0] < a[1]
```

```python
def merge_overlapping_intervals(a, b):
  return [min(a[0], b[0]), max(a[1], b[1])]
```

### Array Techniques

- Array problems often have simple brute-force solutions that use 0(n) space, but subtler solutions that **use the array itself** to **reduce space** complexity to 0(1)
- Filling an array from the front is slow, so see if it's possible to **write values from the back**
- Instead of deleting an entry, consider **overwriting** it.
- When dealing with integers encoded by an array consider **processing the digits from the back** of the array. Alternately, reverse the array so the **LSD is the first entry**

### Others

- Suppose the specified function is `random()` which generates random numbers from 1 to 5 with equal probability. The idea is to use the expression `5*(random() - 1) + random()` which uniformly produces random numbers in range `[1-25]`. So if we exclude the possibility of the random number being one among [8-25] by repeating the procedure, we’re left with numbers between [1-7] having equivalent probability.
- Clearly, the biased coin has the same probability of getting TAILS and then HEADS as the probability of getting HEADS and then TAILS. So, if we exclude the events of two HEADS and two TAILS by repeating the procedure, we're left with only two remaining outcomes having equivalent probability. That's the reason why we will get a fair result.
- Return 0, 1, 2 with equal probability using a specified function:
  - Let's say the specified function is random() which generates 0 or 1 with 50% probability each. Then if we make two different calls to the random() function and store the result in two variables, say x and y, then their sum `(x+y)` can be any of `{0,1,2}`. The probability of getting 0 and 2 is 25% each probability of getting 1 is 50%.
  - Now the problem reduces to decreasing the probability of getting 1 from 50% to 25%. We can easily do so by forcing our function to never generate either `(x = 1, y = 0)` or `(x = 0, y = 1)` which causes the sum to be equal to 1.
- 0 and 1 with 75% and 25% probability :
  - `x = rand() % 2; y = rand() % 2; return x & y` x = {0, 1}, y = {0, 1} (x & y) can be {0,0,0,1}
  - `x = rand() % 2; y = rand() % 2; return !(x | y)`
- We can use `(a > b) * b + (b > a) * a` expression to find the minimum number
- `((num & 1) && printf("odd")) || printf("even")` : using short circuiting to find if a number is odd/even. (num & 1 can be replaced with num % 2)
- multiply(a, b) = multiply(a*2, b/2) if b is even
  b + multiply(a*2, b/2) if b is odd
- Minimum or Maximum of triplet w/o ternary or if statement:

```C
int maximum(int a, int b, int c) {
    int max = a;
    (max < b) && (max = b)
    (max < c) && (max = c)
    return max
}
```

### Applications of finding Arrival and Departure time

- Topological sorting in a DAG
- Finding 2/3 - (edge or vertex) - connected components
- Finding bridges in graphs
- Finding biconnectivity in graphs
- Detecting cycle in directed graphs
- Tarjan's algorithm to find strongly connected components

### Finding cycle

- Using Union-Find
  - Create disjoint sets for each vertex of the graph
  - For every edge u, v in the graph
    - Find the root of the sets to which elements in u and v belongs
    - If both u and v have the same root in disjoint sets, a cycle is found.

### Find maximum product of two integers in an array :

- Naive : consider every pair : O(n^2) and O(1)
- Sort : consider max, secondMax or min and secondMin : O(nlogn) and O(1)
- Linear : keep track of max, secondMax, min and secondMin : O(n) and O(1)

### Find maximum product of three integers in an array:

- Naive : consider every triplet : O(n^3)
- Sort : consider last three or smallest, secondSmallest and largest.
- Linear : find the above mentioned elements in linear time

### Line Sweep

- you can understand it with an example,consider that the visited range are [2,6] and [8,9]
- our seen array is this, initially nothing is visited

```
0 0 0 0 0 0 0 0 0 0 0
0 1 2 3 4 5 6 7 8 9 10
```

- we have to mark 2 to 6 and 8 to 9 with 1
- what he has done is

```
0 0 1 0 0 0 0 -1 1 0  -1
0 1 2 3 4 5 6  7 8 9  10
```

- now when we do a prefix sum, it becomes

```
0 0 1 1 1 1 1 0 1 1 0
0 1 2 3 4 5 6 7 8 9 10
```

- see what happened here is that we started adding 1 after 2nd index, but we want to stop at 7th index as range is 2 to 6 only, how to make 1 as 0? add -1 from there
  now try to do a dry run on the given test cases you will see how this approach works

### Computing remainders by doubling

- for a, b E N, given t = remainder(a, 2b) we have : remainder(a,b) = {t if t < b, t-b if t >= b}

```python
def fast_remainder(a, b):
  if a < b : return a
  if a - b < b : return a - b
  r = fast_remainder(a, b + b)
  if r < b : return r
  return r - b
```
