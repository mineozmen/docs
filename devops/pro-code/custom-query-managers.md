---
description: >-
  Custom state managers can be created and added to runners to query new data
  sources, if needed.
---

# Custom Query Managers

All query managers extend the abstract QueryManager class or one of its subclasses.

```java
public class MyQueryManager extends QueryManager{

  /** 
   * Constructor with provided "alias", event runner which owns the manager
   * as well as all the configurations available to the runner
   */
  public MyQueryManager(String name, EventRunner runner, Map<String, String> config) {
    super(name, runner, config);
  }
  
  /** 
   * Function called during initialization of a query manager, typically
   * used to establish connections.
   */
  @Override
  public void configure() throws HotException {
  }
   
  /** 
   * Function called during termination of a query manager, typically
   * used to close connections with external systems.
   */
  @Override
  public void close() throws IOException {
  }
  
  /**
   * Function called for streaming records from
   * the query manager for a given query definition.
   * Data can be returned in any format preferred by the target
   * system, as convertToJson function is called for the output.
   */
  @Override
  public abstract Stream getRawQueryStream(Query query, InjectionHelper helper) throws HotException{
  }

  /**
   * Function called for translating target system specific records
   * into JsonNode.
   */
   public JsonNode convertToJson(Object object) throws HotException{
   }

}
```

Query managers typically utilize query producers for translating Rierino platform specific query definitions into target system specific language (e.g. SQL).  Query producers implement a wide range of abstract functions to provide this support, depending on their type:

* **CommandProducer:** For translating open ended query definitions to target system
* **SimpleProducer:** For translating regular queries with where condition, field selection and joins
* **AggregationProducer:** For translating group by and aggregation queries
* **PipelineProducer:** For translating step by step query pipelines supported by certain systems (e.g. MongoDB)
* **BundleProducer:** For translating bundled queries which submit multiple queries at once, supported by certain systems (e.g. Elasticsearch)
