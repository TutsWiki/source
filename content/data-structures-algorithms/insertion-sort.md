---
date: 2020-09-08
linktitle: Insertion Sort
menu:
  main:
    parent: data_structures_algorithms
next: /data_structures_algorithms/tree_data_structure
title: Insertion Sort
weight: 2
url: /data-structures-algorithms/insertion-sort/
description: Insertion sort is one of the easiest and efficient sorting algorithms that is a comparison based sorting technique. Insertion sort is very advantageous in cases where the number of elements is small
keywords:
  - sorting
  - insertion
  - algorithms
tags: [DSA]
---
Now when there are sorting algorithms already available like [Merge Sort](/data-structures-algorithms/merge-sort/) and Quick Sort which can sort a large number of elements in quick time and that too efficiently in most of the cases, we still might require to rely on other sorting techniques in some cases. Today we will discuss one such sorting technique called **Insertion Sort**. Insertion sort is one of the easiest and efficient sorting algorithms that is a comparison based sorting technique. Insertion sort is very advantageous in cases where the number of elements is small and can provide the best case time complexity of `O(n)`.

Let's discuss some of the major advantages of Insertion Sort

## Advantages
1. Implementation of insertion sort is very easy as compared to sorting algorithms like quick sort, merge sort or heap sort.
2. Very efficient in the case of a small number of elements.
3. If the elements are already in sorted order it won't spend much time in useless operations and will deliver a run time of `O(n)`.
4. It is more efficient when compared to other simple algorithms like Bubble sort and Selection Sort.
5. It is a stable sorting technique, that is, the order of keys is maintained.
6. It requires constant "additional" memory, no matter the number of elements.
7. It can sort the elements as soon as it receives them.
8. It can turn out to be very efficient in case of nearly sorted elements.

## Disadvantages
1. One of the major disadvantages of Insertion sort is its Average Time Complexity of `O(n^2)`.
2. If the number of elements is relatively large it can take large time as compared to Quick Sort or Merge Sort. 

## Algorithm
The basic working of Insertion sort is fairly simple, what it does is picks an element and places it in its correct position. It does this for every element and finally, we get the sorted array.

Let's look at the algorithm more deeply.

1. Iterate from the second element to the last element.
2. Select the current element and compare it with the previous element.
3. If the element is small (in case of ascending order) keep on moving it to previous positions, until it is in its correct position in the sorted part.
4. Keep on repeating the above until there are no more elements left.

## Example
Let's have a look at an example to get a clearer picture of the algorithm. The number that is in bold represent the sorted part of the array.

- Consider the array: `17 13 23 2 7 1 34`
- Creating an initial marker at the second position.
  - **17** 13 23 2 7 1 34
- After 1st iteration
  - **13 17** 23 2 7 1 34
- After 2nd iteration
  - **13 17 23** 2 7 1 34
- After 3rd iteration
  - **2 13 17 23** 7 1 34
- After 4th iteration
  - **2 7 13 17 23** 1 34
- After 5th iteration
  - **1 2 7 13 17 23** 34
- After 6th iteration
  - **1 2 7 13 17 23 34**

## Code
```java
import java.util.Arrays;
public class Sorting {
    void insertionSort(int arr[]) {
        int lengthOfArray = arr.length; // Length of input array
        int value; // to store current element
        int pos; // to store position of current element
        for (int i = 1; i < lengthOfArray; i++) {
            pos = i - 1;
            value = arr[i];
            while (pos >= 0 && arr[pos] > value) {
                arr[pos + 1] = arr[pos];
                pos = pos - 1;
            }
            arr[pos + 1] = value;
            // Printing value to show array after each iteration
            System.out.println("Array after iteration " + i + ":" +
                Arrays.toString(arr));
        }
    }
    public static void main(String[] args) {
        Sorting sort = new Sorting(); // creating object of class Sorting
        int[] arr = {
            17,
            13,
            23,
            2,
            7,
            1,
            34
        };
        sort.insertionSort(arr); // method call
        System.out.println("Array after applying insertion sort : " +
            Arrays.toString(arr));
    }
}
```
Output
```
Array after iteration 1: [13, 17, 23, 2, 7, 1, 34]
Array after iteration 2: [13, 17, 23, 2, 7, 1, 34]
Array after iteration 3: [2, 13, 17, 23, 7, 1, 34]
Array after iteration 4: [2, 7, 13, 17, 23, 1, 34]
Array after iteration 5: [1, 2, 7, 13, 17, 23, 34]
Array after iteration 6: [1, 2, 7, 13, 17, 23, 34]
Array after applying insertion sort: [1, 2, 7, 13, 17, 23, 34]
```
For the sake of understanding let’s take another array as input.  
Array: `[9, 4, 6, 2, 7, 11, 3, 5]`  
This is the output that will be generated on passing the above array:
```
Array after iteration 1: [4, 9, 6, 2, 7, 11, 3, 5]
Array after iteration 2: [4, 6, 9, 2, 7, 11, 3, 5]
Array after iteration 3: [2, 4, 6, 9, 7, 11, 3, 5]
Array after iteration 4: [2, 4, 6, 7, 9, 11, 3, 5]
Array after iteration 5: [2, 4, 6, 7, 9, 11, 3, 5]
Array after iteration 6: [2, 3, 4, 6, 7, 9, 11, 5]
Array after iteration 7: [2, 3, 4, 5, 6, 7, 9, 11]
Array after applying insertion sort: [2, 3, 4, 5, 6, 7, 9, 11]
```
## Performance
- Worst-case time complexity: `O(n^2)`
- Best case time complexity: `O(n)`
- Average case time complexity: `O(n^2)`
- Auxiliary space complexity: `O(1)`

## Conclusion
After studying the above algorithm and going through examples it is clear that insertion sort works best when the elements are nearly sorted or the input size is small. In these two cases, the insertion sort will perform better than most of the sorting algorithms. This is the reason insertion sort is also used as a base case for highly complex sorting algorithms like [Merge Sort](/data-structures-algorithms/merge-sort/) and Quick Sort. 