---
description: >-
  Custom state managers can be created and added to runners to utilize new data
  sources, if needed.
---

# Custom State Managers

## Base State Managers

All state managers extend the abstract StateManager class or one of its subclasses.

```java
public class MyStateManager<T> extends StateManager<T>{

  /** 
   * Constructor with provided "alias", event runner which owns the manager
   * as well as all the configurations available to the runner
   */
  public MyStateManager(String name, EventRunner runner, Map<String, String> config) {
    super(name, runner, config);
  }
  
  /** 
   * Function called during initialization of a state manager, typically
   * used to establish connections.
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
   * Function called during termination of a state manager, typically
   * used to close connections with external systems.
   */
  @Override
  public void close() throws IOException {
  }
  
  /**
   * Function called for reading a single record for given id from
   * the state manager and returning selected data fields.
   * Data can be returned in any format preferred by the target
   * system, as convertToAggregate function is called for the output.
   */
  @Override
  public Object getRawValueByID(String ID, String fields[]) throws HotException {  
  }

  /**
   * Function called for reading a list of records for given ids from
   * the state manager and returning selected data fields.
   * Data can be returned in any format preferred by the target
   * system, as convertToAggregate function is called for the output.
   */
  public Stream getRawStreamByID(String[] IDs, String fields[]) throws HotException {
  }

  /**
   * Function called for streaming records from
   * the state manager and returning selected data fields.
   * Data can be returned in any format preferred by the target
   * system, as convertToAggregate function is called for the output.
   * Typically limit and skip are also applied on the returned stream.
   */
   public Stream getRawStream(String fields[]) throws HotException {
   }

  /**
   * Function called for translating target system specific records
   * into aggregates of T type, usually utilizing JSON manipulation
   * or similar conversion techniques.
   */
   public Aggregate<T> convertToAggregate(Object object) throws HotException {
   }

}
```

{% hint style="info" %}
For optimization purposes, additional state manager functionssuch as getCount() can be also overridden.
{% endhint %}

{% hint style="info" %}
If the state manager implementation doesn't require a database-side field selection, it can be implemented locally using JsonUtil.keepFields function as well.
{% endhint %}

State managers can also implement the following optional methods to allow re-establishing connections when there is a disruption:

```java
/** 
 * Function called to check if a received exception may be fixed with reconnect,
 * allowing triggering of reconnect in case of accessibility failures and not
 * for user errors such as wrong-formatted requests.
 */
@Override
protected boolean canReconnect(Exception e){
}

/** 
 * Function called to re-establish connection, called in case of an exception 
 * that returns canReconnect(e) as true.
 */
@Override
protected void reconnect() throws HotException{
}
```

## Writeable State Managers

State managers which are writeable (i.e. can allow changes on their records) extend a WriteableStateManager class, implementing the following functions.

<pre class="language-java"><code class="lang-java">/** 
 * Function called to remove all current records from a state manager.
 * Implementing state managers need to handle partition logic, in case
 * "shared" attribute is supported, as multiple partition state managers
 * can end up removing records multiple times otherwise.  
 */
@Override
public void removeAll() throws HotException{
}

<strong>/** 
</strong> * Function called to execute a given impact record (such as CREATE, DELETE).
 * This function is typically isolated from implementing classes, which
 * extend EndStateManager or JournalStoreManager class instead.
 */
@Override
protected Aggregate&#x3C;T> storeImpact(JournalEntry impact, boolean returnResult) throws HotException{
}
</code></pre>

While it is possible to implement a new state manager extending WriteableStateManager class, two specialized subclasses are also provided for simpler implementation.

### End State Managers

End state managers keep the latest version of aggregate records after update operations and are the most commonly used types of state managers. Following functions are implemented by these classes:

```java
/** 
 * Function called to CREATE a new aggregate record.
 * If returnResult is true, created record should be returned.
 * This function can return RECREATE error, in case a record with the same id
 * already exists in the target system.
 */
@Override
protected Aggregate<T> saveObject(Aggregate<T> object, boolean returnResult) throws HotException{
}

/** 
 * Function called for soft delete and undelete actions on an existing record.
 * This function should set "deleted" field to the value of provided argument.
 */
@Override
protected void setDelete(Aggregate object, boolean delete) throws HotException

/** 
 * Function called for full record update for an existing or new aggregate.
 * If returnResult is true, updated record should be returned.
 * This function can return STALE or JUMP error, based on compatibility of
 * provided input with the existing aggregate details (e.g. instanceVersion, offset).
 */
@Override
protected Aggregate<T> replaceObject(Aggregate<T> object, boolean returnResult) throws HotException{
}

/** 
 * Function with similar impact as replaceObject, with a fire and forget
 * approach and no validations, typically used for reload operations
 * on cache/read replica states.
 */
@Override
protected void replaceState(Aggregate<T> updated) throws HotException{
}

/** 
 * Function used for hard delete operations, removing an existing record.
 */
@Override
protected void removeObject(Aggregate object) throws HotException{
}
```

The following functions are implemented by EndStateManager, but use optimistic locking with separate read & write steps, so state manager implementations that support optimized atomic operations can override them as well:

```java
/** 
 * Function for performing patch operations on an existing aggregate record.
 * If returnResult is true, updated record should be returned.
 * This function can return STALE or JUMP error, based on compatibility of
 * provided input with the existing aggregate details.
 */
@Override
protected Aggregate<T> updateObject(Aggregate<ObjectNode> object, boolean returnResult) throws HotException{
}

/** 
 * Function for adding or replacing an array element in an existing record.
 * If returnResult is true, updated record should be returned.
 * This function can return STALE or JUMP error, based on compatibility of
 * provided input with the existing aggregate details.
 */
@Override
protected Aggregate<T> putArrayElement(String arrayPath, Aggregate<ObjectNode> object, boolean returnResult) throws HotException{
}

/** 
 * Function for removing an array element from an existing record.
 * If returnResult is true, updated record should be returned.
 * This function can return STALE or JUMP error, based on compatibility of
 * provided input with the existing aggregate details.
 */
@Override
protected Aggregate<T> removeArrayElement(String arrayPath, Aggregate<ObjectNode> object,boolean returnResult) throws HotException{
}
```

### Journal Store Managers

Journal store managers store impacts applied on aggregate records, providing basis for event sourcing implementations. These managers provide ability to "playback" historical versions of aggregates as well. Following functions are implemented by this type of state manager classes:

#### Read Functions

```java
/** 
 * Function used for producing and returning snapshot of an aggregate record
 * at a given version ID in the past.
 */
@Override
public Aggregate<T> getVersionByID(String ID, String[] fields, String version) throws HotException{
}

/** 
 * Function used for producing and returning all historical versions of
 * an aggregate record.
 */
@Override
public List<Aggregate<T>> getVersionsByID(String ID, String[] fields) throws HotException{
}

/** 
 * Function used for producing and returning all historical versions of
 * a list of aggregate records.
 */
@Override
public Stream<List<Aggregate<T>>> getVersionsStreamByID(String[] IDs, String[] fields) throws HotException{
}

/** 
 * Function used for producing and returning all historical versions of
 * a all aggregate records.
 */
@Override
public Stream<List<Aggregate<T>>> getVersionsStream(String[] fields) throws HotException{
}
```

#### Write Functions

```java
/** 
 * Function used for storing a new aggregate creation event.
 */
@Override
protected Aggregate<T> storeCreateImpact(JournalEntry impact, boolean returnResult) throws HotException{
}

/** 
 * Function used for storing all non-create & non-remove events.
 */
@Override
protected Aggregate<T> storeGenericImpact(JournalEntry impact, boolean returnResult) throws HotException;

/** 
 * Function used for storing an aggregate removal event.
 */
@Override
protected void storeRemoveImpact(JournalEntry impact) throws HotException;

/** 
 * Function used for overriding current state of a record with a given
 * alternative snapshot.
 */
@Override
protected void replaceWithSnapshot(Aggregate snapshot) throws HotException;
```

{% hint style="info" %}
A special JournalStoreManager subclass also exists (SingleJournalStoreManager), allowing use of any EndStateManager as an event sourcing base, without implementing a custom state manager class.
{% endhint %}

## Special Interfaces

Some state managers implement a list of special interfaces, for performance improvement purposes or additional functionalities.

### Bulkable Interface

This interface allows execution of a batch list of update operations all at once on the target system (used when buffer is enabled or write operations include multiple actions).&#x20;

```java
/** 
 * Returns the list of JournalActions which can not be executed
 * atomically, if any (triggering non-bulk execution for them).
 */
@Override
ArrayList<JournalAction> nonAtomics(){
}

/** 
 * Executes list of multiple requests (created from following functions) at once.
 */
@Override
void bulkExecute(List requestList) throws HotException{
}

/** 
 * Creates an aggregate override command that can be executed by bulkExecute.
 */
@Override
Object bulkReplaceState(Aggregate<T> updated) throws HotException{
}

/** 
 * Creates an aggregate hard delete command that can be executed by bulkExecute.
 */
@Override
Object bulkRemoveObject(Aggregate updated) throws HotException{
}

/** 
 * Creates an aggregate creation command that can be executed by bulkExecute.
 */
@Override
Object bulkSaveObject(Aggregate<T> updated) throws HotException{
}

/** 
 * Creates an aggregate replacement command that can be executed by bulkExecute.
 */
@Override
Object bulkReplaceObject(Aggregate<T> updated) throws HotException{
}

/** 
 * Creates an aggregate update command that can be executed by bulkExecute.
 */
@Override
Object bulkUpdateObject(Aggregate<ObjectNode> updated) throws HotException{
}

/** 
 * Creates an aggregate soft delete/undelete command that can be executed by bulkExecute.
 */
@Override
Object bulkSetDelete(Aggregate updated, boolean delete) throws HotException{
}

/** 
 * Creates an aggregate array element update command that can be executed by bulkExecute.
 */
@Override
Object bulkPutArrayElement(String path, Aggregate<ObjectNode> updated) throws HotException{
}

/** 
 * Creates an aggregate array element removal command that can be executed by bulkExecute.
 */
@Override
Object bulkRemoveArrayElement(String path, Aggregate<ObjectNode> updated) throws HotException{
}
```

{% hint style="warning" %}
Bulkable state managers must also take into account version & offset validation rules in executing batch requests.
{% endhint %}

### Queryable Interface

This interface allows use of a query manager implementation to perform certain read operations (e.g. query by ids, select fields) in a more optimized manner.

```java
/** 
 * Returns the query manager that should execute queries on behalf of the state manager.
 * Typically initialized in configure() function of the state manager.
 */
@Override
public QueryManager getQueryManager(){
}

/** 
 * Returns the "from" expression tobe passed to the query manager for requests
 * received on the state manager, commonly mapping on to state alias or table name.
 */
@Override
public String getQueryFrom(){
}
```

### Journalable Interface

This interface allows storing and reading journal records regarding updates applied on a state manager, typically used for translating pulse records to journal records in CDC operations.

```java
/** 
 * Reads a journal record with given ID from state manager's system.
 */
@Override
Object getRawJournalByID(String ID) throws HotException{
}

/** 
 * Converts a read journal record in system representation to ObjectNode.
 */
@Override
ObjectNode rawJournalToNode(Object object) throws HotException{
}
```

### Customizable Interface

This interface allows storing and reading custom data elements, which enrich/override aggregate data for specific use cases (e.g. dislaying different content for different customer segments).

```java
/**
 * Function called for reading a single record for given id, applying customData
 * that matches customBys entries.
 */
@Override
public Object getRawValueByID(String ID, String fields[], String customBys[]) throws HotException{
}

/**
 * Function called for reading a list of records for given ids, applying customData
 * that matches customBys entries.
 */
@Override
public Stream getRawStreamByID(String[] IDs, String fields[], String customBys[]) throws HotException{
}

/**
 * Function called for reading all records, applying customData
 * that matches customBys entries.
 */
@Override
public Stream getRawStream(String fields[], String customBys[]) throws HotException{
}

/**
 * Function called for converting a read record on which customBys are applied.
 */
@Override
public Aggregate<T> convertToAggregate(Object object, String customBys[]) throws HotException{
}
```

## ID Generators

ID generators allow automated assignment of unique ID values to new aggregate records on state managers. All ID generators create String IDs and require the following functions:

```java
/**
 * Function to create the next ID for a given event.
 */
@Override
public String generateID(Event event) throws HotException{
}

/**
 * Optional function to close any connections when the generator is terminating.
 */
@Override
public void close() throws IOException {
}
```

If an ID generator uses number based IDs (e.g. sequential, random numbers), it can be implemented using NumberGenerator class and used together with NumberIDGenerator instead as well, with the following functions:

```java
/**
 * Function to create the next number.
 */
@Override
public Long nextNumber() throws HotException{
}
```

{% hint style="info" %}
Parent ID generator classes provide additional functionalities for all their subclasses (e.g. applying prefixes, fillers).
{% endhint %}
