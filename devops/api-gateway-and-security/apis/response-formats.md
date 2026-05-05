---
description: >-
  In addition to JSON, gateway servers can return results in other formats such
  as XML, HTML, CSV and plain text
---

# Response Formats

Rierino gateway servers support returning responses in following formats, while request body should always be in JSON format:

## JSON

By default, all API responses are returned in JSON data format across all gateway servers even when "Accept: application/json" header is not provided.

## XML

It is possible to request response in XML format using either of the following:

* **Accept Header:** Sending requests with "Accept: application/xml" header.&#x20;
* **Path Extension:** Sending requests with ".xml" extension in url path (e.g. /api/request/rpc/Hello.xml).

Returned XML is a direct conversion of JSON response into XML form, wrapped inside a \<root> element.

## HTML

It is possible to request response as a HTML text using either of the following:

* **Accept Header:** Sending requests with "Accept: text/html" header.&#x20;
* **Path Extension:** Sending requests with ".html" extension in url path (e.g. /api/request/rpc/HomePage.html).

Returned HTML is the "html" (or "result.html" if missing) element of JSON response body, hence called saga endpoint should return produced html contents in such element.

## Plain Text

It is possible to request response as a plain text using either of the following:

* **Accept Header:** Sending requests with "Accept: text/plain" header.&#x20;
* **Path Extension:** Sending requests with ".txt" extension in url path (e.g. /api/request/rpc/Hello.txt).

Returned text is the "plain" (or "result.plain" if missing) element of JSON response body, hence called saga endpoint should return produced text contents in such element.

## CSV

It is possible to request response as a comma separated list using either of the following:

* **Accept Header:** Sending requests with "Accept: text/csv" header.&#x20;
* **Path Extension:** Sending requests with ".csv" extension in url path (e.g. /api/request/rpc/GetProducts.csv).

Returned CSV contents is produced from the "list" (or "result.list" if missing) element of JSON response body, hence called saga endpoint should return a JSON array in such element.
