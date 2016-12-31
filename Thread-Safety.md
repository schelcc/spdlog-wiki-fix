# Thread Safety

### Functions under the ```spdlog::``` namespace 
The functions under ```spdlog::``` are thread safe, **but the following** should not be called while loggers in other threads are running:
```c++
void spdlog::set_pattern(const std::string&);
void spdlog::set_formatter(formatter_ptr);
void spdlog::set_error_handler(log_err_handler);
```

### Loggers
Logger objects are thread safe **except for the following methods** (should not be called by concurrently on same instance):

```c++
void spdlog::logger::set_pattern(const std::string&);
void spdlog::logger::set_formatter(formatter_ptr);
void spdlog::set_error_handler(log_err_handler);
```

### Sinks
- **Thread safe sinks:** sinks ending with ```_mt``` (e.g ```daily_file_sink_mt```)
- **Non thread safe sinks:** sinks ending with ```_st``` (e.g ```daily_file_sink_st```)


