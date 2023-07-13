---
title: "What Is Span In C#? And How It Improves The Performance?"
seoDescription: "Span Is A ref struct In C# That Can Help You Save A Lot Of Memory Allocation. Because Span Is a ref struct It Can Be Only Allocated On The Stack."
datePublished: Thu Mar 24 2022 09:36:38 GMT+0000 (Coordinated Universal Time)
cuid: cl14svndk00e10vnvashhdfa1
slug: what-is-span-in-csharp-and-how-it-improves-the-performance
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1650274340823/yT9bX_LOw.png
tags: csharp, performance, dotnet, dotnetcore, programming-tips

---

## What Is Span?
Span is a `ref struct` in c# that can help you save a lot of memory allocation. Because span is `ref struct` it can be only allocated on the stack and not on the heap which means it does not require garbage collection which effectively means that there gonna be no pause in your application for garbage collection. Let's see what span is and how you can use it with an example:

## Example
In this example, we need to get extract the day, month, year from a string that is formatted in a specific way. For that, we can use the `Substring` method.

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

It looks just right but here is a problem that this is allocating a string on the heap each time we use the `Substring` method which is expensive because then the garbage collector needs to go and do some garbage collection which effectively makes your application pause. We can fix this issue using a `ReadOnlySpan`.

### With Span
```csharp
var dateAsText = "03 02 2022";

// Create a tuple with (Day, Month, Year) in it.
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

This code actually performs better than the previous one we will talk about how it does that after benchmarking both functions let's do some benchmarking now with the `Benchmark.NET` library. If you need to know how to use the Benchmark.NET library to Benchmark.NET code read this blog post: [Benchmark Your .NET Code With Benchmark.NET](https://programmingfire.com/benchmark-your-dotnet-code-with-benchmark-dotnet)

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

As you can see the `DateAsTupleWithSpan` function is more than half faster than the `DateAsTupleWithSubstring` one which is a big performance improvement but the interesting thing that happens on the memory allocation which you can see the span did not allocate even a single byte on the heap where the substring one allocated `96B` on the heap which then needs to be garbage collected through the garbage collector which again pauses your application. But really how span did the same thing that substring did but without allocating even a single byte on the heap. Here's a diagram that shows this. 

## How Does Substring Works?
![HowSpanWorks.dio-Substring.drawio.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648114283029/zn_wpS_KL.png)

## How Does Span Works?

![HowSpanWorks.dio-Span.drawio.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1648114378004/QCy_TdmPU.png)
