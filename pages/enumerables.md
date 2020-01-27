## Enumerables
LINQ operations go hand-in-hand with Enumerables.  These are the things that appear after `in` in `foreach` statements.  For example:
```csharp
foreach (var c in collection) {
	Console.WriteLine(c);
}
```
`collection` implements IEnumerable.  GetEnumerator is what will return the values of `c` to iterate over.

The following example returns Fibonacci and Pell series, but generalizing a mechanism we'll call MetalSeries (since Fibonacci numbers describe a golden ration, Pell numbers describe a silver ratio, etc.):
```csharp
public class MetalSeries : IEnumerable<int>
{
    private readonly int mult;
    public MetalSeries (int m) {
        mult = m;
    }
    
    public IEnumerator<int> GetEnumerator()
    {
        int current = 0, next = 1;
        while(current >= 0) {
            yield return current;
            (current, next) = (next, current + mult * next);
        }
    }   
    
    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
    
    public static MetalSeries Gold => new MetalSeries(1);
    public static MetalSeries Silver => new MetalSeries(2);
    public static MetalSeries Bronze => new MetalSeries(3);
}

...

var fibonacci = MetalSeries.Gold.Take(10).ToArray();  	// 0, 1, 1, 2, 3, 5, 8, 13, 21, 34
var pell = MetalSeries.Silver.Take(10).ToArray();		// 0, 1, 2, 5, 12, 29, 70, 169, 408, 985

```
