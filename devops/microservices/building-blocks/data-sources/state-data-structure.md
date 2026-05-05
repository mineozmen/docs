# State Data Structure

All Rierino state manager use a standard structure for keeping and returning data records. Records with structure are referred to as aggregates, which contain the following standard fields. Apart from the id, data and customData fields, all other data fields are automatically calculated or assigned by the state manager, write event handler or the requesting system.

* **id:** Unique identifier for the record, either automatically generated or assigned by system users. For automated id assignments, Rierino provides a number of alternative id generators, which are configured at the related state managers and can be further extended.
* **partitionID:** Partition number assigned to the record at create time, which is calculated from the last characters of its id field. Partition id is used for deciding on which stream partition to assign a record's changes (e.g. pulses, journals), ensuring sending of all changes of a record to the same runner in a partitioned & distributed deployment.
* **deleted:** Whether the record is deleted or not, used when state managers perform soft delete instead of removing records (e.g. for recovery or auditing purposes). When soft delete option is used on a state manager, queries running on such state should always include "deleted = false" filter to exclude such records.
* **createTime:** Epoch time in ms of the record creation event.
* **updateTime:** Epoch time in ms of the last record updateevent.
* **instanceVersion:** Number of updates this record has received so far, typically used in optimistic locking use cases and detecting receival of outdated update requests (e.g. when using at-least-once event streams). This value is automatically incremented by the state managers on each update.
* **offset:** Offset of the write event request which has triggered final update on this record, typically used for detecting receival of outdated update requests.
* **offsetPartition:** Partition from which the last write event request is received, which allows consistent use of offset field when an event stream system is repartitioned or replaced.
* **journalOffset:** Similar to offset field, but tracking offsets of CDC records in journal streams.
* **journalPartition:** Similar to offsetPartition field, but used together with journalOffset instead.
* **writerKey:** A unique identifier of the last update on a record, including offset, partition and id details.
* **updaterId:** Id of the user who have performed the last update on this record, if identified.
* **updaterSys:** The system which has triggered the last update on this record.
* **data:** Actual contents of the record (such as product data), which is typically structured according to a specific JSON schema.
* **customData:** Array of customizations on data field for different scenarios (such as personalization of product data for different customer segments).
