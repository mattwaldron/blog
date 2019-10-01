layout: page
title: "LINQ Basics"
permalink: /linqbasics/
#LINQ

Many languages support concepts of map, filter, and reduce:
* map is 1:1, and translates elements from one value or type to another
* filter is many:few, and does not translate types or modify values
* reduce is many:1 with sometimes a change in type (e.g.: average)

LINQ's versions of map, filter, and reduce are select, where, and aggregate.  Each takes a predicate representing the transformation or filter criteria.

For example, to create a list of powers of two, one could do:
```csharp
var powersOfTwo = Enumerable.Range(0, 10).Select(x => Math.Pow(2, x)).ToList();
// Output: 1, 2, 4, 8, 16, 32, 64, 128, 256, 512
```

The predicate for Select can take one argument, as above, or a second argument for the index:
```csharp
powersOfTwo.Select((x, i) => $"2^{i} = {x}");
// Output: 2^0 = 1, 2^1 = 2, 2^2 = 4, 2^3 = 8, 2^4 = 16, 2^5 = 32, 2^6 = 64, 2^7 = 128, 2^8 = 256, 2^9 = 512
```

The predicate for filter must return a boolean, and values yield true are passed-through the filter stage.  For example, to see what powers of two yield [Mersenne primes](https://en.wikipedia.org/wiki/Mersenne_prime), one can do:
```csharp
powersOfTwo.Where(x => IsPrime(x-1));
// Output: 4, 8, 32, 128
```

TODO: describe aggregate
