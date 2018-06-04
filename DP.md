DIVIDE AND CONQUER:
1. Dividing the problem into K subproblems
   Recursively solving the sub problem
   Conquer or combining their answers

2. Parallelism: Different subproblem executed on different processors. No need for advance planning.
3. Uses cache to solve since small subproblems.
4. Slow because of calling subproblems repeatedly.
5. Examples-> Tower of Hanoi, Binary and ternary search, Finding max and min, merge sort, quick sort.
6. Problems->
            a. Search an infinite array with size unknown in O(log n) by moving as 0, 2, 4...
            b. GIVEN SORTED ARRAY(P12)??????
            c. 2 sorted list. Fining median in 2(m+n) in O(n), by merge sort till n. 
               For O(log n), apply binary search approach on both together. m1 and m2 equal or not, based on which decide.
            d. "Strassen's algorithm" or matrix multiplication- Ci,j = Sum(Ai,k * Bk,j) in O(n3) to O(n2)bcoz of repeat.
            e. "Stock pricing problem" : finding min. in input and max in output but date of input shud b less than output.
               O(n2) if fiinding profit of each, 
               O(nlogn) by DnC by finding in half recursively inside and also considering min max left right.
            f. Drop eggs from n floors to find unbreakable, cannot use binary???????
            g. 2n array [a1 a2 a3 a4 b1 b2 b3 b4] ->[a1 b1 a2 b2 a3 b3 a4 b4] without extra memory.O(nlogn)
               Divide and Shuffle center [a1 a2 b1 b2 a3 a4 b3 b4]..."If n instead of 2"
            h. "Max. value contiguous subsequence" O(n log n)recursively finding for left right and 
               mid by left and right from mid loop.
            i. "Closest pair of points"
               In one dimension (either x or y), we can sort and find the min.
               In 2-d if we do brute force formulae(sqrt(square(x1-x2)-square(y1-y2))) O(n2)
               D&c(nlogn)-> Sort array on x(O(nlogn)), divide median is 0(-&+x),
                     find closest pairs L & R in set 1 & 2,
                     delta=min(L,R), eliminate points farther than delta apart l in O(n),
                     now sort on y, scan for 2*delta.
                     
DYNAMIC PROGRAMMING
1. In dp subproblems overlap but in dnc they r independent. expoential complexity reduced to polynomial
2. Memoization/Caching/laissez-faire approach is maintaining table of subproblems already solved.
3. Properties: Optimal substructure(subproblems), Overlapping subproblems
4. Approaches: 
              Bottom-up(solving normally smallest input but checking if subproblem already solved)(e.g. for loop 1 to n), 
              Top-down(subproblem to problem)(e.g. recursive with memoization)
              Both use space for array saving
5. Examples-> a. "Fibonacci"(2^n-> both bottom and top down O(n), bottom with space 1 using last 2),
              b. Factorial(single one is not dp, since no overlapping)
              (m after finding n is time-O(max(m,n)), space-O(max(m,n)), calculated from n table or incresed if m>n)
              c. "Longest Common subsequence" not necessarily contiguous
                 
            
            
            
            
            
            
            
            
            
            
            
            
