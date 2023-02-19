# CMake

```cpp
// CMake adds -DNDEBUG to the CMAKE_C_FLAGS_{ RELEASE, MINSIZEREL } by default.
// See: https://stackoverflow.com/a/8594122
#ifndef NDEBUG
    // Code only for debug builds.
#endif
```

- C++ Cross-Platform with Visual Studio: https://learn.microsoft.com/en-us/cpp/build/get-started-linux-cmake?view=msvc-170

- C++ header cache at: `C:\Users\USER\AppData\Local\Microsoft\Linux\HeaderCache\1.0`

- CMake may not find boost lib (from e.g. vcpkg) when boost lib is header only: https://stackoverflow.com/a/72566558. Then just `target_link_libraries(app PUBLIC Boost::boost)`