### CMake

Library can be installed and examples can be built using [CMake](http://www.cmake.org/). Version of CMake must be 3.0+ and can be checked by `--version`:
```bash
> cmake --version
cmake version 3.3.1

CMake suite maintained and supported by Kitware (kitware.com/cmake).
```

### Generate

Generate project in `_builds` directory using `CMakeLists.txt` from current (`.`) directory, run one of the commands depending on the OS and generator you need:
```bash
> cmake -H. -B_builds -DCMAKE_BUILD_TYPE=Release # Default generator is Makefile for *nix platform
> cmake -H. -B_builds -GXcode # Xcode generator on OS X (multi-config, no need for CMAKE_BUILD_TYPE)
> cmake -H. -B_builds "-GVisual Studio 12 2013" # Visual Studio IDE, Windows, multi-config
```

or toolchains:
```bash
> cmake -H. -B_builds -DCMAKE_TOOLCHAIN_FILE=/path/to/iOS.cmake -GXcode
> cmake -H. -B_builds -DCMAKE_TOOLCHAIN_FILE=/path/to/android.cmake -DCMAKE_BUILD_TYPE=Release
```

To add examples use `SPDLOG_BUILD_EXAMPLES=ON` option:
```cmake
> cmake -H. -B_builds -DSPDLOG_BUILD_EXAMPLES=ON -DCMAKE_BUILD_TYPE=Release
```

### Build/test

Open generated project and run build/test targets. Also command line can be used too:
```
> cmake --build _builds --config Release
> cd _builds && ctest -VV -C Release
```

### Install

Headers can be installed along with the [config](http://www.cmake.org/cmake/help/v3.3/manual/cmake-packages.7.html#config-file-packages) files so library can be found by `find_package(spdlog CONFIG REQUIRED)`:

```cmake
[spdlog]> cmake -H. -B_builds -DCMAKE_INSTALL_PREFIX=/path/to/install -DCMAKE_BUILD_TYPE=Release
[spdlog]> cmake --build _builds --target install
```

Usage can be tested by building `examples` as a stand-alone project:
```cmake
[spdlog/examples]> cmake -H. -B_builds -DCMAKE_PREFIX_PATH=/path/to/install -DCMAKE_BUILD_TYPE=Release
[spdlog/examples]> cmake --build _builds
```


