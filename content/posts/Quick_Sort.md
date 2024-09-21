+++
title = "Quick_sort"
author = ["Bruce"]
date = 2023-05-19
series = ["Leetcode"]
tags = ["Quick_sort", "Divide and Conquer"]
draft = false
+++
### Relevent Questions
- [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/description/)
- [AcWing 785. 快速排序](https://www.acwing.com/problem/content/787/)
- [AcWing 786. 第k个数](https://www.acwing.com/problem/content/788/)
### Algorithm Proof
The algorithm proof uses the loop invariant method from the book “Introduction to Algorithms”.

### Quick Sort Template (Using j as the Partition)
Quick sort is a divide-and-conquer algorithm, which involves three steps:

- Divide into subproblems
- Recursively solve the subproblems
- Combine the subproblems
```c++
void quick_sort(int q[], int l, int r) {
    // Termination condition for recursion
    if (l >= r) return;
    // Step 1: Divide into subproblems
    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j) {
        do i++; while (q[i] < x);
        do j--; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    // Step 2: Recursively solve the subproblems
    quick_sort(q, l, j);
    quick_sort(q, j + 1, r);
    // Step 3: Combine the subproblems. This step is not needed for quick sort, but it is crucial for merge sort.
}
```
### Problem to Prove
After the while loop ends, ( $q[l…j] \leq x $) and ( $q[j+1…r] \geq x $).

($ q[l…j] \leq x $) means all elements in ( $q[l], q[l+1], \ldots, q[j-1], q[j] $) are less than or equal to ( x ).

### Proof
Loop Invariant: ($ q[l…i] \leq x$ ) and ( $q[j…r] \geq x $).

#### Initialization: 
Before the loop starts, ( i = l - 1 ) and ( j = r + 1 ). Thus, ( q[l…i] ) and ( q[j…r] ) are empty, so the loop invariant holds.
#### Maintenance: 
Assume the loop invariant holds at the beginning of a loop iteration, i.e., ( $q[l…i] \leq x $) and ( $q[j…r] \geq x $).

Executing
```c++
do i++; while(q[i] < x);
会使得 q[l..i-1] <= x, q[i] >= x

do j--; while(q[j] > x);
会使得 q[j+1 ... r] >= x, q[j] <= x

if(i < j) swap(q[i], q[j]);
会使得 q[l..i] <= x, q[j..r] >= x
```

#### Termination: 
When the loop ends, ($ i \geq j $). Normally, according to the loop invariant, the result is obvious: ( $q[l…i] \leq x $) and ( $q[j…r] \geq x$ ). 
Therefore, ( $q[l…j] \leq x $) and ( $q[j+1…r] \geq x$ ) is evident.
However, the last loop iteration is special because the if statement will not execute. Since ( $i \geq j$ ), the if statement does not execute, ensuring:

( $q[l…i-1] \leq x $) and ( $q[i] \geq x$ )
( $q[j+1…r] \geq x $) and ( $q[j] \leq x $)
( $i \geq j$ )
From ( $q[l…i-1] \leq x$ ) and ( $i \geq j$ ) (that is, ( $i-1 \geq j-1 $)) and ( $q[j] \leq x $), we get ( $q[l…j] \leq x $). Since ( $q[j+1…r] \geq x$ ), the problem is proved.

### Summary: 
The loop invariant holds for all iterations except the last one, but the final requirement is still met.

Note:  At the end of the loop, check for array out-of-bounds or infinite partitioning.

### Boundary Case Analysis
Quick sort is a divide-and-conquer algorithm, and the worst case is dividing ( n ) into 0 and ( n ), or ( n ) into ( n ) and 0, causing infinite partitioning.

#### Analysis 1: 
When using ( j ) as the partition, ( x ) cannot be ( q[r] ). If using ( i ) as the partition, ( x ) cannot be ( q[l] ).
#### Analysis 2: 
``do i++; while (q[i] < x)`` and ``do j--; while (q[j] > x) ``cannot use ( $q[i] \leq x $) and ($ q[j] \geq x $).
#### Analysis 3: 
Can ``if (i < j) swap(q[i], q[j]); ``use ( $i \leq j$ )? 

Yes, because swapping ( q[i] ) and ( q[j] ) when ( i = j ) has no effect.
#### Analysis 4: 
Can the last statement use quick_sort(q, l, j-1), quick_sort(q, j, r)?

No, because it may cause infinite partitioning.
#### Analysis 5: 
The range of ( j ) is ( [l…r-1] ).
#### Analysis 6: 
Can while (i < j) be changed to while (i <= j)? 

No, because it may cause infinite partitioning.
#### Analysis 7: 
In the loop invariant proof, do i++; while (q[i] < x); makes ( q[l…i-1] \leq x ) and ( q[i] \geq x ).
#### Analysis 8: 
The benefit of using do-while loops is that the loop variables ( i ) and ( j ) are always updated, preventing the loop from getting stuck.
### Other Templates
Using ( i ) as the Partition:
```c++
// Ascending order
void quick_sort(int q[], int l, int r) {
    if (l >= r) return;
    int i = l - 1, j = r + 1, x = q[l + r + 1 >> 1]; // Note: round up
    while (i < j) {
        do i++; while (q[i] < x);
        do j--; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, i - 1);
    quick_sort(q, i, r);
}

// Descending order (change two conditions)
void quick_sort(int q[], int l, int r) {
    if (l >= r) return;
    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j) {
        do i++; while (q[i] > x); // Change here
        do j--; while (q[j] < x); // Change here
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j);
    quick_sort(q, j + 1, r);
}
```
Python Version:

```Python

def quick_sort(q, l, r):
    if l >= r:
        return
    i = l - 1
    j = r + 1
    x = q[l + r >> 1]
    while i < j:
        i += 1
        while q[i] < x:
            i += 1
        j -= 1
        while q[j] > x:
            j -= 1
        if i < j:
            q[i], q[j] = q[j], q[i]
    quick_sort(q, l, j)
    quick_sort(q, j + 1, r)

n = int(input())
q = list(map(int, input().split()))
quick_sort(q, 0, n - 1)
print(' '.join(map(str, q)))
```

### Summary of Quick Sort
Given an array ( q ), left endpoint ( l ), and right endpoint ( r ):

- Determine the partition boundary ( x ).
- Divide ( q ) into two subarrays: ( \leq x ) and ( \geq x ).
- ( i ) means all elements before ( i ) are ( <= x ), i.e., ( $q[l…i-1] \leq x $).
- ( j ) means all elements after ( j ) are ( >= x ), i.e., ( $q[j+1…r] \geq x $).
- After the while loop, ($ q[l…j] \leq x$ ) and ( $q[j+1…r] \geq x $).
- Recursively process the two subarrays
