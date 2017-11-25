By default spdlog saves thread ids in thread local storage to gain a few micros for each call, but if your program forks, you **must** uncomment the ```SPDLOG_DISABLE_TID_CACHING``` flag in tweakme.h to prevent undefined thread ids in the children logs:

```c++
#define SPDLOG_DISABLE_TID_CACHING
```
