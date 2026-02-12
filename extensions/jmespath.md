---
description: >-
  Both admin UI and runners have custom functions added to standard JMESPath
  library
icon: brackets-curly
---

# JMESPath

The following functions can be used in addition to specification provided by JMESPath language, to support common use cases:

## Implementation Domain

### Server Side

These functions are available wherever JMESPath expressions are used by runners (e.g. transform steps, input/output patterns).

### Client Side

These functions are available in [conditional display](../design/user-interface/uis/extended-scope/conditional-display.md), eval value widget, [table editor](../design/user-interface/uis/widgets/array-widgets.md#table-editor) pattern cells as well as name & icon field definitions of [listers](../design/user-interface/uis/listers.md). They are not supported in other JMESPath use cases.

### Batch

These functions are available wherever JMESPath expressions are used by Python runners and libraries (e.g. ML training steps, python event handlers).

{% hint style="info" %}
Customization on the client side includes async functions, which are not applicable for certain use cases, which is why these functions are not used in all UI elements.
{% endhint %}

Some functions are only available on batch, client or server side, which are marked as follows:

**\[C]:** Available on Client-Side.

**\[S]:** Available on Server-Side.

**\[B]:** Available on Batch.

**Unmarked:** Available on all.

{% hint style="warning" %}
Server-side JMESPath implementation returns null when the input node is null, regardless of the expression provided (including constant value expressions), due to Java JMESPath library implementation logic.&#x20;

This applies to all areas where JMESPath is used (i.e. transform steps, event input and output patterns).
{% endhint %}

## Functions

### add\_all(array, array)

Merges contents of two arrays provided and returns them as a new array.

### atob(string)

Decodes a given base64 string and returns it as a UTF-8 string.

### btoa(any)

Encodes a given value into a base64 string.

### cartesian(array)

Creates the cartesian product of an array of arrays provided.

### compare(any, any) \[S, C]

Performs a comparison between the first and second parameters and returns 0 (if equal), -1 if (first < second) or 1 (if first > second) as the result.

### cond(boolean, any, any) \[S,C]

If a given condition value is true, returns the first given parameter, otherwise returns the second given parameter.

### cron(string, string, string, number) \[S]

A versatile function, processing given cron pattern, followed by an action, using the given time zone for a given epochMs time. Actions supported are as follows:

* last: Returns epoch ms for the last time this cron pattern was applicable before given time.
* next: Returns epoch ms for the next time this cron pattern willbe applicable after given time.
* fromLast: Returns ms from last time this cron pattern was applicable before given time.
* toNext: Returns ms to next time this cron pattern willbe applicable after given time.
* match: Returns whether the pattern matches given time.

If time zone is null or "SYSTEM", server time zone is used for the pattern evaluation.&#x20;

### csv\_to\_json(string, object=null) \[S]

Parses a given CSV text and converts into an array, using an optional second parameter for parsing options (from [Apache commons CSV format](https://commons.apache.org/proper/commons-csv/apidocs/org/apache/commons/csv/CSVFormat.html)).

### date\_to\_string(number, string="MM-dd-yyyy")

Converts a date given in epoch milliseconds into a string using the given Java date format.

### decode\_url(string) \[S, C]

Decodes a string which is in encoded URL form.

### decrypt(string, string, string = "AES/ECB/PKCS5Padding", string = "AES") \[S]

Decrypts a given encrypted string using a given key string. Two optional parameters are used for specifying the algorithm and key algorithm.

{% hint style="warning" %}
Please note that "AES/ECB/PKCS5Padding" is not the recommended algorithm for operations requiring high level of security and should be replaced with the preferred secure encryption algorithm.
{% endhint %}

### deep\_merge(...object)

Deep merges all given (nested) objects. For shallow merges standard merge() function can be used instead.&#x20;

### deep\_diff(any, any) \[S]

Compares given two records and returns the list of differences in a nested manner with "new" vs. "old" values where differences are identified.

### distinct(array) \[S, C]

Returns distinct list of values inside an array.

### divide(number, number)

Divides a given number by another given number. Returns null if the divisor is 0.

### dynamic(any, string) \[S, C]

Executes a dynamic jmespath expression given as string on a given parameter.

### dynamic\_repeat(any, array, string) \[S, C]

Executes a dynamic jmespath expression using each entry of a given array as "entry" and a given parameter as "input" to produce a new array.

### dynamic\_filter(any, array, string) \[S, C]

Executes a dynamic jmespath expression that returns boolean for filtering a given array using each entry of that array as "entry" and a given parameter as "input".

### element\_at(array, number) \[S, C]

Returns element of a given array at given index, supporting variable index access to arrays.

### encode\_url(string) \[S, C]

Encodes a string into encoded URL form.

### encrypt(string, string, string="AES/ECB/PKCS5Padding", string="AES") \[S]

Encrypts a given plain string using a given key string. Two optional parameters are used for specifying the algorithm and key algorithm.

{% hint style="warning" %}
Please note that "AES/ECB/PKCS5Padding" is not the recommended algorithm for operations requiring high level of security and should be replaced with the preferred secure encryption algorithm.
{% endhint %}

{% hint style="info" %}
For encrypting/decrypting more complex contents, to\_json/from\_json functions can be used.
{% endhint %}

### find\_paths(any, string) \[S]

Searches all Json paths for a given record that match a given regex expression string.

### from\_json(any) \[S, C]

Converts a json object/array into string.

### get\_element(object, string) \[S,C]

Returns element from a given json object using a given json path string.

### get\_env(string) \[S]

Returns environment variable for given name, intended for server-side use cases only.

### group\_by(array, string) \[S, C]

Groups entries of a given array by a given field and returns grouped results in  \[{id:"", list:\[]}] form.

### guid() \[S, C]

Creates and returns a glbally unique ID string.

### hash(string, string, string="SHA-256", number=1) \[S]

Hashes given plain string using a given key string. Two optional parameters are used for specifying the algorithm and hash iteration count.

### index\_array(array, string="field", number=0)

Adds an index field to all entries of a given array, using given string as field name and starting index value from given number.

### intersect(array, array) \[S, C]

Finds intersection of entries in given two arrays.

### json\_to\_csv(array, object=null) \[S]

Prints a given array as CSV text, using an optional second parameter for printing options (from [Apache commons CSV format](https://commons.apache.org/proper/commons-csv/apidocs/org/apache/commons/csv/CSVFormat.html)).

### k\_vtoo(string, any) \[S, C]

Converts a given key string and value entry to an object.

### lookup(array, array, string, boolean=false) \[S,B]

Joins two arrays with a key string field and returns them as a new array. If last parameter is true, a full join is performed instead of left join.

### lookup(string, string) \[C]

Performs a lookup from a given [source](../design/api-mapping/) for a given id string.

{% hint style="info" %}
Client side lookup function is only available within evaluation providers (i.e. not available with Handlebars templates) due to specific nature of async access.

Not implemented on server side to avoid unintended access to sensitive states on a runner.
{% endhint %}

### ltoo(array, string)

Converts a list of key-value pairs in a provided array into an object using key string field.

### merge\_index(array, array, string=null, string=null)

Merges two given arrays on index of their entries. If field names are given as 3rd and 4th parameters, entry contents are nested under these fields, otherwise they are merged with spreaded fields.

### minus(number)

Returns negative of a given number.

### mod(number, number)

Returns modulo of a given number using another given number as base.

### multiply(number, number)

Multiplies two given numbers.

### now()

Returns current time in epoch milliseconds.

### otol(object, string, string)

Converts an object to list of key-value pairs using a key string field and value string field.

### power(number, number)

Returns power of a given number using another given number as base.

### rand(number, number) \[S, C]

Returns a random integer between given two numbers.

### regex(string, string) \[C,B]

Returns matched contents of a string with a given regex expression string.

### remove\_all(array, array)

Removes all entries of the second array from the first array.

### remove\_elements(object, array) \[S, C]

Removes all fields of a given object that are listed in a given string array.

### remove\_nulls(object, boolean=false) \[S, C]

Removes fields with null values from a given object with option to remove empty strings as well.&#x20;

### repeat(object, array, string)

Repeats a given object for each element of a given array with array elements merged as a key string field.

### replace(string, string, string)

Replaces occurences of the 2nd string with the 3rd string within the 1st string.

### salt\_key(number) \[S, C]

Creates a secure random key with given number of characters.

### set\_element(object, string, any)

Adds/sets the value of a given object at the given string path.

### slug(string, boolean=false)

Converts a given string into a slug, with option to use transliteration. Trims the string, converts to lower case, transliterates or normalizes, removes non-ascii letters, replaces non-alphanumeric characters with hyphen, removes leading & trailing hyphens.

### split(string, string)

Splits a given string with a given delimiter string.

### split\_array(array, number) \[S]

Splits a given array into a list of arrays, each containing the given number of records at most.

### string\_to\_date(string, string="MM-dd-yyyy")

Converts a date given as a string in the given Java date format into epoch time in milliseconds.

### substr(string, number, number=null)

Returns substring of a given string with a given start index and an optional end index.

### to\_int(number) \[S, C]

Returns the integer part of the number. Similar to floor function, but returns an int value instead of double.

### to\_json(string) \[S, C]

Converts a string into a json object/array.

### to\_lower(string) \[S, C]

Converts a string into lower case.

### to\_upper(string) \[S, C]

Converts a string into a upper case.

### trim(string) \[S, C]

Returns trimmed version of a given string.

### type(any) \[S, C]

Returns type of the input parameter (OBJECT, ARRAY, NUMBER, STRING, BOOLEAN or NULL).

### uuid() \[S, C]

Creates a universally unique ID value.

### validate\_hash(string, string, string, string="SHA-256", number=1) \[S]

Compares given plain string with a hashed string, using a given key string. Two optional parameters are used for specifying the algorithm and hash iteration count.
