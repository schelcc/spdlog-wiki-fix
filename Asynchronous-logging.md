Asynchronous logging works be using the spdlog::async_logger.

To turn async mode on call ```spdlog::set_async_mode(q_size) //size must be power of 2``` to tell spdlog to create async loggers from now on.


Each such logger has a queue with pre allocated slots and a worker thread.

When the user logs a message to an async logger the following happen:

1. The next empty slot in the queue is used to store the log message. If the queue is full (exceeded it's max_size), the call blocks until there is room again.

2. The worker thread pops the message from the queue and log the message using the logger's sinks.

3. If an exception happens in the worker thread (e.g. failed logging to file) an exception will be re-thrown when the user calls the next log message. This way the user can be notified on errors happening in the worker thread.

#### Full queue policy
User can decide what to do when the queue is full:
* Block the call until there is more room (default behaviour)
* Discard the message and never block - by setting the ```spdlog::async_overflow_policy```:
```c++
spdlog::set_async_mode(q_size, spdlog::async_overflow_policy::discard_log_msg)
```

#### Thread safety
async loggers are always thread safe. Moreover, they use lockfree queues to prevent locking even when multiple threads use the same logger simultaneously. 


#### Warm-up function
Sometimes the user needs to do some initialization on the newly created worker thread (for example set thread affinity or priority).

To achieve this the user can provide a callback function or lambda to the ```set_async_mode``` call:

```c++
spdlog::set_async_mode(q_size, spdlog::async_overflow_policy::block_retry, []() {/* Some init code */});
```