Sinks are the one that actually write the log to its target.
They are always of ```type std::shared_ptr<sink>``` because they are meant to be shared between loggers


