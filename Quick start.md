```c++

#include <iostream>
#include "spdlog/spdlog.h"

int main(int, char* [])
{
    try
    {  
        //Create a file rotating logger with 5mb size max and 3 rotated files
        auto file_logger = spdlog::rotating_logger_mt("file_logger", "myfilename", 1024 * 1024 * 5, 3);
        file_logger->info("Hello spdlog", 1,2,3,123.49);
        file_logger->info() << "Streams " << "are supported too  " << "!!!";
    }
    catch (const spdlog::spdlog_ex& ex)
    {
        std::cout << "Log failed: " << ex.what() << std::endl;
    }
}
```