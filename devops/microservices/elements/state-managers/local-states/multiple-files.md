---
description: >-
  This state manager (com.rierino.state.manager.MultiFileStateManager) provides
  read-write access to a local folder with ided json files.
---

# Multiple Files

## Manager Parameters

| Parameter | Definition                                                                          | Example      | Default |
| --------- | ----------------------------------------------------------------------------------- | ------------ | ------- |
| path      | File path for the state contents                                                    | states/test/ | -       |
| dynamic   | Whether state manager reads files on each get request or only once at the beginning | true         | false   |
