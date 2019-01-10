For convenience, spdlog creates a default global logger (to stdout, colored and multithreaded).

It can be used easily by calling ```spdlog::info(..), spdlog::debug(..), etc``` directly.

It's instance can be replaced to any other logger (shared_ptr):
```c++
spdlog::set_default_logger(some_other_logger);
spdlog::info("Use the new default logger");
```


