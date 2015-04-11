spdlog maintains a global (per process) registry object for loggers.
The main purpose is so loggers can be accessed easily from anywhere in the project without passing them around.

```c++
spdlog::get("logger1")->info("hello");
.. 
.. 
some other source file..
..
auto l = spdlog::get("logger2");
l->info("hello again");
```

### Registering new loggers
Normally  there is no need to register loggers as they are registered automatically for you.

To register manually created loggers(i.e. not created by the spdlog.h factory functions) use the ```"register_logger(std::shared_ptr<logger>)"``` function:
```c++
spdlog::register_logger(some_logger);
```
which will register ```some_logger``` using it's name.

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
