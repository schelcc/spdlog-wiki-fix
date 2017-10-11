Even though the [format](http://fmtlib.net/latest/syntax.html) syntax is preferred by spdlog, it can be used with printf syntax instead. 

To change your project to printf syntax you can do one of the following:

* Uncomment the line containing `#define SPDLOG_FMT_PRINTF` in [tweakme.h](https://github.com/gabime/spdlog/blob/master/include/spdlog/tweakme.h) header file.
* Add the compile definition `SPDLOG_FMT_PRINTF` to your build (for instance, `g++ -DSPDLOG_FMT_PRINTF -std=c++11 -pthread  -Ispdlog/include my_log.cpp`)