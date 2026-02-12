---
description: >-
  Custom handlers can be created and added to runners to build highly
  specialized actions, if needed.
---

# Custom Handlers

## Creating Event Handlers

Event handlers are the most commonly used type of handlers, which can be used to perform event steps within a saga flow. All event handlers extend the abstract EventHandler class.

<pre class="language-java"><code class="lang-java">package com.example.handler;

import java.util.Map;

import com.rierino.core.message.Event;
import com.rierino.core.util.HotException;
import com.rierino.runner.EventRunner;
import com.rierino.handler.EventHandler;

<strong>public class MyEventHandler extends EventHandler{
</strong>
    {
        /**
         * actionMap provides a mapping between action names
         * (as used in sagas) and the functions called in handlers.
         * It is recommended to use universally unique action names 
         * across handlers to allow easy customization of UIs by action. 
         * Any number of entries can be added to actionMap, in case the
         * handler will support more than one actions. 
         */
        actionMap.put("MyAction", this::myAction);
    }

    /** 
     * Constructor with provided "alias", event runner which owns the handler
     * as well as all the configurations available to the runner
     */
    public MyEventHandler (String name, EventRunner runner, Map&#x3C;String, String> config) {
        super(name, runner, config);
    }
    
    /**
     * Action function that is mapped to "MyAction" in actionMap, processing
     * an inputEvent and returning a produced outputEvent.
     * The function can simply throw a HotException for errors, 
     * which is automatically handled to return an event with failed status.
     * Within the function, other runner elements (such as state managers)
     * can be accessed for accessing configuration details.
     */
    public Event myAction(Event inputEvent) throws HotException {
        //TODO: utilize inputEvent to produce and return an outputEvent
    }

}
</code></pre>

{% hint style="info" %}
[Dynamic handlers](../../configuration/dynamic-handlers.md#handler-codes) can be also used with the same class structure for real-time compilation using OpenHFT event handlers.
{% endhint %}

## Creating Role Handlers

Role handlers are similar to event handlers, but are able to process less structured role data, instead of events, and are directly triggered from streams, instead of being used in sagas.

{% hint style="info" %}
In most cases, EventHandlers are preferred, as they provide more flexibility with event metadata configurations.&#x20;
{% endhint %}

```java
package com.example.handler;

import java.util.Map;

import com.rierino.core.message.RoleData;
import com.rierino.core.util.HotException;
import com.rierino.runner.EventRunner;
import com.rierino.handler.RoleHandler;

public class MyRoleHandler extends RoleHandler{

    {
        /**
         * roleMap acts similar to actionMap, providing
         * mapping of role names to class functions
         * that can consume RoleData.
         */
        roleMap.put("MyRole", this::myRole);
    }

    /** 
     * Constructor with same structure as EventHandler constructor
     */
    public MyRoleHandler(String name, EventRunner runner, Map<String, String> config) {
        super(name, runner, config);
    }
    
    /**
     * Data processing function that is mapped to "MyRole" in roleMap, processing
     * an input role data and returning a collection of produced data.
     * Event runner orchestrates forwarding of produced outputs to the right
     * stream, based on its element configurations (e.g. forwarding to a Kafka topic).
     * The function can simply throw a HotException for errors, 
     * which is automatically handled to return an event with failed status.
     * Within the function, other runner elements (such as state managers)
     * can be accessed for accessing configuration details.
     */
    public Collection<RoleData> myRole(RoleData roleData) throws HotException{
        //TODO: utilize roleData to produce and return a collection of roleData
    }

}
```

{% hint style="info" %}
RoleHandler is the parent class of EventHandler, so EventHandlers can also process role data if needed.
{% endhint %}

## Optional Functions

Handlers provide a number of additional functions for their lifecycle management, which are triggered by the event runner during initialization, rebuild, shutdown and commit stages.&#x20;

```java
/** 
 * Function called during initialization of a handler, typically
 * used to establish connections or load configuration from state managers.
 */
@Override
public void configure() throws HotException {
}

/** 
 * Function called when the runner performs a commit action, typically
 * used to process buffers or commit to external systems.
 */
@Override
public void commit() throws HotException {
}

/** 
 * Function called during termination of a handler, typically
 * used to close connections with external systems.
 */
@Override
public void close() throws IOException {
}
```

## Optional Commands

Similar to role data and events, handlers are capable of receiving and responding to commands, such as reloading data from states (typically sent through async channels such as Kafka topics).&#x20;

```java
{
   /**
    * commandMap acts similar to actionMap, providing
    * mapping of commands to class functions
    * that can consume Command input.
    */
   commandMap.put("MYCOMMAND", this::myCommand);
}

/**
 * Command processing function that is mapped to "MYCOMMAND" in commandMap, processing
 * an input command and returning updated command with its results.
 */
public Command myCommand(Command command) throws HotException {
    //TODO: utilize command to produce and return an outputCommand
}
```

## Supporting Features

All handlers have access to a variety of capabilities for processing events, role data and commands. Most frequently used features are as follows:

### Variable Injection

Allows injection of event metadata, replacing variables that are provided with %%name%% notation (typically in saga event step configurations). Entire events can be injected with the following lines:

```java
if(!InjectionHelper.skipsInjections(outputEvent)){
  InjectionHelper helper = InjectionHelper.createFor(outputEvent, getRunner());
  outputEvent = helper.injectEvent(outputEvent);
}
```

### Output Caching

It is possible to make any handler cacheable by using available runWithCache function as a wrapper to output produced. Caching can be performed with the following lines:

```java
boolean canCache = canCache(inputEvent);
String cacheState = (canCache) ? parameters.get(HandlerUtil.CACHE_STATE_PARAMETER) : null;
String cacheKey = (canCache) ? [SOME UNIQUE IDENTIFIER] : null;

ObjectNode resultNode = runWithCache(cacheState, cacheKey, () -> {
    //TODO: produce and return output for the event payload
});
```

### Input / Output Mapping

Handlers typically consume certain sections of event payloads and store their outputs to a specific path on the payload. These mappings are facilitated through certain utility functions as follows:

#### Input Mapping

```java
//Read input part of the event payload (based on inputElement event metadata)
JsonNode inputPayload = inputEvent.pullInputPayload();
//Optional: Apply "inputPattern" event metadata parameter to transform read input
ObjectNode toApply = (ObjectNode) HandlerUtil.applyInputPattern(inputEvent, inputPayload);
```

#### Output Mapping

```java
//Optional: Apply "outputPattern" event metadata parameter to transform produced output
JsonNode outputNode = HandlerUtil.applyOutputPattern(inputEvent, resultNode);
//Merge produced output into event payload (based on outputElement event metadata)
if (outputNode!=null && !outputNode.isNull()) {
  JsonNode outputPayload = outputEvent.pullOutputPayload();
  JsonUtil.extendJsonObject((ObjectNode) outputPayload, (ObjectNode) outputNode);
}
```

### Access to Data Sources

Handlers can access data sources - such as database tables - through state manager and query manager elements available within their event runners.

#### During Configuration

When state managers are used as sources of configuration (e.g. query definitions read from a database for QueryEventHandler), they are typically mapped in configure() function:

```java
String queryState = pullParameter("query.state");
this.stateManager = getRunner().getStateProcessor().getStateManager(queryState);
```

Some handlers use state managers to produce their own assets for operations (such as pre-compiled Handlebars templates), which require real-time tracking of updates in state managers to keep these assets in synch. This is typically also performed in configure() function, using state listeners:

```java
//create state listener
this.stateListener = new FunctionalStateListener(
        "myListener", 
        1000,
        (aggregate, journal) -> {},
        () -> {}
);
this.stateListener.withManager(this.stateManager).withRunner(getRunner());
this.stateListener.build();

//add listener to state manager
this.stateManager.addListener(stateListener);
```

#### During Actions

They can also be used inside the action functions, for reading / writing records on specific state managers:

```java
String domain = eventMeta.getDomain();
String id = inputPayload.get("id").asText();
Aggregate foundAgg = getRunner().getStateProcessor().aggregateById(domain, id);
```

### Access to System Configurations

Some handlers use system configurations to access details of how to communicate with target systems (such as ModelScoreEventHandler for configuring access to model file system). These details are accessible through configuration loader as follows:

```java
String modelStoreSystem = pullParameter("model.store.system");
String uri = ConfigurationLoader.pullParameter(this.config, ConfigurationType.SYSTEM, modelStoreSystem, "uri");
```
