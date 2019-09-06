# Language binding
## Python
On a reasonably sized log message (under 1000 bytes) spdlog will take to complete a single log transaction around **4% (async mode enabled)** and **7% (sync mode)** of the time required by python's standard logging logger (FileLogger).
### Installation from pypi.org

`pip install spdlog`

### Source
GitHub repo: [spdlog-python](https://github.com/bodgergely/spdlog-python)
### Features

* Async mode support
* ConsoleLogger
* FileLogger
* DailyLogger
* RotatingLogger
* SyslogLogger
* LogLevels
* Sinks

Missing:
* String formatting
