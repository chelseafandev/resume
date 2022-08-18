## tcmalloc 

[**tcmalloc github**](https://github.com/google/tcmalloc)
[**google performance tool github**](https://github.com/gperftools/gperftools)
[**pprof github**](https://github.com/google/pprof)

TCMalloc is Google's customized implementation of C's `malloc()` and C++'s `operator new` used for memory allocation within our C and C++ code. TCMalloc is a fast, multi-threaded malloc implementation.

오리지널 pprof는 더 이상 지원하지 않음(대신, Golang용으로 pprof를 제공하고 있음)
> gperftools is a collection of a high-performance multi-threaded `malloc()` implementation, plus some pretty nifty performance analysis tools.

> gperftools was original home for pprof program. But do note that original pprof (which is still included with gperftools) is now deprecated in favor of Go version at [https://github.com/google/pprof](https://github.com/google/pprof)