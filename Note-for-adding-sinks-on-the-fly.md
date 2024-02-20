We had a need to add a callback sink to our logs so we can reprocess log messages to different formats (json and MQTT) while keeping the original logging. I discovered a small trap based on not knowing the C++ standard inside out. To add a sink, be careful how you access `sinks()` in the logger. This doesn't work:

```cpp
auto sinks = log_->sinks();
sinks.push_back(std::make_shared<spdlog::sinks::callback_sink<std::mutex> >([this](const spdlog::details::log_msg& msg) {this->LogCallback(msg);}));
```

`"auto sinks = ..."` gives you a copy of the sinks vector so you're not modifying the sinks vector in the logger. The C++ compiler is actually doing what it should with the C++ standard, `"auto var = xyz"` is never supposed to give you a reference, it calls the copy constructor of the RHS object.

You need to use `auto&` or the original sink_ptr type defined in `common.h`, but it needs to be a reference to the vector. 

```cpp
auto& sinks = log_->sinks();
sinks.push_back( .....etc
```

Hopefully this saves someone else an hour of head-scratching. 

---

See details [#3014](https://github.com/gabime/spdlog/issues/3014)