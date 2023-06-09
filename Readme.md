# TOBDD
First-ever high-throughput thread-safe BDD (Binary Decision Diagrams) library for scalable data plane verification.

As of our research, TOBDD is currently the fastest BDD library available, achieving exceptional performance in various large-scale data plane verification benchmarks and use cases.

> The project was originally named Nanobdd, but we will now refer to it as TOBDD to reflect its focus on throughput-optimized BDD operations.

# Features
- Fully **lock-free** concurrency
- **Automatic referencing** for BDD nodes
- User controlled **garbage collection**
- Easy-to-use APIs by C++ **operator overloading**
- And of course, it is **thread-safe**!

# Install
## Dependencies
TOBDD depends on tbb for concurrent data structures. 

CMake (>=v3.2) and g++(>=v9) is required for compilation.

## Compile and install
Nanobdd follows the standard CMake project structure, the quick installation steps are as follows:
```
git clone https://github.com/guodong/nanobdd
cd nanobdd
mkdir build
cd build
cmake ..
make
sudo make install
```

# Basic usage
A simple c++ code to use TOBDD is as follows:
```c++
// include the nanobdd header file
#include <nanobdd/nanobdd.h>
#include <assert.h>

int main(int argc, char** argv) {
  // init nanobdd with node table size, cache size, and the number of variables
  nanobdd::init(1000, 1000, 3);

  // get the three variables
  auto x = nanobdd::getVar(0);
  auto y = nanobdd::getVar(1);
  auto z = nanobdd::getVar(2);

  // do magic using c++ operators
  auto xy = x & y;
  auto xyz = xy & z;
  auto xyZ = xy & !z;

  assert(xy == (xyz | xyZ));
  assert(xy != nanobdd::bddFalse());

  return 0;
}
```

Compile and execute the above code by:
```
g++ -o exe test.cpp -lnanobdd -ltbb
./exe
```
If no exceptions, that means the assertions are passed.

# Thread-safe concurrency
The most powerful feature of nanobdd is that it is thread-safe, which is achieved lock-free algorithms. One can safely perform any bdd operations in different threads, nanobdd will handle all underlay data contensions.
An example for using C++17 parallel STL:
```c++
std::for_each(
  std::execution::par,
  somebdds.begin(),
  somebdds.end(),
  [&](auto bdd) {
    // operate your bdd here
  });
```
See `examples/paralle.cpp` for full example.

# Performance
We have compared nanobdd with other librarys including Buddy, JDD and Sylvan in a network verification project on a 40 CPU cores server. Typically, nanobdd is 2~10x faster than others.

# Author and contact
Author: Dong Guo (PhD candidate of Tongji University)

Email: gd@tongji.edu.cn
