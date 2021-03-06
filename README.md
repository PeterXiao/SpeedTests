# Speed Tests

When I learn a new programming language, I always implement the
Münchausen numbers problem in the given language. The problem is
simple but it includes a lot of computations, thus it gives an
idea of the execution speed of a language.

## Münchausen numbers

A [Münchausen number](https://en.wikipedia.org/wiki/Perfect_digit-to-digit_invariant)
is a number equal to the sum of its digits raised to each digit's power.

For instance, 3435 is a Münchausen number because
3<sup>3</sup>+4<sup>4</sup>+3<sup>3</sup>+5<sup>5</sup> = 3435.

0<sup>0</sup> is not well-defined, thus we'll consider 0<sup>0</sup>=0.
In this case there are four Münchausen numbers: 0, 1, 3435, and 438579088.

## Exercise

Write a program that finds all the Münchausen numbers. We know that the largest
Münchausen number is less than 440 million.

## Updates

Dates are in `yyyy-mm-dd` format.

**2020-06-24:** Cache should be a global array everywhere. Remove buffered output, we only
print 4 lines. I also started to measure the execution times with [hyperfine](https://github.com/sharkdp/hyperfine).
A benchmark automation solution is also being built.

**2020-06-23:** Debug output was removed, thus the output of the programs is only 4 lines now.
All benchmarks were re-run. Lesson learned: printing to stdout is expensive.

## Implementations

In the implementations I tried to use the same (simple) algorithm in order
to make the comparisons as fair as possible.

All the tests were run on my home desktop machine (Intel Core i5-2500 CPU @ 3.30GHz with 4 CPU cores)
using Linux. Execution times are wall-clock times and they are measured with
[hyperfine](https://github.com/sharkdp/hyperfine) (warmup runs: 2, benchmarked runs: 3).

The following implementations were received as pull requests: D, Zig.

If you know how to make something faster, let me know!

Languages are listed in alphabetical order.

The EXE files were not stripped. The indicated sizes could be further reduced with the
command `strip -s`.

### C

* gcc (GCC) 10.1.0
* clang version 10.0.0

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `gcc -O2 main.c -o main -lm` | 5.724 ± 0.003 | 16,680 |
| `clang -O2 main.c -o main -lm` | 4.414 ± 0.003 | 16,624 |

Note: switches `-O3` and `-Ofast` gave the same result as `-O2`, so
they were removed from the table.

[see source](c)


### C#

* .NET Core SDK (3.1.103)

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `dotnet publish -o dist -c Release` | 8.038 ± 0.043 | 93,592 |


[see source](cs)


### C++

* g++ (GCC) 10.1.0
* clang version 10.0.0

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `g++ -O2 --std=gnu++2a main.cpp -o main` | 5.688 ± 0.004 | 17,200 |
| `clang++ -O2 --std=c++2a main.cpp -o main` | 4.862 ± 0.003 | 17,144 |

[see source](cpp)


### D

* DMD64 D Compiler v2.092.0
* gdc (GCC) 10.1.0
* LDC - the LLVM D compiler (1.21.0)

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `dmd -release -O main.d` | 12.409 ± 0.041 | 1,952,592 |
| `gdc -frelease -Ofast main.d -o main` | 5.864 ± 0.005 | 2,328,800 |
| `ldc2 -release -O main.d` | 4.871 ± 0.005 | 20,536 |

[see source](d)


### Dart

* Dart VM version: 2.8.3 (stable) (Tue May 26 18:39:38 2020 +0200) on "linux_x64"
* Node.js v14.3.0

| Execution | Runtime (sec) | compiled / transpiled output size (bytes) |
|-----|:---:|:---:|
| `dart main.dart` | 30.639 ± 0.006 | -- |
| `dart2native main.dart -o main && ./main` | 17.458 ± 0.019 | 5,944,552 |
| `dart2js main.dart -m -o main.js && node main.js` | 15.541 ± 0.038 | 33,034 |

(`*`): in the first case, the Dart code is executed as a script

[see source](dart)


### Go

* go version go1.14.4 linux/amd64

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `go build -o main` | 9.124 ± 0.004 | 2,078,422 |

[see source](go)


### Java

* java version "1.8.0_201"

| Execution | Runtime (sec) | Binary size (bytes) |
|-----|:---:|:---:|
| `javac Main.java && java Main` | 8.025 ± 0.006 | 986 |

(`*`): the binary size is the size of the `.class` file

[see source](java)


### Kotlin

* Kotlin version 1.3.72-release-468 (JRE 1.8.0_201-b09)
* java version "1.8.0_201"

| Execution | Runtime (sec) | JAR size (bytes) |
|-----|:---:|:---:|
| `kotlinc main.kt -include-runtime -d main.jar && java -jar main.jar` | 7.935 ± 0.005 | 1,364,024 |

[see source](kotlin)


### Nim

* Nim Compiler Version 1.2.2 [Linux: amd64]
* gcc (GCC) 10.1.0
* clang version 10.0.0

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `nim c -d:release --gc:arc main.nim` | 7.068 ± 0.004 | 69,280 |
| `nim c -d:release main.nim` | 6.905 ± 0.004 | 89,024 |
| `nim c -d:danger --gc:arc main.nim` | 6.755 ± 0.007 | 46,864 |
| `nim c -d:danger main.nim` | 6.69 ± 0.005 | 80,112 |
| `nim c --cc:clang -d:release main.nim` | 6.487 ± 0.007 | 68,848 |
| `nim c --cc:clang -d:release --gc:arc main.nim` | 5.981 ± 0.003 | 48,864 |
| `nim c --cc:clang -d:danger --gc:arc main.nim` | 5.792 ± 0.004 | 38,912 |
| `nim c --cc:clang -d:danger main.nim` | 5.65 ± 0.005 | 68,040 |

(`*`): if `--cc:clang` is missing, then the default `gcc` was used

[see source](nim)


### Python 3

* Python 3.8.3
* Python 3.6.9 (?, Apr 17 2020, 09:36:06) [PyPy 7.3.1 with GCC 9.3.0]

| Compilation | Runtime (sec) | Notes |
|-----|:---:|:---:|
| `python3 main.py` | 418.923 ± 1.797 | -- |
| `pypy3 main.py` | 25.615 ± 0.097 | -- |

[see source](python3)


### Rust

* rustc 1.42.0 (b8cedc004 2020-03-09)

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `cargo build --release` | 4.987 ± 0.002 | 2,654,648 |

Stripped size of the EXE: `203,072` bytes.

[see source](rust)


### Zig

* zig 0.6.0

| Compilation | Runtime (sec) | EXE size (bytes) |
|-----|:---:|:---:|
| `zig build -Drelease-fast` | 4.88 ± 0.008 | 172,552 |

Stripped size of the EXE: `6,000` bytes. And it's statically linked!

[see source](zig)
