---
description: >-
  This state manager (com.rierino.state.manager.EtcdStateManager) uses etcd for
  storing and reading key-value data, typically about systems configurations.
---

# Etcd Namespace

## Manager Parameters

| Parameter | Definition                                                                                           | Example                         | Default |
| --------- | ---------------------------------------------------------------------------------------------------- | ------------------------------- | ------- |
| system    | Name of the [system ](../../systems.md#etcd)which defines etcd service details (url, user, password) | {url:"http://etcd.example.com"} | -       |
| namespace | Namespace on the etcd service for storing state details                                              | store\_infrastructure           | -       |
