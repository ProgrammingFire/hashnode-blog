## Benchmark Your .NET Code With Benchmark

## What is Benchmark.NET?
Benchmark.NET helps you to transform methods into benchmarks, track their performance, and share reproducible measurement experiments. It's no harder than writing unit tests! Under the hood, it performs a lot of magic that guarantees reliable and precise results thanks to the perfolizer statistical engine. Benchmark.NET protects you from popular benchmarking mistakes and warns you if something is wrong with your benchmark design or obtained measurements. The results are presented in a user-friendly form that highlights all the important facts about your experiment.

## Benchmark Example
### Defining Benchmarks
This Is A Sample Benchmark Using Benchmark.NET Comparing The Performance Of MD5 And SHA256 For Hashing:

```csharp
using System;
using System.Security.Cryptography;
using BenchmarkDotNet.Attributes;

public class Md5VsSha256
{
        private const int N = 10000;
        private readonly byte[] data;

        private readonly SHA256 sha256 = SHA256.Create();
        private readonly MD5 md5 = MD5.Create();

        public Md5VsSha256()
        {
            data = new byte[N];
            new Random(42).NextBytes(data);
        }

        [Benchmark]
        public byte[] Sha256() => sha256.ComputeHash(data);

        [Benchmark]
        public byte[] Md5() => md5.ComputeHash(data);
}
```

As You Can See Benchmark Is Not More Than Just A Method Inside Of A Class Which Is A Super Amazing And Super Clean Design In My Opinion. Now To Run These Benchmarks Use The `BenchmarkRunner` Class Like This:

### Running Benchmarks
```csharp
using BenchmarkDotNet.Running;

BenchmarkRunner.Run<Md5VsSha256>();
```

Now When You Are Gonna Run This Project You Will See Some Stuff Happening. This Library Under The Hood Gonna Run Your Benchmark A Lot Of Time To Get The Best Average Results. After Some Time You Will See The Benchmark Results Printed To The Console Which Looks Like This.

```
| Method |      Mean |     Error |    StdDev |
|------- |----------:|----------:|----------:|
| Sha256 | 100.90 us | 0.5070 us | 0.4494 us |
|    Md5 |  37.66 us | 0.1290 us | 0.1207 us |
```

The Benchmark Results Is In Markdown Table Format So You Can Render It Like A Table In A Markdown View Like This.

| Method |      Mean |     Error |    StdDev |
|------- |----------:|----------:|----------:|
| Sha256 | 100.90 us | 0.5070 us | 0.4494 us |
|    Md5 |  37.66 us | 0.1290 us | 0.1207 us |

This Is Just One Benchmark To Calculate The Speed Of The Hashing Function. But Now We Have Another Benchmark Class Which Needs To Calculate The Speed But Also The Memory Allocation.

## Benchmark Example With Memory Diagnoser
### Defining Benchmarks
```csharp
public class StringBenchmark
{
	[Benchmark]
	public string StringFormat()
	{
		return String.Format("{Year}/{Month}/{Day}", 2010, 02, 03);
	}

	[Benchmark]
	public string StringConcat()
	{
		return String.Concat(2010, "/", 02, "/", 03);
	}

	[Benchmark]
	public string StringConcat_ToString()
	{
		return String.Concat(2010.ToString(), "/", 02.ToString(), "/", 03.ToString());
	}

	[Benchmark]
	public string StringBuilder()
	{
		var builder = new StringBuilder();
		builder.Append(2010);
		builder.Append("/");
		builder.Append(02);
		builder.Append("/");
		builder.Append(03);
		return builder.ToString();
	}

	[Benchmark]
	public string StringInterpolation()
	{
		return $"{2010}/{02}/{03}";
	}
}
```

Now This Is A Normal Benchmark Comparing The Different Methods For Building A String With Integers In It. In This Benchmark I Don't Care About The Speed A Lot But I Care About The Memory Allocation Because Strings Are Expensive For The Memory To Diagnose The Memory We Can Add The `MemoryDiagnoser` Attribute To Our Class.

```csharp
[MemoryDiagnoser]
public class StringBenchmark
```

Now Run The Benchmarks With The Same `BenchmarkRunner.Run` Method Like This:

### Running The Benchmarks With Memory Diagonser
```csharp
using BenchmarkDotNet.Running;

BenchmarkRunner.Run<StringBenchmark>();
```

Now The Results Have Information About The Memory Allocation And The Garbage Collection Time Which We Need In This Scenario.

```
|                Method |      Mean |     Error |    StdDev |    Median |  Gen 0 | Allocated |
|---------------------- |----------:|----------:|----------:|----------:|-------:|----------:|   
|          StringFormat | 183.87 ns |  5.614 ns | 15.834 ns | 180.06 ns | 0.1070 |     112 B |
|          StringConcat | 194.34 ns |  5.565 ns | 15.421 ns | 190.29 ns | 0.2599 |     272 B |
| StringConcat_ToString | 130.83 ns |  2.715 ns |  3.806 ns | 130.38 ns | 0.1299 |     136 B |
|         StringBuilder |  94.70 ns |  5.123 ns | 14.780 ns |  87.33 ns | 0.1377 |     144 B |
|   StringInterpolation | 157.69 ns | 13.882 ns | 40.931 ns | 142.50 ns | 0.0381 |      40 B |
```

|                Method |      Mean |     Error |    StdDev |    Median |  Gen 0 | Allocated |
|---------------------- |----------:|----------:|----------:|----------:|-------:|----------:|   
|          StringFormat | 183.87 ns |  5.614 ns | 15.834 ns | 180.06 ns | 0.1070 |     112 B |
|          StringConcat | 194.34 ns |  5.565 ns | 15.421 ns | 190.29 ns | 0.2599 |     272 B |
| StringConcat_ToString | 130.83 ns |  2.715 ns |  3.806 ns | 130.38 ns | 0.1299 |     136 B |
|         StringBuilder |  94.70 ns |  5.123 ns | 14.780 ns |  87.33 ns | 0.1377 |     144 B |
|   StringInterpolation | 157.69 ns | 13.882 ns | 40.931 ns | 142.50 ns | 0.0381 |      40 B |

Now We Can Know That `StringConcat` Is The Slowest And The Least Memory Efficient Because Of Boxing In C# But The `StringInterpolation` Is The Best In Memory Allocation Because It Does Not Do Any Boxing Or Unboxing. But As You Can See It Is Very Easy To Do Benchmark Your .NET Code With Benchmark.NET.

## Next Steps
Learn More About Benchmark.NET On benchmarkdotnet.org