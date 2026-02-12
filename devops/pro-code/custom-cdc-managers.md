---
description: >-
  Custom CDC managers can be created and added to track changes in different
  systems, if needed.
---

# Custom CDC Managers

All CDC managers extend the abstract CDCManager class or one of its subclasses.

```java
public class MyCDCManager extends CDCManager{

  /** 
   * Constructor with provided "alias", as well as its configurations.
   * Note: CDCManagers are not necessarily managed by EventRunners,
   * which is why they don't have access to them.
   */
  public MyCDCManager(String name, Map<String, String> config) {
    super(name, config);
  }
  
  /** 
   * Function called at the beginning of CDC loop, typically used for
   * establishing connection and applying offet alignment.
   */
  @Override
  public void start() throws HotException {
  }
   
  /** 
   * Function called on each loop iteration, for receiving the next
   * record. If no new records are received, the loop sleeps for poll
   * period.
   */
  @Override
  public CDCRecord tryNext() throws HotException {
  }
   
  /** 
   * Function called at the end for closing CDC loop.
   */
  @Override
  public void close() throws IOException {
  }

}
```

{% hint style="info" %}
Base CDCManager class handles additional functionalities, such as polling for records, handling errors and retries.
{% endhint %}
