# RcppThread

<!-- badges: start -->
[![R build status](https://github.com/tnagler/RcppThread/workflows/R-CMD-check/badge.svg)](https://github.com/tnagler/RcppThread/actions)
[![CRAN version](http://www.r-pkg.org/badges/version/RcppThread)](https://cran.r-project.org/package=RcppThread) 
[![CRAN downloads](http://cranlogs.r-pkg.org/badges/RcppThread)](https://cran.r-project.org/package=RcppThread)
<!-- badges: end -->

Provides R-friendly threading functionality: 

  * thread safe versions of [Rcpp's](http://www.rcpp.org/)
    `checkUserInterrupt()` and `Rcout`,
  * an interruptible thread class that otherwise behaves like 
   [`std::thread`](http://en.cppreference.com/w/cpp/thread/thread),
  * classes for the [thread pool
    pattern](https://en.wikipedia.org/wiki/Thread_pool) and parallel for loops
    for easy and flexible parallelism.

The library is header-only, platform-independent, and only 
requires a 
[C++11-compatible compiler](http://en.cppreference.com/w/cpp/compiler_support#cpp11).

## Functionality

For a detailed description of its functionality and examples, see the associated
[JSS paper](https://doi.org/10.18637/jss.v097.c01)
or the [API documentation](https://tnagler.github.io/RcppThread/).

## Installation

Release version from CRAN:

``` r
install.packages("RcppThread")
```

Latest development version from github:

``` r
# install.packages("devtools")
devtools::install_github("tnagler/RcppThread")
```

## How to use it

###### with cppFunction

Pass `"RcppThread"` to the `depends` argument and `"cpp11"` to the `plugins`
argument. For example:
``` r
Rcpp::cppFunction('void func() { /* actual code here */ }', 
                       depends = "RcppThread", plugins = "cpp11")
```

###### with sourceCpp

Add 
``` cpp
// [[Rcpp::plugins(cpp11)]]
// [[Rcpp::depends(RcppThread)]]
```
before including any headers in your source code.

###### in another R package

1. Add the line `CXX_STD = CXX11` to the `src/Makevars(.win)` files of your package.
2. Add `RcppThread` to the `LinkingTo` field of your `DESCRIPTION` file.

## Automatic override of `std::cout` and `std::thread`

There are preprocessor options to replace all occurrences of `std::cout` and 
`std::thread` with calls to `RcppThread::Rcout` and `RcppThread::Thread` 
(provided that the RcppThread headers are included first). To enable this, use 
```
#define RCPPTHREAD_OVERRIDE_COUT 1    // std::cout override
#define RCPPTHREAD_OVERRIDE_THREAD 1  // std::thread override
```
before including the RcppThread headers.


## References

Nagler, T. (2021). "R-Friendly Multi-Threading in C++." _Journal of Statistical
Software, Code Snippets_, *97*(1), 1-18. [doi: 10.18637/jss.v097.c01](https://doi.org/10.18637/jss.v097.c01)

