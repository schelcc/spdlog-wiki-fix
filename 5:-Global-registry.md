Loggers are stored(as shared pointers) in spdlog's global registry so they can be accessed easily from anywhere in the project without passing them around

```c++
spdlog::get("logger1")->info("hello");
auto l = spdlog::get("logger2");
l->info("hello again");
```

Loggers created by spdlog:: factory functions get registered automatically using the given name.
Manually created loggers are not registered and can be using::
```c++
spdlog::register_logger(logger_shared_ptr, "logger_unique_name");
```