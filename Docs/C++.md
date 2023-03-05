---
tags:
  - C++
  - Programming
---

# C++

- Unofficial Standard: [http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)

- Delegates:
  - [https://www.codeproject.com/Articles/1170503/The-Impossibly-Fast-Cplusplus-Delegates-Fixed](https://www.codeproject.com/Articles/1170503/The-Impossibly-Fast-Cplusplus-Delegates-Fixed)
  - [https://www.codeproject.com/Articles/5277036/Asynchronous-Multicast-Delegates-in-Modern-Cpluspl](https://www.codeproject.com/Articles/5277036/Asynchronous-Multicast-Delegates-in-Modern-Cpluspl)
  - [http://marcmo.github.io/delegates/](http://marcmo.github.io/delegates/)

- Clang format and clang tidy: [http://www.mycpu.org/clang-format/](http://www.mycpu.org/clang-format/)

## Static Analysis

### cppcheck

- Website: [http://cppcheck.net](http://cppcheck.net)
- Visual Studio Plugin: [https://github.com/VioletGiraffe/cppcheck-vs-addin/releases](https://github.com/VioletGiraffe/cppcheck-vs-addin/releases)

## Build

C++ can be built with [CMake](./CMake.md).
## Documentation

[Doxygen](./Doxygen.md) can generate documentation from C++ source code.

## Callbacks

- [https://blog.mbedded.ninja/programming/languages/c-plus-plus/callbacks/](https://blog.mbedded.ninja/programming/languages/c-plus-plus/callbacks/)
- [https://stackoverflow.com/a/12338786](https://stackoverflow.com/a/12338786)

# Smart Pointers

- [https://stackoverflow.com/a/8114913](https://stackoverflow.com/a/8114913)

## Header Files

### Includes

```cpp
// See: https://stackoverflow.com/a/32606280
#include <cstdio> // Should be used, because everything wrapped in std namespace.
#include <stdio.h> // Should not be used, because everything in global namespace.
```

## Exceptions

```cpp
#include <stdexcept>

try
{
    // Code that might throw an exception.
}
catch (std::exception& exception)
{
    // Code to deal with the exception.
}
```

## Macros

- `__func__` gives the name of the current function (with class name). Useful for logging.

## Arguments
```cpp
int main(int argc, char** argv)
{
	// Easy way
	std::vector<std::string> v(argv, argv + argc);
	// Loop through arguments
}
```