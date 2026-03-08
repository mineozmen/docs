---
description: >-
  API based systems provide shared connection and authorization details for
  external service integrations.
---

# API Based Systems

## REST

Includes settings required for connecting with an http server for standard REST API calls.

| Setting               | Definition                                                                                                               | Example                                           | Default                       |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------- | ----------------------------- |
| url                   | Base URL for REST endpoint                                                                                               | http://localhost:8080/api                         | -                             |
| contentType           | Media content type for REST communications ("query" for url parameters, "none" for no content)                           | application/xml                                   | application/json              |
| trust                 | Whether endpoint should be trusted without SSL validation                                                                | true                                              | false                         |
| auth.method           | Method to use for authentication to endpoint (input, basic, bearer, oauth2, oauth1, jwt, custom)                         | basic                                             | -                             |
| auth.token.header[^1] | Header to include generated token in                                                                                     | custom-auth                                       | Authorization                 |
| auth.prefix[^1]       | Prefix to include for generated token                                                                                    | none (for no prefix)                              | Bearer (Basic for basic auth) |
| header.\[header]      | Additional header to send in system communications                                                                       | special-header=value                              | -                             |
| clientPrefix          | Client library specific prefix to use for setting client properties                                                      | jersey.config.client                              | -                             |
| client.\[property]    | Client library specific property to include                                                                              | async.threadPoolSize=10                           | -                             |
| alternateUrls         | Comma separated list of alternate URLs for fallback                                                                      | http://back1.example.com,http://back2.example.com | -                             |
| retries               | Maximum number of retries allowed in case of server failure                                                              | 5                                                 | 0                             |
| backoffMs             | Milliseconds to wait between retries                                                                                     | 500                                               | 1000                          |
| failTTLMs             | Milliseconds to wait until reconsidering a base or alternate URL                                                         | 10000                                             | 5000                          |
| responseCookies       | Json path to return cookie details received in response (in {path, value, domain, expiry, \[name]} format)               | cookies                                           | -                             |
| responseHeaders       | Json path to return header details received in response (in {\[name]:\[]} format)                                        | headers                                           | -                             |
| connector             | Rest conector to use (default for multi part body support, apache for native PATCH calls without method header override) | apache                                            | default                       |

{% embed url="https://www.youtube.com/watch?v=4KXbjt1qylk" %}

{% file src="../../../../.gitbook/assets/system-shopify-0001.json" %}
Example Rest System Definition (Can be Imported on Element Screen)
{% endfile %}

{% hint style="info" %}
When using XML contents, json body elements starting with [- character are converted](#user-content-fn-1)[^1] to attributes. E.g.&#x20;

{"soapenv:Envelope": {"-xmls:soapenv": "[http://schemas.xmlsoap.org/soap/envelope/](http://schemas.xmlsoap.org/soap/envelope/)"} }&#x20;

generates

\<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
{% endhint %}

{% hint style="info" %}
Cookies can be sent using "header.cookie" header parameter with "\[name]=\[value];\[name]=\[value]" format as the header value, similar to regular headers.
{% endhint %}



Based on auth.method, additional system parameters are used:

### Input Authentication Method

Authentication with a request body element:

| Parameter    | Definition                                             | Example      | Default |
| ------------ | ------------------------------------------------------ | ------------ | ------- |
| auth.key     | Key to include in each request body for authentication | cmVhbGx5Pw== | -       |
| auth.element | Json path to include authentication key in body        | user.apiKey  | -       |

{% file src="../../../../.gitbook/assets/system-translate-0001.json" %}
Example Input Authentication Rest System Definition (Can be Imported on Element Screen)
{% endfile %}

### Basic Authentication Method

Username / password based basic authentication:

| Parameter     | Definition                          | Example | Default |
| ------------- | ----------------------------------- | ------- | ------- |
| auth.username | Username to include in each request | admin   | -       |
| auth.password | Password to include in each request | pass    | -       |

{% file src="../../../../.gitbook/assets/system-jenkins_default-0001.json" %}
Example Basic Authentication Rest System Definition (Can be Imported on Element Screen)
{% endfile %}

### Bearer Authentication Method

Authentication with a bearer token:

| Parameter   | Definition                           | Example        | Default |
| ----------- | ------------------------------------ | -------------- | ------- |
| auth.token  | Token to include in each request     | cmVhbGx5Pw==   | -       |
| auth.prefix | Bearer prefix to use in each request | Special\_Token | Bearer  |

{% file src="../../../../.gitbook/assets/system-deployer_default-0001.json" %}
Example Bearer Authentication Rest System Definition (Can be Imported on Element Screen)
{% endfile %}

### OAuth2 Authentication Method

OAuth2 based token authentication:

| Parameter                    | Definition                                                                                                                     | Example                                             | Default              |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------- | -------------------- |
| auth.prefix                  | Bearer prefix to use for access token in each request                                                                          | Special\_Token                                      | Bearer               |
| auth.provider.authentication | Client authentication model (if required, none, basic or input)                                                                | input                                               | basic                |
| auth.provider.key            | Key to use for client authentication                                                                                           | somekey                                             | -                    |
| auth.provider.secret         | Secret to use for client authentication                                                                                        | somesecret                                          | -                    |
| auth.provider.url            | Url of OAuth provider                                                                                                          | http://example.com/oauth                            | -                    |
| auth.provider.refreshUrl     | Url for getting new tokens when useRefresh is true                                                                             | http://example.com/oauth/refresh                    | \[auth.provider.url] |
| auth.provider.mapping[^1]    | JMESPath pattern for converting response from provider url to standard OAuth                                                   | {"access\_token": data.accessToken}                 | -                    |
| auth.token.grant             | Grant type (password, clientCredentials, refreshToken, custom)                                                                 | password                                            | -                    |
| auth.token.scope             | Authentication scope to apply                                                                                                  | customer.read                                       | -                    |
| auth.token.useRefresh        | Whether the refresh\_token received from OAuth endpoint should be used for [getting new access tokens](#user-content-fn-2)[^2] | true                                                | false                |
| auth.token.service           | Class name for OAuth provider service if custom class is needed                                                                | com.github.scribejava.core.builder.api.DefaultApi20 | -                    |

Depending on the grant type parameter, additional settings are applicable as follows:

#### Password Grant Type

| Setting       | Definition                | Example | Default |
| ------------- | ------------------------- | ------- | ------- |
| auth.username | Username for scribe grant | admin   | -       |
| auth.password | Password for scribe grant | pass    | -       |

#### Refresh Token Grant Type

| Setting           | Definition               | Example | Default |
| ----------------- | ------------------------ | ------- | ------- |
| auth.refreshToken | Refresh token for scripe | -       | -       |

#### Custom Grant Type

| Setting                        | Definition                                                                             | Example                                                 | Default |
| ------------------------------ | -------------------------------------------------------------------------------------- | ------------------------------------------------------- | ------- |
| auth.provider.system           | Name of [runner system](#user-content-fn-3)[^3] to use for token requests              | custom\_auth                                            | -       |
| provider.custom.url            | Url endpoint on custom system to call for access tokens                                | /GetToken                                               | -       |
| provider.custom.method         | Http method to use for access token calls                                              | GET                                                     | -       |
| provider.custom.content        | Content type for access token calls                                                    | query                                                   | -       |
| provider.custom.input          | Json string to use as call body/parameters for access tokens                           | { username: "user", password: $\{{rierinoKV.secret\}} } | -       |
| provider.custom.refreshUrl     | Url endpoint on custom system to call for refreshing tokens                            | /RefreshToken                                           | -       |
| provider.custom.refreshMethod  | Http method to use for token refresh calls                                             | GET                                                     | -       |
| provider.custom.refreshContent | Content type for token refresh calls                                                   | query                                                   | -       |
| provider.custom.refreshInput   | Json string to use as call body/parameters for token refresh                           | -                                                       | -       |
| provider.custom.refreshPath    | Json path to add refresh token received from access token call to call body/parameters | refresh\_token                                          | -       |

{% file src="../../../../.gitbook/assets/system-ebayseller_default-0001.json" %}
Example Scribe Authentication Rest System Definition (Can be Imported on Element Screen)
{% endfile %}

### JWT Authentication Method

Authentication generating JWT token wiith a private key:

| Parameter       | Definition                                              | Example        | Default |
| --------------- | ------------------------------------------------------- | -------------- | ------- |
| auth.prefix     | Bearer prefix to use in each request                    | Special\_Token | Bearer  |
| auth.ephemeral  | Whether each request should generate a new access token | true           | false   |
| auth.issuer     | Issuer of the token                                     | rierino        | -       |
| auth.subject    | Subject of the token                                    | 12345          | -       |
| auth.audience   | Target audience for the token                           | google         | -       |
| auth.payload    | Payload of the token                                    | foo            | -       |
| auth.id         | Id of the token                                         | 12345          | -       |
| auth.ttl        | TTL of token in seconds                                 | 36000          | 3600    |
| auth.claim.\*   | Claims of the token                                     | user=123       | -       |
| auth.header.\*  | Header of the token                                     | typ=JWT        | -       |
| auth.privateKey | Private key for signing token                           | AAAAAAAA       | -       |
| auth.algorithm  | Algorithm for signing token                             | RS256          | RS256   |

In addition to parameters, it is possible to send token inputs (e.g. issuer, ttl, claim, header) using input authentication payload.

{% file src="../../../../.gitbook/assets/system-translatev3-0001.json" %}
Example JWT Authentication Rest System Definition (Can be Imported on Element Screen)
{% endfile %}

[^1]: Since 0.3.4

[^2]: When set to false, original method (e.g. password, client credentials) is used for getting new access tokens each time instead

[^3]: All connection details are loaded from the given runner system element for access and refresh token calls
