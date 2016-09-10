---
layout: post
title: Topcoder LargestSubsequence Problem
date: 2016-09-05 15:16:14 -0400
categories: programming algorithms
---

Of late, I've been solving a lot of problems on [Topcoder][topcoder] to brush
up on my algorithmic problem solving skills. [LargestSubsequence][ls] is one of
the problems that I solved recently. Here's the statement of the problem:

[topcoder]: https://www.topcoder.com/
[ls]: https://community.topcoder.com/stat?c=problem_statement&pm=11471

> For Strings x and y, we say y is a subsequence of x if y can be obtained from
> x by erasing some (possibly all or none) of the letters in x. For example,
> "tpcdr" is a subsequence of "topcoder", while "rt" is not.
> Given a String s, return the lexicographically largest subsequence of s.

How does one go about finding a solution to such a problem? An efficient and
correct solution was not immediately obvious to me. An approach I find commonly
useful is to begin thinking about the brute-force solution, and then try to
optimize away the repeated work. In this case, the brute-force solution would
be to generate all of the subsequences of the string, and then compare each of
these to find the largest. There are \\(2^n\\) possible subsequences, so the
brute-force solution would run in \\(O(2^n)\\) time. If you're familiar with
asymptotic notation, you will recognize that this is unacceptably slow.
Further, I don't see a way to work from the brute-force solution to a more
efficient solution.

Let's try a different approach: what information can we glean about the
solution from the problem statement? It turns out that the first character of
the solution *must be* the largest character in the string. To understand why
this is, think about any two subsequences such that the first character of one
subsequence is the largest character in the string, and the first character of
the second subsequence is any other character in the string. The first
subsequence is always lexicographically larger than the second. Some further
analysis makes it clear that the second letter of the solution is the largest
letter in the substring between the first letter we just found and the end of
the input string. In other words, we have a recurrence:

```
largestString(s) = maxCharInS + largestString(s[maxIndex+1..])
```

In code, this looks like

```java
public String getLargest(String s)
{
  if (s.length() == 0) return s;

  int maxIndex = 0;
  for (int i = 1; i < s.length(); i++) {
    if (s.charAt(i) > s.charAt(maxIndex)) {
      maxIndex = i;
    }
  }

  return s.charAt(maxIndex) + getLargest(s.substring(maxIndex+1));
}
```

Let's consider the performance of this approach. The worst case here would be
if the string is sorted in descending order. In that case, you would compare
every pair of characters in the string. There are \\(\binom{N}{2} =
\frac{n(n-1)}{2}\\) comparisons made, for an overall running time of
\\(O(n^2)\\).  The space complexity of this solution is \\(O(n).\\)

With the constraints of the problem in the statement (the length of \\(s\\)
will never be greater than 50), an \\(O(n^2)\\) solution more than suffices.
However, you can imagine that if the string had 10 million elements, this
approach would be too slow. Can we do any better? And if so, how much better?

In the earlier mentioned worst case, the correct solution would be return the
whole string. In other words, if you can verify that a string is strictly
descending, you can return it right away. However, you cannot verify that a
string is strictly descending in less than \\(O(n)\\). Maybe you could think of
a quicker way to construct the string? In general, we must create a new string,
since the solution will not necessarily be a substring of the input.  Then, in
the worst case, the space complexity *must* be \\(O(n)\\). This means that the
time complexity of the best algorithm is at least linear in the worst case
because we need \\(O(n)\\) to access \\(O(n)\\) memory locations.

Let's find one such algorithm. Notice that the last letter of the input string
always appears in the solution. Here's an informal proof why (we'll use
induction on the earlier-defined recurrence): if the string is of length one,
then the whole string is returned, and the last character is in the solution.
Now suppose this holds for a string of length \\(n\\). Add a single character
\\(c\\) to the beginning of the input string. There are two possible solutions:

1. If \\(c\\) is greater than every element in the rest of the string, the
   result is `c + largestString(s[1..])`
2. Otherwise, the result is `largestString(s[1..])`

By induction, the last letter will be included.

So here's the approach: set the max character to be the last character in the
string and add it to a `StringBuilder` (Java's mutable string class). Iterating
from the back of the string to the front, everytime we see a character larger
than the current max, we update the max and add the character to the front of
the `StringBuilder`.  Finally, we call `toString()` to return a string as
required.

Here's the code:

```java
public String getLargest(String s) {
  StringBuilder sb = new StringBuilder(s.length());
  char max = '`'; // 1 less than 'a', similar to using -1 for non-negative ints
  for (int i = s.length() - 1; i >= 0; i--) {
    char c = s.charAt(i);
    if (c >= max) {
      max = c;
      sb.insert(0, c);
    }
  }
  return sb.toString();
}
```

So there we have it, a linear time algorithm. Thanks for reading!
