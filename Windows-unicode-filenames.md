spdlog supports unicode filenames under windows. 
To enable it please uncomment the 
```c++
#define SPDLOG_WCHAR_FILENAMES
```
line in the ```tweakme.h``` file and use the SPDLOG_FILENAME_T (or L..) macro when specifying filenames:

```c++
auto file_logger = spd::rotating_logger_mt("file_logger", L"logs/mylogfile", 1048576 * 5, 3);
auto file_logger2 = spd::rotating_logger_mt("file_logger2", SPDLOG_FILENAME_T("logs/mylogfile2"), 1048576 * 5, 3);
```