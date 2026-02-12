---
description: >-
  This handler (com.rierino.handler.ConversionRoleHandler) provides ability to
  enrich or convert pulse/journal records extracting related data from a state
  or query manager.
---

# Enrich Role Data

## Handler Parameters

| Parameter      | Definition                                                                  | Example          | Default |
| -------------- | --------------------------------------------------------------------------- | ---------------- | ------- |
| source.state   | Name of the state manager to use for enriching data                         | product          | -       |
| output.journal | Name of the stream for sending produced outputs                             | variant\_journal | -       |
| trigger.state  | Name of the state manager whose changes should trigger outputs              | product\_local   | -       |
| query.state    | Name of the state manager keeping query records (if a query should be used) | query            | -       |
| query.id       | Id of the query to use for enriching data                                   | variant\_query   | -       |
| buffer         | Number of records to buffer before sending to output                        | 100              | -       |
| reload         | Whether handler allows reload from source                                   | false            | true    |
| journalAction  | Journal action to apply for source feeds                                    | CREATE           | UPSERT  |

## Roles

### conversionPulse

Converts received pulse record into output and send. If there is a query.state defined for handler, %%id%% variable is injected in used query, otherwise the record with pulse id is retrieved from the source.state.

### conversionJournal

Converts received journal record into output and send with the same logic as conversionPulse.

## Commands

### FULLRELOAD

Performs conversion of all records in source state. If there is a query.state defined, it is called without any variable injections to produce outputs.

### OFFSETRELOAD

Performs conversion of all records in source state which are updated after the given "offset" in given "offsetPartition" of command parameters. If there is a query.state defined, it is called replacing %%offset%% and %%offsetPartition%% variables to produce outputs.

### IDRELOAD

Acts same as conversionPulse role, using "id" parameter of the command.
