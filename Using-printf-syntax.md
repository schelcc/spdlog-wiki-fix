Even though the format-syntax is preferred by spdlog, it can also be used with printf-syntax. To change your project to this syntax you can:

* Uncomment the line containing `#define SPDLOG_FMT_PRINTF` in [tweakme.h](https://github.com/gabime/spdlog/blob/master/include/spdlog/tweakme.h) header file.
* Add the compile definition `SPDLOG_FMT_PRINTF` to your build (for instance, `g++ -DSPDLOG_FMT_PRINTF -std=c++11 -pthread  -Ispdlog/include my_log.cpp`)