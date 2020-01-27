## Set Operations
LINQ exposes [set](https://en.wikipedia.org/wiki/Set_theory) operations that are (intended to be) obviously intuitive.

Concat, is not necessarily a set operation, but combines two enumerables, keeping duplicates.  We'll use the series from the 'enumerables' page for an example:
```csharp
var fibonacci = MetalSeries.Gold.Take(10).ToArray();
var pell = MetalSeries.Silver.Take(7).ToArray();
var silverAndGold = fibonacci.Concat(pell);
// Result: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 0, 1, 2, 5, 12, 29, 70
```

Distinct is also not a traditional set operation, but is equivalent to the `set` function in Python.  Distinct removes duplicate items from an enumerable:
```csharp
silverAndGold.Distinct();
// Result: 0, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 12, 29, 70: 
```

Distinct can be customized in case the equality of two object is not obvious, or if you want to customize the meaning of equality (e.g.: two numbers are close enough that they can be considered equal for the application).  This is a little cumbersome, as it involves defining an EqualityComparer, rather than consuming a predicate like many LINQ functions:
```csharp
class ToleranceComparer : EqualityComparer<double>
{
    private readonly double tolerance;
    public ToleranceComparer(double t) { tolerance = t; }

    public override bool Equals(double x, double y) => Math.Abs(x - y) < tolerance;
    public override int GetHashCode(double v) => 0;
}

...

Enumerable.Repeat<Func<double>>(() => random.NextDouble(), 100)
                .Select(f => f())
                .Distinct(new ToleranceComparer(0.1))
                .OrderBy(v => v);
// Result: 0.004274, 0.1563, 0.3027, 0.413, 0.5751, 0.6917, 0.8103, 0.9659
```

Union is the combination of values in two sets, which ends up being the combination of Concat and Distinct:
```csharp
fibonacci.Union(pell);
// Result: 0, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 12, 29, 70
fibonacci.Concat(pell).Distinct().SequenceEqual(fibonacci.Union(pell)); // true
```

Intersect yields the numbers common to both sets:
```csharp
fibonacci.Intersect(pell);  // 0, 1, 2, 5
```