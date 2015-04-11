Each logger contains a vector of one or more std::shared_ptr<sink>. On each log call (if the log level is right) the logger will emit the log string to it's sinks which do the actual work.



spdlog's sinks have _mt (multi threaded) or _st (single threaded) suffixes to indicate there thread safety.
While single threaded sinks cannot be used from multiple threads simultaneously they are faster because no locking is employed.

### Implementing your own sink

A sink must implement the [sink](../tree/master/include/spdlog/sinks/sink.h) interface.
To implement a new sink all you need to do is implement your own ```void log(const details::log_msg& msg)``` function 


For example: 
```c++
#include "spdlog/sinks/sink.h"
class my_sink : public sink
{
  void log(const details::log_msg& msg) override
  {
    // Your code here. 
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