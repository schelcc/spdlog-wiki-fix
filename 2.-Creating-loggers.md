Each logger contains a vector of one or more std::shared_ptr<sink>. On each log call (if the log level is right)  the logger will call the ```"sink(log_msg)"``` function on each of them.

spdlog's sinks have _mt (multi threaded) or _st (single threaded) suffixes to indicate there thread safety.
While single threaded sinks cannot be used from multiple threads simultaneously they are faster because no locking is employed.

The easiest way to create loggers is to use the factory functions in [spdlog.h](https://github.com/gabime/spdlog/tree/master/include/spdlog/spdlog.h):

```c++
//Create console, multithreaded logger
auto console = spd::stdout_logger_mt("console");
```

```c++
//Create rotating file multithreaded logger
 auto file_logger = spd::rotating_logger_mt("file_logger", "logs/mylogfile", 1048576 * 5, 3);
```



