### IncrementalCH

This repository contains a sequential and parallel implementation of the algorithms in "Randomized Incremental Convex Hull is Highly Parallel" by Guy E. Blelloch et al. using Cilk. It was developed over a short span of time as a final project for CMU 15-418. Issues would be appreciated if readers have comments or suggestions.

There are four main parts:
1. The paper's sequential algorithm.
2. The paper's parallel algorithm.
3. The paper's MultiMap data structure.
4. A parallel quicksort.

## Dependencies

OpenCilk Release 1.0. This can be installed and built from [MIT's website]{http://cilk.mit.edu/}.

Once built, edit the `BUILD_PREFIX` in this project's Makefile to the path prefix to your build. Then the project is ready for usage

## Usage

Once OpenCilk is built and the Makefile is modified, making will generate three executables: **seqhull**, **parhull**, and **quicksort**. Including the timing or not can be changed from inside their respective .cpp files.

1. To use **seqhull**, pipe in a testcase from test2D
2. To use **parhull**, pipe in a testcase from test2D. If it is crashing, then the MultiMap is likely not big enough, so make it bigger in parhull.cpp. 
3. To use **quicksort**, enter the number of items to sort and the program will print how long it took 

## Test Case Generation

There are three test case generator written in Python. They all avoid creating colinear points.

1. **2D_generator.py**: This generates small uniformly random test cases.
2. **2D_generator_fast.py**: This uses primes to generate large test cases with small convex hulls.
3. **2D_generator_parabolic.py**: This uses parabolas to generate large test cases with large convex hulls.

# Important Notes

Unfortunately MIT's OpenCilk does not include a way to set the number of workers during runtime, so the user must inject it manually as an environment variable with the shell command

> `export CILK_NWORKERS=n`

## Known Issues

In addition, OpenCilk's method to get the current worker id is deprecated, so the per thread timing in **parhull** may not be accurate.

This program will not solve arbitrary complex hulls and may crash if given one. The input data must have **no colinear points**. The test cases generators ensure this, but if the user adds any others, make sure that they have no colinear points.
