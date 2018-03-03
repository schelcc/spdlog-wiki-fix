In _tweakme.h_, uncomment the following to prevent child processes from inheriting log file descriptors

```c++
#define SPDLOG_PREVENT_CHILD_FD
```
