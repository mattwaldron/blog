# LINQ

## Basics
Many languages support concepts of map, filter, and reduce:
* map is 1:1, and translates elements from one value or type to another
* filter is many:few, and does not translate types or modify values, but just removes items from a collection
* reduce is many:1 with sometimes a change in type (e.g.: average)

LINQ's versions of map, filter, and reduce are Select, Where, and Aggregate.  Each takes a predicate representing the transformation or filter criteria.

For example, to create a list of powers of two, one could do:
```csharp
var powersOfTwo = Enumerable.Range(0, 10).Select(x => Math.Pow(2, x)).ToList();
// Result: 1, 2, 4, 8, 16, 32, 64, 128, 256, 512
```

The predicate for Select can take one argument, as above, or a second argument for the index:
```csharp
powersOfTwo.Select((x, i) => $"2^{i} = {x}");
// Result: 2^0 = 1, 2^1 = 2, 2^2 = 4, 2^3 = 8, 2^4 = 16, 2^5 = 32, 2^6 = 64, 2^7 = 128, 2^8 = 256, 2^9 = 512
```

The predicate for Where (filtering) must return a boolean, and values that yield true are passed-through the filter stage.  For example, to see what powers of two yield [Mersenne primes](https://en.wikipedia.org/wiki/Mersenne_prime), one can do:
```csharp
powersOfTwo.Where(x => IsPrime(x-1));
// Result: 4, 8, 32, 128
```

The boolean predicate used in Where can often be used in other functions.  The First() function returns the first element of an enumerable.  One might be initially inclined to write
```csharp
var firstMersenne = powersOfTwo.Where(x => IsPrime(x-1)).First();
```
but this can be simplified to
```csharp
var firstMersenne = powersOfTwo.First(x => IsPrime(x-1));
```

LINQ's version of reduce is Aggregate.  The predicate passed to Aggregate takes two arguments, and must return a value of the same type.  Aggregate is smart enough to perform the operation on the first to elements, and then subsequent operations on the previous result and the next element.  In contrast, accumulate in the C++ STL must be provided an initial value.  For example:
```csharp
var sigma = Enumerable.Range(1, 10).Aggregate((a, b) => a+b);    // 55
var gamma = Enumerable.Range(1, 10).Aggregate((a, b) => a*b);    // 3628800
```
