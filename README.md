# CROLL-PI Interpreter

The goal of this interpreter for the croll-pi calculus is to study its expressiveness from a practical point of view.
The calculus was originally defined by I. Lanese, C. A. Mezzina, A. Schmitt and J-B. Stefani.

## The interpreter

  - croll-interpreter.maude: the source of the interpreter
  - USAGE: a description of the interpreter

## Examples
### Simple Encodings

Using the reversibility of croll-pi, it is possible to encode some error handling mechanism. In the following file, we provide

  - a simple mechanism to deal with transient error that restart failed computations, similar to the stabilizer of [1].
  - a simple try/catch mechanism
  - simple-encodings.maude

[1] Lightweight checkpointing for concurrent ml, J. Funct. Program. 2010


### Reversibility for Speculative Parallelism

It is possible to use reversibility to achieve speculative parallelism. In the following file, we provide a new implementation of the car repair use-case of [2]. In that scenario, an insurance company needs to help one of its consumer whose car just broke by providing him a rental car, a garage to repair his broken car and a truck to pick it up. In our implementation, several company can provide these services, and we use speculative parallelism to choose the first one that answer with a price in budget. Speculative Parallelism is achieve by rollbacking the pending requests when we got a positive answer.

  - car-repair.maude

[2] Dynamic Error Handling in Service Oriented Applications, Fundam. Inf. 2009


### Reversibility for Computation

Finally, we studied the possibility to use reversibility as a mean for computation, with the implementation of the eight-queen problem in croll-pi, using a standard backtracking algorithm. Here, we use reversibility to achieve backtacking in a concurrent setting. As a result, we provide two implementation of the algorithm: one where each queen in turn look for a valid position; one where every queen are started in parallel.

  - eight-queen.maude (sequential version)
  - eight-queen.maude (highly concurrent version)

On a DELL Latitude e6410 with 1.8Go of memory and Ubuntu 12.04 LTS:

  - the sequential version of the eight queen problem runs in about 410ms
  - the parallel version of the eight queen problem runs in about 15s

