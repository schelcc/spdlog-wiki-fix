I haven't seen any json examples anywhere with spdlog so I thought I'd share how we did it. It's not hard to have human-readable and json-based machine-readable logs. 

```cpp
// Set up the opening brace and an array named "log"
// we're setting a global format here but as per the docs you can set this on an individual log as well
spdlog::set_pattern(set_pattern("{\n \"log\": [");

auto mylogger = spdlog::basic_logger_mt("json_logger", "mylog.json");
mylogger->info(""); // this initializes the log file with the opening brace and the "log" array as above

// We have some extra formatting on the log level %l below to keep color coding when dumping json to the console and we use a full ISO 8601 time/date format
std::string jsonpattern = {"{\"time\": \"%Y-%m-%dT%H:%M:%S.%f%z\", \"name\": \"%n\", \"level\": \"%^%l%$\", \"process\": %P, \"thread\": %t, \"message\": \"%v\"},"};

spdlog::set_pattern(jsonpattern);
```

Then, log whatever you like as normal, for example:

```cpp
mylogger->info(“We have started.”);
```

This will give you a log entry structured like this, although it will all be on one line:

```json
{
     "time": "2021-01-10T13:44:14.567117-07:00",
     "name": "json_logger",
     "level": "info",
     "process": 6828,
     "thread": 23392,
     "message": "We have started."
}
```

You have to make sure yourself whatever you put in your log messages is valid json, you can make those as complex as you need with a complete json object if necessary. Most C++ json libraries have the ability to dump out a std::string that can be parsed as json and can then be passed as an argument to spdlog, or you can just do plain text messages like this example.

When you're finished logging, you have to close out the "log" array. We also drop the log to clean it up ourselves:

```cpp
auto mylogger = spdlog::get("json_logger");

// All we're doing below is setting the same log format, without the "," at the end
std::string jsonlastlogpattern = { "{\"time\": \"%Y-%m-%dT%H:%M:%S.%f%z\", \"name\": \"%n\", \"level\": \"%^%l%$\", \"process\": %P, \"thread\": %t, \"message\": \"%v\"}" };
spdlog::set_pattern(jsonlastlogpattern);

// below is our last log entry
mylogger->info("Finished.");

// set the last pattern to close out the "log" json array and the closing brace
spdlog::set_pattern("]\n}");

// this writes out the closed array to the file
mylogger->info("");
spdlog::drop("json_logger");
```

You end up with a log file that looks something like this (our setup and drop is done on a different thread from actual work, hence the different thread ids) :

```json
{
   "log": [
      {
         "time": "2021-01-10T13:44:14.567117-07:00",
         "name": "json_logger",
         "level": "info",
         "process": 6828,
         "thread": 23392,
         "message": "We have started."
      },
      {
         "time": "2021-01-10T13:44:23.932518-07:00",
         "name": "json_logger",
         "level": "info",
         "process": 6828,
         "thread": 8048,
         "message": "We are doing something."
      },
      {
         "time": "2021-01-10T13:44:26.927726-07:00",
         "name": "json_logger",
         "level": "info",
         "process": 6828,
         "thread": 8048,
         "message": "Look a number 123.456"
      },
      {
         "time": "2021-01-10T13:44:29.631340-07:00",
         "name": "json_logger",
         "level": "info",
         "process": 6828,
         "thread": 23392,
         "message": "Finished."
      }
   ]
}
```

You have to watch what minimum log level you have enabled in your compile, if you've disabled "info" then you'll need to use a higher severity to set up the initial json and close it out at the end. 