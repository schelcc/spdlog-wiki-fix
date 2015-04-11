Loggers are stored(as shared pointers) in spdlog's global registry so they can be accessed easily from anywhere in the project without passing them around

```c++
spdlog::get("logger1")->info("hello");
.. 
.. 
some other source file..
..
auto l = spdlog::get("logger2");
l->info("hello again");
```

Loggers created by spdlog:: factory functions get registered automatically using the given name.
Manually created loggers are not registered and can be using::
```c++
spdlog::register_logger(logger_shared_ptr);
```

### Registry conflicts
spdlog will throw a spdlog_ex on attempting to register using a name that already exists in the registy

### Removing loggers from the registry
The "drop()" function can be used to remove a logger from the registry.

If no other shared_ptr pointing to the logger exists, the logger will be closed and all its resources will be freed.
```c++
spdlog::drop("logger_name");
//or remove them all
spdlog::drop_all()
```
