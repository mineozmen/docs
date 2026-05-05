---
description: >-
  Query managers are used for querying data from an underlying data store such
  as a SQL database
---

# Query Managers

Connection details for query managers are defined using system elements, which allows defining them once and reusing them across different state and query elements.

Query managers connect to a specific system, translate Rierino query format to query statements in that system's domain specific language and return results in JSON form. Typically, a separate query manager is defined for each database.

It is possible to define any number and type of new query managers with specialized configurations, and typical managers deployed with Rierino are described in this section. A number of settings are shared across all query manager types:

| Setting           | Definition                                                                                                      | Example                                     | Default                        |
| ----------------- | --------------------------------------------------------------------------------------------------------------- | ------------------------------------------- | ------------------------------ |
| manager           | Fully qualified class name for the query manager                                                                | com.rierino.query.manager.MongoQueryManager | -                              |
| retriesPerStep    | Allowed number of retries at each backoff step during reconnect                                                 | 3                                           | 1                              |
| backoffSteps      | Comma separated list of milliseconds to wait between backoff steps                                              | 10,20,30                                    | 10,100,200,500,1000,1000,10000 |
| logDetail         | Whether detailed trace logs should be performed (always[^1]/never[^2]) independent of the current logging level | never                                       | -                              |
| telemetry.enabled | Whether handler should produce OpenTelemetry  traces & metrics                                                  | false                                       | true                           |

{% file src="../../../../.gitbook/assets/query-master-0001.json" %}
Example Query Manager Definition (Can be Imported on Element Screen)
{% endfile %}

[^1]: Typically used for debugged managers

[^2]: Typically used for sensitive managers &#x20;
