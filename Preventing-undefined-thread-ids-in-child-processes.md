By default spdlog saves thread ids in thread local storage to gain a few micros for each call, but if your program forks, you **must** uncomment the ```SPDLOG_DISABLE_TID_CACHING``` flag in [tweakme.h](https://github.com/gabime/spdlog/blob/master/include/spdlog/tweakme.h).

This will prevent undefined thread ids in child log messages:

```c++
#define SPDLOG_DISABLE_TID_CACHING
```
