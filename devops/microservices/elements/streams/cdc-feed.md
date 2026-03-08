---
description: >-
  Provides real-time input from change data logs of a data source, typically
  used for with a Samza event runner
---

# CDC Feed

Includes settings required for change data capture flows of a specific data table from [CDC systems](../systems/#cdc). Typically, name of this stream matches the source table name.

{% embed url="https://www.youtube.com/watch?v=MCvRGhUhijg" %}

While Samza runner CDC configurations are mainly done at system level, Spring runners allow the following configurations on each CDC stream:

| Setting                  | Definition                                                          | Example     | Default |
| ------------------------ | ------------------------------------------------------------------- | ----------- | ------- |
| parameter.system         | Name of the system this stream is based on                          | mongo\_cdc  | -       |
| parameter.offset.state   | Name of the state that stores CDC resume token                      | cdc\_offset | -       |
| parameter.offset.initial | Initial offset to use, if missing from cdc state                    | 100         | -       |
| parameter.asPulse        | Whether stream should produce pulses instead of CDC records         | false       | true    |
| parameter.pollMs         | Milliseconds to wait if no new records are received from the stream | 5000        | 1000    |
