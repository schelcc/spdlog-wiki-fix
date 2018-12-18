Spdlog **will not** throw exceptions while logging (since version [39cdd08](https://github.com/gabime/spdlog/tree/39cdd08a5475c63959174747a140de86c24e4849)). 

It might throw during the construction logger or sink, because it is considered fatal.

If an error happens during logging, the library will print an error message to stderr.
To avoid flooding the screen with error messages, the rate is limited per logger to 1 message/minute.

This behaviour can be changed by calling ```spdlog::set_error_handler(new_handler_fun)``` or ```logger->set_error_handler(new_handler_fun)```:

**Globally change the error handler:**

```c++
    spdlog::set_error_handler([](const std::string& msg) {
        std::cerr << "my err handler: " << msg << std::endl;
    });
```


**For a specific logger:**

```c++
    critical_logger->set_error_handler([](const std::string& msg) {
        throw std::runtime_error(msg);
    });

```

**The default error handler**

`_default_err_handler` will use this to print error
```c++
    fmt::print(stderr, "[*** LOG ERROR ***] [{}] [{}] {}\n", date_buf, name(), msg);
```