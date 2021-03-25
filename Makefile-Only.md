### Makefile

The library can also be utilized via Makefile like so:

```Makefile
LDFLAGS += -L/path/to/lib -lspdlog
```

```main.cpp
#define SPDLOG_COMPILED_LIB 1
#include "spdlog/spdlog.h"

// ... lots of amazing code.
```