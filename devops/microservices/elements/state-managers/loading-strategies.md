---
description: >-
  It is possible to synchronize different state managers using different
  methods, based on the use case.
---

# Loading Strategies

State managers can be written on to directly using certain event handlers such as the "write" event handler, which translates API requests into create, update, delete, etc. actions.

However, certain use cases (such as cache, search engine, read states) require synchronization between the master write state manager and other state managers (configured as the "loader.state"). State synchronization is achieved in a number of ways:

## Real-Time CDC Sync

This strategy uses Rierino's built-in change data capture capabilities, which automatically publish pulse / journal records when a record is updated in a source state. When a state manager is set-up to listen to these publications (e.g. Kafka topics), it automatically updates its records based on the received notification.

CDC based loading provides the lowest latency in data synchronization with real-time loading of updated records in milliseconds.

While this strategy is the optimal option for faster synchronization while enabling additional triggered events (e.g. webhook calls to 3rd party systems) on data updates, it is more complex to configure than the other strategies as it requires setting up runners for producing change data streams. Hence, it is typically recommended for managing state of business assets that have change more frequently or need to be synchronized across a variety of systems (such as product data).

## Caching with TTL

This strategy uses cache state managers and assigns a time-to-live value (e.g. 15 seconds) to each record. When a record is read and is discovered to be "too old", it is automatically reloaded.

This strategy is relatively simple to set-up, as it only requires configuring cache state managers. It is especially recommended for cases where records don't change often and using old versions of these records for a few seconds does not have important impact (e.g. query, variable data).

The only limitation to this approach is record updates do not trigger any state listeners, so if the users of these states expect notifications on record updates (e.g. saga, model), it should not be preferred.&#x20;

## Periodical Tracked Reload

This strategy performs regular checks (e.g. every 30 seconds) on the loader state to identify any new and updated records (basedon updateTime or any other preferred field). Then, these identified records are updated on the target state manager.&#x20;

These updates can trigger single or bulk notifications on state listeners. It is also quite simple to set-up this approach, using loader parameters for any state manager, making it ideal for small data sets which are used by handlers taking actions upon their updates (e.g. saga, model, rule, code data).

## Periodical Full Reload

Similar to previous strategy, this approach performs regular reloads from the loader state. Instead of checking whether records are updated, it performs full reload of the records.

The main downside of using this approach is it forces reload of all records, so if there are any state listeners taking actions based on such updates, it would force them to execute on each cycle. So, this strategy is typically during development and testing.

## Static Reload

This strategy loads records once when the runner is initialized and only performs reloads if there is a rebuild / restart / reload command received. This strategy is typically used during development and testing when records are not expected to change.&#x20;
