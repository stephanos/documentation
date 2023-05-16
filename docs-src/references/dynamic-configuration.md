---
id: dynamic-configuration
title: Dynamic configuration reference
description: Dynamic condifiguration key values can be set to override the default values in a Cluster configuration.
sidebar_label: Dynamic configuration
tags:
  - reference
---

Temporal Cluster provides [dynamic configuration](/concepts/what-is-cluster-configuration#dynamic-configuration) keys that you can update and apply to a running Cluster without restarting your services.

The dynamic configuration keys are set with default values when you create your Cluster configuration.
You can override these values as you test your Cluster setup for optimal performance according to your workload requirements.

For the complete list of dynamic configuration keys, see https://github.com/temporalio/temporal/blob/master/common/dynamicconfig/constants.go.
Ensure that you check server releases notes for any changes to these keys and values.

To check the default values set for a dynamic configuration key, check the following links:

- [Frontend Service](https://github.com/temporalio/temporal/blob/5783e781504d8ffac59f9848b830868f3139b980/service/frontend/service.go#L176)
- [History Service](https://github.com/temporalio/temporal/blob/5783e781504d8ffac59f9848b830868f3139b980/service/history/configs/config.go#L309)
- [Matching Service](https://github.com/temporalio/temporal/blob/5783e781504d8ffac59f9848b830868f3139b980/service/matching/config.go#L125)
- [Worker Service](https://github.com/temporalio/temporal/blob/5783e781504d8ffac59f9848b830868f3139b980/service/worker/service.go#L193)

Note that setting dynamic configuration is optional.
Change these values only if you need to override the default values to achieve better performance on your Temporal Cluster.
Also, ensure that you test your changes before setting these in production.

To override the default dynamic configuration values, specify your custom values and constraints for the dynamic configuration keys that you want to change in a YAML configuration file.

- Each dyanmic configuration key can have zero or more values.
- Each value can have zero or more constraints.
- There are only three types of constraints that you can define:
  - `namespace`: `string`
  - `taskQueueName`: `string`
  - `taskType`: `int` (`1`:`Workflow`, `2`:`Activity`) A value is selected and returned if all its has exactly the same constraints as the ones specified in query filters (including the number of constraints).

Use the following format in your dynamic configuration file.

```yaml
testGetBoolPropertyKey:
  - value: false
  - value: true
    constraints:
      namespace: "your-namespace"
  - value: false
    constraints:
      namespace: "your-other-namespace"
testGetDurationPropertyKey:
  - value: "1m"
    constraints:
      namespace: "your-namespace"
      taskQueueName: "longIdleTimeTaskqueue"
testGetFloat64PropertyKey:
  - value: 12.0
    constraints:
      namespace: "your-namespace"
testGetMapPropertyKey:
  - value:
      key1: 1
      key2: "value 2"
      key3:
        - false
        - key4: true
          key5: 2.0
```

For example to override the default maximum queries per second made to the Persistence database from the Frontend Service, add the following to your dynamic configuration file.

```yaml
#...
frontend.persistenceMaxQPS:
  - value: 3000 # The default value for this key on the Frontend Service is 2000.
    constraints: {}
#...
```

You can also set the maximum queries that can be made from a Namespace on the Frontend with the `frontend.PersistenceNamespaceMaxQPS` key.

```yaml
#...
frontend.PersistenceNamespaceMaxQPS:
  - value: 3500 # The default value for this key on the Frontend Service is 2000.
    constraints:
      namespace: "your-namespace"
#...
```

For examples on how dynamic configuration is set, see:

- [docker-compose](https://github.com/temporalio/docker-compose/tree/main/dynamicconfig)
- [samples-server](https://github.com/temporalio/samples-server/blob/main/tls/config/dynamicconfig/development.yaml)