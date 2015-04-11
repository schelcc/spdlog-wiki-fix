Sinks are the objects that actually write the log to their target. Each sink should be responsible for only single target (e.g file, console, db)

Each logger contains a vector of one or more```std::shared_ptr<sink>```. On each log call (if the log level is right) the logger will call the ```"sink(log_msg)"``` function on each of them.



spdlog's sinks have _mt (multi threaded) or _st (single threaded) suffixes to indicate the thread safety.
While single threaded sinks cannot be used from multiple threads simultaneously, they are faster because no locking is employed.


### Available sinks

**rotating_file_sink**  ([sinks/file_sinks.h] (https://github.com/gabime/spdlog/tree/master/include/spdlog/sinks/file_sinks.h)):

When reaching file max size it will close the file, rename it and create a new file. Both max size and max files to hold are configurable in the ctor.

Usage example: 
```c++
auto rotating = make_shared<spdlog::sinks::rotating_file_sink_mt> ("log_filename", ".log", 1024*1024, 5, false);
```
will create a thread safe sink which will keep its file to max of 1MB and max of 5 rotated files. The last param(false) indicates not to flush upon every log call (for performance reasons) but to rely on whenever the OS decides to flush to the disk. 

**daily_file_sink** ([sinks/file_sinks.h] (https://github.com/gabime/spdlog/tree/master/include/spdlog/sinks/file_sinks.h)):

A file sink that will creates a new log file (with the timestamp appended to its name) every day on the specified time.

Usage example:
```c++
auto daily = make_shared<spdlog::sinks::daily_file_sink_mt> ("log_filename", ".log", 14, 55);
```
will create a thread safe sink which will create a new log file each day on 14:55.

**simple_file_sink** ([sinks/file_sinks.h] (https://github.com/gabime/spdlog/tree/master/include/spdlog/sinks/file_sinks.h)):

A simple file sink that just writes to the give log file with no restrictions.

Usage example:
```c++
auto simple = make_shared<spdlog::sinks::simple_file_sink_mt> ("filename.log", true);
```
will create file logger to "filename.log" and auto flush set to true (flush every log call to disk)


**stdout_sink/stderr_sink** ([sinks/stdout_sinks.h] 
(https://github.com/gabime/spdlog/tree/master/include/spdlog/sinks/stdout_sinks.h)):

Write to standard output or error

Usage example:
```c++
auto console = make_shared<spdlog::sinks::stdout_sink_mt> ();
auto err = make_shared<spdlog::sinks::stderr_sink_mt> ();
```


**ostream_sink** ([sinks/ostream_sink.h] (https://github.com/gabime/spdlog/tree/master/include/spdlog/sinks/ostream_sink.h)):

Write to the given std::ostream

Usage example:
```c++
std::ostringsteam oss;
auto ostream_sink = make_shared<spdlog::sinks::ostream_sink_mt> (oss);
```

**null_sink** ([sinks/null_sink.h] (https://github.com/gabime/spdlog/tree/master/include/spdlog/sinks/null_sink.h)):

null sink that throws away its log - can be used debugging or as reference implementation.

Usage example:
```c++

auto null_sink = make_shared<spdlog::sinks::null_sink_st> ();



**syslog_sink** ([sinks/syslog_sink.h] (https://github.com/gabime/spdlog/tree/master/include/spdlog/sinks/syslog_sink.h)):

Linux only syslog sink. Sends its log to syslog.


Usage example:

```c++
auto syslog = make_shared<spdlog::sinks::syslog_sink> ("my_identity");
```
### Implementing your own sink

A sink must implement the [sink](https://github.com/gabime/spdlog/tree/master/include/spdlog/sinks/sink.h) interface.
To implement a new sink all you need to do is implement your own ```void log(const details::log_msg& msg)``` function 


For example: 
```c++
#include "spdlog/sinks/sink.h"
class my_sink : public sink
{
  void log(const details::log_msg& msg) override
  {
    // Your code here. 
    //details::log_msg is a struct containing the log entry info like level, timestamp, thread id etc.
    // msg.formatted contains the formatted log.
    // msg.raw contains pre formatted log
    std::cout << msg.formatted.str();

 }
};
..
```

If your sink needs to use locks for thread safety, you can inherit from [base_sink] (../tree/master/include/spdlog/sinks/base_sink.h) which will do the locking for you on each log call.
In this case you will need to implement the protected "_sink_it" function.

For example: 
```c++
class my_threaded_sink : public base_sink < std::mutex >
{
..
protected:
    void _sink_it(const details::log_msg& msg) override
    {
        //Your code here
    }
..
}
```
