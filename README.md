# cs-analysis-of-algorithms-readme

## What You Should Know Before Reading This

  * [Java List Interface](http://www.codejava.net/java-core/collections/java-list-collection-tutorial-and-examples)
  * [Selection Sort](http://www.go4expert.com/articles/selection-sort-algorithm-absolute-t27888/)
  * [Java ArrayList Usage](http://www.tutorialspoint.com/java/java_arraylist_class.htm)

## Overview

This README reviews the basic ideas of algorithm analysis.  After this lesson, you should be able to compare algorithms written in Java and predict which will be more efficient for large problems.


## Objectives

1.  Describe the goals of algorithm analysis.
2.  Define constant time, linear time, and quadratic time algorithms.
3.  Analyze a Java function and classify its run time.
4.  Read and write Big O notation.
5.  Analyze the selection sort algorithm.


## Algorithm analysis

If you have worked with the Java Collections Framework (JCF), you might have wondered why Java provides two implementations of the List interface.  The short answer is that for some methods `LinkedList` is faster, and for other methods `ArrayList` is faster.  Which one is better depends on how you use it.

To decide which one is better for a particular application, one approach is to try them both and see how long they take.  This approach, which is called "profiling" has a few problems:

1.  Before you compare the algorithms, you have to implement them both.

2.  The results might depend on what kind of computer you use.  One algorithm might be better on one machine; the other might be better on a different machine.

3.  The results might depend on the size of the problem or the data provided as input.

We can address some of these problems using [analysis of algorithms](http://en.wikipedia.org/wiki/Analysis_of_algorithms).  When it works, we can use algorithm analysis to compare algorithms without having to implement them.  But we have to make some assumptions:

1.  To avoid dealing with the details of computer hardware, we usually identify the basic operations that make up an algorithm — like addition, multiplication, and comparison of numbers — and count the number of operations each algorithm requires.

2.  To avoid dealing with the details of the input data, we try to analyze the average performance for the inputs we expect.  If that's not possible, a common alternative is to analyze the worst case scenario, invoking the principle that we should hope for the best, but prepare for the worst.

3.  Finally, we have to deal with the possibility that one algorithm works best for small problems and another for big ones.  In that case, we usually focus on the big ones, because for big problems a good algorithm is often much faster than a bad one.

This kind of analysis lends itself to simple classification of algorithms.  For example, if we know that the run time of Algorithm A tends to be directly proportional to the size of the input, `n`, and Algorithm B tends to be directly proportional to `n`<sup>2</sup>, we expect A to be faster than B, at least for large values of `n`.

Most simple algorithms fall into just a few categories.

*   Constant time:  An algorithm is "constant time" if the run time does not depend on the size of the input.  For example, if you have an array of `n` elements and you use the bracket operator (`[]`) to access one of the elements, this operation takes pretty much the same amount of time regardless of how big the array is.

*   Linear:  An algorithm is "linear" if the run time is directly proportional to the size of the input.  For example, if you add up the elements of an array, you have to access `n` elements and perform `n-1` additions.  The total number of operations (element accesses and additions) is `2n-1`, which is directly proportional to `n`.

*   Quadratic:  An algorithm is "quadratic" if the run time is directly proportional to `n`<sup>2</sup>.  For example, if the input has 2 elements, it might require 4 operations; with 3 elements, it might require 9, and so on.

For example, here's an implementation of a simple algorithm called [selection sort](https://en.wikipedia.org/wiki/Selection_sort):

```java
public class SelectionSort {

	/**
	 * Swaps the elements at indexes i and j.
	 */
	public static void swapElements(int[] array, int i, int j) {
		int temp = array[i];
		array[i] = array[j];
		array[j] = temp;
	}

	/**
	 * Finds the index of the lowest value
	 * starting from the index at `start` (inclusive)
   * and going to the end of the array.
	 */
	public static int indexLowest(int[] array, int start) {
		int lowIndex = start;
		for (int i = start; i < array.length; i++) {
			if (array[i] < array[lowIndex]) {
				lowIndex = i;
			}
		}
		return lowIndex;
	}

	/**
	 * Sorts the elements (in place) using selection sort.
	 */
	public static void selectionSort(int[] array) {
		for (int i = 0; i < array.length; i++) {
			int j = indexLowest(array, i);
			swapElements(array, i, j);
		}
	}
}

```

The first method, `swapElements`, swaps two elements of the array.  Reading and writing elements are constant time operations, regardless of the size of the array.  That works because if we know where the beginning of the array is, we can compute the location of element `i` or `j` with one multiplication and one addition.  And those are constant time operations.  Since everything in `swapElements` is constant time, the whole method is constant time.

The second method, `indexLowest` finds the index of the smallest element of the array starting at a given index, `start`.  Each time through the loop, it accesses two elements of the array and performs one comparison.  Since these are all constant time operations, it doesn't really matter which ones we count.  To keep it simple, let's count the number of comparisons.

1.  If `start` is 0, `indexLowest` traverses the entire array, and the total number of comparisons is `n`.

2.  If `start` is 1, the number of comparisons is `n-1`.

3.  In general, the number of comparisons is `n-start`.

So we normally consider `indexLowest` to be linear time, except for special values of `start`.

The third method, `selectionSort`, sorts the array.  It loops from `0` to `n-1`, so the loop executes `n` times.  Each time, it performs a constant time operation, `swapElements`, and then calls `indexLowest`.

The first time `selectionSort` calls `indexLowest`, it performs `n` comparisons.  The second time it performs `n-1` comparisons, and so on.  The total number of comparisons is

    n + n-1 + n-2 + ... + 1 + 0

The sum of this series is `n(n+1)/2`, which is directly proportional to `n`<sup>2</sup>; and that means that `selectionSort` is quadratic.

To get to the same result a different way, we can think of `indexLowest` as a nested loop.  Each time we call `indexLowest`, the number of operations is directly proportional to `n`.  We call it `n` times, so the total number of operations is directly proportional to `n`<sup>2</sup>.


## Big O notation

All constant time algorithms belong to a set called O(1).  So another way to say that an algorithm is constant time is to say that it is in O(1).  Similarly, all linear algorithms belong to O(n), and all quadratic algorithms belong to O(n<sup>2</sup>).  This way of classifying algorithms is called "big O notation".

NOTE: I am providing a casual definition of big O notation.  For a more mathematical treatment, see [Big O notation](https://en.wikipedia.org/wiki/Big_O_notation).

This notation provides a convenient way to write general rules about how algorithms behave when we compose them.  For example, if you perform a linear time algorithm followed by a constant algorithm, the total run time is linear.  Using ∈ to mean "is a member of":

<tt>If f ∈ O(n) and g ∈ O(1), f+g ∈ O(n)</tt>.

If you perform two linear operations, the total is still linear:

<tt>If f ∈ O(n) and g ∈ O(n), f+g ∈ O(n)</tt>.

In fact, if you perform a linear operation any number of times, `k`, the total is linear, as long as `k` is a constant that does not depend on `n`.

<tt>If f ∈ O(n) and k is a constant, kf ∈ O(n)</tt>.

But if you perform a linear operation `n` times, the result is quadratic:

<tt>If f ∈ O(n), nf ∈ O(n<sup>2</sup>)</tt>.

In general, we only care about the largest exponent of `n`.  So if the total number of operations is `2*n + 1`, it belongs to O(`n`).  The leading constant, 2, and the additive term, 1, are not important for this kind of analysis.  Similarly, <tt>n<sup>2</sup> + 100n + 100</tt> is in O(n<sup>2</sup>).

One other piece of vocabulary you should know: an "order of growth" is a set of algorithms whose runtimes grow in the same way as problem size increases; for example, all linear algorithms belong to the same order of growth because their runtimes increase linearly with problem size.

NOTE: In this context, an "order" is a group, like the *Order of the Knights of the Round Table*, which is a group of knights, not a way of lining them up.  So you can imagine the *Order of Linear Algorithms* as a set of brave, chivalrous, and particularly efficient knights.



## Resources

[Analysis of algorithms](http://en.wikipedia.org/wiki/Analysis_of_algorithms)

[Selection sort](https://en.wikipedia.org/wiki/Selection_sort)

[Big O notation](https://en.wikipedia.org/wiki/Big_O_notation)
