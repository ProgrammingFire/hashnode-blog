## What Is Span In C#? And How It Improves The Performance?

## What Is Span?
Span Is A `ref struct` In C# That Can Help You Save A Lot Of Memory Allocation. Because Span Is `ref struct` It Can Be Only Allocated On The Stack And Not On The Heap Which Means It Does Not Require Garbage Collection Which Effectively Means That There Gonna Be No Pause In Your Application For Garbage Collection. Let's See What Span Is And How You Can Use It With An Example:

## Example
In This Example, We Need To Get Abstract The Day, Month, Year From A String That Is Formatted In A Specific Way. For That, We Can Use The `Substring` Method. 

### With Substring
```csharp
var dateAsText = "03 02 2022";

// Create A Tuple With (Day, Month, Year) In It.
(int day, int month, int year) DateAsTupleWithSubstring()
{
    var dayAsText = dateAsText.Substring(0, 2);
    var monthAsText = dateAsText.Substring(3, 2);
    var yearAsText = dateAsText.Substring(6);
    var day = int.Parse(dayAsText);
    var month = int.Parse(monthAsText);
    var year = int.Parse(yearAsText);
    return (day, month, year);
}
```

It Looks Just Right But Here Is A Problem That This Is Allocating A String On The Heap Each Time We Use The `Substring` Method Which Is Expensive Because Then The Garbage Collector Needs To Go And Do Some Garbage Collection Which Effectively Makes Your Application Pause. We Can Fix This Issue Using A `ReadOnlySpan`.

### With Span
```csharp
var dateAsText = "03 02 2022";

// Create A Tuple With (Day, Month, Year) In It.
(int day, int month, int year) DateAsTupleWithSpan()
{
    ReadOnlySpan<char> dateAsSpan = dateAsText;
    var dayAsText = dateAsSpan.Slice(0, 2);
    var monthAsText = dateAsSpan.Slice(3, 2);
    var yearAsText = dateAsSpan.Slice(6);
    var day = int.Parse(dayAsText);
    var month = int.Parse(monthAsText);
    var year = int.Parse(yearAsText);
    return (day, month, year);
}
```

This Code Actually Performs Better Than The Previous One We Will Talk How It Does That After Benchmarking The Both Functions Let's Do Some Benchmarking Now With `Benchmark.NET` Library. If You Need To Know How To Use The Benchmark.NET Library To Benchmark .NET Code Read This Blog Post: [Benchmark Your .NET Code With Benchmark](https://programmingfire.com/benchmark-your-dotnet-code-with-benchmark-dotnet)

## Benchmarks
### Defining Benchmarks
```csharp
using BenchmarkDotNet.Attributes;

[MemoryDiagnoser]
public class SpanVsSubstring
{
    private readonly string dateAsText = "03 02 2022";

    [Benchmark]
    // Create A Tuple With (Day, Month, Year) In It.
    public (int day, int month, int year) DateAsTupleWithSubstring()
    {
        var dayAsText = dateAsText.Substring(0, 2);
        var monthAsText = dateAsText.Substring(3, 2);
        var yearAsText = dateAsText.Substring(6);
        var day = int.Parse(dayAsText);
        var month = int.Parse(monthAsText);
        var year = int.Parse(yearAsText);
        return (day, month, year);
    }

    [Benchmark]
    public (int day, int month, int year) DateAsTupleWithSpan()
    {
        ReadOnlySpan<char> dateAsSpan = dateAsText;
        var dayAsText = dateAsSpan.Slice(0, 2);
        var monthAsText = dateAsSpan.Slice(3, 2);
        var yearAsText = dateAsSpan.Slice(6);
        var day = int.Parse(dayAsText);
        var month = int.Parse(monthAsText);
        var year = int.Parse(yearAsText);
        return (day, month, year);
    }
}
```

### Benchmark Results
|                   Method |      Mean |    Error |    StdDev |    Median |  Gen 0 | Allocated |
|------------------------- |----------:|---------:|----------:|----------:|-------:|----------:| 
| DateAsTupleWithSubstring | 143.13 ns | 8.278 ns | 24.147 ns | 134.34 ns | 0.0918 |      96 B | 
|      DateAsTupleWithSpan |  69.98 ns | 2.363 ns |  6.780 ns |  68.32 ns |      - |         - |

As You Can See The `DateAsTupleWithSpan` Function Is More Than Half Faster Than The `DateAsTupleWithSubstring` One Which Is A Big Performance Improvement But The Interesting Thing Happens On The Memory Allocation Which You Can See The Span Did Not Allocate Even A Single Byte On The Heap Where The Substring One Allocated `96 B` On The Heap Which Then Needs To Be Garbage Collected Through The Garbage Collector Which Again Pauses Your Application. But Really How Span Did The Same Thing That Substring Did But Without Allocating Even A Single Byte On The Heap. Here's A Diagram That Shows This.

## How Does Substring Works?
![HowSpanWorks.dio-Substring.drawio.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648114283029/zn_wpS_KL.png)

## How Does Span Works?

![HowSpanWorks.dio-Span.drawio.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648114378004/QCy_TdmPU.png)
