# cs-analysis-of-algorithms-readme


## Overview

This README reviews the basic ideas of algorithm analysis.  After this lesson, you should be able to read an algorithm written in Java and classify it as constant time, linear time, or quadratic time.


## Objectives

1.  Describe the goals of algorithm analysis.
2.  Define constant time, linear time, and quadratic time algorithms.
3.  Analyze a Java function and classify its run time.
4.  Read and write Big O notation.
5.  Analyze the selection sort algorithm.


## Algorithm analysis

In the previous README, I posed the question, "Why does Java provide two implementations of the List interface?"  The short answer is that for some methods `LinkedList` is faster, and for other methods `ArrayList` is faster.  Which one is better depends on how you use it.

To decide which one is better for a particular application, one approach is to try them both and measure the runtimes.  But before you get to that point, you can often make a good guess using [analysis of algorithms](http://en.wikipedia.org/wiki/Analysis_of_algorithms).

 
When it works, algorithm analysis can predict performance and guide design decisions.  But there are a few problems we have to get past first:

1.  In practice, the relative performance of two algorithms might depend on characteristics of the hardware, so one algorithm might be faster on Machine A, another on Machine B.  The usual solution to this problem is to specify a machine model and analyze the number of steps, or operations, an algorithm requires under a given model.

2.  Relative performance might depend on the details of the dataset. For example, some sorting algorithms run faster if the data are already partially sorted; other algorithms run slower in this case.  A common way to avoid this problem is to analyze the worst case scenario.

3.  Relative performance also depends on the size of the problem.  A sorting algorithm that is fast for small lists might be slow for long lists.  The usual solution to this problem is to express run time (or number of operations) as a function of problem size, and group functions into categories depending on how quickly they grow as problem size increases.

The good thing about this kind of comparison is that it lends itself to simple classification of algorithms.  For example, if I know that the run time of Algorithm A tends to be proportional to the size of the input, n, and Algorithm B tends to be proportional to n<sup>2</sup>, then I expect A to be faster than B, at least for large values of n.  This kind of analysis comes with some caveats, but we’ll get to that later.

Most simple algorithms fall into just a few categories.

*   Constant time:  An algorithm is "constant time" if the run time does not depend on the size of the input.  For example, if you have an array of n elements and you use the bracket operator (`[]`) to access one of the elements, this operation takes pretty much the same amount of time regardless of how big the array is.

*   Linear:  An algorithm is "linear" if the run time is proportional to the size of the input.  For example, if you add up the elements of an array, you have to access n elements and perform n-1 additions.  The total number of operations (element accesses and additions) is 2n-1, which is proportional to n.

*   Quadratic:  An algorithm is "quadratic" if the run time is proportional to n<sup>2</sup>.

For example, here's an implementation of a simple sort algorithm called [selection sort](https://en.wikipedia.org/wiki/Selection_sort):

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
	 * between indices low and high (inclusive).
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

The first method, `swapElements`, swaps two elements of the array.  Reading and writing elements are constant time operations, so the method is constant time.

The second method, `indexLowest` finds the index of the smallest element of the array starting at a given index, `start`.  Each time through the loop, it accesses two elements of the array and performs one comparison.  Since these are all constant time operations, it doesn't really matter which ones we count.  To keep it simple, let's count the number of comparisons.

1.  If start is 0, `indexLowest` traverses the entire array, and the total number of comparisons is n.

2.  If start is 1, the number of comparisons is n-1.

3.  In general, the number of comparisons is `n-start`.

So we normally consider `indexLowest` to be linear time, except for special values of `start`.

The third method, `selectionSort`, sorts the array.  It loops from 0 to n-1, so the loop executes n times.  Each time, it performs a constant time operation, `swapElements`, and then calls `indexLowest`.

The first time `selectionSort` calls `indexLowest`, it performs n comparisons.  The second time it performs n-1 comparisons, and so on.  The total number of comparisons is

n + n-1 + n-2 + ... + 1 + 0

The sum of this series is n(n+1)/2, which is proportional to n<sup>2</sup>; and that means that `selectionSort` is quadratic.


## Big O notation

All constant time algorithms belong to a set called O(1).  So another way to say that an algorithm is contant-time is to say that it is in O(1).  Similarly, all linear algorithms belong to O(n), and all quadratic algorithms belong to O(n).  This way of classifying algorithms is called "big O notation".

NOTE: I am providing a casual definition of big O notation.  For a more mathematical treatment, see [Big O notation](https://en.wikipedia.org/wiki/Big_O_notation).

This notation provides a convenient way to write some general rules about how algorithms behave when we compose them.  For example, if you perform a linear time algorithm followed by a constant algorithm, the total run time is linear.  Using ∈ to mean "is a member of":

If f ∈ O(n) and g ∈ O(1), f+g ∈ O(n).

If you perform two linear operations, the total is still linear:

If f ∈ O(n), 2f ∈ O(n).

In fact, if you perform a linear operation any number of times, k, the total is linear, as long as k is a constant that does not depend on n.

If f ∈ O(n) and k is a constant, kf ∈ O(n).

But if you perform a linear operation n times, the result is quadratic:

If f ∈ O(n), nf ∈ O(n<sup>2</sup>).

One other piece of vocabulary you should know: an "order of growth" is a set of algorithms whose runtimes grows in the same way as problem size increases; for example, all linear algorithms belong to the same order of growth because their runtimes increase linearly with problem size.



## Resources

[Analysis of algorithms](http://en.wikipedia.org/wiki/Analysis_of_algorithms)

[Selection sort](https://en.wikipedia.org/wiki/Selection_sort)

[Big O notation](https://en.wikipedia.org/wiki/Big_O_notation)

