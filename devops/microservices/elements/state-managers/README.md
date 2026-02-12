---
description: >-
  State managers are used for defining key elements storing aggregates (such as
  product, price definitions)
---

# State Managers

State managers are used for executing read / write operations on underlying database, search and cache management systems. Typically, each state element maps to a single database table or an equivalent record source.

Connection details for state managers are defined using system elements, which allows defining them once and reusing them across different state elements.

Each state element has a different list of applicable settings, such as whether a state keeps (i.e. soft deletes) or removes deleted records, which is specific to the type of state element being configured.&#x20;

It is possible to define any number and type of new state managers with specialized configurations, and typical managers deployed with Rierino are described in this section. A number of settings are shared across all state manager types:

| Setting                  | Definition                                                                                                                                                      | Example                                             | Default                                        |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------- | ---------------------------------------------- |
| manager                  | Fully qualified class name for the state manager                                                                                                                | com.rierino.state.manager.MongoStateManager         | -                                              |
| data                     | Fully qualified class name for the aggregate data type                                                                                                          | com.rierino.core.saga.Saga                          | com.fasterxml.jackson.databind.node.ObjectNode |
| manipulator              | Fully qualified class name for the manipulator responsible for editing aggregates                                                                               | com.rierino.state.manipulator.ReflectiveManipulator | com.rierino.state.manipulator.JsonManipulator  |
| readOnly                 | Whether the state manager should refuse write requests                                                                                                          | true                                                | false                                          |
| keepDeleted              | Whether the state manager should only mark deleted records or remove them                                                                                       | true                                                | false                                          |
| filterDeleted            | Whether the state manager should automatically exclude deleted records from results or not                                                                      | false                                               | true                                           |
| writeBranched            | Whether the state manager should store records, applying current branch id                                                                                      | false                                               | true                                           |
| keepFields               | Comma separated list of fields that state manager should only keep                                                                                              | id,data.name,data.description                       | -                                              |
| shared                   | Whether the state manager shares data with other runners or not, which changes reload behavior                                                                  | true                                                | false                                          |
| priority                 | State initialization & loading priority (in case it depends on others and needs to wait)                                                                        | 100                                                 | 0                                              |
| disableReconnect         | Whether reconnect attempts should be disabled in case of an error                                                                                               | true                                                | false                                          |
| retriesPerStep           | Allowed number of retries at each backoff step during reconnect                                                                                                 | 3                                                   | 1                                              |
| backoffSteps             | Comma separated list of milliseconds to wait between backoff steps                                                                                              | 10,20,30                                            | 10,100,200,500,1000,1000,10000                 |
| optimisticIterations     | Number of retries for non-atomic operations using optimistic locking                                                                                            | 2                                                   | 3                                              |
| ignorePartition          | Whether the state manager serves a specific partition or not                                                                                                    | true                                                | false                                          |
| reload                   | Whether state should reload on start                                                                                                                            | true                                                | false                                          |
| loader.state             | Name of the state manager which acts as the data feed for a copy state (required for query based loaders [as well](#user-content-fn-1)[^1])                     | product\_write                                      | -                                              |
| loader.query.state       | Name of the state manager which keeps queries                                                                                                                   | query                                               | -                                              |
| loader.query.id          | ID of the query for loading data from loader state                                                                                                              | es\_product\_feed-0001                              | -                                              |
| loader.buffer            | Number of records to buffer before loading from loader state                                                                                                    | 100                                                 | -1                                             |
| loader.singular          | Whether reloads should fire full load listeners or single journal listeners                                                                                     | true                                                | false                                          |
| loader.trackSingular     | Whether "tracked" reloads should fire full or single listeners                                                                                                  | true                                                | false (true if loader.singular is true)        |
| loader.reloadMs          | Milliseconds between each state reload (schedules regular updates)                                                                                              | 15000                                               | -                                              |
| loader.reloadDelayMs     | Milliseconds to delay first reload for (if scheduled for regular updates)                                                                                       | 60000                                               | -                                              |
| loader.reloadTrackField  | Loader records' data [field ](#user-content-fn-2)[^2]to use for identifying new records only for regular updates                                                | updateTime                                          | -                                              |
| loader.updateOn          | Whether records received from loader should automatically trigger updates (always) or depend on whether a field is updated or not (updateTime, instanceVersion) | updateTime                                          | always                                         |
| pulse                    | Stream which feeds pulse records for updating this state with its loader                                                                                        | product\_pulse                                      | -                                              |
| journal                  | Stream which feeds journal records for updating this state with its loader                                                                                      | product\_journal                                    | -                                              |
| validate.instanceVersion | Whether write operations should validate instance versions or not                                                                                               | false                                               | true                                           |
| validate.offset          | Whether write operations should validate writer offsets or not                                                                                                  | false                                               | true                                           |
| validate.journalOffset   | Whether write operations should validate journal offsets or not                                                                                                 | false                                               | true                                           |
| buffer.size              | Number of requests to buffer before applying changes on state                                                                                                   | 1000                                                | -1                                             |
| logDetail                | Whether detailed trace logs should be performed (always[^3]/never[^4]) independent of the current logging level                                                 | never                                               | -                                              |
| maxRowsToRead            | Maximum number of rows that can be retrieved from the state for read & condition handlers (-1 for unlimited)                                                    | 1000                                                | -1                                             |
| queryId                  | ID of the query for reading records from this state manager (in read & condition handlers)                                                                      | read\_product                                       | -                                              |
| queryDomain              | Domain for running queries                                                                                                                                      | -                                                   | master                                         |
| queryHandler             | Event handler to use for running queries                                                                                                                        | -                                                   | query                                          |
| queryPattern             | Jmespath pattern to apply on query results                                                                                                                      | {id: id, data:{\}}                                  | -                                              |
| impactQueryId            | ID of the query for [reading records](#user-content-fn-5)[^5] to update (in write handler)                                                                      | write\_product                                      | -                                              |
| impactDataPattern        | [Jmespath pattern](#user-content-fn-6)[^6] to use for processing and updating read records                                                                      | impact.action!='CREATE'                             | -                                              |
| sagaHandler              | Event handler to use for running write actions as sagas                                                                                                         | -                                                   | saga                                           |
| impactsSaga              | Saga path to call for running all write actions                                                                                                                 | /Write\_Product                                     | -                                              |
| \[action]ImpactSaga      | Saga path to call for running a specific write actions                                                                                                          | Create=/Create\_Product                             | -                                              |
| telemetry.enabled        | Whether handler should produce OpenTelemetry  traces & metrics                                                                                                  | false                                               | true                                           |

{% hint style="info" %}
Using query configurations on state managers allows automated filteration, transformation of records for all read/write operations, as well as enforcing automated access control (e.g. seller accessing his product records only).
{% endhint %}

{% hint style="info" %}
Read, write and condition handlers allow bypassing use of query/sagas using "noActionChange" event metadata parameter, to access original state records as is.
{% endhint %}

{% file src="../../../../.gitbook/assets/state-icon_write-0001.json" %}
Example State Definition (Can be Imported on Element Screen)
{% endfile %}

[^1]: State for the "from" expression can be used as the loader state &#x20;

[^2]: When undefined, performs full reloads each time

    For query states, this value is passed on as %%checkpoint%% variable in executed query

[^3]: Typically used for debugged managers

[^4]: Typically used for sensitive managers &#x20;

[^5]: If query doesn't return records for an update operation, they are considered as non-accessible records for the user

[^6]: Receives {impact, claims, current} as the input and can return true/false to accept/reject update or an updated Aggregate data for update operation
