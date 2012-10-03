is_strict_weak_ordering
=======================

Utilities for certifying whether a binary relation on a set is a [strict weak ordering](http://en.wikipedia.org/wiki/Strict_weak_ordering).

Sometimes we'd like to sort a dataset with respect to a user-defined comparison function. However, the behavior of algorithms such as `std::sort` are only
defined for relations which have the "strict weak" property: they must be

  * [irreflexive](http://en.wikipedia.org/wiki/Reflexive_relation)
  * [asymmetric](http://en.wikipedia.org/wiki/Asymmetric_relation)
  * [transitive](http://en.wikipedia.org/wiki/Transitive_relation)
  * transitivity of equivalence

Parallel implementations of sorting algorithms such as `thrust::sort` often depend critically on this property.

For a given dataset and a binary relation on that set, we can certify whether or not the binary relation is a strict weak ordering:

```
#include <vector>
#include <iostream>
#include <functional>
#include <cstdlib>
#include <algorithm>
#include <limits>
#include "is_strict_weak_ordering.hpp"

int main()
{
  std::vector<int> int_vec(1000);

  std::generate(int_vec.begin(), int_vec.end(), std::rand);

  // this should pass
  std::cout << "is_strict_weak_ordering(int_vec): " << is_strict_weak_ordering(int_vec.begin(), int_vec.end(), std::less<int>()) << std::endl;

  
  std::vector<float> float_vec(1000);

  std::generate(float_vec.begin(), float_vec.end(), std::rand);

  // add a NaN to make the test fail
  float_vec[5] = std::numeric_limits<float>::quiet_NaN();

  // this should fail
  std::cout << "is_strict_weak_ordering(float_vec): " << is_strict_weak_ordering(float_vec.begin(), float_vec.end(), std::less<float>()) << std::endl;

  return 0;
}

```

