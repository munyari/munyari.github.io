---
layout: post
title: The Flyweight Pattern in Java
date: 2016-10-07 14:16:38 -0400
tags: programming java design-patterns
---

Types in Java fall into two categories, the [primitive types][prim] (`char`,
`boolean`, `byte`, `short`, `int`, `long`, `float`, `double`) and the [reference
types][ref] (objects and arrays). Each primitive type has a corresponding
reference type, often called a boxed primitive, which allow these types to be
used anywhere an object would be required, as well as adding some useful methods
and fields. Conversion from a primitive to a boxed primitive is called boxing
and the converse is called unboxing. Objects are typically created by invoking a
constructor, or using a static factory method. One of the major differences
between these two techniques is that a constructor requires a new object's
reference to be returned, while a factory method merely has to return an
object a subtype of that type[^fn5]. This means that the same object can be
returned on multiple invocations when using a factory method.

[^fn5]: *Effective Java*, Item 1

[ref]: http://www.docstore.mik.ua/orelly/java-ent/jnut/ch02_10.htm
[prim]: https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html

As a result static factory methods facilitate the implementation of the
[flyweight pattern][flyweight]. In the case of an immutable class, if some
values are requested much more frequently than others, it may make sense from a
performance and memory perspective for the class to keep a cache of the most
commonly requested values, instead of creating a new copy wherever one is
requested. The [Java Language Specification (JLS)][jls] gives us a simple
case-study of this pattern in use. According to the JLS, if `p` is an `int` in
\\([-128, 127]\\) it is guaranteed that two boxing operations on `p` will return
the same reference. In other words, the `Integer` class must cache all of the
values in \\([-128, 127]\\)[^fn1].

[^fn1]: there are similar rules for `Character` and `Boolean`

On my laptop (openjdk 8.u102-1)[^fn2], running

```java
public class Flyweight
{
  public static void main(String[] args)
  {
    System.out.println(Integer.valueOf(127) == Integer.valueOf(127));
    System.out.println(Integer.valueOf(128) == Integer.valueOf(128));
    System.out.println(new Integer(127) == new Integer(127));
  }
}
```

prints

```java
true
false
false
```

[^fn2]: Note that this behavior is JVM specific, as a JVM is only required to cache the values in \\([-128, 127]\\), but may cache more.

As discussed above, explicitly invoking the constructor twice will create two
new objects. Since `==` on objects tests equality of reference, this will fail.
The first example shows that `Integer.valueOf(127)` will always return the same
reference. Relying on this behavior may result in subtle bugs: `==` may work in
the most common cases and fail in rarer cases. If your application uses the
first 127 integers often your application may work seamlessly for months before
the bug appears, at which point you'll probably spend a lot of time attempting
to debug this simple error. As a rule of thumb, when testing object equality `==` will rarely do want you want; use the `equals()` method instead.

[flyweight]: https://en.wikipedia.org/wiki/Flyweight_pattern
[jls]: https://docs.oracle.com/javase/specs/jls/se8/html/jls-5.html#jls-5.1.7


Some takeaways:
<!-- - For an immutable class, use factory methods instead of constructors where -->
<!--   possible to[^2]. -->

- Don't use `==` comparison on objects. It could potentially lead to a subtle
  bug. Instead use `Object.equals()`.
- Consider using `valueOf` or other static factories instead of constructors to
  take advantage of caching. This can positively impact your applications
  performance.[^fn4]
- Don't use the boxed primitive constructors.[^fn3]


[^fn3]: The constructors of the boxed primitives are [deprecated in Java 9][dep]
[^fn4]: There are other advantages of using static factories over constructors, see the first chapter of *Effective Java* for a discussion of the trade-offs.

[dep]: http://download.java.net/java/jdk9/docs/api/java/lang/Integer.html#Integer-int-

