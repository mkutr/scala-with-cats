## Data and Codata Extensibility

We have seen that codata can represent types with an infinite number of elements, such as the infinite stream we just implemented. This is one expressive difference from data, which must always be finite. We'll now look at another, which is the type of extensibility we get from data and from codata. Together these gives use guidelines to choose between the two.

Firstly, let's define extensibility. It means the ability to extend code wihtout modifying existing code. (IOf we allow modification of existing code then any extension becomes trivial.) In particular there are two dimension along which we can extend code: adding new functions or adding new elements. We will see that data and codata have orthogonal extensibility: it's easy to add new functions to data but addimg new elements is impossible, while adding new elements to codata is straight-forward but adding new functions is impossible.

Let's start with a concrete example of both data and codata. For data we'll use the familiar `List` type.

```scala mdoc:silent
enum List[A] {
  case Empty()
  case Pair(head: A, tail: List[A])
}
```

For codata, we'll use `Set` as our exemplar.

```scala mdoc:silent
trait Set[A] {
  def contains(elt: A): Boolean
  def insert(elt: A): Set[A]
  def union(that: Set[A]): Set[A]
}
```

We know there are lots of methods we can define on `List`. The standard library is full of them! We also know that any method we care to write can be written using structural recursion. Finally, we can write these methods without modifying existing code.

Imagine `filter` was not defined on `List`. We can easily implement it as

```scala mdoc:silent
def filter[A](list: List[A], pred: A => Boolean): List[A] = 
  list match {
    case Empty() => Empty()
    case Pair(head, tail) => 
      if pred(head) then Pair(head, filter(tail, pred))
      else filter(tail, pred)
  }
```

We could even use an extension method to make it appear as a normal method.

```scala mdoc:silent
extension [A](list: List[A]) {
  def filter[A](pred: A => Boolean): List[A] = 
    list match {
      case Empty() => Empty()
      case Pair(head, tail) => 
        if pred(head) then Pair(head, tail.filter(pred))
        else tail.filter(pred)
    }
}
```

So we add new functions to data without issue.

What about adding new elements to data? Perhaps we want to add a special case to optimize single-element lists. It is impossible without changing existing code. By definition, we cannot add a new element to an `enum` without changing the definition of the `enum`, and doing so breaks all existing functions.

So in summary we can add new functions to data, but not new elements.

Now let's look at codata. This has the opposite extensibility; duality strikes again! In the codata case we can easily add new elements. We simply implement the `trait` that defines the codata interface. We saw this when we defined, for example, `ListSet`.

**code goes here**

What about adding new functionality? If the functionality can be defined in terms of existing functionality then we're ok. We can easily define this functionality, and we can use the extension method trick to make it appear like a built-in. However, if we want to define a function that cannot be expressed in terms of existing functions we are out of luck. Let's saw we want to define some kind of iterator over the elements of a `Set`. We might use a `LazyList`, the standard library's equivalent of `Stream` we defined earlier, because we know some sets have an infinite number of elements. Well, we can't do this without changning the definition of `Set`, which in turn breaks all existing implementations. We cannot define it in a different way because we don't know all the possible implementations of `Set`.

So in summary we can add new elements to codata, but not new functions.

If we tabulate this we clearly see that data and codata have orthogonal extensibility.

**table here**

This difference in extensibility gives us another rule for choosing between data and codata as an implementation strategy, in addition to the finite vs infinite distinction we saw earlier. If we want extensibilty of functions but not elements we should use data. If we have a fixed interface but an unknown number of possible implementations we should use codata.

You might wonder if we can have both forms of extensibility. Achieving this is called the **expression problem**. There are various ways to solve the expression problem, and we'll see one that works particularly well in Scala in a later chapter.
>>>>>>> 9467b5a (First draft of extensibility section)