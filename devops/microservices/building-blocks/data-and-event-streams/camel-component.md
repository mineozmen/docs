---
description: Maps input/outputs for a Camel event runner
---

# Camel Component

Includes settings required for consumer and producers of a Camel endpoint:

| Setting                            | Definition                                       | Example                                  | Default |
| ---------------------------------- | ------------------------------------------------ | ---------------------------------------- | ------- |
| parameter.camelRoute               | Uri for input stream                             | timer://test?fixedRate=true\&period=5000 | -       |
| parameter.exchange                 | Exchange pattern for the stream                  | InOut                                    | -       |
| parameter.camelGetHeaders          | Whether input received should include headers    | true                                     | false   |
| parameter.camelGetProperties       | Whether input received should include properties | true                                     | false   |
| parameter.camelPartitionHeader[^1] | Parameter name for setting message partition     | kafka.PARTITION\_KEY                     | -       |
| parameter.camelKeyHeader[^1]       | Parameter name for setting message key           | kafka.KEY                                | -       |
| parameter.camelOffsetHeader[^1]    | Parameter name for setting message offset        | kafka.OFFSET                             | -       |

[^1]: Can be set at system level as well
