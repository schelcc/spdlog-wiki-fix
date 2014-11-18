spdlog's default logging format is of the form:

```[2014-31-10 23:46:59.678] [info] [mylogger] Some message```

There are 2 ways to customize log format:
* Use ```logger.set_pattern(std::string pattern_string)``` (*recommended*)

* Implement custom formatter that implements the [formatter](https://github.com/gabime/spdlog/blob/master/include/spdlog/formatter.h) interface
and call ```set_formatter(std::share_ptr<custom_formatter>)``` 

## Customizing log format using set_pattern:
Can be set globally to all loggers:
```
spdlog::set_pattern("*** [%H:%M:%S %z] [thread %t] %v ***");
```
or to customize a specific logger:
```
some_logger.set_pattern("*** [%H:%M:%S %z] [thread %t] %v ***");
```

## Pattern flags:
Pattern flags are in the form of %[flag] and resembles [strftime] (http://www.cplusplus.com/reference/ctime/strftime/) function


|flag|meaning|example|
|-------|:-------:|:-----:|------:|
|%v|The actual text to log|"some user text"|
|%t|thread id|"1232"|
|%n|logger's name|"some logger name"
|%l|The log level of the message|"debug", "info", etc|
|%a|Abbreviated weekday name|"Thu"|
|%A|Full weekday name|"Thursday"|
|%b|Abbreviated month name|"Aug"|
|%B|Full month name|"August"|
|%c|Date and time representation|"Thu Aug 23 15:35:46 2014"|
|%C|Year in 2 digits|"14"|
|%Y|Year in 4 digits|"2014"|
|%D or %x|Short MM/DD/YY date|"08/23/01"|
|%m|Month 1-12|"11"|
|%d|Day of month 1-31|"29"|
|%H|Hours in 24 format  0-23|"23"|
|%I|Hours in 12 format  1-12|"11"|
|%M|Minutes 0-59|"59"|
|%S|Minutes 0-59|"58"|
|%e|Millisecond part of the current second 0-999|"678"|
|%p|AM/PM|"AM"|
|%r|12 hour clock|"02:55:02 pm"|
|%R|24-hour HH:MM time, equivalent to %H:%M|"23:55"|
|%T or %X|ISO 8601 time format (HH:MM:SS), equivalent to %H:%M:%S|"23:55:59"|
|%z|ISO 8601 offset from UTC in timezone ([+/-]HH:MM)|"+02:00"|
|%%|The % sign|"%"|
|%+|spdlog's default format|"[2014-31-10 23:46:59.678] [info] [mylogger] Some message"


