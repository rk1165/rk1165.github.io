+++
title = 'LeetCode Problems'
date = 2024-03-17

+++

|  Problem                                                                        | Remark                                                                      | Status | Level |
| --------------------------------------------------------------------------------| ----------------------------------------------------------------------------| ------ | ----- |
| [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/)   | Sort array and compare first and last                                       |   C    | E
| [Pascals Triangle](https://leetcode.com/problems/pascals-triangle/)             | 1, prevRow values from col 1 to row, 1 to currRow                           |   C    | E
| [Unique Email Addresses](https://leetcode.com/problems/unique-email-addresses/) |                                                                             |   C    | E
| [Isomorphic Strings](https://leetcode.com/problems/isomorphic-strings/)         | map chars to numbers and check they are same for both strings  a->1 g->2    |   C    | E
| [Can Place Flowers](https://leetcode.com/problems/can-place-flowers/)           | calc. next and prev, check if both are 0, flower count++, cmp. count ==n    |        | E
| [Majority Element](https://leetcode.com/problems/majority-element/)             | cnt = majority = 0, cnt = 0 ? maj = num, count += num == majority ? 1 : -1  |        | E
| [Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/) | map + stack, put in map till curr elem > peek, push curr to stack           |        | E
| [Find Pivot Index](https://leetcode.com/problems/find-pivot-index/)             | leftSum = 0, leftSum * 2 == sum - nums[i], return i                         |   C    | E
| [Find All Numbers Disappeared in An Array](https://tinyurl.com/47n89kz9)        | -1 * index, index = Math.abs(nums[i])-1, find positive ones                 |   C    | E
| [Word Pattern](https://leetcode.com/problems/word-pattern/)                     | map to digit and check if present or not and if present has same value      |   C    | E
| [Valid Palindrome II](https://leetcode.com/problems/valid-palindrome-ii/)       |                                                                             |        | E
| [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)         | merge from end. merge while j >= 0                                          |   C    | E
| [Move Zeroes](https://leetcode.com/problems/move-zeroes/)                       | count zeros and if zeros > 0 nums[i-zeros] = nums[i]                        |   C    | E
| [Remove Duplicates From Sorted Array](https://tinyurl.com/f5sknd2u)             | l = 0, r = 1, compare left, right if not eq nums[++l] = nums[r]             |   C    | E
| [Contains Duplicate II](https://leetcode.com/problems/contains-duplicate-ii/)   |                                                                             |        | E
| [Implement Stack Using Queues](https://leetcode.com/problems/implement-stack-using-queues/) |                                                                 |   C    | E
| [Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/) | start = 0, end = n-1, idx = end, if abs(nums[start]) > abs(nums[end]) |   C    | E
| [Valid Perfect Square](https://leetcode.com/problems/valid-perfect-square/) | check mid * mid value and use binary search algo                                |   C    | E 
| [Sqrt(x)](https://leetcode.com/problems/sqrtx/)                                |                                                                              |        | E
| [Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)      | temp = head.next, head.next = prev, prev = head, head = temp                 |   C    | E
| [Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) | reverse 2nd half and compare with first half                                |   C    | E
| [Remove Linked List Elements](https://leetcode.com/problems/remove-linked-list-elements/) |  create dummy node, prev = curr, curr = head, move curr and cmp   |        | E
| [Remove Duplicates From Sorted List](https://leetcode.com/problems/remove-duplicates-from-sorted-list/) |                                                     |   C    | E
| [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) | move slow and fast pointers. return slow                              |   C    | E
| [Intersection of Two Linked Lists](https://leetcode.com/problems/intersection-of-two-linked-lists/) | a = hA, b = hB, while (a != b) switch heads             |        | E
| [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/) | pushAllLeft(root), pushAllLeft(curr.right)  in !empty loop    |   C    | E
| [Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/) | stack.push(right), stack.push(left)                         |   C    | E
| [Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/) | `in`, `out` stack. pop `in`, put in `out`, traverse `out` |   C    | E
| [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/)         | compare and swap and recurse on left and right nodes.                       |   C    | E
| [Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)                                                                             |        | E
| [Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)                                                                                   |        | E
| [Same Tree](https://leetcode.com/problems/same-tree/)                           | compare p & q                                                               |   C    | E
| [Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)                                                                             |        | E
| [Convert Sorted Array to Binary Search Tree](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)                                       |        | E
| [Merge Two Binary Trees](https://leetcode.com/problems/merge-two-binary-trees/) | n = new (r1.val + r2.val), n.l = merge(r1.l, r2.l), n.r = merge(r1.r, r2.r) |   C    | E
| [Path Sum](https://leetcode.com/problems/path-sum/)                             | hasSum(root.l, target - root.val) || hasSum(root.r, target - root.val)      |   C    | E
| [Minimum Distance between BST Nodes](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)                                                       |        | E
| [Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)                 | compare (l.l, r.r) && (l.r, r.l)                                            |   C    | E
| [Kth Largest Element In a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/) | PQ                                                        |   C    | E
| [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)           | PQ with reverse comparator                                                  |   C    | E
| [Island Perimeter](https://leetcode.com/problems/island-perimeter/)                                                                                           |        | E
| [Verifying An Alien Dictionary](https://leetcode.com/problems/verifying-an-alien-dictionary/)                                                                 |        | E
| [Excel Sheet Column Title](https://leetcode.com/problems/excel-sheet-column-title/)                                                                           |        | E
| [Greatest Common Divisor of Strings](https://leetcode.com/problems/greatest-common-divisor-of-strings/)                                                       |        | E
| [Plus One](https://leetcode.com/problems/plus-one/)                                                                                                           |        | E
| [Ugly Number](https://leetcode.com/problems/ugly-number/)                       | repeated divide by 2, 3, and 5                                              |   C    | E
| [Shift 2D Grid](https://leetcode.com/problems/shift-2d-grid/)                                                                                                 |        |
| [Roman to Integer](https://leetcode.com/problems/roman-to-integer/)             |                                                                             |   C    | E
| [Counting Bits](https://leetcode.com/problems/counting-bits/)                                                                                                 |        | E
| [Reverse Bits](https://leetcode.com/problems/reverse-bits/)  | start from pos 31 and shift bits in result if curr bit is 1                                    |        | E
| [Add to Array-Form of Integer](https://leetcode.com/problems/add-to-array-form-of-integer/) | Use LList, add to head (K + num[i]) % 10 and k % 10 while k > 0 |        | E
| [Add Binary](https://leetcode.com/problems/add-binary/)                                                                                                       |        | E
| [Flood Fill](https://leetcode.com/problems/flood-fill/)  |                                                                                                    |   C    | E
| [Reverse Vowels of a String](https://leetcode.com/problems/reverse-vowels-of-a-string/) | inner while loop moving i & j based on if not vowel, the swap vowel |   C    | E
| [Summary Ranges](https://leetcode.com/problems/summary-ranges/)  | if diff is 1 keep looping, then club the values                                            |   C    | E
| [Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)                                                                   |        | E
| [Convert 1D Array Into 2D Array](https://leetcode.com/problems/convert-1d-array-into-2d-array/)                                                               |   C    | E
| [Binary Tree Paths](https://leetcode.com/problems/binary-tree-paths/)                                                                                         |        | E
| [Number of Recent Calls](https://leetcode.com/problems/number-of-recent-calls/)                                                                               |        | E
| [Leaf-Similar Trees](https://leetcode.com/problems/leaf-similar-trees/)                                                                                       |        | E
| [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)                                                                   |        | E
| [Longest Palindrome](https://leetcode.com/problems/longest-palindrome/)                                                                                       |        | E
| [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/)                                                                       |        | E
| [Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)                                                                         |        | E
| [Construct String From Binary Tree](https://leetcode.com/problems/construct-string-from-binary-tree/)                                                         |        | M
| [Group Anagrams](https://leetcode.com/problems/group-anagrams/)                                                                                               |   C    | M
| [Sort an Array](https://leetcode.com/problems/sort-an-array/)                                                                                                 |   C    | M
| [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/)                                                                             |   C    | M
| [Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)                                                                   |   C    | M
| [Two Sum II Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)                                                           |   C    | M
| [Container With Most Water](https://leetcode.com/problems/container-with-most-water/)                                                                         |   C    | M
| [Fruits into Basket](https://leetcode.com/problems/fruit-into-baskets/)                                                                                       |   C    | M
| [Maximum Number of Vowels in a Substring of Given Length](https://leetcode.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length/)             |   C    | M
| [Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements/)                                                                             |   C    | M
| [Min Stack](https://leetcode.com/problems/min-stack/)                                                                                                         |   C    | M
| [Evaluate Reverse Polish Notation](https://leetcode.com/problems/evaluate-reverse-polish-notation/)                                                           |   C    | M
| [Removing Stars From a String](https://leetcode.com/problems/removing-stars-from-a-string/)                                                                   |   C    | M
| [Decode String](https://leetcode.com/problems/decode-string/)                                                                                                 |   C    | M
| [Find Minimum In Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)                                                   |   C    | M
| [Populating Next Right Pointers In Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)                                     |   C    | M
| [Design Twitter](https://leetcode.com/problems/design-twitter/)                                                                                               |   C    | M
| [Sum Root to Leaf Numbers](https://leetcode.com/problems/sum-root-to-leaf-numbers/)                                                                           |   C    | M
| [Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/)                               |   C    | M
| [Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)                                                           |   C    | M
| [Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)                                                         |   C    | M
| [Binary Tree Right Side View](https://leetcode.com/problems/binary-tree-right-side-view/)                                                                     |   C    | M
| [Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)                                           |   C    | M
| [Find Bottom Left Tree Value](https://leetcode.com/problems/find-bottom-left-tree-value/)                                                                     |   C    | M
| [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)                                                                       |   C    | M
| [Kth Largest Element In An Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)                                                             |   C    | M
| [Find The Kth Largest Integer In The Array](https://leetcode.com/problems/find-the-kth-largest-integer-in-the-array/)                                         |   C    | M
| [Max Area of Island](https://leetcode.com/problems/max-area-of-island/)                                                                                       |   C    | M
| [Count Sub Islands](https://leetcode.com/problems/count-sub-islands/)                                                                                         |   C    | M
| [House Robber](https://leetcode.com/problems/house-robber/)                                                                                                   |   C    | M
| [Unique Paths](https://leetcode.com/problems/unique-paths/)                                                                                                   |   C    | M
| [Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)                                                                                             |   C    | M
| [Reverse Integer](https://leetcode.com/problems/reverse-integer/)                                                                                             |   C    | M
| [Top K Frequent Words](https://leetcode.com/problems/top-k-frequent-words/)                                                                                   |   C    | M
| [Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)                                                                         |   C    | M
| [Maximum Level Sum of a Binary Tree](https://leetcode.com/problems/maximum-level-sum-of-a-binary-tree/)                                                       |   C    | M
| [Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)                               |   C    | M
| [Factorial Trailing Zeroes](https://leetcode.com/problems/factorial-trailing-zeroes/)                                                                         |   C    | M
| [Peak Index in a Mountain Array](https://leetcode.com/problems/peak-index-in-a-mountain-array/)                                                               |   C    | M
| [Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)                                                                   |   C    | M
| [Binary Tree Level Order Traversal II](https://leetcode.com/problems/binary-tree-level-order-traversal-ii/)                                                   |   C    | M
| [Equal Row and Column Pairs](https://leetcode.com/problems/equal-row-and-column-pairs/)                                                                       |   C    | M
| [Valid Sudoku](https://leetcode.com/problems/valid-sudoku/)                                                                                                   |        | M
| [Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)                                                                   |        | M
| [Sort Colors](https://leetcode.com/problems/sort-colors/)                                                                                                     |        | M
| [Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl/)                                                                         |        | M
| [Brick Wall](https://leetcode.com/problems/brick-wall/)                                                                                                       |        | M
| [Best Time to Buy And Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)                                                       |        | M
| [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)                                                                                 |        | M
| [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)                                                                 |        | M
| [Largest Number](https://leetcode.com/problems/largest-number/)                                                                                               |        | M
| [Continuous Subarray Sum](https://leetcode.com/problems/continuous-subarray-sum/)                                                                             |        | M
| [Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences/)                                                                               |        | M
| [Non Decreasing Array](https://leetcode.com/problems/non-decreasing-array/)                                                                                   |        | M
| [Remove Duplicates From Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)                                               |        | M
| [3Sum](https://leetcode.com/problems/3sum/)                                                                                                                   |        | M
| [4Sum](https://leetcode.com/problems/4sum/)                                                                                                                   |        | M
| [Rotate Array](https://leetcode.com/problems/rotate-array/)                                                                                                   |        | M
| [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)                               |        | M
| [Permutation In String](https://leetcode.com/problems/permutation-in-string/)                                                                                 |        | M
| [Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)                                                                         |        | M
| [Generate Parentheses](https://leetcode.com/problems/generate-parentheses/) - BT                                                                              |        | M
| [Daily Temperatures](https://leetcode.com/problems/daily-temperatures/)                                                                                       |        | M
| [Online Stock Span](https://leetcode.com/problems/online-stock-span/)                                                                                         |        | M
| [Simplify Path](https://leetcode.com/problems/simplify-path/)                                                                                                 |        | M
| [Remove All Adjacent Duplicates In String II](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string-ii/)                                     |        | M
| [Single Element in a Sorted Array](https://leetcode.com/problems/single-element-in-a-sorted-array/)                                                           |        | M
| [Find Peak Element](https://leetcode.com/problems/find-peak-element/)                                                                                         |        | M
| [Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/)                                                                                       |        | M
| [Search In Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)                                                               |        | M
| [Search In Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)                                                         |        | M
| [Find First And Last Position of Element In Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)             |        | M
| [Maximum Twin Sum Of A Linked List](https://leetcode.com/problems/maximum-twin-sum-of-a-linked-list/)                                                         |   C    | M
| [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)                                                           |        | M
| [Swapping Nodes in a Linked List](https://leetcode.com/problems/swapping-nodes-in-a-linked-list/)                                                             |        | M
| [Design Browser History](https://leetcode.com/problems/design-browser-history/)                                                                               |        | M
| [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)                                                                                             |   C    | M
| [Find The Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)                                                                         |        | M
| [Swap Nodes In Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)                                                                                     |        | M
| [Sort List](https://leetcode.com/problems/sort-list/)                                                                                                         |        | M
| [Partition List](https://leetcode.com/problems/partition-list/)                                                                                               |        | M
| [Rotate List](https://leetcode.com/problems/rotate-list/)                                                                                                     |        | M
| [Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)                                                                               |        | M
| [Design Circular Queue](https://leetcode.com/problems/design-circular-queue/)                                                                                 |        | M
| [Insertion Sort List](https://leetcode.com/problems/insertion-sort-list/)                                                                                     |        | M
| [Split Linked List in Parts](https://leetcode.com/problems/split-linked-list-in-parts/)                                                                       |        | M
| [LRU Cache](https://leetcode.com/problems/lru-cache/)                                                                                                         |        | M
| [Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)       |        | M
| [Construct Binary Tree From Preorder And Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)         |        | M
| [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)                                                                     |        | M
| [Kth Smallest Element In a Bst](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)                                                                 |        | M
| [Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/)                                                                       |        | M
| [Binary Search Tree Iterator](https://leetcode.com/problems/binary-search-tree-iterator/)                                                                     |        | M
| [Convert Bst to Greater Tree](https://leetcode.com/problems/convert-bst-to-greater-tree/)                                                                     |        | M
| [Seat Reservation Manager](https://leetcode.com/problems/seat-reservation-manager/)                                                                           |        | M
| [Reorganize String](https://leetcode.com/problems/reorganize-string/)                                                                                         |        | M
| [Longest Happy String](https://leetcode.com/problems/longest-happy-string/)                                                                                   |        | M
| [Car Pooling](https://leetcode.com/problems/car-pooling/)                                                                                                     |        | M
| [Subsets](https://leetcode.com/problems/subsets/) - BT                                                                                                        |        | M
| [Combination Sum](https://leetcode.com/problems/combination-sum/) - BT                                                                                        |        | M
| [Combinations](https://leetcode.com/problems/combinations/)   - BT                                                                                            |        | M
| [Permutations](https://leetcode.com/problems/permutations/)  - BT                                                                                             |        | M
| [Subsets II](https://leetcode.com/problems/subsets-ii/)   - BT                                                                                                |        | M
| [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/) - BT                                                                                  |        | M
| [Permutations II](https://leetcode.com/problems/permutations-ii/)  - BT                                                                                       |        | M
| [Restore IP Addresses](https://leetcode.com/problems/restore-ip-addresses/)                                                                                   |        | M
| [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/) - BT                                            |        | M
| [Implement Trie Prefix Tree](https://leetcode.com/problems/implement-trie-prefix-tree/)                                                                       |        | M
| [Number of Islands](https://leetcode.com/problems/number-of-islands/)                                                                                         |        | M
| [Rotting Oranges](https://leetcode.com/problems/rotting-oranges/)                                                                                             |        | M
| [Course Schedule](https://leetcode.com/problems/course-schedule/)                                                                                             |        | M
| [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)                                                                                       |        | M
| [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/)                                                               |        | M
| [Redundant Connection](https://leetcode.com/problems/redundant-connection/)                                                                                   |        | M
| [House Robber II](https://leetcode.com/problems/house-robber-ii/)                                                                                             |        | M 
| [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)                                                                 |        | M
| [Decode Ways](https://leetcode.com/problems/decode-ways/)                                                                                                     |        | M
| [Coin Change](https://leetcode.com/problems/coin-change/)    - DP:MM                                                                                          |        | M
| [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/)                                                                           |        | M
| [Word Break](https://leetcode.com/problems/word-break/)                                                                                                       |        | M
| [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)                                                               |        | M
| [Triangle](https://leetcode.com/problems/triangle/)    - DP:MM                                                                                                |        | M
| [Perfect Squares](https://leetcode.com/problems/perfect-squares/)                                                                                             |        | M
| [Minimum Cost For Tickets](https://leetcode.com/problems/minimum-cost-for-tickets/)  - DP:DM                                                                  |        | M
| [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)                                                                       |        | M
| [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)                                                             |        | M
| [Stone Game](https://leetcode.com/problems/stone-game/)                                                                                                       |        | M
| [Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)   - DP:MM                                                                                 |        | M
| [Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)                                                                                           |        | M
| [Jump Game](https://leetcode.com/problems/jump-game/)                                                                                                         |        | M
| [Jump Game II](https://leetcode.com/problems/jump-game-ii/)                                                                                                   |        | M
| [Gas Station](https://leetcode.com/problems/gas-station/)                                                                                                     |        | M
| [Hand of Straights](https://leetcode.com/problems/hand-of-straights/)                                                                                         |        | M
| [Two City Scheduling](https://leetcode.com/problems/two-city-scheduling/)                                                                                     |        | M
| [Maximum Length of Pair Chain](https://leetcode.com/problems/maximum-length-of-pair-chain/)                                                                   |        | M
| [Merge Intervals](https://leetcode.com/problems/merge-intervals/)                                                                                             |        | M
| [Non Overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)                                                                         |        | M
| [Rotate Image](https://leetcode.com/problems/rotate-image/)                                                                                                   |   C    | M
| [Spiral Matrix](https://leetcode.com/problems/spiral-matrix/)                                                                                                 |        | M
| [Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii/)                                                                                           |        | M
| [Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)                                                                                         |        | M
| [Integer to Roman](https://leetcode.com/problems/integer-to-roman/)                                                                                           |        | M
| [Pow(x, n)](https://leetcode.com/problems/powx-n/)                                                                                                            |        | M
| [Multiply Strings](https://leetcode.com/problems/multiply-strings/)                                                                                           |        | M
| [Robot Bounded In Circle](https://leetcode.com/problems/robot-bounded-in-circle/)                                                                             |        | M
| [Zigzag Conversion](https://leetcode.com/problems/zigzag-conversion/)                                                                                         |        | M
| [Find Missing Observations](https://leetcode.com/problems/find-missing-observations/)                                                                         |        | M
| [Contiguous Array](https://leetcode.com/problems/contiguous-array/)                                                                                           |        | M
| [3Sum Closest](https://leetcode.com/problems/3sum-closest/)                                                                                                   |        | M
| [Odd Even Linked List](https://leetcode.com/problems/odd-even-linked-list/)                                                                                   |        | M
| [Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)                                             |        | M
| [Path Sum II](https://leetcode.com/problems/path-sum-ii/)                                                                                                     |        | M
| [Path Sum III](https://leetcode.com/problems/path-sum-iii/)                                                                                                   |        | M
| [All Nodes Distance K in Binary Tree](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)                                                     |        | M
| [Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/)                                                                                   |        | M
| [Next Permutation](https://leetcode.com/problems/next-permutation/)                                                                                           |        | M
| [Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)                                                               |        | M
| [String Compression](https://leetcode.com/problems/string-compression/)                                                                                       |        | M
| [Max Number of K-Sum Pairs](https://leetcode.com/problems/max-number-of-k-sum-pairs/)                                                                         |        | M
| [Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)                                                                           |        | M
| [Number of Provinces](https://leetcode.com/problems/number-of-provinces/)                                                                                     |        | M
| [Combination Sum III](https://leetcode.com/problems/combination-sum-iii/)      - BT                                                                           |        | M
| [H-Index](https://leetcode.com/problems/h-index/)                                                                                                             |        | M
| [Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)                                                 |        | M
| [Flatten Binary Tree to Linked List](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)                                                       |        | M
| [Single Number II](https://leetcode.com/problems/single-number-ii/)                                                                                           |        | M
| [Bitwise AND of Numbers Range](https://leetcode.com/problems/bitwise-and-of-numbers-range/)                                                                   |        | M
| [Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)  - DP:DM                                            |        | M
| [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)                                                                                   |        | M
| [Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)                                                                                 |        | M
| [Find All Duplicates in an Array](https://leetcode.com/problems/find-all-duplicates-in-an-array/)                                                             |        | M
| [Letter Case Permutation](https://leetcode.com/problems/letter-case-permutation/)  - BT                                                                       |        | M
| [Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)                                             |        | M
| [Maximum Binary Tree](https://leetcode.com/problems/maximum-binary-tree/)                                                                                     |        | M
| [Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)                                                                   |        | M
| [First Missing Positive](https://leetcode.com/problems/first-missing-positive/)                                                                               |        | H
| [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)                                                                                     |        | H
| [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)                                                                               |        | H
| [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)                                                                     |        | H
| [Merge K Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)                                                                                   |        | H
| [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/)                                                                   |        | H
| [Find Median From Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) - 2Heaps                                                          |        | H
| [N Queens](https://leetcode.com/problems/n-queens/)      - BT                                                                                                 |        | H
| [N Queens II](https://leetcode.com/problems/n-queens-ii/)                                                                                                     |        | H
| [Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)                                                                                 |        | H
| [Burst Balloons](https://leetcode.com/problems/burst-balloons/)                                                                                               |        | H
| [Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)                                                                         |        | H
| [Unique Length 3 Palindromic Subsequences](https://leetcode.com/problems/unique-length-3-palindromic-subsequences/)                                           |        |
| [Minimum Number of Swaps to Make The String Balanced](https://leetcode.com/problems/minimum-number-of-swaps-to-make-the-string-balanced/)                     |        |
| [Number of Pairs of Interchangeable Rectangles](https://leetcode.com/problems/number-of-pairs-of-interchangeable-rectangles/)                                 |        |
| [Maximum Product of The Length of Two Palindromic Subsequences](https://leetcode.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences/) |        |
| [Grid Game](https://leetcode.com/problems/grid-game/)                                                                                                         |        |
| [Push Dominoes](https://leetcode.com/problems/push-dominoes/)                                                                                                 |        |
| [Check if a String Contains all Binary Codes of Size K](https://leetcode.com/problems/check-if-a-string-contains-all-binary-codes-of-size-k/)                 |        |
| [Range Sum Query 2D Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/)                                                                   |        |
| [Number of Zero-Filled Subarrays](https://leetcode.com/problems/number-of-zero-filled-subarrays/)                                                             |        |
| [Optimal Partition of String](https://leetcode.com/problems/optimal-partition-of-string/)                                                                     |        |
| [Design Underground System](https://leetcode.com/problems/design-underground-system/)                                                                         |        |
| [Minimum Penalty for a Shop](https://leetcode.com/problems/minimum-penalty-for-a-shop/)                                                                       |        |
| [Number of Subsequences That Satisfy The Given Sum Condition](https://leetcode.com/problems/number-of-subsequences-that-satisfy-the-given-sum-condition/)     |        |
| [Array With Elements Not Equal to Average of Neighbors](https://leetcode.com/problems/array-with-elements-not-equal-to-average-of-neighbors/)                 |        |
| [Boats to Save People](https://leetcode.com/problems/boats-to-save-people/)                                                                                   |        |
| [Number of Sub Arrays of Size K and Avg Greater than or Equal to Threshold](https://leetcode.com/problems/number-of-sub-arrays-of-size-k-and-average-greater-than-or-equal-to-threshold/)                                                                                 |        |
| [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)                                             |        |
| [Minimum Operations to Reduce X to Zero](https://leetcode.com/problems/minimum-operations-to-reduce-x-to-zero/)                                               |        |
| [Frequency of The Most Frequent Element](https://leetcode.com/problems/frequency-of-the-most-frequent-element/)                                               |        |
| [Minimum Number of Flips to Make The Binary String Alternating](https://leetcode.com/problems/minimum-number-of-flips-to-make-the-binary-string-alternating/) |        |
| [Validate Stack Sequences](https://leetcode.com/problems/validate-stack-sequences/)                                                                           |        |
| [Asteroid Collision](https://leetcode.com/problems/asteroid-collision/)                                                                                       |        |
| [Car Fleet](https://leetcode.com/problems/car-fleet/)                                                                                                         |        |
| [Remove K Digits](https://leetcode.com/problems/remove-k-digits/)                                                                                             |        |
| [132 Pattern](https://leetcode.com/problems/132-pattern/)                                                                                                     |        |
| [Capacity to Ship Packages](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)                                                           |        |
| [Successful Pairs of Spells and Potions](https://leetcode.com/problems/successful-pairs-of-spells-and-potions/)                                               |        |
| [Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)                                                                                     |        |
| [Minimize the Maximum Difference of Pairs](https://leetcode.com/problems/minimize-the-maximum-difference-of-pairs/)                                           |        |
| [Maximum Number of Removable Characters](https://leetcode.com/problems/maximum-number-of-removable-characters/)                                               |        |
| [Search Suggestions System](https://leetcode.com/problems/search-suggestions-system/)                                                                         |        |
| [Reorder List](https://leetcode.com/problems/reorder-list/)                                                                                                   |        | 
| [Copy List With Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)                                                                 |        |
| [Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)                                                                                   |        |
| [Minimum Time to Collect All Apples in a Tree](https://leetcode.com/problems/minimum-time-to-collect-all-apples-in-a-tree/)                                   |        |
| [Construct Quad Tree](https://leetcode.com/problems/construct-quad-tree/)                                                                                     |        |
| [Find Duplicate Subtrees](https://leetcode.com/problems/find-duplicate-subtrees/)                                                                             |        |
| [Check Completeness of a Binary Tree](https://leetcode.com/problems/check-completeness-of-a-binary-tree/)                                                     |        |
| [Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/)                                                                   |        |
| [Time Needed to Inform All Employees](https://leetcode.com/problems/time-needed-to-inform-all-employees/)                                                     |        |
| [Count Good Nodes In Binary Tree](https://leetcode.com/problems/count-good-nodes-in-binary-tree/)                                                             |        |
| [Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/)                                                                 |        |
| [House Robber III](https://leetcode.com/problems/house-robber-iii/)                                                                                           |        |
| [Flip Equivalent Binary Trees](https://leetcode.com/problems/flip-equivalent-binary-trees/)                                                                   |        |
| [Operations On Tree](https://leetcode.com/problems/operations-on-tree/)                                                                                       |        |
| [All Possible Full Binary Trees](https://leetcode.com/problems/all-possible-full-binary-trees/)                                                               |        |
| [Trim a Binary Search Tree](https://leetcode.com/problems/trim-a-binary-search-tree/)                                                                         |        |
| [Task Scheduler](https://leetcode.com/problems/task-scheduler/)                                                                                               |        |
| [Maximum Subsequence Score](https://leetcode.com/problems/maximum-subsequence-score/)                                                                         |        |
| [Single Threaded Cpu](https://leetcode.com/problems/single-threaded-cpu/)                                                                                     |        |
| [Process Tasks Using Servers](https://leetcode.com/problems/process-tasks-using-servers/)                                                                     |        |
| [Word Search](https://leetcode.com/problems/word-search/)                                                                                                     |        |
| [Palindrome Partitioning](https://leetcode.com/problems/palindrome-partitioning/)  - BT                                                                       |        |
| [Matchsticks to Square](https://leetcode.com/problems/matchsticks-to-square/)                                                                                 |        |
| [Splitting a String Into Descending Consecutive Values](https://leetcode.com/problems/splitting-a-string-into-descending-consecutive-values/)                 |        |
| [Find Unique Binary String](https://leetcode.com/problems/find-unique-binary-string/)                                                                         |        |
| [Max Length of a Concatenated String With Unique Characters](https://leetcode.com/problems/maximum-length-of-a-concatenated-string-with-unique-characters/)   |        |
| [Partition to K Equal Sum Subsets](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/)                                                           |        |
| [Design Add And Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)                                       |        |
| [Extra Characters in a String](https://leetcode.com/problems/extra-characters-in-a-string/)                                                                   |        |
| [Clone Graph](https://leetcode.com/problems/clone-graph/)                                                                                                     |        |
| [Pacific Atlantic Water Flow](https://leetcode.com/problems/pacific-atlantic-water-flow/)                                                                     |        |
| [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)                                                                                       |        |
| [Reorder Routes to Make All Paths Lead to The City Zero](https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/)               |        |
| [Snakes And Ladders](https://leetcode.com/problems/snakes-and-ladders/)                                                                                       |        |
| [Open The Lock](https://leetcode.com/problems/open-the-lock/)                                                                                                 |        |
| [Find Eventual Safe States](https://leetcode.com/problems/find-eventual-safe-states/)                                                                         |        |
| [Course Schedule IV](https://leetcode.com/problems/course-schedule-iv/)                                                                                       |        |
| [Check if Move Is Legal](https://leetcode.com/problems/check-if-move-is-legal/)                                                                               |        |
| [Shortest Bridge](https://leetcode.com/problems/shortest-bridge/)                                                                                             |        |
| [Accounts Merge](https://leetcode.com/problems/accounts-merge/)                                                                                               |        | 
| [Find Closest Node to Given Two Nodes](https://leetcode.com/problems/find-closest-node-to-given-two-nodes/)                                                   |        |
| [As Far from Land as Possible](https://leetcode.com/problems/as-far-from-land-as-possible/)                                                                   |        |
| [Shortest Path with Alternating Colors](https://leetcode.com/problems/shortest-path-with-alternating-colors/)                                                 |        |
| [Minimum Fuel Cost to Report to the Capital](https://leetcode.com/problems/minimum-fuel-cost-to-report-to-the-capital/)                                       |        |
| [Minimum Score of a Path Between Two Cities](https://leetcode.com/problems/minimum-score-of-a-path-between-two-cities/)                                       |        |
| [Number of Closed Islands](https://leetcode.com/problems/number-of-closed-islands/)                                                                           |        |
| [Number of Enclaves](https://leetcode.com/problems/number-of-enclaves/)                                                                                       |        |
| [Minimum Number of Vertices to Reach all Nodes](https://leetcode.com/problems/minimum-number-of-vertices-to-reach-all-nodes/)                                 |        |
| [Is Graph Bipartite?](https://leetcode.com/problems/is-graph-bipartite/)                                                                                      |        |
| [Evaluate Division](https://leetcode.com/problems/evaluate-division/)                                                                                         |        |
| [Detonate the Maximum Bombs](https://leetcode.com/problems/detonate-the-maximum-bombs/)                                                                       |        |
| [Path with Minimum Effort](https://leetcode.com/problems/path-with-minimum-effort/)                                                                           |        |
| [Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)                                                               |        |
| [Network Delay Time](https://leetcode.com/problems/network-delay-time/)                                                                                       |        |
| [Path with Maximum Probability](https://leetcode.com/problems/path-with-maximum-probability/)                                                                 |        |
| [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)                                                             |        |
| [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)  - DP:DW                                                              |        |
| [Palindromic Substrings](https://leetcode.com/problems/palindromic-substrings/)                                                                               |        |
| [Delete And Earn](https://leetcode.com/problems/delete-and-earn/)                                                                                             |        |
| [Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/) - DP:DW                                                                               |        |
| [Check if There is a Valid Partition For The Array](https://leetcode.com/problems/check-if-there-is-a-valid-partition-for-the-array/)                         |        |
| [Maximum Subarray Min Product](https://leetcode.com/problems/maximum-subarray-min-product/)                                                                   |        |
| [Integer Break](https://leetcode.com/problems/integer-break/)                                                                                                 |        |
| [Number of Longest Increasing Subsequence](https://leetcode.com/problems/number-of-longest-increasing-subsequence/) - DP:DW                                   |        |
| [Uncrossed Lines](https://leetcode.com/problems/uncrossed-lines/)                                                                                             |        |
| [Solving Questions With Brainpower](https://leetcode.com/problems/solving-questions-with-brainpower/)                                                         |        |
| [Count Ways to Build Good Strings](https://leetcode.com/problems/count-ways-to-build-good-strings/)                                                           |        |
| [New 21 Game](https://leetcode.com/problems/new-21-game/)                                                                                                     |        |
| [Best Team with no Conflicts](https://leetcode.com/problems/best-team-with-no-conflicts/)                                                                     |        |
| [Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/)   - DP:MM                                                                         |        |
| [Best Time to Buy And Sell Stock With Cooldown](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/) - DP:DM                         |        |
| [Coin Change II](https://leetcode.com/problems/coin-change-ii/)                                                                                               |        |
| [Target Sum](https://leetcode.com/problems/target-sum/)   - BT, DP:DW                                                                                         |        |
| [Interleaving String](https://leetcode.com/problems/interleaving-string/)                                                                                     |        |
| [Maximal Square](https://leetcode.com/problems/maximal-square/)      - DP:MM                                                                                  |        |
| [Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/)  - DP:MM                                                                                    |        |
| [Maximum Alternating Subsequence Sum](https://leetcode.com/problems/maximum-alternating-subsequence-sum/)                                                     |        |
| [Edit Distance](https://leetcode.com/problems/edit-distance/)                                                                                                 |        |
| [Stone Game II](https://leetcode.com/problems/stone-game-ii/)                                                                                                 |        |
| [Flip String to Monotone Increasing](https://leetcode.com/problems/flip-string-to-monotone-increasing/)                                                       |        |
| [Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/)                                                                 |        |
| [Longest Turbulent Array](https://leetcode.com/problems/longest-turbulent-subarray/)                                                                          |        |
| [Jump Game VII](https://leetcode.com/problems/jump-game-vii/)                                                                                                 |        |
| [Minimize Maximum of Array](https://leetcode.com/problems/minimize-maximum-of-array/)                                                                         |        |
| [Dota2 Senate](https://leetcode.com/problems/dota2-senate/)                                                                                                   |        |
| [Maximum Points You Can Obtain From Cards](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/)                                           |        |
| [Merge Triplets to Form Target Triplet](https://leetcode.com/problems/merge-triplets-to-form-target-triplet/)                                                 |        |
| [Partition Labels](https://leetcode.com/problems/partition-labels/)                                                                                           |        |
| [Valid Parenthesis String](https://leetcode.com/problems/valid-parenthesis-string/)                                                                           |        |
| [Eliminate Maximum Number of Monsters](https://leetcode.com/problems/eliminate-maximum-number-of-monsters/)                                                   |        |
| [Minimum Deletions to Make Character Frequencies Unique](https://leetcode.com/problems/minimum-deletions-to-make-character-frequencies-unique/)               |        |
| [Candy](https://leetcode.com/problems/candy/)                                                                                                                 |        |
| [Insert Interval](https://leetcode.com/problems/insert-interval/)                                                                                             |        |
| [Remove Covered Intervals](https://leetcode.com/problems/remove-covered-intervals/)                                                                           |        |
| [Detect Squares](https://leetcode.com/problems/detect-squares/)                                                                                               |        |
| [Text Justification](https://leetcode.com/problems/text-justification/)                                                                                       |        |
| [Naming a Company](https://leetcode.com/problems/naming-a-company/)                                                                                           |        |
| [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)                                                                           |        |
| [Maximum Frequency Stack](https://leetcode.com/problems/maximum-frequency-stack/)                                                                             |        |
| [Largest Rectangle In Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)                                                               |        |
| [Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)                                                                             |        |
| [LFU Cache](https://leetcode.com/problems/lfu-cache/)                                                                                                         |        |
| [Reverse Nodes In K Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)                                                                           |        |
| [Serialize And Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)                                                 |        |
| [Minimize Deviation in Array](https://leetcode.com/problems/minimize-deviation-in-array/)                                                                     |        |
| [Maximum Performance of a Team](https://leetcode.com/problems/maximum-performance-of-a-team/)                                                                 |        |
| [IPO](https://leetcode.com/problems/ipo/) - 2Heaps                                                                                                            |        |
| [Word Search II](https://leetcode.com/problems/word-search-ii/)                                                                                               |        |
| [Largest Color Value in a Directed Graph](https://leetcode.com/problems/largest-color-value-in-a-directed-graph/)                                             |        |
| [Minimum Number of Days to Eat N Oranges](https://leetcode.com/problems/minimum-number-of-days-to-eat-n-oranges/)                                             |        |
| [Word Ladder](https://leetcode.com/problems/word-ladder/)                                                                                                     |        |
| [Swim In Rising Water](https://leetcode.com/problems/swim-in-rising-water/)                                                                                   |        |
| [Number of Good Paths](https://leetcode.com/problems/number-of-good-paths/)                                                                                   |        |
| [Remove Max Number of Edges to Keep Graph Fully Traversable](https://leetcode.com/problems/remove-max-number-of-edges-to-keep-graph-fully-traversable/)       |        |
| [Find Critical and Pseudo Critical Edges in Min Span Tree](https://leetcode.com/problems/find-critical-and-pseudo-critical-edges-in-minimum-spanning-tree/)   |        |
| [Stickers to Spell Word](https://leetcode.com/problems/stickers-to-spell-word/)                                                                               |        |
| [Stone Game III](https://leetcode.com/problems/stone-game-iii/)                                                                                               |        |
| [Concatenated Words](https://leetcode.com/problems/concatenated-words/)                                                                                       |        |
| [Maximize Score after N Operations](https://leetcode.com/problems/maximize-score-after-n-operations/)                                                         |        |
| [Find the Longest Valid Obstacle Course at Each Position](https://leetcode.com/problems/find-the-longest-valid-obstacle-course-at-each-position/)             |        |
| [Count all Valid Pickup and Delivery Options](https://leetcode.com/problems/count-all-valid-pickup-and-delivery-options/)                                     |        |
| [Longest Increasing Path In a Matrix](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/)                                                     |        |
| [Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/)                                                                                 |        |
| [Count Vowels Permutation](https://leetcode.com/problems/count-vowels-permutation/)    - DP:DW                                                                |        |
| [Number of Ways to Rearrange Sticks With K Sticks Visible](https://leetcode.com/problems/number-of-ways-to-rearrange-sticks-with-k-sticks-visible/)           |        |
| [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)                                                                     |        |
| [Maximum Value of K Coins from Piles](https://leetcode.com/problems/maximum-value-of-k-coins-from-piles/)                                                     |        |
| [Number of Music Playlists](https://leetcode.com/problems/number-of-music-playlists/)                                                                         |        |
| [Number of Ways to Form a Target String Given a Dictionary](https://leetcode.com/problems/number-of-ways-to-form-a-target-string-given-a-dictionary/)         |        |
| [Profitable Schemes](https://leetcode.com/problems/profitable-schemes/)                                                                                       |        |
| [Minimum Cost to Cut a Stick](https://leetcode.com/problems/minimum-cost-to-cut-a-stick/)                                                                     |        |
| [Minimum Interval to Include Each Query](https://leetcode.com/problems/minimum-interval-to-include-each-query/)                                               |        |
| [Data Stream as Disjoint Intervals](https://leetcode.com/problems/data-stream-as-disjoint-intervals/)                                                         |        |
| [Maximum Points on a Line](https://leetcode.com/problems/max-points-on-a-line/)                                                                               |        |
| [Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)                                                                                     |        |
| [Basic Calculator](https://leetcode.com/problems/basic-calculator/)                                                                                           |        |
| [Palindrome Pairs](https://leetcode.com/problems/palindrome-pairs/)                                                                                           |        |
| [Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/)                                                           |        |
| [01 Matrix](https://leetcode.com/problems/01-matrix/)                                                                                                         |        |
| [Bus Routes](https://leetcode.com/problems/bus-routes/)                                                                                                       |        |
| [Random Pick with Weight](https://leetcode.com/problems/random-pick-with-weight/)                                                                             |        |
| [Smallest Range Covering Elements from K Lists](https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/)                                 |        |
| [Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)            - BT                                                                                 |        |
| [Longest Subarray of 1's After Deleting One Element](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/)                        |        |
| [Determine if Two Strings Are Close](https://leetcode.com/problems/determine-if-two-strings-are-close/)                                                       |        |
| [Delete the Middle Node of a Linked List](https://leetcode.com/problems/delete-the-middle-node-of-a-linked-list/)                                             |        |
| [Longest ZigZag Path in a Binary Tree](https://leetcode.com/problems/longest-zigzag-path-in-a-binary-tree/)                                                   |        |
| [Keys and Rooms](https://leetcode.com/problems/keys-and-rooms/)                                                                                               |        |
| [Nearest Exit from Entrance in Maze](https://leetcode.com/problems/nearest-exit-from-entrance-in-maze/)                                                       |        |
| [Smallest Number in Infinite Set](https://leetcode.com/problems/smallest-number-in-infinite-set/)                                                             |        |
| [Total Cost to Hire K Workers](https://leetcode.com/problems/total-cost-to-hire-k-workers/)                                                                   |        |
| [Domino and Tromino Tiling](https://leetcode.com/problems/domino-and-tromino-tiling/)    - DP:DW                                                              |        |
| [Best Time to Buy and Sell Stock with Transaction Fee](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/) - DP:DM           |        |
| [Minimum Flips to Make a OR b Equal to c](https://leetcode.com/problems/minimum-flips-to-make-a-or-b-equal-to-c/)                                             |        |
| [Minimum Number of Arrows to Burst Balloons](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/)                                       |        |
| [Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)                                         |        |
| [Game of Life](https://leetcode.com/problems/game-of-life/)                                                                                                   |        |
| [Minimum Genetic Mutation](https://leetcode.com/problems/minimum-genetic-mutation/)                                                                           |        |
| [Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)                                                             |        |
| [Best Time to Buy and Sell Stock IV](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)    - DP:DM                                            |        |
| [Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/)                                                                     |        | 
| [Longest Word in Dictionary](https://leetcode.com/problems/longest-word-in-dictionary/)                                                                       |        | 
| [Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/)                                               |        | 
| [Split a String Into the Max Number of Unique Substrings](https://leetcode.com/problems/split-a-string-into-the-max-number-of-unique-substrings/)             |        | 
| [Count of Range Sum](https://leetcode.com/problems/count-of-range-sum/)                                                                                       |        | 
| [Minimum Number of K Consecutive Bit Flips](https://leetcode.com/problems/minimum-number-of-k-consecutive-bit-flips/)                                         |        | 
| [Count Unique Characters of All Substrings of a Given String](https://leetcode.com/problems/count-unique-characters-of-all-substrings-of-a-given-string/)     |        | 
| [Course Schedule III](https://leetcode.com/problems/course-schedule-iii/)                                                                                     |        | 
| [Sliding Window Median](https://leetcode.com/problems/sliding-window-median/)                                                                                 |        | 
| [Prefix and Suffix Search](https://leetcode.com/problems/prefix-and-suffix-search/)                                                                           |        |
| [Sort Items by Groups Respecting Dependencies](https://leetcode.com/problems/sort-items-by-groups-respecting-dependencies/)                                   |        |
| [Meeting Rooms](https://leetcode.com/problems/meeting-rooms/)                                                                                                 |   P    |
| [Encode and Decode Strings](https://leetcode.com/problems/encode-and-decode-strings/)                                                                         |   P    | M
| [Wiggle Sort](https://leetcode.com/problems/wiggle-sort/)                                                                                                     |   P    | 
| [Walls And Gates](https://leetcode.com/problems/walls-and-gates/)                                                                                             |   P    |
| [Number of Connected Components In An Undirected Graph](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)                 |   P    |
| [Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/)                                                                                           |   P    |
| [Paint House](https://leetcode.com/problems/paint-house/)                                                                                                     |   P    |
| [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/)                                                                                           |   P    |
| [Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)                                                                                           |   P    |
| [Employee Free Time](https://leetcode.com/problems/employee-free-time/)                                                                                       |   P    |
| [Shortest Path to Get Food](https://leetcode.com/problems/shortest-path-to-get-food/)                                                                         |   P    |
| [Minimum Knight Moves](https://leetcode.com/problems/minimum-knight-moves/)                                                                                   |   P    |
| [Inorder Successor in BST](https://leetcode.com/problems/inorder-successor-in-bst/)                                                                           |   P    |
| [Design In-Memory File System](https://leetcode.com/problems/design-in-memory-file-system/)                                                                   |   P    |
| [Design Hit Counter](https://leetcode.com/problems/design-hit-counter/)                                                                                       |   P    |
| [Index Pairs of a String](https://leetcode.com/problems/index-pairs-of-a-string/)                                                                             |   P    |
| [Generalized Abbreviation](https://leetcode.com/problems/generalized-abbreviation/)                                                                           |   P    |
| [Sequence Reconstruction](https://leetcode.com/problems/sequence-reconstruction/)                                                                             |   P    | 
| [Factor Combinations](https://leetcode.com/problems/factor-combinations/)                                                                                     |   P    | 
| [Rearrange String k Distance Apart](https://leetcode.com/problems/rearrange-string-k-distance-apart/)                                                         |   P    | 
| [Design Search Autocomplete System](https://leetcode.com/problems/design-search-autocomplete-system/)                                                         |   P    |
| [Word Squares](https://leetcode.com/problems/word-squares/)                                                                                                   |   P    |
| [Design HashSet](https://leetcode.com/problems/design-hashset/)                                                                                               |   IM   | E
| [Design HashMap](https://leetcode.com/problems/design-hashmap/)                                                                                               |   IM   | E
| [First Bad Version](https://leetcode.com/problems/first-bad-version/)                                                                                         |   IM   | E
| [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)                                                                             |   IM   |
| [Time Based Key Value Store](https://leetcode.com/problems/time-based-key-value-store/)                                                                       |   IM   | M
| [Design Linked List](https://leetcode.com/problems/design-linked-list/)                                                                                       |   IM   | M
| [Insert Delete Get Random O(1)](https://leetcode.com/problems/insert-delete-getrandom-o1/)                                                                    |   IM   | M


### Dynamic Programming

- [DP Patterns](https://leetcode.com/discuss/general-discussion/458695/dynamic-programming-patterns)
- [DP Solving](https://leetcode.com/discuss/general-discussion/1000929/solved-all-dynamic-programming-dp-problems-in-7-months)
- [DP for Beginners](https://leetcode.com/discuss/general-discussion/662866/DP-for-Beginners-Problems-or-Patterns-or-Sample-Solutions)


### Graph related problems

- **Union Find:**
  - https://leetcode.com/problems/most-stones-removed-with-same-row-or-column/
  - https://leetcode.com/problems/number-of-operations-to-make-network-connected/
  - https://leetcode.com/problems/satisfiability-of-equality-equations/
  - https://leetcode.com/problems/accounts-merge/
  - https://leetcode.com/problems/connecting-cities-with-minimum-cost/
- **DFS from boundary**
  - https://leetcode.com/problems/surrounded-regions/
  - https://leetcode.com/problems/number-of-enclaves/
- **Shortest time**
  - https://leetcode.com/problems/time-needed-to-inform-all-employees/
- **Islands Variants**
  - https://leetcode.com/problems/number-of-closed-islands/
  - https://leetcode.com/problems/number-of-islands/
  - https://leetcode.com/problems/keys-and-rooms/
  - https://leetcode.com/problems/max-area-of-island/
  - https://leetcode.com/problems/flood-fill/
  - https://leetcode.com/problems/coloring-a-border/
- **Hash/DFS**
  - https://leetcode.com/problems/employee-importance/
  - https://leetcode.com/problems/find-the-town-judge/
- **Cycle Find**
  - https://leetcode.com/problems/find-eventual-safe-states/
- **BFS for shortest path**
  - https://leetcode.com/problems/01-matrix/
  - https://leetcode.com/problems/as-far-from-land-as-possible/
  - https://leetcode.com/problems/rotting-oranges/
  - https://leetcode.com/problems/shortest-path-in-binary-matrix/
- **Graph coloring**
  - https://leetcode.com/problems/possible-bipartition/
  - https://leetcode.com/problems/is-graph-bipartite/
- **Topological Sort**
  - https://leetcode.com/problems/course-schedule-ii/
- **Shortest Path**
  - https://leetcode.com/problems/network-delay-time/
  - https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/
  - https://leetcode.com/problems/cheapest-flights-within-k-stops/

### Sliding Window problems

- https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/
- https://leetcode.com/problems/number-of-substrings-containing-all-three-characters/
- https://leetcode.com/problems/replace-the-substring-for-balanced-string/
- https://leetcode.com/problems/max-consecutive-ones-iii/
- https://leetcode.com/problems/subarrays-with-k-different-integers/
- https://leetcode.com/problems/get-equal-substrings-within-budget/
- https://leetcode.com/problems/longest-repeating-character-replacement/
- https://leetcode.com/problems/shortest-subarray-with-sum-at-least-k/
- https://leetcode.com/problems/minimum-size-subarray-sum/
- https://leetcode.com/problems/sliding-window-maximum/